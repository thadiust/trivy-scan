# Changelog

## Unreleased

- **SARIF `mode: both`**: merge **fs** and **config** into a single SARIF **`runs[0]`** (combined **`results`**, deduped **`tool.driver.rules`** by **`id`**) so **`github/codeql-action/upload-sarif`** accepts the file (one run per category; [GitHub changelog 2025-07-21](https://github.blog/changelog/2025-07-21-code-scanning-will-stop-combining-multiple-sarif-runs-uploaded-in-the-same-sarif-file/)).
- **Fix `trivy config`**: do not pass **`--no-progress`** or **`--ignore-unfixed`** (not registered on the config subcommand; caused flag error + full help text in CI).
- Enforce **one** space-separated **`paths`** entry — Trivy allows a single **`PATH`** per run.
- Default **`trivy_version`** **`0.69.3`** ( **`0.65.0`** was not published — install returned HTTP 404).
- Clearer **curl** error when checksums or tarball URL is missing.

## [0.1.0] — 2026-04-10

- Initial composite action: Trivy `fs` and `config` scans with optional merged SARIF output.
- Repo scaffold: `LICENSE`, Dependabot, `actionlint` workflow (reusable from `workflow-python`).

