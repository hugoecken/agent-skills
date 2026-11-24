# Personal agent skills

Choose any skill that fits your project. Each folder under `skills/` contains its own instructions and any references it uses. Framework-specific conventions apply only when you choose the corresponding skill.

These skills capture personal engineering conventions. The consuming project owns its stack, requirements, approval process and existing implementation decisions.

## Catalogue

| Skill                                                                    | Purpose                                                                                    |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| [java-spring](skills/java-spring/SKILL.md)                               | Application boundaries, JPA, transactions and Java mapping                                 |
| [java-testing](skills/java-testing/SKILL.md)                             | Focused JVM, Spring, database and runtime evidence                                         |
| [react-feature-architecture](skills/react-feature-architecture/SKILL.md) | Shared React feature, state, form and effect ownership                                     |
| [openapi-codegen](skills/openapi-codegen/SKILL.md)                       | Source-first REST contracts, generators and response validation                            |
| [liquibase-schema](skills/liquibase-schema/SKILL.md)                     | Baseline posture, durable invariants and schema evolution                                  |
| [github-delivery](skills/github-delivery/SKILL.md)                       | Issues, topic branches, draft PRs and human-directed merges                                |
| [parallel-issue-delivery](skills/parallel-issue-delivery/SKILL.md) | Explicit parallel issue batches, isolated worktrees and fresh-context reviews |
| [figma-design-governance](skills/figma-design-governance/SKILL.md)       | Library/screen ownership and approved design evidence                                      |
| [code-documentation](skills/code-documentation/SKILL.md)                 | Useful Java, Python and TypeScript contracts                                               |
| [application-logging](skills/application-logging/SKILL.md)               | Safe operational logs and failure ownership                                                |
| [docker-conventions](skills/docker-conventions/SKILL.md)                 | Source-built images, local infrastructure and explicit configuration                       |
| [nextjs-separate-backend](skills/nextjs-separate-backend/SKILL.md)       | Next.js rendering with a separate business backend                                         |
| [nextjs-testing](skills/nextjs-testing/SKILL.md)                         | Vitest/RTL and real Next/Playwright boundaries                                             |
| [proportionate-design](skills/proportionate-design/SKILL.md)             | Minimal behavior, strict scope, removal-first changes and direct public library mechanisms               |
| [design-prototyping](skills/design-prototyping/SKILL.md)                 | Standalone interactive sketches and incremental review                                     |
| [react-native-expo](skills/react-native-expo/SKILL.md)                   | Expo routing, native UI and lifecycle                                                      |
| [expo-testing](skills/expo-testing/SKILL.md)                             | jest-expo/RNTL and native evidence                                                         |
| [python-development](skills/python-development/SKILL.md)                 | Python architecture, explicit mapping, settings and owned lifecycle                        |
| [python-testing](skills/python-testing/SKILL.md)                         | pytest, controlled I/O and ingestion characterization                                      |

## Install or update

1. Download or check out the reviewed `main` branch of this repository.
2. Copy the **complete folder** of each selected skill into the consuming project's `.agents/skills/` directory. If your agent uses a different skills directory, use its documented location.
3. When updating, review any local edits, then replace the selected folder in full so retired files are removed. Review and commit the diff in the consuming project.

For example, to install a single skill for the first time from the repository root, with the destination directory already present, run

```sh
cp -R skills/code-documentation /path/to/project/.agents/skills/
```

Keep project-specific commands, paths and sources of authority in the consuming project's existing instructions. When updating a selected skill, preserve any third-party skills alongside it and their licenses.

## Evaluation

[Scenario prompts](evaluations/scenarios.md) cover representative architecture, runtime and delivery decisions. Give an evaluator the request, selected skills and minimum raw context without the expected answer. Use a disposable directory, inspect actual outputs, and record missing build/runtime proof. Replay targeted scenarios when a material policy change warrants it, not on every documentation edit. Keep them here, outside project installations. These exercises complement structural validation; they do not certify an application's behavior or replace its tests. Measuring improvement requires comparable with/without-skill runs using the same task and raw context; a successful exercise alone makes no efficacy claim.

No license is selected for the personal content at this time. Existing third-party license terms remain applicable to their own material.
