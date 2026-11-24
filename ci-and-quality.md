# Standard: CI and quality

## Required reusable components

Every repository must run the three reusable workflows from
`garretpatten/template`, pinned as shown:

| Component                                                                     | Pin type                                                                  |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `garretpatten/quality-checks/.github/workflows/quality-checks.yaml`           | Full SHA                                                                  |
| `garretpatten/security-guardrails/.github/workflows/security-guardrails.yaml` | `v1` (own-action exception, see [action-pinning.md](./action-pinning.md)) |
| `garretpatten/codeowners-enforcer`                                            | Full SHA                                                                  |

- `quality-checks` v2 covers: Prettier, Markdownlint, Yamllint, ESLint,
  Ruff, ShellCheck, Hadolint, jq, Taplo, and actionlint, with StyLua, typos,
  editorconfig, shfmt, and golangci-lint for matching content. Tools
  auto-enable from the PR diff; there are no `*_run` toggles and no secrets.
- `security-guardrails` covers: OpenGrep SAST, supply-chain scanning
  (Trivy/Dependency Review/SBOM), TruffleHog verified secrets.
- `codeowners-enforcer` fails the check when a changed file lacks an owner.

## Tool selection (auto-detected from the PR diff)

The `detect` job classifies changed files, and each tool job runs only when
matching content changed. No configuration is needed:

| Repository content                          | Tool                             |
| ------------------------------------------- | -------------------------------- |
| Shell scripts (`*.sh`)                      | ShellCheck                       |
| JavaScript/TypeScript (`*.js`, `*.ts`, ...) | ESLint                           |
| Python (`*.py`)                             | Ruff                             |
| TOML (`*.toml`)                             | Taplo                            |
| Dockerfiles                                 | Hadolint                         |
| JSON (`*.json`)                             | jq                               |
| Markdown / YAML (docs-only repos included)  | Markdownlint, Prettier, Yamllint |
| GitHub Actions workflows (`.yaml`)          | actionlint                       |

## Required status checks

The `CI` ruleset's `required_status_checks` contexts are the aggregate
summaries, not per-tool job names:

- `Scans / OpenGrep - SAST`
- `Scans / Supply Chain`
- `Scans / TruffleHog - Secrets`
- `Linters / checks`
- `Enforce Code Owners`

Repositories that call the accessibility workflow additionally require
`Accessibility Audits / checks`. The summary jobs always run and fail when
any applicable tool job failed or the run was cancelled, so one context
covers every enabled tool.

## Change discipline

When linter content changes in a repository, update the `quality-checks`
workflow pin and the `CI` ruleset contexts in the same pull request so
checks never drift from protection.
