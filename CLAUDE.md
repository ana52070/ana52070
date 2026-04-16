# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **GitHub profile repository** (`ana52070/ana52070`) — a special repo whose `README.md` is displayed on the user's GitHub profile page. The owner is a CS student (Tianjin University of Technology) focused on embedded systems, ROS2, and LLM-on-hardware.

There are no build steps, tests, or lint commands. The "code" here is Markdown and YAML workflows.

## File Structure

```
ana52070/
├── README.md                      # GitHub profile display (main file)
├── SETUP.md                       # Deployment guide (not displayed on profile)
└── .github/workflows/             # Must be placed here on the actual GitHub repo
    ├── snake.yml                  # Generates contribution snake SVGs → pushed to `output` branch
    └── blog-post-workflow.yml     # Fetches RSS from chuiyu.wiki → updates README
```

> Note: The workflow files are currently at repo root (`snake.yml`, `blog-post-workflow.yml`). They must be moved to `.github/workflows/` on GitHub to function.

## GitHub Actions Workflows

### snake.yml
- Runs daily at UTC 00:00 (Beijing 08:00) and on every push to `main`
- Uses `Platane/snk/svg-only@v3` to generate two SVGs (light + dark theme)
- Pushes output to the `output` branch via `crazy-max/ghaction-github-pages@v4`
- Requires repo **Settings → Actions → General → Workflow permissions** set to "Read and write permissions"

### blog-post-workflow.yml
- Runs daily at UTC 01:00 (Beijing 09:00)
- Uses `gautamkrishnar/blog-post-workflow@v1` to fetch RSS from `https://chuiyu.wiki/feed.xml`
- Updates the `<!-- BLOG-POST-LIST:START -->` / `<!-- BLOG-POST-LIST:END -->` markers in README.md
- To change the RSS source, edit the `feed_list` field in the workflow file

## README.md Architecture

The README uses HTML + Markdown with external badge/stats services. Key dynamic sections:

| Section | How it updates |
|---|---|
| Contribution snake (`~/contribution-graph`) | `snake.yml` action → SVGs in `output` branch |
| Blog posts (`~/latest-from-wiki`) | `blog-post-workflow.yml` → edits README markers |
| Stats, Streak, Top Languages, Trophy | External APIs (vercel/herokuapp), no action needed |
| Featured project cards (`~/featured-projects`) | GitHub API via `github-readme-stats.vercel.app` |

## Common Edits

- **Add/swap a featured project**: Change `repo=xxx` in the `~/featured-projects` table
- **Update in-progress work**: Edit the `~/now-building` YAML block (plain text, no automation)
- **Change card theme**: Replace `theme=tokyonight` — options: `dark`, `radical`, `dracula`, `nord`, `gruvbox`, `onedark`
- **Fix Streak widget**: Swap `github-readme-streak-stats.herokuapp.com` for `streak-stats.demolab.com` if it's down
- **Remove blog section**: Delete the entire `~/latest-from-wiki` block; other sections are independent
- **Change capsule-render header text**: URL-encode Chinese characters (e.g., `%E5%90%AC%E9%A3%8E%E5%90%B9%E9%9B%A8`)
