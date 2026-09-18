# gstack — نسخه فارسی

ترجمه و ویراستاری فارسی README پروژه gstack، با حفظ نام فرمان‌ها، کدها، مسیرها و اصطلاحات فنی.

> gstack یک لایه Workflow متن‌باز برای Claude Code است که یک Agent عمومی را به تیمی از نقش‌های تخصصی تبدیل می‌کند: برنامه‌ریزی محصول، معماری، طراحی، بازبینی کد، QA، امنیت و انتشار.

## درباره gstack

gstack پاسخ Garry Tan به شیوه جدید توسعه نرم‌افزار با Agentهای هوش مصنوعی است. ایده اصلی پروژه این است که به‌جای یک Prompt عمومی، برای هر مرحله از چرخه توسعه یک نقش و Workflow مشخص داشته باشیم.

gstack رایگان و دارای مجوز MIT است و از Markdown و فرمان‌های Slash استفاده می‌کند.

**برای چه کسانی؟**
- بنیان‌گذاران و مدیران عامل، به‌ویژه افراد فنی
- کاربران تازه‌کار Claude Code
- Tech Leadها و Staff Engineerها

## شروع سریع

1. gstack را نصب کنید.
2. `/office-hours` را برای توضیح ایده اجرا کنید.
3. `/plan-ceo-review` را برای بازبینی محصول اجرا کنید.
4. `/review` را روی Branch دارای تغییر اجرا کنید.
5. `/qa` را روی URL محیط Staging اجرا کنید.

## نصب

پیش‌نیازهای اصلی: Claude Code، Git، Bun نسخه 1.0 یا بالاتر و Node.js در Windows.

در macOS، استفاده از Aside پیشنهاد شده است. اگر Aside در دسترس نباشد، gstack از Browser داخلی خود استفاده می‌کند.

نصب پایه:

    git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
    cd ~/.claude/skills/gstack
    ./setup

برای حالت تیمی:

    cd ~/.claude/skills/gstack && ./setup --team
    ~/.claude/skills/gstack/bin/gstack-team-init required

در صورت تمایل می‌توانید required را به optional تغییر دهید.

## OpenClaw

OpenClaw می‌تواند Sessionهای Claude Code را از طریق ACP اجرا کند و از Skillهای gstack استفاده کند.

نمونه کاربردها:
- ممیزی امنیتی: `/cso`
- بازبینی کد: `/review`
- تست یک URL: `/qa https://...`
- ساخت یک قابلیت از ابتدا تا انتشار: `/autoplan` سپس پیاده‌سازی و `/ship`

Skillهای بومی OpenClaw:

    clawhub install gstack-openclaw-office-hours gstack-openclaw-ceo-review gstack-openclaw-investigate gstack-openclaw-retro

## سایر Agentها

gstack فقط برای Claude نیست و می‌تواند برای Hostهای مختلف نصب شود:

| Agent | Flag |
|---|---|
| OpenAI Codex CLI | `--host codex` |
| OpenCode | `--host opencode` |
| Cursor | `--host cursor` |
| Factory Droid | `--host factory` |
| Kiro | `--host kiro` |
| Slate | `--host slate` |
| OpenClaw | `--host openclaw` |
| Hermes | `--host hermes` |
| GBrain | `--host gbrain` |

## چرخه Sprint

gstack یک فرایند است، نه مجموعه‌ای از ابزارهای جدا:

**فکر کردن → برنامه‌ریزی → ساخت → بازبینی → تست → انتشار → بازنگری**

هر مرحله خروجی مرحله بعد را تغذیه می‌کند.

