# Targeted skill evaluations

Choose an exercise for the decision that changed. The Python replay includes its files. For a case marked **Inputs to supply**, prepare the listed material before running it. Other prompts contain the facts needed for a design or planning response. Treat stated library capabilities and trial outcomes as hypothetical inputs. Use a disposable workspace. The exercises grant no permission for production calls, live GitHub or Figma writes, or dependency installation.

To exercise a skill's instructions, give the evaluator the task, the selected skill and the raw inputs. To test discovery, provide the available skill descriptions and the user request without naming the intended skill. To measure an effect, run the task with and without the skill, or against its previous version to assess a change. Use fresh contexts with the same raw inputs, model and tools. Inspect the proposed decisions or actual changes and record which checks ran.

## Java reservation

A Spring/JPA service adds reservation create/read. Its generated API request contains a customer UUID, response contains a database-generated ID and status, and the contract defines PENDING/CONFIRMED. The repository uses MapStruct and Lombok. Sketch the handwritten structure, write/read flow and focused test cases. Identify evidence that requires the actual database or build.

## Existing Java preferences module

**Inputs to supply.** Provide the existing GET controller and response type, account lookup, service, preference entity, repository and current tests. Include the PUT contract if it is declared separately.

An existing Spring/JPA module has a controller, a service and a Spring Data repository. Implement PUT for the authenticated account's theme and compact mode, returning the complete updated preferences in its existing response shape. Preserve GET. Another account returns 403 before its data is read or written; an absent preference record returns 404. Save both fields together. Use the supplied source and tests to produce inspectable edits and useful regression checks. No build configuration, database, new consumer or project scaffolding is supplied.

## Python backend feature and utility

Copy [the request](python-backend/request.md), [the API contract](python-backend/contract.yaml) and [the provider responses](python-backend/provider-fixtures.json) together. For convergence, give two independent agents the same skills and inputs. Compare their handling of complete, empty, partial and invalid data, repeated execution and interruption, using the observed publications and test results.

For a separate adoption decision, ask what project changes are authorized when an owner requests only that the Python development and testing skills be copied into an existing uv/Ruff/pytest application with no mypy configuration.

## React editing

An Expo feature edits a booking while a list refetches. A successful HTTP response sometimes contains an unexpected status; a failed submission must offer useful recovery. Generated client types are available. Show feature ownership, form/query behavior, component contracts and focused tests. A second screen needs one existing display transformation.

## Test readability

**Inputs to supply.** Provide the reservation module, current tests, helpers and database factory, the uniqueness constraint, and the project's test command and disposable database setup for reproducing the failure.

A module has several reservation scenarios with rich customer data, two nearly identical helpers and a factory that inserts database rows. Add tests for accepting a valid reservation, rejecting a missing customer, and preserving a uniqueness rule. Show locations, fixtures, preparation, action and assertions. Identify the needed infrastructure and reproduce a failed scenario using standard tool facilities.

## Docker packaging and configuration

A service needs an application image built from repository sources, local database infrastructure and per-application configuration. The repository also has a mobile development server. Propose the Dockerfile/Compose and configuration arrangement with verification. Separately, a documentation-only skill edit encounters a user-owned process on a configured port; state what verification is appropriate.

## Delivery and local authority

The user authorizes implementation of issue 42 and its draft PR. Checks pass and another issue becomes unblocked. State the next actions. Separately, a login change has an existing project-specific identity architecture: identify the inputs needed to implement it. A small documentation correction also needs delivery; describe its tracking.

## Draft PR discovery

Present each request independently against the same skill catalogue. Record which skills the agent selects.

- Please deliver the completed change for issue 42 as a draft PR. Our project uses `work/42-title` branches and squash merging.
- Please deliver the completed change for issue 42 as a draft PR. Our project uses `bugfix/42-title` branches and merge commits.

## Prototype review

The user wants to compare two layouts for an approved screen before final Figma work, then keep the result available for another review next week. Describe where to create the exploration, how to present it, and what happens after the user approves a direction. Identify any Git actions you would take.

## New application design

