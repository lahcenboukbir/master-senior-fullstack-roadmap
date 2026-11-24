# Priority Map — Master / Know-Only / Skip-for-Now
### How to actually use this roadmap to become a senior fullstack web developer who deeply understands *why*, *how*, and *when* — not five careers stapled together

> **Review log (2026-11-22):** every Part and Phase below was re-reviewed section by section. Net changes: 0.3 IDE Mastery deleted; Node/NestJS track deleted (Java is the only backend); 1.4 → Know Only; 10.4 and 12.2 → Master; 13.5 Kubernetes, 13.6 Terraform and the Phase 14 ecosystem block → Know Only. Everything else was confirmed as tagged.
>
> **Original ground rule: nothing gets deleted** (relaxed on 2026-11-22 — deletions are now allowed when you decide so explicitly, and everything stays recoverable from git history). Every phase and subsection across all six detail files stays exactly as written. This file is a lens you look at them through, not an edit to them. Its only job is to answer one question honestly, for every single subsection you already have: *is this a fundamental — the "why/how/when" underneath everything you build — or is it a genuinely advanced, specialist, or enterprise-scale concern you can pick up later when a real project calls for it?*
>
> **This map has no deadline behind it.** You're not optimizing for fastest-employable or trimming hours off a schedule — you're optimizing for actually understanding the machine, the language, the data structures, and the systems you work with, not just calling the right library function. That changes the tiering: **fundamentals and core mechanics default to Master**, even the ones that don't show up in daily ticket work. What gets deprioritized is genuinely advanced, niche, or enterprise/platform-team territory (Kubernetes, Terraform, distributed consensus algorithms, DDD strategic design, FinOps, chaos engineering) — not "how a CPU works" or "how TCP actually behaves," which are exactly the kind of fundamentals that turn a coder into an engineer.
>
> **The three tiers:**
> - 🎯 **MASTER** — do every Practice exercise, answer every "Answer This" question from memory, use spaced repetition (per `How_To_Learn_This_Roadmap.md`). This is both your daily employable skillset **and** the fundamentals that explain why it works.
> - 👀 **KNOW ONLY** — read the subsection once, understand the vocabulary and the shape of the problem it solves, skip most of the Practice exercises. Reserved for things that are genuinely advanced, specialist, or situational — not for basics that happen to not show up in daily ticket work. Revisit with real depth once a real project or genuine curiosity pulls you there.
> - ⏸️ **SKIP FOR NOW** — don't even read it yet. It's not wasted work — it's already written and waiting for the day a real need, a curious weekend, or a career pivot actually calls for it.
>
> **One honest caveat before the tables:** a chunk of what's tagged "Know Only" or "Skip" below (deep distributed-systems theory, advanced DDD, platform/infra depth) shows up disproportionately in **interview processes**, especially at larger or interview-heavy companies. If you're specifically targeting that kind of company, bump those sections back up before an interview cycle.

---

## Part 0 — Foundation Reset

| Section | Tier | Why |
|---|---|---|
| 0.1 Linux as Daily Driver | 🎯 MASTER | Cheap, foundational, you'll live in a terminal regardless of specialization |
| 0.2 Terminal Mastery | 🎯 MASTER | Daily-use skill, high leverage relative to effort |
| 0.3 IDE Mastery | 🗑️ REMOVED | Deleted from the roadmap on 2026-11-22 — you chose to learn your editor on the job; numbering of 0.4–0.9 kept stable |
| 0.4 Chrome DevTools Mastery | 🎯 MASTER | Non-negotiable for fullstack web debugging |
| 0.5 Git Internals & Discipline | 🎯 MASTER | Every job, every day |
| 0.6 HTML Semantics | 🎯 MASTER | Core web fundamentals |
| 0.7 CSS Fundamentals | 🎯 MASTER | Core web fundamentals |
| 0.8 How HTTP Really Works | 🎯 MASTER | Everything you build sits on this |
| 0.9 Environment & Habits Checklist | 🎯 MASTER | Free — just habits |

