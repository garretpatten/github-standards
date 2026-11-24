# Standard: Secret hygiene

1. **Secret scanning** must be enabled on the repository (see
   [security-policy.md](./security-policy.md)).
2. **Push protection** must be enabled where available so secret-containing
   commits are rejected at push time.
3. **TruffleHog verified-secret scan** remains required on every pull
   request via `security-guardrails`.
4. **No secrets in code, workflows, or history.** If a secret is ever found:
   - rotate/revoke it immediately;
   - purge it from history only after rotation;
   - record the incident in the repository's `SECURITY.md` follow-ups.

5. **Prefer OIDC / short-lived tokens** over long-lived secrets. For
   third-party automation prefer GitHub Apps or OIDC federation instead of
   personal access tokens stored in repositories.

## Known false positives

Exclusions live in `.truffleignore` in the repository. Each exclusion must
have a justification comment in the file. An exclusion without justification
is an audit finding.

## Audit checks

```bash
gh api repos/OWNER/REPO --jq '.security_and_analysis.secret_scanning.status, .security_and_analysis.secret_scanning_push_protection.status'
gh api repos/OWNER/REPO/contents/.truffleignore --jq '.content' | base64 -d
```
