# Browser App MVP

## Current version

A static, browser-based learning dashboard. No backend or account system.

## Features

- Dashboard with lesson cards and progress summary
- Session reader fetches Markdown from the repository's raw GitHub branch
- Personal notes saved to browser localStorage
- Completion state saved locally
- Playbook view
- Responsive layout and dark theme toggle
- Links back to source content on GitHub

## Architecture

```text
Browser
 ├── index.html
 ├── style.css
 ├── app.js
 ├── localStorage (notes/progress/theme)
 └── Fetch Markdown
      └── raw.githubusercontent.com
           └── GitHub repository sessions/*.md
```

## Limitations

- Progress and notes are browser-local and not synced between devices.
- Content fetching requires internet access.
- Markdown rendering is deliberately lightweight. It is not a full Markdown parser; tables in lesson source are currently displayed as raw Markdown paragraphs.
- No login, database, backend, user analytics or multi-user features.
- This is a learning MVP, not a production security-sensitive application.

## Run locally\n\nBecause browsers restrict file-to-file requests, run a local static HTTP server from the repository root rather than opening the HTML directly:\n\n```bash\npython -m http.server 8000\n```\n\nThen visit `http://localhost:8000`. Lesson files are fetched from same-origin relative paths, so the app works with private repositories when served from a deployment that includes the Markdown files.\n\n## Deployment

This app can be deployed as a static site to Vercel or GitHub Pages. Build is not required; use the repository root as the static output. The GitHub Pages workflow is committed, but Pages must be enabled in repository Settings. Vercel project creation was attempted through the connected integration and denied with HTTP 403; no deployment was created.