# Session 000 — مهندسی سیستم‌ها: از کاربر تا سازنده

**Stage:** 0 · Mental Models  
**سطح:** Foundation → Engineering  
**زمان پیشنهادی:** 90–150 دقیقه، در چند نوبت  
**پیش‌نیاز:** ندارد  
**هدف:** ساخت مدل ذهنی برای فهم، تحلیل، ساخت، آزمایش و نگهداری سیستم‌های نرم‌افزاری  
**ایمنی:** GREEN؛ مطالعه و آزمایش‌های این جلسه بدون تغییر تنظیمات سیستم هستند.

---

## نقشه یادگیری

این دوره قرار نیست تو را به مصرف‌کننده دستورها تبدیل کند. هدف، توانایی مستقل در مواجهه با یک مسئله ناشناخته است:

> مسئله را تعریف کنم؛ سیستم را به اجزای قابل فهم تقسیم کنم؛ رفتار واقعی را مشاهده کنم؛ فرضیه بسازم؛ کم‌خطر آزمایش کنم؛ نتیجه را بررسی کنم؛ و در صورت نیاز، راه‌حل را به محصول قابل نگهداری تبدیل کنم.

این جلسه پایه تمام جلسات بعدی است. اگر مدل‌های این جلسه روشن نباشند، یادگیری Linux، Networking، Security، Programming و AI Engineering به مجموعه‌ای از اصطلاحات پراکنده تبدیل می‌شود.

## 1. سیستم چیست؟

**System** مجموعه‌ای از اجزا است که از طریق روابط و تعامل‌ها، رفتاری را تولید می‌کنند.

برای توصیف یک سیستم، پنج چیز را مشخص کن:

1. **Boundary — مرز:** چه چیز داخل سیستم است و چه چیز خارج آن؟
2. **Components — اجزا:** چه بخش‌هایی وجود دارند؟
3. **Interfaces — رابط‌ها:** اجزا از چه راهی با یکدیگر تعامل می‌کنند؟
4. **Inputs/Outputs — ورودی و خروجی:** چه چیزی وارد می‌شود و چه چیزی تولید می‌شود؟
5. **State — وضعیت:** چه اطلاعاتی رفتار آینده را تغییر می‌دهد؟

### مثال: باز کردن یک وب‌سایت

Boundary را می‌توان در چند سطح تعریف کرد:

- سطح Browser: مرورگر، Cache، DNS resolver سیستم، Network stack.
- سطح Client–Server: دستگاه کاربر، شبکه، سرور و سرویس.
- سطح Application: Frontend، API، Database، Authentication.

مرز را برای سؤال موردنظر تعیین می‌کنیم. اگر مشکل در مرورگر است، لازم نیست ابتدا کل دیتاسنتر را تحلیل کنیم. اگر مشکل در API است، بررسی CSS ممکن است بی‌ربط باشد.

### تمرین مدل سیستم

برای یک سیستم انتخابی، این جدول را تکمیل کن:

| سؤال | پاسخ |
|---|---|
| هدف سیستم چیست؟ | |
| مرز آن کجاست؟ | |
| اجزای اصلی چیست؟ | |
| ورودی‌ها چیست؟ | |
| خروجی‌ها چیست؟ | |
| چه Stateهایی دارد؟ | |
| چه چیزی خارج از کنترل سیستم است؟ | |

---

## 2. مدل، واقعیت و Abstraction

ما نمی‌توانیم یک سیستم پیچیده را با تمام جزئیات در ذهن نگه داریم. بنابراین از **Abstraction (انتزاع)** استفاده می‌کنیم: جزئیات غیرضروری برای سؤال فعلی را کنار می‌گذاریم و ویژگی‌های مهم را حفظ می‌کنیم.

مدل، خود سیستم نیست؛ نمایش ساده‌شده‌ای از سیستم است.

### مثال: شبکه

برای سؤال «چرا سایت باز نمی‌شود؟» مدل اولیه می‌تواند چنین باشد:

`Browser → DNS → Route → Connection → TLS → HTTP → Application`

این مدل مفید است، اما کامل نیست. ممکن است:
- DNS از Cache پاسخ بگیرد؛
- Proxy یا VPN در مسیر باشد؛
- اتصال قبلاً برقرار شده و Reuse شود؛
- HTTP/3 از QUIC استفاده کند؛
- CDN درخواست را پاسخ دهد؛
- درخواست به چند سرویس داخلی منتقل شود.

**قاعده مهندسی:** مدل ساده را برای شروع استفاده کن، اما هر زمان شواهد با مدل ناسازگار شد، مدل را اصلاح کن. مدل را به حقیقت مطلق تبدیل نکن.

### انواع مدل

