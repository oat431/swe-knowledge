# Spring Batch Checklist

> Spring Batch 5.x-specific companion to the general [Batch Processing Checklist](batch.md).
> Covers Spring Batch 5.2+ (Spring Boot 4.x, Java 21+, virtual threads, chunk-oriented processing).
> Last updated: 2026-07-17

---

## 1. Spring Batch Version & Setup

- [ ] **Spring Batch 5.2+** — Part of Spring Boot 4.x ecosystem. Java 21 minimum. Jakarta EE 10. Virtual threads auto-configured for tasklet execution.
- [ ] **Starter dependency** — `spring-boot-starter-batch`. Pulls in Spring Batch core + infrastructure. For testing: `spring-boot-starter-batch-test` (or `spring-batch-test` directly).
- [ ] **Job Repository database** — Spring Batch requires a metadata schema (`BATCH_JOB_INSTANCE`, `BATCH_JOB_EXECUTION`, `BATCH_STEP_EXECUTION`, etc.). Auto-created with `spring.batch.jdbc.initialize-schema: always` (dev) or `never` (production — manage with Flyway/Liquibase).
- [ ] **No in-memory job repository in production** — `MapJobRepository` is for tests only. Production must use JDBC repository for restart/resume, job history, and concurrent job prevention.
- [ ] **`@EnableBatchProcessing` removed in Boot 4** — Spring Boot 4 auto-configures batch infrastructure. Remove `@EnableBatchProcessing` — it now DISABLES auto-config. Define `Job` and `Step` beans directly.
- [ ] **Single job per application (recommended)** — One Spring Boot app = one batch job. Simpler deployment, scaling, monitoring. Multiple jobs in one app: use `spring.batch.job.name` to select which runs.
- [ ] **`spring.batch.job.enabled: false`** — Disable auto-run on startup if triggering externally (scheduler, API, message). Set to `true` only for cron-triggered containers.

## 2. Job Architecture

- [ ] **Job → Step → Chunk/Tasklet** — A `Job` has ordered `Step`s. Each step is either chunk-oriented (read-process-write) or a `Tasklet` (single operation). Most production work is chunk-oriented.
- [ ] **Chunk-oriented processing** — `ItemReader` → `ItemProcessor` → `ItemWriter`. Reader reads one item at a time. Processor transforms. Writer commits in chunks. This is the core pattern.

```java
@Bean
public Step processOrdersStep(JobRepository jobRepository, 
                               PlatformTransactionManager txManager) {
    return new StepBuilder("processOrders", jobRepository)
        .<OrderInput, OrderOutput>chunk(500, txManager)
        .reader(orderReader())
        .processor(orderProcessor())
        .writer(orderWriter())
        .faultTolerant()
        .skipLimit(100)
        .skip(ValidationException.class)
        .retryLimit(3)
        .retry(DeadlockLoserDataAccessException.class)
        .build();
}
```

- [ ] **Tasklet for simple operations** — File cleanup, sending summary email, calling an API once, running a stored procedure. Not for iterating over data — use chunk for that.

```java
@Bean
public Step cleanupStep(JobRepository jobRepository, 
                         PlatformTransactionManager txManager) {
    return new StepBuilder("cleanup", jobRepository)
        .tasklet((contribution, chunkContext) -> {
            fileService.deleteExpiredFiles();
            return RepeatStatus.FINISHED;
        }, txManager)
        .build();
}
```

- [ ] **Chunk size tuning** — Start with 100–1000. Too small: commit overhead dominates. Too large: memory pressure + long rollback on failure. Profile with production-scale data. Different steps can have different chunk sizes.
- [ ] **Job parameters** — Pass runtime config (date range, file path, tenant ID) via `JobParameters`. Immutable per execution. Access in reader/processor/writer via `@Value("#{jobParameters['date']}")` or `StepScope`.

```java
@Bean
@StepScope
public FlatFileItemReader<OrderInput> orderReader(
        @Value("#{jobParameters['inputFile']}") String filePath) {
    return new FlatFileItemReaderBuilder<OrderInput>()
        .name("orderReader")
        .resource(new FileSystemResource(filePath))
        .delimited()
        .names("orderId", "amount", "date")
        .targetType(OrderInput.class)
        .build();
}
```

