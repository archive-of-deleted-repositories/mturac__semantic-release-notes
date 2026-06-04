# changelog-update

Insert a new release section into CHANGELOG.md following Keep a Changelog format.

## Steps

1. **Check for CHANGELOG.md**
   - If it does not exist, create it with this header:
     ```markdown
     # Changelog

     All notable changes to this project will be documented in this file.

     The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
     and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
     ```

2. **Obtain the new release section**
   - Call `release-draft` skill (or accept pre-drafted text from the caller).
   - The section must be in the format produced by `release-draft`.

3. **Locate insertion point**
   - Find the line immediately after the `# Changelog` header block (the first blank line after the header).
   - Insert the new release section there, followed by a blank line.

4. **Preserve existing entries**
   - All existing release sections remain untouched below the insertion point.

5. **Show diff before writing**
   ```bash
   diff CHANGELOG.md CHANGELOG.md.new
   ```
   Print the unified diff to stdout.

6. **Confirm with user**
   Ask: `Write these changes to CHANGELOG.md? [y/N]`
   - `y` → overwrite `CHANGELOG.md` with the new content.
   - `N` or no input → discard, leave file unchanged.

## Keep a Changelog section format

```markdown
## [1.2.0] — 2024-01-15

### Added
- (auth) Add OAuth2 PKCE flow (#234)

### Fixed
- (api) Fix pagination offset calculation (#231)

### Changed
- (db) Reduce query count on list endpoints (#229)

### Breaking Changes
- Removed deprecated /v1/users endpoint
```

Map release-draft groups → Keep a Changelog sections:
| release-draft | Keep a Changelog |
|---|---|
| Features | Added |
| Bug Fixes | Fixed |
| Performance | Changed |
| Breaking Changes | Breaking Changes |
| Reverts | Reverted |
