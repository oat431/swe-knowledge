# NestJS Batch Processing Checklist

> NestJS-specific companion to the general [Batch Processing Checklist](batch.md).
> TypeScript + NestJS standalone mode, BullMQ for queues, @nestjs/schedule for cron.
> NestJS 10+, Node 20+. Last updated: 2026-07-17

---

## 1. Project Setup & Mode Selection

- [ ] **Standalone application (one-shot jobs)** — No HTTP server. Runs job, exits. Perfect for K8s CronJob.

```typescript
// main.ts — standalone batch
async function bootstrap() {
  const app = await NestFactory.createApplicationContext(AppModule, {
    logger: ['error', 'warn', 'log'],
  });

  const job = app.get(OrderSyncJob);
  try {
    await job.run();
    process.exit(0);
  } catch (error) {
    console.error('Job failed:', error);
    process.exit(1);
  } finally {
    await app.close();
  }
}
bootstrap();
```

- [ ] **Long-running worker (queue consumer)** — HTTP server + BullMQ workers. Runs indefinitely, processes jobs from queue. Health endpoint for K8s liveness.
- [ ] **Hybrid (API + scheduled jobs)** — Same app serves API and runs scheduled tasks. Simple but couples concerns. Fine for small apps. Split when scaling independently.
- [ ] **TypeScript strict** — `strict: true`, `noUncheckedIndexedAccess`. Batch code handles `undefined`/`null` constantly — strict catches misses.
- [ ] **SWC builder** — `@swc/core` in `nest-cli.json`. Faster builds. Batch dev cycle: change → build → test against real data.
- [ ] **Separate `main.ts` per mode** — `main.api.ts`, `main.batch.ts`, `main.worker.ts`. Same modules, different entry points. Build target per Dockerfile.

---

## 2. Module Structure

```
src/
├── common/
│   ├── batch/              ← Reusable batch primitives
│   │   ├── chunk.processor.ts    ← Generic chunk-oriented processing
│   │   ├── checkpoint.service.ts ← Cursor persistence
│   │   ├── skip-tracker.ts       ← Error counting + threshold
│   │   └── batch.interfaces.ts   ← Reader/Processor/Writer interfaces
│   ├── config/
│   └── database/
├── jobs/
│   ├── order-sync/
│   │   ├── order-sync.job.ts       ← Orchestrates the job
│   │   ├── order-sync.reader.ts    ← Reads from source
│   │   ├── order-sync.processor.ts ← Transforms/validates
│   │   ├── order-sync.writer.ts    ← Writes to target
│   │   └── order-sync.module.ts
│   ├── report-gen/
│   └── file-import/
├── workers/                ← BullMQ processors (if queue-based)
├── app.module.ts
└── main.ts
```

- [ ] **Feature modules per job** — Each job is a NestJS module. Independent, testable, deployable alone if needed.
- [ ] **Shared batch primitives** — Generic `ChunkProcessor`, `CheckpointService`, interfaces. Don't re-implement pagination loop in every job.
- [ ] **DI for everything** — Reader, Processor, Writer are `@Injectable()`. Swap implementations in tests. Mock external dependencies.

---

## 3. Batch Primitives (Build Once, Reuse)

- [ ] **Reader/Processor/Writer interfaces** — Spring Batch-inspired but TypeScript-idiomatic.

```typescript
// batch.interfaces.ts
export interface ItemReader<T> {
  read(cursor: string | null, chunkSize: number): Promise<{ items: T[]; nextCursor: string | null }>;
}

export interface ItemProcessor<I, O> {
  process(item: I): Promise<O | null>; // null = filtered
}

export interface ItemWriter<T> {
  write(items: T[]): Promise<void>;
}

export interface BatchJobConfig {
  name: string;
  chunkSize: number;
  skipLimit: number;
  retryAttempts: number;
  timeoutMs: number;
}
```

- [ ] **Generic chunk processor** — Reusable orchestration loop. Reads chunks, processes items, writes results, tracks progress.

