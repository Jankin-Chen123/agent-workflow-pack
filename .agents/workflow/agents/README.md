# Agents

本目录把 `.agents` 工作流组织成少量复合项目岗位。每个文件都是一个可由 Agent 软件加载或参考的角色定义。

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

先由 `AGENTS.md` 判断任务阶段，再选择一个主责岗位。跨岗位任务由 `solution-architect` 拆解后再分配；范围和文档类任务由 `product-docs-lead` 牵头。

## Visibility Rule

每个子 Agent 只在关键步骤向用户说明当前状态，避免普通短答被状态块打断。关键步骤包括开始重要工作、切换阶段、切换主责 Agent、调用关键 skill/plugin、执行开发、验证、部署、自我升级或交付产物。

关键步骤必须说明：

- 当前 Agent
- 当前工作
- 使用的 skill/plugin 以及原因
- 当前正在更新或产出的文件

推荐格式：

```text
当前 Agent：implementation-engineer
当前工作：实现 Part 003 的前端交互
使用能力：ui-ux-pro-max（UI/UX 检查），playwright（浏览器验证）
当前产物：projects/<project-slug>/parts/part-003-frontend-flow.md
```
