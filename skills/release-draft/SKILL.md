# release-draft

Draft release notes for a version from conventional commits.

## Inputs

- `version` — explicit version string (e.g. `v1.2.0`) OR `auto` to determine from bump logic
- `from` — start ref (defaults to last git tag)
- `to` — end ref (defaults to `HEAD`)
- `--write` — optional flag; if set, also write to `RELEASE_NOTES.md`

## Steps

1. **Resolve date range**
   - `from`: run `git describe --tags --abbrev=0` to find the last tag if not supplied.
   - `to`: default `HEAD`.
   - `date`: use `date +%Y-%m-%d` (current date).

2. **Parse commits**
   Invoke the `commit-parse` skill with the resolved `from`..`to` range.

3. **Determine semver bump** (when `version` is `auto`)
   | Condition | Bump |
   |---|---|
   | Any commit with breaking change | **major** |
   | Any `feat` commit | **minor** |
   | Only `fix` / `perf` / other | **patch** |
   Read current version from `package.json` → `version` field if present; otherwise start from `v0.1.0`.

4. **Generate release notes**
   ```markdown
   ## v1.2.0 — 2024-01-15

   ### Breaking Changes
   - Removed deprecated /v1/users endpoint

   ### Features
   - (auth) Add OAuth2 PKCE flow (#234)

   ### Bug Fixes
   - (api) Fix pagination offset calculation (#231)

   ### Performance
   - (db) Reduce query count on list endpoints (#229)
   ```
   - Sections are omitted if empty.
   - Each bullet: `(scope) description (#PR)` — omit scope/PR if absent.

5. **Output**
   - Always print to stdout.
   - If `--write` flag: write/overwrite `RELEASE_NOTES.md` in cwd.

6. **Guardrail**
   Never run `git tag`, `git push`, or any publish command.
   This skill only drafts — it does not release.
