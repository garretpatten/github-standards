# Standard: README branding

Every repository's `README.md` is the project face shown on GitHub. It must
look professional: a clean, succinct branding header with status badges,
followed by well-structured content. No marketing fluff, no clutter.

## Header block

The top of `README.md` uses a single HTML block (wrapped in
`<!-- markdownlint-disable MD033 MD041 -->` / `<!-- markdownlint-enable
MD033 MD041 -->`), containing, centered:

1. **Project badge** — one `for-the-badge` style shields.io badge with a
   relevant logo and the project name.
2. **Title** — an `<h1 align="center">` heading.
3. **Tagline** — one bold sentence stating what the project is for.
4. **Description** — one short sentence (optionally two lines) naming the
   platform, scope, and notable constraint.
5. **Metadata badges** — flat-square shields.io badges for license,
   platform/runtime (and language where useful).
6. **CI badges** — one flat-square workflow status badge per enabled CI
   workflow, each linking to the workflow's run history and pinned to the
   default branch.

A horizontal rule (`---`) separates the header block from the body. The body
is plain Markdown.

## Content

Minimum sections, in order, as applicable:

- **Overview** — two or three sentences: what, why, where it fits.
- **Features** — concise bullets.
- **Quick start** — prerequisites and installation steps with fenced code
  blocks.
- Integration/usage sections as relevant.
- **Security** and **License** pointers where a policy exists.

Keep prose tight: full sentences, active voice, no emoji, no badges inside
the body.

## Style rules

- Badges live only in the header block; at most six total.
- Every badge has descriptive `alt` text.
- Badge URLs must resolve (HTTP 200) against the live repository.
- Workflow badge branch parameter matches the default branch.
- The header block is the only raw HTML in the file.

## Applicability

The standard applies to repositories with a user-facing purpose. A
documentation-only repository (for example `github-standards`) may keep a
plain `README.md`; note the choice in the repository when audited.

## Exemplars

- [macOS-setup-scripts](https://github.com/garretpatten/macOS-setup-scripts)
- [google-mcp-server](https://github.com/garretpatten/google-mcp-server)

## Audit checks

```bash
# Fetch the README and confirm the branding block
gh api repos/OWNER/REPO/contents/README.md --jq '.content' | base64 -d |
  head -60

# Badge URLs resolve (sample)
curl -sL -o /dev/null -w '%{http_code}\n' '<BADGE_URL>'

# Workflow badge files exist
gh api repos/OWNER/REPO/actions/workflows --paginate \
  --jq '.workflows[] | .path + " | " + .state'
```

Verify: header block present with project badge, title, tagline, metadata
badges, and one workflow badge per active CI workflow; badge URLs return
200; body uses plain Markdown sections.
