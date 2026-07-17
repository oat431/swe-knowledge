# Angular Launch Checklist

> Tick every box before an Angular app hits production. Framework companion to [[Frontend Launch]]. Angular is opinionated — lean into it.

---

## Project Setup

- [ ] **Angular CLI** — `ng new project --strict --style=scss`. `ng update @angular/cli @angular/core` for upgrades.
- [ ] **Standalone components** — default since v17. No `NgModule` unless you're migrating legacy code or need `@NgModule` for specific module-scoped providers.
- [ ] **TypeScript strict** — `tsconfig.json`: `strict: true`, `noUncheckedIndexedAccess`, `noPropertyAccessFromIndexSignature`. Angular's strict templates: `strictTemplates: true` in `angularCompilerOptions`.
- [ ] **pnpm** (optional) — `ng config cli.packageManager pnpm`. Angular officially supports it.
- [ ] **ESLint** — `@angular-eslint`. Replaced deprecated TSLint since v12. `ng lint` in CI.

---

## Signals (Angular 17+)

- [ ] **`signal()` over `BehaviorSubject`** — reactive state within components and services. `count = signal(0)`. Read: `count()`. Set: `count.set(5)`. Update: `count.update(v => v + 1)`.
- [ ] **`computed()`** — derived state. `doubleCount = computed(() => this.count() * 2)`. Lazy, memoized, auto-tracks deps.
- [ ] **`effect()`** — side effects when signals change. `effect(() => console.log(this.count()))`. Auto-cleanup on destroy. Not for state derivation — use `computed` for that.
- [ ] **`input()` / `output()`** — replace `@Input()` and `@Output()`. `name = input<string>()`. `submit = output<FormData>()`. Type-safe.
- [ ] **`model()`** — two-way binding without `@Input` + `@Output` boilerplate. `value = model(0)`. `[(value)]` in parent.
- [ ] **`linkedSignal()`** — signal that resets when source changes. Good for dependent form state.
- [ ] **`resource()` (experimental)** — declarative async data loading with signals. Wraps HTTP calls with loading/error states. Only replace RxJS for simple fetch scenarios where stream cancellation and retry aren't needed.

---

## State Management

- [ ] **Signals for local state** — component-level state: `signal()`, `computed()`. No store needed for UI state.
- [ ] **NgRx SignalStore** — for global state. Lightweight, signal-based. `withState`, `withComputed`, `withMethods`. Replaces classic `@ngrx/store` (RxJS-based).
- [ ] **No NgRx for new projects** — unless team is already deep in RxJS + NgRx. SignalStore is simpler, smaller bundle.
- [ ] **TanStack Query (Angular)** — `@tanstack/angular-query-experimental`. For server state. `injectQuery`, `injectMutation`. Auto-caching, refetch.
- [ ] **URL as state** — `ActivatedRoute.queryParams` (or `input() + withComponentInputBinding()`). Filters, pagination in URL.

---

## Components

- [ ] **`OnPush` change detection** — default. Signals + `OnPush` = optimal rendering. `markForCheck()` only when needed.
- [ ] **Control flow syntax** — `@if`, `@for`, `@switch` in templates. No more `*ngIf`, `*ngFor`. `@for (item of items; track item.id)` — track is mandatory in v17+.
- [ ] **`@defer`** — lazy-load parts of template. `@defer (on viewport) { <heavy-component /> } @placeholder { <skeleton /> } @loading { <spinner /> } @error { <error /> }`.
- [ ] **`@let`** — declare template-local variables. `@let total = items().reduce(...)`. Replaces complex expressions in bindings.
- [ ] **`ngSrc` / `ngSrcset`** — replace `[src]`. Native lazy-loading, priority hints, preconnect.
- [ ] **`viewChild` / `viewChildren`** — signal-based queries. `canvas = viewChild<ElementRef>('myCanvas')`. No `@ViewChild` decorator.

---

## Dependency Injection