- [ ] **Step flow — sequential, conditional, parallel** — Sequential: `.start(stepA).next(stepB).next(stepC)`. Conditional: `.on("FAILED").to(errorStep).from(stepA).on("*").to(stepB)`. Parallel: `FlowBuilder` with `SplitState` and `TaskExecutor`.

## 3. ItemReader

- [ ] **Database readers** — `JdbcCursorItemReader` (cursor-based, low memory, single-threaded), `JdbcPagingItemReader` (paging, multi-thread safe, restartable). Paging preferred for production. `JpaPagingItemReader` if using JPA (but raw JDBC is faster for batch reads).
- [ ] **File readers** — `FlatFileItemReader` (CSV/fixed-width), `JsonItemReader`, `StaxEventItemReader` (XML). Configure `linesToSkip` for headers, `encoding` for non-UTF-8.
- [ ] **Custom reader** — Implement `ItemReader<T>`. Return `null` to signal end of input. For API pagination, message queue consumption, or non-standard sources.
- [ ] **Reader thread safety** — `JdbcCursorItemReader` is NOT thread-safe. For multi-threaded steps: use `JdbcPagingItemReader` or wrap with `SynchronizedItemStreamReader`. Or use partitioning instead of multi-threading.
- [ ] **`@StepScope` for late binding** — Readers that depend on `JobParameters` or `StepExecutionContext` must be `@StepScope`. Creates a new instance per step execution. Required for restartability.
- [ ] **`saveState: true` (default)** — Readers track their position in `ExecutionContext`. On restart, reading resumes from last committed chunk. Don't disable unless you handle restart yourself.

## 4. ItemProcessor

- [ ] **Single responsibility** — One processor, one transformation. Chain with `CompositeItemProcessor` if multiple transformations needed. Each processor is independently testable.

```java
@Bean
public CompositeItemProcessor<RawOrder, EnrichedOrder> compositeProcessor() {
    return new CompositeItemProcessorBuilder<RawOrder, EnrichedOrder>()
        .delegates(List.of(
            validationProcessor(),
            enrichmentProcessor(),
            transformProcessor()
        ))
        .build();
}
```

- [ ] **Return `null` to filter** — Processor returns `null` → item is skipped (not written, not counted as error). Use for filtering invalid records or deduplication.
- [ ] **No side effects in processor** — Processors can be retried on failure. Side effects (API calls, writes) in processor = duplicated on retry. Keep processors pure: transform data only. Side effects belong in the writer.
- [ ] **`ItemProcessor` is optional** — If reader output = writer input (no transformation), skip the processor. Don't add a pass-through processor for ceremony.

## 5. ItemWriter

- [ ] **Database writers** — `JdbcBatchItemWriter` (fastest, raw SQL batch), `JpaItemWriter` (entity-managed, slower but convenient). For high volume: JDBC batch always.

```java
@Bean
public JdbcBatchItemWriter<OrderOutput> orderWriter(DataSource dataSource) {
    return new JdbcBatchItemWriterBuilder<OrderOutput>()
        .dataSource(dataSource)
        .sql("""
            INSERT INTO processed_orders (order_id, amount, status, processed_at)
            VALUES (:orderId, :amount, :status, :processedAt)
            ON CONFLICT (order_id) DO UPDATE SET
                amount = EXCLUDED.amount,
                status = EXCLUDED.status,
                processed_at = EXCLUDED.processed_at
            """)
        .beanMapped()
        .build();
}
```

- [ ] **File writers** — `FlatFileItemWriter` (CSV), `JsonFileItemWriter`, `StaxEventItemWriter` (XML). Set `shouldDeleteIfExists: true` or `append: true` depending on use case.
- [ ] **Composite writer** — `CompositeItemWriter` to write to multiple destinations (DB + file, primary + audit table). All writers in the same transaction if possible.
- [ ] **Custom writer** — Implement `ItemWriter<T>`. Receives a `Chunk<T>` (list of items). Bulk operations here: batch API calls, bulk inserts, message publishing.
- [ ] **Idempotent writes** — Use `ON CONFLICT` / `MERGE` / upsert. Job may restart and re-process the last chunk. Writer must handle duplicates gracefully.

