---
name: Weekly Report Status
description: Publishes a concise weekly activity report covering commits, issues, and pull requests from the last 7 days.
on:
  schedule: weekly on monday
  workflow_dispatch:
engine: copilot
permissions:
  contents: read
  issues: read
  pull-requests: read
  copilot-requests: write
strict: true
tools:
  github:
    mode: gh-proxy
    toolsets: [default]
safe-outputs:
  create-issue:
    title-prefix: "[weekly-report] "
    max: 1
---

# Weekly Report Status

Generate a concise activity report for this repository covering the **last 7 full days ending at workflow start (UTC)**.

## Steps

1. Determine the report window: `now - 7 days` (UTC) to `now` (UTC).
2. Gather activity within that window:
   - **Commits**: commits pushed to the default branch.
   - **Issues**: issues opened, closed, or commented on.
   - **Pull requests**: pull requests opened, merged, closed, or reviewed.
3. Summarize findings concisely, grouped by category (Commits, Issues, Pull Requests).
4. If there was no activity in a category, state that clearly (e.g., "No commits in the last 7 days.").
5. If there was no activity at all across all categories, state that clearly at the top of the report instead of calling `noop` — always publish a report so the team has a signal of a quiet week.

## Report Format

Use this structure:

### Overview

1-2 sentences summarizing the week's activity level.

### Commits

List of notable commits (author, short SHA, message) or "No commits in the last 7 days."

### Issues

List of opened/closed/notable issues (number, title, state) or "No issue activity in the last 7 days."

### Pull Requests

List of opened/merged/closed pull requests (number, title, state) or "No pull request activity in the last 7 days."

Publish this report as a new issue using the `create-issue` safe output. Keep the report concise and focused on the highlights, not an exhaustive log.