```typescript
@Injectable()
export class ChunkOrchestrator {
  constructor(
    private readonly checkpoint: CheckpointService,
    private readonly skipTracker: SkipTracker,
  ) {}

  async run<I, O>(config: BatchJobConfig, reader: ItemReader<I>, processor: ItemProcessor<I, O>, writer: ItemWriter<O>): Promise<BatchResult> {
    let cursor = await this.checkpoint.load(config.name);
    const stats = { read: 0, written: 0, skipped: 0, errors: 0 };

    while (true) {
      const { items, nextCursor } = await reader.read(cursor, config.chunkSize);
      if (items.length === 0) break;

      stats.read += items.length;
      const results: O[] = [];

      for (const item of items) {
        try {
          const result = await processor.process(item);
          if (result !== null) results.push(result);
          else stats.skipped++;
        } catch (error) {
          stats.errors++;
          this.skipTracker.record(item, error);
          if (this.skipTracker.isExceeded(config.skipLimit)) {
            throw new Error(`Skip limit exceeded: ${config.skipLimit}`);
          }
        }
      }

      if (results.length > 0) {
        await writer.write(results);
        stats.written += results.length;
      }

      cursor = nextCursor;
      await this.checkpoint.save(config.name, cursor);
    }

    return stats;
  }
}
```

- [ ] **Checkpoint service** — Persist cursor to database. Load on restart.

```typescript
@Injectable()
export class CheckpointService {
  constructor(@InjectRepository(BatchCheckpoint) private repo: Repository<BatchCheckpoint>) {}

  async save(jobName: string, cursor: string | null): Promise<void> {
    await this.repo.upsert({ jobName, cursorValue: cursor, updatedAt: new Date() }, ['jobName']);
  }

  async load(jobName: string): Promise<string | null> {
    const record = await this.repo.findOneBy({ jobName });
    return record?.cursorValue ?? null;
  }

  async reset(jobName: string): Promise<void> {
    await this.repo.delete({ jobName });
  }
}
```

---

## 4. Scheduling (@nestjs/schedule)

- [ ] **`@nestjs/schedule`** — `ScheduleModule.forRoot()` in `AppModule`. Cron, interval, and timeout decorators.

```typescript
@Injectable()
export class OrderSyncScheduler {
  private readonly logger = new Logger(OrderSyncScheduler.name);

  constructor(private readonly orderSyncJob: OrderSyncJob) {}

  @Cron('0 2 * * *', { name: 'order-sync', timeZone: 'Asia/Bangkok' })
  async handleCron() {
    this.logger.log('Starting scheduled order sync');
    try {
      const result = await this.orderSyncJob.run();
      this.logger.log('Order sync complete', result);
    } catch (error) {
      this.logger.error('Order sync failed', error.stack);
      // Alert: send to Slack/PagerDuty
    }
  }
}
```

- [ ] **Timezone-aware** — `timeZone: 'Asia/Bangkok'` in `@Cron()`. Don't assume UTC. DST-safe.
- [ ] **No overlapping** — Use lock: simple `boolean` flag, Redis lock (`@nestjs/cache-manager`), or `ShedLock`-style DB lock.

```typescript
@Injectable()
export class OrderSyncScheduler {
  private isRunning = false;

  @Cron('0 */30 * * * *')
  async handleCron() {
    if (this.isRunning) {
      this.logger.warn('Previous run still active, skipping');
      return;
    }
    this.isRunning = true;
    try {
      await this.orderSyncJob.run();
    } finally {
      this.isRunning = false;
    }
  }
}
```

- [ ] **Dynamic cron** — `SchedulerRegistry` to add/remove jobs at runtime. Useful for tenant-specific schedules or feature flags.
- [ ] **Timeout for scheduled jobs** — Wrap with `AbortController` or `Promise.race` with timeout. Don't let a stuck job block the next schedule.

---

## 5. Queue-Based Processing (BullMQ)

