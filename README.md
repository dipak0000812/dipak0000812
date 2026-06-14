<p align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,100:1e293b&height=250&section=header&text=Dipak%20Dhangar&fontSize=42&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Backend%20Systems%20·%20Applied%20ML%20·%20Open%20Source&descAlignY=60&descSize=16" />
</p>

<div align="center">

[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=flat-square&logo=vercel&logoColor=white)](https://dipak-dhangar.vercel.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dipak-dhangar/)
[![Gmail](https://img.shields.io/badge/Gmail-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:dhangardip09@gmail.com)
[![LeetCode](https://img.shields.io/badge/LeetCode-FFA116?style=flat-square&logo=leetcode&logoColor=white)](https://leetcode.com/u/Dipak1106/)

</div>

<p align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,100:1e293b&height=200&section=header&text=Dipak%20Dhangar&fontSize=38&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Backend%20%26%20Systems&descAlignY=60&descSize=15" />
</p>



---

Second-year AIML engineering student. I spend most of my time on backend systems - mainly Go and Python - and I'm trying to get better at the stuff that's hard to learn from tutorials: concurrency bugs, retry logic, database locking, that kind of thing. I also have some background in ML (scikit-learn, LSTMs, basic NLP with spaCy) from coursework and a couple of projects.

---

## Projects

### [Orchestrix](https://github.com/dipak0000812/Orchestrix) — Job Orchestration Engine
`Go` `PostgreSQL` `Docker` `Prometheus`

A job orchestration engine I built to actually run into and fix concurrency problems instead of just reading about them. It has a worker pool, a job state machine (PENDING → SCHEDULED → RUNNING → SUCCEEDED / FAILED / RETRYING), retries with exponential backoff, and a graceful shutdown that drains in-flight jobs.

A few specific bugs I hit and how I fixed them:

- **Duplicate job pickup**: when I ran multiple scheduler instances, they'd sometimes grab the same job from the DB at the same time. Fixed it with `SELECT FOR UPDATE SKIP LOCKED` inside a transaction so each instance claims a non-overlapping set of jobs.
- **Jobs retrying forever for no reason**: if a job's executor type wasn't registered, it would just keep retrying until it hit the max and fail silently - no useful error, just wasted cycles. I split errors into "retryable" vs "permanent" so missing executors and panics fail immediately instead of looping.
- **Import cycle**: adding Prometheus metrics to the worker package created a cycle between `worker` and `api`. Pulled the metrics into their own package that both import separately.

v1 is functional locally with Docker Compose. Not deployed yet - planning to put it on Render and possibly look at swapping the DB-polling scheduler for a Redis or Kafka-backed queue later.

---

### [PRISM-AI](https://github.com/dipak0000812/PRISM-AI) — PR Risk Analysis Tool
`Python` `FastAPI` `Next.js` `Groq`

Built for the GitLab AI Hackathon (ZerothLayer). The idea: score how risky a pull request is *before* a human reviews it, without relying on an LLM to do the actual judgment.

How it works:
- `tree-sitter` parses changed files (Python / JS / TS) into ASTs
- `NetworkX` builds an import dependency graph and works out what else in the codebase could be affected by the change
- A set of deterministic checks (PR size, file churn, whether auth/payment code was touched, test coverage delta, dependency depth, how familiar the author is with that part of the codebase) produce a 0-100 risk score
- An LLM (Llama 3.3 via Groq) takes that score and writes a plain-language summary for the reviewer - it doesn't influence the score itself

The reasoning behind that order: I wanted the risk number to stay the same regardless of what the LLM outputs, so it's reproducible and you can sanity-check it without trusting the model.

Working code, runs locally. Haven't deployed it yet.

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

| Domain        | Stack                                                                 |
| ------------- | -----------------------------------------------------------           |
| Languages     | Go · Python                                                           |
| Familiar With | C · C++ · Rust                                                        |
| Backend       | REST APIs · FASTAPI . Worker pools · PostgreSQL · Docker · Prometheus |
| ML & AI       | Scikit-learn · LSTM · spaCy · Claude API                              |
          

---

## Right now

Deploying Orchestrix and PRISM-AI so they're not just "clone and run locally." After that, planning to look at Redis-backed queues for Orchestrix and see how it compares to the current DB-polling approach.

