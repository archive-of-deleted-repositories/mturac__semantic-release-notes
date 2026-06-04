# /release-draft

Draft release notes for a specific version.

## Usage

```
/release-draft [version]
```

- `version` — explicit version string (e.g. `v1.2.0`) or `auto` to infer from bump logic. Defaults to `auto`.

## Behavior

1. Resolve last tag → HEAD as the commit range.
2. Run `commit-parse` skill.
3. Determine semver bump if `version` is `auto`.
4. Generate formatted release notes and print to stdout.
5. Ask the user whether to also write `RELEASE_NOTES.md`.

Guardrail: never tags or pushes. Drafts only.

## Examples

```
/release-draft              # auto-determine version
/release-draft v2.0.0       # explicit major bump
/release-draft v1.3.0       # explicit minor bump
```
