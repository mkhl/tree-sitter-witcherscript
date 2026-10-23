# Release Process

## 1. Land all changes on `master`

Ensure all code changes, grammar updates, and tests are committed and pushed. Merge the `ci: parser` PR if `build-and-test` opens one.

## 2. Commit the version bump

```sh
tree-sitter version X.Y.Z
cargo update -p tree-sitter-witcherscript --offline
```

This bumps the version in these files; commit no other changes:

| File | Field |
|------|-------|
| `package.json` | `"version"` |
| `Cargo.toml` | `version` |
| `Cargo.lock` | `tree-sitter-witcherscript` version |
| `pyproject.toml` | `version` |
| `CMakeLists.txt` | `VERSION` |
| `Makefile` | `VERSION :=` |
| `tree-sitter.json` | `metadata.version` |

Commit message must be exactly:

```
Version X.Y.Z
```

Push the commit, then merge the `ci: Update package-lock.json` PR it triggers.

## 3. Trigger the release workflow

Run the **Create Release** workflow from GitHub Actions (`workflow_dispatch`). The tag `vX.Y.Z` is read from `tree-sitter.json`. Inputs:

- **Title** - optional, defaults to the tag
- **Summary** - optional, prepended above auto-generated release notes
- **Pre-release** - check if applicable

The workflow generates the parser, runs the test suite, and creates a draft release. Publish the draft once reviewed.
