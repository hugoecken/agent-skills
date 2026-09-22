# Generator and validation boundaries

## Java

Generate server interfaces and DTOs into the configured build output. Implement interfaces in API adapters. Keep generated client models in infrastructure adapters and map object DTOs immediately to application/domain values. Contract-owned generated enums may be used directly in application APIs only when they carry exactly the intended concept; they never enter pure domain code.

Keep shared generated model packages limited to genuinely shared enums and technical primitives. Compile through the owning reactor so annotation processing and generated imports are exercised. Prove unions, enum serialization and null handling with actual generated output where configuration changes their behavior.

## Python

Use the repository-pinned OpenAPI Generator and asynchronous HTTPX configuration, with one declarative batch configuration per adopted contract. Respect the owning internal/public boundary; language does not change service ownership. Keep generated classes, models and enums inside the infrastructure API adapter in ingestion applications. Map to typed application/domain values using explicit functions. Validate imports and actual request/response behavior against controlled transport; never production data.

## TypeScript, Orval and Zod

Use Orval for client functions, transport types and Zod response schemas from the same source contract. Reuse its generated query keys/functions where the existing mode supplies them; do not assume a fetch-only project already generates Query hooks.

Inspect the installed Orval version and native output modes. Select supported Zod generation and a supported transport/mutator boundary for that version. If the existing mode needs an adoption change, make it explicit; do not invent an API or patch generated output. Keep handwritten transport behavior thin and outside generated directories.

For every consumed successful JSON response, run its generated schema at the transport boundary **once**, before the request resolves as success and before TanStack Query stores it. Rejected validation is a typed boundary failure, not partial success or a cast. Handle 204, streams and non-JSON responses according to their contract instead of parsing them as JSON. Route non-success HTTP statuses through their error contract, not the success schema. Preserve credentials, cancellation signals, timeouts, status/headers where required and safe error handling.

Form Zod schemas remain separate when editing has different defaults, intermediate strings or cross-field rules. Do not force a transport schema onto an unfinished form or validate the same server response again in every screen.

Tests should demonstrate a valid response admitted, a malformed response rejected before cache success, correct no-content/error handling and preserved cancellation when affected. TypeScript compilation alone cannot prove runtime validation.

Mechanics: [Orval documentation](https://orval.dev/overview) and [OpenAPI Generator configuration](https://openapi-generator.tech/docs/configuration/). Check the installed version before using optional features.
