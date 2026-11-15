# Standard: Dependency hygiene

1. **Lockfiles must be committed** for every language in use that supports
   them:
   - JavaScript/TypeScript: `package-lock.json`, `yarn.lock`, or
     `pnpm-lock.yaml`
   - Python: `poetry.lock`, `Pipfile.lock`, `requirements.txt` (pinned), or
     `uv.lock`
   - Rust: `Cargo.lock`
   - Go: `go.sum`

2. **No floating ranges** in production dependency declarations where
   avoidable. Prefer exact versions or caret ranges resolved by a committed
   lockfile; never use `latest`.

3. **Supply-chain scanning stays enabled.** The `security-guardrails`
   reusable workflow runs:
   - Trivy vulnerability and license scanning (with Software Composition
     Analysis / SBOM generation)
   - Dependency Review on pull requests
   - CycloneDX SBOM emission where the scan pipeline produces one

4. **License compliance** is audited by the existing Trivy license checks;
   do not disable license scanning in `.trivyignore` except with a documented
   reason in that file.

## Audit checks

```bash
gh api repos/OWNER/REPO --jq '.security_and_analysis.dependency_graph.status'
```

Expect `enabled`. Lockfile presence is a filesystem check on the default
branch; a repository using a language above must show the corresponding
lockfile in `gh api repos/OWNER/REPO/git/trees/HEAD?recursive=1`.