- [ ] **`@nestjs/bullmq`** — Redis-backed job queue. Retries, priority, concurrency, rate limiting, scheduling, dashboard.

```typescript
// app.module.ts
@Module({
  imports: [
    BullModule.forRoot({ connection: { host: 'redis', port: 6379 } }),
    BullModule.registerQueue({ name: 'order-processing' }),
  ],
})
export class AppModule {}
```

- [ ] **Producer — enqueue jobs** — From API endpoint, scheduler, or another job.

```typescript
@Injectable()
export class OrderProducer {
  constructor(@InjectQueue('order-processing') private queue: Queue) {}

  async enqueueBatch(orderIds: string[]): Promise<void> {
    const jobs = orderIds.map(id => ({
      name: 'process-order',
      data: { orderId: id },
      opts: { attempts: 3, backoff: { type: 'exponential', delay: 5000 } },
    }));
    await this.queue.addBulk(jobs);
  }

  async scheduleDaily(date: string): Promise<void> {
    await this.queue.add('daily-sync', { date }, {
      repeat: { pattern: '0 2 * * *' },
      jobId: `daily-sync-${date}`, // prevent duplicates
    });
  }
}
```

- [ ] **Consumer — process jobs** — `@Processor()` decorator. Handles one job at a time (configurable concurrency).

```typescript
@Processor('order-processing')
export class OrderConsumer extends WorkerHost {
  private readonly logger = new Logger(OrderConsumer.name);

  constructor(private readonly orderService: OrderProcessingService) {
    super();
  }

  async process(job: Job<{ orderId: string }>): Promise<void> {
    this.logger.log(`Processing order ${job.data.orderId}`, { jobId: job.id, attempt: job.attemptsMade });

    await this.orderService.processOrder(job.data.orderId);

    // Update progress for long jobs
    await job.updateProgress(100);
  }

  @OnWorkerEvent('failed')
  onFailed(job: Job, error: Error) {
    this.logger.error(`Job ${job.id} failed after ${job.attemptsMade} attempts`, error.stack);
    if (job.attemptsMade >= job.opts.attempts) {
      // Final failure — alert
      this.alertService.notify(`Order processing failed: ${job.data.orderId}`);
    }
  }
}
```

- [ ] **Concurrency control** — `@Processor('queue', { concurrency: 5 })`. Limits parallel processing per worker instance. Size to DB pool / downstream capacity.
- [ ] **Rate limiting** — BullMQ built-in: `limiter: { max: 100, duration: 60000 }` (100 jobs per minute). For external API rate limits.
- [ ] **Job priority** — `priority: 1` (highest) to `priority: N`. Critical jobs processed first.
- [ ] **Retry strategy** — `attempts: 3`, `backoff: { type: 'exponential', delay: 5000 }`. Transient failures auto-retry. Permanent failures → `failed` event.
- [ ] **Dead letter** — After max attempts, job moves to `failed` state. Query failed jobs via BullMQ API. Replay with `queue.retryJobs()`.
- [ ] **Bull Board dashboard** — `@bull-board/nestjs`. Visual queue monitoring: active, waiting, completed, failed, delayed. Mount at `/admin/queues`.

---

## 6. Data Reading Patterns

- [ ] **Database cursor (TypeORM)** — Keyset pagination. Returns chunk + next cursor.

```typescript
@Injectable()
export class OrderReader implements ItemReader<OrderEntity> {
  constructor(
    @InjectRepository(OrderEntity) private repo: Repository<OrderEntity>,
  ) {}

  async read(cursor: string | null, chunkSize: number) {
    const qb = this.repo.createQueryBuilder('order')
      .orderBy('order.id', 'ASC')
      .take(chunkSize);

    if (cursor) {
      qb.where('order.id > :cursor', { cursor });
    }

    const items = await qb.getMany();
    const nextCursor = items.length > 0 ? items[items.length - 1].id : null;
    return { items, nextCursor };
  }
}
```