- **Conceptual model:** توضیح ساده ذهنی؛ مثلاً «IP مثل آدرس».
- **Structural model:** اجزا و ارتباطات؛ مثلاً دیاگرام Component.
- **Behavioral model:** رفتار در طول زمان؛ مثلاً State machine.
- **Data model:** ساختار و رابطه داده‌ها.
- **Operational model:** چه فرآیندها، دستورات و رخدادهایی هنگام اجرا دیده می‌شوند.

---

## 3. State — وضعیت؛ مفهوم بسیار مهم

**State** اطلاعاتی است که در لحظه‌ای از زمان نگهداری می‌شود و می‌تواند رفتار آینده را تحت تأثیر قرار دهد.

مثال‌ها:
- وضعیت اتصال: disconnected / connected
- وضعیت کاربر: anonymous / authenticated
- وضعیت سفارش: pending / paid / shipped
- وضعیت سرویس: stopped / starting / running / failed
- وضعیت Firewall: مجموعه Ruleهای فعال
- وضعیت برنامه: مقادیر متغیرها، فایل‌ها، Database و Cache

### State و Event

- **State:** وضعیت فعلی.
- **Event:** رخداد که در سیستم اتفاق می‌افتد.
- **Transition:** تغییر State در اثر Event و قواعد سیستم.

مثال سفارش:

`Created → PaymentConfirmed → Packed → Shipped → Delivered`

این فقط یک مسیر نیست. سیستم باید حالت‌های خطا را هم مدل کند: PaymentFailed، Cancelled، RefundPending و غیره.

### چرا State اهمیت دارد؟

بسیاری از خطاها زمانی رخ می‌دهند که:
- State مورد انتظار با State واقعی متفاوت است؛
- State در دو بخش سیستم ناسازگار شده است؛
- Event دوبار پردازش می‌شود؛
- یک Transition غیرمجاز اتفاق می‌افتد؛
- State پس از Crash به شکل ناقص باقی می‌ماند.

**State را با Output اشتباه نگیر.** دو سیستم ممکن است اکنون Output یکسانی بدهند، اما State متفاوتی داشته باشند و در Event بعدی رفتار متفاوتی نشان دهند.

---

## 4. Data Flow و Control Flow

دو سؤال را جدا کن:

### Data Flow — داده از کجا به کجا می‌رود؟

مثلاً:
`User Input → Validation → API → Database → API Response → UI`

### Control Flow — چه چیزی تعیین می‌کند قدم بعدی چیست؟

مثلاً:
- اگر ورودی معتبر است، ادامه بده.
- اگر کاربر Authenticated است، داده را نمایش بده.
- اگر پرداخت موفق است، سفارش را تأیید کن.
- اگر Timeout شد، Retry یا Fail کن.

در سیستم واقعی، Data Flow و Control Flow در هم تعامل دارند، اما هنگام تحلیل بهتر است جداگانه رسم شوند.

---

## 5. Interface و Contract

اجزا باید بتوانند بدون دانستن تمام جزئیات داخلی یکدیگر، تعامل کنند.

**Interface** راه تعامل را تعریف می‌کند.  
**Contract** رفتار و تعهدات مورد انتظار را مشخص می‌کند.

مثال API:
- Endpoint: `POST /orders`
- ورودی: JSON با شناسه محصول و تعداد
- خروجی: Order ID یا خطای مشخص
- قواعد: تعداد مثبت، محصول موجود، کاربر مجاز

Contract باید شامل این موارد باشد:
- Input schema
- Output schema
- Preconditions
- Postconditions
- Error behavior
- Authentication/Authorization requirements
- Versioning/compatibility expectations

### چرا Interface مهم است؟

اگر یک component را بتوان با component دیگری جایگزین کرد، بدون آنکه مصرف‌کنندگان به جز contract وابسته باشند، معماری انعطاف بیشتری دارد.

اما Interface زیاد نیز هزینه دارد. Abstraction بی‌مورد می‌تواند فهم و نگهداری را سخت کند. **Interface را برای مرزهای واقعی و تغییرپذیر ایجاد کن، نه برای هر تابع کوچک.**

---

## 6. Failure و Reliability

سیستم‌ها همیشه طبق انتظار کار نمی‌کنند. خطا را باید به‌عنوان بخشی از طراحی در نظر گرفت، نه اتفاقی استثنایی.

### Failure در مقابل Fault و Error

Terminology در منابع متفاوت است، اما یک تفکیک عملی:

- **Fault:** علت یا نقص؛ مثلاً تنظیم DNS اشتباه.
- **Error:** وضعیت داخلی نادرست؛ مثلاً برنامه IP اشتباه دریافت کرده.
- **Failure:** رفتار قابل مشاهده که نیاز کاربر را برآورده نمی‌کند؛ مثلاً سایت باز نمی‌شود.

### Failure domains

