# /release-bump

Suggest a semver bump based on commits since the last tag.

## Usage

```
/release-bump
```

## Behavior

1. Find last tag: `git describe --tags --abbrev=0`
2. Run `commit-parse` skill for last-tag → HEAD.
3. Apply bump rules:
   | Condition | Suggested bump |
   |---|---|
   | Any breaking change | **major** |
   | Any `feat` commit | **minor** |
   | Only `fix`, `perf`, or other | **patch** |
4. Print current version, suggested next version, and a one-line rationale.
5. List the commits that drove the bump decision.

Does not modify any files or run git commands beyond log/describe.

## Example output

```
Current version : v1.1.0
Suggested bump  : minor  (2 feat commits found)
Next version    : v1.2.0

Driving commits:
  feat(auth): Add OAuth2 PKCE flow (#234)
  feat(api): Add cursor-based pagination (#228)
```
