# Verification

Exact commands the `github-audit` skill uses to verify each standard. All
commands are read-only.

## General notes

- **Rate limits:** the REST API allows ~5,000 core requests/hour and 100
  search requests/minute. When scanning all repositories, list them once
  (`gh repo list garretpatten --limit 100 --json name,isPrivate,defaultBranchRef`)
  and reuse the result; avoid per-file `contents` calls where a tree call
  suffices.
- **Pagination:** every list endpoint paginates at 30 items by default. Pass
  `--paginate` to `gh api` (or `per_page=100` + cursor loops) for repos,
  rulesets, workflows, and trees.
- **Settings requiring owner tokens:** security & analysis settings depend on
  the token scopes; failures here are "cannot verify", not "non-compliant".
- Free-plan **private** repositories cannot host rulesets; consult
  [exceptions.md](./exceptions.md) before flagging.

## List repositories (starting point)

```bash
gh repo list garretpatten --limit 100 --paginate \
  --json name,isPrivate,visibility,defaultBranchRef,description,repositoryTopics
```

## Branch protection / rulesets (branch-protection.md)

```bash
# One active ruleset named CI, scoped to ~DEFAULT_BRANCH
gh api repos/OWNER/REPO/rulesets --paginate |
  jq '[.[] | select(.enforcement == "active")]'

# Full ruleset detail (rules, conditions, bypass actors)
gh api repos/OWNER/REPO/rulesets/<RULESET_ID> |
  jq '{name, conditions, rules, bypass_actors}'
```

Expect: `rules` contains `deletion`, `non_fast_forward`,
`required_signatures`, `pull_request` (1 review, dismiss stale, code-owner,
last push, threads resolved, `allowed_merge_methods == ["merge"]`),
`required_status_checks` (strict, contexts matching enabled jobs); exactly
one `bypass_actors` entry: `{"actor_id": 5, "actor_type": "RepositoryRole",
"bypass_mode": "always"}`.

## Signed commits (signed-commits.md)

```bash
gh api repos/OWNER/REPO/commits?sha=master --paginate |
  jq '[.[] | select(.parents | length <= 1)
       | {sha: .sha[0:8], verified: .commit.verification.verified}]'
```

All entries must be `verified: true`.

## Dependabot (dependabot.md)

```bash
gh api repos/OWNER/REPO/contents/.github/dependabot.yaml --jq '.content' |
  base64 -d
gh api repos/OWNER/REPO --jq
  '.security_and_analysis.dependabot_security_updates.status'
gh api repos/OWNER/REPO --jq
  '.security_and_analysis.dependabot_alerts.status'
```

## Security settings (security-policy.md, secret-hygiene.md)

```bash
gh api repos/OWNER/REPO --jq '.security_and_analysis'
gh api repos/OWNER/REPO/vulnerability-reporting --jq '.vulnerability_report'
```

## Community files (community-files.md, codeowners.md)

```bash
gh api repos/OWNER/REPO/contents --jq '.[].name'
gh api repos/OWNER/REPO/contents/.github/CODEOWNERS --jq '.content' |
  base64 -d
```

## Workflows: permissions and pins (workflows-permissions.md, action-pinning.md)

```bash
gh api "repos/OWNER/REPO/git/trees/HEAD?recursive=1" --paginate --jq \
  '.tree[] | select(.path | test("^\\.github/workflows/.*\\.(ya?ml)$")) | .path'

# For each workflow file:
gh api repos/OWNER/REPO/contents/<PATH> --jq '.content' | base64 -d |
  grep -nE '^(permissions:|uses:|#)' -A0
```

Every third-party `uses:` ref must match `^[0-9a-f]{40}$`.

## CI contexts vs jobs (ci-and-quality.md)

```bash
# All workflow files and their jobs
gh api repos/OWNER/REPO/actions/workflows --paginate --jq '.workflows[] | .path + " | " + .state'

# Recent check-run names (sampled)
gh api repos/OWNER/REPO/commits/HEAD/check-runs --jq '.check_runs[].name'
```

Cross-check rule: every `check-runs` name that appears on merged PRs must be
present in the ruleset's `required_status_checks` contexts, and vice versa.

## Metadata (repository-metadata.md)

```bash
gh api repos/OWNER/REPO --jq
  '{description, topics, has_issues, has_discussions, has_wiki,
    default_branch, private, homepage}'
```

## README branding (readme-branding.md)

```bash
# Header block: project badge, title, tagline, metadata badges, CI badges
gh api repos/OWNER/REPO/contents/README.md --jq '.content' | base64 -d |
  head -60

# Badge URLs resolve (sample a few)
curl -sL -o /dev/null -w '%{http_code}\n' '<BADGE_URL>'

# One workflow badge per active CI workflow
gh api repos/OWNER/REPO/actions/workflows --paginate \
  --jq '.workflows[] | .path + " | " + .state'
```

Verify: centered branding block (for-the-badge project badge, centered H1,
bold tagline), flat-square metadata badges (license, runtime), one workflow
status badge per enabled CI workflow pinned to the default branch; badge
URLs return 200; body is plain Markdown. Documentation-only repositories
may keep a plain README (see readme-branding.md applicability).

## Pull requests (pull-requests.md)

```bash
# Sample recent PR titles and body sizes
gh api "repos/OWNER/REPO/pulls?state=closed&per_page=20" --paginate \
  --jq '.[] | {number, title, body_len: (.body // "" | length)}'

# Inspect a sampled PR body
gh api repos/OWNER/REPO/pulls/<NUMBER> --jq '.body'
```

Verify: titles are imperative and noise-free; bodies link the related issue
(`Closes #N` when applicable), summarize changes as bullets, state how the
change was verified; no automated footers or signatures.

## Standards-file drift detection

The audit skill should compare this repository's canonical standards files
against local copies where practical:

```bash
gh api repos/garretpatten/github-standards/contents/dependabot.md \
  --jq '.content' | base64 -d > /tmp/canonical-dependabot.md
diff /tmp/canonical-dependabot.md ~/.cache/github-audit/dependabot.md
```

Cache and refresh the canonical set at the start of each audit run so the
audit always reflects the current standards.
