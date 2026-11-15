# Standard: Signed commits

All commits merged into protected branches (the default branch) must be
**cryptographically signed**. The `CI` ruleset enforces this via
`required_signatures`, so unsigned commits cannot merge even with approvals.

## Signing setup (Garret Patten)

- Method: **GPG**.
- Key: RSA 4096, key ID `64E5152761C10A3B`
  (fingerprint `D085C6D1071B1F49D30C38BB64E5152761C10A3B`), uid
  `Garret Patten <garret.patten@proton.me>`.
- The GPG key must be uploaded to the GitHub account (Settings → SSH and GPG
  keys) and the committing email must be verified by the key.
- Local git configuration:

```bash
git config --global user.name 'Garret Patten'
git config --global user.email 'garret.patten@proton.me'
git config --global commit.gpgsign true
git config --global user.signingkey 64E5152761C10A3B
```

## Agents

Agents (Opencode, Grok, Grok Bots) committing to any repository must produce
signed commits using the configured key on the local machine. If signing is
unavailable, the agent must stop and ask instead of committing unsigned
work.

## Audit checks

```bash
gh api repos/OWNER/REPO/commits/SHA |
  jq '[.commit.verification.verified, .commit.verification.reason]'
```

Every commit reachable from the default branch (excluding merge commits made
by the GitHub web UI) must show `verified: true`.
