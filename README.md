# Agent Workflow Pack

这是一个用于 Agent 软件的项目团队工作流包。当 Codex、Claude Code、Claude Codex 或类似 Agent 在本目录下启动时，应优先读取 `.claude/CLAUDE.md`，再按 `.claude/agents` 中的岗位角色和 `.claude/skills` 中的技能流程开展工作。

## Structure

- `.claude/CLAUDE.md`：顶层项目经理规则，负责工作流、角色分配、阶段推进和完成标准。
- `.claude/MANIFEST.md`：记录 agents、skills、plugins 的来源、移除原因和维护规则。
- `.claude/agents/`：真实软件项目团队岗位，按相近职责合并为产品文档、方案架构、全栈实现、质量安全、交付运维。
- `.claude/skills/`：项目开发技能，包括 OpenAI curated skills，以及第三方 `ui-ux-pro-max` UI/UX 设计智能 skill。
- `.claude/plugins/`：从 OpenAI curated 插件缓存复制的项目级插件，包括 Superpowers 和 GitHub。

## Intended Use

把本目录作为项目工作流模板使用。Agent 进入项目后，应先完成项目阅读，再根据任务类型选择合适岗位和技能，最后用可验证结果交付。