- [ ] **Database cursor (Prisma)** — Built-in cursor pagination.

```typescript
@Injectable()
export class OrderReader implements ItemReader<Order> {
  constructor(private prisma: PrismaService) {}

  async read(cursor: string | null, chunkSize: number) {
    const items = await this.prisma.order.findMany({
      take: chunkSize,
      ...(cursor && { skip: 1, cursor: { id: cursor } }),
      orderBy: { id: 'asc' },
    });
    const nextCursor = items.at(-1)?.id ?? null;
    return { items, nextCursor };
  }
}
```

- [ ] **File reading (streaming)** — `readline` or `csv-parse` with stream API. Don't load entire file into memory.

```typescript
import { createReadStream } from 'fs';
import { parse } from 'csv-parse';

@Injectable()
export class CsvReader {
  async *readStream(filePath: string): AsyncGenerator<Record<string, string>> {
    const parser = createReadStream(filePath).pipe(
      parse({ columns: true, skip_empty_lines: true }),
    );
    for await (const record of parser) {
      yield record;
    }
  }
}
```

- [ ] **API pagination** — Fetch page by page with cursor/offset. Respect `Retry-After` headers. Use `axios` or `got` with retry interceptor.
- [ ] **AsyncGenerator for streaming** — `async function*` yields items one at a time. Memory-efficient for large datasets. Compose with `for await...of`.


---

## 7. Data Writing Patterns

- [ ] **Bulk insert (TypeORM)** — `repo.save(items)` does individual INSERTs. Use `repo.insert(items)` or query builder for true bulk.

```typescript
@Injectable()
export class OrderWriter implements ItemWriter<ProcessedOrder> {
  constructor(private dataSource: DataSource) {}

  async write(items: ProcessedOrder[]): Promise<void> {
    await this.dataSource
      .createQueryBuilder()
      .insert()
      .into(ProcessedOrderEntity)
      .values(items)
      .orUpdate(['amount', 'status', 'processed_at'], ['order_id']) // upsert
      .execute();
  }
}
```

- [ ] **Bulk insert (Prisma)** — `prisma.order.createMany({ data: items, skipDuplicates: true })`. Or `$transaction` for upserts.

```typescript
@Injectable()
export class OrderWriter implements ItemWriter<ProcessedOrder> {
  constructor(private prisma: PrismaService) {}

  async write(items: ProcessedOrder[]): Promise<void> {
    await this.prisma.$transaction(
      items.map(item =>
        this.prisma.processedOrder.upsert({
          where: { orderId: item.orderId },
          update: { amount: item.amount, status: item.status, processedAt: new Date() },
          create: item,
        }),
      ),
    );
  }
}
```

- [ ] **Transaction per chunk** — Wrap writer in transaction. Chunk fails → rollback one chunk, not entire job. TypeORM: `dataSource.transaction()`. Prisma: `prisma.$transaction()`.
- [ ] **File writer** — `createWriteStream` + CSV/JSON stringifier. Flush per chunk. Close on completion.
- [ ] **Composite writer** — Write to multiple targets: DB + audit log, primary + replica, DB + event bus. All in same transaction where possible.

---

## 8. Error Handling & Resilience

- [ ] **Skip tracker** — Count errors per type. Exceed threshold → abort. Report skipped items at end.

```typescript
@Injectable()
export class SkipTracker {
  private skipped: Array<{ item: unknown; error: Error; timestamp: Date }> = [];

  record(item: unknown, error: Error): void {
    this.skipped.push({ item, error, timestamp: new Date() });
  }

  isExceeded(limit: number): boolean {
    return this.skipped.length > limit;
  }

  getReport() {
    return {
      count: this.skipped.length,
      byType: this.groupByErrorType(),
      items: this.skipped,
    };
  }

  private groupByErrorType() {
    return this.skipped.reduce((acc, { error }) => {
      const type = error.constructor.name;
      acc[type] = (acc[type] || 0) + 1;
      return acc;
    }, {} as Record<string, number>);
  }
}
```

