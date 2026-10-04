---
name: update-github-info

on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read

tools:
  edit:
  web-fetch:
  github:
    toolsets: [repos]

network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com

safe-outputs:
  create-pull-request:
    allowed-files:
      - site/content/github-info.md
      - site/content/archive/*.md
    draft: true
    max: 1
---

# Update GitHub Info

Keep `site/content/github-info.md` current for Mona to review.

1. Use GitHub repository API tools to read `notes/mona-notes.md`, `site/content/github-info.md`, and any repository guidance or reference files you need. Do not use terminal, CLI, or sandboxed commands to read repository guidance or reference files.
2. Use the web-fetch tool to fetch `https://github.blog/latest/`.
3. Use the web-fetch tool to fetch `https://github.blog/changelog/`.
4. Use the web-fetch tool to fetch Awesome Copilot workflows from `https://awesome-copilot.github.com/workflows/`.
5. Compare the current public information with Mona's notes and the existing content. Preserve relevant material, factual accuracy, source links, and the file's established structure and voice.
6. If a meaningful update is needed, update only `site/content/github-info.md` with useful, current information. Do not make speculative changes.
7. If no meaningful update is needed, create `site/content/archive/YYYY-MM-DD-update-github-info.md` using the current UTC date. Provide a complete, auditable rationale that lists every source consulted with its URL, summarizes the relevant findings from each source, compares those findings with Mona's notes and the existing content, explains each inclusion or exclusion decision, and concludes why `site/content/github-info.md` does not need an update. Do not include hidden chain-of-thought or unrelated analysis.
8. After creating either an update or a no-update archive record, use the `create_pull_request` safe-output tool to open a draft pull request for Mona to review. Give it a concise title and summarize the sources consulted, the decision reached, and the file created or changed in the body.