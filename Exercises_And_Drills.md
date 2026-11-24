# Exercises & Drills
### The small reps that sit underneath the milestone projects

> `Projects_And_Practice.md` tells you what to **ship** per phase. This file tells you what to **practice** along the way — the small, frequent reps that make the big projects possible. Reading gets you recognition; these get you recall and skill.
>
> **Pacing assumption:** 10–20 hours/week (see `How_To_Learn_This_Roadmap.md` §3). A drill takes minutes to a couple of hours; a Build takes 1–2 days; a Break-it lab takes 1–3 hours.

---

## The Three Tags

| Tag | What it is | Why it works |
|---|---|---|
| 🔁 **Drill** | A short rep you repeat until it's automatic (15–60 min). | Fluency. You stop looking things up. |
| 🔨 **Build** | Re-implement a mechanism from scratch (1–2 days). | You learn what the library is hiding. |
| 💥 **Break-it** | Cause a known failure on purpose, observe it, fix it, measure the fix (1–3 h). | You recognise the failure in production. |

**Rule for every item:** it has a **Check** — a concrete thing you can verify. If you can't tick the Check, you're not done. Do not move on because time passed.

**Rule for Build items:** no standard-library shortcut for the thing being built. A hash map built with `HashMap` is not a hash map.

---

## Part 0 — Foundation Reset

- 🔁 **Terminal fluency.** Do a full day of work without a file-manager GUI: navigate, search (`find`/`grep`), move, rename in bulk, `xargs`. *Check:* you did 10 tasks that would normally be clicks and never opened Explorer.
- 🔁 **Pipe chains.** From a real log file, produce: top 10 IPs, error count per hour, slowest 5 requests. *Check:* each is a single one-liner you can explain stage by stage.
- 🔁 **Git history surgery.** On a throwaway repo: amend, interactive rebase (squash, reword, reorder), cherry-pick, `reflog` a "lost" commit back. *Check:* you recovered a commit you had deleted with `reset --hard`.
- 💥 **Merge conflict lab.** Create the same conflict three ways (same line, delete-vs-modify, rename-vs-modify) and resolve each. *Check:* you can predict which kind of conflict Git will report before running the merge.
- 🔨 **Raw HTTP server** (the Part 0 milestone) — before it, hand-type a request with `nc`/`telnet` against any server and read the raw response.
- 🔁 **CSS layout from a blank file.** Rebuild three layouts (holy grail, card grid, sticky-sidebar) with zero frameworks. *Check:* each is responsive without a single media query where Grid/`auto-fit` can do it.

---

## Phase 1 — Computer Science Fundamentals

- 🔁 **Number systems.** 20 conversions decimal↔binary↔hex, plus two's complement of 5 negative numbers, by hand. *Check:* 20/20 with no calculator, under 10 minutes.
- 🔁 **Bit tricks.** Implement `isPowerOfTwo`, `countSetBits`, `swap` without a temp, and a bitmask permission set (read/write/exec). *Check:* each is 1–3 lines and you can explain it.
- 💥 **Float lab.** Show why `0.1 + 0.2 !== 0.3`, then find a real case where this would corrupt money. *Check:* you can explain the fix (integers of cents, or `BigDecimal`).
- 💥 **Cache locality.** Row-major vs column-major traversal of a 10k×10k array; time both. *Check:* a measured ratio and a one-paragraph explanation involving cache lines.
- 💥 **Race condition lab.** Two threads increment a shared counter 1M times each; observe the wrong total; fix with a lock, then with an atomic. *Check:* you can state which is faster and why.
- 💥 **Deadlock lab.** Make two threads deadlock on purpose with two locks; fix it with consistent lock ordering. *Check:* a thread dump showing the deadlock, then a clean run.
- 🔁 **Wireshark.** Capture one DNS lookup, one TCP handshake, one TLS 1.3 handshake. *Check:* you narrate every packet in order.
- 🔨 **Tiny process scheduler.** Simulate round-robin vs shortest-job-first over a list of jobs; report average wait time. *Check:* the two algorithms give different numbers on the same input and you can say why.
- 🔨 **Toy cache simulator** — the Phase 1 milestone.

---

## Phase 2 — Data Structures & Algorithms

