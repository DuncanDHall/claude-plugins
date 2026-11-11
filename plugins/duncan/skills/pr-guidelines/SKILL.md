---
name: pr-guidelines
description: Guidelines for GitHub pull requests and GitLab merge requests. Use when opening a PR or MR, or when writing or editing its description.
---

# PR and MR guidelines

Follow every guideline below when opening a pull request or merge request, or when editing its description.

## Add a `/resume` command for each conversation

End the description with a section that has one `/resume` command for each Claude Code conversation that worked on the PR or MR, oldest first:

```markdown
## Claude Code conversations

- `/resume <first-conversation-id>`
- `/resume <second-conversation-id>`
```

Get the ID of this conversation from the environment:

```sh
echo "$CLAUDE_CODE_SESSION_ID"
```

Never remove a line that an earlier conversation added:

1. Before changing a description, read the current one:
   - GitHub: `gh pr view NUMBER --json body -q .body`
   - GitLab: `glab mr view NUMBER -F json | jq -r .description`
2. Copy every existing `/resume` line into the new description, in the same order.
3. If the list does not have this conversation's ID, add a line for it at the end.
