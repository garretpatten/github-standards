# Standard: Community files

GitHub's Community Standards / Security & quality overview lists a set of
recommended files. Every repository must contain the **required** files below.

## File requirements

| File                               | Status       | Standard                                                                          |
| ---------------------------------- | ------------ | --------------------------------------------------------------------------------- |
| `README.md`                        | **Required** | Overview, links, purpose; branding per [readme-branding.md](./readme-branding.md) |
| `LICENSE` (MIT)                    | **Required** | [license.md](./license.md)                                                        |
| `SECURITY.md`                      | **Required** | [security-policy.md](./security-policy.md)                                        |
| `CONTRIBUTING.md`                  | **Required** | [contributing.md](./contributing.md)                                              |
| `.github/CODEOWNERS`               | **Required** | [codeowners.md](./codeowners.md)                                                  |
| `.github/ISSUE_TEMPLATE/`          | Recommended  | Bug report and feature request forms                                              |
| `.github/pull_request_template.md` | Recommended  | PR checklist                                                                      |
| `CODE_OF_CONDUCT.md`               | Optional     | Present in the template; keep when present                                        |
| `.github/dependabot.yaml`          | **Required** | [dependabot.md](./dependabot.md)                                                  |
| `.github/.codeownersignore`        | Optional     | [codeowners.md](./codeowners.md)                                                  |

`CODE_OF_CONDUCT.md` is optional for solo personal repositories. If present,
document that choice in the file. The template ships the Contributor Covenant;
consumers keep it unless deliberately removing it.

## Repository description and topics

- Description: non-empty and accurate (see
  [repository-metadata.md](./repository-metadata.md)).
- Topics: at least one relevant topic.
- Homepage: set when the project has one.

## Admins accept content reports

The "Admins accept content reports" setting (Repository → Settings → Admin
options) must be enabled. There is no REST API for this setting as of
2026-11-15; the audit skill flags it as **manual verification**.

## Audit checks

```bash
gh api repos/OWNER/REPO/contents --jq '.[].name' | grep -iE '^(LICENSE|SECURITY.md|CONTRIBUTING.md|README.md)$'
gh api repos/OWNER/REPO --jq '{description: .description, topics: .topics, homepage: .homepage}'
```
