# Automation Rules

记录以后无需用户重复提醒也要执行的流程。

## Active Rules

### 1. Active Work Disclosure

Every user-facing response must identify the active Agent, what it is doing, and what skill/plugin capability is being used. When starting a new phase, switching Agent role, invoking important skills/plugins, implementing code, verifying behavior, deploying, or upgrading workflow capabilities, show a full status block:

```text
当前 Agent：<agent-name>
当前工作：<what is being done>
使用能力：<skills/plugins and why>
当前产物：<file or artifact path, if any>
```

For very short replies, a one-line disclosure is acceptable, but it must still include Agent, work, and capability.

### 2. Missing Capability Handling

When the current workflow lacks a required capability:

1. Check installed `.claude/skills` and `.claude/plugins`.
2. Search curated skill market using `skill-installer` when available.
3. If the user provides a GitHub skill/plugin, inspect it before installation.
4. Install only after required authorization.
5. Update routing docs, agent assignments, manifest, README, and memory.
6. Verify frontmatter/manifests/scripts/references.
7. Report what changed and how it will be used next time.

### 3. Preference Capture

When the user says a rule should apply in the future, write it to `USER_PREFERENCES.md` and/or `AUTOMATION_RULES.md` in the same task unless it contains secrets or one-time instructions.
