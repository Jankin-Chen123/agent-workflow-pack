# Workflow Manifest

本文件记录 `.agents` 工作流包的来源、安装内容和维护规则，方便后续 Agent 审计和更新。

## Agents

项目保留 5 个复合子 Agent：

- `product-docs-lead`
- `solution-architect`
- `implementation-engineer`
- `quality-security-engineer`
- `delivery-ops-engineer`

## Plugins

项目级插件位于 `plugins`。

- `superpowers`
  - Source: local OpenAI curated plugin cache
  - Purpose: planning, TDD, debugging, review, parallel-agent, and branch completion workflows
- `github`
  - Source: local OpenAI curated plugin cache
  - Purpose: GitHub PR, issue, comment, CI, and Actions workflows

Removed intentionally:

- notion: not needed for the current workflow package.
- supabase: not needed for the current workflow package.

## Skills

项目级 skills 位于 `.agents/skills`。

OpenAI curated skills:

- `cloudflare-deploy`
- `figma-generate-design`
- `figma-implement-design`
- `figma-use`
- `jupyter-notebook`
- `netlify-deploy`
- `openai-docs`
- `pdf`
- `playwright`
- `playwright-interactive`
- `render-deploy`
- `screenshot`
- `security-best-practices`
- `security-ownership-map`
- `security-threat-model`
- `vercel-deploy`

Third-party skill:

- `ui-ux-pro-max`
  - Source: `https://github.com/nextlevelbuilder/ui-ux-pro-max-skill.git`
  - Installed path: `.agents/skills/ui-ux-pro-max`
  - Note: data and scripts were copied from `src/ui-ux-pro-max` because the skill folder uses repository symlinks.

Removed intentionally:

- gh-fix-ci
- gh-address-comments

Reason: the `github` plugin already contains these GitHub operation skills, so standalone duplicates were removed from `.agents/skills`.

## Templates

项目产物模板位于 `.agents/workflow/templates/project`。

- `00-requirements.template.md`: requirements document template
- `01-design-spec.template.md`: design specification and design artifact template
- `02-development-plan.template.md`: development plan and Part breakdown template
- `part.template.md`: independent Part execution record template

Agents should copy these templates into `projects/<project-slug>/` and fill them through user collaboration.

## Local Memory

本地用户记忆位于 `.agents/memory`。该目录包含用户偏好、对话中沉淀的重复流程、自我升级日志和本地自动化规则，属于用户私有状态。

`.agents/memory` 必须被 `.gitignore` 忽略，不能提交或推送到 GitHub。

- `USER_PREFERENCES.md`: durable user preferences and communication defaults
- `AUTOMATION_RULES.md`: repeatable workflows that should run without repeated reminders
- `SELF_UPGRADE_LOG.md`: missing capabilities, installed skills/plugins, and verification evidence

Agents must read this directory at startup and update it when the user creates a durable preference or when the workflow upgrades itself.

可提交的记忆模板位于 `.agents/workflow/templates/memory`。如果 `.agents/memory` 不存在，Agent 应复制模板并在本地填充。

## Maintenance Rules

1. Prefer marketplace, curated, or well-used third-party skills over custom local skills.
2. If a plugin already provides a skill, do not duplicate that skill in `.agents/skills`.
3. Every generated project must live under `projects/<project-slug>/` and keep requirements, design specification, development plan, and Part documents as source-of-truth artifacts.
4. Do not start production code for a generated project until `00-requirements.md`, `01-design-spec.md`, and `02-development-plan.md` exist and have been accepted by the user.
5. Every major user-facing update should disclose current Agent, current work, used skills/plugins, and current artifact.
6. When missing capabilities are discovered, follow the Self-Upgrade Protocol in `AGENTS.md`.
7. After adding or removing skills/plugins, update:
   - `AGENTS.md`
   - `.agents/workflow/agents/README.md`
   - `.agents/skills/README.md`
   - `plugins/README.md`
   - `README.md`
8. Verify every project-doc reference resolves to an installed skill, plugin, Agent, local memory path, or documented template path.
9. Before every commit, verify `.agents/memory` is not tracked by Git.
10. Restart Codex after installing new skills or plugins so the runtime can discover them.