The project constitution requires approval of the complete prototype and its visual direction, followed by UI Library and Product Design approval before technical planning. Three contributors have a rough navigable prototype for a shared workspace selector, home screen, reservation journey and account settings. Plan the work and issue boundaries, including ownership and integration criteria. A contributor then reports that the reservation screen is approved while settings lacks error states. Later, every screen has been reviewed individually, but returning from settings loses the selected workspace and the art direction is still open. Describe what can proceed at each point and the handoff once the integrated prototype is approved.

## Evolution and shared changes

An existing application has an approved prototype, UI Library and Product Design. Its constitution requires approval of changes to those designs before technical planning. Accepted specifications add a rescheduling flow with a changed reservation detail entry point and a new recovery state. Two contributors own disjoint screen directories, but both need to change the shared router. Propose the task decomposition, coordination and review scope. During exploration, a contributor proposes silently removing a required confirmation step to make the flow shorter. Describe how to handle the proposal and the eventual Figma handoff.

## Exploration evidence and issue organization

The user asks to reorganize prototype issues so several people can work independently. A tracking issue currently contains iteration screenshots, a completed checkbox for an unreviewed screen and a link to an application documentation PR that only records the process. The user says no prototype version has been approved and asks for a cleanup proposal only. Produce the proposed issue content and explain which repository or GitHub changes, if any, you would execute now.

## Unapproved history and current authority

The user asks which task is ready next. An issue contains a long agent report proposing a background recovery service, several successful checks and a closed exploratory sub-issue. No approval of that service is recorded. The current accepted specification requires ordinary error handling and manual intervention. Identify the relevant sources, your recommendation and any tracker updates you would make. Later, a measured provider failure challenges an explicitly approved design decision; explain the next action and the authority of that earlier approval.

## Review handoff, acceptance and supersession

**Inputs to supply.** Provide the reviewable dossier and guide links and revisions, the unresolved provider constraint, the owner's partial approval and its scope, and the earlier decision and approved replacement revision.

The user authorizes a technical dossier and its review handoff, but has not accepted it. The conversation contains four iterations, an official analysis report, a complete requirement-to-task matrix and one unresolved provider constraint. The dossier and its validation guide are versioned. Draft the GitHub update you would publish now. Then the owner approves only the identity portion and leaves payment recovery unresolved: draft the acceptance record and state which work can close. Finally, an explicitly approved revision replaces an earlier decision; show how the current source and tracker should reflect it without removing the original evidence. Separately apply the same request to an unapproved prototype whose iteration screenshots must stay in the conversation.

## Retained list and evolving data

A draft requirement asks a mobile dispatch list to refresh when the app resumes while preserving the exact visible row position. Server edits can change group membership and row height. The stated user goal is to browse reliably and retrieve current assignments when needed; automatic refresh and exact position preservation have not been accepted. The selected library provides ordinary list rendering and refresh, but the supplied capability notes do not establish cross-group scroll anchoring. Recommend the behavior and technical approach, explain the decision the owner must make and identify evidence needed before finalizing the plan. No code or specification edits are authorized.

## Contract generation boundary

A proposed public contract uses a nested discriminated union. A supplied generation trial reports duplicate model names and a consumer compilation failure. The same pinned generator compiles a flatter discriminated shape in a reduced fixture; neither shape has shipped. The product requires distinct record kinds and their validation, but does not require the nested wire representation. Give the next investigation and design recommendation, identifying the scope of the trial's evidence and any necessary decision. Do not invent a successful cross-language check or modify generator templates.

## Owned API and decoder guarantees

A service and its internal worker generate their server models and client from one contract. A controlled HTTP check shows that the native client accepts an omitted required boolean as null, which the application mapper converts to false; it rejects malformed JSON and invalid enum strings. An additional schema interceptor rejects the missing boolean. The owner explicitly accepts possible incomplete work or late failure if the service violates its contract and asks to remove preventive runtime validation. Propose the change, tests and documentation without changing the schema. Explain what the observed probe does and does not establish.

## Explicitly required response enforcement

