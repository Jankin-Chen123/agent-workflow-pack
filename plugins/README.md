# Plugins

本目录保存从本机 OpenAI curated 插件缓存中复制出的项目级插件。它们用于让进入本项目的 Agent 复用成熟插件能力，而不是在项目内重新发明工作流。

Codex 插件发现元数据位于 `.agents/plugins/marketplace.json`，其中的 `source.path` 指向本目录下的插件文件夹。

## Installed Plugins

- `superpowers`：软件开发方法论插件，覆盖头脑风暴、计划、TDD、系统化调试、并行 Agent、代码评审和完成分支等流程。
- `github`：GitHub 协作插件，覆盖 PR 评论处理、Issue/PR triage、CI 检查和 GitHub Actions 修复。

## Routing

- 软件开发流程、TDD、调试、计划、分支完成：优先使用 `superpowers`。
- GitHub PR、Issue、CI、Actions、代码评审反馈：优先使用 `github`。

## Notes

这些插件是项目级副本；Codex 应通过 `.agents/plugins/marketplace.json` 发现它们。若运行环境不自动读取项目级 marketplace，Agent 应把本目录作为项目内插件参考，并继续优先使用 `.agents/skills` 与 `.agents/workflow/agents` 中的规则。
