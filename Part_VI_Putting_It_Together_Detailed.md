# Part VI — Putting It Together (Detailed)
### Companion deep-dive to `Master_Senior_Fullstack_Roadmap.md` → The Complete Tech Stack Reference, The Master Project, Suggested Progression

> This file is different in kind from the five that came before it. Parts 0–V each taught you something new. This one doesn't — it's where everything from those five files gets assembled, tested against a real build, and turned into evidence you can actually point to. Think of it less as a syllabus and more as a **construction plan and inspection checklist** for the one project that proves the rest of this roadmap actually took.
>
> Three things live here:
> 1. **The Complete Tech Stack Reference, expanded** — not just a list, but *why* each piece was chosen and what you'd reach for instead in a different context.
> 2. **The Master Project, fully specified** — turned from a feature table into a real build plan with milestones, acceptance criteria, and an explicit map back to every phase in Parts I–V that each milestone is meant to prove.
> 3. **Suggested Progression, with checkpoints** — not just an ordering, but honest "am I actually ready to move on" self-assessment gates between stages, plus a closing section on what "done" with this whole roadmap actually looks like (hint: it doesn't look like "done").

---

## Table of Contents

- [1 — The Complete Tech Stack Reference (Expanded)](#1--the-complete-tech-stack-reference-expanded)
- [2 — The Master Project: Full Specification](#2--the-master-project-full-specification)
  - [2.1 — Vision & Scope](#21--vision--scope)
  - [2.2 — Feature-to-Phase Map](#22--feature-to-phase-map)
  - [2.3 — Suggested Repository Structure](#23--suggested-repository-structure)
  - [2.4 — Phased Build Plan with Acceptance Criteria](#24--phased-build-plan-with-acceptance-criteria)
  - [2.5 — Definition of Done](#25--definition-of-done)
- [3 — Suggested Progression, with Checkpoints](#3--suggested-progression-with-checkpoints)
- [4 — The Final Synthesis: How to Know You're Actually Senior](#4--the-final-synthesis-how-to-know-youre-actually-senior)
- [5 — Turning This Into a Portfolio](#5--turning-this-into-a-portfolio)

---

## 1 — The Complete Tech Stack Reference (Expanded)

> The master roadmap lists the stack flatly. Here, each entry includes *why it's the default choice for this roadmap*, and what you'd reach for instead if the context changed — because a senior engineer's value isn't knowing one stack, it's knowing why a stack was chosen at all.

### Languages
**Tier: 🎯 Mostly Master — Java/TypeScript/JS/SQL/Bash Master; HCL (Terraform) is Skip-adjacent, see Priority_Map.md**

| Tool | Why It's the Default Here | When You'd Choose Differently |
|---|---|---|
| **Java 21** | Forces explicit typing, memory, and OOP discipline (Part I); massive enterprise/fintech footprint; virtual threads make I/O-bound concurrency simple without reactive complexity — the **single backend language** of this roadmap: depth in one ecosystem beats breadth across several | A greenfield startup prioritizing raw iteration speed might reach for Node/TypeScript or Python instead; a systems/perf-critical service might reach for Go or Rust |
| **TypeScript** | Strict-mode type safety catches a large class of bugs before runtime (Part II, 4.3); near-universal on the frontend — and on this roadmap it is the **frontend** language only | Plain JavaScript only makes sense for tiny scripts where the tooling overhead of TS genuinely isn't worth it |
| **JavaScript (fundamentals)** | The browser's only native language; TypeScript is a layer *on top of* this, not a replacement for understanding it (Part II, 4.1) | N/A — this isn't optional if you touch a browser at all |
| **SQL** | PostgreSQL is the backbone of the whole data layer (Part III); SQL fluency transfers to almost every relational and many analytical systems | NoSQL query languages (e.g., MongoDB's query API) matter once you're actually using a document store for a genuine reason (Part III, 8.8) |
| **Bash** | The universal glue for Linux operations, CI scripts, and Docker entrypoints (Part 0, Part IV) | PowerShell if you're deep in a Windows-only ops environment — worth knowing exists, not worth mastering unless your infra demands it |
| **YAML** | The format of Docker Compose, GitHub Actions, and Kubernetes manifests (Part IV) | JSON where a tool specifically prefers it (some Terraform/AWS tooling) |
| **HCL** | Terraform's native configuration language (Part IV, 13.6) | Pulumi's HCL alternative (real TypeScript/Python for IaC) if your team strongly prefers a general-purpose language over a DSL |

### Frontend
**Tier: 🎯 Master**
HTML5 (semantic, Part 0) → plain CSS (Part 0) → Tailwind CSS (Part II, 4.7) → JavaScript ES6+ deeply (Part II, 4.1–4.2) → TypeScript strict mode (Part II, 4.3) → React 18+ (Part II, 4.5) → Next.js (Part II, 4.6) → TanStack Query, Zustand, React Router, Vite (Part II) → Playwright (E2E, Part IV, 11.4), Vitest (unit).

*Why this specific order matters, restated:* each layer is genuinely built on the one below it. Skipping straight to React without JavaScript fluency means every React bug looks like a React problem when it's actually a closures or `this`-binding problem wearing a React costume.

### Mobile
**Tier: ⏸️ Skip For Now — per Priority_Map.md, deprioritized for a fullstack *web* focus**
React Native, Expo, Expo Router, NativeWind, React Navigation, EAS Build + Submit, Fastlane (Part II, Phase 6). Chosen specifically because it reuses your React investment almost entirely — the alternative (separate native Swift/Kotlin codebases) is a real, valid choice for teams that need platform-specific performance or APIs badly enough to justify maintaining two codebases, but it's a different bet than this roadmap makes.

### Backend — Java + Spring Boot (the single backend)
**Tier: 🎯 Master**
Java 21, Spring Boot 3, Spring Security, Spring Data JPA, Hibernate, MapStruct, Flyway, JUnit 5, Mockito, Testcontainers, Maven, resilience4j (Part II, Phase 5, Track B). The professional/enterprise-job-market stack and your only backend. The Part II ecosystem-awareness notes cover Python/Go/Rust as further alternatives with their own genuine tradeoffs (different concurrency models, different deployment simplicity) — know they exist and roughly when each wins, without needing production depth in all of them.

### Databases
**Tier: 🎯 Master**
PostgreSQL (primary, Part III, Phase 8), pgvector (AI/embedding search, Part III, 8.5/8.8), Redis (cache/pub-sub/queues, Part III, 8.6), PgBouncer (connection pooling, Part III, 8.5), Flyway migrations (Part II, 5B.3). The broader NoSQL/vector-DB landscape (Part III, 8.8) is awareness-level — you reach for MongoDB, Cassandra, Elasticsearch, or Neo4j only once a real, specific problem shape actually calls for it, not by default.

### Infrastructure & DevOps
**Tier: 👀 Mostly Know Only — Docker + basic CI stay Master; Kubernetes/Terraform/deep AWS drop to Skip, see Priority_Map.md**
Linux (Ubuntu, Part 0), Git (Part 0/Part IV), GitHub Actions (Part IV, 13.4), Docker + Docker Compose (Part IV, 13.3), Kubernetes at working awareness (Part IV, 13.5), AWS — EC2, RDS, S3, ElastiCache, CloudFront, IAM, CloudWatch, Route 53, SQS, ECS (Part IV, 13.7), Terraform (Part IV, 13.6), Nginx, Let's Encrypt (Part IV, 13.8).

### Observability
**Tier: 👀 Know Only — health checks themselves are Master (Part II: Spring Boot Actuator), the full Prometheus/Grafana stack is Know Only**
Prometheus, Grafana, Sentry, Spring Boot Actuator + Micrometer (Part IV, 13.9) — implementing the RED method and a real SLI/SLO/error-budget model, not just "having dashboards."

### API & Communication
**Tier: 🎯 Master**
REST (Part II, Phase 7), WebSockets (Part II, 7.3), RabbitMQ + Spring AMQP (Part II, 5B.6), OpenAPI/Swagger (Part II, 7.5) — with GraphQL/gRPC/SSE/tRPC/AsyncAPI as ecosystem awareness (Part II, 7.6).

### Learning Resources (Reference)
LeetCode + Neetcode.io (Part I, Phase 2), VisuAlgo, Excalidraw (system design diagramming, Part IV, Phase 10), PortSwigger Web Security Academy (Part IV, Phase 14), Refactoring.guru (Part I, Phase 3), nand2tetris.org (Part I, Phase 1), *Designing Data-Intensive Applications* by Martin Kleppmann (the single best book for tying Parts III and IV together into one coherent mental model).

---

## 2 — The Master Project: Full Specification

> A **Developer Tools SaaS Platform** — a real product developers would plausibly pay for. Not a tutorial project. The goal is that by the time it's built, you could walk a hiring panel through *any* part of it and explain not just what it does, but why you made each real design decision, what you traded away, and what would break first under real load.

### 2.1 — Vision & Scope

**One sentence:** a platform where a developer can sign up, generate scoped API keys, hit a rate-limited REST API, get real-time usage notifications, search their own uploaded documents with AI-grounded answers, see their usage on a dashboard, and pay for it — all built on infrastructure you provisioned and can observe.

**Explicitly out of scope** (name these upfront — a senior engineer scopes deliberately, Part V, 15.1/15.4): a public marketplace, a plugin ecosystem, white-labeling, enterprise SSO. Naming what you're *not* building is as much a design decision as naming what you are.

### 2.2 — Feature-to-Phase Map

> Each feature exists specifically to force you to apply a real phase from Parts I–V, not just to look impressive on a feature list.

| Feature | Engineering Challenge | Proves (Part / Phase) |
|---|---|---|
| User Authentication | JWT + refresh token rotation + Argon2 + TOTP MFA | Part II 5B.4, Part IV 14.3 |
| API Key Management | Generate, rotate, revoke — scoped permissions | Part II 7.4, Part IV 14.4 |
| Rate Limiting | Per-user Redis token bucket, distributed across nodes | Part III 8.6, Part IV 10.4/12.4 |
| Background Jobs | RabbitMQ + Spring Boot workers + dead letter queue + idempotency | Part II 5B.6, Part IV 10.6 |
| Real-time Notifications | WebSockets + Redis pub/sub | Part II 7.3, Part IV 10.6 |
| AI-Powered Search | RAG with pgvector — search across user's documents | Part III 8.5/8.8 |
| Usage Analytics | Metrics collection + Grafana dashboards | Part IV 13.9 |
| Billing Integration | Stripe webhooks with idempotency keys | Part II 5B.8 |
| Multi-tenancy | Row-level security in PostgreSQL | Part II 5B.8, Part III 8.5 |
| Mobile App | React Native companion app with offline support | Part II Phase 6 |
| CI/CD + IaC | Automated pipeline, Terraform-provisioned infra | Part IV 13.4/13.6 |
| Observability | RED-method dashboards, real SLO, alerting | Part IV 13.9 |
| Test Suite | Right test type per layer, load-tested to a real breaking point | Part IV Phase 11/12 |
| Security Hardening | OWASP Top 10 addressed, one vulnerability deliberately introduced then fixed | Part IV Phase 14 |

If a feature doesn't map to something you genuinely struggled with or had to look up while building this roadmap, that's a signal — either you already had that skill solid, or you're taking a shortcut worth noticing (Part V, 17.5's Technical Debt Quadrant applies to your *learning process* too, not just code).

### 2.3 — Suggested Repository Structure

```
devtools-saas/
├── backend/                    ← Spring Boot 3, Java 21 — the only backend
│   ├── src/main/java/com/devtools/
│   │   ├── domain/               ← Entities, domain services, repository interfaces (Part I, 3.4)
│   │   ├── application/          ← Use cases, application services, DTOs
│   │   ├── infrastructure/       ← JPA repos, RabbitMQ, Stripe, Redis integrations
│   │   └── presentation/         ← Controllers, request/response models
│   └── src/test/java/            ← Unit (Mockito), integration (Testcontainers)
├── frontend/                   ← React 18 + TypeScript + Vite — consumes the Java API
│   ├── src/
│   │   ├── features/            ← Feature-based organization, not type-based
│   │   ├── shared/               ← Shared UI, hooks, utilities
│   │   └── api/                  ← TanStack Query hooks per domain
│   └── e2e/                     ← Playwright tests
├── mobile/                     ← React Native + Expo
├── infra/
│   ├── terraform/                ← VPC, EC2/ECS, RDS, S3, CloudFront, IAM
│   └── docker-compose.yml        ← Local dev: Postgres, Redis, RabbitMQ
├── .github/workflows/           ← CI/CD pipelines
└── docs/
    ├── adr/                      ← Architecture Decision Records (Part V, 16.2)
    └── openapi.yaml              ← API-first spec (Part II, 7.5)
```

> One backend (Spring Boot), one frontend (React). The earlier plan of a NestJS backend plus a second Java rebuild was dropped on 2026-11-22.

### 2.4 — Phased Build Plan with Acceptance Criteria

> Building in this order isn't arbitrary — each phase produces something that the *next* phase genuinely depends on, mirroring how a real product actually gets built (auth before anything user-scoped makes sense; tests from the start rather than bolted on; infra before scale-sensitive features).

**Phase 1 — Core API** **Tier: 🎯 Master**
Build (Spring Boot + Spring Data JPA): user registration/login, JWT + refresh tokens, basic CRUD on one core resource, PostgreSQL + Flyway migrations.
*Acceptance criteria:* a user can register, log in, receive a valid JWT, and perform CRUD on their own resources only (server-side ownership check present, Part IV 14.4) — and cannot access another user's resources by ID manipulation (a deliberate IDOR check).

**Phase 2 — Frontend** **Tier: 🎯 Master**
Build: React + TypeScript app consuming the API, real forms with validation, protected routes.
*Acceptance criteria:* a fresh clone of the repo, following the README, gets a new developer to a working local full-stack environment in under 15 minutes (this is itself a test of Part 0/Part V documentation discipline).

**Phase 3 — Testing From the Start** **Tier: 🎯 Master**
Build: unit tests (JUnit 5 + Mockito), integration tests (Testcontainers against real Postgres), a first Playwright E2E test for the critical login path.
*Acceptance criteria:* the test suite genuinely follows the pyramid shape (Part IV, 11.1) — many fast unit tests, some integration, one or two E2E — not inverted.

**Phase 4 — Infrastructure** **Tier: 🎯 Master**
Build: Docker Compose for local dev (Postgres + Redis + RabbitMQ), then a real GitHub Actions CI pipeline (lint → unit → integration → coverage gate → build).
*Acceptance criteria:* a deliberately broken PR (failing test, or coverage below threshold) is *actually* blocked from merging — verify this by opening one on purpose.

**Phase 5 — Advanced Backend** **Tier: 🎯 Master**
Build: RabbitMQ background jobs (Spring AMQP) with a genuinely idempotent handler and a dead letter queue, WebSocket-based real-time notifications, Redis-based distributed rate limiting.
*Acceptance criteria:* deliver the same message to a job handler twice and confirm idempotency (Part II, 5B.6); kill a WebSocket connection mid-session and confirm the heartbeat detects it (Part II, 7.3); confirm rate limiting is correctly per-user *and* per-IP with separate limits (Part IV, 14.9).

**Phase 6 — Deploy to AWS with Terraform** **Tier: 👀 Simplify — skip Terraform/full AWS, a single EC2/PaaS deploy is enough, see Priority_Map.md**
Build: the full stack provisioned via Terraform (VPC, EC2/ECS, RDS, S3, CloudFront), remote state with locking, GitHub Actions deploying automatically on merge to `main` with a required approval gate before production.
*Acceptance criteria:* `terraform destroy` followed by a fresh `terraform apply` fully reconstructs the environment from code alone — no manual console clicks required anywhere in the stack.

**Phase 7 — Monitoring** **Tier: 👀 Know Only — a basic Grafana dashboard is enough, don't over-invest in the full observability stack**
Build: Prometheus scraping Actuator metrics, Grafana dashboards using the RED method, Sentry error tracking, at least one alert tied to an explicit SLO you've written down.
*Acceptance criteria:* deliberately trigger the alert condition (e.g., force an elevated error rate) and confirm the alert actually fires — an untested alert is not a real safety net (directly echoing Part IV, 13.11's backup-testing principle).

**Phase 8 — AI Feature** **Tier: 🎯 Master**
Build: RAG-powered document search using pgvector — embed uploaded documents, retrieve relevant chunks at query time, stream an AI-generated answer grounded in the user's own data.
*Acceptance criteria:* ask a question with an answer *not* present in the uploaded documents and confirm the system doesn't confidently hallucinate an answer — it should indicate it doesn't know, or ground its answer visibly in retrieved context.

**Phase 9 — Billing** **Tier: 🎯 Master**
Build: Stripe integration with PaymentIntents, idempotency keys, and signature-verified webhook handling.
*Acceptance criteria:* replay the exact same webhook payload twice and confirm no duplicate charge or duplicate state change occurs (Part II, 5B.8's idempotency exercise, now against a real payment provider).

**Phase 10 — Mobile App** **Tier: ⏸️ Skip For Now — drop from the required build path per Priority_Map.md**
Build: React Native + Expo companion app — auth with `expo-secure-store`, offline support via TanStack Query's persisted cache, push notifications.
*Acceptance criteria:* force-quit the app while offline and relaunch it, confirming previously-fetched data still renders from cache rather than an error state (Part II, 6.8).

**Phase 11 — (removed)** The former "Track B Rebuild" phase no longer exists: the Master Project is built once, in Java + Spring Boot, in Phases 1–9 above.

### 2.5 — Definition of Done

The Master Project is "done" (in the sense that it's ready to represent you) when you can, without notes:
- Walk through the system diagram end to end and name every single point of failure and what happens when it fails (Part IV, 10.2).
- Explain the consistency-model choice for at least 3 different pieces of data in the system and why each is correct for that data (Part IV, 10.3).
- Show a real load test result with p50/p95/p99 latency and the specific resource that becomes the bottleneck at the breaking point (Part IV, 12.5).
- Point to one deliberately-introduced-then-fixed security vulnerability and explain both the exploit and the fix (Part IV, Phase 14 capstone).
- Show a real Grafana dashboard with a real alert that you've proven actually fires (Part IV, 13.9).
- Explain one Architecture Decision Record you wrote, including an alternative you seriously considered and rejected, and why (Part V, 16.2).

---

## 3 — Suggested Progression, with Checkpoints

> The master roadmap's stage table gives an ordering. This section adds honest self-assessment gates — the actual question to ask yourself before moving from one stage to the next, and what it looks like if you're not actually ready yet (as opposed to just being impatient to move on).

| Stage | Focus | **Checkpoint: Am I actually ready to move on?** |
|---|---|---|
| 1 | Foundation Reset (Part 0) | Can you explain, from raw memory, what happens from typing a URL to seeing a rendered page (Part I, 1.5) — every layer, not just "it loads"? If you have to look up any step, that is not a failure, it is a signal to stay here a bit longer. |
| 2 | Java fundamentals (5B.1) + DSA (Phase 2), together | Can you implement a hash map with separate chaining from scratch in Java, with no reference, in under 20 minutes? This is a genuine proxy for "the concept is internalized," not "I have read about it." |
| 3 | CS Fundamentals (Phase 1), interleaved with stage 2 | Can you explain why a `HashMap` lookup is fast in terms of what the CPU and cache actually do, and what the JVM does when you call `new`? |
| 4 | Software Design & Clean Code in Java (Phase 3) + Project Lifecycle | Look at a real class you wrote before this roadmap. Can you now name at least 2 real SOLID violations in it, specifically (not generically)? Bonus: can you point to where a Spring service you built applies Dependency Inversion the same way? |
| 5 | Spring Boot (5B.2+) with Databases (Phase 8), then API Design + Security | Have you run the 1M-row indexing exercise (Part III, 8.2) with your own real before/after numbers, and personally exploited then fixed a real vulnerability in your Spring app (not just read the OWASP list)? Reading is not the checkpoint here — doing is. |
| 6 | Frontend (Phase 4) | Can your React app consume your Java API end to end — auth, errors, pagination — without you hand-waving any step? |
| 7 | Testing + Performance + Docker/CI | Has your CI pipeline actually blocked a real bad merge at least once? If it has never fired, either your code has been suspiciously perfect, or the pipeline is not really gating anything yet. |
| 8 | System Design + AWS (lightened) | Can you design one common system (e.g. a URL shortener) on paper and defend every tradeoff — consistency, caching, failure modes? |
| 9 | Master Project | Can you satisfy every item in [2.5's Definition of Done](#25--definition-of-done) for the Java/Spring Boot build, without notes? |
| 10 | Mobile (React Native) — skipped by default | Does your mobile app correctly handle being offline, not just "usually work when there is a connection"? |
| 11 | Data Engineering awareness | Could you write a one-page design doc for what a data pipeline around your project would look like, even without building it (Part III, Phase 9 capstone)? |
| Ongoing | Senior mindset, communication, engineering practices | There is no checkpoint that says "done" here — see Section 4. |

**A general rule for every checkpoint above:** if you find yourself able to *describe* a concept fluently but you've never actually *reproduced* the failure mode it's protecting against (a race condition, an N+1 query, a deadlock, a cache stampede, an IDOR), you're not at the checkpoint yet. Across every Part file, the "Practice" sections were built specifically to force reproduction, not just explanation — that's not incidental, it's the actual mechanism by which this material sticks.

---

## 4 — The Final Synthesis: How to Know You're Actually Senior

> This roadmap explicitly has no finish line — but that's an unsatisfying answer if you're asking "so how do I know I'm making real progress?" Here's a more honest way to check.

### The single best test
Pick any "Answer This" question from any of the six part files, at random, without preparing. Answer it out loud, from memory, to an imagined skeptical colleague who will ask a genuine follow-up. If you can answer *and* handle a reasonable follow-up, that topic is solid. If you can recite the answer but freeze on the follow-up, you have a memorized fact, not understanding — go back and do the "Practice" exercise for that section for real, not just read it again.

### A cross-cutting checklist (pulling one representative thread from each Part)
- **Part 0** — Can you debug a real HTTP issue using only `curl -v` and DevTools' Network tab, without guessing?
- **Part I** — Can you explain why an `ArrayList` beats a `LinkedList` for iteration in terms of cache lines, not just "it's usually faster"?
- **Part II** — Can you point to a real N+1 query you fixed, with a real before/after query count?
- **Part III** — Can you read a real `EXPLAIN ANALYZE` plan and say, specifically, why the planner chose what it chose?
- **Part IV** — Can you name a specific single point of failure in a system you've built, and what you did about it (or consciously decided not to)?
- **Part V** — Can you produce a real ADR you wrote for a real decision, including the alternative you rejected?

If most of these are genuine "yes, and here's the specific example" rather than "yes, in theory" — that's real signal. If several are "in theory," that's not failure, it's just an accurate map of where to spend the next round of deliberate practice.

### What "senior" actually looks like, restated from Part V
Not someone who has memorized more facts than a mid-level engineer. Someone who, faced with an unfamiliar problem, reliably reaches for first principles (Part V, 15.2) instead of cargo-culting a familiar-looking fix; who can explain a tradeoff instead of asserting a best practice (Part V, 15.4); who writes code and systems that the *next* person — including future-you — can safely change; and who makes the people around them measurably more effective, not just themselves.

---

## 5 — Turning This Into a Portfolio

> A brief, practical addendum — the Master Project is only valuable externally if it's presented well.

- **The README is not an afterthought.** It should explain *why* the project exists, the real architectural decisions (link to your ADRs), and include actual screenshots/diagrams — not just "clone and run" instructions.
- **Keep your ADRs in the repo, publicly.** A hiring engineer skimming `docs/adr/` and seeing genuine tradeoff reasoning is far more convincing than a polished UI alone — it's direct evidence of Part V's thinking, not just Part II's execution.
- **Have the load-test numbers ready.** "It handles X req/s at p99 under Yms, and the bottleneck at failure was Z" is a concrete, defensible claim (Part IV, Phase 12) — vastly more credible than "it's fast."
- **Have the vulnerability story ready.** The deliberately-introduced-then-fixed security bug from Phase 14's capstone is genuinely good interview material — it demonstrates you understand an attack from the inside, not just that you can recite the OWASP Top 10.
- **Be honest about scope.** Referring back to [2.1's explicit non-goals](#21--vision--scope) in an interview ("I deliberately didn't build X because it wasn't the point of this exercise") reads as senior-level scoping discipline (Part V, 15.1) — far better than apologizing for what's missing.

---

> This closes the six-part detailed companion to `Master_Senior_Fullstack_Roadmap.md`. The master file remains the map; these six files are the terrain. Return to any section, at any point, once real work surfaces a gap — that's not a sign you did the roadmap "wrong" the first time. It's the actual, intended way this material gets learned.
