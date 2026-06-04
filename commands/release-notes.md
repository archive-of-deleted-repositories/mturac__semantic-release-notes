# /release-notes

Draft release notes between two git refs.

## Usage

```
/release-notes [from] [to]
```

- `from` — start ref (tag, branch, SHA). Defaults to the last git tag.
- `to` — end ref. Defaults to `HEAD`.

## Behavior

1. Run the `commit-parse` skill for the `from`..`to` range.
2. Run the `release-draft` skill with the parsed commits.
3. Print the formatted release notes to stdout.

Does not write any files or run any git commands beyond log/describe.

## Examples

```
/release-notes                      # last tag → HEAD
/release-notes v1.1.0               # v1.1.0 → HEAD
/release-notes v1.0.0 v1.1.0        # between two tags
/release-notes main feature/auth    # branch diff
```