**Part 0 reviewed section by section on 2026-11-22: everything stays Master except 0.3 IDE Mastery, which was deleted.** It's small and it's the floor everything else stands on.

---

## Part I — Core Engineering Foundations

### Phase 1 — Computer Science Fundamentals

| Section | Tier | Why |
|---|---|---|
| 1.1 How a CPU Works | 🎯 MASTER | This is "how a computer actually works" — fetch-decode-execute, binary/hex, IEEE 754 — exactly the kind of fundamental that explains WHY code behaves the way it does |
| 1.2 Memory Hierarchy | 🎯 MASTER | Directly explains real performance bugs you'll actually debug |
| 1.3 Operating Systems Fundamentals | 🎯 MASTER | Concurrency, GC, deadlocks — daily-relevant for Java backend work |
| 1.4 Networking Deep Dive | 👀 KNOW ONLY | HTTP/DNS/TLS/CORS basics are covered at Master in 0.8 already — this is the layer underneath: TCP handshake, congestion control, DNS resolution, epoll. Core "how the internet actually works," not infra-specialist trivia |
| 1.5 How the Browser Works | 🎯 MASTER | Explains frontend performance work you'll actually do |
| 1.6 Compilers, Interpreters & the JVM (general mechanics) | 🎯 MASTER | Lexing/parsing/AST/JIT is how your source code becomes execution — a real fundamental. The JVM-specific flags/tuning stay lighter-touch, but the general mechanics are core |

### Phase 2 — Data Structures & Algorithms

| Section | Tier | Why |
|---|---|---|
| 2.1 Complexity Analysis | 🎯 MASTER | The universal vocabulary for every tradeoff discussion |
| 2.2 Arrays & Strings | 🎯 MASTER | Constant real use |
| 2.3 Linked Lists | 🎯 MASTER | Foundational, common interview topic |
| 2.4 Stacks & Queues | 🎯 MASTER | `ArrayDeque` shows up constantly in real code |
| 2.5 Hash Maps & Hash Sets | 🎯 MASTER | You use `HashMap` every single day — know what it's actually doing |
| 2.6 Trees | 🎯 MASTER | BSTs/B-trees underpin databases you'll use constantly (Phase 8) |
| 2.7 Heaps | 🎯 MASTER | Core structure — build one from scratch at least once even though `PriorityQueue` covers daily use |
| 2.8 Graphs | 🎯 MASTER | BFS/DFS, topological sort, cycle detection are core algorithmic thinking tools, not a fullstack-CRUD niche |
| 2.9 Tries | ⏸️ SKIP FOR NOW | Niche, autocomplete-specific |
| 2.10 Union-Find | ⏸️ SKIP FOR NOW | Competitive-programming territory |
| 2.11 Sorting Algorithms | 🎯 MASTER | Understanding tradeoffs matters even though libraries do the sorting |
| 2.12 Searching & Shortest Paths | 🎯 MASTER | Binary search is essential; Dijkstra/Bellman-Ford/A\* is the lighter part of this one |
| 2.13 Dynamic Programming | 🎯 MASTER | This *is* the "recognize the pattern, reason from first principles" skill — the deep problem-solving thinking this whole roadmap is meant to build, not just an interview trick |
| 2.14 Advanced & Niche DS&A | ⏸️ SKIP FOR NOW | Segment trees, string matching, comp geometry — genuinely specialist |
| 2.15 Problem-Solving Patterns Reference | 🎯 MASTER | Your cheat sheet — cheap and high-value to internalize |

### Phase 3 — Software Design & Clean Code

| Section | Tier | Why |
|---|---|---|
| 3.1 Clean Code Principles | 🎯 MASTER | This is what separates good code from bad code, every day |
| 3.2 SOLID Principles | 🎯 MASTER | Directly shapes how you write Spring Boot / React code well |
| 3.3 Design Patterns | 🎯 MASTER | Creational/Structural/Behavioral are used constantly — Distributed patterns (Saga, Outbox, Event Sourcing, CQRS) subset is 👀 Know Only |
| 3.4 Architecture Patterns | 🎯 MASTER | Layered/Clean Architecture is your default — full DDD strategic depth subset is 👀 Know Only (basics only needed) |
| 3.5 Refactoring | 🎯 MASTER | You'll do this every week of your career |

