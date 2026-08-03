# Part IV — Systems at Scale (Detailed)
### Companion deep-dive to `Master_Senior_Fullstack_Roadmap.md` → Phases 10, 11, 12, 13, 14

> This file expands **Phase 10 (System Design & Architecture)**, **Phase 11 (Testing & Quality Engineering)**, **Phase 12 (Performance Engineering)**, **Phase 13 (DevOps & Infrastructure)**, and **Phase 14 (Security Fundamentals)** into full depth.
>
> **Why this part is where "developer" becomes "engineer":** everything in Parts I–III lets you build a feature that works. This part is about what happens when that feature meets real traffic, real failure modes, real attackers, and a real team that has to deploy and operate it for years. None of these five phases are optional specializations — a "senior fullstack" who can't reason about scale, can't write a test that actually catches regressions, can't explain why an endpoint is slow, can't deploy their own code, or ships an auth bug is not actually senior in the parts that matter most when things go wrong.
>
> These five phases are also the most interconnected in the whole roadmap: a system design decision (10) creates the performance profile you have to defend (12), which you validate with tests (11), which you deploy and observe through infrastructure (13), all of which has to survive an adversarial environment (14). Read them in order once, then expect to bounce between them constantly in real work.

---

## Table of Contents

**Phase 10 — System Design & Architecture**
- 🎯 [10.1 — The Foundation Questions](#101--the-foundation-questions)
- 🎯 [10.2 — Scalability & Availability](#102--scalability--availability)
- 🎯 [10.3 — Consistency Models](#103--consistency-models)
- 👀 [10.4 — Load Balancing](#104--load-balancing)
- 🎯 [10.5 — Caching Architecture](#105--caching-architecture)
- 👀 [10.6 — Asynchronous Communication](#106--asynchronous-communication)
- 🎯 [10.7 — Microservices vs Monolith](#107--microservices-vs-monolith)
- ⏸️ [10.8 — Distributed Systems Fundamentals](#108--distributed-systems-fundamentals)
- 👀 [10.9 — Common Systems to Design](#109--common-systems-to-design)
- [Phase 10 Capstone](#phase-10-capstone)

**Phase 11 — Testing & Quality Engineering**
- 🎯 [11.1 — The Testing Pyramid](#111--the-testing-pyramid)
- 🎯 [11.2 — Test-Driven Development](#112--test-driven-development)
- 🎯 [11.3 — Unit Testing (Deep)](#113--unit-testing-deep)
- 🎯 [11.4 — Integration & E2E Testing](#114--integration--e2e-testing)
- 👀 [11.5 — Performance/Load Testing](#115--performanceload-testing)
- ⏸️ [11.6 — Beyond the Basics](#116--beyond-the-basics)
- 🎯 [11.7 — CI/CD Testing Pipeline](#117--cicd-testing-pipeline)
- [Phase 11 Capstone](#phase-11-capstone)

**Phase 12 — Performance Engineering**
- 🎯 [12.0 — The Golden Rule](#120--the-golden-rule)
- 🎯 [12.1 — Database Performance](#121--database-performance)
- 👀 [12.2 — Backend Performance (Track A: Node.js/NestJS now, Track B: Java/Spring Boot later)](#122--backend-performance-track-a-nodejsnestjs-now-track-b-javaspring-boot-later)
- 🎯 [12.3 — Frontend Performance](#123--frontend-performance)
- 👀 [12.4 — Caching & Infrastructure Performance](#124--caching--infrastructure-performance)
- 👀 [12.5 — Load Testing](#125--load-testing)
- [Phase 12 Capstone](#phase-12-capstone)

**Phase 13 — DevOps & Infrastructure**
- 🎯 [13.1 — Git Mastery (Team Context)](#131--git-mastery-team-context)
- 👀 [13.2 — Linux & Bash for Operations](#132--linux--bash-for-operations)
- 🎯 [13.3 — Containerization: Docker](#133--containerization-docker)
- 🎯 [13.4 — CI/CD: GitHub Actions](#134--cicd-github-actions)
- ⏸️ [13.5 — Container Orchestration: Kubernetes](#135--container-orchestration-kubernetes)
- ⏸️ [13.6 — Infrastructure as Code: Terraform](#136--infrastructure-as-code-terraform)
- 👀 [13.7 — AWS Core Services](#137--aws-core-services)
- 👀 [13.8 — Web Servers & Proxies](#138--web-servers--proxies)
- 👀 [13.9 — Monitoring & Observability](#139--monitoring--observability)
- 👀 [13.10 — Deployment Strategies](#1310--deployment-strategies)
- ⏸️ [13.11 — Backups & Cron](#1311--backups--cron)
- [Phase 13 Capstone](#phase-13-capstone)

**Phase 14 — Security Fundamentals**
- 🎯 [14.1 — Security Mindset](#141--security-mindset)
- 🎯 [14.2 — OWASP Top 10](#142--owasp-top-10)
- 🎯 [14.3 — Authentication Deep Dive](#143--authentication-deep-dive)
- 🎯 [14.4 — Authorization](#144--authorization)
- 🎯 [14.5 — Common Attack Vectors](#145--common-attack-vectors)
- 🎯 [14.6 — Security Headers](#146--security-headers)
- 🎯 [14.7 — Cryptography Fundamentals](#147--cryptography-fundamentals)
- 🎯 [14.8 — Secrets Management](#148--secrets-management)
- 🎯 [14.9 — API Security](#149--api-security)
- [Phase 14 Capstone](#phase-14-capstone)

---

# Phase 10 — System Design & Architecture

**Phase tier: 🎯👀 Mixed — foundational concepts Master; distributed-systems depth (10.8) Skip**

> Real systems don't fail because of bad code. They fail because of bad design decisions made under pressure, without understanding the tradeoffs.

## 10.1 — The Foundation Questions

**Tier: 🎯 Master**

### What to Learn
- Before designing anything: **what problem are we actually solving?** — a surprising number of system-design failures are really "we designed an excellent solution to the wrong problem."
- **Scale requirements** — users, requests/second, data volume — these numbers change the correct answer entirely; a design for 100 users and a design for 100 million users are not the same design scaled up, they're often different designs.
- **Consistency requirements** — does this data need to be correct *right now* for every reader, or can it be eventually correct?
- **What can we sacrifice** — availability, consistency, latency, or cost — there is no design that maximizes all four simultaneously; naming what you're willing to give up is a design decision, not a failure to design well.
- **What happens when each component fails** — asked explicitly, per component, at design time — not discovered for the first time during an incident.
- **Know your numbers** — a rough intuition like "1M QPS ≈ 1KB per request ≈ 1 Gbps bandwidth" lets you sanity-check a design on the spot instead of needing a spreadsheet for every back-of-envelope estimate.

### Practice
1. Take a system you've already built (even a small one) and retroactively answer all five foundation questions for it, honestly — including the ones you never actually considered at the time.
2. Do a back-of-envelope estimate for a hypothetical system (e.g., "a URL shortener with 10M new URLs/day and 100M redirects/day") — estimate storage growth per year and peak QPS, using round numbers, without a calculator.

### Answer This
- Why is "what are we willing to sacrifice" a legitimate design question, rather than a sign the design is incomplete?
- Why does asking "what happens when X fails" at design time matter more than handling it well during an actual incident?

---

## 10.2 — Scalability & Availability

**Tier: 🎯 Master**

### What to Learn
- **Vertical scaling** (a bigger machine — simpler, but has a real ceiling and gets disproportionately expensive) vs **horizontal scaling** (more machines — more complex, but effectively unlimited) — horizontal scaling requires **stateless services** (Phase 7.1's REST statelessness principle, generalized): if a server holds session state in its own memory, you can't freely route a request to any instance.
- **Auto-scaling** on real metrics (CPU, request count, queue depth) rather than a fixed instance count that someone has to remember to adjust.
- **Availability numbers** — 99.9% = 8.7 hours of downtime per year, 99.99% = 52 minutes/year, 99.999% = 5 minutes/year — each additional "nine" is not a linear increase in engineering effort, it's often an order-of-magnitude increase, which is *why* teams should deliberately choose a realistic target rather than reflexively promising "99.999%."
- **Single points of failure (SPOF)** — identify every component that, if it alone fails, takes the whole system down with it, and treat finding these as a deliberate design review activity, not something you hope you'll notice.
- **Redundancy** — active-active (multiple instances actively handling traffic simultaneously) vs active-passive (a standby instance that only takes over on failure) — different cost and failover-time tradeoffs.
- **Graceful degradation** — a non-critical component failing should degrade the experience, not take down the entire system (e.g., a recommendation service failing shouldn't prevent checkout from completing).
- **Health checks** — liveness ("is the process even running") vs readiness ("is it currently able to serve traffic correctly") — these answer genuinely different questions, and conflating them causes real orchestration bugs (e.g., an instance that's alive but still warming up its cache being sent traffic too early).

### Practice
1. Diagram a system you've built or are building, and mark every single point of failure explicitly on the diagram — for each one, write down what would actually happen if it went down right now.
2. Design (on paper) a graceful-degradation path for one non-critical dependency in a real system — what should the user actually see/experience if that dependency is unavailable, versus a hard failure?

### Answer This
- Why is going from 99.9% to 99.99% availability typically a much bigger engineering effort than the numbers alone suggest?
- Why do liveness and readiness checks need to be separate concepts, rather than one combined "is it healthy" check?

---

## 10.3 — Consistency Models

**Tier: 🎯 Master**

### What to Learn
- **Strong consistency** — every read reflects the most recent write, immediately, everywhere — the intuitive default most people assume, but genuinely expensive to guarantee at scale and across geographic distance.
- **Eventual consistency** — reads may be stale for a period, but the system guarantees convergence eventually — cheaper and more available, at the cost of that staleness window being a real, visible behavior a user or another system might notice.
- **Choosing per use case, not globally** — a bank balance needs strong consistency; a social media "like" count tolerates eventual consistency just fine — the correct choice is a property of the specific data, not a single decision for the whole system.

### Practice
1. For three different kinds of data in a real or hypothetical app (account balance, "like" count, search index freshness), decide and justify which consistency model each genuinely needs — and identify what would go visibly wrong if you picked the *other* one for each.

### Answer This
- Why would choosing strong consistency for a "like" counter be a wasted, overly expensive design decision?
- Why would choosing eventual consistency for a bank balance be a real, dangerous bug rather than just a minor inconvenience?

---

## 10.4 — Load Balancing

**Tier: 👀 Know Only**

### What to Learn
- **Algorithms** — round robin (simplest, ignores current load per server), least connections (routes to the currently-least-busy server), IP hash (the same client consistently reaches the same server — useful when session affinity is needed), **consistent hashing** (the specific technique that matters most for distributed caches/databases: when a node is added or removed, only roughly 1/N of keys need to be remapped, instead of nearly all of them as with naive modulo-based hashing).
- **Layer 4 vs Layer 7** — Layer 4 (transport layer — routes based on IP/port, faster, less intelligent) vs Layer 7 (application layer — can route based on the actual HTTP request content, e.g., path-based routing, but with more overhead).
- Nginx/HAProxy as concrete, real tools implementing reverse proxying and load balancing.

### Practice
1. Implement (or trace through by hand) what happens to key-to-node assignment when a node is added under naive modulo hashing versus consistent hashing, for a small example (e.g., 4 keys, 3 nodes, then adding a 4th) — count how many keys actually had to move in each case.

### Answer This
- Why does consistent hashing remap far fewer keys than naive modulo hashing when the number of nodes changes?
- When would Layer 7 routing be necessary, where Layer 4 genuinely couldn't do the job?

---

## 10.5 — Caching Architecture

**Tier: 🎯 Master**

### What to Learn
- **What to cache** — expensive computations, read-heavy/rarely-changing data, external API responses; **what NOT to cache** — user-specific real-time data, and anything security-sensitive where staleness could mean a stale permission check.
- **Layers** — browser cache → CDN → application cache (Redis, Phase 8.6) → database buffer pool (Phase 8.1) — each layer exists to avoid doing the more expensive work of the layer below it.
- **Cache warming** — pre-populating a cache before real traffic hits it, avoiding a cold-start latency spike right after a deploy or a cache flush.
- **Eviction policies** — LRU (Least Recently Used), LFU (Least Frequently Used), TTL-based — each models a different assumption about what data is worth keeping when the cache is full.
- **Cache stampede** — the same failure mode from Phase 8.6, generalized to the whole caching architecture, not just Redis specifically.

### Practice
1. Identify one piece of data in a real system that's genuinely safe to cache (read-heavy, rarely changes) and one that would be dangerous to cache (security-sensitive, or must always be fresh), and write down precisely why each belongs in its category.

### Answer This
- Why is caching a permission/authorization check specifically dangerous, even if the performance win looks tempting?
- What's the actual difference between what LRU and LFU optimize for, and when would each be the wrong choice?

---

## 10.6 — Asynchronous Communication

**Tier: 👀 Know Only**

### What to Learn
- **Message queues vs direct HTTP calls** — a queue decouples the producer from the consumer's availability; the consumer can be down entirely and the message simply waits, whereas a direct HTTP call fails immediately if the callee is unreachable.
- **At-least-once vs exactly-once delivery** — at-least-once (a message may be delivered more than once — consumers must be idempotent, Phase 5A.7 now / 5B.6 later) is the realistic, commonly-implemented guarantee; true exactly-once delivery across a real distributed system is extraordinarily hard to guarantee end-to-end, and most systems that claim it are really doing at-least-once delivery plus idempotent processing, which produces an *effectively* exactly-once outcome without the underlying delivery guarantee actually being exactly-once.
- **Backpressure** — what happens when consumers process slower than producers produce; the queue fills up, and the system needs an explicit strategy (slow the producer, drop low-priority messages, scale out consumers) rather than silently growing an unbounded queue until something else breaks.
- **Dead letter queues** — messages that fail processing repeatedly get routed aside for inspection and manual/automated retry, instead of blocking the whole queue or being silently lost.
- **Event-driven architecture** — services communicate by emitting/reacting to events instead of direct calls; know the real distinction between **event notification** (a lightweight "something happened, go find out more if you care" signal) and **event-carried state transfer** (the event itself carries the full data needed, so subscribers don't need to call back to the source) — this distinction changes coupling and failure characteristics significantly.

### Practice
1. Design a message handler that is genuinely idempotent (using an idempotency key, Phase 5A.7/5A.9 now, 5B.6/5B.8 later) and then deliberately deliver the same message to it twice, confirming the end state is correct either time.
2. Design (on paper) what should happen to a producer, a queue, and a consumer under sustained backpressure — what's your actual strategy, and what would happen without one?

### Answer This
- Why is "true exactly-once delivery" usually really "at-least-once delivery plus an idempotent consumer" under the hood?
- What real, concrete failure happens to a system with no backpressure strategy at all, once a consumer falls meaningfully behind?

---

## 10.7 — Microservices vs Monolith

**Tier: 🎯 Master**

### What to Learn

| Architecture | When to Use |
|---|---|
| Monolith | Start here — simple to develop, deploy, and debug |
| Modular Monolith | Best of both — structured module boundaries, one deployable unit |
| Microservices | When the pain of staying together genuinely exceeds the pain of splitting |

- Most teams adopt microservices too early, trading a real, immediate cost (network complexity, distributed transactions, operational overhead, harder local debugging) for a scaling/team-independence benefit they don't actually need yet.
- Split a module out when it has genuinely independent scaling needs, a different deployment cadence than the rest of the system, or clear separate team ownership — not because "microservices are the modern way to do it."
- **Service discovery** — how services locate each other in a dynamic environment where instances come and go.
- **API Gateway** — a single entry point handling cross-cutting concerns (auth, routing, rate limiting) so individual services don't each reimplement them.
- **Service mesh** (awareness level — Istio, Linkerd) — an infrastructure layer handling service-to-service communication concerns (retries, mTLS, observability) outside of application code.
- **Data ownership** — each microservice owns its own data exclusively; no service reaches directly into another service's database — this is the actual boundary that makes "micro" services independently deployable at all; without it, you have a distributed monolith with all the operational cost and none of the independence benefit.

### Practice
1. Take a monolith (real or hypothetical) and identify one module that has a genuinely independent scaling need or deployment cadence — write down the specific, concrete evidence for that claim, not just "it feels like it should be separate."
2. Write down what would have to change (data migration, new inter-service communication, new deployment pipeline) to actually extract that module as a separate service — and honestly assess whether that cost is currently justified.

### Answer This
- Why is "each service owns its own data" the real boundary that makes microservices independently deployable, rather than just having separate deployables?
- What specific, concrete signal (not vibes) should trigger splitting a module out of a monolith?

---

## 10.8 — Distributed Systems Fundamentals

**Tier: ⏸️ Skip For Now**

### What to Learn
- **The Fallacies of Distributed Computing** — the network is not reliable, not zero-latency, and not infinite-bandwidth; every distributed design that implicitly assumes otherwise will eventually break in production.
- **CAP theorem** — during an actual network partition, a system must choose Consistency or Availability (it cannot have both); **PACELC** extends this: even *without* a partition, there's still a latency-vs-consistency tradeoff in normal operation.
- **Consensus algorithms** (awareness — Raft, Paxos, ZAB) — how distributed systems agree on a single value/leader despite failures; this is what etcd, Consul, and ZooKeeper actually implement under the hood, and why systems built on top of them (like Kubernetes) can make strong claims about cluster state agreement.
- **2PC (Two-Phase Commit)** — a coordinator asks all participants to prepare, then commits only if all agreed; the real weakness is that a coordinator failure mid-protocol can leave participants blocked, holding locks, indefinitely.
- **Saga pattern** (Phase 3.3) — a sequence of local transactions with compensating actions, avoiding 2PC's blocking-coordinator weakness by trading strict atomicity for eventual, compensatable consistency.
- **TCC (Try-Confirm-Cancel)** — a variant distributed-transaction pattern (reserve/try, then confirm or cancel) — know it exists as a third option alongside 2PC and Saga, each with different practical tradeoffs.
- **Idempotency** and **idempotency keys** (Phase 5A.9/5B.8, 7.2) as the practical foundation that makes "at-least-once delivery" tolerable.
- **Distributed locking with Redis** (`SET key value NX EX 30` — atomic set-if-not-exists with an expiry, used as a simple distributed lock) — know its real limitations (clock drift, a process holding the lock past the TTL due to a pause) rather than treating it as a perfect mutex.
- **Circuit breaker** (Phase 3.3) and **retry with exponential backoff + jitter** — retrying immediately and repeatedly against a struggling service makes the problem worse (a "retry storm"); backoff with jitter spreads retries out in time so many clients don't all hammer the recovering service at the exact same moment.
- **Bulkhead pattern** — isolate different workloads into separate resource pools (e.g., separate thread pools per downstream dependency) so one slow/failing dependency can't exhaust resources needed by unrelated work.

### Practice
1. Walk through, step by step, what happens if the coordinator in a 2PC protocol crashes after telling participants to "prepare" but before telling them to "commit" — write down exactly what state the participants are left in and why that's a real operational problem.
2. Design a Saga (on paper) for a 3-step business process (e.g., reserve inventory → charge payment → schedule shipping), including the compensating action for each step if a later step fails.
3. Implement a Redis-based distributed lock and deliberately demonstrate its weakness: hold the "critical section" longer than the lock's TTL and observe a second process incorrectly acquiring the lock while the first is still working.
4. Implement retry with exponential backoff and jitter against a simulated flaky dependency, and compare request timing/success patterns against naive immediate retries.

### Answer This
- Why does a coordinator crash mid-2PC-protocol leave participants blocked, specifically?
- Why does a Saga trade away 2PC's atomicity guarantee, and what does it gain in exchange?
- Why does jitter matter in addition to exponential backoff, not just backoff alone?
- What real failure does a Redis-based distributed lock not actually protect against, that a "real" mutex within a single process would?

---

## 10.9 — Common Systems to Design

**Tier: 👀 Know Only**

### What to Learn
For each system below, practice working through: functional requirements, a rough scale estimate, the key components, the database choice(s), and the real tradeoffs involved.

| System | Core Challenges |
|---|---|
| URL Shortener | Hashing/ID-generation strategy, redirect performance, analytics, caching |
| Rate Limiter | Token bucket vs sliding window, distributed counters in Redis |
| Real-time Chat | WebSockets, fan-out, delivery guarantees, message history/storage |
| Notification System | Multi-channel delivery, fan-out at scale, deduplication |
| Authentication Service | JWT, refresh tokens, revocation, session management at scale |
| Payment System | Idempotency, consistency, fraud detection, webhook reliability |
| Search System | Inverted index, ranking, full-text vs vector search |
| File Upload Service | Presigned URLs, chunked upload, CDN distribution |
| Video Streaming | Object storage, CDN, adaptive bitrate, encoding pipeline |
| Distributed Cache | Consistent hashing, eviction policies |

### Practice
> **PROJECT (recap, expanded from the master roadmap):** Design and build all three: a **URL Shortener** (PostgreSQL + Redis cache + rate limiting — walk through your ID-generation strategy explicitly: auto-increment + base62 encoding vs a hash of the URL, and the collision/predictability tradeoffs of each), a **Real-time Notification System** (WebSockets + Redis pub/sub + RabbitMQ + persistence — explicitly design what happens if a user is offline when a notification is sent), and a **Rate Limiter** service (token bucket in Redis, distributed, per-user — explicitly handle the case of two requests arriving at the exact same instant needing an atomic check-and-decrement, tying back to 8.6's Lua-script atomicity discussion).

### Answer This
- For a URL shortener, what's the real tradeoff between an auto-incrementing ID (encoded to base62) and a hash of the destination URL, in terms of collisions, predictability, and reversibility?
- For a rate limiter, why does a naive "read count, check, increment" sequence risk a race condition under concurrent requests, and what fixes it?

---

## Phase 10 Capstone

> **PROJECT:** Complete the three systems from 10.9's practice section end to end — actually built and running, not just diagrammed. For each, write a short design document covering the five foundation questions from 10.1, your consistency-model choice and why, your caching strategy, and at least one explicit single-point-of-failure you identified and how you mitigated (or consciously accepted) it.

---

# Phase 11 — Testing & Quality Engineering

**Phase tier: 🎯 Mostly Master — core testing practice Master; advanced techniques (mutation/chaos/property-based) Skip**

> If you're afraid to change your code, your tests are failing you — even if they all currently pass.

## 11.1 — The Testing Pyramid

**Tier: 🎯 Master**

### What to Learn
```
        /\
       /E2E\          ← Few, slow, expensive — critical paths only
      /------\
     /Integration\    ← Some, medium speed — component contracts
    /------------\
   /  Unit Tests  \   ← Many, fast, cheap — the foundation
  /----------------\
```
- Invert this shape (many slow E2E tests, few fast unit tests) and you get a suite that's slow, brittle, and expensive to maintain — the shape itself is the actual lesson, not just "write some of each kind."

### Practice
1. Audit a real test suite you have access to (yours or an open-source project's) and roughly categorize its tests by type — is the actual shape closer to a pyramid or an inverted pyramid? What does that predict about how painful the suite is to run and maintain?

### Answer This
- Why does an inverted testing pyramid (many E2E tests, few unit tests) tend to produce a slow, brittle suite specifically, rather than just "a differently-balanced" one?

---

## 11.2 — Test-Driven Development

**Tier: 🎯 Master**

### What to Learn
- **RED** — write a failing test first, for behavior that doesn't exist yet.
- **GREEN** — write the *minimum* code needed to make it pass — deliberately resisting the urge to also build in extra functionality not yet demanded by a test.
- **REFACTOR** — clean up the implementation (Phase 3.5) while the tests stay green throughout.
- TDD's real value is **design feedback**, not test coverage as an end in itself — writing the test first forces you to design the API/interface from the caller's perspective before you've committed to an implementation, which naturally produces more decoupled, testable code.

### Practice
1. Pick one small, well-scoped feature and implement it strictly test-first, one RED-GREEN-REFACTOR cycle at a time — resist writing any implementation code that isn't currently demanded by a failing test.
2. Afterward, honestly compare the resulting design against how you'd have likely designed it test-after — note anywhere TDD pushed you toward a cleaner interface earlier than you'd have found it otherwise.

### Answer This
- Why is TDD described as a design technique rather than purely a testing technique?
- What specifically should you resist doing during the GREEN step, and why?

---

## 11.3 — Unit Testing (Deep)

**Tier: 🎯 Master**

### What to Learn
- **Arrange-Act-Assert** — structure every test the same way: set up state, perform the action, assert the outcome — consistent structure makes tests fast to read and review.
- **Test doubles** — **dummy** (a placeholder never actually used, just satisfies a parameter list), **stub** (returns a hardcoded response for a specific call), **fake** (a real, working implementation not suitable for production — e.g., an in-memory repository), **mock** (verifies that specific interactions/calls actually happened), **spy** (wraps a real implementation while also recording calls made to it) — these are genuinely different tools solving different verification needs, not interchangeable jargon for "test double."
- **Jest** (near-term, Track A) — `jest.fn()`/`jest.spyOn()` for setup, `mockReturnValue`/`mockRejectedValue` for stubbing behavior, `expect(...).toHaveBeenCalledWith(...)` for inspecting exactly what arguments a mocked dependency was actually called with, `describe`/`it`/`beforeEach` for organizing and isolating tests.
- **Mockito + JUnit 5** (later, Track B) — `@Mock`/`@InjectMocks` for setup, `when(...).thenReturn(...)`/`thenThrow(...)` for stubbing behavior, `ArgumentCaptor` for inspecting exactly what arguments a mocked dependency was actually called with (the same role `toHaveBeenCalledWith` plays in Jest), `verify(...)` for confirming an interaction happened; `@Test`, `@ParameterizedTest`, `@Nested` for organizing tests. The underlying testing concepts (AAA, test doubles, TDD) are identical across both — only the API surface differs.
- **Naming convention** — `should_returnUser_when_validIdProvided` (or an equivalent descriptive style) makes a test failure immediately informative from the test name alone, before even reading the assertion.

### Anti-patterns
- **Testing implementation, not behavior** — asserting on internal method calls/private state instead of observable outputs — this kind of test breaks on every refactor, even ones that don't change behavior at all, actively fighting against the safety net tests are supposed to provide.
- **Order-dependent tests** — a test that only passes if a specific other test ran first (often due to shared mutable state) — catastrophically fragile, since test execution order isn't guaranteed and shouldn't need to be.
- **Over-mocking** — mocking so much of a unit's real dependencies that the test ends up verifying your mocks behave as configured, rather than verifying the actual code under test.

### Practice
1. Write the same unit test scenario twice: once testing behavior (observable output for a given input) and once testing implementation (asserting a specific private method was called) — then refactor the implementation without changing its behavior, and confirm the behavior-based test still passes while the implementation-based test breaks, even though nothing actually went wrong.
2. Use `expect(...).toHaveBeenCalledWith(...)` (Jest, now) or `ArgumentCaptor` (Mockito, later) to assert not just that a mocked email service was called, but that it was called with the exact expected recipient and subject.
3. Deliberately create two tests that share mutable state and only pass in a specific run order; then fix the fragility by properly isolating state per test.

### Answer This
- Why does a test that asserts on internal implementation details break during a safe refactor, and why is that a problem rather than the test "correctly noticing a change"?
- What does `toHaveBeenCalledWith`/`ArgumentCaptor` let you verify that a bare "was it called" check doesn't?
- Why is over-mocking sometimes described as "testing your mocks, not your code"?

---

## 11.4 — Integration & E2E Testing

**Tier: 🎯 Master**

### What to Learn
- **Integration tests** verify that real components work together correctly — the database layer actually talking to a real database, not a mock of one.
- **Testcontainers** (Phase 5A.8 now, 5B.7 later) — spin up real PostgreSQL/Redis in Docker specifically for the test run, then tear down — avoiding both the "mocked away entirely" gap and the "shared, stateful test database" maintenance burden.
- **E2E tests** simulate a real user driving the entire real system end to end. **Playwright** — `page.goto`, `page.click`, `page.fill`, `expect(locator).toBeVisible()` — browser automation with built-in auto-waiting that reduces (but doesn't eliminate) flakiness compared to older tools.
- **Page Object Model** — encapsulate a page's selectors and interactions behind a class, so a UI change requires updating one place instead of every test that touches that page.
- **Scope discipline** — E2E tests should cover critical user journeys only (registration, login, checkout) — trying to E2E-test everything reproduces the inverted-pyramid problem from 11.1.
- **Flaky tests are the #1 enemy of E2E suites** — caused by timing assumptions, shared state between tests, or genuine environment instability; fixed with explicit waits for actual conditions (not arbitrary sleeps) and full test isolation (each test creates and cleans up its own data).

### Practice
1. Write one integration test against a real PostgreSQL instance via Testcontainers, deliberately using a Postgres-specific feature that an in-memory substitute would behave differently on (reusing the exercise from Phase 5A.8/8.5 now, 5B.7/8.5 later, in a testing-focused context).
2. Write one E2E test for a genuinely critical path (e.g., login) using Playwright and the Page Object Model, then deliberately introduce a flaky-timing bug (assert immediately after an action without waiting for a real condition) and observe intermittent failures; fix it with a proper explicit wait.

### Answer This
- What real gap does Testcontainers close that pure mocking of the database layer leaves open?
- Why does asserting immediately after a UI action (without waiting for a real condition) tend to produce a flaky test rather than a reliably-failing one?

---

## 11.5 — Performance/Load Testing

**Tier: 👀 Know Only**

### What to Learn
- **Load testing** — simulate expected real-world traffic and confirm the system handles it within acceptable latency/error thresholds.
- **Stress testing** — push traffic beyond expected capacity specifically to find where and how the system breaks.
- **Spike testing** — a sudden, sharp traffic surge (not a gradual ramp) — tests a different failure mode than a sustained high load.
- **Soak testing** — moderate load sustained for hours, specifically to catch slow memory leaks or gradual resource exhaustion that a short test would never reveal.
- **k6** — write load-test scenarios as JavaScript, defining virtual users and their behavior over time.
- **Metrics that matter** — p50/p95/p99 latency (never just the average, which hides the worst experiences that a meaningful fraction of real users actually have), error rate, throughput.
- **The discipline** — find a bottleneck, fix it, then measure again — never assume a fix worked without re-measuring (this is Phase 12's Golden Rule, applied here specifically to load testing).

### Practice
1. Load test a real endpoint with k6 at increasing traffic levels until you find its actual breaking point, recording p50/p95/p99 latency and error rate at each level, not just whether it "worked."
2. Run a soak test (sustained moderate load for an extended period) against a service and watch memory/connection usage over time for any slow growth that a 30-second load test would never reveal.

### Answer This
- Why is looking only at average latency actively misleading, and what does p99 reveal that the average hides?
- Why does a soak test catch bugs that a short load test structurally cannot?

---

## 11.6 — Beyond the Basics

**Tier: ⏸️ Skip For Now**

### What to Learn
- **Contract testing (Pact)** — consumer-driven contracts let a frontend/consumer team define exactly what they expect from an API; that contract is verified automatically against the real backend in CI, catching a breaking API change before it ever reaches a shared environment (directly connects to Phase 7.6).
- **Property-based testing** — instead of hand-picking specific example inputs, define a *property* that must hold for all valid inputs (e.g., "sorting a list then reversing it should equal reversing then sorting"), and let the framework (fast-check for JS, jqwik for Java) generate hundreds of random inputs, including edge cases you'd never have thought to write by hand.
- **Mutation testing** — a tool (Stryker for JS/TS, PIT for Java) automatically introduces small deliberate bugs ("mutants") into your code and re-runs your test suite against each mutant; if a test suite still passes despite a real bug being introduced, that's a test suite with a coverage-shaped hole — mutation testing measures whether your tests would actually *catch* a bug, which raw line/branch coverage percentages don't tell you at all.
- **Visual regression testing** — automatically screenshot UI components/pages and diff them against an approved baseline, catching unintended visual changes from a CSS or component refactor that functional tests would never notice. Tools: Chromatic, Percy, or Playwright's built-in screenshot comparison.
- **Chaos engineering** — deliberately inject failures (kill an instance, add artificial latency, drop packets) to validate that a system actually degrades the way you assumed it would; the process is explicit: define the steady state → hypothesize it holds under the injected chaos → inject the failure → observe what actually happens. Requires mature observability *before* attempting it, and should start in staging, not production. "Game days" are scheduled chaos exercises run with the whole team present, deliberately, rather than surprise incidents.

### Practice
1. Set up Pact (or a similar tool) for one real consumer-provider relationship, deliberately introduce a breaking change to the provider, and confirm the contract test catches it before deployment.
2. Write 3-5 property-based tests (fast-check or jqwik) for a real data-transformation function, and let the framework find at least one edge case you hadn't manually considered.
3. Run a mutation-testing tool against an existing test suite with reasonably high line coverage, and find at least one surviving mutant — a real bug your "well-covered" tests didn't actually catch — then write the missing test.
4. In a staging environment only, deliberately kill one non-critical service instance while a load test is running, and observe whether the system actually degrades gracefully the way you assumed, or whether it fails harder than expected.

### Answer This
- What real gap does contract testing close that a passing backend test suite plus a passing frontend test suite, run independently, doesn't?
- Why can a test suite have high line coverage and still fail to catch real bugs — and how does mutation testing expose that gap specifically?
- Why must chaos engineering start in staging with solid observability already in place, rather than "just try it in production and see"?

---

## 11.7 — CI/CD Testing Pipeline

**Tier: 🎯 Master**

### What to Learn
```
Every push triggers automatically:
1. Lint                      ← < 30 seconds
2. Unit tests                ← < 1 minute
3. Integration tests         ← < 5 minutes
4. Security dependency scan
5. Coverage threshold check  ← block merge if below threshold
6. E2E tests on critical paths ← blocking on main branch
```
- The ordering matters — fast, cheap checks run first so a trivial mistake (a lint error) fails in seconds, not after waiting for a 5-minute integration suite.
- A slow pipeline (30+ minutes) gets bypassed in practice — people stop running it locally and start ignoring its failures, which defeats the entire point of having it.

### Practice
1. Build (or audit an existing) CI pipeline against this exact ordering, and time each stage — identify which stage is the actual bottleneck and whether it can be parallelized or made faster without losing real signal.

### Answer This
- Why does the *ordering* of pipeline stages matter, not just which checks exist?
- What actually happens, in practice, to a CI pipeline that regularly takes 30+ minutes?

---

## Phase 11 Capstone

> **PROJECT:** Take an existing project (yours, ideally your Phase 5/10 Master Project work) and: practice strict TDD for one full feature from scratch. Build a GitHub Actions pipeline matching the ordering in 11.7 that genuinely blocks merges on failing tests or an insufficient coverage threshold. Load test the API with k6, find its real breaking point, and document p50/p95/p99 at each traffic level tested. Write 5 property-based tests for a real data transformation function. Run a mutation-testing tool against your existing suite and fix at least one surviving mutant you find.

---

# Phase 12 — Performance Engineering

**Phase tier: 🎯👀 Mixed — Golden Rule + DB/frontend performance Master; JVM tuning and infra performance Know Only**

> Performance is a design property, not an afterthought. Measure first — never optimize what you haven't measured.

## 12.0 — The Golden Rule

**Tier: 🎯 Master**
```
Measure → Identify bottleneck → Fix → Measure again
Never optimize without data. Never guess.
```
- **Profiling** (finds *where* time is actually spent) is a different activity from **benchmarking** (measures *how fast* a specific thing is) — you generally profile first to find the target, then benchmark that specific target before and after a change.
- **Flame graphs** — wide sections represent more time spent; reading one correctly means looking for width, not just depth.
- **JMH (Java Microbenchmark Harness)** exists specifically because naive hand-timed loops in Java produce misleading results due to JIT warm-up (Phase 1.6) — a proper microbenchmark accounts for this.

### Answer This
- Why is "I made this faster" without a before/after measurement not a real performance claim?
- Why can a naive hand-timed loop benchmark in Java give a misleading result compared to a JMH benchmark?

---

## 12.1 — Database Performance

**Tier: 🎯 Master**

### What to Learn
- `EXPLAIN ANALYZE` (Phase 8.2) — read every node's `cost`, `actual rows`, and `actual time`, and make this a reflex on every non-trivial query, not a last resort.
- **N+1 detection** — enable query logging in development (Prisma's query log now, Hibernate SQL logging later) and literally count queries fired per request; don't rely on assumption alone (Phase 5A.4/5B.3, 8.4).
- **Connection pool sizing** — the rough heuristic `(cores × 2) + effective spindle count`, and **HikariCP** tuning (`maximumPoolSize`, `connectionTimeout`, `leakDetectionThreshold`) — a pool sized too large can *hurt* throughput by causing excessive context switching and lock contention on the database side, which is counterintuitive if you assume "more connections = more parallelism" without limit.
- **Slow query logging** (`log_min_duration_statement` in Postgres) — surface genuinely slow queries automatically instead of relying on someone noticing a page feels sluggish.
- **Prepared statements** — prevent SQL injection (Phase 14) *and* enable query plan caching, a genuine two-for-one benefit.
- **Cursor-based pagination over OFFSET** (Phase 8.4) and **materialized views** for precomputed expensive aggregations, refreshed on a schedule.

### Practice
1. Enable slow query logging in a local Postgres instance, run a deliberately expensive query, and confirm it gets logged automatically without you needing to notice the slowness yourself first.
2. Deliberately misconfigure a HikariCP pool with an excessively large `maximumPoolSize` under concurrent load, and compare actual throughput against a properly-sized pool — confirm that "more connections" made things worse, not better, past a certain point.

### Answer This
- Why can an oversized connection pool actually reduce throughput instead of increasing it?
- Why does using a prepared statement give you both a security benefit and a performance benefit from the same mechanism?

---

## 12.2 — Backend Performance (Track A: Node.js/NestJS now, Track B: Java/Spring Boot later)

**Tier: 👀 Know Only**

### Track A — Node.js / NestJS Performance (near-term)
- **Node's single-threaded event loop** — a CPU-bound synchronous block stalls the *entire process*, not just one request (Phase 5A.1); offload genuinely CPU-heavy work to a `worker_threads` Worker Thread instead of blocking the event loop.
- **Avoiding blocking the event loop** — synchronous `fs`/crypto calls, large `JSON.parse`/`stringify` operations, or a tight synchronous loop in a hot path all stall every concurrent request, not just the one that triggered them.
- **Clustering / PM2** — a single Node process can only use one CPU core; scale across cores with the built-in `cluster` module or a process manager like PM2.
- **HTTP response compression** (gzip/Brotli) for large JSON payloads specifically — same mechanism, same benefit, framework-independent.
- **Avoiding unnecessary serialization** — return Prisma `select`-scoped projections (Phase 5A.4/8.4) instead of full records with unneeded relations attached.
- **Profiling** — Node's built-in `--prof` flag plus `clinic.js` (`clinic doctor`, `clinic flame`) for flame graphs, or Chrome DevTools attached via `--inspect`.

### Track B — Java / Spring Boot Performance (deferred)
- **JVM tuning** — `-Xmx`/`-Xms` heap sizing, and choosing **G1GC vs ZGC** (Phase 1.3) as a genuine throughput-vs-latency tradeoff, not a "pick the newest one" decision.
- **Avoiding object allocation in hot paths** — every allocation is eventually GC work; a tight, frequently-executed loop that allocates unnecessarily creates real, measurable GC pressure.
- **`CompletableFuture`** for genuinely parallel I/O operations, and **virtual threads (Java 21)** specifically for I/O-bound endpoints, letting you write simple blocking-style code while still achieving high concurrency (Phase 5B.1) — the same underlying goal as Node's single-threaded-but-non-blocking model, achieved by a completely different mechanism (many cheap JVM-managed threads vs one event loop).
- **Spring Boot startup time** — lazy bean initialization, and GraalVM native image awareness (compiling ahead-of-time to a native binary, trading longer build times for near-instant startup and lower memory — relevant for serverless/scale-to-zero deployment models).
- **HTTP response compression** (gzip/Brotli) for large JSON payloads specifically.
- **Avoiding unnecessary serialization** — return DTO projections (Phase 5B.3/8.4) instead of full entities with lazy associations that might trigger further loading.

### Practice
1. **(Track A, now)** Deliberately block the event loop with a synchronous CPU-heavy operation inside a NestJS handler, fire several concurrent requests, and confirm every other request stalls until it finishes — then move the work to a Worker Thread and confirm requests interleave properly again.
2. **(Track B, later)** Profile a real endpoint with a JVM profiler (async-profiler or Java Flight Recorder), identify one specific hot-path allocation, eliminate or reduce it, and measure the GC-pressure difference before and after; then convert one I/O-bound endpoint to use virtual threads and compare its behavior under high concurrent load against a traditional bounded thread pool.

### Answer This
- Why does a single synchronous CPU-heavy operation in a Node handler stall *every* concurrent request, not just the one that triggered it?
- Why does an unnecessary allocation in a hot loop translate into real, measurable cost, even though the JVM's GC is quite fast?
- Why do virtual threads let Java keep simple blocking-style code while still scaling to high I/O concurrency, and how does that compare mechanically to Node achieving high I/O concurrency on a single thread?

---

## 12.3 — Frontend Performance

**Tier: 🎯 Master**

### What to Learn
- Reiterating and applying Phase 4.9's Core Web Vitals, code splitting, image optimization, and bundle analysis specifically as a *measured, iterative* performance-engineering discipline: baseline, change one thing, re-measure, keep or revert based on the actual number.
- `React.memo`/`useMemo`/`useCallback` only after the Profiler has identified a real, specific re-render cost (Phase 4.5) — not as a default habit.
- Critical rendering path optimization — inlining critical CSS, deferring non-critical scripts (Phase 1.5).

### Practice
1. Take a real page, record Lighthouse's baseline Core Web Vitals, apply exactly one optimization, re-measure, and record the delta — repeat for at least 3 separate optimizations, keeping a clear before/after log for each individually (not one big combined "did lots of stuff" measurement, which would tell you nothing about which change actually mattered).

### Answer This
- Why is measuring one change at a time more valuable than making several changes and measuring once at the end?

---

## 12.4 — Caching & Infrastructure Performance

**Tier: 👀 Know Only**

### What to Learn
- Redis cache-aside with real hit/miss monitoring (know your actual hit rate, don't assume it) — cache warming, and cache stampede protection (Phase 8.6/10.5) applied specifically as a performance-engineering lever.
- CDN cache policies and invalidation strategies as a genuine performance decision, not just a deployment detail.
- **HTTP/2 multiplexing** and header compression, and **why HTTP/3/QUIC** specifically wins on high-packet-loss networks like mobile cellular connections (Phase 1.4) — a concrete case where the "best" protocol depends on your actual user base's network conditions.

### Answer This
- Why would HTTP/3 provide a meaningfully bigger real-world improvement for a mobile-heavy user base than for an office-network user base?

---

## 12.5 — Load Testing

**Tier: 👀 Know Only**

### What to Learn
- Reiterating Phase 11.5's k6-based load testing specifically through a performance-engineering lens: use load testing not just to validate "does it work under load" (a QA question) but to actively find and characterize the system's specific bottleneck (a performance-engineering question) — p50/p95/p99 latency, error rate, and throughput, correlated against what's actually saturating (CPU, DB connections, thread pool, memory) at the breaking point.

### Practice
1. Load test an endpoint to its breaking point and, at the point of failure, correlate the failure with a specific resource metric (CPU, DB connection pool exhaustion, thread pool queue depth) rather than just recording "it broke around X req/s" — identify the actual bottleneck resource, not just the symptom.

### Answer This
- Why is "it broke at 500 req/s" a less useful finding than "it broke at 500 req/s because the DB connection pool was exhausted"?

---

## Phase 12 Capstone

> **PROJECT:** Take your Master Project (Phase 5/10) and run a complete, honest performance audit: load test it with k6 to find its actual breaking point and the specific resource that's the bottleneck at that point. Profile the slowest endpoint with a real profiler — Node's `--prof`/`clinic.js` now, a JVM profiler (async-profiler/Java Flight Recorder) when this capstone is repeated against the Track B rebuild — and fix the top allocation/query issue you find. Measure Core Web Vitals before and after at least 3 individually-measured frontend optimizations. Document every single change with its measured before/after impact — no unmeasured claims of "this should be faster."

---

# Phase 13 — DevOps & Infrastructure

**Phase tier: 👀 Mostly Know Only — Git/Docker/basic CI Master; Kubernetes/Terraform/deep AWS Skip — the biggest cut for a fullstack (not DevOps) focus**

> Infrastructure is not someone else's problem. Engineers who understand the full path from code to production build systems that actually work reliably, and can fix them at 2am without paging someone else.

## 13.1 — Git Mastery (Team Context)

**Tier: 🎯 Master**

### What to Learn
- **Branching strategies** — Git Flow (feature/develop/release/hotfix branches — heavier process, less common for fast-moving teams now), Trunk-Based Development (everyone commits to `main` frequently, short-lived branches — the strategy most modern CI/CD practices assume), GitHub Flow (simple, PR-based — the practical default for most teams).
- Advanced Git (Phase 0.5, reapplied in a team context) — rebase vs merge tradeoffs on a shared branch specifically (never rebase something already pushed and shared, Phase 0.5), interactive rebase to clean history *before* opening a PR (not after), `git bisect` to find a regression across a real team's commit history, hooks to automate checks before commits/pushes.
- Conventional commits, applied consistently across a team, to enable automated changelogs and clear PR history.

### Practice
1. On a real feature branch, before opening a PR, use interactive rebase to turn a messy WIP commit history into a small number of clean, well-described commits — practice this as a pre-PR habit, not a one-off exercise.

### Answer This
- Why is Trunk-Based Development the branching model most modern CI/CD practices implicitly assume?

---

## 13.2 — Linux & Bash for Operations

**Tier: 👀 Know Only**

### What to Learn
- File permissions (`chmod`/`chown`/`umask`, Phase 0.1) applied to real production file/log/secret handling.
- Processes (`ps`/`top`/`htop`/`kill`, `systemd` services) for operating a real long-running service, not just observing a local dev process.
- SSH (key-based auth, `ssh-agent`, bastion hosts, port forwarding, Phase 0.2) for accessing real infrastructure securely.
- Shell scripting discipline — `set -euo pipefail` (exit immediately on any command failure, unset variable use, or pipeline failure — the default posture for any script that touches production, since silent partial failures are far worse than loud immediate ones); `flock` to prevent two instances of the same cron job from overlapping and corrupting shared state.
- `strace`/`ltrace`/`perf`/eBPF for tracing what a running process is actually doing at the syscall/kernel level when something behaves mysteriously — genuinely useful debugging tools beyond just "add more logging."
- `netstat`/`ss`, `tcpdump` for diagnosing real network-level issues in production (Phase 1.4).

### Practice
1. Write a real operational shell script (e.g., a deploy or backup script) using `set -euo pipefail` from the first line, and deliberately test that it actually halts on an unexpected failure partway through, rather than silently continuing.
2. Use `strace` on a real running process to observe what syscalls it's actually making, and use that to explain a piece of its behavior you couldn't have determined from application logs alone.

### Answer This
- Why is `set -euo pipefail` considered close to a non-negotiable default for any script that touches production state?
- What can `strace` reveal about a process's behavior that application-level logging fundamentally can't?

---

## 13.3 — Containerization: Docker

**Tier: 🎯 Master**

### What to Learn
- **Images vs containers** — an image is an immutable blueprint; a container is a running instance of one — the same distinction as a class and an object.
- **Dockerfile instructions** — `FROM`, `WORKDIR`, `COPY`, `RUN`, `EXPOSE`, `ENV`, `ENTRYPOINT`, `CMD` — and **layer caching**: Docker caches each instruction's result as a layer, so ordering instructions from least-frequently-changing (dependencies) to most-frequently-changing (your actual application code) means most builds only need to re-run the last few layers, dramatically speeding up iterative builds.
- **Multi-stage builds** — build your application in one stage (with the full build toolchain — `npm ci` + `tsc`/`nest build` for NestJS now, or the full JDK/Maven for Spring Boot later), then copy only the compiled artifact into a minimal runtime-only final image (a slim `node:alpine` base now, a JRE-only base later) — the build tools never ship in the final image at all.
- **`.dockerignore`** — exclude irrelevant files from the build context so they can't accidentally get copied in or bloat build time.
- **Docker networking** — bridge (default, isolated per-host network), host (container shares the host's network namespace directly), none.
- **Docker Compose** — define a multi-service local environment (app + database + cache + queue) declaratively, so `docker compose up` reproduces the same environment for every developer.
- **Container security** — run as a non-root `USER` inside the container (a container running as root, if ever escaped, has root on the host too — a real, serious risk); never bake secrets into an image layer (even a "deleted" file in an earlier layer is still recoverable from the image's layer history); scan images for known CVEs (Snyk/`docker scan`); prefer minimal base images (`alpine`/`distroless`) to shrink both attack surface and image size.
- **Named volumes** (for persistent data like a database's files, managed by Docker) vs **bind mounts** (map a host directory directly into the container, typically used for local dev hot-reload).

### Practice
1. Write a genuine multi-stage Dockerfile for a NestJS application: `npm ci` + build in one stage, copy only the compiled `dist/` output and production `node_modules` into a minimal `node:alpine` final stage, and compare the final image size against a naive single-stage build that includes the full dev toolchain. Repeat this exercise later for the Spring Boot rebuild (Maven build stage → minimal JRE-only final stage) and compare the two Dockerfiles' shape.
2. Write a `docker-compose.yml` bringing up NestJS + PostgreSQL + Redis together (Redis doubling as your BullMQ backend), and confirm a fresh developer machine can run the entire stack with one command. Add a RabbitMQ service when the Track B/Spring Boot version arrives later.
3. Deliberately bake a secret into an early Dockerfile layer, then delete it in a later layer, and confirm — by inspecting the image's layer history — that the "deleted" secret is still recoverable, demonstrating why secrets must never enter any layer at all, not even temporarily.

### Answer This
- Why does layer-ordering in a Dockerfile (dependencies before app code) meaningfully speed up iterative rebuilds?
- Why is a container running as root a real host-level security risk, not just a container-scoped one?
- Why is a secret "deleted" in a later Dockerfile layer still actually present and recoverable in the built image?

---

## 13.4 — CI/CD: GitHub Actions

**Tier: 🎯 Master**

### What to Learn
- **Workflows, jobs, steps** — the structural hierarchy; jobs run in parallel by default, `needs` creates explicit ordering dependencies between them.
- **Secrets** (`${{ secrets.X }}`) — never hardcoded in workflow files, which are plain text and typically version-controlled and visible to anyone with repo read access.
- **Environment variables** at workflow/job/step scope, chosen deliberately based on how widely a value needs to be visible.
- **Matrix builds** — run the same job across multiple parameter combinations (e.g., multiple Java or Node versions) without duplicating the job definition.
- **Dependency caching** (Maven/npm) between runs — a real, direct lever on total pipeline time.
- **Artifacts** — pass build outputs (test reports, compiled binaries) between jobs or preserve them for later inspection.
- **Environments and required-reviewer approvals** — a genuine staging-to-production gate enforced by the platform itself, not just a team convention that can be accidentally skipped.
- **Reusable workflows** — DRY (Phase 3.1) applied to pipeline configuration itself, avoiding copy-pasted YAML across many repos.

### Practice
1. Build a real pipeline: on every push/PR, lint → unit test → integration test (with Testcontainers, Phase 5A.8/11.4 now, 5B.7/11.4 later) → coverage gate → build a Docker image → push to a registry on merge to `main` → deploy to staging automatically → require an explicit human approval before deploying to production.
2. Add Maven/npm dependency caching to the pipeline and measure the real time saved on a second run versus a cold one.

### Answer This
- Why must secrets be injected via the platform's secrets mechanism rather than hardcoded in a workflow file, even in a private repo?
- What real problem does a required-approval "environment" gate solve that a team's verbal agreement not to deploy without checking wouldn't reliably solve?

---

## 13.5 — Container Orchestration: Kubernetes

**Tier: ⏸️ Skip For Now**

### What to Learn (working awareness level, not deep operator/CRD mastery)
- **Pod, Deployment, Service, Ingress** — the core objects: a Pod is the smallest deployable unit (one or more tightly-coupled containers); a Deployment manages a desired number of Pod replicas and handles rolling updates; a Service gives a stable network identity/address to a dynamic set of Pods; Ingress handles external HTTP routing into the cluster.
- **ConfigMap & Secret** — externalized configuration and sensitive values, injected into Pods rather than baked into images (directly connects to 13.3's "never bake secrets into an image" principle).
- **Horizontal Pod Autoscaler** — scale replica count automatically based on real metrics (CPU, memory, or custom metrics).
- **Rolling deployments** — replace Pods gradually, maintaining availability throughout (Phase 13.10).
- **Liveness vs readiness probes** — the exact same distinction as Phase 10.2's liveness/readiness health checks, now expressed as literal Kubernetes configuration that determines whether the orchestrator restarts a Pod (liveness) or simply stops routing traffic to it temporarily (readiness).
- **Resource limits** — explicit CPU/memory requests and limits per container, preventing one misbehaving Pod from starving others on the same node.

### Practice
1. Deploy a simple containerized app to a local Kubernetes cluster (Minikube or similar), configure both a liveness and a readiness probe with genuinely different conditions, and deliberately break each independently to confirm Kubernetes reacts differently (restarting the Pod vs just removing it from load-balancing).
2. Set resource limits on a Pod and deliberately exceed them (e.g., an intentional memory leak) to observe Kubernetes's actual enforcement behavior (e.g., an OOM-kill).

### Answer This
- Why does a failing readiness probe cause different orchestrator behavior than a failing liveness probe?
- Why do resource limits on one Pod matter for the health of other, unrelated Pods on the same node?

---

## 13.6 — Infrastructure as Code: Terraform

**Tier: ⏸️ Skip For Now**

### What to Learn
- **Declarative infrastructure** — describe the desired end state; Terraform figures out the sequence of API calls needed to get there, rather than you scripting each step imperatively.
- **Core workflow** — `terraform init` (set up providers/modules) → `terraform plan` (show exactly what would change, without changing anything yet) → `terraform apply` (actually make the change) → `terraform destroy`.
- **Providers, resources, variables, outputs** — the basic building blocks of any Terraform configuration.
- **Remote state** — storing `terraform.tfstate` in a shared backend (e.g., S3) with locking (e.g., a DynamoDB lock table), specifically to prevent two team members from applying conflicting changes concurrently against a local, un-synced state file — a real, common source of infrastructure drift and corruption if skipped.
- **Modules** — reusable, parameterized infrastructure components, avoiding copy-pasted resource definitions across environments.
- **Data sources** — read information about infrastructure that already exists (not managed by this particular Terraform configuration) so you can reference it without re-declaring it.

### Practice
1. Provision a real, small AWS stack (VPC, EC2, RDS, S3, IAM roles) with Terraform, using remote state with locking, and confirm — by deliberately running `terraform plan` from two different terminals "simultaneously" — that the lock actually prevents a concurrent apply.
2. Extract one piece of the stack into a reusable module and instantiate it twice (e.g., for two environments) with different variable values.

### Answer This
- Why is local (unshared) Terraform state a genuine risk for any team of more than one person?
- What specifically does `terraform plan` let you catch before it's too late to easily undo?

---

## 13.7 — AWS Core Services

**Tier: 👀 Know Only**

### What to Learn

| Service | What to Know |
|---------|--------------|
| EC2 | Instance types, security groups, key pairs, AMIs, user data scripts |
| S3 | Bucket policies, presigned URLs, multipart uploads, static hosting, lifecycle rules |
| RDS | Managed PostgreSQL, Multi-AZ for HA, read replicas, automatic backups |
| ElastiCache | Managed Redis, cluster mode, replication groups |
| CloudFront | CDN, distributions, cache behaviors, invalidations |
| IAM | Least privilege, roles over long-lived users, never root for daily work |
| CloudWatch | Log groups, metrics, dashboards, alarms |
| Route 53 | Hosted zones, A/CNAME/alias records, health checks, routing policies |
| SQS / SNS | Standard vs FIFO queues, visibility timeout, dead letter queues |
| ECS / Fargate | Task definitions, services, load balancer integration — containers without managing raw servers |
| Lambda | Cold starts, execution time limits, event triggers, statelessness |
| ACM | Free, auto-renewing TLS certificates, integrates with ALB/CloudFront |

- **IAM least privilege specifically** — a role/policy should grant exactly the permissions a service needs and nothing more; overly broad IAM policies are one of the most common real-world cloud security incidents, not a theoretical risk.

### Practice
1. Provision a real small application stack across at least EC2 (or ECS/Fargate), RDS, S3, and CloudFront, using IAM roles scoped as narrowly as you can reasonably make them — deliberately try to over-scope one role first, then narrow it down and confirm the application still works with the reduced permission set.

### Answer This
- Why is an overly broad IAM policy a real, common incident cause rather than just theoretical caution?
- Why would you use an IAM role instead of a long-lived IAM user's access keys for an application running on EC2/ECS?

---

## 13.8 — Web Servers & Proxies

**Tier: 👀 Know Only**

### What to Learn
- **Nginx** — reverse proxy, load balancing, SSL/TLS termination, rate limiting, and static file serving — one tool covering several of the concerns from Phase 10.4/10.5 in a real, concrete configuration file.
- **Let's Encrypt + Certbot** — free, automatically-renewing TLS certificates, removing "we forgot to renew the cert" as an entirely avoidable class of outage.

### Practice
1. Configure Nginx as a reverse proxy in front of a real application, with TLS termination via Let's Encrypt/Certbot, and confirm automatic renewal is actually configured (not just "it worked once").

### Answer This
- Why does SSL/TLS termination at the reverse proxy (rather than in the application itself) make sense as a default architecture?

---

## 13.9 — Monitoring & Observability

**Tier: 👀 Know Only**

### What to Learn
- **The three pillars** — logs (structured JSON, correlation IDs, Phase 5A.6 now / 5B.5 later), metrics (the **RED method** — Rate, Errors, Duration — a solid default framework for what to measure on any service), distributed tracing (following one request's full path across multiple services).
- **Prometheus** — scrapes metrics (e.g., from a `@nestjs/terminus`-exposed endpoint now, a Spring Boot Actuator endpoint later, Phase 5A.6/5B.5) and stores them as time series; **Grafana** — builds dashboards and alerts on top of that data; **Sentry** — captures real-time application errors with full stack traces and context.
- **SLIs, SLOs, SLAs** — an SLI is the actual measured metric (e.g., request success rate); an SLO is your internal target for that metric (e.g., 99.9%); an SLA is the external, often contractual, commitment to a customer — these three are related but distinct, and conflating them causes real miscommunication about what's actually being promised to whom.
- **Error budget** — the acceptable amount of "badness" (downtime, error rate) implied by your SLO, treated as a real, spendable budget — once it's exhausted for a period, the team should prioritize reliability work over new features, as a genuine, pre-agreed policy rather than an ad-hoc argument each time.

### Practice
1. Set up Prometheus scraping a `@nestjs/terminus`/metrics endpoint (a Spring Boot Actuator endpoint later, with Track B) and build a Grafana dashboard showing request rate, error rate, and p95/p99 latency (the RED method, made concrete).
2. Define a specific SLO for a real endpoint (e.g., "99.9% of requests complete under 500ms") and set up a Grafana alert that fires when the error budget for a rolling window is close to exhausted.

### Answer This
- What's the practical difference between an SLI, an SLO, and an SLA, and why does conflating them cause real confusion with stakeholders?
- What is an error budget actually meant to do, organizationally, once it's spent?

---

## 13.10 — Deployment Strategies

**Tier: 👀 Know Only**

### What to Learn

| Strategy | When to Use |
|----------|-------------|
| Recreate | Dev/staging only — has real downtime |
| Rolling | Zero downtime — gradual instance replacement |
| Blue-Green | Instant rollback — two full identical environments |
| Canary | Safest — release to a small % of real users first |
| Feature Flags | Decouples the act of deploying code from the act of releasing a feature to users |

- Each strategy is a genuine tradeoff between deployment complexity/cost and blast-radius/rollback-speed if something goes wrong — there isn't one universally correct choice.

### Practice
1. Implement a rolling deployment for a real service and deliberately deploy a broken version, confirming how quickly (and how) you can detect and roll it back mid-rollout.
2. Implement a feature flag around one real feature and demonstrate deploying the code with the flag off, then separately "releasing" it by flipping the flag — without a new deployment.

### Answer This
- Why does "deploy" not have to mean the same thing as "release" once feature flags are in the picture?
- What specifically does Blue-Green buy you over Rolling, at what specific cost?

---

## 13.11 — Backups & Cron

**Tier: ⏸️ Skip For Now**

### What to Learn
- **The 3-2-1 backup rule** — 3 copies of data, on 2 different media types, with 1 copy offsite — a simple, durable heuristic against many different failure modes at once (hardware failure, site-level disaster, accidental deletion).
- **Automated restore testing** — a backup you've never actually restored from is an unverified assumption, not a real safety net; point-in-time recovery capability specifically matters when the failure is a bad deploy or bad data change, not just total data loss.
- **Cron job discipline** — correct syntax, explicit logging (so a silently-failing cron job doesn't go unnoticed for months), and `flock` (13.2) to prevent overlapping runs of the same job corrupting shared state.

### Practice
1. Take a real backup of a database and actually perform a full restore from it into a separate environment — confirm it works, don't just assume the backup process "must be fine" because it completes without an error.

### Answer This
- Why is an untested backup not actually a safety net, regardless of how reliably the backup job itself appears to run?

---

## Phase 13 Capstone

> **PROJECT:** Deploy your Master Project to AWS using EC2/ECS + RDS + ElastiCache + S3 + CloudFront, entirely provisioned via Terraform with remote state and locking. Set up GitHub Actions to deploy automatically on every merge to `main`, with a required-approval gate before production. Add Prometheus + Grafana dashboards using the RED method, with real alerts for error rate and latency tied to an explicit SLO you've defined. Perform an actual full restore from a real backup into a separate environment, and document that it worked.

---

# Phase 14 — Security Fundamentals

**Phase tier: 🎯 Mostly Master — OWASP/auth/attack vectors/cryptography fundamentals Master (non-negotiable); Zero Trust/WAF/SIEM/compliance Skip**

> Security is a property of every feature, not a feature itself. The question is never "will we be attacked?" — it's "when, and will we be ready?"

## 14.1 — Security Mindset

**Tier: 🎯 Master**

### What to Learn
- **Think like an attacker** — for every endpoint/feature, ask "how would someone abuse this specifically?" as a deliberate design step, not an afterthought.
- **Defense in depth** — multiple independent layers of protection, so a single failure doesn't mean total compromise.
- **Principle of least privilege** — every user, service, and credential gets only the access it actually needs, nothing more (directly echoes Phase 13.7's IAM discussion).
- **Assume breach** — design as though an attacker may already have a foothold somewhere; this changes what you design (e.g., internal traffic that still verifies identity, Phase 14's Zero Trust awareness) versus assuming the perimeter alone is sufficient.

### Practice
1. Pick one real endpoint you've built and spend 15 minutes deliberately trying to think of ways to abuse it (not attack it technically yet, just brainstorm abuse scenarios) before reading further into this phase — compare your list against what you find in 14.2–14.5.

### Answer This
- Why does "assume breach" change a design, concretely, compared to designing around "keep attackers out entirely"?

---

## 14.2 — OWASP Top 10

**Tier: 🎯 Master**

### What to Learn

| # | Vulnerability | Prevention |
|---|--------------|-----------|
| 1 | Broken Access Control | Server-side authorization on every request; check ownership, not just authentication |
| 2 | Cryptographic Failures | TLS everywhere, Argon2 for passwords, AES-256 at rest |
| 3 | Injection | Parameterized queries always — never concatenate user input |
| 4 | Insecure Design | Threat model before building (STRIDE framework) |
| 5 | Security Misconfiguration | No default credentials, debug mode off in prod, no public S3 buckets |
| 6 | Vulnerable Components | Snyk/Dependabot in CI, patch regularly, SBOM generation |
| 7 | Authentication Failures | Rate limiting, MFA, secure session management |
| 8 | Data Integrity Failures | Code signing, verify dependency integrity |
| 9 | Logging & Monitoring Failures | Log all auth events and anomalies |
| 10 | SSRF | Whitelist URLs, block internal IP ranges |

- Memorizing the list is far less valuable than being able to point to a real, specific example of each in a codebase you've seen, and explaining the concrete fix.

### Practice
1. For each of the 10, write one concrete real (or plausible) example from a real system you know, and the specific one-line fix — force yourself past the generic description into a genuine scenario.

### Answer This
- Why is "checking the user is logged in" not the same as fixing Broken Access Control?
- Why does a Security Misconfiguration (like debug mode left on in production) count as a top-tier vulnerability class, rather than "just sloppiness"?

---

## 14.3 — Authentication Deep Dive

**Tier: 🎯 Master**

### What to Learn
- **Password storage** — never plaintext, never MD5/SHA-1 (fast hashes, exactly the wrong property for passwords); **Argon2id** (or bcrypt) is deliberately slow and tunable, making brute-forcing computationally expensive even if the hash database is stolen.
- **JWT structure** — `header.payload.signature`, all Base64URL-encoded (not encrypted by default — anyone can *read* a JWT's payload, they just can't forge a valid signature without the key); signing — HS256 (symmetric, one shared secret both signs and verifies — if that secret leaks anywhere, forgery becomes possible from that single point) vs RS256 (asymmetric — a private key signs, a public key verifies, so the verifying party never needs the secret that could forge a token).
- **Access + refresh token pattern** — short-lived access tokens (minutes) limit the damage window if one leaks; long-lived refresh tokens (days/weeks) are used only to mint new access tokens, and should be **rotated on every use** — issuing a new refresh token each time and invalidating the old one, so that if a stolen refresh token is ever used by an attacker *and* later by the legitimate user (or vice versa), the mismatch is detectable as a signal of theft.
- **OAuth2 flows** — Authorization Code + PKCE (the correct flow for public clients like SPAs and mobile apps, since PKCE prevents an intercepted authorization code from being exchanged by anyone other than the party that initiated the flow) vs Client Credentials (machine-to-machine, no human user involved at all).
- **TOTP (MFA)** — a shared secret plus the current time produces a verifiable, time-limited one-time code, without any network round-trip needed to generate it.
- **Storage** — store JWTs in `httpOnly` cookies rather than `localStorage`, specifically because JavaScript (and therefore an XSS payload) cannot read an `httpOnly` cookie's value at all, closing off the most common practical path for token theft.

### Practice
1. Decode a real JWT's payload (base64 decode by hand or with a tool, not a library) and confirm you can read its claims without ever verifying the signature — directly demonstrating that a JWT's payload is not confidential by default.
2. Implement refresh token rotation and write a test simulating token theft: use the same (now-stale) refresh token twice and confirm the system detects and reacts to the reuse (e.g., by invalidating the whole token family) rather than silently accepting it.

### Answer This
- Why is a JWT's payload readable by anyone, even though it can't be forged without the signing key?
- Why does RS256 avoid a risk that HS256 carries, specifically about where the "forging" capability lives?
- What specific attack does refresh token rotation make detectable that a non-rotating refresh token wouldn't reveal at all?
- Why does storing a JWT in an `httpOnly` cookie close off a specific, common theft vector that `localStorage` doesn't?

---

## 14.4 — Authorization

**Tier: 🎯 Master**

### What to Learn
- **RBAC** (Role-Based Access Control — permissions attached to roles, users assigned roles) vs **ABAC** (Attribute-Based Access Control — permissions computed from attributes of the user, resource, and context, enabling finer-grained rules than a fixed role set can express).
- **IDOR (Insecure Direct Object Reference)** — an endpoint that trusts a client-supplied ID (`GET /orders/456`) without verifying the *current* authenticated user actually owns/may access order 456 — one of the most common, easy-to-introduce, and easy-to-miss real vulnerabilities.
- **Always enforce authorization server-side** — a client-side check (hiding a button, disabling a menu item) is a UX nicety, never a security control, since any client-side check can be trivially bypassed by calling the API directly.

### Practice
1. Deliberately build (or find) an endpoint that checks authentication but not resource ownership, then access another user's resource by simply changing an ID in the request — confirm the IDOR vulnerability exists, then fix it with an explicit server-side ownership check and confirm the same request is now correctly rejected.

### Answer This
- Why is IDOR specifically a failure of authorization, not authentication — what did the attacker actually have to prove to exploit it?
- Why is a client-side-only permission check never a real security boundary?

---

## 14.5 — Common Attack Vectors

**Tier: 🎯 Master**

> Learn these by doing, on PortSwigger's Web Security Academy — reading about them is a poor substitute for actually exploiting and then fixing one yourself.

### What to Learn
- **SQL injection** — `' OR '1'='1` and similar payloads exploit string-concatenated queries; parameterized queries are the fix, full stop — not "sanitizing" input as a substitute.
- **XSS (Cross-Site Scripting)** — stored (payload persisted and served to other users later), reflected (payload bounced back immediately in a response, e.g., via a query param), DOM-based (payload never even touches the server, purely a client-side JS vulnerability); prevented by output encoding, a Content Security Policy, and `httpOnly` cookies (limiting the *damage* even if an XSS does occur).
- **CSRF** — a malicious site tricks a logged-in user's browser into making an unwanted authenticated request to your site (using their real, valid session cookie); prevented by `SameSite` cookies and CSRF tokens for state-changing requests.
- **IDOR** — see 14.4.
- **JWT attacks** — the `alg: none` attack (an attacker sets the algorithm header to "none" and strips the signature — a naive verifier that trusts the header's claimed algorithm can be fooled into accepting an unsigned token) and weak/guessable secrets (an HS256 secret that's short or reused can be brute-forced, letting an attacker forge valid tokens).

### Practice
1. Complete the SQL injection and XSS learning paths on PortSwigger Web Security Academy end to end — actually exploit each vulnerability class yourself in their labs, then articulate the fix for each in your own words.
2. Build a deliberately vulnerable toy endpoint (e.g., string-concatenated SQL, or unescaped output reflecting a query param), attack it yourself with a real payload, confirm the exploit works, then fix it and confirm the same payload no longer succeeds.

### Answer This
- Why is "sanitizing" input treated as a weaker fix than parameterized queries for SQL injection?
- What's the concrete difference between stored, reflected, and DOM-based XSS, in terms of where the payload actually lives?
- Why does the `alg: none` JWT attack succeed against a naive verifier — what assumption is that verifier wrongly trusting?

---

## 14.6 — Security Headers

**Tier: 🎯 Master**

### What to Learn
```
Content-Security-Policy
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000; includeSubDomains
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy
```
- Each header closes off a specific, real attack surface: CSP restricts which resources/scripts can execute at all (a real defense-in-depth layer against XSS even if an injection somehow occurs); `X-Frame-Options`/CSP `frame-ancestors` prevent clickjacking (your page being embedded in an invisible iframe on an attacker's site to trick users into clicking something they didn't mean to); HSTS forces browsers to only ever connect via HTTPS for your domain, closing a downgrade-attack window.

### Practice
1. Add all of these headers to a real application and verify, using a browser's dev tools or an online header-scanning tool, that each is actually present and correctly configured — not just "added to some config that may or may not be applied."

### Answer This
- Why does CSP act as a meaningful defense-in-depth layer against XSS, even for an app that's already careful about output encoding?
- What real attack does `X-Frame-Options: DENY` specifically prevent?

---

## 14.7 — Cryptography Fundamentals

**Tier: 🎯 Master**

### What to Learn
- **Symmetric encryption** (AES-256 — the same key both encrypts and decrypts, fast, but requires securely sharing that one key with every party that needs it) vs **asymmetric encryption** (RSA/ECC — a public key encrypts, only the corresponding private key decrypts, solving the key-distribution problem at the cost of being much slower — which is why real systems like TLS use asymmetric crypto briefly to establish a shared symmetric key, then switch to symmetric encryption for the actual bulk data).
- **Hashing** — SHA-256 for integrity verification (fast, deterministic, one-way) vs Argon2/bcrypt for passwords specifically (deliberately slow, salted, tunable) — using a fast general-purpose hash like SHA-256 for passwords is itself a real, common mistake, not just "using an old algorithm."
- **Digital signatures** — prove both authenticity (who signed it) and integrity (it hasn't been altered since signing), the mechanism underlying RS256 JWTs (14.3) and TLS certificate validation (Phase 1.4).
- **Key management** — never hardcode keys in source code or config files committed to Git; rotate keys regularly and immediately after any suspected compromise or team member departure; use a real secrets system (AWS KMS, HashiCorp Vault) rather than ad-hoc handling.

### Practice
1. Hash the same password with SHA-256 and with bcrypt/Argon2, and compare the actual computation time of each — make the "fast hash is wrong for passwords" lesson concrete and measured, not just asserted.

### Answer This
- Why does TLS use asymmetric crypto only briefly, then switch to symmetric encryption for the actual data transfer?
- Why is using SHA-256 directly for password hashing a real mistake, given that it's a perfectly legitimate hash for other purposes?

---

## 14.8 — Secrets Management

**Tier: 🎯 Master**

### What to Learn
- Never commit secrets to Git — ever, including in a "private" repo, since history persists and access can change over time.
- Environment variables are acceptable for local development; production should use a real secrets manager — **AWS Secrets Manager**/Parameter Store or **HashiCorp Vault** — rather than environment variables baked into a deployment config, since a real secrets manager provides access auditing, rotation support, and fine-grained access control that plain env vars don't.
- Rotate credentials on a regular schedule *and* immediately, unconditionally, after any team member's departure — not "if we remember to."

### Practice
1. Set up a real secret in AWS Secrets Manager (or HashiCorp Vault) and have an application retrieve it at runtime instead of reading it from an environment variable baked into the deployment — confirm you can rotate the secret without redeploying the application.

### Answer This
- Why is a real secrets manager a meaningfully stronger practice than environment variables in production, beyond just "it's more official"?
- Why must credential rotation after a team member's departure be unconditional rather than judgment-based ("only if we think they might misuse it")?

---

## 14.9 — API Security

**Tier: 🎯 Master**

### What to Learn
- Reiterating and applying Phase 7.4 specifically as a security-fundamentals discipline: authentication on every non-public endpoint with no silent exceptions; distributed rate limiting (per-user and per-IP, separately, Phase 8.6); input validation at the API boundary regardless of what the frontend already checks (14.4's client-side-is-not-a-boundary principle); output filtering to avoid ever returning fields the caller isn't authorized to see; API key hygiene (sufficient entropy, rotation, scope restriction, never embedded in client-side code where it's trivially extractable); SPF/DKIM/DMARC for email deliverability and anti-spoofing, specifically relevant to any API that sends transactional email.

### Practice
1. Audit one real API you've built against this exact checklist, endpoint by endpoint, and fix the first real gap you find.

### Answer This
- Why does an API key embedded in frontend/client code provide essentially no real security, regardless of how "hidden" it looks in minified code?

---

## Phase 14 Capstone

> **PROJECT:** Build a complete auth system: Argon2 password hashing, JWT access tokens + refresh token rotation (with a working test proving reuse of a stale refresh token is detected), rate limiting on the login endpoint, and TOTP-based MFA. Then, deliberately build a vulnerable endpoint (missing ownership check → IDOR, or string-concatenated SQL → injection), attack it yourself using OWASP ZAP or manually, fix it, and confirm the fix holds. Complete the SQL injection and XSS learning paths on PortSwigger Web Security Academy. Generate an SBOM for a real project and review every dependency for known CVEs.

---

## Ecosystem Awareness (All of Phase 14, Beyond the Core)

**Tier: ⏸️ Skip For Now**

> Awareness level — recognize these when they come up, know roughly what they solve, don't expect production mastery yet.

- **Zero Trust Architecture** — no implicit trust even for traffic already inside your own network; every request is verified regardless of its origin; micro-segmentation limits how far an attacker can move laterally even after breaching one component.
- **WAF (Web Application Firewall)** — filters/monitors HTTP traffic at the edge against known attack patterns (OWASP Core Rule Set), plus bot detection and DDoS mitigation (AWS WAF, Cloudflare WAF, ModSecurity).
- **SIEM (Security Information and Event Management)** — aggregates logs across every system, applies correlation rules to detect attack patterns spanning multiple systems that no single log would reveal alone, and integrates with incident response.
- **Penetration testing basics** — reconnaissance, vulnerability scanning, exploit validation, and reporting, as a structured process (Burp Suite, OWASP ZAP) rather than ad-hoc poking.
- **Compliance awareness** — not full expertise, but knowing what each standard actually requires *from the code you write*:

| Standard | What It Requires from Engineering |
|----------|-----------------------------------|
| GDPR | Data minimization, right to erasure, 72h breach notification, encryption at rest/transit |
| PCI-DSS | Never store raw card data, tokenization, audit logs, quarterly vulnerability scans |
| SOC2 | Availability/security/confidentiality controls, audit trail, access reviews |
| HIPAA | Encryption, access controls, audit logs, minimum-necessary data access |

### Answer This
- Why does Zero Trust treat internal network traffic with the same suspicion as external traffic?
- What real gap does a SIEM's cross-system correlation close that reviewing any single system's logs in isolation wouldn't?