## 6. Fault Tolerance (Skip, Retry, Restart)

- [ ] **Skip** — Skip items that cause specific exceptions. `skipLimit(100)` + `.skip(ValidationException.class)`. Skipped items logged via `SkipListener`. Too many skips → step fails (configurable threshold).
- [ ] **Skip policy** — Default `LimitCheckingItemSkipPolicy`. Custom: implement `SkipPolicy` for complex logic (skip first N of type X, skip all of type Y, fail on type Z).
- [ ] **Retry** — Retry transient failures: deadlocks, optimistic lock, network timeouts. `.retryLimit(3).retry(DeadlockLoserDataAccessException.class)`. Exponential backoff with `RetryTemplate` + `BackOffPolicy`.

```java
@Bean
public Step resilientStep(JobRepository jobRepository, 
                           PlatformTransactionManager txManager) {
    return new StepBuilder("resilient", jobRepository)
        .<Input, Output>chunk(500, txManager)
        .reader(reader())
        .processor(processor())
        .writer(writer())
        .faultTolerant()
        .skipLimit(200)
        .skip(DataValidationException.class)
        .noSkip(CriticalDataException.class)
        .retryLimit(3)
        .retry(DeadlockLoserDataAccessException.class)
        .retry(OptimisticLockingFailureException.class)
        .noRetry(ValidationException.class)
        .listener(skipListener())
        .build();
}
```

- [ ] **Restart** — Jobs are restartable by default. On restart: completed steps are skipped, failed step resumes from last committed chunk (if reader saves state). Set `allowStartIfComplete(false)` (default) so completed steps don't re-run.
- [ ] **`preventRestart()`** — For jobs that must NOT be restarted (one-shot migrations, destructive operations). Failed → must be investigated manually, not auto-restarted.
- [ ] **No-rollback exceptions** — Some exceptions shouldn't trigger chunk rollback: `.noRollback(ValidationException.class)`. The transaction commits despite the exception, and processing continues.
- [ ] **Listeners for fault tracking** — `SkipListener` (log skipped items + reason), `RetryListener` (log retry attempts), `ChunkListener` (before/after chunk for progress). Write skipped items to error table for manual review.

## 7. Scaling & Parallelism

- [ ] **Multi-threaded step** — Simplest scaling: set `TaskExecutor` on the step. Each thread processes a chunk independently. Reader MUST be thread-safe (`JdbcPagingItemReader`, `SynchronizedItemStreamReader`). Loses restartability (reader state not per-thread).

```java
@Bean
public Step multiThreadedStep(JobRepository jobRepository,
                               PlatformTransactionManager txManager) {
    return new StepBuilder("parallel", jobRepository)
        .<Input, Output>chunk(200, txManager)
        .reader(pagingReader())
        .processor(processor())
        .writer(writer())
        .taskExecutor(new VirtualThreadTaskExecutor()) // Boot 4 virtual threads
        .throttleLimit(10) // max concurrent chunks
        .build();
}
```

- [ ] **Partitioning (preferred for scaling)** — Master step partitions data by range/key → worker steps process partitions independently. Each partition is a full step with its own reader state. Restartable. Scalable to remote workers.

```java
@Bean
public Step masterStep(JobRepository jobRepository, 
                        Step workerStep) {
    return new StepBuilder("masterStep", jobRepository)
        .partitioner("workerStep", rangePartitioner()) // partition by ID range
        .step(workerStep)
        .gridSize(10) // number of partitions
        .taskExecutor(new VirtualThreadTaskExecutor())
        .build();
}

@Bean
public Partitioner rangePartitioner() {
    return gridSize -> {
        // Query min/max ID, divide into gridSize ranges
        Map<String, ExecutionContext> partitions = new HashMap<>();
        // ... partition by ID range
        return partitions;
    };
}
```

