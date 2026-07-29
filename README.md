# Reza Jowkar

**Senior Python Engineer · Fintech · Algorithmic Trading · Payment Systems · Security Tooling**

Founder of [**RezaGift.com**](https://rezagift.com) — a digital-goods marketplace for gift cards, accounts, and multi-region payment processing.

I build **production systems that handle real money** — trading bots that execute on live exchanges, payment flows that settle in crypto, and compliance scanners that run against real sites. 15+ years across software engineering, payment systems, and embedded/security work.

My work is characterised by **governance over speed**: champion/challenger model promotion, backtest↔live parity guarantees, multi-stage execution locks, and forensic post-mortems that get written down before the next version ships.

<p>
  <img src="https://img.shields.io/badge/Python-Expert-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" alt="FastAPI" />
  <img src="https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white" alt="Flask" />
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL" />
  <img src="https://img.shields.io/badge/Celery-37814A?style=flat-square&logo=celery&logoColor=white" alt="Celery" />
  <img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white" alt="Redis" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white" alt="scikit-learn" />
  <img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=flat-square&logo=tensorflow&logoColor=white" alt="TensorFlow" />
  <img src="https://img.shields.io/badge/Binance%20Futures-F0B90B?style=flat-square&logo=binance&logoColor=black" alt="Binance Futures" />
</p>

---

## 🛠 What I Build

### 🤖 Crypto RG — Algorithmic Futures Trading System

A production Binance Futures trading platform with a Flask control dashboard, an ML ensemble signal engine, and an execution layer designed on the assumption that **something will fail mid-trade**.

- **Signal engine** — a single *Reference Core* shared by backtest, paper, and live paths, so a strategy cannot behave differently in production than it did in validation. Parity is asserted by automated verification suites, not by trust.
- **ML ensemble** — Random Forest, Gradient Boosting, SVM, and XGBoost combined with 3× LSTM networks, plus dedicated no-trade / meta / segment filter models. Walk-forward validation with reproducibility locks on every artifact.
- **Model governance** — active models are reference-locked. Retraining never overwrites production: a challenger must clear an automated quality gate before promotion to champion.
- **Real-money safety chain** — live orders stay blocked until a valid champion lock, a paper-trading attestation, and two explicit environment acknowledgements are all present. Deliberately hard to enable by accident.
- **Operational resilience** — crash auto-restart, runtime watchdog, keep-awake guardian, durable signal state across restarts, network health diagnostics, and Telegram-driven signal distribution with executor-confirmed position closes.
- **Risk governance** — leverage and position-size guards, per-trade margin caps, aggregate exposure ceilings, staged TP1/TP2 exits with forced stop-loss, cooldown windows, and session-hour filtering.

> Notable engineering: a forensic audit traced 45 of 56 recorded paper trades to a *phantom-close* bug in the executor key lifecycle. The fix shipped with a dedicated regression verifier — and every result predating it was formally invalidated rather than quietly kept.

`Python` · `Flask` · `CCXT` · `Binance Futures API` · `scikit-learn` · `XGBoost` · `TensorFlow/Keras` · `pandas` · `NumPy`

---

### 🔍 SiteWatcher — Large-Scale Website Compliance & Monitoring

A distributed scanning platform that audits websites at scale against a 41-check compliance ruleset, with batch orchestration, scoring trends, and exportable inspection reports.

- **Async service architecture** — FastAPI API layer, Celery workers, Redis broker, PostgreSQL persistence, Alembic migrations, containerised with Docker Compose.
- **Standards-correct crawling** — a `robots.txt` matcher implemented to spec (wildcard spans, `$` anchoring, longest-pattern-wins, `Allow` tie-break, named user-agent precedence) after the stdlib parser was found to read `Disallow: /?` as `Disallow: /` and silently refuse entire domains.
- **Headless rendering** — Playwright-driven page capture for JavaScript-heavy sites, with challenge-page detection that matches on word boundaries instead of fragments.
- **Intelligent page selection** — compliance sections outrank editorial ones during crawl expansion, and numeric path segments are recognised as article IDs so a news headline can't occupy the about-page slot.
- **Reporting** — PDF and Excel exports with full RTL/Persian typography support (`arabic-reshaper` + `python-bidi`), S3-compatible artifact storage, dead-letter queues, and OIDC/session-based operator consoles behind a unified Swagger surface.
- **Operator experience** — single-command bootstrap that provisions the environment, generates credentials, boots the stack, and opens a tabbed console.

`FastAPI` · `Celery` · `Redis` · `PostgreSQL` · `SQLAlchemy 2.x` · `Playwright` · `Pydantic v2` · `Alembic` · `Docker` · `boto3` · `ReportLab`

---

### 🛡️ Security Scanner — Automated Website Security Auditing

A self-hosted security assessment engine that scores real-world sites accurately across wildly inconsistent server configurations.

- Multi-source verification with fallback paths, so a single failed probe never becomes a false verdict.
- Network resilience tuned against unstable routes, TLS interception, and sinkhole-filtered DNS.
- Calibrated scoring that distinguishes *misconfigured* from *deliberately hardened* — including protocol-specific exemptions that prevent legitimate setups being penalised.
- Reliability guards and performance/accuracy passes hardened across successive audit rounds.

`Python` · `TLS/HTTP analysis` · `network diagnostics` · `scoring heuristics`

---

### 🔐 InternetGuard — Windows Network Enforcement

A firewall enforcement layer for Windows 11 that restricts network access to an explicit allow-list, built for controlled research and operational lockdown environments.

- Path-based allow-listing with per-application enforcement rules.
- **Dry-run mode** and full firewall state backup before any change is applied.
- Emergency restore paths (including a raw fallback) so a bad policy can never leave a machine unreachable.
- Startup enforcer with install/remove lifecycle, live status reporting, and a diagnostics suite.
- Crypto-only enforcement profile for isolating trading traffic from everything else.

`PowerShell` · `Windows Firewall API` · `system administration`

---

### 🧪 Security Research Lab

A controlled environment for vulnerability research, payload analysis, and defensive tooling development — the testbed the scanners above are validated against.

---

### 🧠 Offline AI Infrastructure

Self-hosted LLM inference stack running DeepSeek-R1 14B, Qwen3 14B, and Gemma3 12B locally via Ollama — for workloads where data must never leave the machine. Paired with a local Stable Diffusion generation pipeline (`ai_generator_pro`) for automated visual asset production.

`Ollama` · `Stable Diffusion` · `local inference` · `GPU workloads`

---

### 📰 AI Content Agent

An automated content pipeline for RezaGift.com — AI-assisted article generation, newsletter distribution, and templated publishing integrated directly into the storefront.

---

### 💳 RezaGift.com — Payment & Regional Commerce

Production digital-goods marketplace handling multi-region gift cards, account verification, and cryptocurrency settlement. Years of live operation with real customers, real chargebacks, and real regulatory friction — the experience that informs everything above.

---

## 💼 Technical Stack

| Domain | Technologies |
|---|---|
| **Languages** | Python (expert), JavaScript, PowerShell, Bash, SQL, PHP |
| **ML / Data** | scikit-learn, XGBoost, LightGBM, TensorFlow / Keras, pandas, NumPy, walk-forward validation |
| **Backend** | FastAPI, Flask, SQLAlchemy 2.x, Pydantic v2, async patterns, WebSockets, REST design |
| **Distributed** | Celery, Redis, task queues, dead-letter handling, batch orchestration |
| **Data Stores** | PostgreSQL, Alembic migrations, S3-compatible object storage |
| **Trading / Finance** | CCXT, Binance Futures API, backtesting engines, risk governance, execution safety |
| **Automation / Crawling** | Playwright, BeautifulSoup, httpx, robots.txt compliance, headless rendering |
| **Infrastructure** | Docker, Docker Compose, Linux server administration, automated deployments, CI-style quality gates |
| **AI / LLM** | Ollama, local model serving, Stable Diffusion, prompt-driven content pipelines |
| **Specialized** | Firmware modification, mobile/embedded reverse engineering, payment processor integration, multi-region commerce, RTL/Persian document generation |

---

## 🧭 How I Work

- **Parity before performance** — if backtest and live can diverge, the backtest is a fiction. Verification suites enforce it.
- **Governed promotion** — no model, config, or strategy reaches production without passing an explicit gate.
- **Fail-closed by default** — real-money paths stay locked until every precondition is proven present.
- **Forensics get written down** — every significant bug ships with a changelog entry, a root-cause narrative, and a regression test.
- **Built for the operator** — one-click launchers, clear status surfaces, and emergency restore paths, because production systems are run by people under time pressure.

---

## 🤝 Available For

- **Algorithmic trading bot development** — crypto spot & futures, signal engineering, backtesting infrastructure, execution safety layers
- **Payment system integration** — multi-region processing, crypto settlement, gift card APIs, verification flows
- **Fintech backend engineering** — high-reliability Python services, async architectures, distributed task systems
- **Security audit tooling** — automated scanning platforms, compliance engines, reporting pipelines
- **ML systems for production** — not notebooks: model governance, reproducibility, walk-forward validation, promotion pipelines
- **Technical consulting** — architecture review, reliability hardening, system forensics

---

## 📫 Contact

🌐 **Website:** [rezagift.com](https://rezagift.com)
📧 **Email:** RezaGift.com@yahoo.com
💼 **LinkedIn:** *(coming soon)*

---

<div align="center">

### 📌 Pinned Projects

*See pinned repositories below* ⬇️

</div>
