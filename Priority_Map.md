# Priority Map — Master / Know-Only / Skip-for-Now
### How to actually use this roadmap to become a senior fullstack web developer — not a distributed-systems staff engineer, not a DevOps/SRE, not a security researcher

> **Ground rule: nothing gets deleted.** Every phase and subsection across all six detail files stays exactly as written. This file is a lens you look at them through, not an edit to them. Its only job is to answer one question honestly, for every single subsection you already have: *does a senior fullstack web developer need to drill this, or just recognize it when it shows up?*
>
> **The three tiers:**
> - 🎯 **MASTER** — do every Practice exercise, answer every "Answer This" question from memory, use spaced repetition (per `How_To_Learn_This_Roadmap.md`). This is your actual, daily, employable skillset.
> - 👀 **KNOW ONLY** — read the subsection once, understand the vocabulary and the shape of the problem it solves, skip most of the Practice exercises. Revisit with real depth only when a real project or job forces you to — not on a schedule.
> - ⏸️ **SKIP FOR NOW** — don't even read it yet. It's not wasted work — it's already written and waiting for the day a real need, a curious weekend, or a career pivot actually calls for it.
>
> **One honest caveat before the tables:** a chunk of what's tagged "Know Only" or "Skip" below (DSA graph/DP depth, system design, some distributed-systems theory) shows up disproportionately in **interview processes**, especially at larger or interview-heavy companies, independent of whether the job itself uses it daily. If you're specifically targeting that kind of company, bump those sections back up before an interview cycle — this map optimizes for *the job*, not *the gauntlet to get the job*, and those aren't always the same list.

---

## Part 0 — Foundation Reset

| Section | Tier | Why |
|---|---|---|
| 0.1 Linux as Daily Driver | 🎯 MASTER | Cheap, foundational, you'll live in a terminal regardless of specialization |
| 0.2 Terminal Mastery | 🎯 MASTER | Daily-use skill, high leverage relative to effort |
| 0.3 IDE Mastery | 🎯 MASTER | Where you spend 80% of your hours |
| 0.4 Chrome DevTools Mastery | 🎯 MASTER | Non-negotiable for fullstack web debugging |
| 0.5 Git Internals & Discipline | 🎯 MASTER | Every job, every day |
| 0.6 HTML Semantics | 🎯 MASTER | Core web fundamentals |
| 0.7 CSS Fundamentals | 🎯 MASTER | Core web fundamentals |
| 0.8 How HTTP Really Works | 🎯 MASTER | Everything you build sits on this |
| 0.9 Environment & Habits Checklist | 🎯 MASTER | Free — just habits |

**All of Part 0 stays Master.** It's small and it's the floor everything else stands on.

---

## Part I — Core Engineering Foundations

### Phase 1 — Computer Science Fundamentals

| Section | Tier | Why |
|---|---|---|
| 1.1 How a CPU Works | 👀 KNOW ONLY | Good mental model, not something you'll drill or use daily |
| 1.2 Memory Hierarchy | 🎯 MASTER | Directly explains real performance bugs you'll actually debug |
| 1.3 Operating Systems Fundamentals | 🎯 MASTER | Concurrency, GC, deadlocks — daily-relevant for Java backend work |
| 1.4 Networking Deep Dive | 👀 KNOW ONLY | You need HTTP/DNS/TLS/CORS practically (already covered at Master in 0.8) — TCP internals, `epoll`, zero-copy is infra-engineer territory |
| 1.5 How the Browser Works | 🎯 MASTER | Explains frontend performance work you'll actually do |
| 1.6 Compilers, Interpreters & the JVM | 👀 KNOW ONLY | Nice intuition, not something to drill regularly |

### Phase 2 — Data Structures & Algorithms

| Section | Tier | Why |
|---|---|---|
| 2.1 Complexity Analysis | 🎯 MASTER | The universal vocabulary for every tradeoff discussion |
| 2.2 Arrays & Strings | 🎯 MASTER | Constant real use |
| 2.3 Linked Lists | 🎯 MASTER | Foundational, common interview topic |
| 2.4 Stacks & Queues | 🎯 MASTER | `ArrayDeque` shows up constantly in real code |
| 2.5 Hash Maps & Hash Sets | 🎯 MASTER | You use `HashMap` every single day — know what it's actually doing |
| 2.6 Trees | 🎯 MASTER | BSTs/B-trees underpin databases you'll use constantly (Phase 8) |
| 2.7 Heaps | 👀 KNOW ONLY | You'll use `PriorityQueue`, rarely build one from scratch |
| 2.8 Graphs | 👀 KNOW ONLY | BFS/DFS concept is useful; deep graph algorithms rarely needed in CRUD fullstack work |
| 2.9 Tries | ⏸️ SKIP FOR NOW | Niche, autocomplete-specific |
| 2.10 Union-Find | ⏸️ SKIP FOR NOW | Competitive-programming territory |
| 2.11 Sorting Algorithms | 🎯 MASTER | Understanding tradeoffs matters even though libraries do the sorting |
| 2.12 Searching & Shortest Paths | 🎯 MASTER | Binary search is essential; Dijkstra/Bellman-Ford/A\* is the lighter part of this one |
| 2.13 Dynamic Programming | 👀 KNOW ONLY | Rarely used directly in fullstack CRUD work — **bump to Master if interviewing at DSA-heavy companies** |
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

