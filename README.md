# github-standards

This repository is the **canonical source of truth** for all GitHub repository
standards maintained by **Garret Patten**. Every other repository, and every
agent (Opencode, Grok, Grok Bots) operating on those repositories, **must read
and conform to the standards defined here**.

The `github-audit` skill loads and follows the files in this repository as the
authoritative source. `AGENTS.md` (or equivalent) files in other repositories
should contain a short pointer to this repository rather than restating the
standards.

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

## Proposing changes

All changes to these standards require a **pull request with signed commits**
into this repository. See [contributing.md](./contributing.md) for the process.
Dependabot does not open pull requests automatically here; dependency bumps are
performed manually and intentionally.

## Scope

This repository applies every standard it defines to itself. It was created
from [garretpatten/template](https://github.com/garretpatten/template) and
inherits the CI stack (`codeowners-enforcer`, `security-guardrails`,
`quality-checks`), CODEOWNERS, `dependabot.yaml`, and the `CI` ruleset.

**Last verified: 2026-11-15.**
