# Standard: Security policy

Every repository must have a `SECURITY.md` file at the repository root or at
`.github/SECURITY.md`.

## Required content outline

`SECURITY.md` must contain, at minimum:

1. **Supported versions** — for personal projects, a statement that security
   fixes ship on the default branch (`master`) only.
2. **Private reporting instructions** — private vulnerability reporting is
   preferred (GitHub Security Advisories → "Report a vulnerability"), with a
   fallback email: `garret.patten@proton.me`.
3. **Explicit statement** that security vulnerabilities must **not** be opened
   as public GitHub issues.
4. **Response expectation** — reports are acknowledged within **7 days**.
5. A short "out of scope" section (social engineering, physical access,
   theoretical attacks without a plausible path).

## Repository settings that must be enabled

| Setting                         | Requirement                                         |
| ------------------------------- | --------------------------------------------------- |
| Dependabot alerts               | Enabled                                             |
| Dependency graph                | Enabled                                             |
| Secret scanning                 | Enabled                                             |
| Push protection                 | Enabled (where available)                           |
| Private vulnerability reporting | Enabled (where available)                           |
| Dependabot security updates     | **Disabled** (see [dependabot.md](./dependabot.md)) |
| Dependabot version updates      | **Disabled** (see [dependabot.md](./dependabot.md)) |

Public repositories get these features for free. Private repositories on free
plans may not support some settings (see
[exceptions.md](./exceptions.md) for `license-verifier`); where a setting is
unavailable, document it and rely on manual checks.

## Audit checks

1. `SECURITY.md` exists at root or `.github/`.
2. The file mentions the fallback email and prohibits public issues.
3. Settings verification (owner/admin token required):

```bash
gh api repos/OWNER/REPO/vulnerability-reporting \
  --jq '.vulnerability_report'
gh api repos/OWNER/REPO --jq '{dep_alerts: .security_and_analysis.dependabot_alerts.status, secret_scanning: .security_and_analysis.secret_scanning.status, push_protection: .security_and_analysis.secret_scanning_push_protection.status, dep_graph: .security_and_analysis.dependency_graph.status}'
```
