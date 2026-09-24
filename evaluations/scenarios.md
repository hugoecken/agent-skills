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

## New application design

Accepted V1 specifications cover a shared workspace selector, a home screen, a reservation journey and account settings. No product code exists. A rough prototype has links between placeholder screens, but the return paths and workspace context are unresolved. Three contributors are available. Plan the next work and concrete issue boundaries, including file ownership and integration criteria. State what can happen concurrently and what evidence is needed to move to the next stage. One contributor later reports that their reservation screen is approved while settings still has no error states; describe the available next actions.

## Evolution and shared changes

An existing application has an approved prototype, UI Library and Product Design. Accepted specifications add a rescheduling flow with a changed reservation detail entry point and a new recovery state. Two contributors own disjoint screen directories, but both need to change the shared router. Propose the task decomposition, coordination and review scope. During exploration, a contributor proposes silently removing a required confirmation step to make the flow shorter. Describe how to handle the proposal and the eventual Figma handoff.

## Exploration evidence and issue organization

The user asks to reorganize prototype issues so several people can work independently. A tracking issue currently contains iteration screenshots, a completed checkbox for an unreviewed screen and a link to an application documentation PR that only records the process. The user says no prototype version has been approved and asks for a cleanup proposal only. Produce the proposed issue content and explain which repository or GitHub changes, if any, you would execute now.

## Complete prototype handoff and limited Figma maintenance

All visual and interactive requirements of an agreed V1 have been reviewed in isolation. On integration, the back action from account settings loses the selected workspace. There is no final art direction yet. Describe the next work before production planning, then the order of operations once the complete integrated prototype is approved. Separately, a user asks only to rename a private Figma documentation helper without changing product design; state the appropriate scope.