**Also 🎯 MASTER, standalone file:** `Project_Lifecycle_And_Planning.md` — briefs, user stories, C4/ERD/sequence diagramming via Mermaid, and running your own backlog/sprints. This isn't part of a numbered phase, but it's exactly the "how do I actually run a project, not just write code for one" skill — read it once after Phase 3, apply it before starting the Master Project's build.

---

## Part II — Building Software

### Phase 4 — Frontend Engineering

**All of 4.1–4.9 stays 🎯 MASTER.** This is literally half of "fullstack." No cuts here.

### Phase 5 — Backend Engineering (Java + Spring Boot — the single backend language)

> **Decision (2026-11-22): one backend language — Java.** Depth in one ecosystem beats breadth across three; TypeScript is kept for the frontend only. Java fundamentals are studied *together with* Phase 2 (DSA, practiced in Java) and Phase 1 (CPU/memory/OS/JVM); Spring Boot is studied *with* Phase 8 (databases). The earlier Node.js/NestJS track was **deleted** on 2026-11-22. Section IDs stay stable: the sections are still numbered 5B (there is no 5A any more).

**Java + Spring Boot (the only backend track)**

| Section | Tier | Why |
|---|---|---|
| 5B.1 Java Fundamentals (Deep) | 🎯 MASTER | Your only backend language — OOP, generics, collections internals, concurrency, modern Java 8–21. Also the vehicle for all DSA practice |
| 5B.2 Spring Boot Core Concepts | 🎯 MASTER | DI/IoC, Spring Web, validation, DTOs, error handling — the framework you will use daily |
| 5B.3 Spring Data JPA | 🎯 MASTER | Study alongside Phase 8 so you understand what the database is doing under the ORM; N+1, transactions, Flyway |
| 5B.4 Spring Security | 🎯 MASTER | Every real app needs auth; pairs with Phase 14 |
| 5B.5 Spring Boot Production Features | 🎯 MASTER | Profiles, Actuator, Micrometer, structured logging, graceful shutdown |
| 5B.6 Async & Background Jobs | 🎯 MASTER | @Async, @Scheduled, RabbitMQ + Spring AMQP, idempotency |
| 5B.7 Testing Spring Boot | 🎯 MASTER | JUnit 5, Mockito, MockMvc, Testcontainers — non-negotiable |
| 5B.8 Backend Production Concerns | 👀 KNOW ONLY | Webhooks/payments/multi-tenancy/search — high-value but situational; go deep the day a real project needs one specifically |

### Phase 6 — Mobile Engineering (React Native)

**All of 6.1–6.9 → ⏸️ SKIP FOR NOW.** You said fullstack *web*. This entire phase waits until (if ever) that changes.

### Phase 7 — API Design

| Section | Tier | Why |
|---|---|---|
| 7.1 REST Principles (Deep) | 🎯 MASTER | Your default API style |
| 7.2 REST Best Practices | 🎯 MASTER | Pagination, error envelopes — daily craft |
| 7.3 WebSockets | 🎯 MASTER | Real-time features are common in modern fullstack apps |
| 7.4 API Security | 🎯 MASTER | Non-negotiable |
| 7.5 API Documentation | 🎯 MASTER | OpenAPI/Swagger, expected professional practice |
| 7.6 Beyond REST | 👀 KNOW ONLY | GraphQL/gRPC/tRPC/AsyncAPI — recognize them, don't build fluency yet |

---

## Part III — Data

### Phase 8 — Databases