یک سیستم را به Failure domainهای مستقل‌تر تقسیم کن:
- Device
- Operating System
- Local Network
- ISP/Internet path
- DNS
- Remote Service
- Application
- Database
- Identity/Authentication
- Dependency provider

هدف این نیست که با دیدن خطا حدس بزنیم؛ هدف محدودکردن حوزه‌ای است که باید شواهد جمع کنیم.

### Fault isolation

وقتی یک سیستم مشکل دارد:
1. علائم دقیق را ثبت کن.
2. محدوده اثر را تعیین کن.
3. یک Boundary انتخاب کن.
4. در آن مرز مشاهده انجام بده.
5. فرضیه را با یک آزمایش رد یا تأیید کن.

---

## 7. Observability — مشاهده‌پذیری

سه منبع کلاسیک مشاهده:

- **Logs:** رخدادهای متنی یا ساختاریافته.
- **Metrics:** اندازه‌گیری عددی در طول زمان؛ مانند CPU، latency، error rate.
- **Traces:** مسیر یک درخواست بین اجزا.

در آزمایش‌های محلی، خروجی Command، Status، فایل Log، Process list و Network socket نیز Evidence هستند.

### Evidence vs Assumption

| نوع | مثال |
|---|---|
| Evidence | دستور `ss` یک Listener روی Port 8080 نشان می‌دهد. |
| Assumption | احتمالاً برنامه روی این Port کار می‌کند. |
| Hypothesis | اگر درخواست HTTP به این Port برسد، برنامه باید پاسخ دهد. |
| Test | درخواست محلی به Endpoint ارسال می‌کنیم. |
| Result | پاسخ HTTP 200 یا Error ثبت می‌شود. |
| Conclusion | نتیجه فقط فرضیه را در محدوده آزمایش پشتیبانی یا رد می‌کند. |

Evidence باید قابل بازبینی باشد. «به نظرم سرور خراب است» Evidence نیست.

---

## 8. تفکر مهندسی: از علامت به علت

فرض کن «وب‌سایت کار نمی‌کند».

### مسیر ضعیف

- چند دستور تصادفی اجرا کن.
- چند تنظیم را تغییر بده.
- Restart کن.
- امیدوار باش.

### مسیر مهندسی

**Observe → Define → Decompose → Hypothesize → Test → Verify → Record**

1. **Observe:** دقیقاً چه اتفاقی می‌افتد؟
2. **Define:** مشکل را به جمله‌ای قابل آزمون تبدیل کن.
3. **Decompose:** سیستم را به لایه‌ها تقسیم کن.
4. **Hypothesize:** یک علت احتمالی و قابل رد شدن بنویس.
5. **Test:** یک آزمایش کم‌خطر طراحی کن.
6. **Verify:** نتیجه واقعی را بررسی کن.
7. **Record:** چه آموختیم؟ چه چیز هنوز Unknown است؟

### مثال فرضیه خوب

«نام دامنه در Resolver سیستم حل نمی‌شود؛ بنابراین درخواست قبل از ایجاد اتصال به مقصد شکست می‌خورد.»

این فرضیه قابل آزمایش است.

### مثال فرضیه ضعیف

«اینترنت خراب است.»

بیش از حد مبهم است و آزمایش مشخصی ندارد.

---

## 9. Risk و Engineering Trade-offs

راه‌حل مهندسی همیشه بهترین راه از نظر یک معیار نیست. باید Trade-offها را آشکار کنیم.

معیارهای رایج:
- Complexity
- Cost
- Performance
- Reliability
- Security
- Maintainability
- Time-to-market
- Reversibility

### Risk

Risk را به‌صورت ساده از دو مؤلفه تحلیل کن:

`Risk ≈ Probability × Impact`

این فرمول تخمین دقیق نیست؛ یک ابزار اولویت‌بندی است.

| اقدام | احتمال مشکل | اثر احتمالی | برگشت‌پذیری |
|---|---|---|---|
| مشاهده Log | کم | کم | بالا |
| تغییر یک Config در محیط آزمایش | متوسط | محدود | معمولاً بالا |
| تغییر Firewall سرور Production | متغیر | ممکن است قطع دسترسی شود | گاهی پایین |
| حذف Database | کم/متغیر | بسیار بالا | پایین، مگر Backup معتبر |

**اصل:** ابتدا مشاهده و اقدامات برگشت‌پذیر. سپس تغییر. قبل از تغییر حساس، Backup و Rollback plan.

---

## 10. Lifecycle ساخت نرم‌افزار

`Problem → Discover → Specify → Design → Plan → Build → Test → Deploy → Operate → Learn`

### Problem
چه نیاز واقعی وجود دارد؟ کاربر چه درد یا هزینه‌ای دارد؟