---

## Part II — Building Software

### Phase 4 — Frontend Engineering

**All of 4.1–4.9 stays 🎯 MASTER.** This is literally half of "fullstack." No cuts here.

### Phase 5 — Backend Engineering (Two Tracks: JS Now, Java Later)

> **Sequencing decision, not a scope cut:** the plan is full-stack JS (React + Node/NestJS) first — this covers freelance work and lets you ship real projects fastest with one language end to end — then Java + Spring Boot once JS is solid, aimed specifically at the professional/enterprise job market. Track B isn't neglected, it's deliberately queued. NestJS was chosen over bare Express as the primary JS framework specifically because its architecture (DI, modules, decorators, guards/pipes/interceptors) pre-trains the same mental model Spring Boot uses — Track A is a rehearsal for Track B, not a detour from it.

**Track A — Node.js + NestJS (learn this first)**

| Section | Tier | Why |
|---|---|---|
| 5A.1 Node.js Fundamentals (Deep) | 🎯 MASTER | Your primary runtime, right now |
| 5A.2 Express Fundamentals | 🎯 MASTER | Brief but necessary — understand what Nest abstracts before trusting the abstraction |
| 5A.3 NestJS Core Concepts | 🎯 MASTER | Your primary framework, right now — and your rehearsal for Spring Boot's DI/annotation model |
| 5A.4 Prisma & the Data Layer | 🎯 MASTER | You'll write this every day |
| 5A.5 Auth & Security in NestJS | 🎯 MASTER | Every real app needs auth |
| 5A.6 NestJS Production Features | 🎯 MASTER | Config, logging, health checks — real deployment basics |
| 5A.7 Async & Background Jobs (BullMQ) | 🎯 MASTER | Common real-world need |
| 5A.8 Testing NestJS | 🎯 MASTER | Non-negotiable |
| 5A.9 Backend Production Concerns | 👀 KNOW ONLY | Webhooks/payments/multi-tenancy/search — high-value but situational; go deep the day a real project needs one specifically |

**Track B — Java + Spring Boot (deferred until after the Track A capstone ships)**

| Section | Tier | Why |
|---|---|---|
| 5B.1 Java Fundamentals (Deep) | ⏸️ SKIP FOR NOW | Deferred by design — this is the professional-job-market track, tackled after JS is solid, not neglected |
| 5B.2 Spring Boot Core Concepts | ⏸️ SKIP FOR NOW | Same — revisit once the NestJS capstone is done |
| 5B.3 Spring Data JPA | ⏸️ SKIP FOR NOW | Same |
| 5B.4 Spring Security | ⏸️ SKIP FOR NOW | Same |
| 5B.5 Spring Boot Production Features | ⏸️ SKIP FOR NOW | Same |
| 5B.6 Async & Background Jobs | ⏸️ SKIP FOR NOW | Same |
| 5B.7 Testing Spring Boot | ⏸️ SKIP FOR NOW | Same |
| 5B.8 Backend Production Concerns | ⏸️ SKIP FOR NOW | Same — was already Know Only even at Master-tier timing, now simply later |

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
| 10.4 Load Balancing | 👀 KNOW ONLY | Recognize the concepts, rarely configure this yourself |
| 10.5 Caching Architecture | 🎯 MASTER | Extremely practical, directly usable |
| 10.6 Asynchronous Communication | 👀 KNOW ONLY | Practical RabbitMQ side already Mastered in 5.6 — this is the deeper theory layer |
| 10.7 Microservices vs Monolith | 🎯 MASTER | You'll be part of this decision even as an IC |
| 10.8 Distributed Systems Fundamentals | ⏸️ SKIP FOR NOW | 2PC/Saga/consensus/TCC — genuine distributed-systems specialty |
| 10.9 Common Systems to Design | 👀 KNOW ONLY | Valuable for interview prep specifically — bump up if interviewing at system-design-heavy companies |

### Phase 11 — Testing & Quality Engineering

| Section | Tier | Why |
|---|---|---|
| 11.1 The Testing Pyramid | 🎯 MASTER | Foundational mental model |
| 11.2 Test-Driven Development | 🎯 MASTER | Real, usable daily discipline |
| 11.3 Unit Testing (Deep) | 🎯 MASTER | Daily craft |
| 11.4 Integration & E2E Testing | 🎯 MASTER | Daily craft |
| 11.5 Performance/Load Testing | 👀 KNOW ONLY | Useful conceptually; deep k6 mastery only if a job specifically needs it |
| 11.6 Beyond the Basics | ⏸️ SKIP FOR NOW | Mutation testing, chaos engineering, property-based testing — rare in typical fullstack jobs |
| 11.7 CI/CD Testing Pipeline | 🎯 MASTER | Expected professional practice |

