# commit-parse

Parse conventional commits between two git refs and emit a structured list.

## Steps

1. **Fetch commits**
   ```bash
   git log <from>..<to> --format='%H %s %b' --no-merges
   ```
   - `<from>` and `<to>` are tags, branches, or SHAs supplied by the caller.
   - Omit `<from>` to fetch all commits up to `<to>` (or HEAD if `<to>` is also omitted).

2. **Parse conventional commit format**
   Pattern: `type(scope)!: description` or `type: description`
   - **types**: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `ci`, `build`, `revert`
   - **scope** (optional): word inside parentheses
   - **breaking marker**: `!` after type/scope OR `BREAKING CHANGE:` in the commit body/footer

3. **Extract breaking changes**
   - `!` after the type/scope → mark as breaking, extract description
   - `BREAKING CHANGE: <text>` in the body → capture `<text>` as the breaking change note

4. **Group commits**
   | Group | Types |
   |---|---|
   | Features | `feat` |
   | Bug Fixes | `fix` |
   | Performance | `perf` |
   | Breaking Changes | any commit with breaking marker |
   | Reverts | `revert` |

5. **Skip by default**
   `chore`, `test`, `style`, `ci`, `build`, `docs`, `refactor`
   Pass `--all` to include everything.

6. **Deduplicate**
   Remove commits whose subject is identical to another in the set (common with merge commits back-merging the same change).

7. **Extract PR number**
   Scan subject and body for `#\d+`; capture the first match as the PR reference.

8. **Output format** (structured list, one entry per commit)
   ```
   type | scope | description | SHA (short 7-char) | PR
   feat | auth  | Add OAuth2 PKCE flow | a1b2c3d | #234
   fix  | api   | Fix pagination offset | e4f5g6h | #231
   ```
   Emit as plain text table or JSON array depending on caller preference.