- [ ] **Retry with backoff** — For transient errors in readers/writers. Use `p-retry` or hand-rolled.

```typescript
import pRetry from 'p-retry';

async function withRetry<T>(fn: () => Promise<T>, attempts = 3): Promise<T> {
  return pRetry(fn, {
    retries: attempts,
    onFailedAttempt: (error) => {
      console.warn(`Attempt ${error.attemptNumber} failed. ${error.retriesLeft} retries left.`);
    },
  });
}
```

- [ ] **Error classification** — Custom exception hierarchy. `TransientError` (retry), `ValidationError` (skip), `CriticalError` (abort).

```typescript
export class TransientError extends Error { readonly retryable = true; }
export class ValidationError extends Error { readonly retryable = false; }
export class CriticalError extends Error { readonly retryable = false; readonly abort = true; }
```

- [ ] **Dead letter table** — Persist failed items for inspection and replay.

```typescript
@Entity('batch_dead_letters')
export class DeadLetterEntity {
  @PrimaryGeneratedColumn('uuid') id: string;
  @Column() jobName: string;
  @Column() executionId: string;
  @Column('jsonb') payload: Record<string, unknown>;
  @Column() errorType: string;
  @Column() errorMessage: string;
  @Column({ default: 0 }) retryCount: number;
  @CreateDateColumn() createdAt: Date;
}
```

- [ ] **Graceful shutdown** — `app.enableShutdownHooks()`. `OnModuleDestroy` → finish current chunk → save checkpoint → close connections.

```typescript
@Injectable()
export class OrderSyncJob implements OnModuleDestroy {
  private isShuttingDown = false;

  onModuleDestroy() {
    this.isShuttingDown = true;
  }

  async run() {
    while (hasMoreData && !this.isShuttingDown) {
      await this.processChunk();
    }
    if (this.isShuttingDown) {
      this.logger.warn('Shutdown requested, stopping after current chunk');
    }
  }
}
```

---

## 9. Observability

- [ ] **Logger per job** — `new Logger(OrderSyncJob.name)`. Structured context: job name, execution ID, chunk number.

```typescript
@Injectable()
export class OrderSyncJob {
  private readonly logger = new Logger(OrderSyncJob.name);
  private readonly executionId = randomUUID();

  async run() {
    this.logger.log('Job started', { executionId: this.executionId });
    // ... processing
    this.logger.log('Chunk processed', {
      executionId: this.executionId,
      chunk: chunkNum,
      read: stats.read,
      written: stats.written,
      skipped: stats.skipped,
      durationMs: elapsed,
    });
  }
}
```

- [ ] **Pino for structured JSON** — `nestjs-pino`. JSON logs in production. Pretty in dev. Automatic request context.
- [ ] **Metrics** — `@willsoto/nestjs-prometheus` or custom. Counters: items processed, errors, skips. Histogram: chunk duration. Gauge: active jobs.

```typescript
@Injectable()
export class BatchMetrics {
  private readonly itemsProcessed: Counter<string>;
  private readonly chunkDuration: Histogram<string>;

  constructor(@InjectMetric('batch_items_total') itemsCounter: Counter<string>,
              @InjectMetric('batch_chunk_duration') chunkHist: Histogram<string>) {
    this.itemsProcessed = itemsCounter;
    this.chunkDuration = chunkHist;
  }

  recordChunk(job: string, outcome: 'success' | 'skipped' | 'error', count: number) {
    this.itemsProcessed.inc({ job, outcome }, count);
  }

  observeChunkDuration(job: string, durationSec: number) {
    this.chunkDuration.observe({ job }, durationSec);
  }
}
```

- [ ] **Health endpoint for workers** — Long-running worker needs `/health`. Report unhealthy if: queue connection lost, last heartbeat stale, memory pressure.
- [ ] **BullMQ events** — Listen to `completed`, `failed`, `stalled` events. Push to metrics. Alert on `stalled` (worker died mid-job).
- [ ] **Job summary logging** — On completion: total read, written, skipped, errored, duration. Parseable for alerting rules.

