# NestJS API Checklist

> NestJS-specific companion to [[API Launch]]. Tick the general checklist first. Assumes TypeScript + Express (default) or Fastify.

---

## Project Setup

- [ ] **`@nestjs/cli`** — `nest new project --strict`. TypeScript strict mode on.
- [ ] **`tsconfig.json`** — `strict: true`, `noUncheckedIndexedAccess`, `noUnusedLocals`, `paths` for `@/` aliases.
- [ ] **ESLint + Prettier** — `@nestjs/eslint-config`. Pre-commit: `lint-staged` + `husky`.
- [ ] **SWC builder** — `@nestjs/swc`. Faster than tsc. Swap in `nest-cli.json`. Or Bun/Vite for extreme speed.

---

## Module Structure

```
src/
├── common/            ← guards, interceptors, pipes, filters, decorators
├── config/            ← `@nestjs/config` module + validation
├── database/          ← TypeORM/Prisma module
├── modules/
│   ├── user/          ← controller, service, repository, dto, entity, module
│   └── order/
├── app.module.ts
└── main.ts
```

- [ ] **Feature modules** — each domain is a `@Module({ imports, controllers, providers, exports })`. No god module.
- [ ] **`AppModule` imports feature modules** — nothing else. `main.ts` bootstraps `AppModule`.
- [ ] **No circular imports** — `forwardRef(() => Module)` only as last resort. Usually means cross-cutting logic needs extracting.

---

## `main.ts` Bootstrapping

- [ ] **Global pipes** — `app.useGlobalPipes(new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true }))`
- [ ] **Global filters** — `app.useGlobalFilters(new HttpExceptionFilter())` for consistent error shape.
- [ ] **Global interceptors** — `ClassSerializerInterceptor` + `@Exclude()/@Expose()` on entities.
- [ ] **Global prefix** — `app.setGlobalPrefix('api/v1')`.
- [ ] **CORS** — `app.enableCors({ origin: config.cors.origins })`. Specific origins, not `*`.
- [ ] **Shutdown hooks** — `app.enableShutdownHooks()`. Gracefully closes DB connections, message brokers.
- [ ] **Versioning** — `app.enableVersioning({ type: VersioningType.URI })` or `Header` type.

---

## Controllers

- [ ] **`@Controller('users')`** — route prefix on class. Method decorators: `@Get(':id')`, `@Post()`, `@Patch(':id')`, `@Delete(':id')`.
- [ ] **DTOs with validation** — `class CreateUserDto { @IsEmail() email: string; @MinLength(8) password: string; }`
- [ ] **`@UseGuards(JwtAuthGuard)`** — on class or method. `@Public()` decorator for unauthenticated routes.
- [ ] **`@ApiTags`, `@ApiOperation`, `@ApiResponse`** — Swagger decorators. Not on every method if verbose, but on every controller.
- [ ] **Controller is thin** — parse request into DTO → call service → transform response DTO. No business logic.

---

## Providers & DI

- [ ] **Services are `@Injectable()`** — injected via constructor. `constructor(private userService: UserService) {}`
- [ ] **Repository pattern** — `@Injectable()` repository class wrapping TypeORM `Repository<T>` or Prisma client. Injected into services.
- [ ] **Custom providers** — `useFactory`, `useClass`, `useValue` for non-class dependencies (config objects, third-party clients).
- [ ] **`@Inject()`** only for custom tokens — constructor injection with class types doesn't need it.

---

## Validation & Transformation

- [ ] **`class-validator` decorators** — `@IsString()`, `@IsInt()`, `@IsEnum()`, `@IsOptional()`, `@ValidateNested()`. On every DTO field.
- [ ] **`class-transformer`** — `@Type(() => NestedDto)` for nested objects. `@Transform()` for custom transforms.
- [ ] **ValidationPipe global** — `whitelist: true` (strip unknown), `forbidNonWhitelisted: true` (error on unknown), `transform: true` (auto-type).
- [ ] **Custom validators** — `@ValidatorConstraint()`. For domain rules beyond built-in decorators.
- [ ] **Validation groups** — `@ValidateIf()` or separate DTOs for create vs update. Groups get confusing — separate DTOs are simpler.

---

## Database (TypeORM / Prisma)

- [ ] **TypeORM** — `@nestjs/typeorm`. `TypeOrmModule.forRootAsync()` with config. `@Entity()` classes. Repository pattern via `@InjectRepository()`.
- [ ] **Prisma** — `PrismaService extends PrismaClient implements OnModuleInit`. `@Injectable()` singleton. No extra repository layer needed — Prisma IS the repository.
- [ ] **`onModuleInit`** — `await this.$connect()` in Prisma service. `onModuleDestroy` — `await this.$disconnect()`.
- [ ] **Migrations** — TypeORM: `typeorm migration:generate`. Prisma: `prisma migrate dev`. Versioned, committed.
- [ ] **Transactions** — TypeORM: `dataSource.transaction()`. Prisma: `prisma.$transaction()`. Interactive transactions for complex logic.
- [ ] **Connection pool** — TypeORM: `extra: { max: 20 }`. Prisma: `connection_limit` in datasource URL.

---

## Authentication & Authorization