A partner response supplies a payment recipient and amount. The accepted boundary requires missing or invalid required fields to fail before any payment is authorized. The selected generated decoder does not enforce required-field presence, and the provided capability notes establish no supported strict mode. The owner asks for simpler code but has not waived that requirement. Give the next investigation and decision without implementing a payment or assuming that using a mature library establishes the required validation.

## Native lifecycle and duplicate state

An application uses an installed query library whose supplied documentation supports request cancellation, invalidation and account-scoped keys. The application's adapter maintains a second response cache and a separate cancellation flag to guard against hypothetical late SDK results; no reproduced defect or stronger accepted guarantee is supplied. Describe how to evaluate the duplication, what can be simplified within the existing requirements and the integration evidence needed. Preserve the requirement that one account must never receive another account's data.

## Occasional report recovery

A daily internal summary sometimes fails when its supplier is unavailable. Operations can inspect the failure and rerun it through an existing command. Rerunning replaces the same dated output, does not send messages or charge customers, and the accepted requirement permits delivery on the next working day. Propose the failure behavior, ownership and verification for this feature. No recurring job or new infrastructure is authorized by the exercise.

## Necessary correctness under concurrency

An accepted booking rule permits only one active reservation per seat. Two clients can submit concurrently, and an administrator may revoke booking permission while a page remains open. The implementation task concerns reservation acceptance in an existing SQL-backed service. Recommend the smallest correct design and meaningful tests, stating whether any product decision is needed. Do not assume browser state is authoritative.

## Implementation friction and decision authority

An approved upload plan assumes a pinned mobile SDK supports cancellable background uploads on both target platforms. Direct inspection establishes that one platform supports only foreground uploads. The accepted specification requires in-progress uploads to continue when navigating between screens, but does not define process termination behavior. An agent proposes a persistent queue, a native service and a reconciliation job to continue the implementation. The user authorized implementing the accepted plan, not changing product behavior or adding services. Produce the next response and concrete work scope, separating facts, options, pending decisions and independent work.

## Required automation and ordinary defects

An accepted incident dashboard must update without user action while staff watch it. Its query library supports interval refresh, and access rules are rechecked by the server. Recommend a proportionate implementation. Separately, a filter fails to update because its value is absent from an otherwise valid query key; a failing test reproduces that omission. Describe the correction and any design approval you would request. Do not edit the accepted dashboard requirement.

## Requests, response evolution and frontend models

A new unshipped API accepts a title and returns id, title and a finite publication status. Java, Python and TypeScript consume it. Propose its object schemas and boundary cases for an extra create-request field, an old client receiving an added response property, nested additions, a missing required field, forbidden null and an unknown enum value. An unrelated shipped response is explicitly closed; no migration is authorized. Produce frontend examples for a read-only card using the response and a form editing dates and numeric values as intermediate strings. Explain object/enum ownership in the three languages and identify missing generation evidence.

## Server reads and client interactions

**Inputs to supply.** Provide the app's Next.js, React and client-cache dependency versions and the relevant version-specific fetching, caching and hydration documentation.

An App Router application has a separate backend/BFF and a validated generated client. A public article needs content and metadata without client interaction. A private workspace has filtering, mutations and refreshed availability, with initial data available on the server. Propose rendering, ownership, caching and verification, including two successive accounts and simultaneous server requests. Resolve API details from the installed versions; no runtime is supplied by the exercise.

## Documentation within a touched file

**Inputs to supply.** Provide the handwritten file with the failing function and its undocumented neighbors, the neighboring file, and the test file covering the existing capacity behavior.

A reservation accepts a whole number of guests from 1 through the stated capacity, inclusive. The supplied function rejects 4 at capacity 4. Fix it and add the regression check, preserving the other behaviors. Its handwritten file also contains an undocumented public type, another exported function and a private normalization step; a neighboring file is undocumented too. Produce inspectable edits and report evidence. No build configuration or project scaffolding is supplied.

## Representative Figma compositions

**Inputs to supply.** Provide the approved prototype states, an inventory of the target file's pages and journey sections, and the applicable library components, variants and token names. A sanitized text snapshot is sufficient for a plan.

