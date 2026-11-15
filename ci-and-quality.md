# Standard: CI and quality

## Required reusable components

Every repository must run the three reusable workflows from
`garretpatten/template`, pinned as shown:

| Component                                                                     | Pin type                                                                  |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `garretpatten/quality-checks/.github/workflows/quality-checks.yaml`           | Full SHA                                                                  |
| `garretpatten/security-guardrails/.github/workflows/security-guardrails.yaml` | `v1` (own-action exception, see [action-pinning.md](./action-pinning.md)) |
| `garretpatten/codeowners-enforcer`                                            | Full SHA                                                                  |

- `quality-checks` covers: Prettier, Markdownlint, Yamllint, ESLint, Ruff,
  ShellCheck, Hadolint, jq, Taplo (plus opt-in StyLua, typos, editorconfig,
  shfmt, golangci-lint).
- `security-guardrails` covers: OpenGrep SAST, supply-chain scanning
  (Trivy/Dependency Review/SBOM), TruffleHog verified secrets.
- `codeowners-enforcer` fails the check when a changed file lacks an owner.

## Linter selection (content-based table)

| Repository content                          | Enable                                             |
| ------------------------------------------- | -------------------------------------------------- |
| Shell scripts (`*.sh`)                      | `shellcheck_run`                                   |
| JavaScript/TypeScript (`*.js`, `*.ts`, ...) | `eslint_run`                                       |
| Python (`*.py`)                             | `ruff_run`                                         |
| TOML (`*.toml`)                             | `taplo_run`                                        |
| Dockerfiles                                 | `hadolint_run`                                     |
| JSON (`*.json`)                             | `jq_run`                                           |
| **Always on** (docs-only repos included)    | `markdownlint_run`, `prettier_run`, `yamllint_run` |

A toggle must be `true` when the repository contains relevant content, and
may be `false` only when it provably does not (check the tree, not vibes).

## Required status checks

The `CI` ruleset's `required_status_checks` contexts must **exactly match**
the enabled workflow job display names. With all toggles enabled, the check
names are:

- `Scans / OpenGrep - SAST`
- `Scans / Supply Chain`
- `Scans / TruffleHog - Secrets`
- `Linters / ESLint - JavaScript/TypeScript Linter`
- `Linters / Hadolint - Dockerfile Linter`
- `Linters / jq - JSON Validator`
- `Linters / Markdownlint - Markdown Linter`
- `Linters / Prettier - Code Formatter`
- `Linters / Ruff - Python Linter`
- `Linters / ShellCheck - Shell Script Linter`
- `Linters / Taplo - TOML Linter`
- `Linters / Yamllint - YAML Linter`
- `Enforce Code Owners`

Linters that skip because no matching files changed still report success, so
keeping all toggles on and all contexts required is the default posture.

## Change discipline

When linter content changes in a repository, update **both** the
`quality-checks.yaml` toggles **and** the `CI` ruleset contexts in the same
pull request so checks never drift from protection.
