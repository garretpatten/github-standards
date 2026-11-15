# Exceptions

Any repository exempt from the standards program must be listed here with
justification and a review date. New exceptions are added by signed pull
request.

| Repository         | Scope of exception                                                                                                                                         | Justification                                                   | Review date |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- | ----------- |
| `m11s`             | **Fully excluded** from the standards program. No audits, no rulesets, no community-file requirements.                                                     | Personal/experimental repository outside the standards program. | 2027-11-15  |
| `license-verifier` | Private repository on a free plan: no rulesets available; Dependabot security-updates setting cannot persist. All other standards apply; fixes are manual. | Free-plan private repositories do not support these features.   | 2027-11-15  |

Future exceptions must include:

1. Repository name.
2. Which standards are relaxed and why.
3. A review date (annual at minimum).

For free-plan private repositories, apply the workarounds:

- Branch protection: enforce equivalent discipline manually (never push to
  the default branch; review before merge) and keep `SECURITY.md`,
  `CONTRIBUTING.md`, `CODEOWNERS`, `LICENSE`, `dependabot.yaml` in place.
- Dependabot: run `gh api repos/OWNER/REPO/vulnerability-alerts -X PUT` to
  enable alerts when the API allows it, and check alerts manually.