- [ ] **Parallel steps (independent steps concurrently)** — Use `Flow` + `SplitState` when steps have no data dependency. Step A and Step B run simultaneously:

```java
@Bean
public Job parallelJob(JobRepository jobRepository, Step stepA, Step stepB, Step stepC) {
    Flow flowA = new FlowBuilder<SimpleFlow>("flowA").start(stepA).build();
    Flow flowB = new FlowBuilder<SimpleFlow>("flowB").start(stepB).build();
    
    return new JobBuilder("parallelJob", jobRepository)
        .start(flowA)
        .split(new SimpleAsyncTaskExecutor()) // run in parallel
        .add(flowB)
        .next(stepC) // runs after both A and B complete
        .end()
        .build();
}
```

- [ ] **Remote partitioning** — For distributed processing across multiple JVMs. Master sends partition metadata via messaging (RabbitMQ, Kafka). Workers pick up partitions and report back. Use `spring-batch-integration` module.
- [ ] **Virtual threads for I/O-bound steps** — Boot 4 auto-configures virtual threads. Ideal for steps that call external APIs, read from slow sources, or wait on I/O. Thousands of concurrent virtual threads without pool tuning.
- [ ] **Throttle limit** — `throttleLimit(N)` caps concurrent chunk processing. Prevents overwhelming DB connection pool or downstream services. Set ≤ connection pool size.

## 8. Job Scheduling & Triggering

- [ ] **Kubernetes CronJob** — Deploy batch app as K8s CronJob. `spring.batch.job.enabled: true`. Container starts → job runs → exits. Simplest for scheduled execution.

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: order-sync-job
spec:
  schedule: "0 2 * * *"  # 2 AM daily
  concurrencyPolicy: Forbid
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: order-sync
              image: myapp/order-sync:1.2.3
              env:
                - name: SPRING_BATCH_JOB_NAME
                  value: orderSyncJob
              resources:
                limits:
                  memory: "1Gi"
                  cpu: "500m"
          restartPolicy: OnFailure
      backoffLimit: 2
```

- [ ] **REST API trigger** — Expose `JobLauncher` via a controller for on-demand runs. Validate parameters, prevent duplicate runs, return job execution ID.

```java
@RestController
@RequestMapping("/api/v1/jobs")
public class JobController {
    private final JobLauncher jobLauncher;
    private final Job orderSyncJob;

    @PostMapping("/order-sync")
    public ResponseEntity<JobExecutionResponse> launch(
            @RequestBody @Valid JobTriggerRequest request) {
        JobParameters params = new JobParametersBuilder()
            .addString("date", request.getDate())
            .addLong("timestamp", System.currentTimeMillis()) // unique run
            .toJobParameters();
        JobExecution execution = jobLauncher.run(orderSyncJob, params);
        return ResponseEntity.accepted()
            .body(new JobExecutionResponse(execution.getId(), execution.getStatus()));
    }
}
```

- [ ] **Message-triggered** — Listen on Kafka/RabbitMQ topic → launch job with message payload as parameters. For event-driven batch (e.g., "file uploaded" → process file).
- [ ] **`@Scheduled` (simple, not recommended for production)** — Spring's `@Scheduled(cron = "...")` works but lacks: missed-run detection, distributed locking, job history outside the app. Use ShedLock if you must: `@SchedulerLock(name = "orderSync", lockAtLeastFor = "5m")`.
- [ ] **Job uniqueness** — Same `JobParameters` = same `JobInstance`. Can't launch the same instance twice (unless restarting a failed one). Add a unique parameter (`timestamp`, `runId`) for repeated runs with same logical parameters.

## 9. Job Repository & Metadata

- [ ] **Schema management** — Production: manage batch tables with Flyway/Liquibase alongside your app schema. `spring.batch.jdbc.initialize-schema: never`. Schema scripts in `org/springframework/batch/core/schema-*.sql`.
- [ ] **Table prefix** — `spring.batch.jdbc.table-prefix: BATCH_` (default). Change if multiple batch apps share a database: `APP1_BATCH_`, `APP2_BATCH_`.
- [ ] **Execution cleanup** — Job execution history grows forever. Schedule cleanup of old executions (> 90 days) or use `spring.batch.jdbc.isolation-level-for-create` wisely. Custom cleanup job or database procedure.
- [ ] **Separate datasource for job repository** — If your batch job processes data in a different database, configure two datasources: one for batch metadata (`@BatchDataSource`) and one for business data.

```java
@Configuration
public class DataSourceConfig {
    @Bean
    @BatchDataSource
    public DataSource batchDataSource() {
        return DataSourceBuilder.create()
            .url("jdbc:postgresql://localhost/batch_meta")
            .build();
    }

