# Laboratory Safety and Operational Boundaries

این آزمایشگاه برای یادگیری است. هدف، مشاهده و فهم است؛ نه ایجاد اختلال در سیستم‌های دیگر.

## Rules

1. آزمایش‌های اولیه فقط روی دستگاه خودت، VM، container آموزشی یا سیستم‌هایی که مجوز صریح داری.
2. هیچ scanning، probing، stress testing یا packet capture روی شبکه دیگران بدون permission و scope روشن.
3. دستورات `sudo`، تغییر firewall، SSH config، route، DNS system-wide، partition، disk formatting، user deletion و recursive permission changes نیازمند بررسی جداگانه هستند.
4. هیچ credential، token، private key، password، .env واقعی یا اطلاعات حساس را در repo، chat، screenshot یا log commit نکن.
5. قبل از تغییر config: backup، read current state، plan، verify، rollback plan.
6. روی Production: ابتدا read-only diagnostics؛ تغییر فقط با approval و plan.
7. Script ناشناس را با `curl | bash` یا `wget | sh` اجرا نکن. ابتدا source، provenance و effects را بررسی کن.
8. برای commandهای تمرینی، از test data و disposable environment استفاده کن.
9. یادگیری packet capture: capture محدود به interface و filter مشخص؛ packet payload ممکن است sensitive باشد.
10. اگر در تمرین چیزی را نمی‌فهمی، متوقف شو؛ «اجرا کن ببین چه می‌شود» روش مناسب برای destructive operations نیست.

## Risk labels

- **GREEN:** مشاهده/read-only؛ معمولاً کم‌خطر.
- **YELLOW:** تغییر local محدود؛ فهم و rollback لازم.
- **RED:** privileged/destructive، network-wide یا production-impacting؛ فقط بعد از بررسی، backup و تأیید.

## Read-only examples (still understand the output)

`pwd`, `whoami`, `id`, `uname -a`, `ip addr`, `ip route`, `ss -tulpen`, `df -h`, `free -h`, `ps`, `journalctl` (read mode), `curl -I` to a public test endpoint.

## Safe practice

Prefer:
- VM snapshot before configuration experiments;
- local test services bound to loopback;
- temporary files in a dedicated lab directory;
- explicit target names;
- command timeouts for tests;
- read-only first, change second.