---

## 10. Testing

- [ ] **Unit tests for processors** — Pure transform logic. Mock nothing. Fast.

```typescript
describe('OrderProcessor', () => {
  let processor: OrderProcessor;

  beforeEach(() => {
    processor = new OrderProcessor();
  });

  it('should transform valid order', async () => {
    const input = { orderId: '1', amount: 100, status: 'pending' };
    const result = await processor.process(input);
    expect(result).toEqual({ orderId: '1', amount: 100, status: 'processed' });
  });

  it('should return null for cancelled orders (filtered)', async () => {
    const input = { orderId: '2', amount: 0, status: 'cancelled' };
    expect(await processor.process(input)).toBeNull();
  });

  it('should throw ValidationError for negative amount', async () => {
    const input = { orderId: '3', amount: -50, status: 'pending' };
    await expect(processor.process(input)).rejects.toThrow(ValidationError);
  });
});
```

- [ ] **Integration tests** — Full job with real DB (Testcontainers). Seed data → run → verify output.

```typescript
describe('OrderSyncJob (integration)', () => {
  let app: INestApplication;
  let job: OrderSyncJob;

  beforeAll(async () => {
    const module = await Test.createTestingModule({ imports: [AppModule] })
      .overrideProvider(ConfigService)
      .useValue(testConfig)
      .compile();
    app = module.createNestApplication();
    await app.init();
    job = app.get(OrderSyncJob);
  });

  it('should process all seeded orders', async () => {
    // seed 100 orders
    const result = await job.run();
    expect(result.written).toBe(100);
    expect(result.errors).toBe(0);
  });

  it('should be idempotent on re-run', async () => {
    await job.run(); // first
    await job.run(); // second — upserts, no duplicates
    const count = await repo.count();
    expect(count).toBe(100); // not 200
  });
});
```

- [ ] **BullMQ job tests** — Use `@nestjs/bullmq` testing utilities. Enqueue → process → verify side effects.
- [ ] **Checkpoint/resume test** — Run job → abort after N chunks → re-run → verify resumes correctly.
- [ ] **Skip threshold test** — Feed job with data exceeding skip limit → verify job aborts with correct error.

---

## 11. Configuration

- [ ] **`@nestjs/config` with validation** — Type-safe batch config. Fail fast on invalid values.

```typescript
// batch.config.ts
import { registerAs } from '@nestjs/config';
import { IsInt, IsString, Min, Max, validateSync } from 'class-validator';
import { plainToInstance } from 'class-transformer';

export class BatchConfig {
  @IsInt() @Min(10) @Max(10000)
  chunkSize: number = 1000;

  @IsInt() @Min(0)
  skipLimit: number = 100;

  @IsInt() @Min(1)
  retryAttempts: number = 3;

  @IsInt() @Min(1)
  concurrency: number = 4;

  @IsInt() @Min(60000)
  timeoutMs: number = 7_200_000; // 2 hours

  @IsString()
  redisHost: string = 'localhost';
}

export default registerAs('batch', () => {
  const config = plainToInstance(BatchConfig, {
    chunkSize: parseInt(process.env.BATCH_CHUNK_SIZE, 10),
    skipLimit: parseInt(process.env.BATCH_SKIP_LIMIT, 10),
    retryAttempts: parseInt(process.env.BATCH_RETRY_ATTEMPTS, 10),
    concurrency: parseInt(process.env.BATCH_CONCURRENCY, 10),
    timeoutMs: parseInt(process.env.BATCH_TIMEOUT_MS, 10),
    redisHost: process.env.REDIS_HOST,
  });
  const errors = validateSync(config);
  if (errors.length > 0) throw new Error(`Batch config invalid: ${errors}`);
  return config;
});
```

