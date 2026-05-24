<p align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,100:1e293b&height=250&section=header&text=Dipak%20Dhangar&fontSize=42&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Backend%20Systems%20·%20Applied%20ML%20·%20Open%20Source&descAlignY=60&descSize=16" />
</p>

<div align="center">

[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=flat-square&logo=vercel&logoColor=white)](https://dipak-dhangar.vercel.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dipak-dhangar/)
[![Gmail](https://img.shields.io/badge/Gmail-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:dhangardip09@gmail.com)
[![LeetCode](https://img.shields.io/badge/LeetCode-FFA116?style=flat-square&logo=leetcode&logoColor=white)](https://leetcode.com/u/Dipak1106/)

</div>

---

Second-year AIML student. My main interest is backend systems — I want to understand what breaks under concurrency, how queues fail, and where consistency gets hard. I build things to answer those questions, not to ship products. Also contribute to open source projects in the API tooling and Linux kernel workflow space. Alongside systems work, I've built with scikit-learn, LSTMs, and spaCy — enough to understand where ML fits and where it doesn't.

---

## Projects

### [Orchestrix](https://github.com/dipak0000812/Orchestrix) — Job Orchestration Engine

![Go](https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white)

Async job engine in Go. Worker pool, full state machine (`PENDING → SCHEDULED → RUNNING → SUCCEEDED/FAILED/RETRYING`), exponential backoff, graceful drain on shutdown. Built primarily to work through real concurrency and failure-handling problems.

Three specific things I had to debug and fix:

**Scheduler race** — Multiple scheduler instances polling the same DB table would double-claim jobs. Fixed by wrapping job claims in `SELECT FOR UPDATE SKIP LOCKED` inside a transaction. Each instance atomically acquires a non-overlapping job set.

**Silent retry loops** — Unregistered executor types caused jobs to hit max retries silently, burning resources with no signal. Added explicit error classification: missing executors and panics route directly to `FAILED`; only actual execution errors are retryable.

**Import cycle** — Attaching Prometheus instrumentation to the worker package created a `worker → api → worker` dependency cycle. Resolved by extracting metrics into a standalone package imported independently by both.

`v1` done. Currently exploring Redis/Kafka-backed queues as an alternative to DB polling and adding priority scheduling.

---

### [PRISM-AI](https://github.com/dipak0000812/PRISM-AI) — PR Risk Analysis Tool

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![Claude](https://img.shields.io/badge/Claude-D4A017?style=flat-square)

*GitLab AI Hackathon — ZerothLayer.*

Static analysis pipeline that scores pull request risk before review. The LLM generates the human-readable summary at the end — it does not do the analysis.

**Pipeline:**
- tree-sitter parses changed files into ASTs (Python + JS/TS)
- NetworkX builds the import dependency graph; BFS reversal computes structural blast radius
- Six deterministic factors produce a 0–100 risk score: PR size, file churn rate, whether auth/payment modules were touched, test delta, dependency traversal depth, author familiarity with the module
- Groq (Llama-3.3-70b) generates reviewer hints from the structured score — not from raw diff text

The design decision I'm most satisfied with: keeping the LLM downstream of deterministic scoring so the risk number is auditable and reproducible regardless of model behavior.

---

### [SolanaStreaks](https://github.com/dipak0000812/SolanaStreaks) — Prediction Market Prototype · [Live](https://solana-streaks-6ynn.vercel.app/)

![Rust](https://img.shields.io/badge/Rust-000000?style=flat-square&logo=rust&logoColor=white)
![Solana](https://img.shields.io/badge/Solana-9945FF?style=flat-square&logo=solana&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)

Hackathon prototype. On-chain prediction market with streak-based multipliers built on Solana devnet using Rust/Anchor. Main focus was understanding Anchor's account ownership model, escrow logic, and how bet payout instructions get structured on-chain. Not actively maintained.

---

## Open Source

[![Microcks](https://img.shields.io/badge/Microcks-Active_Contributor-0D47A1?style=flat-square)](https://github.com/microcks)
[![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-Contributor-425CC7?style=flat-square)](https://github.com/open-telemetry)
[![kworkflow](https://img.shields.io/badge/kworkflow-Contributor-E95420?style=flat-square)](https://github.com/kworkflow/kworkflow)
[![Layer5](https://img.shields.io/badge/Meshery-Contributor-00B39F?style=flat-square)](https://layer5.io)

- **Microcks** — API mocking and contract testing; active contributions
- **OpenTelemetry** — Contribution under review
- **kworkflow** — Linux kernel developer workflow tooling
- **Meshery / Layer5** — Contributor, earned First Design badge
- Hacktoberfest 2024 — 4 PRs merged

---

## Technical Focus

| Domain        | Stack                                                       |
| ------------- | ----------------------------------------------------------- |
| Languages     | Go · Python · TypeScript                                    |
| Familiar With | C · C++ · Rust                                              |
| Backend       | REST APIs · Worker pools · PostgreSQL · Docker · Prometheus |
| ML & AI       | Scikit-learn · LSTM · spaCy · Claude API                    |
| Exploring     | Distributed systems · Kafka · Redis · Linux internals       |

---

## Currently

- Extending Orchestrix with Redis/Kafka queue backend; want to understand the operational tradeoffs vs DB polling under load
- Working through Raft and consistency model literature (Lamport clocks, CRDT basics)
- Contributing to Microcks, OpenTelemetry, kworkflow

---

## Stats

<div align="center">

<sub>Stats reflect public repository activity.</sub><br/><br/>

<img src="https://github-readme-stats.vercel.app/api?username=dipak0000812&show_icons=true&theme=tokyonight&hide_border=true&count_private=true&rank_icon=github" height="150"/>
&nbsp;&nbsp;
<img src="https://github-readme-stats.vercel.app/api/top-langs/?username=dipak0000812&theme=tokyonight&hide_border=true&layout=compact&langs_count=5&hide=html,css" height="150"/>

</div>

---

<div align="center">

`dhangardip09@gmail.com` · [Portfolio](https://dipak-dhangar.vercel.app) · [LinkedIn](https://www.linkedin.com/in/dipak-dhangar/)

</div>
