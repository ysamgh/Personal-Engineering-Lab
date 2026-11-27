# Engineering Mastery Roadmap

هدف: ایجاد توانایی مستقل در تحلیل، ساخت، ایمن‌سازی، Deploy و نگهداری نرم‌افزار با AI.

## مسیر و خروجی‌های قابل مشاهده

| Stage | حوزه | مفاهیم | خروجی/آزمایش |
|---|---|---|---|
| 0 | Mental models | Systems, abstraction, state, data flow, failure | رسم نقشه یک سیستم |
| 1 | Computer fundamentals | CPU, RAM, storage, process, kernel, syscall | مشاهده Process و منابع |
| 2 | Internet & networking | Ethernet/Wi-Fi, IP, subnet, routing, DNS, TCP/UDP, ports, sockets | دنبال‌کردن درخواست HTTP |
| 3 | Linux | Filesystem, users, permissions, processes, systemd, logs, shell | Linux diagnostic notebook |
| 4 | Security foundations | Threat model, attack surface, least privilege, patching, SSH, firewall, secrets | Server hardening checklist در محیط آزمایش |
| 5 | Programming | Python, control flow, functions, data structures, exceptions, files, JSON | CLI utility |
| 6 | Engineering workflow | Git, branches, commits, review, specs, issue decomposition | Repository و PR workflow |
| 7 | Web | HTML/CSS/JS, client-server, HTTP, REST, cookies, auth | Web app ساده |
| 8 | Data | SQL, relational model, indexes, transactions, migrations | CRUD app با PostgreSQL |
| 9 | Architecture | Modular design, boundaries, state, queues, caching, trade-offs | Architecture decision records |
| 10 | Containers | Images, containers, namespaces, mounts, networks, Compose | سرویس چندکانتینری |
| 11 | Delivery | CI, testing pyramid, deploy, rollback, observability, backup | Deploy و restore آزمایشی |
| 12 | AI engineering | Context, specs, tool use, evals, code review, agent boundaries | AI-assisted MVP pipeline |

## روش عمق

هر Stage در سه لایه تدریس می‌شود:
1. **Conceptual:** چرا و چگونه کار می‌کند؟ مدل و محدودیت.
2. **Operational:** ابزار، دستور، مشاهده و تشخیص.
3. **Engineering:** تصمیم طراحی، ریسک، trade-off و کاربرد در پروژه.

## Project Ladder

1. Network observation labs (بدون تغییر شبکه)
2. Linux diagnostics notebook
3. Python CLI utility with tests
4. HTTP/API client
5. Small web application
6. Database-backed application
7. Docker Compose deployment
8. Hardened test server + monitoring + backup/restore
9. AI-assisted MVP delivery from a real problem statement

## Gate Criteria

هر مرحله با «خروجی» تمام می‌شود؛ صرفاً تمام‌کردن متن کافی نیست. Gate شامل:
- 5 سؤال recall بدون کمک
- یک آزمایش عملی
- یک troubleshooting scenario
- یک توضیح 3-5 دقیقه‌ای از مدل ذهنی
- یک artifact (note, diagram, script, test, configuration) در repo

## برنامه زمانی

12 هفته پیشنهادی با انعطاف. ترتیب مهم‌تر از سرعت است. اگر یک مفهوم مبهم است، توقف و آزمایش می‌کنیم؛ اگر مفهوم پایه‌ای روشن است، از تمرین به پروژه منتقل می‌شویم. مسیر را براساس عملکرد واقعی تنظیم می‌کنیم.
