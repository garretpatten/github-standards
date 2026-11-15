# Standard: Contributing

Every repository must have a `CONTRIBUTING.md` file at the repository root or
at `.github/CONTRIBUTING.md`.

## Required content outline

`CONTRIBUTING.md` must contain, at minimum:

1. **How to propose changes** — fork or branch, open a pull request, and sign
   all commits (see [signed-commits.md](./signed-commits.md)).
2. **Codeowner reviews** — a reference to `.github/CODEOWNERS` and the
   requirement of one approving review, including a codeowner (see
   [codeowners.md](./codeowners.md)).
3. **Coding standards pointer** — lint and format checks come from the
   `quality-checks` reusable workflow (see
   [ci-and-quality.md](./ci-and-quality.md)); run `npm install && npm run
lint` locally before pushing.
4. **AI contributions policy** — AI-generated contributions (Opencode, Grok,
   Grok Bots) are acceptable provided they meet every other standard:
   signed commits, passing checks, no secrets, correct documentation.
5. **Security pointer** — a link to `SECURITY.md`; vulnerabilities are not
   reported in public issues.
6. A link to the Code of Conduct when one is present (see
   [community-files.md](./community-files.md)).

## Optional templates

Issue templates (`.github/ISSUE_TEMPLATE/`) and a pull request template
(`.github/pull_request_template.md`) are recommended. The template repository
ships both (bug report and feature request forms, plus a PR checklist).

## Audit checks

1. `CONTRIBUTING.md` exists at root or `.github/`.
2. The file references signed commits, CODEOWNERS, `SECURITY.md`, and the
   local lint commands.
