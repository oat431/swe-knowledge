# Jest / Vitest Testing Checklist

> The **JS/TS unit & integration testing** checklist — framework-specific companion to [[qa]].
> Vitest is preferred for Vite-based projects (React, Vue, SvelteKit, Astro). Jest for legacy or non-Vite stacks (Next.js Pages Router, Express, NestJS).
> Covers: describe/it blocks, matchers, mocking, spies, snapshots, async, coverage, component testing, common patterns, pitfalls.
> Last updated: 2026-08-07

---

## 1. Framework Selection & Setup

- [ ] **Vitest for Vite projects** — If your project uses Vite (React + Vite, Vue, SvelteKit, Astro, Nuxt 3), use Vitest. It shares Vite's config, transform pipeline, and module resolution. Zero duplicate config.
- [ ] **Jest for non-Vite stacks** — Next.js (Pages Router / older App Router setups), Express, NestJS, plain Node.js. Jest with `ts-jest` or `@swc/jest` for TypeScript.
- [ ] **Next.js 14+ with Turbopack** — Prefer Vitest. Jest requires `next/jest` config wrapper and doesn't support Turbopack transforms yet. Vitest works natively.
- [ ] **Install & configure** — Vitest: `npm i -D vitest @testing-library/react @testing-library/jest-dom jsdom`. Jest: `npm i -D jest ts-jest @testing-library/react @testing-library/jest-dom`.
- [ ] **Config file** — Vitest: `vitest.config.ts` (or inline in `vite.config.ts`). Jest: `jest.config.ts`. Both support `setupFiles`, `setupFilesAfterEach`, `globals`, `environment`.
- [ ] **TypeScript paths resolved** — `tsconfig.json` `paths` aliases (`@/`, `~/`) must be resolved in test config. Vitest: automatic via Vite config. Jest: `moduleNameMapper` required.
- [ ] **Test environment set** — `environment: 'jsdom'` for component tests, `'node'` for backend/unit. Vitest supports per-file env via `// @vitest-environment jsdom` comment.
- [ ] **Globals enabled** — `test: { globals: true }` (Vitest) or `@types/jest` installed (Jest). Prefer explicit imports (`import { describe, it, expect } from 'vitest'`) for clarity and IDE support.
- [ ] **Setup files** — `setupFiles` runs before test framework loads (env vars, polyfills). `setupFilesAfterFramework` runs after (custom matchers, global mocks). Keep these separate and minimal.

---

## 2. Test Structure (describe / it / test)

- [ ] **`describe` blocks group by module/feature** — One top-level `describe` per file, nested `describe` per function or behavior category. Max 2 levels of nesting — deeper = too granular.
- [ ] **`it` / `test` describes expected behavior** — `it('returns empty array when no items exist')` not `it('getItems')`. The test name is the spec — readable without opening the body.
- [ ] **AAA pattern enforced** — Arrange → Act → Assert. Visually separated by blank lines. If you can't identify each section, the test is too complex.
- [ ] **One assertion concept per test** — Multiple `expect()` calls on the same behavior OK (e.g., check status + body). Multiple behaviors in one test = split it.
- [ ] **No test interdependence** — Tests must run in any order. `vitest --sequence.shuffle` or `jest --randomize` in CI proves this. Shared state between tests is a defect.
- [ ] **`beforeEach` for setup, not logic** — Use for resetting mocks, creating fresh fixtures. Never put assertions or complex logic in hooks — they're invisible when reading a test.
- [ ] **`beforeAll` only for expensive, immutable setup** — DB connections, server starts. Never for state that tests mutate. Pair with `afterAll` for cleanup.
- [ ] **`afterEach` for cleanup** — `vi.restoreAllMocks()`, `jest.restoreAllMocks()`, DOM cleanup. Prevents cross-test pollution. Better than manual restore in each test.

```ts
// ✅ Good structure
describe('UserService', () => {
  describe('createUser', () => {
    it('creates user with hashed password', () => {
      // Arrange
      const input = { email: 'a@b.com', password: 'secret123' };
      // Act
      const result = await service.createUser(input);
      // Assert
      expect(result.password).not.toBe('secret123');
      expect(result.email).toBe('a@b.com');
    });

    it('throws on duplicate email', () => { /* ... */ });
  });
});
```

---

## 3. Matchers & Assertions

