# Agents

本目录把 `.claude` 工作流组织成少量复合项目岗位。每个文件都是一个可由 Agent 软件加载或参考的角色定义。

## Roles

- `product-docs-lead.md`：需求、范围、优先级、验收标准、README、发布说明和交接材料。
- `solution-architect.md`：架构、模块边界、接口、数据流、任务拆解、技术风险和工程取舍。
- `implementation-engineer.md`：前端、后端、数据、API、UI、业务逻辑、迁移和集成实现。
- `quality-security-engineer.md`：测试策略、回归、验收、缺陷复现、代码审查和安全风险。
- `delivery-ops-engineer.md`：环境、CI/CD、部署、监控、运行手册、发布和回滚。

## Skill Assignment

- `product-docs-lead`：`ui-ux-pro-max`、`figma-use`、`figma-generate-design`、`screenshot`、`pdf`、`openai-docs`；插件：`superpowers`
- `solution-architect`：`figma-use`、`figma-implement-design`、`openai-docs`、`jupyter-notebook`、`security-threat-model`、`security-ownership-map`；插件：`superpowers`、`github`
- `implementation-engineer`：`ui-ux-pro-max`、`figma-implement-design`、`figma-use`、`playwright`、`playwright-interactive`、`screenshot`、`jupyter-notebook`、`openai-docs`；插件：`superpowers`、`github`
- `quality-security-engineer`：`ui-ux-pro-max`、`playwright`、`playwright-interactive`、`screenshot`、`security-best-practices`、`security-threat-model`、`security-ownership-map`；插件：`superpowers`、`github`
- `delivery-ops-engineer`：`vercel-deploy`、`netlify-deploy`、`cloudflare-deploy`、`render-deploy`、`security-best-practices`、`screenshot`；插件：`github`

## Routing Rule

先由 `.claude/CLAUDE.md` 判断任务阶段，再选择一个主责岗位。跨岗位任务由 `solution-architect` 拆解后再分配；范围和文档类任务由 `product-docs-lead` 牵头。