| Skill | نقش | کاربرد |
|---|---|---|
| `/office-hours` | YC Office Hours | بازتعریف مسئله و ایجاد سند طراحی |
| `/plan-ceo-review` | CEO / Founder | بازنگری محصول و Scope |
| `/plan-eng-review` | Eng Manager | معماری، جریان داده، Edge Case و تست |
| `/plan-design-review` | Senior Designer | بازبینی طراحی و تشخیص AI Slop |
| `/plan-devex-review` | DX Lead | بررسی تجربه توسعه‌دهنده |
| `/design-consultation` | Design Partner | ساخت Design System و Mockup |
| `/review` | Staff Engineer | یافتن باگ‌های Production |
| `/investigate` | Debugger | یافتن علت ریشه‌ای خطا |
| `/design-review` | Designer Who Codes | Audit و اصلاح طراحی |
| `/devex-review` | DX Tester | تست تجربه واقعی Developer |
| `/design-shotgun` | Design Explorer | تولید و مقایسه Variantهای طراحی |
| `/design-html` | Design Engineer | تبدیل Mockup به HTML/CSS آماده انتشار |
| `/qa` | QA Lead | تست، اصلاح و Regression Test |
| `/qa-only` | QA Reporter | گزارش QA بدون تغییر کد |
| `/pair-agent` | Multi-Agent Coordinator | هماهنگی Agentهای مختلف از طریق Browser |
| `/cso` | Chief Security Officer | ممیزی امنیتی |
| `/ship` | Release Engineer | تست، Coverage، Push و ایجاد PR |
| `/land-and-deploy` | Release Engineer | Merge، Deploy و بررسی Production |
| `/canary` | SRE | پایش پس از Deploy |
| `/benchmark` | Performance Engineer | بررسی Performance و Core Web Vitals |
| `/document-release` | Technical Writer | به‌روزرسانی مستندات |
| `/document-generate` | Documentation Author | تولید مستندات جدید |
| `/retro` | Eng Manager | Retrospective هفتگی |
| `/browse` | QA Engineer | کنترل Browser و تعامل با وب |
| `/scrape` | Data Extractor | استخراج داده از صفحات وب |
| `/setup-browser-cookies` | Session Manager | واردکردن Cookieهای Browser واقعی |
| `/autoplan` | Review Pipeline | اجرای خودکار CEO → Design → DX → Eng |
| `/learn` | Memory | مدیریت یادگیری‌های پروژه |
| `/make-pdf` | Publisher | تبدیل Markdown به سند PDF/HTML/DOCX |
| `/diagram` | Diagram Maker | تولید Mermaid، Excalidraw و SVG/PNG |

## ابزارهای قدرتمند

- `/codex` — دریافت نظر دوم از OpenAI Codex
- `/claude-code` — دریافت نظر دوم از Claude Code
- `/careful` — هشدار پیش از فرمان‌های مخرب
- `/freeze` — محدودکردن ویرایش به یک Directory
- `/guard` — فعال‌سازی هم‌زمان Careful و Freeze
- `/open-gstack-browser` — اجرای GStack Browser نمایان
- `/setup-deploy` — تنظیمات اولیه Deploy
- `/setup-gbrain` — راه‌اندازی GBrain
- `/sync-gbrain` — همگام‌سازی کد با GBrain
- `/gstack-upgrade` — ارتقای gstack
- `/ios-qa` — QA روی iPhone واقعی

## Browser و QA

در macOS، gstack ابتدا از Aside استفاده می‌کند. این روش امکان استفاده از Sessionهای واقعی، Cookieها و Loginهای موجود را فراهم می‌کند.

اگر Aside وجود نداشته باشد، Browser داخلی gstack به‌صورت خودکار استفاده می‌شود.

`/open-gstack-browser` مرورگر GStack Browser را با Sidebar، قابلیت‌های ضد Bot و مسیریابی مدل اجرا می‌کند.

### دفاع در برابر Prompt Injection

gstack هنگام خواندن صفحات وب از دفاع چندلایه استفاده می‌کند؛ از جمله Data Marking، حذف عناصر مخفی، پاک‌سازی ARIA، URL Blocklist و یک Classifier محلی برای تشخیص محتوای مشکوک.

Kill Switch اضطراری:

    GSTACK_SECURITY_OFF=1

### Browser handoff

اگر Agent به CAPTCHA، MFA یا Auth Wall برسد:

    $B handoff

پس از حل مشکل توسط کاربر:

    $B resume

Agent از همان نقطه ادامه می‌دهد.

## Sprintهای موازی