### Discover
واقعیت، محدودیت، کاربران، داده، محیط، ابزارها و Unknownها را کشف کن.

### Specify
Behavior مورد انتظار را روشن کن. مثال و Acceptance Criteria تعریف کن.

### Design
Architecture و Data Flow و Interfaces را متناسب با اندازه پروژه طراحی کن.

### Plan
کار را به Taskهای کوچک، مستقل و قابل تأیید تقسیم کن.

### Build
کمترین راه‌حل قابل استفاده را بساز.

### Test
با Evidence بررسی کن که Behavior مورد انتظار برقرار است.

### Deploy
راه‌حل را به محیط استفاده منتقل کن.

### Operate
Monitoring، Backup، Security، Incident و Maintenance.

### Learn
از خطا، استفاده واقعی و Feedback، سیستم و روش را اصلاح کن.

MVP به معنی «بی‌کیفیت» نیست؛ یعنی کوچک‌ترین محصولی که یک فرض مهم را در شرایط واقعی بررسی می‌کند. MVP باید Scope محدود داشته باشد، اما برای Scope خودش قابل تست و قابل استفاده باشد.

---

## 11. استفاده درست از AI در مهندسی

AI را به عنوان:
- Research assistant
- Explainer
- Pair programmer
- Test generator
- Reviewer
- Debugging partner
- Documentation assistant

استفاده کن؛ اما خروجی آن را Evidence فرض نکن.

### قرارداد کاری با AI

برای Taskهای مهم:
1. Context و هدف را مشخص کن.
2. محدودیت‌ها را بیان کن.
3. خروجی قابل تست بخواه.
4. قبل از تغییر، Plan و Files affected را بررسی کن.
5. تغییرات را کوچک نگه دار.
6. Test و Verification بخواه.
7. Diff را Review کن.
8. Secretها را وارد Prompt، Repo یا Log نکن.
9. برای اقدام حساس، تأیید انسانی لازم است.
10. ادعای «Done» را با تست واقعی بررسی کن.

### Prompt specification

به جای «این برنامه را درست کن»، بگو:
- Problem
- Expected behavior
- Current behavior
- Environment
- Constraints
- Acceptance criteria
- Files/components in scope
- Out of scope
- Verification steps

---

## 12. تمرین عملی — سیستم را تحلیل کن

یک سیستم واقعی انتخاب کن: Browser، Telegram، یک وب‌سایت، اپ موبایل، دستگاه IoT یا سرویس تولیدی.

بدون نیاز به تغییر سیستم، یک System Brief بنویس:

```text
System:
Goal:
Boundary:
Main components:
Interfaces:
Inputs:
Outputs:
Important state:
Data flow:
Control flow:
Dependencies:
Failure modes:
Evidence I can collect:
One testable hypothesis:
```

### Challenge (فقط این Challenge؛ پاسخ را در کارت‌های مرور هم خواهیم دید)

**سناریو:** اپلیکیشن تو نشان می‌دهد سفارش «پرداخت‌شده» است، اما انبار آن را «پرداخت‌نشده» می‌بیند.

قبل از پیشنهاد راه‌حل:
1. سه Component احتمالی را نام ببر.
2. Stateهای متناقض را مشخص کن.
3. دو Hypothesis متفاوت بنویس.
4. برای هر Hypothesis یک Evidence یا آزمایش کم‌خطر پیشنهاد بده.
5. توضیح بده چرا Restart کردن همه سرویس‌ها اولین اقدام مهندسی مناسبی نیست.

---

## 13. خروجی جلسه

این Artifact را در یادداشت‌های خود نگه دار:

- System Brief
- یک دیاگرام ساده
- یک Hypothesis + Test
- سه اصطلاح که اکنون می‌توانی توضیح بدهی
- سه اصطلاح هنوز مبهم
- نتیجه Teach-back

## 14. Teach-back

بدون نگاه‌کردن به متن، با زبان خودت توضیح بده:

- System و Boundary چیست؟
- State با Event چه تفاوتی دارد؟
- Abstraction چه فایده و چه خطری دارد؟
- Evidence با Assumption چه تفاوتی دارد؟
- چرا یک Hypothesis باید قابل رد شدن باشد؟
- Engineering با «اجرای دستور» چه تفاوتی دارد؟

**این جلسه را فقط پس از انجام Challenge و Teach-back تکمیل کن.** تسلط واقعی را با شواهد ثبت می‌کنیم، نه با تیک مطالعه.

## References

- [NIST SP 800-160 — Systems Security Engineering](https://csrc.nist.gov/pubs/sp/800/160/v1/r1/final)
- [Google SRE Book — Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/)
- [NASA Systems Engineering Handbook](https://www.nasa.gov/reference/systems-engineering-handbook/)
- [The Twelve-Factor App](https://12factor.net/)
