---
name: nextjs-testing
description: "Test Next.js and React web behavior with Vitest, React Testing Library and Playwright, choosing real framework evidence for routing, async server rendering and caching."
---

# Frontend Web Testing

Apply this policy when adding or changing behavior or tests in the Next.js web application.

## Goal And Timing

Tests protect supported user behavior and important frontend boundaries without freezing component structure or visual
implementation. Every feature ships with the proportionate tests needed to prove its accepted behavior and risks;
tests are part of the same delivery rather than a later stabilization phase. Follow the project’s required test order
and workflow. A regression fix starts with a failing reproduction whenever practical.

Prefer the cheapest test that can fail for the meaningful regression. More tests and higher coverage are not goals by
themselves. Do not test framework, browser, generated-client, or component-library behavior that the application does not own.

## Consistent Shape

- Place `*.test.ts` and `*.test.tsx` in `__tests__` beside the owning file, feature role or shared module.
- Reserve the project-level `tests` directory for cross-cutting contract or build proofs that do not belong to one
  source module.
- Name tests after observable behavior in user or domain language, not component methods or implementation state.
- Structure each test with visible `// Given`, `// When` and `// Then` comments and blank lines between phases.
- Keep one behavior per test. Group a coherent scenario family only when grouping improves navigation.
- Use explicit meaningful fixtures and deterministic values. Keep one-off data and fakes inside the owning test.
- Extract shared setup, builders, or test renderers only after several tests share the same stable requirement.

## Choosing What To Test

### Pure Behavior

Test schemas, mappers, formatters, view-model derivation, reducers, policies, and other deterministic functions through
direct inputs and outputs. Cover meaningful accepted and rejected cases, boundary values, and non-obvious branches.
Do not enumerate equivalent permutations merely to increase coverage.

### Components And Hooks

Test a component when the application owns observable interaction, conditional content, validation feedback, focus behavior,
or an accessibility contract that could regress. Interact through semantic roles, names, labels, and visible outcomes.
Do not assert internal state, hook calls, component trees, styling classes, or incidental DOM structure.

Test a custom hook directly only when it owns reusable behavior that is clearer without a component. Otherwise test
the component or feature outcome that consumes it.

### Server Boundaries

Test public-rendering adapters, metadata derivation, cache policy, and route-owned transformations as ordinary
functions when their contract can be isolated. Use framework-aware setup only when Next.js request, cache, redirect,
header, or rendering semantics are the behavior under test.

### API responses

Exercise the feature's actual response handling against a controlled transport. Check the visible effects of successful data, validation failures and API errors when those cases change. If a generated client is involved, check the configured generation and type interfaces separately from its runtime decoding.

## Maintainability Rules

- Prefer real values, pure functions, and small explicit fakes over broad module mocks.
- Mock time, randomness, browser APIs, or transport only when the scenario owns that boundary; restore global state
  after every test.
- Never mock React or Next.js internals to make an implementation testable.
- Avoid broad snapshots, source-spelling assertions, private-function access, arbitrary delays, retry-based stabilization, and tests
  that only prove rendering did not throw.
- Test how the application composes its UI and responds to user actions.
- Keep tests readable before making them abstract. Small duplication is preferable to a helper that hides the scenario.
- Fix nondeterminism at its source and keep tests independent from execution order, uncontrolled local services, and secrets. Framework/end-to-end tests may start explicitly controlled local services whose lifecycle and fixtures they own.
- Use Vitest and React Testing Library for pure/component behavior, and Playwright for actual browser/framework behavior. Preserve existing configuration; adopting or migrating a runner must be in scope.

## Verification

- Run the narrowest relevant test while developing.
- Run the web test target before completion.
- Run the configured checks for affected API contracts, types and consumers. Regenerate a client when its inputs change.
- Run the production build when routing, server/client boundaries, configuration, or rendering changes.
- Run formatting and repository diff-hygiene checks, and report intentionally skipped checks with the reason.

## Framework and data-flow evidence

Do not pretend Vitest renders asynchronous React Server Components with full Next request/cache semantics. Use the actual Next runtime and Playwright for async rendering, route transitions, redirects, authentication visibility or cache behavior when those boundaries are the claim. A production build is necessary for affected framework boundaries but is not an end-to-end behavior test.

Exercise response compatibility and validation according to the API contract. Cover error feedback, invalidation, mutation retries and dirty form values across refetch when those behaviors change. Isolate the feature's data cache between tests when it uses one. When server prefetch/hydration is used, verify its client interaction and request/identity isolation through the actual framework boundary.

Report browser/runtime checks that could not run, with residual risk and any equivalent evidence. A DOM test cannot substitute for a missing framework proof.

## Readable scenarios and reusable data

The test name, scenario-determining inputs, action and expected result must be visible together. One behavior may need several assertions; do not split one outcome mechanically. Integration tests stay near their boundary owner; reserve transversal locations for truly cross-boundary behavior. Identify which collaborators are real and which are controlled doubles. Helpers must not hide the action, database writes, installed mocks or unrelated setup.

Keep simple values inline. Use targeted typed factories for rich valid objects that are actually reused, with explicit overrides for the scenario's decisive fields. Search existing factories before adding one, and share only identical meaning at the narrowest owner. A private helper may name one coherent step, and a public boundary may have one consumer; neither permits speculative test DSLs, generic object mothers or automatic reflection-based object graphs.

If the project's fixtures generate secondary values, seed them per test and keep scenario-defining values explicit. Control the clock for time-sensitive data.

Consult the [test examples](references/examples.md) when you need to distinguish a form rerender from a refetch in a regression test, or build a typed factory with explicit scenario values. No new E2E infrastructure is introduced by this policy.

Focused architecture tests may protect important dependency boundaries. Avoid tests that assert source spelling or incidental file layout rather than the boundary itself.
