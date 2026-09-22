---
name: python-development
description: "Implement or refactor typed Python scripts and applications with explicit lifecycle and errors. Includes conditional guidance for asynchronous provider ingestion; does not impose an ingestion architecture on ordinary scripts."
---

# Python development

Read the requested behavior, repository Python version, `pyproject.toml`, lock and actual run/check targets. For new managed Python projects (an application/package with its own pyproject) use uv, Ruff, mypy and pytest. A standalone script inside an existing repository follows its existing tooling; creating a script alone does not authorize a new toolchain or dependency installation. Keep the pinned interpreter and dependencies. Adding this skill does not install mypy in an existing project, migrate packaging or upgrade dependencies without an accepted task.

## Ordinary Python first

Use a `src` layout for a new installable package. Keep a small script small; do not add layers, a package or dependency injection solely to match an application template. Prefer focused pure functions; classes should own meaningful state or lifecycle. Compose at an explicit entry point and perform no I/O, startup or scheduling at import time.

Type stable signatures and boundaries; infer obvious locals. Use built-in generics and supported union syntax for the pinned Python. Narrow dynamic input instead of propagating `Any` or untyped dictionaries. Prefer pathlib, context managers, enums and dataclasses over custom frameworks.

Use frozen dataclasses for immutable value/result roles where appropriate. `frozen=True` is shallow: nested collections still need deliberate immutable representation. A construction or reconciliation phase can own explicit mutable state rather than forcing every intermediate into a frozen copy.

Catch precise exceptions where recovery is possible. Preserve causes and meaningful context without leaking payloads. Do not turn failure into `None`, an empty collection or invented data unless that is the accepted outcome. Validate typed configuration once at startup. Avoid mutable global state, generic serializers, registries, broad inheritance and a DI framework.

## Resources and asynchronous work

Use context managers and one clear resource owner. Reuse an application-lifetime HTTPX async client for related requests and close it at shutdown. Do not create a client per record, nest event loops or call blocking requests/sleeps from an async path.

Created tasks must have a lifetime, bounded concurrency, awaited result or supervised lifecycle, cancellation propagation and cleanup. Preserve cancellation rather than swallowing it in generic recovery. Retry only the failing idempotent I/O boundary, with bounded attempts/backoff and a stopping condition; do not retry the whole application implicitly.

## Provider ingestion mode

Only when the task actually concerns provider parsing, synchronization, scheduling or internal API ingestion, read [ingestion boundaries](references/ingestion.md). Do not load that architecture for an unrelated CLI or small utility.

## Evidence

Use focused observable tests, then the owning verification target with configured lint/format/type checks. New projects include mypy; report its absence honestly in an existing project rather than claiming it passed. Regenerate and import affected clients when contracts change. Keep controlled fixtures and local services; never validate through production writes. Inspect generated ignored files, final diff and resource cleanup. A refactor preserves supported behavior unless an accepted correction changes it.

Mechanics: [Python typing](https://docs.python.org/3/library/typing.html), [asyncio tasks](https://docs.python.org/3/library/asyncio-task.html) and [HTTPX async clients](https://www.python-httpx.org/async/).
