# Provider ingestion

## Active roles

Use only roles that real behavior needs: `application` owns use cases, commands, ports and run results; `domain` owns pure values, reconciliation and invariants; `infrastructure` owns provider HTTP/parsing, generated clients and scheduling; `config` owns typed startup settings; the bootstrap composes lifecycle. An observability module is useful only when logging/metrics configuration warrants it. No empty role tree or generic managers/processors.

Use `Protocol` at real replaceable outbound seams, not on every class. Keep domain/application independent of HTTPX, parsing-library nodes, raw dictionaries and generated transport packages.

## Parsing, contracts and writes

Separate download, decode, parse, normalize, match and internal writes. A parser takes controlled text/bytes/document input and returns typed provider records without network calls. Keep provider encodings, vocabulary, quirks and markup inside the provider adapter.

Generated internal clients, object models **and enums** remain in the internal API adapter. Explicit typed functions map application/domain values to requests and responses back. Do not make generated transport classes the domain or introduce a mapping framework. This stricter Python boundary is deliberate even when other languages reuse contract-owned enums.

Preserve source priority, aliases, missing values, malformed-record treatment, dates, identifiers and matching fallbacks unless the task accepts a correction. Name pure policies for decisions. Do not create a universal parser for unrelated providers.

Represent create/update/replace/skip/no-op and authoritative-completeness decisions explicitly. An unavailable or partial provider response is not an authoritative empty dataset: never convert a failure into emptiness that drives deletion or replacement. Keep idempotence, write order, batching and partial-failure semantics visible.

## Runtime

Bound concurrency and prevent overlapping scheduled runs according to accepted behavior. Share and close HTTP clients. Own and await every task; test cancellation and shutdown. Retry idempotent I/O, not the entire reconciliation/writing sequence. Preserve actual schedules, timeouts, backoff, TLS, proxy and startup settings unless explicitly changed.

Catch unexpected failures once at the run/process boundary with safe context. Summarize outcomes and bounded failures; no per-record success noise or raw provider data. Logging cannot change decisions.

## Refactoring evidence

Characterize the old seam before replacement. Feed old and new implementations the same sanitized fixtures and compare typed normalized records, reconciliation decisions, request values, ordered writes and failure/cancellation outcomes. Differences block replacement unless they are accepted and tested corrections. Never run two production implementations as active writers.

Fixtures are reviewed controlled inputs, never silently refreshed from live providers. Run the owning tests and configured verification target, affected client generation and controlled local integration only where needed. Do not migrate packaging, Docker or a type checker alongside behavior unless the task owns both.