- [ ] **Equality matchers** — `toBe()` for primitives/references, `toEqual()` for deep object comparison, `toStrictEqual()` for type-strict deep (catches `undefined` vs missing key).
- [ ] **Truthiness** — `toBeTruthy()`, `toBeFalsy()`, `toBeNull()`, `toBeUndefined()`, `toBeDefined()`, `toBeNaN()`. Prefer specific over vague: `toBeNull()` > `toBeFalsy()`.
- [ ] **Numbers** — `toBeCloseTo()` for floating point (never `toBe(0.1 + 0.2, 0.3)`). `toBeGreaterThan()`, `toBeLessThanOrEqual()`.
- [ ] **Strings** — `toContain()`, `toMatch(/regex/)`, `toHaveLength()`. For error messages: `toThrow(/expected message/)`.
- [ ] **Arrays & iterables** — `toContain()`, `toContainEqual()`, `toHaveLength()`, `arrayContaining()`, `objectContaining()`.
- [ ] **Objects** — `toMatchObject()` (partial match), `toHaveProperty('path.to.key', value)`, `objectContaining()` for subset assertions.
- [ ] **Exceptions** — `toThrow()`, `toThrow(ErrorClass)`, `toThrow(/message/)`. Must wrap in function: `expect(() => fn()).toThrow()`.
- [ ] **`@testing-library/jest-dom` matchers** — `toBeInTheDocument()`, `toBeVisible()`, `toHaveTextContent()`, `toHaveAttribute()`, `toHaveClass()`, `toBeDisabled()`. Essential for DOM testing.
- [ ] **Custom matchers** — `expect.extend({ toBeValidUser(received) { ... } })`. Register in setup file. Good for domain-specific assertions reused across tests.
- [ ] **`expect.assertions(n)`** — Declare expected assertion count. Catches early returns that skip assertions. Critical for async tests with callbacks.

---

## 4. Mocking (jest.fn / vitest.fn)

- [ ] **`vi.fn()` / `jest.fn()` for function mocks** — Create standalone mock functions. `vi.fn().mockReturnValue(42)`, `.mockResolvedValue(data)`, `.mockRejectedValue(error)`.
- [ ] **`vi.mock()` / `jest.mock()` for module mocks** — Mock entire modules. Hoisted to top of file (runs before imports). `vi.mock('./api', () => ({ fetchData: vi.fn() }))`.
- [ ] **`vi.doMock()` for non-hoisted mocks** — Use when mock depends on runtime state. Not hoisted — runs at call site. Pair with dynamic `import()`.
- [ ] **`vi.importActual()` / `jest.requireActual()`** — Get real module inside a mock. Useful for partial mocks: spread real exports, override one function.
- [ ] **Mock implementations** — `.mockImplementation((arg) => ...)`, `.mockImplementationOnce()` for sequential returns. `.mockName('myMock')` for readable failure messages.
- [ ] **Auto-mocking** — `vi.mock('./module')` without factory auto-generates mocks for all exports. Convenient but opaque — prefer explicit factories for clarity.
- [ ] **Reset mocks in `afterEach`** — `vi.restoreAllMocks()` or `jest.restoreAllMocks()`. Or configure `restoreMocks: true` in config. Forgotten mocks are the #1 source of cross-test pollution.
- [ ] **Mock return values vs implementations** — `mockReturnValue` for static data, `mockImplementation` when the mock needs logic (conditional returns, argument-dependent behavior).

```ts
// ✅ Explicit mock factory
vi.mock('@/services/auth', () => ({
  AuthService: {
    login: vi.fn().mockResolvedValue({ token: 'test-token' }),
    logout: vi.fn().mockResolvedValue(undefined),
  },
}));
```

---

## 5. Spies

- [ ] **`vi.spyOn()` / `jest.spyOn()`** — Wrap existing methods to track calls without replacing behavior. `const spy = vi.spyOn(console, 'error')`.
- [ ] **Spy assertions** — `spy.toHaveBeenCalled()`, `.toHaveBeenCalledTimes(n)`, `.toHaveBeenCalledWith(args)`, `.toHaveBeenLastCalledWith(args)`, `.toHaveBeenNthCalledWith(n, args)`.
- [ ] **Spy + override implementation** — `spy.mockImplementation(() => ...)`. Track calls AND change behavior. Restore with `spy.mockRestore()`.
- [ ] **`mock.calls` / `mock.results`** — Inspect call history directly. `spy.mock.calls[0]` = first call args, `spy.mock.results[0].value` = return value. Useful for complex assertion logic.
- [ ] **Spy on constructor** — `vi.spyOn(Module, 'ClassName')` doesn't work for classes. Use `vi.mock()` instead or spy on prototype methods.
- [ ] **Spy on timers** — `vi.useFakeTimers()` + `vi.spyOn(Date, 'now')`. Always pair with `vi.useRealTimers()` in cleanup.
- [ ] **Spy call order** — `expect(spy1).toHaveBeenCalledBefore(spy2)` (jest-extended) or check `spy.mock.invocationCallOrder`. Important for lifecycle/event tests.

