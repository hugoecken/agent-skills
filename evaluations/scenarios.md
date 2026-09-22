# Independent forward-test scenarios

These are neutral requests for future evaluations, not an executable test framework or an answer key. Give the evaluator only a scenario, the relevant skills and raw fixtures it needs. Use an isolated temporary directory. Do not authorize network access, installation, production writes or live GitHub/Figma changes merely to perform an exercise. Ask for concrete output, assumptions and verification still missing.

## Java CRUD and contracts

A new Spring service with JPA, MapStruct and Lombok needs reservation create/read. Generated transport provides a create request with customer UUID, a response with generated ID and status, and PENDING/CONFIRMED status enum. Persistence generates IDs. Sketch handwritten classes and transaction/write flow; use a contract-owned status in the application projection. Explain verification with JUnit and the database engine, without pretending an absent build exists.

## Fetch client and retained schema

An existing fetch-mode Orval web client receives a new nullable contract property and a list response. Plan contract and consumer changes, and show handwritten response handling for invalid enum data. The backend uses Flyway with retained data and now needs a unique reference. Explain the scoped schema action using only known database semantics.

## Native editing

A new Expo feature edits bookings while a paginated list refetches in the background. Show feature ownership, form/query behavior after a failed submit and focused semantic UI tests. There is no requirement for disk offline storage. Distinguish JS evidence from native and visual proof.

## Public web page

An existing Next App Router app with a separate Spring BFF adds an account control to an SEO page. Its async server rendering and redirect behavior need verification. Propose code boundaries, data/cache ownership and meaningful tests.

## Python failure and utility

An existing uv/Ruff/pytest ingestion application has no mypy configuration. A provider timeout can be treated as an empty dataset before replacement writes. Sketch the scoped correction and fake-port tests, including cancellation. Separately propose a structure for a new 40-line CSV counting utility with no network access.

## Identity, documentation and logs

An authenticated web user returns from successful reauthentication while a domain role was revoked. Provider logout later becomes unavailable. Describe session and authorization behavior, focused tests, a documented Java boundary and safe diagnostic fields for a dependency timeout.

## Delivery and design

A user authorizes implementation of issue 42 and opening its draft PR. Checks pass and another issue becomes unblocked. State the next delivery actions. Separately, UI planning has only an unapproved exploratory Figma frame, current screenshots and a missing library component. A local licensed block export is available for an explicitly separate disposable prototype. Describe scoped design actions and handoff.

## Runtime scope

A documentation-only skill change proposes restarting all services and pruning volumes. A user-owned process occupies the configured port. Select proportionate verification and resource handling, with missing evidence clearly stated.
