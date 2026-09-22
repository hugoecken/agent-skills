---
name: python-testing
description: "Write and review readable pytest tests for typed Python use cases, mapping, configuration, generated clients and async lifecycle. Includes conditional provider-ingestion evidence."
---

# Python testing

Use the repository's verification targets and actual interpreter/tool versions. Tests protect observable behavior and important boundaries; no universal coverage target or mandatory TDD ceremony. A regression starts with a failing reproduction when practical; a refactor characterizes the changed seam. New handwritten modules and their tests use mypy strict. Existing projects adopt missing tooling separately, never as a side effect of installing this skill.

## Location and readable shape

Mirror feature/role ownership under `tests`, keeping integration tests beside their boundary owner. Use `test_<boundary>.py` and `test_<observable_behavior>`. Reserve transversal locations for actual cross-boundary scenarios, not a global unit/integration split. Use visible `# Given`, `# When`, `# Then` phases; a combined `# When / Then` is appropriate for an exception assertion enclosing the operation.

The title, decisive values, real/replaced dependencies, action and expected outcome must be visible together. Several assertions may prove one behavior. Do not hide the action, persistence writes or installed mocks in setup helpers. Keep simple data inline; extract meaningful shared factories or provider setup only for actual repeated needs. Public boundaries may have one consumer, and a private helper may name one coherent step; neither justifies a test DSL or generic object mother.

## What to prove

| Boundary                    | Proof                                                                                                                     |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Pure calculation or mapping | Concrete inputs/outputs, owned invariants, intentional omission, nested fields and absence/null/enum distinctions         |
| Application orchestration   | Real commands/results with fakes of application-owned external needs; meaningful writes, rejections and no-write cases    |
| Provider adapter            | Controlled bytes/JSON/HTML/CSV through the real parser, validation and mapping                                            |
| Generated API adapter       | Real generated request/response path through controlled transport; actual serialization, decoding and translated failures |
| Settings and bootstrap      | Required/invalid values, source precedence, failure before I/O, safe diagnostics and resource assembly/closure            |
| Async lifecycle             | Task ownership, cancellation, cleanup, bounded active/pending work, repeated-run independence and retry limits            |

An application test must not import a concrete adapter to mock its internals. Prefer real dataclasses, enums and generated model values; never mock the object under test. A recording fake exposes only the behavior needed by the scenario and does not become a second framework. Assert collaborator calls only when they are the contract, such as an emitted write or forbidden action.

Source generation/imports prove compatibility, not every runtime guarantee. If validation is the claim, exercise the actual generated client's decoding path using malformed required fields/types/enums/nulls/forbidden extra properties. Record unsupported guarantees as failures, not successful coverage. A handwritten stand-in model cannot certify generated behavior. Do not test generator spelling or default example methods.

## Configuration and async scenarios

Use synthetic settings and explicitly selected temporary environment files. Isolate environment changes through test fixtures and prove the declared source priority. Check that an invalid configuration opens no clients or scheduler. Verify rendered failures omit supplied secrets; do not snapshot raw validation exceptions.

Exercise successful and failing context exits and client closure. Prefer controlled HTTPX transports for adapter logic; disclose that these do not prove sockets/TLS. Reuse the repository's async test mode; a small standalone case can use `asyncio.run` rather than introducing a second async plugin. Do not nest event loops or reuse a client across incompatible test loops.

Use events/barriers or controlled transports to coordinate concurrency/cancellation; no wall-clock sleeps for stability. Prove the concurrency limit actually bounds work, that cancelling one run leaves no surviving tasks and that a second call on the same service has fresh run state. Tests may inspect an exposed client `is_closed` property because resource closure is the contract, not private state.

Partial, malformed and failed data remain distinct from authoritative empty data. For a replacement operation prove failure/partial results cannot trigger deletion or replacement. A cancellation test before a write proves no write began; it cannot prove that cancelling an already-sent request rolled back the remote operation. Test that writes are not implicitly retried unless idempotency is part of their contract.

## Data, typing and examples

Use targeted typed factories for rich valid objects that tests actually share. Put scenario-determining values in explicit parameters/overrides. Faker may supply secondary fields using its standard per-test fixture seed or an instance-local seed; no mutable random generator shared across parallel tests and no custom replay system. Fix relevant time/locale inputs. Explicit boundary cases remain parameterized, not entrusted to random chance. Installing this skill does not add Faker to a small test that only needs literal values.

Annotate test functions, fixtures, fake methods and factory inputs/returns. New code runs mypy strict; do not silence the entire test directory to accommodate mocks or generated imports. Isolate a demonstrated library typing limitation narrowly and report its scope. No `Any` leakage or unsafe cast used to pretend a boundary is validated.

Read [canonical examples](references/examples.md) when adding or restructuring tests; read [targeted test data](references/test-data.md) when a rich reused object needs a factory. The examples use the same kinds of contracts across unit, integration and lifecycle evidence; they are not a new testing framework. For provider synchronization, read [ingestion scenarios](references/ingestion.md) only when that behavior is in scope.

## Verification

Run focused tests during development, then the owning suite and affected consumers/integrations. Include configured Ruff/format and mypy strict for the new managed scope. Meaningful dependency-boundary tests are allowed; avoid source-wording checks, private-method probing, broad snapshots, arbitrary sleeps or live providers. Report passes, actual failures, expected failures and unavailable proof separately. Keep generated output ignored and inspect the final diff. No new E2E infrastructure is introduced by this policy.