> `Part_I_Core_Engineering_Foundations_Detailed.md` already has a **Build From Scratch** exercise per structure. Those are the Builds. Do them first, in order. This section adds the *cadence* around them.

- 🔨 **Core builds, in order:** array list → linked list → stack/queue → hash map → BST → heap → graph (BFS/DFS) → sorts → DP set. Do them in **Java** — it doubles as Java practice.
- 🔁 **The 3-a-week rule.** Three interview-style problems per week, mixed topics, rotating through: arrays/strings, hashing, two pointers, trees, graphs, DP. *Check:* for each, you wrote the Big-O before coding and you resolved it a second time after 3 days without looking.
- 🔁 **Pattern index.** Keep a one-page list: "problem looks like X → try pattern Y." *Check:* after 20 problems it has ≥ 8 patterns, each with a problem link you actually solved.
- 💥 **Hash collision lab.** Give your hash map a deliberately terrible hash function; chart lookup time vs size. *Check:* you see O(n) degrade and can explain what a good hash and resizing fix.
- 💥 **Sort stress test.** Run your quicksort on already-sorted input with a first-element pivot. *Check:* you reproduce O(n²) and fix it with a better pivot.
- 🔁 **Complexity flashcards.** For 15 operations (hash get, BST insert, heap pop, graph BFS, sort…) state time and space cold. *Check:* 15/15.
- 🔨 **Task Scheduler CLI** — the Phase 2 milestone.

---

## Java Warm-Up (do alongside Phase 2, before Phase 5B)

> The roadmap's backend is Java-only, but its Java exercises start late. These close the gap so Spring Boot isn't your first real Java.

- 🔁 **Language reps.** Classes, interfaces, generics, `record`, `enum`, `sealed`, streams, `Optional`, text blocks. *Check:* one small program per feature, no copy-paste.
- 🔁 **Collections fluency.** Implement the same word-frequency task with `HashMap`, `TreeMap`, `LinkedHashMap`; explain the iteration-order and complexity differences.
- 💥 **`equals`/`hashCode` lab.** Put an object with a broken `hashCode` into a `HashSet`; watch lookups fail. *Check:* fixed with a correct contract and a test that proves it.
- 💥 **Mutable-key lab.** Mutate a key after inserting it into a `HashMap`; the entry is "lost". *Check:* you can explain why.
- 💥 **Checked exceptions & resources.** Leak a file handle on purpose; fix it with try-with-resources. *Check:* verified with a handle count or a failing-then-passing test.
- 🔨 **Mini unit-test habit.** Every Java build above gets a JUnit 5 test class. *Check:* `mvn test` is green before each commit.
- 🔨 **Concurrency basics.** `ExecutorService` + `CompletableFuture` fetching 10 URLs in parallel with a timeout. *Check:* total time ≈ slowest request, not the sum.

---

## Phase 3 — Software Design & Clean Code

- 🔁 **Naming pass.** Rename every identifier in one of your own files so a stranger can read it without comments. *Check:* you deleted at least 3 comments that were compensating for bad names.
- 🔁 **Guard-clause refactor.** Flatten a function nested 4+ levels deep. *Check:* max nesting depth ≤ 2.
- 🔨 **Patterns, each standalone:** Observer (event emitter), Strategy (pluggable discount rules), Factory (email/SMS/push), Decorator (logging wrapper), Adapter (wrap a third-party API). *Check:* each ≤ 80 lines and has a test.
- 💥 **Violate-then-fix SOLID.** Write a deliberately bad "god service", then split it. *Check:* one change request ("add a new notification channel") touches 1 file, not 5.
- 🔁 **Code smell hunt.** Read 300 lines of any open-source project; name 5 smells and the refactoring for each.
- 🔨 **Refactor Kata** — the Phase 3 milestone.

---

## Phase 4 — Frontend Engineering