    @Bean
    @Primary
    public DataSource businessDataSource() {
        return DataSourceBuilder.create()
            .url("jdbc:postgresql://localhost/business_data")
            .build();
    }
}
```

- [ ] **Job execution status** — Query `JobExplorer` for execution status, history, running instances. Expose via actuator or custom endpoint for monitoring dashboards.

## 10. Testing

- [ ] **`@SpringBatchTest`** — Provides `JobLauncherTestUtils` and `JobRepositoryTestUtils`. Launch full jobs or individual steps in tests.

```java
@SpringBatchTest
@SpringBootTest
class OrderSyncJobTest {
    @Autowired
    private JobLauncherTestUtils jobLauncherTestUtils;

    @Test
    void shouldProcessAllOrders() {
        // given: test data seeded in DB
        
        JobExecution execution = jobLauncherTestUtils.launchJob(
            new JobParametersBuilder()
                .addString("date", "2026-07-17")
                .toJobParameters()
        );

        assertThat(execution.getStatus()).isEqualTo(BatchStatus.COMPLETED);
        assertThat(execution.getStepExecutions())
            .extracting(StepExecution::getWriteCount)
            .containsExactly(1000);
    }
}
```

- [ ] **Step-level testing** — `jobLauncherTestUtils.launchStep("processOrders")`. Test individual steps in isolation. Faster feedback than full job runs.
- [ ] **Reader/Processor/Writer unit tests** — Test each component independently. Reader: verify it reads expected items. Processor: verify transformation logic (pure function). Writer: verify correct SQL/output.
- [ ] **Testcontainers for integration** — Real database (PostgreSQL, MySQL) in containers. Realistic data volume. Verify: batch metadata written correctly, business data correct, no constraint violations.
- [ ] **Fault tolerance tests** — Inject failures: reader throws on specific items → verify skip/retry behavior. Kill mid-run → restart → verify resume from checkpoint. Verify skipped items logged correctly.
- [ ] **Idempotency test** — Run job → run same job again (new `JobInstance`). Verify: no duplicate records in target, upserts work correctly.
- [ ] **Performance test** — Run with production-scale data. Measure: total duration, memory peak, DB connection usage, throughput (items/sec). Compare chunk sizes and thread counts.

## 11. Observability & Monitoring

- [ ] **Actuator integration** — `spring-boot-starter-actuator` exposes batch metrics. Job execution count, step execution count, active jobs, last execution status.
- [ ] **Micrometer metrics** — Spring Batch 5 auto-publishes: `spring.batch.job.active` (gauge), `spring.batch.job` (timer), `spring.batch.step` (timer), `spring.batch.item.read`/`process`/`write` (counters). Custom metrics in listeners.
- [ ] **Custom `JobExecutionListener`** — Log job start/end, duration, read/write/skip counts. Alert on failure. Push metrics to external monitoring.

```java
@Component
public class JobMetricsListener implements JobExecutionListener {
    private final MeterRegistry meterRegistry;