---

## 6. Snapshot Testing

- [ ] **Use for stable, complex output** — Serialized component trees, JSON responses, error messages. NOT for frequently changing UI or simple values.
- [ ] **`toMatchSnapshot()` vs `toMatchInlineSnapshot()`** — Inline snapshots live in test file (self-documenting, easier review). File snapshots in `__snapshots__/` dir (cleaner test files). Prefer inline for small outputs.
- [ ] **Named snapshots** — `toMatchSnapshot('login-form-rendered')`. Prevents ambiguity when a test has multiple snapshots.
- [ ] **Custom serializers** — `expect.addSnapshotSerializer()` for cleaner output. Strip volatile fields (timestamps, UUIDs) before snapshotting.
- [ ] **Update snapshots deliberately** — `vitest -u` or `jest -u`. Review diffs before committing. Blind `-u` hides regressions.
- [ ] **Snapshot size limit** — If a snapshot exceeds ~50 lines, it's testing too much. Break into focused assertions or smaller component snapshots.
- [ ] **Avoid snapshot anti-patterns** — Don't snapshot entire pages (too brittle). Don't snapshot API responses that change often. Don't use snapshots as a substitute for meaningful assertions.
- [ ] **Property matchers in snapshots** — `expect(obj).toMatchSnapshot({ id: expect.any(String), createdAt: expect.any(Date) })`. Volatile fields matched by type, not value.

---

## 7. Async Testing

- [ ] **Always `await` or return promises** — `it('...', async () => { await expect(fn()).resolves.toBe(value) })`. Forgetting `await` = test passes regardless.
- [ ] **`resolves` / `rejects` matchers** — `await expect(promise).resolves.toEqual(data)`, `await expect(promise).rejects.toThrow(Error)`. Cleaner than try/catch.
- [ ] **`waitFor` for eventual state** — `@testing-library/react`'s `waitFor(() => expect(...))`. Retries until assertion passes or timeout. For UI that updates after async ops.
- [ ] **`findBy*` queries** — `await screen.findByText('Loaded')`. Built-in `waitFor` + `getBy*`. Prefer over manual `waitFor` for simple DOM assertions.
- [ ] **Fake timers** — `vi.useFakeTimers()` + `vi.advanceTimersByTime(5000)`. Test setTimeout/setInterval/debounce without real delays. Always restore in `afterEach`.
- [ ] **`vi.waitFor()`** — Vitest's built-in wait utility. `await vi.waitFor(() => expect(mock).toHaveBeenCalled(), { timeout: 3000 })`.
- [ ] **`act()` wrapper** — React state updates in async tests must be wrapped in `act()`. `@testing-library/react` does this automatically for most APIs, but manual state triggers need explicit wrapping.
- [ ] **Test timeouts** — `it('...', async () => {}, 10000)` for slow integration tests. Don't increase global timeout — isolate per-test.
- [ ] **Callback-based async** — `done` callback pattern (Jest legacy). Prefer promisifying: `new Promise((resolve) => callback(resolve))`. `done` is error-prone and deprecated in Vitest.

```ts
// ✅ Async patterns
it('loads user data on mount', async () => {
  render(<UserProfile userId="123" />);
  // findBy* = waitFor + getBy*
  await expect(screen.findByText('John Doe')).resolves.toBeInTheDocument();
  expect(screen.queryByText('Loading...')).not.toBeInTheDocument();
});
```

---

## 8. Coverage