[Conductor](https://conductor.build) امکان اجرای چند Session Claude Code را در Workspaceهای جداگانه فراهم می‌کند. ساختار gstack کمک می‌کند هر Agent بداند چه کاری انجام دهد و چه زمانی متوقف شود.

### ورودی صوتی

Skillها با عبارت‌های طبیعی مانند اجرای بررسی امنیتی، تست وب‌سایت یا بازبینی مهندسی قابل فعال‌سازی هستند و نیازی به حفظ نام همه فرمان‌ها نیست.

## GBrain

[GBrain](https://github.com/garrytan/gbrain) یک پایگاه دانش پایدار برای Agentهای AI است.

راه‌اندازی:

    /setup-gbrain

مسیرهای اصلی:
- Supabase با URL موجود
- Provision خودکار Supabase
- PGLite محلی
- Remote gbrain MCP

ثبت به‌عنوان MCP Server برای Claude Code:

    claude mcp add gbrain -- gbrain serve

همگام‌سازی:

    /sync-gbrain

هر Repo می‌تواند یکی از این سطوح اعتماد را داشته باشد:
- `read-write`
- `read-only`
- `deny`

## حریم خصوصی و Telemetry

Telemetry در gstack **اختیاری و به‌صورت پیش‌فرض خاموش** است.

در صورت فعال‌سازی، اطلاعاتی مانند نام Skill، مدت اجرا، نتیجه، نسخه gstack و OS ارسال می‌شود. کد، مسیر فایل، نام Repo، Branch، Prompt و محتوای کاربر ارسال نمی‌شوند.

خاموش‌کردن Telemetry:

    gstack-config set telemetry off

gstack برای ارسال‌های خارج از ماشین Receipt ایجاد می‌کند و ابزار `gstack-egress` امکان Audit این ارسال‌ها را فراهم می‌کند.

## حذف نصب

روش اصلی:

    ~/.claude/skills/gstack/bin/gstack-uninstall

برای حذف دستی نیز می‌توان gstack و وضعیت محلی آن را حذف کرد:

    rm -rf ~/.claude/skills/gstack
    rm -rf ~/.gstack

پس از حذف دستی، Hookهای gstack را نیز از `~/.claude/settings.json` حذف کنید.

در پروژه‌هایی که gstack به CLAUDE.md اضافه شده است، بخش‌های مربوط به gstack و Skill routing را نیز حذف کنید.

## رفع مشکلات

**Skill نمایش داده نمی‌شود؟**

    cd ~/.claude/skills/gstack && ./setup

**Browser جایگزین لازم است؟**

    GSTACK_SKIP_ASIDE=1

**Browser داخلی مشکل دارد؟**

    cd ~/.claude/skills/gstack && bun install && bun run build

**نصب قدیمی است؟**

    /gstack-upgrade

**فرمان کوتاه می‌خواهید؟**

    cd ~/.claude/skills/gstack && ./setup --no-prefix

**فرمان Namespaceدار می‌خواهید؟**

    cd ~/.claude/skills/gstack && ./setup --prefix

## مستندات

| سند | موضوع |
|---|---|
| [Skill Deep Dives](docs/skills.md) | فلسفه و Workflow همه Skillها |
| [Diagrams & Document Formats](docs/howto-diagrams-and-formats.md) | Mermaid، Excalidraw و قالب اسناد |
| [Builder Ethos](ETHOS.md) | فلسفه Builder |
| [Using GBrain with GStack](USING_GBRAIN_WITH_GSTACK.md) | راهنمای GBrain |
| [GBrain Sync](docs/gbrain-sync.md) | همگام‌سازی حافظه |
| [Architecture](ARCHITECTURE.md) | معماری و جزئیات داخلی |
| [Browser](BROWSER.md) | معماری Browser |
| [Contributing](CONTRIBUTING.md) | توسعه و مشارکت |
| [CHANGELOG](CHANGELOG.md) | تغییرات نسخه‌ها |

## مجوز

MIT — رایگان و متن‌باز.

---

این فایل نسخه فارسی و ویراستاری‌شده README پروژه [garrytan/gstack](https://github.com/garrytan/gstack) است. نام فرمان‌ها، مسیرها، کدها، متغیرهای محیطی و شناسه‌های فنی عمداً ترجمه نشده‌اند تا قابل استفاده و اجرا باقی بمانند.
