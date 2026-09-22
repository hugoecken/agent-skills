# Personal agent skills

Seventeen reusable skills for development and delivery. Each folder is independent and contains its own instructions and any conditional references. Copy only the skills a project needs; they do not depend on a central router or another skill's files.

These are personal engineering conventions informed by framework documentation, not a claim that every community prescribes one architecture. The target choices include feature-first Java/React, explicit Java persistence ports, React Hook Form/Zod, semantic StyleSheet styling for native, and generated contracts. An existing application is not migrated merely because the skills are installed. Python guidance makes hybrid use cases, immutable internal values, explicit mapping, declared input conversions, strict typing and Pydantic Settings concrete; web frameworks and ORMs remain project decisions. Its linked examples connect composition, adapters and tests without becoming a mandatory scaffold.

## Catalogue

| Skill                                                                    | Purpose                                                              |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------- |
| [java-spring](skills/java-spring/SKILL.md)                               | Application boundaries, JPA, transactions and Java mapping           |
| [java-testing](skills/java-testing/SKILL.md)                             | Focused JVM, Spring, database and runtime evidence                   |
| [react-feature-architecture](skills/react-feature-architecture/SKILL.md) | Shared React feature, state, form and effect ownership               |
| [openapi-codegen](skills/openapi-codegen/SKILL.md)                       | Source-first REST contracts, generators and response validation      |
| [liquibase-schema](skills/liquibase-schema/SKILL.md)                     | Baseline posture, durable invariants and schema evolution            |
| [github-delivery](skills/github-delivery/SKILL.md)                       | Issues, topic branches, draft PRs and human-directed merges          |
| [figma-design-governance](skills/figma-design-governance/SKILL.md)       | Library/screen ownership and approved design evidence                |
| [code-documentation](skills/code-documentation/SKILL.md)                 | Useful Java, Python and TypeScript contracts                         |
| [application-logging](skills/application-logging/SKILL.md)               | Safe operational logs and failure ownership                          |
| [docker-conventions](skills/docker-conventions/SKILL.md)                 | Source-built images, local infrastructure and explicit configuration |
| [nextjs-separate-backend](skills/nextjs-separate-backend/SKILL.md)       | Next.js rendering with a separate business backend                   |
| [nextjs-testing](skills/nextjs-testing/SKILL.md)                         | Vitest/RTL and real Next/Playwright boundaries                       |
| [design-prototyping](skills/design-prototyping/SKILL.md)                 | Disposable responsive exploration before Figma                       |
| [react-native-expo](skills/react-native-expo/SKILL.md)                   | Expo routing, native UI and lifecycle                                |
| [expo-testing](skills/expo-testing/SKILL.md)                             | jest-expo/RNTL and native evidence                                   |
| [python-development](skills/python-development/SKILL.md)                 | Python architecture, explicit mapping, settings and owned lifecycle  |
| [python-testing](skills/python-testing/SKILL.md)                         | pytest, controlled I/O and ingestion characterization                |

## Install or update

1. Get the current reviewed `main` branch of this repository.
2. Copy each selected **complete** `skills/<name>` folder into the project's `.agents/skills/<name>`. For an update, replace that selected folder completely so obsolete references do not remain; review any local differences before replacing it.
3. Review and commit the ordinary Git diff in the project. Keep project-specific authority, paths, commands and design-file links in its existing `AGENTS.md`, manifests and documentation. No rewriting of the portable skill is needed.

For example, from a clone on `main`, a first copy can be as simple as:

```sh
cp -R skills/java-spring /path/to/project/.agents/skills/
```

The destination skills directory must already exist. The example is a first copy, not a command to merge an update into an existing folder. There is no installer, required release/tag, profile or synchronization service. Projects update when the owner chooses, and Git records the installed contents. Improve a personal skill here, merge its review, then copy that folder to the projects that use it. Do not silently overwrite local work.

A project can select the common backend/contracts/delivery skills plus the framework/test skills it actually uses. Do not install web skills in a native-only project or ingestion policies into a simple utility merely to complete a bundle. `liquibase-schema` does not convert an existing Flyway project.

## Repository guidance and official skills

Authentication belongs to each project’s accepted architecture: route identity, session and authorization work there from `AGENTS.md`, rather than installing a portable authentication profile.

Keep `AGENTS.md` short: product/spec authority, repository map and commands, validation/reporting requirements, and task-based skill selection. Apply `code-documentation` to changed handwritten contracts, plus the relevant language/test skills. An installed skill is neither authorization for unrelated changes nor proof that existing code complies.

Keep official Spec Kit, Karpathy and shadcn skills intact when already present. Official Nx skills come directly from [nrwl/nx-ai-agents-config](https://github.com/nrwl/nx-ai-agents-config), with their complete folders and license, not from this personal pack. Verify the current [Nx catalogue](https://nx.dev/.well-known/agent-skills/index.json) at update time. The verified catalogue currently includes `link-workspace-packages`, `monitor-ci`, `nx-generate`, `nx-import`, `nx-plugins`, `nx-run-tasks` and `nx-workspace`.

Copying Nx skills does not enable Nx Cloud, install MCP, change framework versions or rewrite workspace instructions. Some capabilities need unavailable services/tools; report that boundary rather than provisioning them implicitly. Retain the upstream license alongside the copied skills without changing their content.

## Evaluation

[Scenario prompts](evaluations/scenarios.md) cover representative architecture, runtime and delivery decisions. Give an evaluator the request, selected skills and minimum raw context without the expected answer. Use a disposable directory, inspect actual outputs, and record missing build/runtime proof. Replay targeted scenarios when a material policy change warrants it, not on every documentation edit. Keep them here, outside project installations. These exercises complement structural validation; they do not certify an application's behavior or replace its tests. Measuring improvement requires comparable with/without-skill runs using the same task and raw context; a successful exercise alone makes no efficacy claim.

No license is selected for the personal content at this time. Existing third-party license terms remain applicable to their own material.
