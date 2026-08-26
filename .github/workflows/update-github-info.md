---
name: update-github-info
description: Refresh Mona's GitHub information page with the latest official updates.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
engine:
  id: copilot
  harness:
    max-retries: 5
    initial-delay-ms: 30000
    backoff-multiplier: 2
    max-delay-ms: 240000
tools:
  edit:
  web-fetch:
  github:
    toolsets: [repos]
network:
  allowed:
    - defaults
    - github.blog
    - github.com
    - awesome-copilot.github.com
safe-outputs:
  create-pull-request:
    title-prefix: "[mona] "
    labels: [automation]
    draft: true
    max: 1
---

# Update GitHub Info

Read `notes/mona-notes.md` using the GitHub repository API tools. Read the existing `site/content/github-info.md` the same way so that the update preserves its structure and existing useful content.

Use the web-fetch tool to read both official sources:

- https://github.blog/latest/
- https://github.blog/changelog/
- https://awesome-copilot.github.com/workflows/

Select the most useful recent items for developers, keeping summaries short and practical. Add useful Awesome Copilot workflows to the sources represented in `site/content/github-info.md`. Attribute every item to the GitHub Blog, GitHub Changelog, or Awesome Copilot workflows and include its source URL. Update `site/content/github-info.md` with the edit tool.

When using the edit tool, pass patch content as raw patch text. The first line must be exactly `*** Begin Patch`; never JSON-wrap the patch, put it in a JSON field, or retry a patch after the tool reports that it is JSON-wrapped. Keep the patch focused on `site/content/github-info.md`.

When the content is ready, use the `create-pull-request` safe output to open a pull request for Mona to review. Explain the sources checked and the key updates in the pull request title and body. Do not write directly to the default branch, merge changes, or use terminal, CLI, or sandboxed commands to read repository guidance or reference files.