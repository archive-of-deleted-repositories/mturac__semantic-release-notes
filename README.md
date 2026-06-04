# semantic-release-notes

Claude Code plugin that auto-generates human-readable changelogs and release notes from conventional commits and PR titles.

## Install

```bash
claude plugin install semantic-release-notes
```

Or clone and install locally:

```bash
git clone <repo-url> ~/.claude/plugins/semantic-release-notes
claude plugin install --local ~/.claude/plugins/semantic-release-notes
```

## Commands

| Command | Description |
|---|---|
| `/release-notes [from] [to]` | Draft release notes between two refs |
| `/release-draft [version]` | Draft notes for a specific version (auto-detects bump if omitted) |
| `/changelog-update` | Insert latest release into CHANGELOG.md (shows diff, asks before writing) |
| `/release-bump` | Show suggested semver bump from commits since last tag |

## Example Output

```
## v1.2.0 — 2024-01-15

### Breaking Changes
- Removed deprecated /v1/users endpoint

### Features
- (auth) Add OAuth2 PKCE flow (#234)
- (api) Add cursor-based pagination (#228)

### Bug Fixes
- (api) Fix pagination offset calculation (#231)

### Performance
- (db) Reduce query count on list endpoints (#229)
```

## Conventional Commit Reference

| Type | Description | Included by default |
|---|---|---|
| `feat` | New feature | Yes (Features) |
| `fix` | Bug fix | Yes (Bug Fixes) |
| `perf` | Performance improvement | Yes (Performance) |
| `revert` | Revert a commit | Yes (Reverts) |
| `docs` | Documentation only | No |
| `style` | Formatting, whitespace | No |
| `refactor` | Code restructure, no behavior change | No |
| `test` | Adding/updating tests | No |
| `chore` | Build process, tooling | No |
| `ci` | CI configuration | No |
| `build` | Build system changes | No |

Pass `--all` to any command to include all types.

**Breaking changes** are detected from:
- `!` after the type/scope: `feat(api)!: remove v1 endpoint`
- `BREAKING CHANGE:` in the commit body/footer

## Semver Bump Rules

| Commits contain | Bump |
|---|---|
| Any breaking change | **major** |
| Any `feat` | **minor** |
| Only `fix` / `perf` / other | **patch** |

## Skills

- `commit-parse` — fetches and parses commits between refs
- `release-draft` — generates versioned release notes
- `changelog-update` — inserts a release section into CHANGELOG.md