- [ ] **`inject()` function** — replace constructor injection. `private userService = inject(UserService)`. Works anywhere in injection context (components, services, guards, interceptors, directives, pipes).
- [ ] **`providedIn: 'root'`** — tree-shakable singleton services. `@Injectable({ providedIn: 'root' })`. No `providers: []` in NgModule.
- [ ] **Injection token** — `InjectionToken<T>` for non-class dependencies. `export const API_URL = new InjectionToken<string>('API_URL')`.
- [ ] **`takeUntilDestroyed()`** — auto-unsubscribe. `pipe(takeUntilDestroyed())`. No `ngOnDestroy` boilerplate for subscription cleanup.

---

## Routing

- [ ] **Standalone routing** — `provideRouter(routes)`. `Routes = [{ path: '', component: HomeComponent }]`. Lazy: `loadComponent: () => import('./home.component').then(m => m.HomeComponent)`.
- [ ] **Route guards as functions** — `canActivate: [() => inject(AuthService).isLoggedIn()]`. `inject()` in guards.
- [ ] **`withComponentInputBinding()`** — route params → component inputs. `@Input() id!: string` auto-bound from `:id`. Enable in `provideRouter`.
- [ ] **Route resolvers** — `resolve: { user: () => inject(UserService).getUser() }`. Data fetched before navigation. Component receives resolved data via `input()`.
- [ ] **`RouterLink`** — ```<a [routerLink]="['/user', user().id]">```. Active link: `routerLinkActive="active"`.
- [ ] **`ViewTransition`** — `withViewTransitions()` in router. Smooth page transitions with CSS `::view-transition-old/new`. Chrome-only (2024+) but progressively enhanced.

---

## Forms

- [ ] **Reactive forms** — `FormGroup`, `FormControl`, `FormBuilder`. `form = this.fb.group({ email: ['', [Validators.required, Validators.email]]) }`. Type with `FormGroup<{ email: FormControl<string> }>` for strict typing.
- [ ] **Signals with forms** — `form.valueChanges.pipe(takeUntilDestroyed())`. Or `toSignal(form.valueChanges)` for signal-based consumption.
- [ ] **Custom validators** — `function emailDomain(domain: string): ValidatorFn { return (control) => ... }`. Async: `AsyncValidatorFn` returning `Observable<ValidationErrors | null>`.
- [ ] **Form states** — `form.pristine`, `form.dirty`, `form.valid`, `form.invalid`, `form.pending` (for async validators), `form.submitted`. Match every combination: pristine+invalid (no errors shown), dirty+invalid (show errors), valid+submitted (success route).
- [ ] **`[formGroup]` in template** — `[formGroup]="form"`, `formControlName="email"`. Or `[formControl]="form.controls.email"`. Prefer `formControlName` for group forms.
- [ ] **Server-side validation** — Angular only runs client-side rules. Server re-validates all input — display server errors in the same `<mat-error>` or `<div class="error">` component as client-side errors with the form control's `setErrors({ server: message })`.

---

## HTTP & API

- [ ] **`provideHttpClient(withInterceptors([authInterceptor]))`** — functional interceptors. `(req: HttpRequest<unknown>, next: HttpHandlerFn) => ...`.
- [ ] **`httpResource()` (experimental)** — signal-based HTTP. `users = httpResource<User[]>('/api/users')`. Auto-tracks deps, refetch on signal change, loading/error states.
- [ ] **Environment config** — `environment.ts` / `environment.prod.ts`. `provideAppInitializer(() => configService.load())`.
- [ ] **Error interceptor** — catch HTTP errors globally. Show toast, redirect to login on 401.

---

## Styling

- [ ] **`styleUrl` / `styles`** — component-scoped by default (emulated shadow DOM). `ViewEncapsulation.None` only when needed.
- [ ] **Tailwind CSS** — `@tailwindcss/postcss`. Works with Angular CLI natively. `ng add @angular-builders/custom-webpack` if needed.
- [ ] **Component library** — Angular Material (official, accessible, Material 3 in v18+), PrimeNG, or Spartan UI (Radix port for Angular). Don't build dialogs/tooltips from scratch.
- [ ] **CSS variables for theming** — Angular Material theming uses `@use '@angular/material' as mat;`. Custom themes: `mat.define-theme()`.
- [ ] **Dark mode** — toggle `dark` class on `<html>`. Tailwind `dark:` prefix. Material: `@include mat.all-component-colors($dark-theme)`.

