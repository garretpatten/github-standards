# Standard: Branch protection

## Default branch

The canonical default branch is **`master`** (this matches the template and
the rest of the fleet). The specification originally proposed `main`; the
fleet-wide convention `master` was deliberately chosen. The `CI` ruleset is
scoped to `~DEFAULT_BRANCH`, so it follows whatever the default branch is.

Any repository whose default branch is not `master` must be documented in
[exceptions.md](./exceptions.md).

## Ruleset requirements

Exactly **one active ruleset** named `CI`, scoped to `~DEFAULT_BRANCH`
(`ref_name.include: ["~DEFAULT_BRANCH"]`).

Required rules (exact parameters):

| Rule                     | Parameters                                                                                                                                                                                                                        |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deletion`               | blocked (no parameters)                                                                                                                                                                                                           |
| `non_fast_forward`       | blocked (no parameters)                                                                                                                                                                                                           |
| `required_signatures`    | enabled (no parameters)                                                                                                                                                                                                           |
| `pull_request`           | `required_approving_review_count: 1`, `dismiss_stale_reviews_on_push: true`, `require_code_owner_review: true`, `require_last_push_approval: true`, `required_review_thread_resolution: true`, `allowed_merge_methods: ["merge"]` |
| `required_status_checks` | `strict_required_status_checks_policy: true`; contexts are the aggregate summaries (`Linters / checks`, plus `Accessibility Audits / checks` when that workflow is called) and the `Scans / *` and `Enforce Code Owners` contexts |

## Merge method

Merge method: **merge commits only**. Squash merging and rebasing are not
allowed. Revisit deliberately, not casually.

The rule is enforced at **two layers**, and both must agree:

1. **Ruleset layer** — the `CI` ruleset's `pull_request` rule sets
   `allowed_merge_methods: ["merge"]`, governing pull requests that target the
   default branch.
2. **Repository settings layer** — `allow_merge_commit: true`,
   `allow_squash_merge: false`, `allow_rebase_merge: false`. The ruleset only
   reaches pull requests targeting the default branch; the repository settings
   govern the merge buttons everywhere else (for example pull requests between
   topic branches).

## Bypass

Bypass actor: `RepositoryRole` with `actor_id: 5` (repository admin) and
`bypass_mode: always`. No other bypass actors.

## No other active rulesets

No additional branch rulesets may be active. Tag or push rulesets are out of
scope of this standard.

## Private repositories on free plans

Private repositories on free plans (for example `license-verifier`) cannot
have rulesets. Document the exception in
[exceptions.md](./exceptions.md) and rely on branch-level discipline plus
manual review.

## Audit checks

In this repository the active `CI` ruleset is id `27317843`
(<https://github.com/garretpatten/github-standards/rules/27317843>).

```bash
gh api repos/OWNER/REPO/rulesets --paginate |
  --jq '[.[] | select(.enforcement == "active")] | length'
gh api repos/OWNER/REPO/rulesets/RULESET_ID |
  jq '.conditions, .rules, .bypass_actors'
gh api repos/OWNER/REPO --jq '{
  allow_merge_commit, allow_squash_merge, allow_rebase_merge
}'
```

Fetch `RULESET_ID` from `gh api repos/OWNER/REPO/rulesets`. A compliant
repository shows exactly one active ruleset named `CI` with the rules and
bypass actor above, and repository merge settings of
`allow_merge_commit: true`, `allow_squash_merge: false`,
`allow_rebase_merge: false`.
