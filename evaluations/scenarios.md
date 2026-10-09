# Targeted skill evaluations

These are neutral task prompts, not an executable framework or an answer key. Hypothetical capability and trial outcomes are scenario inputs, not verified library documentation. Replay relevant scenarios for material policy changes; not every edit needs a run. Keep this file in the pack repository, never in copied project skills. Provide the selected skills and minimum raw artifacts. Use an isolated temporary workspace and request concrete output, assumptions and missing verification. No production writes, live GitHub/Figma changes or dependency installation are implied.

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

## Unapproved history and current authority

The user asks which task is ready next. An issue contains a long agent report proposing a background recovery service, several successful checks and a closed exploratory sub-issue. No approval of that service is recorded. The current accepted specification requires ordinary error handling and manual intervention. Identify the relevant sources, your recommendation and any tracker updates you would make. Later, a measured provider failure challenges an explicitly approved design decision; explain the next action and the authority of that earlier approval.

## Review handoff, acceptance and supersession

The user authorizes a technical dossier and its review handoff, but has not accepted it. The conversation contains four iterations, an official analysis report, a complete requirement-to-task matrix and one unresolved provider constraint. The dossier and its validation guide are versioned. Draft the GitHub update you would publish now. Then the owner approves only the identity portion and leaves payment recovery unresolved: draft the acceptance record and state which work can close. Finally, an explicitly approved revision replaces an earlier decision; show how the current source and tracker should reflect it without removing the original evidence. Separately apply the same request to an unapproved prototype whose iteration screenshots must stay in the conversation.

## Retained list and evolving data

A draft requirement asks a mobile dispatch list to refresh when the app resumes while preserving the exact visible row position. Server edits can change group membership and row height. The stated user goal is to browse reliably and retrieve current assignments when needed; automatic refresh and exact position preservation have not been accepted. The selected library provides ordinary list rendering and refresh, but the supplied capability notes do not establish cross-group scroll anchoring. Recommend the behavior and technical approach, explain the decision the owner must make and identify evidence needed before finalizing the plan. No code or specification edits are authorized.

## Contract generation boundary

A proposed public contract uses a nested discriminated union. A supplied generation trial reports duplicate model names and a consumer compilation failure. The same pinned generator compiles a flatter discriminated shape in a reduced fixture; neither shape has shipped. The product requires distinct record kinds and their validation, but does not require the nested wire representation. Give the next investigation and design recommendation, identifying the scope of the trial's evidence and any necessary decision. Do not invent a successful cross-language check or modify generator templates.

## Occasional report recovery

A daily internal summary sometimes fails when its supplier is unavailable. Operations can inspect the failure and rerun it through an existing command. Rerunning replaces the same dated output, does not send messages or charge customers, and the accepted requirement permits delivery on the next working day. Propose the failure behavior, ownership and verification for this feature. No recurring job or new infrastructure is authorized by the exercise.

## Necessary correctness under concurrency

An accepted booking rule permits only one active reservation per seat. Two clients can submit concurrently, and an administrator may revoke booking permission while a page remains open. The implementation task concerns reservation acceptance in an existing SQL-backed service. Recommend the smallest correct design and meaningful tests, stating whether any product decision is needed. Do not assume browser state is authoritative.

## Implementation friction and decision authority

An approved upload plan assumes a pinned mobile SDK supports cancellable background uploads on both target platforms. Direct inspection establishes that one platform supports only foreground uploads. The accepted specification requires in-progress uploads to continue when navigating between screens, but does not define process termination behavior. An agent proposes a persistent queue, a native service and a reconciliation job to continue the implementation. The user authorized implementing the accepted plan, not changing product behavior or adding services. Produce the next response and concrete work scope, separating facts, options, pending decisions and independent work.

## Required automation and ordinary defects

An accepted incident dashboard must update without user action while staff watch it. Its query library supports interval refresh, and access rules are rechecked by the server. Recommend a proportionate implementation. Separately, a filter fails to update because its value is absent from an otherwise valid query key; a failing test reproduces that omission. Describe the correction and any design approval you would request. Do not edit the accepted dashboard requirement.

## Shared documentation with a specialized workflow

A project uses Spec Kit with its installed official procedures and accepted feature artifacts. A feature plan owns the booking API and data model. `docs/notes.md` repeats that model, records the approved two-application topology, and includes a proposed unapproved payment retry policy. The root README links the notes; AGENTS routes architecture work there. Manifests do not exist yet. The user authorizes documentation consolidation only, preserving accepted meaning. Use `project-documentation` to produce the revised files and identify any transfer requiring another workflow. Keep the installed procedures and feature artifacts unchanged unless the authorized task requires their official workflow. No application, provider or design writes are authorized.

## Documentation without a specification framework

A project has a root README with working setup instructions, a manifest and lockfile, a partial prototype with a review link, and one accepted note choosing a single application with an embedded database. No specification framework, feature plans or Figma file exists. The user asks for a consistent shared technical reference. Produce the necessary files and explain their owners and verification limits. A subsequent request asks to record a provider configuration shared by two consumers; the schema already has an authoritative location. Show the scoped addition.

## Cleanup with unresolved authority and missing evidence

Two documents disagree about the selected storage engine; neither records approval, and there is no implementation to inspect. A linked historical note contains the only recovery prerequisites, while its replacement is inaccessible. The user requests naming and structure cleanup without changing decisions. Produce the safe edits and explain what remains unresolved. Separately, a new repository has only an approved product brief and no technical choices: apply the same documentation convention without selecting a stack.