- [ ] **Coverage tool configured** — Vitest: `vitest --coverage` (uses `@vitest/coverage-v8` or `c8`). Jest: `jest --coverage` (uses Istanbul). V8 coverage is faster and more accurate.
- [ ] **Thresholds set** — `coverage.thresholds: { branches: 80, functions: 80, lines: 80, statements: 80 }`. Enforce per-PR — coverage must not decrease.
- [ ] **Exclude non-testable code** — Config excludes: `index.ts` barrel files, type-only files, config files, generated code. `coverage.exclude` array.
- [ ] **Branch coverage matters most** — Line coverage misses untested `else` branches. Branch coverage catches them. Set branch threshold equal to line threshold.
- [ ] **Uncovered lines reviewed** — After running coverage, inspect uncovered lines. Are they error handlers? Edge cases? Dead code? Each uncovered line should have a reason.
- [ ] **Coverage is not quality** — 100% coverage with bad assertions = false confidence. Mutation testing (Stryker) validates that tests actually detect changes → [[qa]] §8.
- [ ] **CI coverage reports** — Upload to Codecov/Coveralls. Diff coverage per PR (new lines must be covered). Badge in README for visibility.
- [ ] **Ignore comments** — `/* v8 ignore next */` or `/* istanbul ignore next */` for genuinely unreachable code. Document why. Review these in PR — they're escape hatches, not convenience.

---

## 9. Component Testing (React / Testing Library)

- [ ] **`@testing-library/react`** — `render()`, `screen`, `fireEvent` / `userEvent`. Test from the user's perspective: what they see and interact with.
- [ ] **Query priority** — `getByRole` > `getByLabelText` > `getByPlaceholderText` > `getByText` > `getByTestId`. Use `testId` only as last resort (non-user-facing elements).
- [ ] **`userEvent` over `fireEvent`** — `userEvent` simulates real browser events (focus, input, change sequence). `fireEvent` dispatches single events. Use `userEvent` for realistic interaction testing.
- [ ] **`renderHook` for hooks** — `@testing-library/react`'s `renderHook(() => useMyHook())`. Test custom hooks in isolation without wrapper components.
- [ ] **Component mocks** — Mock child components when testing parent behavior: `vi.mock('./ChildComponent', () => ({ default: () => <div data-testid="child-mock" /> }))`.
- [ ] **Context providers in tests** — Create test wrappers: `render(<Component />, { wrapper: AllProviders })`. Include Router, Theme, Auth, QueryClient.
- [ ] **Portal & modal testing** — `baseElement` vs `container`. Portals render outside component tree — use `screen.getByText()` (searches whole document).
- [ ] **Accessibility assertions** — `jest-axe` or `@axe-core/playwright`. `expect(await axe(container)).toHaveNoViolations()`. Run on every component render test.
- [ ] **Avoid testing implementation details** — Don't test state values directly, don't test method calls on instances. Test rendered output and user interactions only.

```ts
// ✅ Component test pattern
it('submits form and shows success message', async () => {
  const user = userEvent.setup();
  render(<ContactForm />);

  await user.type(screen.getByLabelText('Email'), 'test@example.com');
  await user.type(screen.getByLabelText('Message'), 'Hello');
  await user.click(screen.getByRole('button', { name: /send/i }));

  await expect(screen.findByText('Message sent!')).resolves.toBeInTheDocument();
});
```

---

## 10. Common Patterns

- [ ] **Test utilities file** — `src/test/utils.tsx` with `renderWithProviders()`, mock factories, common setup. DRY without over-abstraction.
- [ ] **Factory functions for test data** — `createMockUser(overrides?)`, `createMockOrder(overrides?)`. Use spread: `{ ...defaults, ...overrides }`. Better than raw object literals in every test.
- [ ] **Parameterized tests** — Vitest: `it.each([cases])`. Jest: `test.each([cases])` or `test.each\`\`table\`\``. Eliminates copy-paste for similar cases.
- [ ] **Custom test hooks** — `function setupComponent(props = {}) { ... return { user, ...result } }`. Encapsulates render + queries + user setup. Keep under 15 lines.
- [ ] **MSW for API mocking** — `msw` (Mock Service Worker) intercepts network requests at the service worker level. More realistic than module mocks for integration tests. Define handlers per test suite.
- [ ] **Test categories with tags** — Vitest: `describe.skip()`, `it.skip()`, `test.todo()`. Jest: `xit`, `xdescribe`. Use `.todo()` to document planned tests.
- [ ] **Conditional test execution** — `describe.skipIf(condition)` (Vitest). Skip platform-specific or env-dependent tests cleanly.
- [ ] **Retry flaky tests (temporarily)** — `it('...', { retry: 3 }, async () => {})` (Vitest) or `jest-retries`. Fix the flake — retries are a band-aid, not a solution.

