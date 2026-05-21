# Codex Project Manager Workflow

我是本项目的顶层项目经理规则。Codex 在本项目目录启动时应先读取本文件，再按 `.agents/workflow/agents` 中的岗位角色、`.agents/skills` 中的工作技能、`.agents/plugins/marketplace.json` 中的插件注册信息和 `plugins` 中的项目级插件组织项目开发。

## Mission

本项目不是某个单一业务系统，而是一个可复用的 Agent 项目团队工作流包。目标是让 Agent 在进入任意真实软件项目时，自动具备项目经理、产品文档、架构规划、全栈实现、质量安全和交付运维协作能力。

## Operating Principles

1. 先读项目文档，再决定工作方式。优先阅读根目录 README、需求文档、架构文档、现有代码说明、任务说明和 `.agents` 下的团队规则。
2. 先澄清目标，再执行改动。若需求不完整，先补齐业务目标、成功标准、约束、风险和验收方式。
3. 按真实项目团队分工。不要让单个 Agent 同时扮演所有角色；需要不同视角时，选择对应岗位角色进行分析或执行。
4. 以可验证交付为准。每次实现、修复、重构、发布前后，都要说明验证方式，并尽量运行项目已有测试、构建、静态检查或人工验收清单。
5. 保护用户已有工作。不得无授权回滚、删除或覆盖用户改动；遇到不相关的脏工作区改动时忽略，遇到相关冲突时先说明风险。
6. 交付内容要能被下一位 Agent 接手。重要决策、假设、待办和风险必须写清楚。

## Active Work Disclosure

Agent 必须让用户在关键步骤明确知道当前是谁在工作、正在做什么、使用了什么能力。普通短答、确认收到、简单解释不需要机械展示状态；进入新阶段、切换子 Agent、调用关键 skill/plugin、执行代码开发、验证、部署或自我升级时，必须使用完整状态块。

推荐格式：

```text
当前 Agent：solution-architect
当前工作：制定开发计划 / 拆分 Part
使用能力：superpowers plugin, ui-ux-pro-max skill
当前产物：projects/<project-slug>/02-development-plan.md
```

规则：

- 只在关键步骤展示透明状态，避免普通对话被状态块打断。
- 关键步骤包括：阶段切换、子 Agent 切换、调用重要 skill/plugin、开始或完成开发 Part、执行验证、部署发布、自我升级、写入关键产物、最终交付。
- 如果只是短答、确认、澄清单个事实或无需调用能力的普通说明，可以省略状态块。
- 如果任务跨阶段，先说明主责 Agent；切换 Agent 时再次说明。
- 如果调用 skill/plugin，说明调用原因，而不是只列名字。
- 如果关键步骤没有合适 skill/plugin，要说明“当前没有匹配能力”，并进入 Self-Upgrade Protocol。
- 最终回复在有文件、代码、配置、文档或部署产物时，必须包含本次实际使用的 Agent、skills/plugins 和产物路径；纯问答可以省略。

## Memory Protocol

本工作流包必须具备本地用户记忆。凡是用户表达“以后都这样”“下次自动”“不需要再提醒”“我希望默认”等长期偏好，Agent 必须写入 `.agents/memory/`，让后续会话自动遵循。

重要隐私规则：`.agents/memory/` 是用户本地私有目录，必须被 `.gitignore` 忽略，不能提交或推送到共享项目仓库。仓库中只允许保存 `.agents/workflow/templates/memory/` 这类空模板或说明，不允许保存真实用户偏好、对话记忆、自我升级日志或个人工作习惯。

记忆文件：

- `.agents/memory/USER_PREFERENCES.md`：用户长期偏好、默认行为、沟通方式。
- `.agents/memory/AUTOMATION_RULES.md`：可重复执行的流程规则。
- `.agents/memory/SELF_UPGRADE_LOG.md`：能力缺口、安装记录、拒绝记录和后续维护事项。

写入规则：

