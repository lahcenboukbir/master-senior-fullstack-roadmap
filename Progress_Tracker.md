# Progress Tracker
### One line per thing you did. Honest, not pretty.

> Per `How_To_Learn_This_Roadmap.md` §3: tracking is for honest self-assessment, not a dashboard to admire. If keeping this file starts to feel like a chore, shrink it — but keep the **Check** and **Surprise** columns, they are the whole point.

**How to use:**
1. Tick the box only when the item's **Check** (in `Exercises_And_Drills.md`) or **Rubric** (in `Projects_And_Practice.md`) is demonstrably met.
2. Paste the proof next to it: a commit hash, a query plan, a number, a screenshot path.
3. Write one **Surprise** — something that behaved differently than you predicted. If you can't, you probably didn't test your prediction.

---

## Weekly Log (copy this block each week)

```
### Week of YYYY-MM-DD   (hours: __ / target 10–20)
Mix:  read __h · build __h · break __h · reflect __h
Done:
  - [ ] item → proof
Stuck on:
Surprise:
Estimate vs actual (for any mini-project finished): est __d / actual __d
Next week:
```

---

## Per-Phase Checklists

### Part 0 — Foundation Reset
- [ ] Terminal-only day
- [ ] Pipe chains on a real log
- [ ] Git history surgery (reflog recovery)
- [ ] Merge-conflict lab (3 kinds)
- [ ] CSS layouts ×3 without frameworks
- [ ] L1 `tail`/`grep` clone
- [ ] **Milestone: Raw HTTP server**

### Phase 1 — CS Fundamentals
- [ ] Number systems 20/20
- [ ] Bit tricks
- [ ] Float lab
- [ ] Cache-locality lab
- [ ] Race-condition lab
- [ ] Deadlock lab
- [ ] Wireshark capture
- [ ] Tiny scheduler
- [ ] L2 Tiny shell
- [ ] **Milestone: Cache simulator**

### Phase 2 + Java Warm-Up
- [ ] Core builds: list · linked list · stack/queue · hash map · BST · heap · graph · sorts · DP
- [ ] 3-a-week rule (weeks done: ____ / 12)
- [ ] Pattern index (≥ 8 patterns)
- [ ] Hash-collision lab · sort stress test · complexity flashcards
- [ ] Java: `equals`/`hashCode` lab · mutable-key lab · resource-leak lab · `CompletableFuture`
- [ ] L3 LRU · L4 Trie · L5 Dijkstra
- [ ] **Milestone: Task Scheduler CLI**

### Phase 3 — Design & Clean Code
- [ ] Naming pass · guard clauses
- [ ] 5 patterns standalone
- [ ] SOLID violate-then-fix
- [ ] L6 Plugin transformer
- [ ] **Milestone: Refactor Kata**

### Project Lifecycle & Planning
- [ ] Brief · 5 stories · C4 · ERD · sequence diagram · backlog
- [ ] **Milestone: plan the Master Project (or HabitHub first — your call, note which)**

### Phase 4 — Frontend (Card A)
- [ ] `MyPromise` · mini-VDOM diff
- [ ] Stale-closure lab · re-render lab · index-key lab
- [ ] TS strict conversion · a11y pass · Lighthouse loop ×2
- [ ] L7 Kanban · L8 Dashboard
- [ ] **Milestone: Card A**

### Phase 5B — Backend (Card B)
- [ ] Mini-DI · transaction proxy · `javap`
- [ ] `@Transactional` self-invocation · N+1 · LazyInit · migration discipline
- [ ] Security filter · test slices · idempotent consumer
- [ ] L9 URL shortener · L10 Pastebin · L11 Webhook receiver
- [ ] **Milestone: Card B** (+ frontend wired to it)

### Phase 7 — API Design (Card C)
- [ ] Spec-first · error envelope · pagination lab · idempotency lab · WS vs SSE
- [ ] L12 Live poll · L13 Chat room
- [ ] **Milestone: Card C**

### Phase 8 — Databases (Card D)
- [ ] SQL reps (weeks done: ____ / 6)
- [ ] Index lab · isolation lab · deadlock lab · stampede lab · backup/restore
- [ ] L14 Search box · L15 Leaderboard
- [ ] **Milestone: Card D**

### Phase 9 — Data Engineering (awareness)
- [ ] One data contract

### Phase 10 — System Design
- [ ] Back-of-envelope ×5 · Design-a-X (weeks done: ____ / 8) · failure-mode table
- [ ] L16 KV store with WAL · L17 Load balancer
- [ ] **Milestone: Rate Limiter Service**

### Phase 11 — Testing (Card E)
- [ ] TDD rep · test doubles · flaky lab · mutation check
- [ ] L18 Property tests
- [ ] **Milestone: Card E**

### Phase 12 — Performance (Card H)
- [ ] Load-test to failure · slow-query day · flame graph · leak lab
- [ ] **Milestone: Card H**

### Phase 13 — DevOps (Card F)
- [ ] Multi-stage Dockerfile · compose · kill-a-container · secrets scan
- [ ] **Milestone: Card F**

### Phase 14 — Security (Card G)
- [ ] PortSwigger ≥ 10 labs · IDOR · SQLi · headers · dependency audit
- [ ] L19 write-ups
- [ ] **Milestone: Card G**

### Phases 15–17 — Becoming Senior
- [ ] Debug log ×5 · ADRs (count: ____) · post-mortems (count: ____) · PR reviews (count: ____)
- [ ] **Milestone: a design doc reviewed before the code**

### 🏁 Master Project (DevTools SaaS)
- [ ] Phase 1 Core API · 2 Frontend · 3 Tests · 4 Infra/CI · 5 Advanced backend
- [ ] 6 Deploy · 7 Monitoring · 8 AI/RAG · 9 Billing
- [ ] Definition of Done (§2.5) — all six can be demonstrated without notes

---

## Milestone Retrospective (one per milestone)

```
Milestone: ______    Dates: ____ → ____    Estimated: __d    Actual: __d
What I built:
What I would do differently:
The hardest bug and how I found it (observe → hypothesise → predict → test):
One thing I can now explain that I couldn't before:
One thing I still can't explain:
```