```ts
// ✅ Parameterized test
it.each([
  { input: '', expected: 'Required' },
  { input: 'ab', expected: 'Min 3 chars' },
  { input: 'valid@email', expected: null },
])('validates "$input" → "$expected"', ({ input, expected }) => {
  const error = validate(input);
  expect(error).toBe(expected);
});
```

---

## 11. Vitest-Specific Features

- [ ] **`vi` global** — Vitest's mocking/spy/timer API. `vi.fn()`, `vi.mock()`, `vi.spyOn()`, `vi.useFakeTimers()`, `vi.hoisted()`. Drop-in compatible with `jest` API.
- [ ] **`vi.hoisted()`** — Declare variables that need to be available before `vi.mock()` hoisting. `const mocks = vi.hoisted(() => ({ fn: vi.fn() }))`.
- [ ] **`vi.stubEnv()` / `vi.stubGlobal()`** — Set environment variables or globals for a test, auto-restored. `vi.stubEnv('API_KEY', 'test')`.
- [ ] **Workspace mode** — Monorepo testing: `vitest --project=api --project=web`. Different configs per package, shared runner.
- [ ] **Browser mode** — `@vitest/browser` runs tests in real browsers (Playwright/WebDriver). More accurate than jsdom for CSS/layout-sensitive tests.
- [ ] **Type testing** — `expectTypeOf(value).toEqualTypeOf<Expected>()` and `assertType()`. Test your types at compile time. Unique to Vitest.
- [ ] **Benchmark tests** — `bench('name', () => {}, { iterations: 1000 })`. Performance regression testing alongside unit tests.
- [ ] **`pool: 'threads'` / `'forks'`** — Thread-based parallelism (default, faster) vs fork-based (more isolated, better for native modules). Switch if tests have side effects.
- [ ] **`--changed` flag** — Run tests only for files changed since last commit. Fast feedback in CI for large monorepos.

---

## 12. Pitfalls & Anti-Patterns

- [ ] **❌ Not awaiting async assertions** — `expect(fetchData()).resolves.toBe(42)` without `await` passes immediately. The test is meaningless.
- [ ] **❌ Mocking everything** — Over-mocking = testing the mock, not the code. Mock boundaries (HTTP, DB, filesystem), not internal functions.
- [ ] **❌ Snapshot-only testing** — Snapshots catch shape changes but not logic errors. Pair with behavioral assertions.
- [ ] **❌ Shared mutable state** — Variables declared outside `describe`/`it` that tests modify. Use `beforeEach` to reset or declare inside each test.
- [ ] **❌ Testing implementation details** — Asserting on internal state, private methods, or component instance properties. Test user-visible behavior only.
- [ ] **❌ Brittle selectors** — `querySelector('.css-1a2b3c')` breaks on style changes. Use `getByRole`, `getByLabelText`, semantic queries.
- [ ] **❌ Catching errors in tests** — `try { fn(); } catch(e) { expect(e.message).toBe('...') }`. Use `expect(() => fn()).toThrow()` instead — cleaner and catches "no error thrown" bugs.
- [ ] **❌ Ignoring test warnings** — React `act()` warnings, console errors, deprecation notices. They signal real problems. Fix them, don't suppress.
- [ ] **❌ `beforeEach` doing too much** — 50-line setup hooks are unreadable. Extract into named functions: `setupAuthenticatedUser()`, `seedDatabase()`.
- [ ] **❌ Coverage chasing** — Writing tests for coverage percentage, not for behavior. Test what matters, not what's easy to test.
- [ ] **❌ Timezone-dependent tests** — `new Date('2024-01-01')` behaves differently across timezones. Use `vi.setSystemTime()` or fixed UTC dates.
- [ ] **❌ Not cleaning up timers** — `vi.useFakeTimers()` without `vi.useRealTimers()` in `afterEach` breaks subsequent tests.

---

## Quick Sanity Check

- [ ] Framework matches build tool (Vitest for Vite, Jest for everything else)
- [ ] All tests use AAA structure, one behavior per test
- [ ] Mocks reset in `afterEach` — no cross-test pollution
- [ ] Async tests properly awaited — no silent failures
- [ ] Coverage thresholds set and enforced in CI
- [ ] Component tests use Testing Library queries (role > label > text > testId)
- [ ] No snapshot-only tests — behavioral assertions present
- [ ] Parameterized tests used for repetitive cases
- [ ] Fake timers cleaned up in `afterEach`
- [ ] Test utils/factories in shared file — no copy-paste

