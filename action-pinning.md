# Standard: Action pinning

1. All **third-party** actions used in `uses:` must be pinned to a
   **full-length commit SHA** (40 hex characters) of a **released version**.
2. A comment naming the version tag must appear above the `uses:` line:

   ```yaml
   # actions/checkout v7.0.1
   uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1
   ```

3. **Floating tags** (`@v1`, `@main`, `@master`) are forbidden for
   third-party actions.
4. **Exception for own actions:** actions and reusable workflows owned by
   `garretpatten` may use a tag or `@master` **only if the pin is explicitly
   documented here or in the consuming repository**. Full-length SHA pins are
   still preferred for own actions when practical.
5. Action pins are updated **manually** (Dependabot cannot open PRs; see
   [dependabot.md](./dependabot.md)). When bumping a pin, update the version
   comment in the same line pair.

## Documented own-action exceptions

| Reference                                                                        | Pin  | Justification                                                     |
| -------------------------------------------------------------------------------- | ---- | ----------------------------------------------------------------- |
| `garretpatten/security-guardrails/.github/workflows/security-guardrails.yaml@v1` | `v1` | Maintainer-tracked release tag of a first-party reusable workflow |
| `garretpatten/codeowners-enforcer@<sha>`                                         | SHA  | (no exception — pinned by full SHA)                               |
| `garretpatten/quality-checks/.github/workflows/quality-checks.yaml@<sha>`        | SHA  | (no exception — pinned by full SHA)                               |

## Audit checks

```bash
gh api repos/OWNER/REPO/git/trees/HEAD?recursive=1 --jq \
  '.tree[] | select(.path | startswith(".github/workflows/")) | .path'
# then for each file:
gh api repos/OWNER/REPO/contents/PATH --jq '.content' | base64 -d |
  grep -oE 'uses: [^ ]+'
```

For each `uses:` referencing an action not owned by `garretpatten`, the ref
must match `^[0-9a-f]{40}$` (full SHA) and the preceding comment line must
name the version.