| Section | Tier | Why |
|---|---|---|
| 8.1 How Databases Actually Work | 🎯 MASTER | Explains every "why is this slow" moment |
| 8.2 Indexing | 🎯 MASTER | The single highest-leverage DB topic there is |
| 8.3 ACID & Transactions | 🎯 MASTER | Correctness bugs live here |
| 8.4 Query Optimization | 🎯 MASTER | You'll do this constantly |
| 8.5 PostgreSQL in Depth | 🎯 MASTER | Your primary database |
| 8.6 Redis Beyond Caching | 🎯 MASTER | Rate limiting, sessions, pub/sub — real daily use |
| 8.7 Scaling Databases | 👀 KNOW ONLY | Sharding/replica strategy is usually a platform team's job, not yours |
| 8.8 The Broader Database Landscape | 👀 KNOW ONLY | Recognize when Mongo/Cassandra/Neo4j is the right call, don't need production depth in all of them |

### Phase 9 — Data Engineering (Awareness)

**All of 9.1–9.5 stays 👀 KNOW ONLY** — this was already designed as an awareness-level phase in the master roadmap; no change needed, it's correctly scoped already.

---

## Part IV — Systems at Scale

### Phase 10 — System Design & Architecture

| Section | Tier | Why |
|---|---|---|
| 10.1 The Foundation Questions | 🎯 MASTER | Cheap mental model, useful in every design conversation |
| 10.2 Scalability & Availability | 🎯 MASTER | Conceptual fluency expected of any senior engineer |
| 10.3 Consistency Models | 🎯 MASTER | Directly relevant to real data decisions you'll make |
| 10.4 Load Balancing | 🎯 MASTER | Promoted on 2026-11-22 — worth real depth even if you rarely configure it yourself |
| 10.5 Caching Architecture | 🎯 MASTER | Extremely practical, directly usable |
| 10.6 Asynchronous Communication | 👀 KNOW ONLY | Practical RabbitMQ side already Mastered in 5.6 — this is the deeper theory layer |
| 10.7 Microservices vs Monolith | 🎯 MASTER | You'll be part of this decision even as an IC |
| 10.8 Distributed Systems Fundamentals | 👀 KNOW ONLY | Promoted from Skip (2026-11-24): idempotency, retry+backoff+jitter, circuit breaker and Redis-lock caveats are everyday production code; consensus/2PC/TCC stay awareness-only |
| 10.9 Common Systems to Design | 👀 KNOW ONLY | Valuable for interview prep specifically — bump up if interviewing at system-design-heavy companies |

### Phase 11 — Testing & Quality Engineering

| Section | Tier | Why |
|---|---|---|
| 11.1 The Testing Pyramid | 🎯 MASTER | Foundational mental model |
| 11.2 Test-Driven Development | 🎯 MASTER | Real, usable daily discipline |
| 11.3 Unit Testing (Deep) | 🎯 MASTER | Daily craft |
| 11.4 Integration & E2E Testing | 🎯 MASTER | Daily craft |
| 11.5 Performance/Load Testing | 👀 KNOW ONLY | Useful conceptually; deep k6 mastery only if a job specifically needs it |
| 11.6 Beyond the Basics | 👀 KNOW ONLY | Promoted from Skip (2026-11-24): read once; optionally do Practice 3 (PIT mutation testing — cheapest, most instructive). Pact and chaos engineering stay awareness-only |
| 11.7 CI/CD Testing Pipeline | 🎯 MASTER | Expected professional practice |

### Phase 12 — Performance Engineering

| Section | Tier | Why |
|---|---|---|
| 12.0 The Golden Rule | 🎯 MASTER | Cheap, universal mindset |
| 12.1 Database Performance | 🎯 MASTER | Reinforces Phase 8 — the highest-value performance work you'll do |
| 12.2 Java / Spring Boot Performance | 🎯 MASTER | Promoted on 2026-11-22 — Java is your only backend, so JVM/GC/virtual-thread depth is worth drilling |
| 12.3 Frontend Performance | 🎯 MASTER | Reinforces Phase 4 |
| 12.4 Caching & Infrastructure Performance | 👀 KNOW ONLY | Conceptual fluency is enough |
| 12.5 Load Testing | 👀 KNOW ONLY | Same as 11.5 |

