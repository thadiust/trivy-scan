# trivy-scan

Composite GitHub Action that runs **[Trivy](https://github.com/aquasecurity/trivy)** in **Wave 1** style:

- `mode=fs` → `trivy fs` (filesystem scan).
- `mode=config` → `trivy config` (IaC/config scan).
- `mode=both` → runs both and merges SARIF runs into one report.

The action always produces machine-readable JSON internally to compute **`finding_count`**, and optionally writes a **SARIF 2.1** report for upload via `github/codeql-action/upload-sarif`.

## Caller responsibilities

- Run on **`ubuntu-latest`** (Linux amd64 tarball install). **`jq`** must be available (included on GitHub-hosted runners; install it on self-hosted runners).
- `actions/checkout` must run before this action.

## Behavior

- **0 findings** → `scan_status=clean`, step succeeds.
- **≥ 1 finding** → `scan_status=findings_found`. If `fail_on_findings=true`, the step exits **1**.
- **Scanner/runtime errors** → `scan_status=scanner_error`, step exits **2**.

## Inputs

| Input | Default | Description |
|------|---------|-------------|
| `trivy_version` | `0.69.3` | Release version (no `v` prefix); must match a real [Trivy release](https://github.com/aquasecurity/trivy/releases). |
| `working_directory` | `.` | Directory to scan (relative to repo root). |
| `mode` | `both` | `fs`, `config`, or `both`. |
| `paths` | `.` | Space-separated paths relative to `working_directory`. |
| `severity` | `HIGH,CRITICAL` | Comma-separated severities. |
| `ignore_unfixed` | `true` | If true, pass **`--ignore-unfixed`** to **`trivy fs`** only (vulnerability scanner). Ignored for **`trivy config`** (misconfig has no unfixed vuln concept). |
| `fail_on_findings` | `true` | If true, exit 1 when findings exist. |
| `write_sarif` | `false` | If true, write SARIF to `sarif_filename`. |
| `sarif_filename` | `trivy-results.sarif` | Path relative to `working_directory`. |

## Outputs

| Output | Description |
|--------|-------------|
| `finding_count` | Total findings across the selected scan mode(s). |
| `scan_status` | `clean`, `findings_found`, or `scanner_error`. |
| `sarif_path` | Repo-relative path to SARIF when `write_sarif=true`; empty otherwise. |

## Example

```yaml
- uses: actions/checkout@v4

- uses: thadiust/trivy-scan@main
  with:
    working_directory: "."
    mode: "both"
    paths: "."
    severity: "HIGH,CRITICAL"
    ignore_unfixed: true
    write_sarif: true
    sarif_filename: "reports/trivy-results.sarif"
    fail_on_findings: true
```

## Publish this repository (first time)

`workflow-python` references **`uses: thadiust/trivy-scan@main`**. This folder is its **own** git repository (not the parent `security-pipeline` workspace).

1. On GitHub: **New repository** → name **`trivy-scan`**, owner **`thadiust`**, **empty** (no README / .gitignore from the UI).
2. From this directory:

```bash
cd /path/to/trivy-scan
git remote add origin https://github.com/thadiust/trivy-scan.git   # skip if already added
git push -u origin main
```

3. After the first push, **`thadiust/trivy-scan@main`** resolves and **`workflow-python`** CI can use it.