---

## Project Tier Scoping Matrix

> **How to use this table:** Pick your tier first, then focus only on the sections marked ✅ (required) or 🟡 (recommended). Skip ❌ sections entirely.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

### Tier Descriptions

| # | Tier | Description | Typical Team | Users | Lifespan |
|---|---|---|---|---|---|
| 1 | 🧪 **POC / Spike** | Validate an idea. Maybe a smoke test. | 1 dev | Internal only | Days–weeks |
| 2 | 🔧 **Prototype / MVP** | Waiting for integration or user validation. | 1–2 devs | Beta testers | Weeks–months |
| 3 | 🏠 **Internal Tool** | Real users (employees), real traffic. | 1–3 devs | Employees | Ongoing |
| 4 | 🟢 **Small Production** | Single service/app, low traffic. Real users. | 1–2 devs | < 1K users | Ongoing |
| 5 | 🔵 **Medium Production** | Multiple services or higher traffic. Real revenue. | 2–5 devs | 1K–100K users | Ongoing |
| 6 | 🟣 **Production Grade** | Full rigor — high-stakes SaaS, enterprise product. | 5+ devs | 100K+ users | Long-term |
| 7 | 🔴 **Mission-Critical / Regulated** | Healthcare, finance, safety systems. | 10+ devs | Varies | Decades |

### Checklist Applicability by Tier

| # | Section | 🧪 POC | 🔧 Prototype | 🏠 Internal | 🟢 Small Prod | 🔵 Medium Prod | 🟣 Production Grade | 🔴 Mission-Critical |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | Framework Selection & Setup | 🟡 any works | 🟡 Vitest if Vite | ✅ Vitest + config | ✅ + path aliases | ✅ + workspace | ✅ + browser mode | ✅ + formal V&V |
| 2 | Test Structure | ❌ | 🟡 basic AAA | ✅ describe/it | ✅ + hooks | ✅ + parameterized | ✅ + custom hooks | ✅ + formal |
| 3 | Matchers & Assertions | 🟡 basic | 🟡 equality only | ✅ full set | ✅ + jest-dom | ✅ + custom matchers | ✅ + domain matchers | ✅ + formal |
| 4 | Mocking | ❌ | 🟡 vi.fn only | ✅ vi.mock + spies | ✅ + MSW | ✅ + factories | ✅ + full isolation | ✅ + formal |
| 5 | Spies | ❌ | ❌ | 🟡 basic | ✅ call tracking | ✅ + call order | ✅ + lifecycle | ✅ + audit trail |
| 6 | Snapshot Testing | ❌ | ❌ | 🟡 inline only | ✅ + serializers | ✅ + property matchers | ✅ + versioned | ✅ + reviewed |
| 7 | Async Testing | 🟡 await | 🟡 basic | ✅ waitFor/findBy | ✅ + fake timers | ✅ + timeouts | ✅ + race conditions | ✅ + formal |
| 8 | Coverage | ❌ | ❌ | 🟡 lines only | ✅ + branches 80% | ✅ + diff coverage | ✅ + mutation test | ✅ + regulatory |
| 9 | Component Testing | ❌ | 🟡 smoke render | ✅ Testing Library | ✅ + userEvent | ✅ + a11y | ✅ + visual regression | ✅ + formal |
| 10 | Common Patterns | ❌ | ❌ | 🟡 factories | ✅ + MSW + utils | ✅ + parameterized | ✅ + full suite | ✅ + formal |
| 11 | Vitest-Specific | ❌ | ❌ | 🟡 if using | ✅ workspace | ✅ + type testing | ✅ + browser + bench | ✅ + full |
| 12 | Pitfalls | 🟡 read | ✅ avoid | ✅ | ✅ | ✅ + team review | ✅ + enforced | ✅ + audited |

---

## Sources

- Complements [[qa]] (general testing strategy), [[api]] (API testing patterns).
- Vitest docs: https://vitest.dev/guide/
- Jest docs: https://jestjs.io/docs/getting-started
- Testing Library: https://testing-library.com/docs/react-testing-library/intro/
- MSW: https://mswjs.io/docs/
- Stryker Mutator: https://stryker-mutator.io/
- Next.js testing guide: https://nextjs.org/docs/app/building-your-application/testing
