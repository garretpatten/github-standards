# Contributing

This repository defines the standards enforced across `garretpatten`
repositories; see [README.md](./README.md). Participants are expected to
follow the [Code of Conduct](./CODE_OF_CONDUCT.md).

## Issues

Security vulnerabilities are **not** tracked in public issues until addressed; see **[SECURITY.md](./SECURITY.md)**.

Use [GitHub Issues](https://github.com/garretpatten/github-standards/issues) with the **Bug report** or **Feature request** form. Include reproduction steps, environment details, and relevant logs (redact secrets and private paths).

## Pull requests

- Branch from **`master`**, focused scope per PR.
- **All commits must be signed** (GPG; see the signing standard in
  [branch-protection.md](./branch-protection.md) and
  [signed-commits.md](./signed-commits.md)).
- A codeowner review is required (`.github/CODEOWNERS` → `@garretpatten`).
- Update **`.github/CODEOWNERS`** when you add paths that need explicit ownership.
- Pin third-party GitHub Actions to full commit SHAs in workflow files; bump pins manually ([action-pinning.md](./action-pinning.md)).
- AI-generated contributions are acceptable provided they meet every other standard: signed commits, passing checks, no secrets, accurate documentation.

### Checks (from repo root)

```bash
npm install
npm run lint
```

Or step by step:

```bash
npx prettier --check .
npx markdownlint-cli2 "**/*.md" "#node_modules"
yamllint .github .yamllint .markdownlint.yaml
actionlint
```

Install **actionlint** and **yamllint** locally if missing (`brew install actionlint`, `pip install yamllint`). **`npm run lint`** mirrors the Prettier, Markdownlint, and Yamllint portions of CI **Quality Checks**.

Documentation-only changes still need **`prettier`** and **`markdownlint`** on touched Markdown files.
