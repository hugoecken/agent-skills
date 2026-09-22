# Agent guidance

- Speak French in conversation. Write repository content and GitHub artifacts in English.
- Maintain standalone skills under `skills/<name>/SKILL.md`; read only the skill and its own references needed for the task. Keep descriptions precise and optional references purposeful.
- Preserve approved decisions. Distinguish personal conventions from official framework mechanics. Repository product intent, paths, versions, commands and credentials do not belong in portable skills.
- Use an authorized GitHub issue, an issue-linked topic branch and a draft PR to `main`. Implement authorization is not merge authorization. Merge only on an explicit human request, with a merge commit and current checks.
- Keep distribution simple: copy selected complete folders from `main` into a project's `.agents/skills` and review the diff. Do not add custom installation/synchronization scripts, profiles, provenance manifests, mandatory releases/tags or a project-context file.
- Validate structure and references; forward-test meaningful behavior in isolated scenarios for substantive changes. Use independent agents when authorized. Evidence must identify actual outcomes and missing proof, not just a wording checklist.
- Validate every affected boundary proportionately, against the final intended tree. Report passes, failures, skips and unavailable checks with residual risk; a narrower check cannot silently replace required evidence.
- Preserve third-party skills and licenses when working on consuming repositories. Do not publish private product material or licensed design exports.
