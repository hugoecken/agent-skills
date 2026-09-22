---
name: container-runtime
description: "Build or verify application-owned Docker and local runtime boundaries, controlled dependencies, startup and representative flows while preserving user processes and data."
---

# Containers and local runtime

Read actual Dockerfiles, Compose files, environment examples, build targets and accepted runtime scope. Use the existing separation between applications and infrastructure. This skill does not authorize deployment, production writes, destructive resets or a migration of the task runner/container stack.

## Build and configuration

Keep Dockerfiles application-owned. Use a builder/runtime split where it removes build tools from the shipped image. Preserve workdir, command, health behavior, permissions and required files unless the accepted change owns them. Do not install Nx in runtime images or wrap simple Dockerfiles in a generic execution framework.

Use configured stable ports and project names. Report a port conflict rather than moving ports or introducing dynamic indirection. Keep safe `.env.example` files; credentials, keys, private provider state, runtime logs and local overrides stay out of Git. Do not dump resolved secrets into evidence.

For dependency/runtime changes, validate the actual version/configuration in the repository, reproducible build inputs and the changed startup contract. Do not assume a process from another checkout represents this source tree.

## Runtime proof

Start only required dependencies for the changed boundary. Documentation/skill-only edits do not need unrelated application stacks. Use controlled data and providers; identify external write paths before running scheduled jobs or ingestion. No production scrape/write as a side effect of a smoke test.

Confirm effective safe configuration, startup, health and required ports, then exercise one representative owned flow including meaningful failure behavior when changed. A running container alone is not proof of a working feature. Include the real database/provider/native boundary when the claim depends on it, rather than substituting mocks silently.

Reuse healthy infrastructure where appropriate. Track the processes/containers started for this task and preserve user-owned services. Keep healthy sessions during iterative visual work; stop or retain task-owned processes according to the requested lifecycle. Do not remove volumes or reset data without explicit authorization for that disposable environment.

## Evidence and cleanup

Evidence accumulates across changed boundaries: a Java check does not replace a Python client, native or contract proof. Identify the actual tested source tree and invalidate affected evidence after relevant edits. Report successful, failed, skipped and unavailable checks separately, with reason, residual risk, replacement proof and any required waiver. Tool absence is not a passed check.

Inspect the full diff, formatting and ignored generated/runtime files before delivery. Report incomplete dependencies and any retained task-owned sessions. Cleanup is scoped to known task-owned resources and never a broad process kill or volume prune.
