# Projects & Practice Companion
### Turning the roadmap into things you actually build

> Reading explains a mechanism. Building it is what makes it yours. This file is the "what do I build this week" companion to the roadmap — for every phase, it gives you **Quick Wins** (small, focused, hours not days), a **Milestone Project** (a real, standalone thing you ship in days, with a concrete "done when" checklist), and where relevant, a note on how that milestone's code becomes part of **The Master Project** (Part VI) later.
>
> **How to use this file:**
> 1. Study the phase's content (in the master roadmap or the relevant `Part_*_Detailed.md`) until the "Answer This" questions there feel easy.
> 2. Do the Quick Wins below *as you go* — don't wait until the whole phase is "done" to start building. Small and immediate beats big and deferred.
> 3. Ship the Milestone Project before moving to the next phase. If it's not shippable (runs, does the thing, has no glaring bug), it's not done.
> 4. Where a phase's milestone note says "feeds the Master Project," keep that code — you'll fold it into the SaaS platform in Part VI rather than starting over.
> 5. No deadlines here either. A milestone taking three weeks instead of three days is fine — the checklist, not the clock, defines "done."

---

## Table of Contents
- [Part 0 — Foundation Reset](#part-0--foundation-reset)
- [Phase 1 — Computer Science Fundamentals](#phase-1--computer-science-fundamentals)
- [Phase 2 — Data Structures & Algorithms](#phase-2--data-structures--algorithms)
- [Phase 3 — Software Design & Clean Code](#phase-3--software-design--clean-code)
- [Project Lifecycle & Planning](#project-lifecycle--planning)
- [Phase 4 — Frontend Engineering](#phase-4--frontend-engineering)
- [Phase 5B — Backend: Java + Spring Boot (the single backend track)](#phase-5b--backend-java--spring-boot-the-single-backend-track)
- [Phase 6 — Mobile (React Native)](#phase-6--mobile-react-native)
- [Phase 7 — API Design](#phase-7--api-design)
- [Phase 8 — Databases](#phase-8--databases)
- [Phase 9 — Data Engineering](#phase-9--data-engineering)
- [Phase 10 — System Design & Architecture](#phase-10--system-design--architecture)
- [Phase 11 — Testing & Quality Engineering](#phase-11--testing--quality-engineering)
- [Phase 12 — Performance Engineering](#phase-12--performance-engineering)
- [Phase 13 — DevOps & Infrastructure](#phase-13--devops--infrastructure)
- [Phase 14 — Security Fundamentals](#phase-14--security-fundamentals)
- [Phase 15-17 — Becoming Senior (non-code practice)](#phase-15-17--becoming-senior-non-code-practice)
- [The Master Project — Full Build Order](#the-master-project--full-build-order)
- [Mini-Project Ladder](#mini-project-ladder)
- [Practice App Spec Cards (HabitHub)](#practice-app-spec-cards-habithub)

> **Also see:** `Exercises_And_Drills.md` (the small reps: Drills, Builds, Break-it labs) and `Progress_Tracker.md` (where you log them). The ladder and spec cards at the bottom of this file fill the gap between a Quick Win (hours) and a Milestone (a week-plus).

---

## Part 0 — Foundation Reset

### Quick Wins
- Provision a fresh Ubuntu VM (or WSL2) from a bare install script — no clicking through a GUI installer by hand twice.
- Parse a real log file (any framework's dev log will do) with `grep`+`awk`+`sed` to produce "error count per endpoint."
- Make three messy commits on a throwaway branch, then clean them into one coherent commit with interactive rebase.
- Build a responsive 3-column-to-1-column layout from a mockup using zero CSS frameworks — pure box model, Flexbox/Grid.

### Milestone Project — Raw HTTP Server
Build an HTTP/1.1 server from scratch using raw TCP sockets (Java's `ServerSocket` — no `HttpServer`, no Spring). It must parse the raw request line/headers/body itself, and respond with correct status lines and headers.

**Done when:**
- `curl -v` against it shows a well-formed response (status line, headers, blank line, body) for at least GET and POST.
- It serves at least two routes with different responses.
- You can explain, from your own code, exactly where in the raw byte stream the headers end and the body begins.

---

## Phase 1 — Computer Science Fundamentals

### Quick Wins
- Convert 20 numbers decimal↔binary↔hex by hand until it's automatic, no calculator.
- Iterate a large 2D array row-first vs column-first in JS, time both, and explain the gap from cache locality.
- Write a script that spawns concurrent async operations sharing a variable, and deliberately produce a race condition.
- Capture a real DNS query and TLS handshake in Wireshark; narrate what each packet is doing.

### Milestone Project — Toy Memory/Cache Simulator
A small CLI tool (any language) that simulates a 2-level cache (L1 + "RAM") over a simple memory array: given a sequence of read addresses, it reports hits/misses per level and total simulated latency, using realistic relative latency numbers.

**Done when:**
- Feeding it a sequential-access pattern vs a random-access pattern over the same data produces visibly different hit rates.
- You can explain, in your own words, why sequential access wins — tied back to cache lines, not just "because."

---

## Phase 2 — Data Structures & Algorithms

> Every subsection in `Part_I_Core_Engineering_Foundations_Detailed.md` already has a "Build From Scratch" exercise (linked list, stack/queue, hash map, BST, heap, graph, sorts, DP problems). Do those in order first — they're the Quick Wins for this phase, not repeated here.

### Milestone Project — A Task Scheduler CLI, Built on Your Own Structures
A command-line task scheduler that uses **only your own from-scratch implementations** (no `HashMap`/`PriorityQueue`/etc. from a standard library) for its core mechanics: your hash map for task lookup by ID, your min-heap for "next task by priority/deadline," your graph + topological sort for task dependencies (task B can't run before task A finishes).

**Done when:**
- `add-task`, `complete-task`, `next-task` (pops the highest-priority ready task), and `list-blocked` all work from the CLI.
- Adding a circular dependency is detected and rejected, not silently accepted.
- You can point at any operation and name its Big-O, backed by which structure is doing the work.

---

## Phase 3 — Software Design & Clean Code

### Quick Wins
- Take a real function from your own PHP/Laravel or JS codebase and refactor it applying Clean Code (naming, guard clauses, no magic numbers) — before/after diff, explain each change.
- Find and fix one real SOLID violation in your own code (a service doing 3+ unrelated things is the easiest to spot).
- Implement Observer (a tiny event emitter), Strategy (swap-able discount/tax calculators), and Factory (notification: email vs SMS vs push) as three standalone mini-exercises.

### Milestone Project — Refactor Kata + Layout
Take any messy 200+ line file (yours, or a deliberately bad one you write first) and refactor it in stages: extract methods → apply SOLID → introduce 2 real design patterns where they fit naturally (not forced) → lay it out per the Clean Architecture folder structure from 3.4.

**Done when:**
- Behavior is provably unchanged (a test suite, or manual verification, passes identically before and after).
- You can point to each pattern used and say *why* it fit here, not just that you used it.
- No function is longer than ~20 lines without a specific, stated reason.

---

## Project Lifecycle & Planning
*(Not a numbered phase — see `Project_Lifecycle_And_Planning.md`, 🎯 Master. Do this milestone before starting the Master Project's build.)*

### Quick Wins
- Write a 1-page brief + 3-5 user stories with real Given/When/Then acceptance criteria for a small idea (even a throwaway one).
- Draw a C4 Container diagram in Mermaid for an app you've already built (Phase 4/5 milestone) — retrofit the diagram to reality, not the other way around.
- Draw the ERD for that same app's actual current schema and check it against what you'd have designed on paper first.
- Run a lightweight solo Event Storming pass (sticky notes or a virtual board) on one feature idea — 45 minutes, timer on.

### Milestone Project — Plan the Master Project Properly, Before Building It
Before writing the Master Project's first line of code (Phase 5B milestone), produce: the 1-page brief, 5 core user stories with acceptance criteria, a C4 Container diagram, an ERD for the core entities, one sequence diagram (the refresh-token flow is a good first pick), and an initial backlog in GitHub Projects (or your tool of choice) broken down from the 11-step build order below.

**Done when:**
- All of the above exists as real files in the repo (`/docs` folder is a reasonable place) *before* the first feature branch, not written retroactively to look tidy.
- The backlog has enough broken-down cards that you could open your tracker Monday morning and know exactly what to start on, with no re-thinking required.
- **Feeds the Master Project:** this literally *is* the planning phase of the Master Project — everything built in the phases below traces back to a story or diagram from this milestone.

---

## Phase 4 — Frontend Engineering

### Quick Wins
- Do the "Under the Hood" build exercises added to `Part_II_Building_Software_Detailed.md`: hand-roll `MyPromise` (+ `MyPromise.all`), and a minimal virtual-DOM diff function that reproduces the array-index-key bug on purpose.
- Build one component with `IntersectionObserver`-based lazy loading and compare its DevTools Performance trace against a `scroll`-event version.
- Convert a real `.js` file to strict-mode TypeScript, fixing every error without reaching for `any`.
- Run Lighthouse on something you've built, fix the single worst-scoring issue, re-run.

### Milestone Project — A Real React + TypeScript App
Not a todo list — pick something with real data shape complexity (a recipe manager, a habit tracker, a small inventory tool). Strict TypeScript from commit one. TanStack Query for all server state, Zustand only if you hit a genuine cross-component state need, React Router with at least one protected route, Tailwind for styling.

**Done when:**
- Core Web Vitals are measured (Lighthouse) before and after one real optimization pass.
- At least one unnecessary re-render was found via the React DevTools Profiler and fixed — with the profiler screenshot/notes to prove it, not just added `useMemo` everywhere preemptively.
- Ships somewhere reachable by a URL (Vercel/Netlify free tier is fine at this stage).

---

## Phase 5B — Backend: Java + Spring Boot (the single backend track)

*(Java is the only backend language on this roadmap — see Priority_Map.md. Study Java fundamentals together with Phase 2, and Spring Boot together with Phase 8.)*

### Quick Wins
- Do the two "Under the Hood" build exercises from `Part_II_Building_Software_Detailed.md`: inspect a synthetic bridge method with `javap -p -c`, and build a minimal transactional proxy from scratch with `java.lang.reflect.Proxy`.
- Deliberately trigger the `@Transactional` self-invocation bug, confirm it silently fails, then fix it properly.
- Convert a hand-written immutable value class into a `record` and diff the generated behavior.
- Build a tiny dependency-injection container with annotations + reflection that resolves a 3-level dependency chain — then compare it to what Spring does.
- Write a custom security filter, confirm (with a breakpoint that never gets hit) that an unauthorized request never reaches the controller.
- Add one RabbitMQ listener with a genuinely idempotent handler — deliver the same message twice on purpose, confirm no duplicate side effect.

### Milestone Project — A Real Spring Boot + JPA API
The backend for the same domain as your Phase 4 frontend (recipe manager, habit tracker, whatever you picked) — or a new one if you would rather. Auth with JWT + refresh rotation + Argon2. Full CRUD via Spring Data JPA + PostgreSQL. At least one relation where you can demonstrate an N+1 problem and then fix it with `JOIN FETCH` or `@EntityGraph`.

**Done when:**
- You have a documented before/after query count for the N+1 fix (enable Hibernate SQL logging, count the queries).
- A Flyway migration was added *after* the fact (not just the initial one) — proving you understand the "never edit an applied migration" discipline.
- Swagger/OpenAPI docs are generated and actually usable to explore the API.
- Full Testcontainers coverage, with the right test type per layer (`@WebMvcTest`, `@DataJpaTest`, a few full `@SpringBootTest`s).
- **Feeds the Master Project:** this becomes the backend's auth + core CRUD layer in Part VI — don't throw it away.

---

## Phase 6 — Mobile (React Native)

*Deferred — this entire phase is Skip For Now unless your goal shifts from fullstack web to include mobile (see `Priority_Map.md`).* No project here for now. When/if you pick it up: the milestone is a React Native + Expo companion app for whatever your Master Project has become at that point, reusing the same backend.

---

## Phase 7 — API Design

### Quick Wins
- Design the full REST spec (OpenAPI) for a small resource *before* writing any controller code — then implement against your own spec.
- Build the same simple broadcast feature two ways: WebSockets and Server-Sent Events — write down concretely when you'd pick each.
- Write one consistent error envelope (per the shape in 7.2) and use it across at least 3 different error types.

### Milestone Project — A WebSocket Feature, End to End
Add a real-time feature to your Phase 5B backend + Phase 4 frontend — live notifications, a live comment thread, a presence indicator, whatever fits your domain. Auth on connect (not via header on the initial handshake — token in the connect message, per 7.3), heartbeat, and a reconnect strategy on the frontend.

**Done when:**
- Killing the server and restarting it, the frontend reconnects on its own without a page reload.
- An unauthenticated connection attempt is rejected before it can do anything.
- **Feeds the Master Project:** this is the real-time notifications feature in Part VI.

---

## Phase 8 — Databases

### Quick Wins
- The 1M-row index exercise from 8.2: query with no index (measure), add a B-tree index (measure), add a composite/partial/covering index (measure each). Keep the `EXPLAIN ANALYZE` output for all of them side by side.
- Deliberately write an N+1 query, count the queries, fix it, count again.
- Implement cache-aside with Redis for one genuinely expensive read, including a cache-stampede guard (mutex or early expiration).

### Milestone Project — Full-Text Search + Rate Limiting
Add PostgreSQL full-text search (`tsvector`/`tsquery` + GIN index) to search across a real text field in your Master-Project-track domain, and add a Redis-backed token-bucket rate limiter (per-user) in front of at least one expensive endpoint.

**Done when:**
- You have `EXPLAIN ANALYZE` proof the GIN index is actually being used (`Bitmap Index Scan`, not a sequential scan).
- Hammering the rate-limited endpoint past its limit returns a proper `429` with a clear reset time, not a silent failure or a crash.
- **Feeds the Master Project:** this is the AI-search-adjacent full-text layer and the rate-limiting feature in Part VI (pgvector RAG search can come later as a stretch goal on top of this).

---

## Phase 9 — Data Engineering

*Awareness-level by design (see `Priority_Map.md`) — no dedicated project. If genuinely curious: instrument one real feature with analytics events (what would a data team need from this schema?) and write a one-page "data contract" describing the event shape. That's enough at this stage.*

---

## Phase 10 — System Design & Architecture

### Quick Wins
- Diagram (Excalidraw) the actual architecture of your Master-Project-track app as it exists right now — every box should be something that's really running, not aspirational.
- Write down, for your current app, what its single points of failure actually are.
- Pick one endpoint and estimate its numbers from scratch: expected QPS, bytes per request, resulting bandwidth — then sanity check against reality if you have any usage data.

### Milestone Project — Design Then Build a Rate Limiter Service
Design it first on paper (functional requirements, scale estimate, DB/cache choice, tradeoffs) — token bucket vs sliding window, why you picked one — *then* implement it as a standalone service (not just a library call) fronting a toy API, using Redis for distributed counters.

**Done when:**
- Two different app instances hitting the same limiter agree on the same limit (proving it's genuinely distributed, not per-instance).
- You can defend, out loud, why you chose token bucket or sliding window over the other.

---

## Phase 11 — Testing & Quality Engineering

### Quick Wins
- Practice TDD for one small real feature: write the failing test first, minimum code to pass, refactor, tests stay green — do this for something you'd normally just code directly, to feel the difference.
- Write the same test three ways to see the test-double spectrum: a stub, a mock (with `verify`), and a fake (in-memory repo implementation).
- Deliberately write one flaky E2E test (a missing wait), watch it fail intermittently, fix it with an explicit wait/assertion instead of a sleep.

### Milestone Project — A Real CI Pipeline That Blocks Bad Merges
GitHub Actions pipeline for your Master-Project-track repo: lint → unit tests → integration tests (Testcontainers) → coverage gate → E2E on the critical path — and it must actually **block** a merge when any stage fails, not just report red.

**Done when:**
- You can point to one real PR (even a throwaway one) where the pipeline actually stopped a broken change from merging.
- The full pipeline runs in a time you're comfortable with (know your own numbers — don't let it silently become a 20-minute pipeline nobody waits for).
- **Feeds the Master Project:** this is the CI/CD pipeline required in Part VI.

---

## Phase 12 — Performance Engineering

### Quick Wins
- Load test one real endpoint with k6, find the actual breaking point (not just "it seemed fine"), record p50/p95/p99.
- Profile your slowest page with Chrome DevTools Performance tab, find the actual bottleneck (not a guess), fix it, re-profile.
- Turn on slow-query logging on your dev database for a day of normal use and see what actually shows up — it's rarely what you'd have guessed.

### Milestone Project — Measure → Fix → Measure, Documented
Take your Master-Project-track app's three slowest real interactions (an API endpoint, a page load, a DB query). For each: baseline measurement → hypothesis → fix → re-measurement, written down like a mini post-mortem.

**Done when:**
- All three have real before/after numbers, not vibes.
- At least one fix came from a genuinely surprising root cause — if none did, you probably didn't measure first, you guessed right on the first try (rare) or didn't actually verify the fix helped.

---

## Phase 13 — DevOps & Infrastructure

*Trimmed by design for a fullstack-not-DevOps focus (see `Priority_Map.md`) — Docker and a basic CI/CD pipeline are the load-bearing pieces; Kubernetes/Terraform/deep AWS wait until a real need pulls you there.*

### Quick Wins
- Write a multi-stage Dockerfile for your backend (build stage → slim runtime stage) and compare image size against a naive single-stage build.
- Write a `docker-compose.yml` that brings up your full local stack (app + Postgres + Redis) with one command.

### Milestone Project — Ship It Somewhere Real
Deploy your Master-Project-track app to a single real environment — a single EC2 instance, or a PaaS (Railway/Render/Fly.io) is genuinely fine per the trimmed scope in `Priority_Map.md`. Full Terraform-provisioned multi-service AWS stacks and Kubernetes are explicitly not required here.

**Done when:**
- It's reachable at a real URL, not just `localhost`.
- A deploy happens via your CI pipeline (Phase 11) on merge to main, not a manual `scp`/manual click.

---

## Phase 14 — Security Fundamentals

### Quick Wins
- Complete the SQL injection and XSS learning paths on PortSwigger Web Security Academy.
- Build the full auth system properly once: Argon2, JWT + refresh rotation, TOTP MFA — this likely already exists from Phase 5B, revisit it specifically through a security lens.
- Add every header from 14.6 (`CSP`, `X-Content-Type-Options`, `HSTS`, etc.) to a real app and verify with a header-scanning tool.

### Milestone Project — Attack Your Own App
Intentionally build one vulnerable endpoint into a throwaway copy of your app (e.g., an IDOR — return another user's data without checking ownership, or a raw SQL string concatenation). Attack it yourself with OWASP ZAP or Burp Suite. Then fix it properly and confirm the same attack now fails.

**Done when:**
- You have before/after evidence (the tool's finding, then the fix, then a clean re-scan).
- You can explain the fix in terms of the actual OWASP Top 10 category it addresses, not just "I added a check."

---

## Phase 15-17 — Becoming Senior (non-code practice)

These are cheap, high-leverage, and most people skip them because they don't produce a repo. Don't skip them.

### Quick Wins
- **Debugging as a discipline (15.3):** next time you hit a real bug, write down the 5 steps (observe → hypothesize → predict → test → repeat) *as you go*, not after — most people skip writing it down and lose the discipline under pressure.
- **ADR practice (16.2):** write one real Architecture Decision Record for a choice you already made in the Master Project (why Spring Boot over a lighter framework, why Postgres over Mongo, whatever) — problem, options considered, decision, consequences.
- **Post-mortem practice (16.2):** the next time something breaks in your own project (it will), write a real post-mortem — what happened, root cause, concrete prevention step — even though no one's making you.
- **Code review practice (16.3):** find one open-source PR in a project you use and leave a genuinely useful review comment (a real question or a real correctness concern, not a nitpick) — this is free reps at a skill you can't practice alone.
- **The 12-Factor audit (17.4):** run your Master Project against all 12 factors as a literal checklist — you will find at least one violation (config in code, or logs not going to stdout, are the two most common). Fix it.

### Milestone "Project" — A Written Design Doc, Reviewed Like a Real One
Before building any non-trivial new feature in the Master Project, write a short design doc first (problem, proposed solution, at least one alternative you considered and rejected, tradeoffs) — then, if at all possible, get one other person (a friend, an online community, even a careful self-review after a day away from it) to actually poke holes in it before you write code.

**Done when:**
- The doc exists *before* the code, not as documentation written after the fact.
- At least one real objection or gap got raised and changed your approach — if nothing did, the review wasn't critical enough.

---

## The Master Project — Full Build Order

The Master Project (full spec in `Master_Senior_Fullstack_Roadmap.md` and `Part_VI_Putting_It_Together_Detailed.md`) is where every phase's milestone above actually lands — it's not a separate 11th project, it's the sum of the ones above, built in this order:

| Step | What | Which phase milestone this *is* |
|---|---|---|
| 1 | Core API — auth, JWT, basic CRUD + PostgreSQL | Phase 5B milestone |
| 2 | Frontend — React + TypeScript wired to the API | Phase 4 milestone |
| 3 | Testing from the start | Phase 11 milestone (build the CI pipeline now, not later) |
| 4 | Infrastructure — Docker Compose, then CI/CD | Phase 13 milestone |
| 5 | Advanced backend — background jobs, WebSockets, Redis caching | Phase 5B quick wins + Phase 7 milestone + Phase 8 milestone |
| 6 | Deploy somewhere real | Phase 13 milestone |
| 7 | Monitoring | Phase 12 practice, lightweight per the trimmed Phase 13 scope |
| 8 | AI feature — RAG search with pgvector | Stretch goal on top of the Phase 8 full-text-search milestone |
| 9 | Billing — Stripe integration | New, no dedicated phase milestone above — build it fresh here |
| 10 | Security pass | Phase 14 milestone, applied to the real app instead of a throwaway copy |

**Done when (the real finish line):** a stranger could clone the repo, follow a README, get it running locally in under 15 minutes, and understand from the code alone — not from you explaining it — why the significant decisions were made the way they were.

---

## Mini-Project Ladder

> Between a Quick Win (hours) and a Milestone (a week or more) sits a gap. These are **2–3 day projects**, each one small enough to finish and different enough in shape to keep things fresh (Strategy 3 in `How_To_Learn_This_Roadmap.md`). Do 1–2 per phase, in order. Each has a one-line **Done when**. They are *not* required for the next milestone — they are the practice that makes the milestone feel easy.

| # | Phase | Mini-project | Done when |
|---|---|---|---|
| L1 | 0 | **`tail`/`grep` clone in Java** — reads a file, filters lines, follows appended output | Matches `grep -n` and `tail -f` behaviour on a growing log |
| L2 | 1 | **Tiny shell** — runs commands, supports pipes and `cd` | `ls \| grep foo \| wc -l` works through your own shell |
| L3 | 2 | **LRU cache** (hash map + doubly linked list) with O(1) get/put | Hits the O(1) bound; a test with eviction order passes |
| L4 | 2 | **Autocomplete** using a trie, with top-5 by frequency | Handles 100k words; lookup time is independent of dictionary size |
| L5 | 2 | **Route finder** on a small city graph (Dijkstra) | Returns the shortest path and rejects unreachable nodes cleanly |
| L6 | 3 | **Plugin-style text transformer** (Strategy + Factory + Decorator) | Adding a new transformer needs zero edits to existing classes |
| L7 | 4 | **Vanilla-JS kanban board** (no framework, drag and drop, localStorage) | Survives refresh; no memory leak after 100 moves (DevTools heap check) |
| L8 | 4 | **Weather/GitHub dashboard** with `fetch`, loading/error states, cancellation | Typing fast never shows a stale result (race-condition safe) |
| L9 | 5B | **URL shortener API** (Spring Boot, Postgres, base62 IDs) | Redirects with 301/302, rejects bad URLs, has click counts |
| L10 | 5B | **Pastebin with expiry** (scheduled cleanup, soft delete) | Expired pastes return 410 and are physically removed by a job |
| L11 | 5B | **Webhook receiver** with signature check and replay protection | The same payload delivered twice has exactly one effect |
| L12 | 7 | **Live poll** (SSE for results, REST for votes) | 50 browser tabs all see the same count within a second |
| L13 | 7 | **Tiny chat room** (WebSocket, rooms, reconnect with backoff) | Kill the server; clients reconnect on their own and resume |
| L14 | 8 | **Search box over a 1M-row dataset** (GIN + trigram) | `EXPLAIN` shows an index scan; p95 under 50 ms |
| L15 | 8 | **Leaderboard on Redis sorted sets** | Top-100 and "my rank" are O(log n) and correct under concurrent updates |
| L16 | 10 | **Tiny key-value store with a write-ahead log** | Kill -9 mid-write; on restart the data is consistent |
| L17 | 10 | **Tiny load balancer** (round-robin + health checks) | A dead backend is removed within one health interval, then restored |
| L18 | 11 | **Property-based tests** for your LRU cache and a date parser | The tool finds at least one bug you hadn't thought of |
| L19 | 14 | **CTF-style practice**: 3 PortSwigger labs documented as write-ups | Each write-up states the root cause and the exact fix |

**Fun-track (one per ~2 weeks, no rules, Strategy 2):** pick any idea from `How_To_Learn_This_Roadmap.md` §5. Not graded, not tracked.

---

## Practice App Spec Cards (HabitHub)

> **Why a Practice App?** The Master Project (`Part_VI_Putting_It_Together_Detailed.md` §2) is a Developer Tools SaaS. That is a large target, and starting there with no prior reps means learning auth, CRUD, tests, CI and WebSockets *and* the product domain at once. **HabitHub** is deliberately a simpler domain, so every hour goes into the engineering, not into understanding the product.
>
> **How it relates to the Master Project:** HabitHub carries Phases 4, 5B, 7, 8, 10, 11, 12, 13 and 14. When you start the Master Project you **lift the reusable parts** — the auth module, CI pipeline, WebSocket gateway, rate limiter, Docker Compose, test setup — and replace the domain. That is a *good* second pass: you rebuild them faster and better, and the differences are what you learned. You are not starting from zero. (This supersedes the "feeds the Master Project" wording in the milestones above: the *code* is lifted and adapted, not used as-is.)
>
> **Core domain (keep it small):** `User` → `Habit` (name, schedule, target) → `Completion` (date, note). Later: `Team`, `Membership`, shared habits, streaks, leaderboards.

Each card is a spec, not a tutorial: you decide how.

### Card A — Frontend (Phase 4 milestone)
- **Required:** habit list/create/edit/archive; a daily check-in view; a calendar heat-map of completions; streak count; protected routes (stubbed auth first).
- **Rules:** strict TypeScript, TanStack Query for server state, Tailwind, React Router.
- **Backend for now:** a mock API (MSW or json-server) — the real one comes in Card B.
- **Stretch:** keyboard-only check-in; dark mode; offline-tolerant optimistic updates.
- **Rubric (pass = all):** Lighthouse performance and accessibility ≥ 90 · one re-render fix documented with Profiler evidence · deployed at a URL · no `any`.

### Card B — Backend (Phase 5B milestone)
- **Required:** register/login; JWT + refresh rotation + Argon2; CRUD for habits and completions scoped to the owner; streak calculation endpoint; pagination; consistent error envelope.
- **Rules:** Spring Boot 3, Java 21, Spring Data JPA, PostgreSQL, Flyway, Testcontainers.
- **Deliberate lesson:** an N+1 on "habits with their last 30 completions", documented before/after.
- **Stretch:** soft delete; per-user timezone for "what day is it"; CSV export.
- **Rubric:** IDOR test proves user A cannot read user B's habit · N+1 fixed with numbers · a Flyway migration added *after* the initial one · OpenAPI usable · test slices used correctly.
- **Then:** wire Card A to this backend and delete the mock.

### Card C — Real-time (Phase 7 milestone)
- **Required:** when a teammate completes a shared habit, other members see a live toast; presence indicator ("3 teammates online").
- **Rules:** token sent after connect (not in the URL), heartbeat, client reconnect with exponential backoff and jitter.
- **Rubric:** server kill/restart → clients reconnect unaided · unauthenticated socket rejected before any message is processed · no duplicate events after a reconnect.

### Card D — Search & limits (Phase 8 milestone)
- **Required:** full-text search over habit notes; a Redis token-bucket rate limiter on search and on the check-in endpoint; Redis cache for the streak computation with a stampede guard.
- **Rubric:** `EXPLAIN ANALYZE` shows `Bitmap Index Scan` on the GIN index · 429 with `Retry-After` · cache hit ratio measured before/after.
- **Stretch:** pgvector "find habits similar to this one" — a gentle first step toward the Master Project's RAG feature.

### Card E — Quality gate (Phase 11 milestone)
- **Required:** GitHub Actions: lint → unit → integration (Testcontainers) → coverage gate → Playwright E2E on login + check-in.
- **Rubric:** one deliberately broken PR is *blocked* · pipeline under 10 minutes (or you have measured and documented why not) · the test pyramid is pyramid-shaped.

### Card F — Ship it (Phase 13 milestone)
- **Required:** multi-stage Docker image; `docker compose up` full stack locally; deployed to a PaaS or one VM; auto-deploy on merge to `main`; a health-check endpoint and one dashboard.
- **Rubric:** reachable at a real URL · a rollback you have actually performed · zero secrets in Git history.

### Card G — Break it (Phase 14 milestone)
- **Required:** on a throwaway copy, add an IDOR and a SQL injection; exploit with ZAP/Burp; fix; re-scan.
- **Rubric:** before/after evidence for each · each mapped to its OWASP Top 10 category · a regression test added to the real app.

### Card H — Performance (Phase 12 milestone)
- **Required:** a k6 load test on check-in and on the streak endpoint; find the breaking point and the bottleneck; fix the worst one; re-test.
- **Rubric:** p50/p95/p99 before and after · the bottleneck is *named* (CPU / DB connections / lock / GC) with evidence.

> **Order of the cards:** A and B in either order (B first is easier); then C → D → E → F → G → H. After H you are ready for the Master Project.

### Rubric rule
A card is **done** only when every rubric item can be demonstrated, not claimed. Record the proof (a screenshot, a query plan, a log line) in `Progress_Tracker.md`.