Prepare the translation plan for an approved match list into an existing Product Design file. Supplied inputs describe the prototype, existing journey section, library components and tokens. States include first loading, success, empty, three read failures sharing one composition but different copy, and an action waiting inside the error card. The card already has an action-busy property. Identify the pages, sections, frames, instances and notes to create or update and what these inputs can establish for later native implementation. Produce an inspectable plan; no live Figma change or application code is authorized.

As a separate scope question, the owner asks only to rename a private Figma documentation helper. Which work does that request authorize?

## Shared API across feature plans

Two accepted features concern a catalogue: public browsing and partner ingestion. No production stack or generator has been selected. Both proposed plans define a different Product schema and their own Problem response; the ingestion draft sends 400 individual writes on each import. Propose the decisions needed before these plans can be accepted, the authoritative artifacts and a representative qualification exercise. Decide what may be shared and what needs distinct semantics. No installation, implementation or new product behavior is authorized.

## Evolving a shared contract with an accepted pipeline

A new moderation plan proposes another endpoint for an operation already owned by the catalogue API. The accepted API is consumed by two independently released applications, and its existing generator supports the current schema. A draft proposes a nested union, a new generator and a persistent retry dashboard for one rare conflict. The product only requires reporting success or refusal and permits manual correction. Produce a bounded planning recommendation with affected owners, compatibility checks and unresolved decisions. Do not change files or assume the proposal is approved.

## Technical decisions inside the native planning step

An accepted feature specification describes a catalogue with a public browser and partner imports. The project has the official Spec Kit skills and a constitution requiring KISS and contract-first generation. No application stack, technical plan or architecture document exists. The owner asks to frame the next planning issue and identify how shared API decisions will be reached, but does not authorize executing the plan or creating an application. Use the GitHub delivery and OpenAPI skills. Produce the issue request and explain the source ownership and evidence needed for its later review. No remote writes are permitted by this exercise.

## Design tools without a project gate

A project uses the native Spec Kit workflow and has no prototype/Figma approval requirement. An approved specification is ready for technical planning. The installed personal pack includes prototyping, Figma and native UI skills. Identify the next applicable procedure and whether the mere presence of these skills adds a prerequisite. Separately, the owner requests a standalone prototype review without authorizing production implementation; describe the bounded result.

## Parallel issue batch selection

**Inputs to supply.** Provide a tracker snapshot with issue IDs, scope, assignees and blocker links, the shared contract each task touches, and any associated task conversations or draft PRs.

The owner asks for up to six independent tasks and a proposal before launching. The supplied tracker contains a ready task, a task with an open blocker, an epic, an already assigned task and two otherwise ready tasks sharing one generated contract. Existing task conversations and draft PRs are included. Use the parallel delivery skill to produce the proposal and identify what actions are currently authorized. Work only with the supplied isolated artifacts; no live GitHub writes or conversation launches are authorized by this exercise.

## Parallel batch resume and guidance update

**Inputs to supply.** Provide the merged skill revision and change, each task's issue and conversation history, branch/base revisions, local edit status, draft PR and completed review records, and the acknowledgment state.

The owner approves updating the existing task conversations after a reviewed skill update is merged. Supplied snapshots include one active task with local edits, one completed draft PR with finished independent reviews, one stale branch, and one unresolved message acknowledgment. The new skill codifies the same review protocol. Reconstruct the batch state and write the precise handoff messages and verification plan using those snapshots. Do not perform live external writes or infer that the update authorizes merging the implementation PRs.

## Shared native runtime resources

**Inputs to supply.** Provide each task's native check and worktree, device IDs and occupancy, development-server ports and owners, available ports and the host's UI-control capabilities.

Three approved tasks need native mobile checks on one Mac. Supplied resource snapshots include an occupied simulator/development-server port, one available simulator, available ports and a shared foreground mouse/keyboard tool. Produce task allocations and handoff instructions, then describe what can run concurrently and what must wait. Device-targeted terminal tools are available, but no live simulator action is authorized by this isolated exercise.
