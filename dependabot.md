# Standard: Dependabot

## Policy

| Setting                                | Value                                          |
| -------------------------------------- | ---------------------------------------------- |
| Dependabot alerts                      | Enabled                                        |
| Dependabot security updates (auto PRs) | **Disabled**                                   |
| Dependabot version updates (auto PRs)  | **Disabled** via `open-pull-requests-limit: 0` |

Manual dependency updates only. **Never** re-enable automatic Dependabot
pull requests without updating this standards document in the same change.

## Uniform `.github/dependabot.yaml`

Every repository must contain this exact file:

```yaml
version: 2
updates:
  - package-ecosystem: 'github-actions'
    directory: '/'
    schedule:
      interval: 'weekly'
    open-pull-requests-limit: 0
    commit-message:
      prefix: 'build'
      include: 'scope'
```

Notes:

- The schedule stays `weekly` (used for alert evaluation cadence); with
  `open-pull-requests-limit: 0`, no PRs are ever opened automatically.
- Action pins are bumped **manually** to full-length commit SHAs of released
  versions (see [action-pinning.md](./action-pinning.md)).
- Only the `github-actions` ecosystem is declared. Ecosystems with committed
  lockfiles may add their own entry **only** with
  `open-pull-requests-limit: 0`, in the same shape as above.

## Audit checks

```bash
gh api repos/OWNER/REPO/contents/.github/dependabot.yaml --jq '.content' |
  base64 -d
gh api repos/OWNER/REPO --jq '.security_and_analysis.dependabot_security_updates.status'
```

The file must match the canonical YAML above (modulo comments), and the
security-updates status must be `disabled` (free-plan private repositories
that cannot persist this setting are covered by
[exceptions.md](./exceptions.md)).
