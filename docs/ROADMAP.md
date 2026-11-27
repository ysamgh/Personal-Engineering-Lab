# Engineering Mastery Roadmap

هدف: توانایی مستقل در تحلیل، ساخت، ایمن‌سازی، Deploy و نگهداری نرم‌افزار با AI. این نقشه عمداً پیش‌نیازمحور است؛ Stageها را با Evidence و mastery جلو می‌بریم، نه با زمان یا تعداد صفحات.

## Curriculum

| Stage | Domain | Scope | Capstone / evidence |
|---|---|---|---|
| 0 | Engineering mental models | System, boundary, state, abstraction, interfaces, data/control flow, failure, evidence, risk | System brief + failure analysis |
| 1 | Computer architecture & OS | Binary/hex, Boolean logic, combinational/sequential circuits, CPU datapath, ISA, instruction cycle, pipeline, registers, memory hierarchy, cache, virtual memory, storage, boot, kernel/user space, syscall, process/thread | Build a simple CPU model; trace instruction and memory behavior |
| 2 | Internet & networking | Link layer, Ethernet/Wi-Fi, MAC, ARP/NDP, IP, subnet, routing, DNS, TCP/UDP, sockets, TLS, HTTP | Trace a request; network diagnosis |
| 3 | Linux | Filesystem, paths, users/groups, permissions, processes, signals, packages, systemd, logs, shell, pipes, environment | Linux diagnostic notebook |
| 4 | Security foundations | Threat model, attack surface, least privilege, identity, secrets, patching, SSH, firewall, backups, incident response | Hardened disposable lab |
| 5 | Programming foundations | Python syntax, types, control flow, functions, collections, exceptions, modules, files, JSON, CLI | Tested CLI tool |
| 6 | Engineering workflow | Git, branches, commits, specs, decomposition, code review, debugging, testing | Repo with issue/spec/test workflow |
| 7 | Web systems | HTML/CSS/JS, browser, client-server, HTTP, REST, cookies, sessions, auth, CORS | Small web app |
| 8 | Data engineering | Relational model, SQL, constraints, indexes, transactions, migrations, backup/restore | Database-backed app |
| 9 | Software architecture | Modular boundaries, dependency direction, state, queues, caching, consistency, failure patterns, ADRs | Architecture decision + implemented system |
| 10 | Containers & runtime | Linux namespaces, cgroups, image layers, containers, mounts, networks, Compose | Multi-service local stack |
| 11 | Delivery & operations | CI, test strategy, deploy, rollback, observability, backup, restore, incident response | Deploy, monitor, restore exercise |
| 12 | AI engineering | Context engineering, tool use, structured specs, evals, safety, agent workflows, human approval | AI-assisted MVP pipeline |

## Learning method: depth before breadth

For every topic:
1. **First principles:** What problem does this concept solve? Why did it exist?
2. **Model:** Simplify, define boundaries, and state where the model breaks.
3. **Mechanism:** What actually happens under the abstraction?
4. **Observation:** Which tool or evidence can reveal behavior?
5. **Experiment:** Make one controlled, safe test.
6. **Failure analysis:** What can fail? What symptom would each failure create?
7. **Engineering choices:** Trade-offs, constraints, cost, security, maintainability.
8. **Leitner cards:** Only durable, high-value knowledge becomes spaced-repetition cards.
9. **Transfer:** Apply the concept to a new scenario.
10. **Artifact:** Produce a diagram, report, script, test, config, or working project.

## Project ladder

1. System brief + failure analysis.
2. Hardware/OS observation notebook.
3. Network request tracing.
4. Linux diagnostic notebook.
5. Python CLI utility with tests.
6. HTTP/API client.
7. Small web application.
8. Database-backed application.
9. Docker Compose deployment.
10. Hardened disposable server with monitoring.
11. Backup and restore drill.
12. AI-assisted MVP from a real problem statement.

## Mastery gate (not a quiz)

Do **not** use standalone quizzes, end-of-lesson tests, random quizzes, or score chasing. Retrieval practice occurs only through Leitner spaced-repetition cards.

Progress to the next topic is based on evidence:
- Study completed and key concepts explained in teach-back.
- Lab/observation completed where applicable.
- One transfer task or troubleshooting exercise completed.
- Artifact produced.
- Open questions documented.

If evidence is weak, revisit the explanation and lab. The Leitner system schedules recall cards independently and does not replace practical work.

## Pace

A stage may take days or weeks. Deep learning and practical independence are the priorities. We advance one well-defined block at a time, and curriculum content is expanded in sequential commits. Avoid broad, shallow content dumps.


## Stage 1 detailed sequence

| Session | Topic | Core deliverable |
|---|---|---|
| 004 | Data representation | Binary/hex conversion and byte-level inspection |
| 005 | Digital logic and Boolean algebra | Truth tables and gate-level circuit |
| 006 | Combinational and sequential circuits | 4-bit counter design with timing assumptions |
| 007 | CPU architecture | CPU datapath block diagram |

Stage 1 remains incomplete after Session 007. Further content includes instruction execution, pipelining, memory hierarchy, storage and I/O, firmware and boot, interrupts, kernel/user mode, system calls, processes and scheduling.
