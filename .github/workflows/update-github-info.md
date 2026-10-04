---
name: update-github-info

on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read

engine:
  id: copilot
  args: ["--allow-all-urls"]

tools:
  edit:
  web-fetch:
  bash: [curl]
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
2. Fetch `https://github.blog/latest/`. Use `web_fetch` when it is available; otherwise use `curl --fail --location --silent --show-error` through the bash tool.
3. Fetch `https://github.blog/changelog/`. Use `web_fetch` when it is available; otherwise use `curl --fail --location --silent --show-error` through the bash tool.
4. Fetch Awesome Copilot workflows from `https://awesome-copilot.github.com/workflows/`. Use `web_fetch` when it is available; otherwise use `curl --fail --location --silent --show-error` through the bash tool. Do not return a no-op merely because `web_fetch` is unavailable when the curl fallback is available.
5. Compare the current public information with Mona's notes and the existing content. Preserve relevant material, factual accuracy, source links, and the file's established structure and voice.
6. On every run, update the YAML frontmatter in `site/content/github-info.md` so `last_updated` contains the current UTC date and time in `YYYY-MM-DDTHH:MM:SSZ` format. Add the frontmatter and field if they do not exist.
7. If a meaningful content update is needed, update `site/content/github-info.md` with useful, current information. Do not make speculative changes.
8. If no meaningful content update is needed, create `site/content/archive/YYYY-MM-DD-update-github-info.md` using the current UTC date. Provide a complete, auditable rationale that lists every source consulted with its URL, summarizes the relevant findings from each source, compares those findings with Mona's notes and the existing content, explains each inclusion or exclusion decision, and concludes why the content in `site/content/github-info.md` does not need an update. Do not include hidden chain-of-thought or unrelated analysis.
9. After updating `last_updated` and either updating the content or creating a no-update archive record, use the `create_pull_request` safe-output tool to open a draft pull request for Mona to review. Give it a concise title and summarize the sources consulted, the decision reached, and the files created or changed in the body.