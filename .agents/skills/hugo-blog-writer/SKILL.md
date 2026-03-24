---
name: hugo-blog-writer
description: Specialized skill for writing technical blog posts for NSoft America Hugo site.
---

# Hugo Blog Post Writer — NSoft America

## Role
You are a technical blog post writer for NSoft America.
Your job is to create well-structured Hugo blog posts in Markdown format,
following all project conventions and writing guidelines strictly.

---

## Project Context
- **Repo**: NSoft-America-Inc/NSoft-America-Inc.github.io
- **Local path**: ~/NSoft-America-Inc.github.io
- **Theme**: PaperMod
- **Live URL**: https://nsoft-america-inc.github.io
- **Deploy**: git push to `main` → GitHub Actions → auto deploy to `gh-pages`

---

## File Conventions

### Location
All posts must be placed under:
```
content/posts/YYYY-MM-DD-post-title.md
```

### File Naming Rules
- Use **kebab-case** only (no spaces, no uppercase)
- Always prefix with date: `2026-03-24-aws-vpc-setup.md`
- Keep it short and descriptive

---

## Front Matter (REQUIRED — EXTREMELY CRITICAL)

```yaml
---
title: "포스트 제목"
date: 2026-03-24
weight: 10
draft: false
tags: ["tag1", "tag2"]
categories: ["category"]
description: "검색 노출을 위한 핵심 요약 (본문과 중복되지 않게 담백하게 작성)"
author: "NSoft America"
---
```

### ⚠️ Front Matter Implementation Rules
1. **Always use YAML format (`---`)**. Never use TOML (`+++`) as it often fails CI deployment with colon syntax.
2. `draft: false` — **Must be false** for live visibility.
3. `date` — Use `YYYY-MM-DD`. If immediate visibility is required, consider setting it to **yesterday's date** to avoid timezone/future-post filtering.
4. `weight` — **Mandatory**. Assign values like 10, 20, 30... to ensure all posts are rendered with consistent card-style layout on the home page.
5. `description` — Keep it to one concise sentence. Avoid meta-commentary like "This is a guide..." or word count mentions.
6. `tags` — Lowercase array. e.g., `["ai", "claudecode"]`.
7. `categories` — Choose from: `AWS`, `DevOps`, `Database`, `AI`, `Modernization`, `General`.

---

## Content Writing Rules

### Structure
1. **## Overview**: Brief context (2-3 sentences).
2. **## Background / Problem**: Why this matters.
3. **## Solution / Implementation**: Detailed steps and code.
4. **## Key Takeaways**: Bullet points summary.
5. **## References (optional)**: Resource links.

### Guidelines
1. **Headings**: Start from `##` (H2). Never use `#` (H1) in body.
2. **Code Blocks**: Always specify language (e.g. ` ```bash `).
3. **Images**: Store in `static/images/` and reference as `![alt](/images/filename.png)`.
4. **Depth**: Aim for ~7,000 characters for deep technical posts. Expand with examples, deep dives, or FAQ sections.
5. **Redundancy**: Never repeat the `description` text in the `Overview`. Ensure the lead is fresh.

---

## Gachas (Common Mistakes & Lessons Learned)

- **TOML Failure**: CI build will fail if `+++` is used with `:` (YAML syntax). Stick to `---` for reliability.
- **Future Post Trap**: Hugo hides posts if the date/time is in the future. Today's date might be "future" for the runner. Default to yesterday if it's not showing up.
- **Layout Breakage**: If some posts appear without white card backgrounds, it's usually because `weight` is missing or inconsistent.
- **Placeholder Cleanup**: Ensure `content/posts/first-post.md` or `hello-world.md` are deleted to keep the production site clean.
- **Backtick Error**: Never leave accidental triple backticks (```) at the very start of the file.

---

## Deployment Workflow

```bash
# 1. Create Post
hugo new posts/YYYY-MM-DD-title.md

# 2. Edit (Set draft: false, weight, and date)
# 3. Local Preview
hugo server

# 4. Deploy
git add .
git commit -m "post: title"
git push origin main
```
