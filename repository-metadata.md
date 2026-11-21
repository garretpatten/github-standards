# Standard: Repository metadata

| Field          | Requirement                                                                |
| -------------- | -------------------------------------------------------------------------- |
| Description    | Non-empty and accurate.                                                    |
| Topics         | A curated, relevant set (aim for 3+; see below).                           |
| Visibility     | Explicit choice: public or private. Private exceptions must be documented. |
| Default branch | `master` (see [branch-protection.md](./branch-protection.md)).             |
| Issues         | Enabled or disabled with a documented reason.                              |
| Discussions    | Enabled or disabled with a documented reason.                              |
| Wiki           | **Disabled** unless actively used.                                         |
| Homepage       | Set when the project has a canonical URL.                                  |
| Tags/projects  | Optional; keep organized when used.                                        |

Current decisions:

- Issues: enabled on all repositories (template ships issue forms).
- Discussions: disabled (personal projects; issues suffice).
- Wiki: disabled everywhere (documentation lives in Markdown files in-repo).

## Topics

Topics make repositories discoverable in your own profile and to agents.
Curate a **relevant set** per repository:

- Lowercase, hyphen-separated, matching GitHub topic rules.
- Cover what the repository **is** and **uses**: language/runtime
  (`typescript`, `python`, `bash`), domain (`google-calendar`,
  `backup-scripts`, `dotfiles`), key technologies (`mcp`,
  `model-context-protocol`, `github-actions`), and platform when relevant
  (`macos`, `arch-linux`).
- Aim for 3 or more; every topic must be accurate — no aspirational or
  decorative topics.
- Add topics when creating a repository, not later as an afterthought.

## Audit checks

```bash
gh api repos/OWNER/REPO --jq \
  '{description: .description, topics: .topics, has_issues: .has_issues,
    has_discussions: .has_discussions,
    has_wiki: .has_wiki, default_branch: .default_branch,
    private: .private, homepage: .homepage}'
```

- `description` must be non-empty; `topics` must form a curated, relevant
  set (3+ preferred; 1 is a finding, 0 is a violation).
- `default_branch` must be `master`.
- `has_wiki` must be `false` (or documented in exceptions).
- `private` reflects the documented visibility decision.
