# Standard: Repository metadata

| Field          | Requirement                                                                |
| -------------- | -------------------------------------------------------------------------- |
| Description    | Non-empty and accurate.                                                    |
| Topics         | At least one relevant topic.                                               |
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

## Audit checks

```bash
gh api repos/OWNER/REPO --jq \
  '{description: .description, topics: .topics, has_issues: .has_issues,
    has_discussions: .has_discussions,
    has_wiki: .has_wiki, default_branch: .default_branch,
    private: .private, homepage: .homepage}'
```

- `description` must be non-empty; `topics` non-empty.
- `default_branch` must be `master`.
- `has_wiki` must be `false` (or documented in exceptions).
- `private` reflects the documented visibility decision.