---

## Testing

- [ ] **Jasmine / Karma** — default. `ng test`. Or Jest via `@angular-builders/jest` (faster, parallel).
- [ ] **Component tests** — `TestBed.configureTestingModule({ imports: [MyComponent] }).compileComponents()`. `fixture.componentInstance`, `fixture.detectChanges()`.
- [ ] **`fakeAsync` / `tick`** — control time in tests. `fakeAsync(() => { service.getData(); tick(1000); expect(component.data()).toBeDefined(); })`. Stable time = deterministic flaky tests eliminated.
- [ ] **Service tests** — `TestBed.inject()` + `HttpClientTestingModule`. `httpTestingController.expectOne('/api/users').flush(mockUsers)`. Each test explicitly expects its requests — no orphaned calls.
- [ ] **Playwright** — E2E for critical flows. Angular supports it well. `page.waitForSelector('app-user-list')`.
- [ ] **Accessibility tests** — `@axe-core/playwright` in E2E. Jasmine-axe in unit tests for component-level a11y audits.

---

## Security

- [ ] **No `bypassSecurityTrustHtml`** unless absolutely forced — and only with DOMPurify first.
- [ ] **`DomSanitizer`** — Angular sanitizes `[innerHTML]` by default. Don't bypass it.
- [ ] **No secrets in Angular code** — everything ships to the browser. Environment files contain public keys, not secrets.
- [ ] **CSP compatible** — Angular v18+ supports strict CSP without `unsafe-inline`. Templates are AOT-compiled, styles are component-scoped. Verify in production build with `ng serve --configuration production`.
- [ ] **Auth tokens** — HTTP-only cookies preferred. If localStorage: accept XSS risk, keep TTL short.

---

## Performance

- [ ] **AOT compilation** — default in production. Verify: `ng build --configuration production`.
- [ ] **Tree-shaking** — `providedIn: 'root'` services. Standalone components. Only imported code ships.
- [ ] **Lazy routes** — `loadComponent: () => import(...)`. Every route not on the critical path.
- [ ] **`@defer` for below-fold content** — `@defer (on viewport)`. Reduces initial bundle.
- [ ] **`trackBy` / `track`** — `@for (item of items; track item.id)`. Prevents full-list re-render on mutation or filter.
- [ ] **Angular DevTools** — profiler tab. Detect unnecessary change detection cycles. `ng.profiler.timeChangeDetection()`.
- [ ] **Bundle budget** — `angular.json`: `"budgets": [{ "type": "initial", "maximumWarning": "500kb" }]`. Breaks build if exceeded.

---

## Build & Deploy

- [ ] **`ng build --configuration production`** — AOT, minification, dead code elimination, service worker if configured.
- [ ] **`@angular/pwa`** — `ng add @angular/pwa`. Service worker, manifest, offline support. `ngsw-config.json` for cache strategy.
- [ ] **`angular.json` budgets** — warn at 500KB initial, error at 2MB. Catches accidental dependency bloat at build time before it reaches users.
- [ ] **SSR (Angular Universal)** — `ng add @angular/ssr`. `provideServerRendering()`. Hybrid: some routes CSR, some SSR.
- [ ] **`@ngx-translate` or Angular i18n** — i18n built-in since Angular 9. `ng extract-i18n`. Or `@ngx-translate/core` for runtime switching.
- [ ] **Environment configs** — `fileReplacements` in `angular.json`. `environment.ts` → `environment.prod.ts` at build time.

---

## Quick Sanity Check

- [ ] `ng build --configuration production` succeeds — zero build errors
- [ ] `ng test` passes — all tests green
- [ ] `ng lint` passes — zero warnings
- [ ] No `console.log` in production (ESLint `no-console` rule)
- [ ] Lighthouse ≥ 90 on mobile
- [ ] `@defer` on all below-fold content
- [ ] All routes lazy-loaded except the initial landing page
- [ ] Service worker registered (if PWA)
- [ ] CSP tested in production mode (templates + styles pass without violations)
- [ ] `track` on every `@for` loop

---

## Sources

- Angular docs — https://angular.dev/
- `[[Frontend Launch]]` — general frontend checklist (tick first)
