# Changelog

## Unreleased

- **Fix `trivy config`**: do not pass **`--no-progress`** or **`--ignore-unfixed`** (not registered on the config subcommand; caused flag error + full help text in CI).
- Enforce **one** space-separated **`paths`** entry — Trivy allows a single **`PATH`** per run.
- Default **`trivy_version`** **`0.69.3`** ( **`0.65.0`** was not published — install returned HTTP 404).
- Clearer **curl** error when checksums or tarball URL is missing.

## [0.1.0] — 2026-04-10

- Initial composite action: Trivy `fs` and `config` scans with optional merged SARIF output.
- Repo scaffold: `LICENSE`, Dependabot, `actionlint` workflow (reusable from `workflow-python`).

