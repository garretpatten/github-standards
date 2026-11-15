# Standard: CODEOWNERS

Every repository must have a `.github/CODEOWNERS` file (repository root or
`.github/`).

## Minimum content

```text
* @garretpatten
```

Additional path-specific owner entries are allowed **below** the catch-all
line (later rules take precedence in CODEOWNERS).

## Enforcement

1. The `CI` ruleset must require code-owner review
   (`require_code_owner_review: true`).
2. CI runs the `codeowners-enforcer` reusable workflow
   (`garretpatten/codeowners-enforcer`), which fails the check when a changed
   file has no effective owner.
3. A `.github/.codeownersignore` file may exempt paths from the enforcer
   check. Exemptions must be documented here or in the repository's own
   `AGENTS.md`; an undocumented exemption is an audit finding.

## Known exemptions

- `garretpatten/github-standards`: `.github/.codeownersignore` is absent —
  no exempted paths.

## Audit checks

```bash
gh api repos/OWNER/REPO/contents/.github/CODEOWNERS --jq '.content' |
  base64 -d | head -1
```

Expect the first non-empty line to resolve every path to `@garretpatten`.
