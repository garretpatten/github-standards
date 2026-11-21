# Standard: Pull requests

Every pull request is a reviewable unit of work. Titles and descriptions must
be clean and succinct so reviewers (human or agent) understand the change at
a glance.

## Title

- Imperative summary of the change, ideally ≤ 60 characters:
  `Add Dependabot config`, not `Added some stuff for dependabot`.
- No trailing issue numbers, no `WIP`/`Draft` markers in the title (use
  GitHub's draft state), no promotional noise.
- Conventional prefixes (`feat:`, `fix:`, `chore:`, `docs:`) are accepted
  where the repository already uses them.

## Description

Use the repository's pull request template when present. The description
contains, at minimum:

1. **Related issue** — a link to the issue the PR resolves, with a closing
   keyword (`Closes #N`) when applicable. Omit only when no issue exists and
   say why in one line.
2. **Changes** — bullets describing what changed and why. One bullet per
   coherent change; no walls of text, no restating the diff line by line.
3. **Verification** — how the change was tested (commands run, checks that
   pass). Fold into `Changes` or a dedicated section when the template
   allows.

## Style rules

- Every sentence earns its place: no filler, no marketing language, no
  emoji, no boilerplate ("This PR...").
- Screenshots only for user-visible interface changes; keep them small and
  relevant.
- No automated footers or signatures ("Generated with ...") — the commit and
  PR authorship speaks for itself.
- The checklist items in the PR template are answered honestly; unchecked
  items are explained.

## Audit checks

```bash
# Sample recent PRs
gh api "repos/OWNER/REPO/pulls?state=closed&per_page=10" --paginate \
  --jq '.[] | {number, title, body_len: (.body // "" | length)}'

# Read a PR body
gh api repos/OWNER/REPO/pulls/<NUMBER> --jq '.body'
```

Verify: titles are imperative and free of noise; bodies link the related
issue, summarize changes in bullets, and state verification; no automated
footers.