### Phase 13 — DevOps & Infrastructure — *the biggest cut, as requested*

| Section | Tier | Why |
|---|---|---|
| 13.1 Git Mastery (Team Context) | 🎯 MASTER | This is just Git, not really "DevOps" — everyone needs this |
| 13.2 Linux & Bash for Operations | 👀 KNOW ONLY | Basics already Mastered in Part 0; `strace`/eBPF-level tooling is ops-specialist |
| 13.3 Containerization: Docker | 🎯 MASTER | Genuinely essential even for a non-DevOps fullstack dev — local dev + understanding your own deploy |
| 13.4 CI/CD: GitHub Actions | 🎯 MASTER | A basic lint→test→build→deploy pipeline is table stakes for a senior dev |
| 13.5 Container Orchestration: Kubernetes | 👀 KNOW ONLY | Promoted from Skip on 2026-11-22 — read once, recognize it; not drilled, not required for the Master Project |
| 13.6 Infrastructure as Code: Terraform | 👀 KNOW ONLY | Promoted from Skip on 2026-11-22 — read once, recognize it; the Master Project still uses a simple deploy |
| 13.7 AWS Core Services | 👀 KNOW ONLY | Know what EC2/S3/RDS *are* conceptually; deep IAM/VPC/ECS design is not your job |
| 13.8 Web Servers & Proxies | 👀 KNOW ONLY | Recognize what Nginx is doing, rarely configure it yourself |
| 13.9 Monitoring & Observability | 👀 KNOW ONLY | Actuator itself is already Mastered in 5.5 — building the Prometheus/Grafana stack from scratch is not |
| 13.10 Deployment Strategies | 👀 KNOW ONLY | Conceptual fluency for architecture conversations |
| 13.11 Backups & Cron | 🎯 MASTER | Promoted from Skip (2026-11-24): tiny section, real payoff — an untested backup isn't a backup, and every backend ends up with cron jobs |

### Phase 14 — Security Fundamentals

| Section | Tier | Why |
|---|---|---|
| 14.1 Security Mindset | 🎯 MASTER | Non-negotiable for anyone shipping web features |
| 14.2 OWASP Top 10 | 🎯 MASTER | Non-negotiable |
| 14.3 Authentication Deep Dive | 🎯 MASTER | You will build auth systems regularly |
| 14.4 Authorization | 🎯 MASTER | IDOR is one of the most common real bugs — non-negotiable |
| 14.5 Common Attack Vectors | 🎯 MASTER | Non-negotiable |
| 14.6 Security Headers | 🎯 MASTER | Cheap, high-value, every app needs these |
| 14.7 Cryptography Fundamentals | 🎯 MASTER | You'll use libraries, not implement crypto yourself — but understanding WHY Argon2 beats MD5, WHY symmetric vs asymmetric keys work the way they do, is the fundamental this roadmap is about, not just library trivia |
| 14.8 Secrets Management | 🎯 MASTER | Practical, needed for any real deployment |
| 14.9 API Security | 🎯 MASTER | Non-negotiable |
| Ecosystem Awareness (Zero Trust/WAF/SIEM/pentest/compliance) | 👀 KNOW ONLY | Promoted from Skip on 2026-11-22 — one read-through for the vocabulary |

---

## Part V — Becoming Senior

### Phase 15 — The Senior Mindset & Problem Solving
**All of 15.1–15.6 stays 🎯 MASTER.** This is nearly free (no infrastructure, no tools) and it's what actually makes you senior rather than just experienced.

### Phase 16 — Communication & Collaboration
**All of 16.1–16.6 stays 🎯 MASTER.** Same reasoning — free, and a massive multiplier on everything else.

### Phase 17 — Engineering Practices & Methodologies

