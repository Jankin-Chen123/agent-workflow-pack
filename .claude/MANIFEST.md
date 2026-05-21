# Workflow Manifest

本文件记录 `.claude` 工作流包的来源、安装内容和维护规则，方便后续 Agent 审计和更新。

## Agents

项目保留 5 个复合子 Agent：

- `product-docs-lead`
- `solution-architect`
- `implementation-engineer`
- `quality-security-engineer`
- `delivery-ops-engineer`

## Plugins

项目级插件位于 `.claude/plugins`。

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

项目级 skills 位于 `.claude/skills`。

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
  - Installed path: `.claude/skills/ui-ux-pro-max`
  - Note: data and scripts were copied from `src/ui-ux-pro-max` because the skill folder uses repository symlinks.

Removed intentionally:

- gh-fix-ci
- gh-address-comments

Reason: the `github` plugin already contains these GitHub operation skills, so standalone duplicates were removed from `.claude/skills`.

## Maintenance Rules

1. Prefer marketplace, curated, or well-used third-party skills over custom local skills.
2. If a plugin already provides a skill, do not duplicate that skill in `.claude/skills`.
3. After adding or removing skills/plugins, update:
   - `.claude/CLAUDE.md`
   - `.claude/agents/README.md`
   - `.claude/skills/README.md`
   - `.claude/plugins/README.md`
   - `README.md`
4. Verify every project-doc reference resolves to an installed skill, plugin, or Agent.
5. Restart Codex after installing new skills or plugins so the runtime can discover them.
