# Personal Engineering Lab

> یک Apprenticeship عملی و مرحله‌به‌مرحله برای تبدیل‌شدن به Software Systems Builder: فهم عمیق سیستم‌ها، مشاهده، آزمایش، ساخت، امنیت، Deploy و نگهداری با کمک AI.

## Browser Learning App

Static learning app: [Open index.html](index.html). Local run:

```bash
python -m http.server 8000
```

Open `http://localhost:8000`. The app has lesson reader, local notes/progress, and Leitner spaced repetition. Enable GitHub Pages in Settings → Pages → Source: GitHub Actions to publish (workflow: `.github/workflows/pages.yml`).

## Learning principles

- First principles and deep understanding, not memorizing commands.
- Observe actual system behavior and keep evidence separate from assumptions.
- Safe, controlled, reproducible labs.
- Practical challenges and teach-back.
- **No standalone quizzes.** Recall questions exist only as Leitner cards with spaced scheduling.
- Progress by evidence and demonstrated capability, not page count.
- AI accelerates work but its output is reviewed and verified.

## Start here

1. [Session 000 — Engineering mental models](sessions/000-engineering-mental-model.md)
2. [Roadmap](docs/ROADMAP.md)
3. [Learning method](docs/LEARNING-METHOD.md)
4. [Lab safety](docs/LAB-SAFETY.md)
5. [Problem-solving playbook](docs/PROBLEM-SOLVING.md)
6. [Session 001 — URL to HTTP](sessions/001-url-to-http.md)
7. [Session 002 — IP, Routing, Ports and Sockets](sessions/002-ip-routing-ports.md)
8. [Session 003 — Terminal and Shell](sessions/003-terminal-shell.md)
9. [Session 004 — Data Representation](sessions/004-data-representation.md)
10. [Session 005 — Digital Logic](sessions/005-digital-logic.md)
11. [Session 006 — Combinational and Sequential Circuits](sessions/006-combinational-sequential-circuits.md)
12. [Session 007 — CPU Architecture](sessions/007-cpu-architecture.md)
13. [Session 008 — Instruction Cycle](sessions/008-instruction-cycle.md)
14. [Session 009 — Memory Hierarchy](sessions/009-memory-hierarchy.md)
15. [Network troubleshooting](playbooks/network-troubleshooting.md)
16. [Glossary](reference/glossary.md)
17. [Progress tracker](PROGRESS.md)

## Engineering lifecycle

`Problem → Discover → Specify → Design → Plan → Build → Test → Deploy → Operate → Learn`

## Mastery evidence

A topic is not mastered merely because it was read. Evidence may include:
- Explain the concept accurately in your own words.
- Observe the mechanism on a real system.
- Perform a safe lab.
- Diagnose a failure or transfer knowledge to a new scenario.
- Produce an artifact: diagram, notebook, test, script, configuration or project.
