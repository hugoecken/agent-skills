# Evaluation request

A new Python application periodically reads a partner catalogue and publishes available items through the internal API described in [contract.yaml](contract.yaml). Provider input examples are in [provider-fixtures.json](provider-fixtures.json). The partner's complete flag states whether it supplied the authoritative full catalogue; an incomplete response must not publish a replacement. A complete empty catalogue may be published. The provider title becomes the internal label. Quantities are JSON integers, not numeric strings. Service URLs, a credential and operation limits come from process configuration.

Using the supplied skills, implement a small vertical slice and focused tests, including a second execution and interruption/failure. Explain structure, data ownership, conversion and verification actually obtained. Do not call real services. Use a real generated client if claiming generated transport proof; otherwise identify that missing proof. Separately implement a small CSV-counting CLI. An existing unrelated uv/Ruff/pytest application has no mypy configuration: explain what installing these skills alone authorizes there.

Keep the solution proportional to this task. Report concrete files, commands/results, boundaries not exercised and material ambiguity in the instructions. No target architecture, file count, class names or answer key is supplied.
