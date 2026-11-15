# Agent instructions

This is `garretpatten/github-standards` — the **canonical source of truth** for
GitHub repository standards. Agents (Opencode, Grok, Grok Bots) must read the
standards files in this repository before auditing or modifying any
`garretpatten` repository, and must apply them here as well.

## Standards files

| Standard             | File                                                   |
| -------------------- | ------------------------------------------------------ |
| License              | [license.md](./license.md)                             |
| Security policy      | [security-policy.md](./security-policy.md)             |
| Contributing         | [contributing.md](./contributing.md)                   |
| Community files      | [community-files.md](./community-files.md)             |
| CODEOWNERS           | [codeowners.md](./codeowners.md)                       |
| Branch protection    | [branch-protection.md](./branch-protection.md)         |
| Signed commits       | [signed-commits.md](./signed-commits.md)               |
| Dependabot           | [dependabot.md](./dependabot.md)                       |
| Dependency hygiene   | [dependency-hygiene.md](./dependency-hygiene.md)       |
| Secret hygiene       | [secret-hygiene.md](./secret-hygiene.md)               |
| Workflow permissions | [workflows-permissions.md](./workflows-permissions.md) |
| Action pinning       | [action-pinning.md](./action-pinning.md)               |
| Repository metadata  | [repository-metadata.md](./repository-metadata.md)     |
| CI and quality       | [ci-and-quality.md](./ci-and-quality.md)               |
| Exceptions           | [exceptions.md](./exceptions.md)                       |
| Verification         | [verification.md](./verification.md)                   |

Changes to standards go through a signed pull request into this repository.

## Before you finish

Do **not** consider work complete until local checks pass. Run from the
repository root:

```bash
npm install
npm run lint
```

Or run the same checks step by step:

```bash
npx prettier --check .
npx markdownlint-cli2 "**/*.md" "#node_modules"
yamllint .github .yamllint .markdownlint.yaml
actionlint
```

**Fix failures instead of ignoring them** when practical:

- Prettier: `npx prettier --write .` (or only the paths you changed), then re-run
  `npx prettier --check .`.
- Markdown/YAML: correct the files per the rule messages, then re-run until
  clean.

| If you edited              | Also run                                                         |
| -------------------------- | ---------------------------------------------------------------- |
| `.github/workflows/*.yaml` | `yamllint .github .yamllint .markdownlint.yaml` and `actionlint` |
| Any `*.md`                 | `markdownlint-cli2` on those paths or `npm run lint:markdown`    |
| YAML or workflow files     | `npm run lint:yaml`                                              |

Install missing tools as needed:

| Tool       | Example install           |
| ---------- | ------------------------- |
| actionlint | `brew install actionlint` |
| yamllint   | `pip install yamllint`    |

CI **Quality Checks** runs PR-scoped linters via `garretpatten/quality-checks`.
CI **Security Guardrails** runs OpenGrep, TruffleHog, and supply-chain scans.
CI **Codeowners Enforcer** requires every changed file to have an owner in
`.github/CODEOWNERS` (or an entry in `.github/.codeownersignore`).

## Commits

Only commit when the user asks. Do not commit secrets. **All commits must be
signed with GPG** (see [signed-commits.md](./signed-commits.md)); if signing is
unavailable, stop and ask instead of committing unsigned work.

## GitHub Actions

All third-party action pins in this repository's workflows must stay on
full-length commit SHAs of released releases, with the version in a comment
above the `uses:` line. First-party reusable workflows follow
[action-pinning.md](./action-pinning.md).

## Layout

| Path                               | Role                                              |
| ---------------------------------- | ------------------------------------------------- |
| `*.md` (root)                      | The standards documents (canonical source)        |
| `.prettierrc`, `.prettierignore`   | Formatting (80-char YAML via override)            |
| `.markdownlint.yaml`               | Markdown lint rules (`MD013` off; Prettier wraps) |
| `.yamllint`                        | YAML lint (80-char lines, `document-start` off)   |
| `.truffleignore`, `.trivyignore`   | Scanner exclusions for security workflows         |
| `.github/CODEOWNERS`               | Required ownership for changed files              |
| `.github/workflows/`               | Quality, security, and CODEOWNERS enforcement     |
| `.github/ISSUE_TEMPLATE/`          | Bug report and feature request issue forms        |
| `.github/pull_request_template.md` | PR checklist and change summary                   |
| `.github/dependabot.yaml`          | Dependency update automation (PRs disabled)       |
| `package.json`                     | Prettier and Markdownlint dev dependencies        |
