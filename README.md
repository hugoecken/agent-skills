# Personal agent skills

Nineteen reusable skills for design, development and delivery. Each folder is independent and contains its own instructions and any conditional references. Copy only the skills a project needs; they do not depend on a central router or another skill's files.

Each skill describes its own tools and personal engineering conventions. Read its `SKILL.md` to decide whether those choices fit your project. Adopting a different tool or architecture is a separate project decision.

## Catalogue

| Skill                                                                    | Purpose                                                                                    |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| [java-spring](skills/java-spring/SKILL.md)                               | Application boundaries, JPA, transactions and Java mapping                                 |
| [java-testing](skills/java-testing/SKILL.md)                             | Focused JVM, Spring, database and runtime evidence                                         |
| [react-feature-architecture](skills/react-feature-architecture/SKILL.md) | Shared React feature, state, form and effect ownership                                     |
| [openapi-codegen](skills/openapi-codegen/SKILL.md)                       | Source-first REST contracts, generators and response validation                            |
| [liquibase-schema](skills/liquibase-schema/SKILL.md)                     | Baseline posture, durable invariants and schema evolution                                  |
| [github-delivery](skills/github-delivery/SKILL.md)                       | Draft PRs on issue-linked `feature/`, `bugfix/` or `tech/` branches and human-requested merge commits |
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

1. Get the current reviewed `main` branch of this repository.
2. Copy each selected **complete** `skills/<name>` folder into the project's `.agents/skills/<name>`. For an update, replace that selected folder completely so obsolete references do not remain; review any local differences before replacing it.
3. Review and commit the diff in the project. Keep project-specific decisions, paths, commands and design links in the project's usual documentation or configuration.

For example, from a clone on `main`, a first copy can be as simple as:

```sh
cp -R skills/java-spring /path/to/project/.agents/skills/
```

The destination skills directory must already exist. The example is a first copy, not a command to merge an update into an existing folder. There is no installer, required release/tag, profile or synchronization service. Projects update when the owner chooses, and Git records the installed contents. Improve a personal skill here, merge its review, then copy that folder to the projects that use it. Do not silently overwrite local work.

Limit updates to the folders you selected. Preserve other installed skills and their licenses.

## Evaluation

[Maintenance scenarios](evaluations/scenarios.md) stay outside project installations. Choose a case that exercises the decision being changed. The Python replay includes its raw inputs. Design questions can be answered from the prompt, and exercises that need a source tree or snapshots list what to supply. Follow the evaluation notes to test skill selection or compare outputs on the same task.

No license is selected for the personal content at this time. Existing third-party license terms remain applicable to their own material.
