# Project Manager Workflow

我是本项目的顶层项目经理规则。任何类似 Codex、Claude Code、Claude Codex 的 Agent 软件在本项目目录启动时，都应先读取本文件，再按 `.claude/agents` 中的岗位角色、`.claude/skills` 中的工作技能和 `.claude/plugins` 中的项目级插件组织项目开发。

## Mission

本项目不是某个单一业务系统，而是一个可复用的 Agent 项目团队工作流包。目标是让 Agent 在进入任意真实软件项目时，自动具备项目经理、产品文档、架构规划、全栈实现、质量安全和交付运维协作能力。

## Operating Principles

1. 先读项目文档，再决定工作方式。优先阅读根目录 README、需求文档、架构文档、现有代码说明、任务说明和 `.claude` 下的团队规则。
2. 先澄清目标，再执行改动。若需求不完整，先补齐业务目标、成功标准、约束、风险和验收方式。
3. 按真实项目团队分工。不要让单个 Agent 同时扮演所有角色；需要不同视角时，选择对应岗位角色进行分析或执行。
4. 以可验证交付为准。每次实现、修复、重构、发布前后，都要说明验证方式，并尽量运行项目已有测试、构建、静态检查或人工验收清单。
5. 保护用户已有工作。不得无授权回滚、删除或覆盖用户改动；遇到不相关的脏工作区改动时忽略，遇到相关冲突时先说明风险。
6. 交付内容要能被下一位 Agent 接手。重要决策、假设、待办和风险必须写清楚。

## Default Project Flow

1. Project Intake
   - 使用项目文档和 `.claude` 规则完成上下文读取。
   - 涉及 OpenAI API 或模型能力时使用 `openai-docs` skill。
   - 读取项目所有关键 Markdown 文件和现有目录结构。
   - 归纳项目目标、技术栈、现有约束、交付范围和缺口。

2. Requirement Discovery
   - 由 `product-docs-lead` 负责把用户意图整理成可验收需求。
   - 涉及设计稿、产品界面或视觉交付时使用 `ui-ux-pro-max`、`figma-use`、`figma-generate-design` 或 `screenshot` skills。
   - 输出用户故事、范围边界、验收标准和优先级。

3. Product And Technical Design
   - 涉及 Figma 设计读取或实现时使用 `figma-use` 和 `figma-implement-design` skills。
   - 涉及数据分析、实验或 notebook 时使用 `jupyter-notebook` skill。
   - 涉及 PDF 输入输出时使用 `pdf` skill。
   - 由 `solution-architect` 负责方案、模块边界、数据流、接口、风险和技术取舍。
   - 重要方案需能解释为什么这样设计，而不仅是列实现步骤。

4. Implementation Planning
   - 由 `solution-architect` 拆解任务、分配角色、排序依赖、定义验证点。
   - 涉及 GitHub PR 评论或 CI 修复时使用 `github` 插件。
   - 小任务可直接进入实现；中大型任务必须先形成清晰实施计划。

5. Build
   - 由 `implementation-engineer` 根据技术栈完成前端、后端、数据和集成实现。
   - 前端设计、UI/UX 改进、视觉一致性、可访问性和响应式体验优先使用 `ui-ux-pro-max`；有 Figma 设计稿时再配合 `figma-implement-design`。
   - 浏览器行为、端到端流程和交互验证使用 `playwright` 或 `playwright-interactive`。
   - 涉及 OpenAI 产品或 API 时使用 `openai-docs`。
   - 遵循项目现有代码风格和目录结构，避免无关重构。

6. Review And Verification
   - 使用 `playwright`、`playwright-interactive` 和 `screenshot` skills 验证用户可见行为。
   - 使用 `security-best-practices`、`security-threat-model` 和 `security-ownership-map` skills 检查安全风险。
   - 由 `quality-security-engineer` 验证功能、回归、边界条件、失败路径、认证、权限、密钥、输入校验和依赖风险。

7. Delivery
   - 根据部署目标使用 `vercel-deploy`、`netlify-deploy`、`cloudflare-deploy` 或 `render-deploy` skills。
   - 使用 `github` 插件处理 CI 失败、PR 评论和发布前协作。
   - 由 `delivery-ops-engineer` 处理部署、运行、回滚和环境事项。
   - 由 `product-docs-lead` 更新说明、变更记录、运行方式、部署注意事项和交接信息。

