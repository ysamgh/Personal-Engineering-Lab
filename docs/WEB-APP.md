# Browser Learning App

## Version scope

Static browser app, local-first. No account or server-side storage.

## Current features

- Dashboard, lesson list, Markdown lesson reader.
- Session 000 is the entry point.
- Browser-local notes and study completion markers.
- Leitner spaced-repetition review: reveal answer, self-rate remembered/forgotten, automatic box movement and due date.
- Card data is version-controlled in `cards/leitner.json`.
- Scheduling data stays local in browser localStorage.
- Playbooks and dark mode.
- Responsive layout.

## Leitner policy

Initial intervals: 1, 3, 7, 14, 30 days for Boxes 1–5. New cards are immediately due. Successful recall moves one box upward; failed/weak recall resets to Box 1. The app schedules the next due date from the new box. Local time and browser storage are used. This simple algorithm is an MVP; export/sync and richer recall ratings are future work.

No standalone quizzes, random quiz mode or lesson-end question sets. Practical labs and teach-back are separate mastery evidence.

## Architecture

```
Browser
 ├─ index.html / style.css / app.js
 ├─ cards/leitner.json (version-controlled card deck)
 ├─ localStorage
 │   ├─ notes
 │   ├─ completion markers
 │   ├─ Leitner box/due dates
 │   └─ theme
 └─ static same-origin files
     ├─ sessions/*.md
     └─ cards/*.json
```

## Run locally

From the repository root:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`. Opening index.html directly as file:// can prevent fetch requests due to browser security restrictions.

## Limitations

- Notes, progress and review schedule are local to this browser/device; no sync or account.
- Internet is required for hosted content unless deployed static files are locally available.
- Markdown rendering is a lightweight custom parser, not a full CommonMark implementation.
- No backend, login, database or analytics.
- MVP; do not store sensitive notes.
- Static hosting includes all files; cards and lessons are public repository content.

## Deployment

GitHub Pages workflow: `.github/workflows/pages.yml`. Enable Pages in repository Settings → Pages and select GitHub Actions. Repository has been made public by the owner. After Pages configuration, push-triggered workflow should publish; check Actions for success and the environment URL.
