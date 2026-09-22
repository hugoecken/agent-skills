# Targeted skill evaluations

These are neutral task prompts, not an executable framework or an answer key. Replay relevant scenarios for material policy changes; not every edit needs a run. Keep this file in the pack repository, never in copied project skills. Provide the selected skills and minimum raw artifacts. Use an isolated temporary workspace and request concrete output, assumptions and missing verification. No production writes, live GitHub/Figma changes or dependency installation are implied.

A successful exercise checks applicability and may reveal ambiguity. It does not certify an application or prove improvement over an agent without the skill. An efficacy claim needs comparable with/without-skill runs, equal task/context and observable outcomes. Record executed checks separately from review conclusions.

## Java reservation

A Spring/JPA service adds reservation create/read. Its generated API request contains a customer UUID, response contains a database-generated ID and status, and the contract defines PENDING/CONFIRMED. The repository uses MapStruct and Lombok. Provide the handwritten structure, write/read flow and focused tests. Identify evidence that cannot be obtained without the actual database/build.

## Python backend feature and utility

A new Python application periodically reads a partner catalogue and publishes available items through an internal API whose client is generated from OpenAPI using HTTPX. The partner JSON contains an item identifier, label, available quantity and a completeness marker. The internal API accepts a named publication request and returns an acknowledgement. Service URLs, a credential and operation limits come from process configuration. Implement a small vertical slice and its tests, including a second execution and interruptions. Explain the selected boundaries and unavailable proof. Separately provide a small CSV-counting CLI.

The [replay request](python-backend/request.md) supplies the concrete neutral task and raw contract/fixtures used for the Python convergence exercise. It does not supply expected implementation files.

For a convergence evaluation, give two independent agents this same prompt, the same raw contract/fixtures and the same relevant skills, without a target architecture or the other agent's output. Compare concrete ownership, models, conversions, error/lifecycle behavior and readable tests; do not require identical file counts or wording. A separate adoption question supplies an existing uv/Ruff/pytest application with no mypy configuration and asks what changes the skill alone authorizes.

## React editing

An Expo feature edits a booking while a list refetches. A successful HTTP response sometimes contains an unexpected status; a failed submission must offer useful recovery. Generated client types are available. Show feature ownership, form/query behavior, component contracts and focused tests. A second screen needs one existing display transformation.

## Test readability

A module has several reservation scenarios with rich customer data, two nearly identical helpers and a factory that inserts database rows. Add tests for accepting a valid reservation, rejecting a missing customer, and preserving a uniqueness rule. Show locations, fixtures, preparation, action and assertions. Identify the needed infrastructure and reproduce a failed scenario using standard tool facilities.

## Docker packaging and configuration

A service needs an application image built from repository sources, local database infrastructure and per-application configuration. The repository also has a mobile development server. Propose the Dockerfile/Compose and configuration arrangement with verification. Separately, a documentation-only skill edit encounters a user-owned process on a configured port; state what verification is appropriate.

## Delivery and local authority

The user authorizes implementation of issue 42 and its draft PR. Checks pass and another issue becomes unblocked. State the next actions. Separately, a login change has an existing project-specific identity architecture: identify the inputs needed to implement it. A small documentation correction also needs delivery; describe its tracking.

## Prototype review

The user wants to compare two layouts for an approved screen before final Figma work, then keep the result available for another review next week. Describe where to create the exploration, how to present it, and what happens after the user approves a direction. Identify any Git actions you would take.