- [ ] **Config per job** — Different jobs may need different chunk sizes, timeouts, skip limits. Override defaults per job.
- [ ] **Runtime tuning without redeploy** — Env vars for chunk size, concurrency, skip limit. K8s ConfigMap or feature flag service.
- [ ] **Dry-run mode** — `DRY_RUN=true` → processor runs, writer is no-op. Validate pipeline without mutations.

---

## 12. Deployment

- [ ] **Dockerfile per entry point** — Same image, different CMD. Or separate images if resource profiles differ.

```dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN corepack enable && pnpm install --frozen-lockfile
COPY . .
RUN pnpm build

FROM node:20-alpine
WORKDIR /app
COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY --from=build /app/package.json ./

# Default: worker mode. Override for one-shot.
CMD ["node", "dist/main.batch.js"]
```

- [ ] **K8s CronJob for scheduled** — Standalone mode. Run → exit. Same pattern as Go/Spring.
- [ ] **K8s Deployment for workers** — Long-running BullMQ consumer. Horizontal Pod Autoscaler based on queue depth.
- [ ] **Resource limits** — Node batch jobs: 256MB–1GB typical. Set `--max-old-space-size` matching container limit. Streaming keeps memory bounded.
- [ ] **Graceful shutdown** — `enableShutdownHooks()` + `terminationGracePeriodSeconds`. Finish current chunk before exit.
- [ ] **Separate from API** — Batch workers don't share pods with API. Different scaling, different resource profiles, different failure blast radius.
- [ ] **Bull Board in production** — Protected behind auth. Monitor queue health, retry failed jobs, drain queues during incidents.

---

## Quick Sanity Check Before Launch

- [ ] Job is idempotent — re-run produces same result (upserts, skipDuplicates)
- [ ] Checkpoint works — kill mid-run, restart, verify resume from last chunk
- [ ] Skip limit tested — bad records skipped, threshold triggers abort
- [ ] BullMQ retry configured — transient failures auto-retry with backoff
- [ ] No overlapping runs — lock prevents concurrent execution of same job
- [ ] Memory bounded — streaming/pagination, not loading entire dataset
- [ ] Graceful shutdown — SIGTERM → finish chunk → save progress → exit
- [ ] Dead letter captures failures — inspectable, replayable from DB/Bull Board
- [ ] Structured logs — JSON, filterable by job name + execution ID
- [ ] Metrics exposed — items processed, chunk duration, error count
- [ ] Exit code correct (standalone) — 0 success, 1 failure, scheduler detects
- [ ] Health endpoint (worker) — reports unhealthy if queue disconnected or stalled
- [ ] Config validated at startup — missing env var → fail fast, not runtime surprise

---

## NestJS Batch Libraries Reference

| Concern | Package | Notes |
|---------|---------|-------|
| **Queue** | `@nestjs/bullmq` | Redis-backed, retries, priority, rate limit |
| **Scheduling** | `@nestjs/schedule` | Cron, interval, timeout decorators |
| **Dashboard** | `@bull-board/nestjs` | Visual queue monitoring |
| **Distributed lock** | `nestjs-redlock` or `ioredis` | Prevent overlapping runs |
| **DB (TypeORM)** | `@nestjs/typeorm` | Repositories, query builder, migrations |
| **DB (Prisma)** | `prisma` + custom module | Type-safe, cursor pagination built-in |
| **Retry** | `p-retry` | Promise-based retry with backoff |
| **CSV** | `csv-parse` / `csv-stringify` | Streaming CSV read/write |
| **Metrics** | `@willsoto/nestjs-prometheus` | Prometheus counters, histograms |
| **Logging** | `nestjs-pino` | Structured JSON, fast |
| **Config** | `@nestjs/config` | Env-based, validated, typed |
| **Orchestration** | `@temporalio/client` + `@temporalio/worker` | Durable workflows |
| **Testing** | `@nestjs/testing` + Testcontainers | Integration with real DB |