- [ ] **`@nestjs/passport` + `@nestjs/jwt`** — `PassportModule`, `JwtModule.registerAsync()`. `JwtStrategy extends PassportStrategy(Strategy)`.
- [ ] **`JwtAuthGuard`** — `extends AuthGuard('jwt')`. Applied globally via `APP_GUARD`.
- [ ] **`@Public()` decorator** — `SetMetadata('isPublic', true)`. Custom guard checks `Reflector` to skip auth on public routes.
- [ ] **`@Roles('admin')`** — `RolesGuard` checks roles from JWT. Combined with `@UseGuards(JwtAuthGuard, RolesGuard)`.
- [ ] **`@CurrentUser()` decorator** — `createParamDecorator((_, ctx) => ctx.switchToHttp().getRequest().user)`. Type-safe user extraction.

---

## Error Handling

- [ ] **Custom exception filter** — `@Catch() implements ExceptionFilter`. Catches `HttpException` + unknown errors. Returns `{ statusCode, message, timestamp, path }`.
- [ ] **`NotFoundException` over generic errors** — `throw new NotFoundException('User not found')`.
- [ ] **Domain exceptions** — `throw new BadRequestException('Insufficient balance')`. NestJS maps to 400.
- [ ] **No stack traces in production** — `exceptionFilter` checks `NODE_ENV` before including stack.

---

## Configuration

- [ ] **`@nestjs/config`** — `ConfigModule.forRoot({ isGlobal: true, validate })`. `ConfigService` injected, never `process.env` directly.
- [ ] **Config validation** — `Joi` or `class-validator` on config object. `validate(config)` throws on missing/invalid env vars at startup. Fail fast.
- [ ] **`.env` gitignored** — `.env.example` committed with dummy values.
- [ ] **`registerAs`** for namespaced config — `database.config.ts`, `auth.config.ts`. `@Inject(databaseConfig.KEY)`.

---

## Logging

- [ ] **`Logger` from `@nestjs/common`** — `private readonly logger = new Logger(UserService.name)`. Class-scoped.
- [ ] **Custom logger** — `NestFactory.create(AppModule, { logger: new MyLogger() })`. Pino or Winston via adapter.
- [ ] **Request logging** — `morgan` middleware or custom interceptor. `:method :url :status :response-time ms`.
- [ ] **No secrets in logs** — filter out `password`, `token`, `authorization` from request/response logging.

---

## Testing

- [ ] **Jest** — `@nestjs/testing`. `Test.createTestingModule({ imports: [], providers: [] }).compile()`.
- [ ] **Unit tests** — services with mocked repositories. `jest.fn()` or `jest-mock-extended`. Fast, parallel.
- [ ] **Controller tests** — `app = module.createNestApplication().init()`. `supertest(app.getHttpServer())`. Real HTTP, no server listen.
- [ ] **E2E tests** — full app with test DB. `beforeAll` starts app + runs migrations. `afterAll` closes.
- [ ] **`@golevelup/ts-jest`** — `createMock<Repository<User>>()`. Cleaner than manual `jest.fn()` for complex mocks.
- [ ] **Test DB** — Docker `postgres:16-alpine` via Testcontainers. Not SQLite (different dialect).

---

## Swagger / OpenAPI

- [ ] **`@nestjs/swagger`** — `SwaggerModule.createDocument(app, config)`. `SwaggerModule.setup('api/docs', app, document)`.
- [ ] **`@ApiProperty()` on DTOs** — type, example, description, required/optional. This IS your API documentation.
- [ ] **`@ApiBearerAuth()`** — marks endpoints needing JWT. Global via `addBearerAuth()` in swagger config.
- [ ] **`@ApiTags('Users')` on controllers** — groups endpoints in Swagger UI.

---

## Performance

- [ ] **Compression** — `compression` middleware via `app.use(compression())`.
- [ ] **Helmet** — `app.use(helmet())`. Security headers.
- [ ] **Rate limiting** — `@nestjs/throttler`. `ThrottlerModule.forRoot({ ttl: 60, limit: 100 })`. `@Throttle()` or `@SkipThrottle()`.
- [ ] **Fastify** — `NestFactory.create(AppModule, new FastifyAdapter())`. Faster than Express. `@nestjs/platform-fastify`.
- [ ] **Clustering** — `cluster` module (Node) or PM2. Use worker threads if CPU-bound.

---

## Quick Sanity Check

- [ ] `ValidationPipe` global with `whitelist` and `transform` — no unexpected fields, auto-typed
- [ ] `JwtAuthGuard` global (or on protected routes via controller-level)
- [ ] `@Public()` decorator works — health/login/register accessible without token
- [ ] `enableShutdownHooks()` called — graceful DB disconnect on SIGTERM
- [ ] `ConfigModule` validates at startup — missing `DATABASE_URL` fails fast, not at first request
- [ ] Error filter returns consistent `{ statusCode, message, timestamp, path }`
- [ ] Swagger UI at `/api/docs` (dev only or gated behind basic auth in prod)
- [ ] Migrations committed + run automatically in CI/CD — no `synchronize: true` in prod
- [ ] `.env` not committed, `.env.example` is

---

## Sources

- NestJS docs — https://docs.nestjs.com/
- `[[API Launch]]` — general API checklist (tick first)
- `[[03 Authentication]]`, `[[03 API Security]]` — auth patterns