### Phase 12 — Performance Engineering

| Section | Tier | Why |
|---|---|---|
| 12.0 The Golden Rule | 🎯 MASTER | Cheap, universal mindset |
| 12.1 Database Performance | 🎯 MASTER | Reinforces Phase 8 — the highest-value performance work you'll do |
| 12.2 Java / Spring Boot Performance | 👀 KNOW ONLY | JVM/GC tuning depth is specialist territory until you hit a real wall |
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
| 13.5 Container Orchestration: Kubernetes | ⏸️ SKIP FOR NOW | Platform/DevOps-engineer territory |
| 13.6 Infrastructure as Code: Terraform | ⏸️ SKIP FOR NOW | Same |
| 13.7 AWS Core Services | 👀 KNOW ONLY | Know what EC2/S3/RDS *are* conceptually; deep IAM/VPC/ECS design is not your job |
| 13.8 Web Servers & Proxies | 👀 KNOW ONLY | Recognize what Nginx is doing, rarely configure it yourself |
| 13.9 Monitoring & Observability | 👀 KNOW ONLY | Actuator itself is already Mastered in 5.5 — building the Prometheus/Grafana stack from scratch is not |
| 13.10 Deployment Strategies | 👀 KNOW ONLY | Conceptual fluency for architecture conversations |
| 13.11 Backups & Cron | ⏸️ SKIP FOR NOW | Ops-team territory |

### Phase 14 — Security Fundamentals

| Section | Tier | Why |
|---|---|---|
| 14.1 Security Mindset | 🎯 MASTER | Non-negotiable for anyone shipping web features |
| 14.2 OWASP Top 10 | 🎯 MASTER | Non-negotiable |
| 14.3 Authentication Deep Dive | 🎯 MASTER | You will build auth systems regularly |
| 14.4 Authorization | 🎯 MASTER | IDOR is one of the most common real bugs — non-negotiable |
| 14.5 Common Attack Vectors | 🎯 MASTER | Non-negotiable |
| 14.6 Security Headers | 🎯 MASTER | Cheap, high-value, every app needs these |
| 14.7 Cryptography Fundamentals | 👀 KNOW ONLY | You'll use libraries, not implement crypto — conceptual fluency is enough |
| 14.8 Secrets Management | 🎯 MASTER | Practical, needed for any real deployment |
| 14.9 API Security | 🎯 MASTER | Non-negotiable |
| Ecosystem Awareness (Zero Trust/WAF/SIEM/pentest/compliance) | ⏸️ SKIP FOR NOW | Genuine security-specialist territory |

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
Drop from the required path: the React Native mobile app (Phase 6, skipped), the full Terraform-provisioned multi-service AWS stack (Phase 13.6, skipped), Kubernetes (Phase 13.5, skipped).

---

## The Shrink, Honestly Quantified

By raw subsection count across all 141 numbered subsections in the six detail files:

| Tier | Count | % |
|---|---|---|
| 🎯 Master | ~92 | ~65% |
| 👀 Know Only | ~31 | ~22% |
| ⏸️ Skip for Now | ~18 | ~13% |

**Note on the counts below:** they predate the Phase 5 Track A/Track B split (Node.js/NestJS added as the near-term Master-tier backend, Java/Spring Boot's existing sections flipped to Skip For Now). That split adds roughly 9 new Master-tier subsections and moves roughly 8 existing ones from Master to Skip — a wash in total Master count, not a further shrink — so treat the percentages below as approximate rather than re-derived post-split.

**That count alone understates the real relief, and here's why:** a "Know Only" subsection costs roughly 10-15% of the time a "Master" subsection does — no Practice exercises, no spaced repetition, no capstone drilling, just one read-through for vocabulary. A "Skip" subsection costs zero time right now. Weighted by actual effort, not heading count, you're closer to a **35-40% real time reduction** from this pass alone — and the two biggest single wins are things that don't show up proportionally in a heading count at all: **Phase 6 (the entire React Native mobile track, including app store deployment) drops to zero**, and **Phase 13 (Kubernetes + Terraform + full AWS service mastery) drops by roughly 70%** — both of which were disproportionately expensive relative to how many subsections they occupy.

This is now an honest, achievable "senior fullstack web developer" curriculum — not five careers stapled together.

---

## How to Actually Use This With `How_To_Learn_This_Roadmap.md`

- Your **main study track** (the interleaved, spaced-repetition, Practice-exercise-driven system from that file) applies to 🎯 Master sections only.
- 👀 Know Only sections are a single pass each — read, don't drill, don't feel obligated to do the Practice exercises or add them to a spaced-repetition deck. If you catch yourself over-investing in a Know Only section "just to be thorough," that's the completionist instinct from the earlier conversation creeping back in — notice it and move on.
- ⏸️ Skip sections don't need a session at all right now. They're not a debt you owe — they're a shelf you can reach for later.
- Revisit this map itself every few months. Your actual job, the projects you end up on, and your own curiosity will legitimately promote some 👀/⏸️ items back up to 🎯 over time — that's the system working correctly, not a sign this map was wrong.