    @Override
    public void afterJob(JobExecution execution) {
        execution.getStepExecutions().forEach(step -> {
            log.info("Step [{}] completed: read={}, written={}, skipped={}, duration={}ms",
                step.getStepName(),
                step.getReadCount(),
                step.getWriteCount(),
                step.getSkipCount(),
                Duration.between(step.getStartTime(), step.getEndTime()).toMillis()
            );
        });
        
        if (execution.getStatus() == BatchStatus.FAILED) {
            meterRegistry.counter("batch.job.failures", 
                "job", execution.getJobInstance().getJobName()).increment();
        }
    }
}
```

- [ ] **Structured logging** — Every log line includes: job name, execution ID, step name, chunk number. MDC: `MDC.put("jobName", ...)`, `MDC.put("executionId", ...)`. Correlate all logs from one execution.
- [ ] **Progress reporting** — `ChunkListener.afterChunk()` to emit progress. Or `StepExecutionListener` to report "step 2/5 complete." Long jobs: log every N chunks ("processed 10,000/100,000 — 10%").
- [ ] **Health check** — Custom actuator health indicator: is a required daily job stuck? Has it missed its schedule? Last successful run > expected threshold → unhealthy.
- [ ] **Alerting** — Job `FAILED` → immediate alert. Job running longer than 2x expected → alert. Job didn't start within expected window → alert. Skip rate > threshold → warning.

## 12. Error Handling & Dead Letter

- [ ] **SkipListener → error table** — Every skipped item: write to an error/quarantine table with the original data, exception message, job execution ID, timestamp. Inspectable and replayable.

```java
@Component
public class ErrorTableSkipListener implements SkipListener<OrderInput, OrderOutput> {
    private final JdbcTemplate jdbc;

