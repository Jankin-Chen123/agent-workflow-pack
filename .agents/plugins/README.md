# Plugin Marketplace

This directory contains Codex plugin marketplace metadata for this workflow pack.

- `marketplace.json` registers local plugins stored at the repository root under `plugins/<plugin-name>`.
- `superpowers` is installed by default because the workflow depends on its planning, TDD, debugging, review, and delivery skills.
- `github` is available for GitHub collaboration tasks and may require authentication in the user's Codex environment.

Do not place plugin source code in this directory. Keep plugin source folders in root-level `plugins/` so marketplace paths remain stable after drag-and-drop import.
