# Project Memory

本目录保存工作流包的项目级记忆。Agent 启动时应先读取这里的规则，再执行 `.claude/CLAUDE.md` 中的工作流。

## Files

- `USER_PREFERENCES.md`：用户长期偏好、默认沟通方式和默认执行习惯。
- `AUTOMATION_RULES.md`：用户希望以后自动执行的重复流程。
- `SELF_UPGRADE_LOG.md`：能力缺口、skill/plugin 安装、拒绝记录和验证证据。

## Rules

- 只记录长期规则，不记录一次性任务。
- 不记录密钥、token、隐私敏感信息。
- 用户当前明确指令优先于历史记忆。
- 记忆变更后，应在最终回复中说明写入了什么。