- 🔁 **DOM without a framework.** Build a filterable, sortable table with vanilla JS only. *Check:* event delegation used, no per-row listeners.
- 🔁 **Async reps.** Write the same fetch-with-retry three ways: callbacks, promises, async/await. *Check:* each handles timeout and cancellation (`AbortController`).
- 🔨 **`MyPromise` + `MyPromise.all`**, and a **mini virtual-DOM diff** (from `Part_II`'s Under the Hood sections).
- 💥 **Stale closure lab.** Reproduce the `useEffect` stale-state bug, then fix it two ways (dependency array, functional update). *Check:* a written explanation of the closure.
- 💥 **Re-render lab.** Find an unneeded re-render with the Profiler; fix it *without* sprinkling `useMemo` everywhere. *Check:* a before/after render count.
- 💥 **Array-index key lab.** Reproduce the wrong-row-state bug from using index as key.
- 🔁 **TypeScript reps.** Convert one `.js` file to strict TS with no `any`; write 3 utility types by hand (`Pick`, `Partial`, `ReturnType`-style). *Check:* `tsc --noEmit` is clean under `strict`.
- 🔁 **Accessibility pass.** Navigate your own app with the keyboard only and a screen reader for 10 minutes. *Check:* you fixed at least 3 real issues.
- 🔁 **Lighthouse loop.** Measure → fix the single worst item → re-measure. Do it twice.

---

## Phase 5B — Backend: Java + Spring Boot

- 🔨 **Mini-DI container** with annotations + reflection, resolving a 3-level dependency chain.
- 🔨 **Transaction proxy** from scratch with `java.lang.reflect.Proxy`.
- 🔁 **`javap` reading.** Disassemble a generic class; find the bridge method.
- 💥 **`@Transactional` self-invocation.** Reproduce the silent failure, then fix it properly.
- 💥 **N+1 lab.** Enable SQL logging, count queries on a list endpoint, fix with `JOIN FETCH`/`@EntityGraph`, count again. *Check:* a written "N+1 → 2" number.
- 💥 **LazyInitializationException lab.** Trigger it from a controller; fix it without turning on open-in-view.
- 💥 **Migration discipline.** Edit an already-applied Flyway migration, watch the checksum failure, then do it correctly with a new migration.
- 🔨 **Custom security filter.** Prove (breakpoint never hit) that an unauthenticated request never reaches the controller.
- 🔁 **Test-slice reps.** Write one each of `@WebMvcTest`, `@DataJpaTest`, `@SpringBootTest` for the same feature. *Check:* you can say what each starts and doesn't.
- 💥 **Idempotent consumer.** Deliver the same RabbitMQ message twice on purpose; no duplicate side effect.

---

## Phase 7 — API Design

- 🔁 **Spec first.** Write an OpenAPI spec for a new resource *before* any controller code. *Check:* a generated client compiles against it.
- 🔁 **Error envelope.** One consistent error shape across validation, not-found, conflict, and auth errors.
- 💥 **Pagination lab.** Implement offset pagination, show it break under concurrent inserts, then implement cursor pagination. *Check:* a test that demonstrates the duplicate/skipped row.
- 💥 **Idempotency-key lab.** Retry a `POST` 3× with the same key; exactly one resource is created.
- 🔁 **WebSocket vs SSE.** Same live-counter feature both ways. *Check:* a written "when I'd pick each."

---

## Phase 8 — Databases

- 🔁 **SQL reps (weekly).** 5 queries/week: joins, window functions (`ROW_NUMBER`, `LAG`), CTEs, aggregates with `HAVING`. *Check:* by week 6 you write window functions without looking.
- 💥 **1M-row index lab.** No index → B-tree → composite → partial → covering; keep every `EXPLAIN ANALYZE`. *Check:* a side-by-side table of timings and plans.
- 💥 **Isolation-level lab.** Reproduce a dirty read, a non-repeatable read, and a lost update in two `psql` sessions; show which isolation level stops each.
- 💥 **Deadlock lab.** Two transactions locking rows in opposite order; read Postgres's deadlock log.
- 💥 **Cache stampede lab.** 100 concurrent requests on a cold key → 100 DB hits; add a mutex or early expiry → 1 hit.
- 🔁 **Schema design.** Normalise a messy spreadsheet to 3NF, then deliberately denormalise one table and justify it.
- 🔁 **Backup/restore.** Take a `pg_dump`, destroy the DB, restore it. *Check:* data identical (row counts + a checksum query).

---

## Phase 9 — Data Engineering *(awareness only)*

- 🔁 **One data contract.** Define the event shape for "habit completed" (fields, types, versioning rule). *Check:* one page, with an example valid and invalid event.

---

## Phase 10 — System Design & Architecture

- 🔁 **Back-of-envelope reps.** 5 estimates: QPS, storage/year, bandwidth, cache size, number of servers. *Check:* each done in under 5 minutes with units written out.
- 🔁 **Design-a-X, weekly.** One system per week (URL shortener → rate limiter → chat → news feed → notification system). Format: requirements → estimates → API → data model → diagram → bottlenecks → trade-offs. *Check:* timed 45 minutes, then you critique your own design the next day.
- 💥 **Failure-mode table.** For your Practice App list every component, what happens when it dies, and what the user sees. *Check:* at least one failure you didn't know the answer to — then test it by killing the process.
- 🔨 **Rate Limiter Service** — the Phase 10 milestone. Add a tiny load balancer or KV store as optional toy builds.

---

## Phase 11 — Testing & Quality Engineering

- 🔁 **TDD rep.** One real feature, strictly red → green → refactor, committed at each step. *Check:* your Git log shows a failing-test commit before each passing one.
- 🔁 **Test doubles.** Same test with a stub, a mock (with `verify`), and a fake. *Check:* a paragraph on which you'd choose and why.
- 💥 **Flaky test lab.** Write a flaky E2E (missing wait); fix with an explicit wait on a condition, never a `sleep`. *Check:* 50 consecutive green runs.
- 💥 **Mutation check.** Break a line of production code on purpose. *Check:* at least one test fails; if none do, write the missing test.
- 🔁 **Coverage honesty.** Find a line that is 100% covered but not actually asserted on.

---

## Phase 12 — Performance Engineering

- 💥 **Load-test to failure.** k6 ramp until errors appear; record p50/p95/p99 and which resource saturated first.
- 💥 **Slow-query log day.** Run your app for a day with slow-query logging on; list the top 3 offenders.
- 🔁 **Flame-graph reading.** Profile one request; name the top frame and explain it.
- 💥 **Memory leak lab.** Leak objects through a static map; watch the heap grow in VisualVM/JFR; fix it.

---

## Phase 13 — DevOps & Infrastructure *(trimmed)*

- 🔨 **Multi-stage Dockerfile.** *Check:* image size reduced and a written explanation of what each stage drops.
- 🔁 **One-command stack.** `docker compose up` brings up app + Postgres + Redis, healthchecks included.
- 💥 **Kill-a-container lab.** Stop Postgres under load; observe app behaviour; add a restart policy and retry logic.
- 🔁 **Secrets hygiene.** Scan your repo history with `gitleaks`; move every secret to environment config.

---

## Phase 14 — Security Fundamentals

- 🔁 **PortSwigger Academy** paths: SQL injection, XSS, access control, auth. *Check:* ≥ 10 labs solved, with notes on the root cause of each.
- 💥 **IDOR lab** on your own app: fetch another user's habit by changing an ID; fix server-side; write the regression test.
- 💥 **SQL injection lab** on a deliberately naive endpoint; fix with parameters.
- 🔁 **Headers.** Add CSP, HSTS, `X-Content-Type-Options`, `Referrer-Policy`; scan with a header checker.
- 🔁 **Dependency audit.** Run `mvn dependency-check`/`npm audit`; fix or justify every high finding.
- 🔨 **Attack Your Own App** — the Phase 14 milestone.

---

## Phases 15–17 — Becoming Senior (written reps)

- 🔁 **Debug log.** For the next 5 real bugs, write the observe → hypothesise → predict → test log live.
- 🔁 **ADR a month.** One ADR per month, minimum.
- 🔁 **Post-mortem per incident.** Every outage, even a local one.
- 🔁 **Review a PR a week** in an open-source project you actually use. *Check:* at least one comment per review that changed something.
- 🔁 **Estimate, then compare.** Before each mini-project estimate the days; record the actual. *Check:* after 5 projects you can state your own estimation error as a ratio.

---

## How These Connect

- **Drills/Builds/Break-its** → fed into the **Mini-Project Ladder** and **Milestone spec cards** in `Projects_And_Practice.md`.
- Log every item in `Progress_Tracker.md` (one line each: what you did, the Check result, what surprised you).
