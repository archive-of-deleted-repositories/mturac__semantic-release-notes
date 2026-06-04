# /changelog-update

Update CHANGELOG.md with the latest release section.

## Usage

```
/changelog-update
```

No arguments. Operates on the current working directory.

## Behavior

1. Run `release-draft` skill to obtain the new release section.
2. Run `changelog-update` skill:
   - Creates CHANGELOG.md if absent.
   - Inserts new section at the top.
   - Shows diff.
   - Waits for user confirmation before writing.

## Example output (diff shown before confirm)

```diff
--- CHANGELOG.md
+++ CHANGELOG.md.new
@@ -4,6 +4,18 @@
+## [1.2.0] — 2024-01-15
+
+### Added
+- (auth) Add OAuth2 PKCE flow (#234)
+
+### Fixed
+- (api) Fix pagination offset (#231)
+
 ## [1.1.0] — 2023-12-01
```