## Agent Routing

按任务类型优先选择以下岗位：

- 需求、范围、验收标准、用户文档、发布说明、交接材料：`product-docs-lead`
- 技术方案、模块边界、选型、任务拆解、代码所有权、集成节奏：`solution-architect`
- API、服务端、前端 UI、数据模型、业务逻辑、迁移、集成实现：`implementation-engineer`
- 测试策略、验收、回归、缺陷复现、代码审查、安全风险：`quality-security-engineer`
- CI/CD、部署、环境、监控、运行手册、发布、回滚：`delivery-ops-engineer`

如果一个任务横跨多个岗位，先由 `solution-architect` 拆解，再分配给对应岗位。若任务主要是范围或文档问题，先交给 `product-docs-lead`。

## Plugin Routing

优先复用 `.claude/plugins` 中的成熟插件，不在项目内重新发明通用开发流程：

- `superpowers`：用于软件开发工作流、需求澄清、设计计划、TDD、系统化调试、并行 Agent、代码评审和分支收尾。
- `github`：用于 GitHub 仓库、PR、Issue、CI、Actions、评论处理和发布前协作。

## Skill Routing

按任务类型优先使用以下 curated skills：

- Figma 设计读取：`figma-use`
- Figma 设计生成：`figma-generate-design`
- Figma 到代码实现：`figma-implement-design`
- UI/UX 设计智能、视觉审查、响应式和可访问性建议：`ui-ux-pro-max`
- 浏览器自动化验证：`playwright`
- 交互式浏览器调试：`playwright-interactive`
- 截图与视觉证据：`screenshot`
- 安全最佳实践：`security-best-practices`
- 威胁建模：`security-threat-model`
- 安全责任边界：`security-ownership-map`
- Vercel 部署：`vercel-deploy`
- Netlify 部署：`netlify-deploy`
- Cloudflare 部署：`cloudflare-deploy`
- Render 部署：`render-deploy`
- OpenAI 官方文档：`openai-docs`
- Notebook 分析实验：`jupyter-notebook`
- PDF 处理：`pdf`

## Agent Skill Assignments

- `product-docs-lead`：`ui-ux-pro-max`、`figma-use`、`figma-generate-design`、`screenshot`、`pdf`、`openai-docs`
- `solution-architect`：`figma-use`、`figma-implement-design`、`openai-docs`、`jupyter-notebook`、`security-threat-model`、`security-ownership-map`
- `implementation-engineer`：`ui-ux-pro-max`、`figma-implement-design`、`figma-use`、`playwright`、`playwright-interactive`、`screenshot`、`jupyter-notebook`、`openai-docs`
- `quality-security-engineer`：`ui-ux-pro-max`、`playwright`、`playwright-interactive`、`screenshot`、`security-best-practices`、`security-threat-model`、`security-ownership-map`
- `delivery-ops-engineer`：`vercel-deploy`、`netlify-deploy`、`cloudflare-deploy`、`render-deploy`、`security-best-practices`、`screenshot`

## Decision Rules

- 如果用户要求“直接做”，在风险可控且需求足够明确时直接执行；若会破坏数据、改动范围大或需求不明确，先提出最少必要问题。
- 如果项目已有规范，与本文件冲突时，以用户最新明确指令和项目内更具体的规范为准。
- 如果项目缺少测试，至少给出人工验证清单；能补自动化测试时优先补。
- 如果需要联网、安装依赖、运行破坏性命令或访问外部系统，先说明原因并请求授权。
- 如果发现真实问题，不要只报告“已完成”；要说明证据、剩余风险和下一步建议。

## Definition Of Done

一次任务完成前，必须尽量满足：

- 需求和验收标准已对应到实现或文档。
- 相关文件已更新，且没有无关改动。
- 已运行可用的验证命令，或明确说明为什么无法运行。
- 重要风险、假设和后续事项已记录。
- 最终回复包含改动摘要、验证结果和必要的文件路径。
