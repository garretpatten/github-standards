# Standard: Workflow permissions

1. Every workflow file in `.github/workflows/` must declare an explicit
   **`permissions:` block** (workflow-level, with job-level narrowing where
   a job needs less or more).
2. Default to least privilege: **`contents: read`** unless a job requires
   more.
3. Every elevated permission must be **justified in a comment** directly
   above the permission or job explaining why it is needed.
4. No workflow may rely on the broad default token permissions; if the
   organization/user setting "Default workflow permissions" is currently
   broad, the explicit blocks neutralize it.

## Required pattern

```yaml
permissions:
  contents: read
```

Example of an elevated job with justification:

```yaml
permissions:
  pull-requests: write
```

- `codeowners-enforcer.yaml` uses `pull-requests: write` to post the
  ownership-warning comment; `contents: read` keeps checkout minimal.

## Audit checks

```bash
gh api repos/OWNER/REPO/contents/.github/workflows --jq '.[].name'
gh api repos/OWNER/REPO/contents/.github/workflows/FILE.yaml --jq '.content' |
  base64 -d | grep -n -A4 '^permissions:'
```

Every workflow must show a `permissions:` block before its jobs. For each
permission beyond `contents: read`, an adjacent comment must justify it.
