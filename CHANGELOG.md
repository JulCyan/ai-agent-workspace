# Changelog

All notable changes to this project will be documented in this file.

## [0.2.0] - 2026-01-29

### Added

- **Smart Rules System**: Introduced tag-based rule loading strategy (`agent.active_rule_tags`) to dynamically load context-relevant rules.
- **Meta-Workspace**: Added `.workspace/ai.code-workspace` to manage the Workspace configuration repo itself.
- **Process Rules**: Separated development workflow rules into `.agent/rules/process.md`.

### Changed

- **Workspace-First Architecture**:
  - Moved all workspace definitions to `.workspace/` directory.
  - Standardized `base.code-workspace` as the single source of truth for settings.
  - Updated `README.md` with strict guidelines (No `code .`).
- **Git Visibility**: Configured `git.ignoredRepositories` to hide the root meta-repo in sub-project workspaces.
- **Documentation**: Standardized global rules in `GEMINI.md` with A/B/C structure (Language, Rule System, Loading Strategy).

### Deprecated

- **.vscode/settings.json**: effective settings moved to `.code-workspace` files.

## [0.1.0] - 2026-01-28

### Added

- Initial release of AI Agent Workspace.
- Centralized `.agent/rules` configuration.
- Workspace isolation with `.gitignore` whitelisting.
- Multi-repo support via `.code-workspace` files.
- Context syncing strategy.
