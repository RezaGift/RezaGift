# Reza Jowkar

### Senior Python Engineer — Distributed Systems · Algorithmic Trading · Compliance Intelligence · Payment Infrastructure

Founder of [**RezaGift.com**](https://rezagift.com) · 15+ years building software that handles real money and real consequences.

<p>
  <img src="https://img.shields.io/badge/Python-Expert-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI" />
  <img src="https://img.shields.io/badge/Celery-37814A?style=for-the-badge&logo=celery&logoColor=white" alt="Celery" />
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL" />
  <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white" alt="Redis" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white" alt="TensorFlow" />
  <img src="https://img.shields.io/badge/Playwright-2EAD33?style=for-the-badge&logo=playwright&logoColor=white" alt="Playwright" />
</p>

I don't build demos. I build systems that run unattended, execute live orders, and produce verdicts that businesses act on — where a silent bug costs money, not a bad review.

That constraint shapes everything: **parity guarantees between test and production**, **fail-closed execution paths**, **governed model promotion**, and **forensic post-mortems written down before the next version ships**.

---

## 📊 By the Numbers

| Project | Domain | Language | Scale | What Makes It Hard |
|:---|:---|:---|---:|:---|
| **SiteWatcher** | Compliance intelligence | Python | **23,000 LOC** · 207 modules | 93 REST endpoints · 338 tests · 41 rule engines · distributed workers |
| **Crypto RG** | Algorithmic trading | Python | **39,800 LOC** · 90 modules | ML ensemble · backtest↔live parity · real-money safety chain |
| **Security Scanner** | Automated auditing | Python | **2,500 LOC** | 1,380-line probe engine · PDF reporting · licensing + integrity |
| **InternetGuard** | Network enforcement | PowerShell | **1,220 LOC** | Kernel-level firewall policy · emergency restore paths |
| **AI Content Agent** | Content automation | PHP | **2,250 LOC** | LLM pipeline integrated into a live storefront |
| **AI Image Generator** | Generative pipeline | Python | **725 LOC** | Local Stable Diffusion automation |

> **≈ 66,000 lines of production Python** across trading, compliance, and security systems — plus PowerShell, PHP, and JavaScript. All of it written, debugged, and operated by one engineer.

---

## 🏆 SiteWatcher — Distributed Website Compliance Intelligence

**The flagship. There is no off-the-shelf equivalent.**

Commercial scanners tell you whether a site is *technically* healthy — SSL, uptime, headers. SiteWatcher answers a fundamentally harder question: **is this business legitimate, compliant, and safe to transact with?** It reads a website the way a regulator would, at scale, and produces a defensible verdict with evidence attached.

### Architecture

```
FastAPI (93 endpoints, 14 route modules)
   ├── Celery workers (7 task pipelines)  ──►  Redis broker
   ├── 35 service modules  ──►  PostgreSQL (23 models, 11 migrations)
   ├── 41 rule engines across 11 domains
   ├── Playwright headless rendering  ──►  S3-compatible artifact store
   └── 338 automated tests across 76 test modules
```

**23,026 lines of Python across 207 modules** — layered into `api / core / db / models / rules / schemas / services / tasks / utils`, containerised with Docker Compose, versioned with Alembic, documented through a unified Swagger surface.

### The Rule Engine — 41 checks across 11 domains

| Domain | Examples |
|:---|:---|
| **Contact & Identity** | contact page presence, phone validity, physical address, identity completeness |
| **Legal Disclosure** | privacy policy, terms of service, refund policy, disclosure baseline |
| **Trust & Verification** | trust badges, verification authenticity, verified-contact alignment |
| **Prohibited Content** | restricted keyword detection, smuggled-goods signals |
| **Site Typology** | storefront vs. lead-gen vs. content vs. directory vs. marketplace vs. landing |
| **Commerce Depth** | catalogue depth, pricing presence, shipping disclosure, cart-action validity |
| **Booking & Service** | booking flow integrity, booking policy, service offer, service area / hours |
| **Industry Verticals** | real estate, tourism, education, beauty, auto-service — each with tailored checks |
| **Imagery** | logo presence, explicit-content detection |
| **Editorial** | content signals, listing density, seller signals |
| **Directory** | contact depth, listing completeness |

Each rule carries its own evidence trail — a verdict is never a number without a reason.

### Engineering Depth — Three Bugs Worth Reading About

**1. The stdlib was silently refusing entire domains.**
Python's `RobotFileParser` runs every pattern through `urlparse`, so WordPress's standard `Disallow: /?` line — meaning *"no query strings"* — arrived as `Disallow: /`, meaning *"no site at all."* Four of ten sites in one audit round were refused wrongly, and one produced no verdict whatsoever. Worse, the outcome turned on luck: when the `robots.txt` fetch *failed*, the fallback allowed the scan, so broken sites got crawled and correct sites got refused.

Replaced with a spec-correct matcher: `*` spans any character run, trailing `$` anchors, longest matching pattern wins, `Allow` breaks exact ties, and a named user-agent group outranks `*`. **Both failure directions have regression tests** — refusing a permitted site hides a business from inspection; crawling a forbidden one breaks the promise the tool makes.

**2. "adjust a moment" was reading as a Cloudflare challenge.**
Zero-width non-joiner handling stripped spaces to normalise Persian text, which made the phrase *"adjust a moment"* contain *"just a moment"* — the Cloudflare interstitial marker. Ordinary editorial sentences were being classified as bot challenges. Fixed: zero-width marks removed, whitespace collapsed to single spaces, every marker matched on **word boundaries** rather than substrings.

**3. A cart icon in the navbar is not a catalogue.**
The phrase «سبد خرید» sits in the menu of lifestyle magazines, insurers, and market-data sites that have never shown a price. All three were being audited for pricing, catalogue depth, shipping, and refunds — and failing checks that didn't apply to them. Cart *actions* («افزودن به سبد», "add to cart", "buy now") still stand alone as evidence; cart *nouns* now require a price or product card beside them.

**Also:** a path segment of five or more digits is recognised as an article ID, so a news headline containing «درباره» can no longer occupy the about-page slot. Compliance sections outrank editorial ones during crawl expansion, so a rotating homepage can't change a verdict between runs.

### Production Features

- **Persian-first by design** — RTL-correct PDF and Excel reports (`arabic-reshaper` + `python-bidi`), Persian compliance console, ZWNJ-aware text normalisation throughout
- **Batch orchestration** at scale with score-trend analytics and dead-letter queues for poison tasks
- **Proxy health monitoring** and network-resilience layers for hostile crawl conditions
- **OIDC + session authentication** with an operator console per role
- **Image analysis** and a **search index** over collected evidence
- **One-command bootstrap** — provisions the environment, installs dependencies, generates credentials, boots the stack, copies the admin key to clipboard, and opens a tabbed console. A working install from a double-click.

`FastAPI` · `Celery` · `Redis` · `PostgreSQL` · `SQLAlchemy 2.x` · `Pydantic v2` · `Playwright` · `Alembic` · `Docker Compose` · `boto3` · `ReportLab` · `openpyxl` · `httpx` · `BeautifulSoup`

---

## 🤖 Crypto RG — Algorithmic Futures Trading System

**39,872 lines of Python.** A production Binance Futures platform built on the assumption that **something will fail mid-trade** — the network, the exchange, the power, the process.

| Component | Lines | Role |
|:---|---:|:---|
| `app.py` | **16,226** | Flask control dashboard — 70 routes, 6 operational tabs |
| `hybrid_backtest_engine.py` | 3,578 | Backtesting engine |
| `ml_engine.py` | 2,690 | ML ensemble training and inference |
| `decision_engine.py` | 1,269 | Signal synthesis |
| `master_signal.py` | 896 | Multi-timeframe aggregation |
| `analysis_engine.py` | 895 | Technical analysis layer |
| `backtest_reference_core.py` | 688 | **Shared truth for backtest, paper, and live** |
| **23 verification scripts** | — | Parity, integrity, and recovery assertions |
| **27 operator tools** | — | Quality gates, champion activation, audits, sweeps |

### The Core Idea: Parity Is Not Assumed, It's Asserted

A single **Reference Core** serves the backtest engine, the paper trader, and the live executor. A strategy *cannot* behave differently in production than it did in validation, because they run the same decision code. This is enforced by a dedicated verification suite — `verify_ml_override_parity`, `verify_parity_reversal`, `verify_source_integrity`, `verify_funding_timezone`, `verify_runtime_state_recovery`, and 18 more — run after every change.

Signal flow is deliberately single-source: the Telegram bot is the source of truth, and paper and live executors are **listeners only**. Position closes belong to the executor; the signal tracker only closes its record after `executor_confirmed_close`. No dual authority, no race.

### ML Ensemble with Real Governance

Random Forest · Gradient Boosting · SVM · XGBoost · **3× LSTM networks** · plus dedicated **no-trade**, **meta**, and **segment** filter models — combined under walk-forward validation with reproducibility locks on every artifact.

The governance layer is what makes it production-grade:

- Active models are **reference-locked**. Retraining never overwrites production.
- A challenger must clear `professional_quality_gate.py` before `activate_champion.py` will promote it.
- Model metadata and walk-forward reports are persisted alongside every artifact.

### Fail-Closed Real-Money Execution

Live orders stay **blocked** until *all* of the following are simultaneously present:

1. A valid `CHAMPION_LOCK.json`
2. A paper-trading attestation
3. `CRYPTO_REAL_MONEY_ACK` in the environment
4. `REAL_LIVE_ORDER_ALLOWED` in the environment

Deliberately hard to enable by accident. Secrets live only in `.env`; config files carry `ENV:` placeholders, and the config writer is specifically built so saving from the UI can never write a real token back to disk.

### Operational Resilience

Crash auto-restart · runtime watchdog · Windows keep-awake guardian · durable signal state across restarts · auto-resume for paper trading (live requires human acknowledgement — never auto-resumed) · network health diagnostics · operator-intent state files separated from runtime state.

### Risk Governance

Leverage caps · per-trade margin ceilings · aggregate exposure limits · staged TP1/TP2 exits with forced stop-loss · cooldown windows · session-hour filtering · position-count and one-side limits · portfolio-level A/B symbol evidence before basket changes.

### The Forensics Culture

> A production audit traced **45 of 56 recorded paper trades to a phantom-close bug** in the executor key lifecycle: a closed-position key was never cleared, so every subsequent signal for that symbol was silently dropped and marked closed within ~10 seconds.
>
> The fix shipped with a dedicated regression verifier — and **every result predating it was formally invalidated** and the evaluation period restarted, rather than quietly kept. 26 changelogs document this discipline across the project's history.

`Python` · `Flask` · `CCXT` · `Binance Futures API` · `scikit-learn` · `XGBoost` · `TensorFlow/Keras` · `pandas` · `NumPy` · `Telegram Bot API`

---

## 🛡️ Security Scanner — Automated Website Security Auditing

**~2,500 lines** of self-hosted security assessment, built to score real-world sites accurately across wildly inconsistent server configurations.

- **1,381-line probe engine** — multi-source verification with fallback paths, so a single failed probe never becomes a false verdict
- **Network resilience** tuned against unstable routes, TLS interception, and sinkhole-filtered DNS
- **Calibrated scoring** that distinguishes *misconfigured* from *deliberately hardened*, including protocol-specific exemptions (SSH and similar) that prevent legitimate setups being penalised
- **PDF report generation**, integrity self-check, and license enforcement
- Hardened across **20+ documented audit rounds** — reliability guards, performance passes, private-URL handling, logging correctness

---

## 🔐 InternetGuard — Windows Network Enforcement

**1,217 lines of PowerShell** implementing allow-list network enforcement on Windows 11, for controlled research and operational lockdown environments.

- Path-based allow-listing with per-application firewall rules
- **Dry-run mode** and **full firewall state backup** before any change is applied
- **Emergency restore paths** — including a raw fallback — so a bad policy can never leave a machine unreachable
- Startup enforcer with install/remove lifecycle, live status reporting, and a diagnostics suite
- Crypto-only enforcement profile for isolating trading traffic from all other network activity
- 11 single-purpose operator commands — enable, disable, dry-run, status, diagnose, restore, install, remove

---

## 🧠 Offline AI Infrastructure

Self-hosted LLM inference for workloads where data must never leave the machine:

**DeepSeek-R1 14B** · **Qwen3 14B** · **Gemma3 12B** — served locally via Ollama, packaged for air-gapped deployment.

Paired with a **725-line Stable Diffusion automation pipeline** for programmatic visual asset generation.

---

## 📰 AI Content Agent

**~2,250 lines of PHP** across 22 modules — an automated content pipeline running inside RezaGift.com. LLM-assisted article generation, templated publishing, and newsletter distribution, integrated directly into a live storefront rather than bolted on beside it.

---

## 💳 RezaGift.com — Payment & Regional Commerce

A production digital-goods marketplace handling **multi-region gift cards, account verification, and cryptocurrency settlement.**

Years of live operation with real customers, real chargebacks, real fraud attempts, and real regulatory friction. This is where the engineering discipline in every project above comes from — you write fail-closed code after the first time a race condition costs you money.

---

## 💼 Technical Stack

| Domain | Technologies |
|:---|:---|
| **Languages** | Python (expert), JavaScript, PowerShell, PHP, Bash, SQL |
| **Backend** | FastAPI, Flask, SQLAlchemy 2.x, Pydantic v2, async patterns, WebSockets, REST design, OpenAPI |
| **Distributed** | Celery, Redis, task queues, dead-letter handling, batch orchestration, worker scaling |
| **Data** | PostgreSQL, Alembic migrations, SQLite, S3-compatible object storage |
| **ML / Data Science** | scikit-learn, XGBoost, LightGBM, TensorFlow / Keras, LSTM, pandas, NumPy, walk-forward validation, ensemble methods, model governance |
| **Trading / Finance** | CCXT, Binance Futures API, backtesting engines, execution safety layers, risk governance, position management |
| **Crawling / Automation** | Playwright, BeautifulSoup, httpx, robots.txt compliance, headless rendering, proxy health management |
| **Infrastructure** | Docker, Docker Compose, Linux administration, automated deployment, quality gates, one-command bootstrap |
| **AI / LLM** | Ollama, local model serving, air-gapped inference, Stable Diffusion, prompt-driven pipelines |
| **Reporting** | ReportLab, openpyxl, RTL/Persian typography (`arabic-reshaper`, `python-bidi`) |
| **Security** | TLS/HTTP analysis, network diagnostics, Windows Firewall API, firmware modification, mobile/embedded reverse engineering |
| **Testing** | pytest, parity verification suites, regression forensics, integration testing at 338-test scale |

---

## 🧭 Engineering Principles

**Parity before performance.** If backtest and live can diverge, the backtest is a fiction. One decision core, asserted by 23 verification scripts.

**Fail-closed by default.** Real-money paths stay locked until every precondition is *proven* present. Four independent gates, all required.

**Governed promotion.** No model, config, or strategy reaches production without clearing an explicit quality gate. Champion/challenger, never overwrite.

**Both failure directions get tested.** A false negative and a false positive are different bugs with different costs. Each gets its own regression test.

**Forensics get written down.** 26 changelogs and counting. Every significant bug ships with a root-cause narrative, a regression verifier, and — when the data was compromised — an honest invalidation of the results that came before.

**Built for the operator.** One-command launchers, clipboard-ready credentials, clear status surfaces, emergency restore paths. Production systems are run by people under time pressure.

---

## 🤝 Available For

**Algorithmic trading systems** — crypto spot & futures, signal engineering, backtesting infrastructure, execution safety layers, risk governance

**Distributed Python backends** — FastAPI + Celery architectures, high-reliability services, task orchestration at scale

**Compliance & audit platforms** — automated scanning engines, rule frameworks, evidence-backed reporting pipelines

**Payment system integration** — multi-region processing, crypto settlement, gift card APIs, verification flows

**Production ML systems** — not notebooks: model governance, reproducibility, walk-forward validation, promotion pipelines, drift control

**Technical consulting** — architecture review, reliability hardening, system forensics, post-mortem investigation

---

## 📫 Contact

🌐 **Website:** [rezagift.com](https://rezagift.com)
📧 **Email:** RezaGift.com@yahoo.com
💼 **LinkedIn:** Private

---

<div align="center">

**📌 Pinned repositories below** ⬇️

*Detailed architecture notes and changelogs available on request.*

</div>
