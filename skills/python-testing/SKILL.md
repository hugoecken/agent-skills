---
name: python-testing
description: "Test Python scripts, typed applications and asynchronous boundaries with pytest and controlled fixtures. Includes conditional ingestion characterization and lifecycle testing."
---

# Python testing

Use pytest through the repository's configured verification target. Protect observable behavior and important boundaries; no coverage percentage or mandatory TDD ceremony. Start a regression fix with a failing reproduction when practical. Characterize an affected seam before refactoring it.

## Ordinary tests

Name files `test_<boundary>.py` and tests `test_<observable_behavior>`. Keep arrange, act and assert visibly separated. Test pure functions using meaningful explicit inputs/results. Keep one-off data local; extract fixtures and fakes only when several tests share a stable need. A small utility does not need an ingestion test architecture.

Prefer real values and small handwritten fakes for external seams. Do not mock dataclasses, enums or the object under test. No private-method probing, broad snapshots, source scans, arbitrary sleeps, production data or live providers. Logs need assertions only when their content is supported behavior.

Use controlled HTTP responses for owned method/path/query/headers, serialization, errors, timeout and retry behavior. Generated code is primarily verified by generation/import/type adaptation, not tests of generator syntax. Seed randomness or control time only when it affects the result.

Async tests own and complete all tasks, await cancellation/cleanup and restore clocks/transports. Replace sleeps with events or controlled sleepers. Test client closure, bounded concurrency, failed task propagation and idempotent retry limits when affected.

## Ingestion scenarios

For provider parsing, scheduling, reconciliation or internal API ingestion, read [ingestion test cases](references/ingestion.md). Keep provider fixtures sanitized and reviewed; never refresh them automatically from the network.

## Evidence

Run focused tests during development and the owning verification target before delivery. Include configured Ruff/format/mypy checks and affected generated-client verification; do not silently introduce a missing type checker in an unrelated task. Local controlled smokes are needed only for changed integration boundaries. Report unavailable fixtures, checks or runtime dependencies and the remaining risk. Passing unit tests do not prove an omitted HTTP, scheduling or persistence boundary.
