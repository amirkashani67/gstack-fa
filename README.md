# gstack — راهنمای فارسی

> این نسخه برای فارسی‌زبان‌هاست: توضیح‌ها فارسی و مفهومی شده‌اند، اما فرمان‌ها، مسیرها، نام فایل‌ها، APIها، متغیرهای محیطی و قطعه‌کدها باید دقیقاً مطابق پروژه اصلی باقی بمانند.

> **منبع فنی:** README رسمی [garrytan/gstack](https://github.com/garrytan/gstack). در صورت اختلاف، نسخه اصلی و فایل‌های خود پروژه مرجع نهایی هستند.

## gstack در یک نگاه

اگر خیلی ساده بگوییم، gstack یک دستیار کدنویسی معمولی نیست؛ یک **روش کار تیمی برای Agent کدنویسی** است. به‌جای اینکه فقط بگویید «این قابلیت را بساز»، کار را بین نقش‌های مختلف تقسیم می‌کند: محصول، معماری، طراحی، Code Review، QA، امنیت و Release.

الگوی اصلی:

`Think → Plan → Build → Review → Test → Ship → Reflect`

مسیر پیشنهادی برای شروع:

`/office-hours` → `/plan-ceo-review` → `/plan-eng-review` → پیاده‌سازی → `/review` → `/qa` → `/ship`

- `/office-hours` مسئله واقعی را قبل از کدنویسی روشن می‌کند.
- `/plan-ceo-review` محصول و Scope را به چالش می‌کشد.
- `/plan-eng-review` معماری، جریان داده، خطاها و تست‌ها را بررسی می‌کند.
- `/review` به‌دنبال Bugهای Production است.
- `/qa` برنامه را در Browser واقعی تست می‌کند.
- `/ship` مراحل نهایی انتشار را انجام می‌دهد.

**نکته:** نام Commandها را ترجمه نکنید؛ همان نام انگلیسی را اجرا کنید.



> ترجمه فارسی README پروژه gstack، بر پایه نسخه اصلی پروژه در [garrytan/gstack](https://github.com/garrytan/gstack). نام فرمان‌ها، کدها، مسیرها، متغیرهای محیطی و شناسه‌های فنی عمداً ترجمه نشده‌اند.

## معرفی

gstack ابزارهای Markdown و slash-commandهای Garry Tan برای تبدیل Claude Code به یک تیم مهندسی مجازی است: نقش‌هایی برای بازاندیشی محصول، معماری، طراحی، بازبینی کد، QA، امنیت و انتشار. پروژه رایگان و تحت مجوز MIT است.

نویسنده توضیح می‌دهد که در ۶۰ روز اخیر ۳ سرویس Production و بیش از ۴۰ قابلیت منتشر کرده و نرخ سالانه ۲۰۲۶ او، بر اساس تغییر منطقی کد، حدود ۸۱۰ برابر سرعت سال ۲۰۱۳ بوده است (۱۱٬۴۱۷ در برابر ۱۴ خط منطقی در روز). این اندازه‌گیری روی ۴۰ مخزن عمومی و خصوصی `garrytan/*` انجام شده است.

**مخاطبان اصلی:**
- بنیان‌گذاران و مدیران عامل، به‌خصوص افراد فنی
- کاربران تازه‌کار Claude Code
- Tech Leadها و Staff Engineerها

## شروع سریع

1. gstack را نصب کنید.
2. `/office-hours` را برای توضیح چیزی که می‌سازید اجرا کنید.
3. `/plan-ceo-review` را روی ایده قابلیت اجرا کنید.
4. `/review` را روی Branch دارای تغییر اجرا کنید.
5. `/qa` را روی URL محیط Staging اجرا کنید.
6. اگر همین‌جا متوقف شوید، خیلی سریع متوجه می‌شوید این Workflow برای شما مناسب است یا نه.

## نصب

**پیش‌نیازها:** Claude Code، Git، Bun v1.0+ و Node.js (فقط Windows).

در macOS، [Aside](https://aside.com) (macOS 15+) به‌عنوان Browser توصیه شده است. Skillهای Browser، `/make-pdf` و `/diagram` ابتدا از Aside استفاده می‌کنند و Sessionهای Loginشده واقعی را حفظ می‌کنند؛ در نبود آن، `./setup` Browser داخلی gstack را می‌سازد.

`/cso` علاوه بر این به Bun دارای چهار Build Flag از نوع `--no-compile-autoload-*` و Toolchain بومی نیاز دارد: C Compiler با قابلیت Static در Linux، Xcode Command Line Tools در macOS یا Visual Studio 2022 Build Tools با Desktop development with C++ در Windows. در صورت نبود پیش‌نیازها، setup بقیه موارد را نصب می‌کند و `/cso` وضعیت `not assessed` را گزارش می‌دهد.

برای Registryهای کندتر می‌توانید `GSTACK_CSO_IMAGE_PULL_TIMEOUT_SECONDS=120` را تنظیم کنید؛ محدوده مجاز ۵ تا ۳۰۰ ثانیه است. Preload کامل حداکثر یک ساعت زمان دارد.

### گام ۱

در Claude Code این را اجرا کنید:

```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
cd ~/.claude/skills/gstack
./setup
```

سپس بخش `gstack` را به CLAUDE.md اضافه کنید و برای Web Browsing استفاده از `/browse` را الزام کنید، ابزارهای `mcp__claude-in-chrome__*` را ممنوع کنید و Skillهای gstack را معرفی کنید.

### گام ۲ — Team Mode

```bash
(cd ~/.claude/skills/gstack && ./setup --team) && ~/.claude/skills/gstack/bin/gstack-team-init required && git add .claude/ CLAUDE.md && git commit -m "require gstack for AI-assisted work"
```

Team Mode فایل Vendored وارد Repo نمی‌کند و هر Claude Code Session یک Auto-update Check سریع دارد. برای حالت غیرالزامی، `required` را به `optional` تغییر دهید.

## OpenClaw

OpenClaw Sessionهای Claude Code را از طریق ACP ایجاد می‌کند. پس از نصب gstack می‌توانید در AGENTS.md مشخص کنید که Sessionهای Claude Code از Skillهای gstack استفاده کنند.

نمونه‌ها:
- ممیزی امنیتی: `Run /cso`
- Code Review: `Run /review`
- QA روی URL: `Run /qa https://...`
- ساخت کامل قابلیت: `Run /autoplan` → پیاده‌سازی → `/ship`
- برنامه‌ریزی: `Run /office-hours` → `/autoplan`

راهنمای کامل: [docs/OPENCLAW.md](docs/OPENCLAW.md)

Skillهای Native OpenClaw:

```
clawhub install gstack-openclaw-office-hours gstack-openclaw-ceo-review gstack-openclaw-investigate gstack-openclaw-retro
```

| Skill | کاربرد |
|---|---|
| `gstack-openclaw-office-hours` | Product interrogation با ۶ سؤال اجباری |
| `gstack-openclaw-ceo-review` | Strategic challenge با ۴ حالت Scope |
| `gstack-openclaw-investigate` | روش Debug علت ریشه‌ای |
| `gstack-openclaw-retro` | Retrospective هفتگی مهندسی |

## سایر AI Agentها

gstack برای ۱۰ AI Coding Agent طراحی شده و Setup آن‌ها را تشخیص می‌دهد:

```bash
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/gstack
cd ~/gstack && ./setup
```

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
| GBrain (mod) | `--host gbrain` |

Reviewهای خارجی به CLI نصب‌شده و Authenticateشده نیاز دارند. فرمان‌های خارجی `/gstack-claude-code` و `/gstack-codex` هستند. `/claude` به `/claude-code` تغییر نام داده است.

برای Agentهایی مانند Zed، Amp و Jules که فقط Rules را می‌خوانند، Digest موجود در [agents-digest/gstack-AGENTS.md](agents-digest/gstack-AGENTS.md) را Copy کنید.

## مثال Workflow

```
You:    I want to build a daily briefing app for my calendar.
You:    /office-hours
Claude: [asks about the pain — specific examples, not hypotheticals]

You:    Multiple Google calendars, events with stale info, wrong locations.
        Prep takes forever and the results aren't good enough...

Claude: [بازتعریف مسئله، استخراج قابلیت‌ها، چالش فرض‌ها و تولید گزینه‌های پیاده‌سازی]

You:    /plan-ceo-review
        [بازبینی Scope و Design Doc]

You:    /plan-eng-review
        [Data Flow، State Machine، Error Path، Test Matrix و Security]

You:    /review
        [AUTO-FIXED] 2 issues. [ASK] Race condition → you approve fix.

You:    /qa https://staging.myapp.com
        [opens real browser, clicks through flows, finds and fixes a bug]

You:    /ship
        Tests: 42 → 51 (+9 new). PR: github.com/you/app/pull/42
```

## چرخه Sprint

**Think → Plan → Build → Review → Test → Ship → Reflect**

هر Skill خروجی مورد نیاز مرحله بعد را فراهم می‌کند.

| Skill | متخصص | کاربرد |
|---|---|---|
| `/office-hours` | YC Office Hours | شش سؤال اجباری برای بازتعریف محصول قبل از کدنویسی |
| `/plan-ceo-review` | CEO / Founder | بازاندیشی مسئله و چهار حالت Scope |
| `/plan-eng-review` | Eng Manager | معماری، Data Flow، Diagram، Edge Case و Test |
| `/plan-design-review` | Senior Designer | امتیازدهی ۰–۱۰ به ابعاد طراحی و تشخیص AI Slop |
| `/plan-devex-review` | Developer Experience Lead | DX Review، TTHW و حالت‌های DX EXPANSION / POLISH / TRIAGE |
| `/design-consultation` | Design Partner | Design System، تحقیق و Mockup و تولید `DESIGN.md` |
| `/review` | Staff Engineer | Bugهای Production و Completeness Gap |
| `/investigate` | Debugger | Root-cause Debugging؛ بدون Investigation هیچ Fixی |
| `/design-review` | Designer Who Codes | Audit و اصلاح طراحی |
| `/devex-review` | DX Tester | Audit زنده Onboarding و تجربه Developer |
| `/design-shotgun` | Design Explorer | تولید ۴–۶ Mockup Variant و Comparison Board |
| `/design-html` | Design Engineer | تبدیل Mockup به HTML/CSS Production با Pretext |
| `/qa` | QA Lead | Test، Fix، Verify و Regression Test |
| `/qa-only` | QA Reporter | گزارش QA بدون تغییر کد |
| `/pair-agent` | Multi-Agent Coordinator | اشتراک Browser با Agentهای دیگر |
| `/cso` | Chief Security Officer | Security Audit و در حالت جامع Runtime/Scanner محدودشده |
| `/ship` | Release Engineer | Sync، Test، Coverage، Push و PR |
| `/land-and-deploy` | Release Engineer | Merge، Deploy و Verify Production |
| `/canary` | SRE | Post-deploy Monitoring |
| `/benchmark` | Performance Engineer | Page Load، Core Web Vitals و Resource Size |
| `/document-release` | Technical Writer | به‌روزرسانی Docs مطابق تغییرات |
| `/document-generate` | Documentation Author | تولید Docs گمشده با Diataxis |
| `/retro` | Eng Manager | Retrospective هفتگی |
| `/browse` | QA Engineer | Browser واقعی و Screenshot |
| `/scrape` | Data Extractor | استخراج داده ساختاریافته از Web |
| `/setup-browser-cookies` | Session Manager | Import Cookieهای Browser واقعی |
| `/autoplan` | Review Pipeline | CEO → Design → DX → Eng |
| `/spec` | Spec Author | تبدیل Intent مبهم به Spec اجرایی در پنج مرحله |
| `/learn` | Memory | مدیریت یادگیری‌های پروژه |
| `/make-pdf` | Publisher | Markdown به سند Publication-quality |
| `/diagram` | Diagram Maker | Mermaid + Excalidraw + SVG/PNG |

### انتخاب Review

| هدف | قبل از کد | بعد از انتشار |
|---|---|---|
| End users | `/plan-design-review` | `/design-review` |
| Developers | `/plan-devex-review` | `/devex-review` |
| Architecture | `/plan-eng-review` | `/review` |
| همه موارد | `/autoplan` | — |

## Power Tools

| Command | کاربرد |
|---|---|
| `/codex` | Second Opinion مستقل از OpenAI Codex CLI |
| `/claude-code` | Second Opinion مستقل از Claude Code |
| `/careful` | هشدار پیش از فرمان‌های مخرب |
| `/freeze` | محدودکردن ویرایش به یک Directory |
| `/guard` | `/careful` + `/freeze` |
| `/unfreeze` | حذف مرز `/freeze` |
| `/open-gstack-browser` | GStack Browser با Sidebar، Anti-bot Stealth و Auto Model Routing |
| `/setup-deploy` | تنظیم Deploy |
| `/setup-gbrain` | راه‌اندازی GBrain |
| `/sync-gbrain` | Index مجدد Code و Refresh کردن Guidance |
| `/gstack-upgrade` | Upgrade به آخرین نسخه |
| `/ios-qa` | iOS Live-Device QA |
| `/ios-fix`, `/ios-design-review`, `/ios-clean`, `/ios-sync` | ابزارهای چرخه QA و Debug iOS |

## Binaryهای مستقل

- `gstack-model-benchmark` — Benchmark بین Claude، GPT/Codex و Gemini؛ مقایسه Latency، Token، Cost و در صورت درخواست LLM-judge.
- `gstack-taste-update` — ذخیره Taste Profile برای انتخاب‌های `/design-shotgun`.
- `gstack-egress` — ثبت و Verify کردن Egress Receiptهای Hash-chained در `~/.gstack/security/egress.jsonl`.
- `gstack-context-bill` — Audit Offline هزینه Token Skills Tree؛ با `--diff`، `--budget` و `--exact`.
- `gstack-code-intelligence` — Interface مشترک برای GBrain، Sourcebot و Graphify.
- `gstack-verify-gate` — Stop Hook اختیاری برای الزام موفقیت Verify Command.
- `gstack-memorable` — Bridge اختیاری به Memorable برای Claude Code.
- `gstack-wtree` — Working-tree fingerprint.
- `gstack-review-log` — Receiptهای Review Pass.
- `gstack-review-read` — وضعیت Freshness یک Review: CURRENT / STALE / UNVERIFIED.
- `gstack-evidence` — Ledger مربوط به Verification Evidence.
- `gstack-issue-guard` — Trust Envelope برای متن Issue/PR.
- `gstack-ios-qa-daemon` — Broker سمت Mac برای iPhone متصل.
- `gstack-ios-qa-mint` — مدیریت Allowlist مربوط به iOS QA.
- `gstack-ios-qa-regen` — بازتولید DebugBridge و Typed Accessorها.

## CSO Evaluation و Hookها

Producer خصوصی Paid برای CSO یک Packaging Contract است و شامل `cso-eval-producer`، `gstack-cso-launcher`، `gstack-cso-core`، `gstack-cso-watchdog` و Manifest مخفی `.gstack-cso-generation` می‌شود. نصب باید Root-owned و Nonwritable باشد و Receiptها Hash همه Artifactها را Bind می‌کنند.

Setup یک Stop Hook پیش‌فرض به نام `gstack-timeline-stop` در `~/.claude/settings.json` ثبت می‌کند. برای Opt-out:

```bash
./setup --no-timeline-stop-hook
```

یا:

```bash
GSTACK_TIMELINE_STOP_HOOK=no
gstack-config set timeline_stop_hook no
```

برای پاک‌سازی Hookهای قدیمی:

```bash
gstack-settings-hook prune-stale --repoint
```

## Continuous Checkpoint Mode

```bash
gstack-config set checkpoint_mode continuous
```

در این حالت Commitهای `WIP:` با Body ساختاریافته `[gstack-context]` ایجاد می‌شوند. `/context-restore` وضعیت Session را بازسازی می‌کند و `/ship` پیش از PR آن‌ها را Squash می‌کند. Push فقط با `checkpoint_push=true` فعال است و پیش‌فرض Local-only است.

## Domain Skills و Raw CDP

`$B domain-skill save` یک Note Per-site ذخیره می‌کند؛ پس از ۳ استفاده موفق فعال می‌شود و با `$B domain-skill promote-to-global` قابل Promotion است.

`$B cdp <Domain.method>` مسیر Raw برای Chrome DevTools Protocol است. Methodها باید با دلیل یک‌خطی در `browse/src/cdp-allowlist.ts` Allowlist شوند و خروجی Data-exfil در Envelope با برچسب UNTRUSTED قرار می‌گیرد.

مرجع عمیق همه Skillها: [docs/skills.md](docs/skills.md)

## چهار Failure Mode Karpathy

gstack چهار Failure Mode را هدف می‌گیرد: فرض اشتباه، پیچیدگی بیش‌ازحد، تغییرات Orthogonal و Imperative به‌جای Declarative. `/office-hours` فرض‌ها را آشکار می‌کند، Confusion Protocol از حدس‌زدن در تصمیم‌های معماری جلوگیری می‌کند، `/review` پیچیدگی و Drive-by Edit را پیدا می‌کند و `/ship` کار را با Test-first به هدف قابل Verify تبدیل می‌کند.

## Design، QA و Browser

`/design-shotgun` چهار تا شش Variant با GPT Image می‌سازد، Comparison Board ایجاد می‌کند و Taste Memory را به کار می‌گیرد. `/design-html` با Pretext خروجی Production HTML/CSS می‌سازد؛ 30KB و بدون Dependency و با تشخیص React/Svelte/Vue.

`/qa` می‌تواند تعداد Workerهای موازی را از ۶ به ۱۲ برساند. هر Fix یک Regression Test تولید می‌کند و `/ship` Coverage Audit می‌دهد.

در Mac با Aside، `/qa`، `/qa-only`، `/design-review`، `/canary`، `/benchmark`، `/scrape` و `/browse` روی Browser واقعی و Sessionهای Loginشده اجرا می‌شوند. بدون Aside، Chromium داخلی gstack خودکار استفاده می‌شود.

`/open-gstack-browser` GStack Browser را با Anti-bot Stealth و Sidebar اجرا می‌کند. Sonnet برای Action و Opus برای Analysis استفاده می‌شود و هر Sidebar Task تا ۵ دقیقه زمان دارد.

### Prompt Injection Defense

gstack روی هر Page Read از Content Filterهای Datamarking، Hidden-element stripping، ARIA scrubbing و URL Blocklist استفاده می‌کند و یک ML Classifier محلی 22MB را در Sidecar Subprocess اجرا می‌کند. Verdict Combiner برای Block نیازمند Agreement است. همه‌چیز محلی است.

Kill Switch:

```bash
GSTACK_SECURITY_OFF=1
```

### Browser Handoff

```bash
$B handoff
```

پس از حل CAPTCHA/Auth/MFA:

```bash
$B resume
```

Agent از همان نقطه ادامه می‌دهد.

### Pair Agent

`/pair-agent` Claude Code را با OpenClaw، Hermes، Codex، Cursor و Agentهای دیگر هماهنگ می‌کند. هر Agent Tab خودش را دارد؛ Scoped Token، Tab Isolation، Rate Limiting و Activity Attribution فعال است و در صورت نصب ngrok، Remote Agent هم می‌تواند متصل شود.

### Multi-AI Second Opinion

در Codex، `/claude-code` Review را به Claude Code می‌فرستد و در Claude Code، `/codex` آن را به OpenAI Codex می‌فرستد. Review، Challenge و Consultation با Session Continuity پشتیبانی می‌شوند.

### Safety Guardrails

`/careful` قبل از `rm -rf`، `DROP TABLE`، force-push و `git reset --hard` هشدار می‌دهد. `/freeze` محدوده ویرایش را قفل می‌کند و `/guard` هر دو را فعال می‌کند.

## ۱۰–۱۵ Sprint موازی

[Conductor](https://conductor.build) Sessionهای Claude Code را در Workspaceهای ایزوله موازی اجرا می‌کند. ساختار Think → Plan → Build → Review → Test → Ship باعث می‌شود هر Agent بداند چه کاری انجام دهد و چه زمانی متوقف شود. نویسنده می‌گوید ۱۰–۱۵ Sprint موازی سقف عملی فعلی اوست.

### Voice

عبارت‌هایی مثل "run a security check"، "test the website" و "do an engineering review" می‌توانند Skill مناسب را فعال کنند.

## حذف نصب

### روش ۱

```bash
~/.claude/skills/gstack/bin/gstack-uninstall
```

گزینه‌های `--keep-state` و `--force` به‌ترتیب برای حفظ State و رد Confirmation هستند.

### روش ۲ — حذف دستی

```bash
pkill -f "gstack.*browse" 2>/dev/null || true
rm -rf ~/.claude/skills/gstack
rm -rf ~/.gstack
rm -rf ${CODEX_HOME:-$HOME/.codex}/skills/gstack* 2>/dev/null
rm -rf ~/.factory/skills/gstack* 2>/dev/null
rm -rf ~/.kiro/skills/gstack* 2>/dev/null
rm -rf ~/.openclaw/skills/gstack* 2>/dev/null
rm -rf ~/.cursor/skills/gstack* 2>/dev/null
rm -rf ~/.config/opencode/skills/gstack* 2>/dev/null
rm -f /tmp/gstack-* 2>/dev/null
rm -rf .gstack .gstack-worktrees .claude/skills/gstack 2>/dev/null
rm -rf .agents/skills/gstack* .factory/skills/gstack* 2>/dev/null
```

حذف دستی Hookهای gstack را در `~/.claude/settings.json` باقی می‌گذارد؛ Hookهایی را که به `.claude/skills/gstack/` اشاره می‌کنند، از جمله SessionStart auto-update، AskUserQuestion PreToolUse/PostToolUse و Stop Hookها حذف کنید.

### CLAUDE.md

Uninstall Script آن را تغییر نمی‌دهد. در پروژه‌هایی که gstack اضافه شده، بخش‌های `## gstack` و `## Skill routing` را حذف کنید.

### Playwright

`~/Library/Caches/ms-playwright/` در macOS باقی می‌ماند چون ابزارهای دیگر ممکن است از آن استفاده کنند.

### Aside

gstack هرگز Aside را نصب نکرده و بنابراین آن را Uninstall نمی‌کند.

---

رایگان، متن‌باز و دارای مجوز MIT. بدون Premium Tier و بدون Waitlist.

**منبع اصلی:** [garrytan/gstack](https://github.com/garrytan/gstack)


## GBrain — حافظه پایدار برای Agent

[GBrain](https://github.com/garrytan/gbrain) یک Knowledge Base پایدار برای Agentهای هوش مصنوعی است؛ یعنی اطلاعاتی که Agent از پروژه یاد می‌گیرد می‌تواند بین Sessionها باقی بماند.

راه‌اندازی:

`/setup-gbrain`

چهار مسیر اصلی دارد: Supabase با URL موجود، Supabase با Provision خودکار، PGLite محلی و Remote gbrain MCP. پس از Init، امکان ثبت GBrain به‌عنوان MCP Server در Claude Code وجود دارد:

`claude mcp add gbrain -- gbrain serve`

برای همگام‌سازی کد:

`/sync-gbrain`

`--full` برای Reindex کامل و `--dry-run` برای پیش‌نمایش است.

سطح Trust هر Repo می‌تواند `read-write`، `read-only` یا `deny` باشد.

برای Sync کردن Memory خود gstack:

`gstack-artifacts-init`

این قابلیت می‌تواند Learnings، Planها، Design Docها، Retroها و Developer Profile را به Git خصوصی منتقل کند و Secret Scanner از AWS Key، Token، PEM و JWT جلوگیری می‌کند.

## حریم خصوصی و Telemetry

Telemetry به‌صورت **پیش‌فرض خاموش** است و فقط با Opt-in فعال می‌شود. در صورت فعال‌سازی، نام Skill، مدت اجرا، نتیجه، نسخه gstack و OS ارسال می‌شود؛ کد، مسیر فایل، نام Repo، Branch، Prompt و محتوای کاربر ارسال نمی‌شود.

خاموش‌کردن:

`gstack-config set telemetry off`

ارسال‌های خارج از دستگاه Receipt زنجیره‌ای در `~/.gstack/security/egress.jsonl` دارند. برای Audit:

`gstack-egress list`
`gstack-egress verify`
`gstack-egress grants`

Analytics محلی:

`gstack-analytics`

## Troubleshooting

**Skill دیده نمی‌شود:** `cd ~/.claude/skills/gstack && ./setup`

**`/browse` یا `/qa` پیام `NEEDS_ASIDE` یا `ASIDE_NOT_RUNNING` می‌دهد:** Aside را باز و Login کنید؛ `aside --version` و `aside repl 'console.log("ok")'` را بررسی کنید. برای اجبار به نادیده‌گرفتن Aside از `GSTACK_SKIP_ASIDE=1` استفاده کنید.

**Browser جایگزین مشکل دارد:** `cd ~/.claude/skills/gstack && bun install && bun run build`

**`/make-pdf` یا `/diagram` Render نمی‌شود:** با Aside تست `bun run ~/.claude/skills/gstack/bin/gstack-render.ts some.html --screenshot /tmp/out.png` را اجرا کنید؛ بدون Aside نیز `bun run build` راهکار اصلی است.

**نصب قدیمی است:** `/gstack-upgrade` یا `auto_upgrade: true` در `~/.gstack/config.yaml`

**Command کوتاه می‌خواهید:** `./setup --no-prefix`

**Command Namespaceدار می‌خواهید:** `./setup --prefix`

**Codex خطای invalid SKILL.md می‌دهد:** نصب Codex را با `./setup --host codex` دوباره Generate کنید.

## مستندات مهم

- [Skill Deep Dives](docs/skills.md) — فلسفه و Workflow تمام Skillها
- [Diagrams & Document Formats](docs/howto-diagrams-and-formats.md) — Mermaid، Excalidraw و PDF
- [Builder Ethos](ETHOS.md) — اصول Builder
- [Using GBrain with GStack](USING_GBRAIN_WITH_GSTACK.md) — راهنمای کامل GBrain
- [GBrain Sync](docs/gbrain-sync.md) — Sync و Privacy
- [Architecture](ARCHITECTURE.md) — معماری و Internals
- [Browser](BROWSER.md) — Browser و مرجع `$B`
- [Contributing](CONTRIBUTING.md) — توسعه و Contributor Mode
- [CHANGELOG](CHANGELOG.md) — تغییرات نسخه‌ها

## فلسفه این ترجمه

این README قرار است **پروژه را برای کاربر فارسی‌زبان قابل فهم‌تر کند، بدون اینکه رفتار پروژه را تغییر دهد**. بنابراین Code، Command، Path، Environment Variable و شناسه‌های فنی عمداً دست‌نخورده می‌مانند. توضیح فارسی می‌تواند مفهوم را روشن کند، اما نباید دستور اجرایی جدیدی به‌جای دستور اصلی بسازد.