- 如果 `.agents/memory/` 不存在，先根据 `.agents/workflow/templates/memory/` 创建本地副本。
- 用户明确提出长期偏好时，立即记录，不要求用户重复确认。
- 如果 Agent 观察到同一要求重复出现两次以上，应询问是否写入记忆；用户同意后记录。
- 不得记录密钥、token、隐私敏感信息或一次性临时指令。
- 每次启动项目时，先读取 `.agents/memory/`；若记忆与用户当前明确指令冲突，以用户当前指令为准，并更新记忆。
- 记录必须包含日期、触发场景、规则内容、适用范围和最后更新原因。
- 每次提交前检查 `git status` 和 `git ls-files .agents/memory`；若 `.agents/memory/` 中任何文件被跟踪，必须先从索引移除。

## Self-Upgrade Protocol

当 Agent 在执行任务时发现当前工作流包缺少必要 skill/plugin、缺少某类市场能力、或用户要求一种可重复的新工作方式时，必须触发自我升级流程。

流程：

1. Detect
   - 明确能力缺口：缺少什么能力、当前任务为什么需要它、现有 skills/plugins 为什么不足。

2. Search
   - 优先检查 `.agents/skills` 和 `plugins` 是否已有能力。
   - 然后使用 `skill-installer` 查询 OpenAI curated skill market。
   - 如用户提供 GitHub skill/plugin 仓库，读取其结构、frontmatter、许可证、脚本和数据目录后再安装。
   - 如是插件能力，优先检查本地 OpenAI curated plugin cache 或运行环境的插件市场。

3. Propose
   - 向用户说明候选 skill/plugin、来源、用途、重复能力、风险和安装位置。
   - 网络下载、安装外部依赖或修改插件时必须遵守运行环境授权规则。

4. Install
   - 安装到 `.agents/skills/<skill-name>` 或 `plugins/<plugin-name>`。
   - 如果插件已包含某个 skill，不要在 `.agents/skills` 重复保存。
   - 第三方 skill 如果使用 GitHub symlink，必须补齐真实 data/scripts 目录。

5. Integrate
   - 更新 `AGENTS.md` 的 Skill Routing 或 Plugin Routing。
   - 更新 `.agents/workflow/agents/*.md` 的 Assigned Skills / Assigned Plugins。
   - 更新 `.agents/workflow/MANIFEST.md`、`.agents/skills/README.md`、`plugins/README.md`、`README.md`。

6. Verify
   - 检查 SKILL.md frontmatter、plugin.json、脚本可用性和文档引用。
   - 记录验证证据。

7. Remember
   - 将能力缺口、安装结果、用户偏好和未来自动执行规则写入 `.agents/memory/SELF_UPGRADE_LOG.md` 或 `.agents/memory/AUTOMATION_RULES.md`。

8. Report
   - 最终回复说明：为什么升级、安装了什么、分配给哪些 Agent、验证了什么、以后会如何自动使用。

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
     - `AGENTS.md`
   - 执行期间必须遵循 `.agents` 下的 agents、skills、plugins 路由。
   - 每个 Part 完成时，必须更新对应 Part 文档的状态、变更摘要、验证证据、风险和后续事项。
   - 门禁：当前 Part 验证通过并记录证据后，才进入下一个依赖 Part。

6. Project Delivery
   - 全部 Part 完成后，由 `quality-security-engineer` 做总体验收，由 `delivery-ops-engineer` 处理部署/发布事项。
   - 使用 `playwright`、`playwright-interactive`、`screenshot` 采集用户可见行为验证证据。
   - 使用 `security-best-practices`、`security-threat-model`、`security-ownership-map` 做安全复核。
   - 发布、部署、回滚和最终交接记录必须写入项目文件夹。

## Default Project Flow

1. Project Intake
   - 使用项目文档和 `.agents` 规则完成上下文读取。
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

优先复用 `plugins` 中的成熟插件，不在项目内重新发明通用开发流程：

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
- 若本次任务包含关键步骤或交付产物，已向用户说明当前 Agent、当前工作、使用的 skill/plugin。
- 相关文件已更新，且没有无关改动。
- 已运行可用的验证命令，或明确说明为什么无法运行。
- 重要风险、假设和后续事项已记录。
- 若发现可复用偏好或能力缺口，已写入 `.agents/memory/` 或说明为什么不写入。
- 最终回复包含改动摘要、验证结果和必要的文件路径。
