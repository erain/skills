# Skills

Reusable workflows for coding agents.

- [reviewable-pr](skills/reviewable-pr/SKILL.md): implement a change, prove its
  behavior with automated and end-to-end tests, and prepare a clear upstream
  draft PR. Terminal demos are optional and require confirmation for each change.

Use it in an agent that supports skills, or ask another coding agent to read
and follow the file directly:

```text
Use $reviewable-pr to fix this issue and prepare an upstream draft PR.
```

```text
Read skills/reviewable-pr/SKILL.md and follow its workflow for this change.
```

For Codex, copy the `skills/reviewable-pr` folder into your skills directory
(usually `~/.codex/skills/`). The workflow itself does not require Codex-specific
tools; use your agent's available shell, repository, and user-input tools.