| Section | Tier | Why |
|---|---|---|
| 17.1 Agile / Scrum / Kanban / XP | 🎯 MASTER | You'll be in these ceremonies regardless of whether you like them |
| 17.2 Lean / Lean Startup | 👀 KNOW ONLY | Useful mindset, not a drilled skill |
| 17.3 Domain-Driven Design — Deep Dive | 👀 KNOW ONLY | Basics already Mastered in 3.4; full strategic DDD is architect-level |
| 17.4 The 12-Factor App | 🎯 MASTER | Short, extremely practical, directly shapes how you build deployable apps |
| 17.5 Technical Debt Quadrant | 🎯 MASTER | Short, high-value mental model |
| 17.6 Spikes, OKRs & FinOps | 👀 KNOW ONLY | Spikes/OKRs are useful vocabulary — **FinOps specifically drops toward skip**, it's genuinely a specialist concern |

---

## Part VI — Putting It Together

Part VI isn't phase-tagged the same way (it's reference/synthesis material), so here's direct guidance instead:

- **Tech Stack Reference** — stays 🎯 MASTER as your reference; when it lists ecosystem-awareness alternatives, those follow the same tiering as the phase they came from above.
- **The Master Project** — stays 🎯 MASTER as your capstone, but **use the trimmed scope below**, not the original full spec.
- **Suggested Progression** — stays 🎯 MASTER as a guide, with Stage 7 (Mobile) removed from your critical path, and Stage 5's AWS/Terraform expectations lightened to match Phase 13's new tiering.

### Trimmed Master Project Scope
Keep: auth (JWT + refresh + Argon2 + MFA), API key management, Redis rate limiting, RabbitMQ background jobs, WebSocket notifications, pgvector RAG search, Grafana dashboard (basic), Stripe billing, row-level-security multi-tenancy, full test suite, CI pipeline (GitHub Actions), Docker Compose locally, and a **simple deploy** — a single EC2 instance or a PaaS (Railway/Render/Fly.io) is genuinely fine here.
Drop from the required path: the React Native mobile app (Phase 6, skipped), the full Terraform-provisioned multi-service AWS stack (Phase 13.6, Know Only — read, not built), Kubernetes (Phase 13.5, Know Only — read, not built).

---

## What Actually Got Deprioritized, Honestly

This isn't a time-cutting exercise — you have no deadline, so total hours isn't the metric that matters here. What this map actually does is separate **what a senior fullstack web developer needs to deeply understand** from **what belongs to adjacent specialties** (platform/DevOps engineering, mobile engineering, distributed-systems architecture, security research). Fundamentals — CS internals, networking, core DSA, core algorithmic thinking, cryptography concepts — stay at Master regardless of whether you'll type them into a ticket next week, because understanding WHY is the whole point.

What's genuinely deprioritized to 👀 Know Only or ⏸️ Skip is specialist/adjacent-career territory, not basics:
- **Phase 6** — the entire React Native mobile track (a different specialization: mobile engineering, not web)
- **Phase 13** — Kubernetes, Terraform, and deep AWS service design (platform/DevOps-engineer territory, not fullstack-dev-who-understands-deployment territory)
- ~~**10.8**~~ — promoted to 👀 Know Only on 2026-11-24 (see table)
- **17.3 / 17.6** — full DDD strategic design and FinOps (architect/cost-engineering specialties)

This is an honest "senior fullstack web developer who understands the fundamentals deeply" curriculum — not five careers stapled together, and not a speed-run either.

---

## How to Actually Use This With `How_To_Learn_This_Roadmap.md`

- Your **main study track** (the interleaved, spaced-repetition, Practice-exercise-driven system from that file) applies to 🎯 Master sections only.
- 👀 Know Only sections are a single pass each — read, don't drill, don't feel obligated to do the Practice exercises or add them to a spaced-repetition deck. If you catch yourself over-investing in a Know Only section "just to be thorough," that's the completionist instinct from the earlier conversation creeping back in — notice it and move on.
- ⏸️ Skip sections don't need a session at all right now. They're not a debt you owe — they're a shelf you can reach for later.
- Revisit this map itself every few months. Your actual job, the projects you end up on, and your own curiosity will legitimately promote some 👀/⏸️ items back up to 🎯 over time — that's the system working correctly, not a sign this map was wrong.
