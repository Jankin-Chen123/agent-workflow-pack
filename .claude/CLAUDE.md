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

## Project Folder Convention

每个使用本工作流包开发的新项目，必须在仓库根目录下创建独立项目文件夹：

```text
projects/<project-slug>/
  00-requirements.md
  01-design-spec.md
  02-development-plan.md
  parts/
    part-001-<short-name>.md
  decisions/
  evidence/
```

命名规则：

- `<project-slug>` 使用小写英文、数字和短横线。
- 需求文档、设计规范、开发计划是该项目后续所有任务的上位依据。
- 每个 Part 独立记录目标、范围、依赖、执行状态、验证证据和交接说明。
- 后续任何代码开发、修复、重构、测试、发布，都必须先读取该项目文件夹中的 3 份核心产物。

## Required Lifecycle

本工作流包的默认用户交互流程是“先共创项目，再逐步实现”。Agent 不得跳过阶段门禁。

1. Requirement Co-Creation
   - 通过聊天交互和插件/skill 调用，与用户持续澄清项目愿景、目标用户、项目名称、业务场景、功能范围、约束、验收标准和非目标。
   - 优先使用 `superpowers` 插件进行需求澄清、头脑风暴和规格化对话。
   - 涉及 OpenAI API 或模型能力时使用 `openai-docs` skill。
   - 产出 `00-requirements.md`，必须明确项目名称、项目目标、用户角色、核心功能、详细需求、验收标准、约束、风险和待确认问题。
   - 门禁：用户确认需求文档后，才进入项目文件夹初始化。

2. Project Folder Initialization
   - 在 `projects/<project-slug>/` 下创建项目文件夹。
   - 将已确认的需求文档写入 `00-requirements.md`。
   - 同步创建 `parts/`、`decisions/`、`evidence/` 子目录。
   - 门禁：项目文件夹和需求文档存在后，才进入设计规范阶段。

3. Design Specification
   - 继续与用户探讨产品结构、信息架构、交互流程、视觉方向、页面清单、组件规范、响应式策略、可访问性要求和关键设计图。
   - UI/UX 设计、视觉审查、响应式和可访问性建议优先使用 `ui-ux-pro-max`。
   - 有 Figma 设计稿或需要生成/实现 Figma 设计时，使用 `figma-use`、`figma-generate-design`、`figma-implement-design`。
   - 产出 `01-design-spec.md`，其中必须包含设计规范和项目设计图。设计图可以是 Mermaid 图、页面结构图、流程图、Figma 链接、截图或其他可复查的视觉产物。
   - 门禁：用户确认设计规范后，才进入开发计划阶段。

4. Development Planning
   - 基于 `00-requirements.md` 和 `01-design-spec.md` 制定开发计划。
   - 优先使用 `superpowers` 插件进行计划、TDD、并行 Agent 和开发分支方法论。
   - 由 `solution-architect` 将项目拆成多个可独立完成的 Part；每个 Part 必须有清晰目标、范围、依赖、输入、输出、验收标准、负责角色和验证方式。
   - 支持多人或多 Agent 前期异步协作，但必须避免共享写入冲突，并明确集成顺序。
   - 产出 `02-development-plan.md`，并在 `parts/` 下为每个 Part 创建独立 Part 文档。
   - 门禁：用户确认开发计划后，才开始代码开发。

5. Part-By-Part Implementation
   - 每次只执行一个明确 Part，或执行多个没有共享写入冲突的 Part。
   - 开始任何 Part 前，必须读取：
     - `00-requirements.md`
     - `01-design-spec.md`
     - `02-development-plan.md`
     - 当前 Part 文档
     - `.claude/CLAUDE.md`
   - 执行期间必须遵循 `.claude` 下的 agents、skills、plugins 路由。
   - 每个 Part 完成时，必须更新对应 Part 文档的状态、变更摘要、验证证据、风险和后续事项。
   - 门禁：当前 Part 验证通过并记录证据后，才进入下一个依赖 Part。

6. Project Delivery
   - 全部 Part 完成后，由 `quality-security-engineer` 做总体验收，由 `delivery-ops-engineer` 处理部署/发布事项。
   - 使用 `playwright`、`playwright-interactive`、`screenshot` 采集用户可见行为验证证据。
   - 使用 `security-best-practices`、`security-threat-model`、`security-ownership-map` 做安全复核。
   - 发布、部署、回滚和最终交接记录必须写入项目文件夹。

## Default Project Flow

1. Project Intake
   - 使用项目文档和 `.claude` 规则完成上下文读取。
   - 涉及 OpenAI API 或模型能力时使用 `openai-docs` skill。
   - 读取项目所有关键 Markdown 文件和现有目录结构。
   - 归纳项目目标、技术栈、现有约束、交付范围和缺口。

2. Requirement Discovery
   - 由 `product-docs-lead` 负责把用户意图整理成可验收需求。
   - 涉及设计稿、产品界面或视觉交付时使用 `ui-ux-pro-max`、`figma-use`、`figma-generate-design` 或 `screenshot` skills。
   - 输出并维护 `projects/<project-slug>/00-requirements.md`。

3. Product And Technical Design
   - 涉及 Figma 设计读取或实现时使用 `figma-use` 和 `figma-implement-design` skills。
   - 涉及数据分析、实验或 notebook 时使用 `jupyter-notebook` skill。
   - 涉及 PDF 输入输出时使用 `pdf` skill。
   - 由 `solution-architect` 负责方案、模块边界、数据流、接口、风险和技术取舍。
   - 输出并维护 `projects/<project-slug>/01-design-spec.md`。

4. Implementation Planning
   - 由 `solution-architect` 拆解任务、分配角色、排序依赖、定义验证点。
   - 涉及 GitHub PR 评论或 CI 修复时使用 `github` 插件。
   - 输出并维护 `projects/<project-slug>/02-development-plan.md` 和 `projects/<project-slug>/parts/*.md`。

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
- 如果项目还没有 `00-requirements.md`、`01-design-spec.md`、`02-development-plan.md`，不得直接开始代码开发，除非用户明确要求临时原型。
- 如果用户要求临时原型，必须记录为探索性工作，不能替代正式需求、设计和开发计划。
- 如果项目已有规范，与本文件冲突时，以用户最新明确指令和项目内更具体的规范为准。
- 如果项目缺少测试，至少给出人工验证清单；能补自动化测试时优先补。
- 如果需要联网、安装依赖、运行破坏性命令或访问外部系统，先说明原因并请求授权。
- 如果发现真实问题，不要只报告“已完成”；要说明证据、剩余风险和下一步建议。

## Definition Of Done

一次任务完成前，必须尽量满足：

- 需求和验收标准已对应到实现或文档。
- 已读取并遵循当前项目的需求文档、设计规范、开发计划和 Part 文档。
- 相关文件已更新，且没有无关改动。
- 已运行可用的验证命令，或明确说明为什么无法运行。
- 重要风险、假设和后续事项已记录。
- 最终回复包含改动摘要、验证结果和必要的文件路径。
