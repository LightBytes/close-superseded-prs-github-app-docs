---
title: Close Superseded PRs GitHub App
---

# Close Superseded PRs GitHub App

Close superseded pull requests automatically after the replacement PR merges.
No per-repo workflow required.

## Install

Install the app for your organization or account, then configure defaults on
the setup page that appears after installation.

> [!NOTE]
> GitHub only applies `closes` keywords when a PR is merged into the default branch.
> The app follows the same rule by acting only on merged PRs.

## Mark a PR as superseding

Add a line to the replacement PR description:

```
Supersedes #123
```

Supported patterns (configurable) include:

- `supersedes #123`
- `superseded by #123`
- `supersedes: #123`
- `supercedes#123`
- `replaces #123`

## Configuration

You can configure defaults at install time and override them per repo.

### Per-repo overrides

Create `.close-superceded.yml` or `.close-superceded.yaml` in the default
branch of the repo to override installation defaults.

Example:

```yaml
superseded_label: superseded
superseded_labels:
  - superseded
  - duplicate
keywords:
  - supersedes
  - superseded
  - supercedes
  - superceded
  - replaces
  - replaced
mark_duplicate_comment: true
ignored_branches:
  - dependabot/**
  - release/*
comment_template_path: .github/close-superseded-comment.md
comment_template: |
  ## Superseded PR

  This PR was superseded by #{{new_pr_number}}.
comment_footer: ""
```

### Supported keys

- `superseded_label`: label to apply (empty disables).
- `superseded_labels`: list of labels to apply (empty disables).
- `keywords`: list of keywords to match (defaults above).
- `mark_duplicate_comment`: adds `Duplicate of #<new PR>` comment.
- `ignored_branches`: branch patterns to ignore (base or head).
- `comment_template_path`: path in the repo to a template file.
- `comment_template`: inline template content.
- `comment_footer`: footer appended to the comment.

## Comment templates

If you provide `comment_template_path` or `comment_template`, the following
placeholders are available:

- `{{new_pr_number}}`
- `{{new_pr_title}}`
- `{{new_pr_url}}`
- `{{superseded_pr_number}}`

## Troubleshooting

- Only merged PRs trigger the app.
- The app reads `.close-superceded.yml` from the default branch.
- Ensure the app is installed on the repo and has access.
