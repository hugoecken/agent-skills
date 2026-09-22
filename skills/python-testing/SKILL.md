---
name: python-testing
description: "Test Python scripts, typed applications and asynchronous boundaries with pytest and controlled fixtures. Includes conditional ingestion characterization and lifecycle testing."
---

# Python testing

Use pytest through the repository's configured verification target. Protect observable behavior and important boundaries; no coverage percentage or mandatory TDD ceremony. Start a regression fix with a failing reproduction when practical. Characterize an affected seam before refactoring it.

## Ordinary tests

Mirror production feature/role ownership under `tests`; keep integrations beside their boundary owner, not in a global unit/integration split. Name files `test_<boundary>.py` and tests `test_<observable_behavior>`. Use visible `# Given`, `# When` and `# Then` comments and blank lines between phases. Test pure functions using meaningful explicit inputs/results. Keep one-off data local; extract fixtures and fakes only when several tests share a stable need. A small utility does not need an ingestion test architecture.

Prefer real values and small handwritten fakes for external seams. Do not mock dataclasses, enums or the object under test. No private-method probing, broad snapshots, source-spelling assertions, arbitrary sleeps, production data or live providers. Logs need assertions only when their content is supported behavior.

Use controlled HTTP responses for owned method/path/query/headers, serialization, errors, timeout and retry behavior. Generated code is primarily verified by generation/import/type adaptation, not tests of generator syntax. Seed randomness or control time only when it affects the result.

Async tests own and complete all tasks, await cancellation/cleanup and restore clocks/transports. Replace sleeps with events or controlled sleepers. Test client closure, bounded concurrency, failed task propagation and idempotent retry limits when affected.

## Ingestion scenarios

For provider parsing, scheduling, reconciliation or internal API ingestion, read [ingestion test cases](references/ingestion.md). Keep provider fixtures sanitized and reviewed; never refresh them automatically from the network.

## Evidence

Run focused tests during development and the owning verification target before delivery. Include configured Ruff/format/mypy checks and affected generated-client verification; do not silently introduce a missing type checker in an unrelated task. Local controlled smokes are needed only for changed integration boundaries. Report unavailable fixtures, checks or runtime dependencies and the remaining risk. Passing unit tests do not prove an omitted HTTP, scheduling or persistence boundary.

## Readable scenarios and reusable data

The test name, scenario-determining inputs, action and expected result must be visible together. One behavior may need several assertions; do not split one outcome mechanically. Integration tests stay near their boundary owner; reserve transversal locations for truly cross-boundary behavior. Identify which collaborators are real and which are controlled doubles. Helpers must not hide the action, database writes, installed mocks or unrelated setup.

Keep simple values inline. Use targeted typed factories for rich valid objects that are actually reused, with explicit overrides for the scenario's decisive fields. Search existing factories before adding one, and share only identical meaning at the narrowest owner. A private helper may name one coherent step, and a public boundary may have one consumer; neither permits speculative test DSLs, generic object mothers or automatic reflection-based object graphs.

Use Faker when generated secondary values are useful, as a test dependency with a project-owned compatible version. Use a native fixed seed per test and explicit construction; never mutable random state shared across parallel tests. Control the clock for time-sensitive data. Assert scenario values, not a hard-coded string tied to a Faker version or call order. Keep boundary cases explicit and parameterized. Small tests do not require a data library, and installing this skill does not add dependencies.

Read [canonical examples](references/examples.md) when adding or substantially restructuring tests or factories. They show a unit test, an integration boundary and a rich typed factory, not a new test framework. For a substantive behavior change run the owning suite, necessary integration proofs and affected consumers; mandatory repository checks still apply. No new E2E infrastructure is introduced by this policy.

Focused architecture tests may protect important dependency boundaries. Avoid tests that assert source spelling or incidental file layout rather than the boundary itself.
