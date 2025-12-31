# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-01-01

### Added
- `allowed-tools` frontmatter to all skills for automatic tool approval
  - `jadongsanyang-setup`: Glob, Read, Edit, Write, Bash(mkdir:*)
  - `jadongsanyang-ready`: Glob, Read
  - `jadongsanyang-planning`: Glob, Grep, Read, Write, Task
  - `jadongsanyang-start`: Glob, Grep, Read, Edit, Write, Bash(git:*), Bash(gh:*), Bash(npm:*), Bash(yarn:*), Bash(npx:*), TodoWrite
- Auto-permission setup: `jadongsanyang-setup` now adds permissions to `.claude/settings.json`
- Explicit prohibition of `bash(for...)` loops in todo scanning (use Glob/Read instead)
- Frontmatter completion status filtering (`completed: true`, `isCompleted: true`)
- Permission configuration guide in README

### Changed
- `jadongsanyang-start` now uses `todos/**/*.md` pattern (includes subfolders)

## [1.0.0] - 2024-12-15

### Added
- Initial release
- `jadongsanyang-setup`: Project setup command
- `jadongsanyang-planning`: Codebase analysis and todo generation
- `jadongsanyang-ready`: Todo list analysis and task recommendation
- `jadongsanyang-start`: Task implementation and PR creation
- Monorepo support with path filtering
- Korean language interface