    @Override
    public void onSkipInProcess(OrderInput item, Throwable t) {
        jdbc.update("""
            INSERT INTO batch_errors (job_execution_id, item_id, error_type, error_message, raw_data, created_at)
            VALUES (?, ?, ?, ?, ?::jsonb, now())
            """,
            getCurrentExecutionId(), item.getOrderId(),
            t.getClass().getSimpleName(), t.getMessage(),
            objectMapper.writeValueAsString(item)
        );
    }
}
```

- [ ] **Exit code mapping** — Map job outcome to process exit code for K8s/scheduler: `COMPLETED` → 0, `FAILED` → 1, `STOPPED` → 2. Scheduler detects failure and alerts.

```java
@Bean
public CommandLineRunner exitCodeMapper(JobExecution execution) {
    return args -> {
        if (execution.getStatus() == BatchStatus.FAILED) {
            SpringApplication.exit(applicationContext, () -> 1);
        }
    };
}
```

- [ ] **No silent swallowing** — Never catch and ignore exceptions in reader/processor/writer. Either skip (with listener logging), retry, or fail the step. Unknown exceptions should fail fast.
- [ ] **Transaction rollback on chunk failure** — Default behavior: if writer throws, entire chunk rolls back. Items are re-read (if reader supports it) and re-processed. Ensure processor is side-effect-free (Section 4).

## 13. Security & Configuration

- [ ] **Secrets via Spring Vault or env vars** — Database credentials, API keys: never in `application.yml`. Use `spring-cloud-starter-vault-config`, Kubernetes secrets mounted as env vars, or AWS Secrets Manager integration.
- [ ] **Least privilege DB user** — Batch job user: `SELECT` on source tables, `INSERT/UPDATE` on target tables, full access to batch metadata tables. Not `DBA` or `ALL PRIVILEGES`.
- [ ] **Profile-based configuration** — `application-batch.yml` for batch-specific config (chunk sizes, thread counts, file paths). Activate with `SPRING_PROFILES_ACTIVE=batch,prod`.
- [ ] **Externalize tuning parameters** — Chunk size, skip limit, retry limit, thread count, throttle limit: all configurable without code change. `@Value` or `@ConfigurationProperties`.

```java
@ConfigurationProperties(prefix = "app.batch.order-sync")
@Validated
public record OrderSyncProperties(
    @Min(10) @Max(10000) int chunkSize,
    @Min(0) int skipLimit,
    @Min(1) int retryLimit,
    @Min(1) int threadCount,
    @NotBlank String inputDirectory
) {}
```

- [ ] **Sensitive data in logs** — Don't log full record contents if they contain PII. Log record IDs, counts, and status — not payloads. Mask sensitive fields in error table entries.

## 14. Deployment & Operations

- [ ] **Container image** — Same as any Spring Boot app. Multi-stage Docker build. `spring-boot:build-image` for Cloud Native Buildpacks. Non-root user. Memory limits set (`-Xmx` or container limits).
- [ ] **Resource requests/limits** — Batch jobs are memory-hungry (chunk buffers, connection pools). Set realistic limits. Monitor actual usage and adjust. OOMKilled = chunk size too large or memory leak.
- [ ] **Graceful shutdown** — `spring.lifecycle.timeout-per-shutdown-phase: 60s`. On SIGTERM: current chunk completes, progress commits, step marks as `STOPPED` (restartable). K8s `terminationGracePeriodSeconds` > max chunk processing time.
- [ ] **Job restart after pod crash** — K8s `restartPolicy: OnFailure` + Spring Batch restartability. Pod crashes → K8s restarts → job resumes from last committed chunk. Verify this works in staging.
- [ ] **Separate from API deployments** — Batch jobs shouldn't share pods/containers with API services. Different scaling needs, different resource profiles, different failure modes. Separate deployment manifests.
- [ ] **Runbook** — How to: manually trigger a job, check execution status, restart a failed job, replay skipped items from error table, adjust chunk size for a slow run, kill a stuck job, backfill a date range.
- [ ] **Log aggregation** — Ship batch job logs to centralized system (ELK, Loki, CloudWatch). Filter by job name + execution ID. Short-lived containers make local log access impractical.

---

## Quick Sanity Check Before Launch

- [ ] Job runs end-to-end in staging with production-scale data
- [ ] Restart tested — kill mid-run, restart, verify resume from checkpoint (no duplicates, no gaps)
- [ ] Skip/retry configured — bad records don't kill the batch, transient failures are retried
- [ ] Chunk size profiled — memory bounded, throughput acceptable, commit frequency reasonable
- [ ] Job uniqueness enforced — same job can't run concurrently (CronJob `concurrencyPolicy: Forbid` or DB lock)
- [ ] Error table captures skipped items — inspectable, replayable
- [ ] Metrics exposed — execution duration, read/write/skip counts, failure alerts
- [ ] Graceful shutdown works — SIGTERM mid-chunk → chunk completes → progress saved
- [ ] No secrets in config — credentials from vault/env, not committed
- [ ] Idempotent writes — job re-run produces same result (upserts, ON CONFLICT)
- [ ] Exit code maps to job status — scheduler/K8s detects failures correctly
- [ ] Backfill tested — can process historical date range without affecting current data
- [ ] Connection pool sized for batch load — not competing with API services

---

## Common Patterns

### Daily ETL Sync
```
Job: dailySyncJob
├── Step 1: extractStep (read from source API/DB → write to staging table)
├── Step 2: transformStep (read staging → validate, enrich → write to target)
├── Step 3: reconcileStep (tasklet: compare counts, log discrepancies)
└── Step 4: notifyStep (tasklet: send completion email/webhook)
```

### File Processing
```
Job: fileProcessingJob
├── Step 1: validateFileStep (tasklet: check file exists, validate header)
├── Step 2: processRecordsStep (chunk: FlatFileItemReader → processor → JdbcBatchItemWriter)
├── Step 3: archiveFileStep (tasklet: move file to archive directory)
└── Step 4: reportStep (tasklet: generate summary, push metrics)
```

### Partitioned High-Volume
```
Job: bulkProcessingJob
├── Step 1: preparationStep (tasklet: create temp tables, gather stats)
├── Step 2: masterProcessingStep (partitioned: split by ID range → N worker steps)
│   ├── Worker partition 0: IDs 1-100,000
│   ├── Worker partition 1: IDs 100,001-200,000
│   └── Worker partition N: ...
├── Step 3: aggregationStep (tasklet: merge results, update summaries)
└── Step 4: cleanupStep (tasklet: drop temp tables, archive logs)
```
