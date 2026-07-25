# The Complete Senior Fullstack Engineer Roadmap
### From Coder to Expert — Every Fundamental, Every Tool, Every Layer of the Stack
#### Core Stack: Java · Spring Boot · React · TypeScript · React Native · PostgreSQL · AWS — plus the full ecosystem around it

> **No deadlines. No shortcuts. No ceiling.**
> The goal is not to "finish" this document. The goal is to become an engineer who understands *why* everything works the way it does — from the CPU instruction cycle to the distributed system your API lives in — and who can pick up any tool in this document and use it competently within days because the fundamentals underneath it are already solid.
>
> This roadmap merges every phase, every practical tool, and every piece of ecosystem awareness you need. Deep-dive sections tell you exactly what to learn, what to build, and what questions you should be able to answer. Awareness sections exist so that when you hit a problem outside your core stack, you already know which tool solves it instead of reinventing it.

---

## How This Document Is Organized

- **Deep-dive topics** — you are expected to master these. They form your daily stack: Java 21, Spring Boot 3, React 18+/TypeScript, React Native, PostgreSQL, Redis, Docker, AWS, Terraform.
- **Landscape / awareness topics** — you don't need production mastery, but you must know what they are, when to reach for them, and roughly how they work. These appear as "Ecosystem Awareness" call-out blocks throughout.
- **Every phase ends with**: tools & practice, hands-on projects, and questions you should be able to answer cold.

---

## Table of Contents

**Part 0 — Setup**
0. [Foundation Reset](#phase-0--foundation-reset)

**Part I — Core Engineering Foundations**
1. [Computer Science Fundamentals](#phase-1--computer-science-fundamentals)
2. [Data Structures & Algorithms](#phase-2--data-structures--algorithms)
3. [Software Design & Clean Code](#phase-3--software-design--clean-code)

**Part II — Building Software**
4. [Frontend Engineering](#phase-4--frontend-engineering)
5. [Backend Engineering — Java + Spring Boot](#phase-5--backend-engineering-java--spring-boot)
6. [Mobile Engineering — React Native](#phase-6--mobile-engineering-react-native)
7. [API Design — REST, GraphQL, gRPC, Realtime](#phase-7--api-design)

**Part III — Data**
8. [Databases](#phase-8--databases)
9. [Data Engineering (Awareness)](#phase-9--data-engineering-awareness)

**Part IV — Systems at Scale**
10. [System Design & Architecture](#phase-10--system-design--architecture)
11. [Testing & Quality Engineering](#phase-11--testing--quality-engineering)
12. [Performance Engineering](#phase-12--performance-engineering)
13. [DevOps & Infrastructure](#phase-13--devops--infrastructure)
14. [Security Fundamentals](#phase-14--security-fundamentals)

**Part V — Becoming Senior**
15. [The Senior Mindset & Problem Solving](#phase-15--the-senior-mindset--problem-solving)
16. [Communication & Collaboration](#phase-16--communication--collaboration)
17. [Engineering Practices & Methodologies](#phase-17--engineering-practices--methodologies)

**Part VI — Putting It Together**
18. [The Complete Tech Stack Reference](#the-complete-tech-stack-reference)
19. [The Master Project](#the-master-project)
20. [Suggested Progression](#suggested-progression-no-deadlines-just-order)

---

## Phase 0 — Foundation Reset

> Fix the environment and habits before touching any new topic. This costs little time and changes everything downstream.

### 0.1 — Linux as Your Daily Driver
**Learn:** Ubuntu install and setup as primary OS; filesystem hierarchy (`/etc`, `/var`, `/usr`, `/home`, `/tmp`, `/proc`, `/sys`); users/groups/permissions (`chmod`, `chown`, `sudo`, `umask`); shells (`bash`, `zsh`), environment variables, `PATH`; process management (`ps`, `top`, `htop`, `kill`, `killall`, `systemctl`); package management (`apt`, `snap`).
**Practice:** Use the terminal for everything — no GUI file manager, no GUI git client. Write a shell script that provisions a fresh Ubuntu box from scratch.
**Answer:** What's the difference between a process and a thread in Linux? What does `chmod 755` mean in binary? How do you make a script run on startup?

### 0.2 — Terminal Mastery
**Learn:** `grep` (patterns, `-r`, `-i`), `awk` (field processing), `sed` (stream editing, in-place replace), `find` (by name/type/size/date), `curl` (headers, POST JSON), `ssh` (`~/.ssh/config`, port forwarding, bastion hosts), `tmux` (persistent sessions, panes), `htop`, piping/redirection (`|`, `>`, `>>`, `2>&1`, `/dev/null`).
**Practice:** Parse a log file with `grep`+`awk`+`sed` to extract error counts per endpoint. Write a `curl` command that POSTs JSON with auth headers.

### 0.3 — Git Internals & Discipline
**Learn:** What Git stores (blobs, trees, commits, refs — not diffs); the staging area/index; `merge` vs `rebase` — what happens to the commit graph; `git reset --soft/--mixed/--hard` (HEAD, index, working tree); `git reflog`; `git bisect`; `git stash`; interactive rebase (`git rebase -i HEAD~N` — squash, reorder, edit); conventional commits (`feat:`, `fix:`, `refactor:`, `chore:`, `docs:`, `test:`); commit messages that explain *why* not *what*; branch strategies (Git Flow vs Trunk-based vs GitHub Flow).
**Practice:** Make messy commits, clean them up with interactive rebase. Intentionally create and resolve a merge conflict.
**Answer:** Merge vs rebase — when do you use each? What does `git reset --hard HEAD~1` do and how do you undo it? What is a detached HEAD?

### 0.4 — HTML Semantics
**Learn:** Why semantic HTML matters (a11y, SEO, screen readers, browser defaults); block vs inline; `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`; why `<button>` beats `<div onClick>` (keyboard access, ARIA roles, focus management); forms (`<input>` types, `<label for>`, `required`, `pattern`, `<fieldset>`, `<legend>`); tables (`<thead>`, `<tbody>`, `<th scope>`); images (`alt`, `loading="lazy"`, `srcset`); meta tags (viewport, charset, Open Graph); how the browser parses HTML into the DOM.
**Practice:** Rebuild a div-soup page with proper semantics. Test it with a screen reader (NVDA/VoiceOver).

### 0.5 — CSS Fundamentals (Before Any Framework)
**Learn:** Box model (`content → padding → border → margin`, `box-sizing: border-box`); specificity calculation (inline > IDs > classes > elements); the cascade; inheritance (what inherits, what doesn't); stacking context and `z-index`; positioning (`static`, `relative`, `absolute`, `fixed`, `sticky`); Flexbox (`flex-direction`, `justify-content`, `align-items`, `flex-grow/shrink/basis`); CSS Grid (`grid-template-areas`, `minmax()`, `auto-fit` vs `auto-fill`); responsive design (mobile-first, media queries, `clamp()`); custom properties (`:root`, `var()`); animations (`transition`, `@keyframes`, only animate `transform`/`opacity`).
**Practice:** Build a responsive layout from a mockup with zero frameworks. Debug a z-index bug by understanding stacking contexts.
**Answer:** What creates a new stacking context? Why does `margin: auto` center horizontally but not vertically? `flex-grow` vs `flex-basis`?

### 0.6 — How HTTP Really Works
**Learn:** HTTP as text (request line, headers, empty line, body); method semantics (GET safe+idempotent, POST, PUT idempotent, PATCH, DELETE); status code families (1xx–5xx); headers (`Content-Type`, `Authorization`, `Cache-Control`, `ETag`, `Location`, `Set-Cookie`); keep-alive; HTTP/1.1 vs HTTP/2 (multiplexing, HPACK, server push); HTTP/3 & QUIC (UDP-based, solves head-of-line blocking); HTTPS/TLS on top of HTTP; cookies (`HttpOnly`, `Secure`, `SameSite`); caching (`Cache-Control`, `ETag`/`If-None-Match`, `Last-Modified`).
**Practice:** `curl -v` to inspect raw headers. Capture a real HTTP exchange with Wireshark.
**Answer:** What happens with a GET + body? `401` vs `403`? What does `Cache-Control: no-cache` actually mean (hint: it doesn't mean "don't cache")?

### Habits to Build Now
- Read error messages fully — the answer is almost always in the stack trace.
- Measure before you optimize — never guess where the bottleneck is.
- Write before you code — pseudocode, design notes, diagrams first.
- Run `EXPLAIN ANALYZE` on every SQL query you write — make it a reflex.
- Keep a learning journal — two lines per session is enough.

---

# Part I — Core Engineering Foundations

## Phase 1 — Computer Science Fundamentals

> Most coders use computers without understanding them. This is the mental model that separates engineers from developers.

### 1.1 — How a CPU Works
Fetch-decode-execute cycle; registers; ALU; clock speed vs instruction count (why GHz alone doesn't predict performance); binary representation (two's complement for negatives, IEEE 754 for floats); hexadecimal; instruction sets (x86-64, ARM) at awareness level.
**Practice:** Convert decimal ↔ binary/hex by hand until automatic. Read `javap -c` output for a simple Java method.

### 1.2 — Memory Hierarchy
Registers → L1 → L2 → L3 → RAM → SSD/HDD, with real latency numbers (registers ~1 cycle, RAM ~100 cycles, SSD ~100,000 cycles); cache lines and why sequential access crushes random access; temporal vs spatial locality.
**Practice:** Iterate a 2D array row-first vs column-first in Java and measure the real-world difference.

### 1.3 — Operating Systems
- **Processes vs Threads** — isolated memory space vs shared memory space; why threads are cheaper to create; thread-local storage vs shared memory; why spawning 10,000 threads kills performance (context-switch overhead).
- **Stack vs Heap** — call frames, local variables, return addresses (LIFO, fixed size) vs dynamically allocated objects (GC-managed); `StackOverflowError` from infinite recursion; what lives where in Java (primitives/references on stack, objects/arrays on heap).
- **Virtual Memory & Paging** — every process gets its own address space; pages (~4KB); page tables; page faults; why virtual memory enables isolation; memory-mapped files (`mmap`) — how DBs and the JVM use it.
- **Scheduling** — preemptive vs cooperative; how the OS decides what runs next; CPU-bound vs I/O-bound tasks behaving differently under load; context-switch cost.
- **Concurrency Primitives** — race conditions, critical sections; mutex vs semaphore vs RWLock; deadlock (detection, prevention, lock ordering, banker's algorithm concept); livelock; starvation; the Java Memory Model — why `volatile`/`synchronized` exist.
- **File Systems** — how files are stored, inodes, why `fsync` matters for DB durability, inode limits.
- **IPC** — pipes, message queues, shared memory, Unix domain sockets, `mmap`.
- **Signals** — `SIGKILL` vs `SIGTERM`, signal handling, signal masks, signal-safe operations, graceful shutdown.
- **Garbage Collection** — reference counting (can't handle cycles); mark-and-sweep; generational GC (young/old gen); stop-the-world pauses; Java GC algorithms — Serial, Parallel, **G1GC** (default since Java 9), **ZGC** (ultra-low pause, Java 21+); JVM heap tuning (`-Xms`, `-Xmx`, `-XX:+UseG1GC`); memory leaks (static collections that grow forever, unclosed streams, listener accumulation).
**Practice:** Write a Java program spawning threads that modify a shared variable — observe the race condition. Use VisualVM/JMC to watch GC behavior under pressure.

### 1.4 — Networking Deep Dive
OSI layers 3–7; IPv4/IPv6, subnets, CIDR; **TCP** — three-way handshake (why not two?), reliability/ordering, congestion control (slow start, AIMD), Nagle's algorithm vs `TCP_NODELAY`, teardown (FIN/FIN-ACK/ACK), `TIME_WAIT`; **UDP** — connectionless, when it beats TCP (DNS, gaming, VoIP), datagram boundaries; **DNS** — recursive vs authoritative resolution, TTL, propagation delay; **TLS 1.3** handshake, certificate chain validation, mTLS; **CORS** — same-origin policy, preflight OPTIONS, allowed origins; **HTTP/2 vs HTTP/3** — multiplexing, QUIC (built on UDP), 0-RTT, head-of-line blocking; **non-blocking I/O** — `select` vs `poll` vs `epoll` vs `io_uring`, how servers handle thousands of connections; **zero-copy** (`sendfile`) — why copying between kernel/user space is expensive; CDN edge caching mechanics.
**Practice:** Wireshark on a real TLS handshake and DNS query. Use `dig`/`nslookup` to trace resolution step by step.

### 1.5 — How the Browser Works
URL → DNS → TCP → TLS → HTTP request → response; rendering pipeline: HTML parse → DOM, CSS parse → CSSOM, DOM+CSSOM → render tree → layout (reflow) → paint → composite; render-blocking resources (`<script>` in `<head>`, `@import`); reflow (expensive) vs repaint (cheaper); critical rendering path; why JS parsing blocks the parser (`defer`/`async`); cookies/sessions/localStorage in a stateless protocol; browser/CDN/server cache layers and how they interact.

### 1.6 — Compilers & Interpreters (Basics)
Lexing/tokenization; parsing and AST; compilation vs interpretation vs JIT; **JVM internals** — bytecode (not machine code), class loader delegation model (bootstrap/extension/application), bytecode interpretation, **JIT compilation** (hot method detection → native code), HotSpot optimizations (inlining, escape analysis, loop unrolling); `javap -c` to disassemble; JVM flags (`-XX:+PrintCompilation`).
**Why this matters:** helps you write faster code and understand cryptic error messages.

### Ecosystem Awareness — CS Fundamentals Extras
- **Escape analysis** — how compilers decide stack vs heap allocation.
- **Boolean logic & discrete math basics** — truth tables, De Morgan's laws — underpin query planners and conditionals.

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| Wireshark | Capture and inspect real network packets |
| curl + netcat | Raw HTTP/TCP requests, byte-by-byte protocol understanding |
| Linux (Ubuntu) | Daily-drive it |
| Node.js (quick tour) | Explore the event loop, hit race conditions on purpose |
| nand2tetris.org | Optional but transformative — build a computer from scratch |

> **PROJECT:** Build an HTTP server from scratch using raw TCP sockets (Java or Go) — no frameworks. You will understand HTTP at a level most web developers never reach.

---

## Phase 2 — Data Structures & Algorithms

> You don't need 500 LeetCode problems. You need to think algorithmically — recognize patterns, reason about tradeoffs, choose the right tool. **Implement everything in Java** — understanding comes from building, not reading.

### 2.1 — Complexity Analysis (Start Here)
Big O (time & space); best/average/worst case; analyzing loops (single = O(n), nested = O(n²), halving = O(log n)); amortized analysis (why `ArrayList.add()` is O(1) amortized despite O(n) resizing); common complexities with real examples: O(1) HashMap lookup, O(log n) binary search, O(n) linear scan, O(n log n) merge sort, O(n²) nested loops, O(2ⁿ) naive recursive Fibonacci.
**Answer:** When does HashMap lookup degrade from O(1)? Why is O(n log n) the comparison-sort lower bound?

### 2.2 — Arrays & Strings
Static vs dynamic arrays, contiguous memory, cache locality; resizing strategy (double when full) → amortized O(1) append; string immutability in Java (`s = s + "x"` in a loop is O(n²)) → `StringBuilder`; techniques: two pointers, sliding window, prefix sums.
**Build:** A dynamic array from scratch (`get`, `set`, `add`, `remove`, auto-resize).

### 2.3 — Linked Lists
Singly vs doubly linked; when arrays beat linked lists (cache locality) and vice versa (frequent mid-list insert/delete); fast/slow pointers (Floyd's cycle detection); reversing in place; finding the middle in one pass.
**Build:** Singly linked list (`addFirst`, `addLast`, `removeFirst`, `get`, `contains`, `reverse`) + cycle detection.

### 2.4 — Stacks & Queues
LIFO vs FIFO; array vs linked-list implementation; `ArrayDeque` in Java for both — never use the legacy `Stack` class; real uses: call stack, undo/redo, BFS, task queues; monotonic stack (next greater/smaller element).
**Build:** Stack on a dynamic array; queue using two stacks.

### 2.5 — Hash Maps & Hash Sets
`hashCode()` → compress to index → bucket; collision handling — separate chaining vs open addressing (linear/quadratic probing); load factor and resize threshold (~0.75); why O(1) average but O(n) worst case; Java 8+ treeification when a bucket exceeds 8 elements; `HashMap` vs `LinkedHashMap` (insertion order) vs `TreeMap` (sorted, red-black tree); `HashSet` vs `TreeSet` vs `LinkedHashSet`; `ConcurrentHashMap` (segment locking in Java 7, CAS in Java 8+).
**Build:** HashMap with separate chaining (`put`, `get`, `remove`, `resize`).

### 2.6 — Trees
Binary tree; BST property (left < node < right); traversals — inorder (sorted for BST), preorder (copy), postorder (delete), BFS/level-order (queue-based); height/depth/diameter; self-balancing trees (AVL, Red-Black) — why they matter; B-trees — why databases and filesystems use them (optimized for disk reads, not memory).
**Build:** BST with `insert`/`search`/`delete` + all four traversals iteratively and recursively; validate-BST check.

### 2.7 — Heaps
Binary heap as an array; max-heap vs min-heap; `siftUp`/`siftDown` (O(log n)); build-heap from array is O(n), not O(n log n); why heapsort isn't used in practice despite O(n log n) (cache misses vs quicksort); priority queue use cases (Dijkstra, scheduling, K-th largest); Java `PriorityQueue` (min-heap by default).
**Build:** Min-heap from scratch (`insert`, `extractMin`, `peek`, `heapify`).

### 2.8 — Graphs
Directed vs undirected, weighted vs unweighted; adjacency list (sparse) vs adjacency matrix (dense, O(1) edge lookup); BFS (shortest path unweighted) vs DFS (cycle detection, topological sort); topological sort (DAGs only); cycle detection (white/gray/black states); connected components / union-find.
**Build:** Graph via adjacency list — `addEdge`, BFS, DFS, cycle detection, topological sort.

### 2.9 — Tries
Prefix tree — each edge a character; `TrieNode{children, isEndOfWord}`; insert/search/prefix-search all O(m); use cases — autocomplete, spell checkers, IP routing.
**Build:** Trie — `insert`, `search`, `startsWith`, `getAllWordsWithPrefix`.

### 2.10 — Union-Find (Disjoint Set Union)
Path compression + union by rank → near-O(1) amortized; use cases — connected components, Kruskal's MST, cycle detection in undirected graphs.

### 2.11 — Sorting
Bubble/Selection/Insertion (O(n²), know why they're slow, insertion sort good for nearly-sorted data); Merge Sort (O(n log n) always, stable, extra O(n) space, external sorting); Quick Sort (O(n log n) average / O(n²) worst, in-place, partition strategy matters); why O(n log n) is the comparison-sort lower bound (information theory); non-comparison sorts (counting sort O(n+k), radix sort); Java's `Arrays.sort()` — dual-pivot quicksort for primitives, TimSort for objects.
**Build:** Merge sort and quicksort from memory, no references.

### 2.12 — Searching & Shortest Paths
Linear search O(n); binary search O(log n) — the template (`left`, `right`, `mid = left + (right-left)/2` to avoid overflow), off-by-one traps, binary search on the answer space (not just arrays); BFS shortest path (unweighted); Dijkstra (weighted, fails on negative edges → Bellman-Ford instead); A* (heuristic-guided search).

### 2.13 — Dynamic Programming
Recognizing optimal substructure + overlapping subproblems (the hardest and most important skill); memoization (top-down) vs tabulation (bottom-up); 1D DP (Fibonacci, climbing stairs, house robber); 2D DP (LCS, edit distance, 0/1 knapsack); deriving the recurrence relation.
**Build:** Fibonacci with/without memoization (compare performance); coin change; longest increasing subsequence; 0/1 knapsack.

### Ecosystem Awareness — Advanced DS&A
- **Segment Tree / Fenwick Tree (BIT)** — range queries + point updates, sparse tables — used in competitive programming and some production analytics.
- **String matching** — KMP, Rabin-Karp, suffix arrays — what regex engines use under the hood.
- **Computational geometry** — convex hull, line sweep, closest pair — relevant for game dev, GIS, graphics.

### Problem-Solving Patterns
| Pattern | Use When | Classic Example |
|---|---|---|
| Sliding Window | Subarray/substring problems | Max sum of k-size window |
| Two Pointers | Sorted arrays, palindromes | Remove duplicates in-place |
| Fast & Slow Pointers | Cycle detection | Linked list cycle |
| Binary Search | Sorted/monotonic space | Search in rotated array |
| BFS/DFS | Tree & graph traversal | Level order traversal |
| Backtracking | All possibilities | Permutations, N-Queens, Sudoku |
| Dynamic Programming | Overlapping subproblems | Coin change, knapsack |
| Divide & Conquer | Independent subproblems | Merge sort, closest pair |
| Monotonic Stack | Next greater/smaller element | Daily temperatures |
| Union-Find | Connected components | Number of islands |
| Topological Sort | Dependency ordering | Course schedule |

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| LeetCode | 50–100 problems focused on patterns, not quantity |
| Neetcode.io | Best structured roadmap — follow it in order |
| VisuAlgo | Watch algorithms run visually |
| Java | Implement every structure from scratch — no built-in collections |

> **PROJECT:** Implement linked list, stack, queue, hash map, BST, and graph from scratch. Implement BFS, DFS, merge sort, and quicksort without references.

---

## Phase 3 — Software Design & Clean Code

> The true cost of software isn't writing it — it's maintaining it. Clean design controls that cost over the product's lifetime.

### 3.1 — Clean Code Principles
Meaningful naming (`validateAndEnrichUserOrder()` not `processData()`); functions do one thing (if you can't describe it in a sentence, split it); short functions (<20 lines is a good heuristic); no magic numbers (named constants); avoid deep nesting (early returns/guard clauses); comments explain WHY not WHAT; DRY, KISS, YAGNI.
**Practice:** Refactor a real function from your own codebase applying all of these.

### 3.2 — SOLID Principles (with concrete bad/good examples)
- **S**ingle Responsibility — one reason to change. Bad: `UserService` that validates + saves + emails + reports. Good: separate `UserValidator`, `UserRepository`, `EmailService`, `ReportGenerator`.
- **O**pen/Closed — open for extension, closed for modification. Bad: giant `if/else`/`switch` you must edit for new behavior. Good: strategy pattern/polymorphism.
- **L**iskov Substitution — subclasses substitutable without breaking behavior. Classic violation: `Square extends Rectangle` breaking `setWidth`/`setHeight`.
- **I**nterface Segregation — don't force fat interfaces on implementers that only need part of them.
- **D**ependency Inversion — high-level modules depend on abstractions, not concrete implementations (inject `UserRepository` interface, not `PostgresUserRepository`).
> Don't memorize definitions — learn to **spot violations in real code**.

### 3.3 — Design Patterns

**Creational**
- **Singleton** — one instance globally; thread-safe via `synchronized`, double-checked locking, or `enum` singleton; why it's often abused (global state in disguise).
- **Factory** — create objects via a method instead of `new`, when the type depends on runtime info.
- **Builder** — step-by-step construction when a constructor needs 3+ parameters.

**Structural**
- **Adapter** — wrap an incompatible/third-party interface to match yours.
- **Decorator** — add behavior without subclassing (e.g. `LoggingUserRepository` wraps `UserRepository`).
- **Proxy** — control access (Spring AOP is built on this); lazy-loading proxy, security proxy, caching proxy.
- **Facade** — simplify a complex subsystem behind one clean interface.

**Behavioral**
- **Observer / Pub-Sub** — decoupled event notification (message queues, React state).
- **Strategy** — interchangeable algorithms at runtime (payment processors, tax rules).
- **Command** — encapsulate actions as objects (undo/redo, task queues).
- **Repository** — abstract data access; domain layer never touches SQL.
- **Circuit Breaker** — CLOSED → OPEN (after failures) → HALF_OPEN (probe) → CLOSED; fail fast on a downstream outage (resilience4j).

**Distributed System Patterns**
- **Saga** — manage distributed transactions with compensating actions (Order → Payment → Shipping; if Shipping fails, compensate Payment and cancel Order).
- **Outbox Pattern** — write to DB + an outbox table in one transaction; a worker relays events reliably — solves the dual-write problem.
- **Event Sourcing** — store events, not state; derive state by replaying; enables audit trails and temporal queries.
- **CQRS** — separate read and write models, often paired with Event Sourcing.

**Anti-Patterns to Recognize:** god object, singleton abuse, premature abstraction, cargo cult programming.

### 3.4 — Architecture Patterns
- **Layered Architecture** — Presentation → Business Logic → Data Access; problem: logic leaks into controllers/repositories.
- **Clean Architecture** — dependencies point inward; core knows nothing about frameworks or DBs.
- **Domain-Driven Design (DDD) basics** — ubiquitous language; entities (identity, e.g. `User` with `userId`); value objects (no identity, e.g. `Money`, `Address`); aggregates (root entity controls access); domain events (`OrderPlaced`); bounded contexts. *(Full DDD deep dive — strategic + tactical design, event storming — is in [Phase 17](#173--domain-driven-design-ddd--deep-dive).)*
- **Hexagonal Architecture (Ports & Adapters)** — ports = interfaces, adapters = implementations (`UserRepository` port, `JpaUserRepository` adapter).
- **MVC** — separation of concerns; where business logic actually lives (rarely the "M"); when MVC breaks down (SPAs, micro-frontends).

**Spring Boot Clean Architecture layout:**
```
src/main/java/com/company/app/
├── domain/          ← Entities, domain services, repository interfaces
├── application/     ← Use cases, application services, DTOs
├── infrastructure/  ← Repository implementations, external integrations
└── presentation/    ← Controllers, request/response models
```

### 3.5 — Refactoring
What refactoring is (structure changes, behavior unchanged); code smells — long method, large class, feature envy, data clumps, primitive obsession, switch statements (often a polymorphism smell), god object; techniques — extract method/class, rename, move method/field, replace conditional with polymorphism; the Boy Scout Rule; refactor safely — tests first, then change, tests stay green.

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| Your existing codebase | Refactor applying Clean Code + SOLID — the best teacher |
| Refactoring.guru | Free visual guide to every pattern |
| SonarQube / SpotBugs / ESLint | Automate code quality in your pipeline |
| IntelliJ inspections | Enable all warnings, fix them |

> **PROJECT:** Implement 5 patterns as real mini-projects — Observer (event emitter from scratch), Repository (DB abstraction), Strategy (multi-provider payment system), Factory (notification system: email/SMS/push), Circuit Breaker (wrap an unstable API with resilience4j).

---

# Part II — Building Software

## Phase 4 — Frontend Engineering

> HTML/CSS are the foundation, JavaScript is the language, TypeScript is the discipline, React is the framework. Learn in this order.

### 4.1 — JavaScript: The Core Language
Single-threaded execution; execution context and `this`; scope (`var` function-scoped/hoisted vs `let`/`const` block-scoped + temporal dead zone); **closures** (functions capturing outer scope — callbacks, module pattern, memoization); **prototypal inheritance** (`[[Prototype]]` chain); `this` binding (`call`, `apply`, `bind`, arrow functions' lexical `this`); type coercion (`==` vs `===`, truthy/falsy); error handling (`try/catch/finally`); memory leaks (detached DOM nodes, listener accumulation).

**ES6+ to master:** destructuring, spread/rest, template literals, optional chaining (`?.`), nullish coalescing (`??`), modules (`import`/`export`, dynamic `import()`), generators/iterators, `WeakMap`/`WeakSet`.

### 4.2 — Async JavaScript
The event loop (call stack + task queue + microtask queue); macrotasks (`setTimeout`, I/O) vs microtasks (`Promise.then`, `queueMicrotask` — always run before the next macrotask); callbacks and callback hell; Promises (states, chaining, `.then/.catch/.finally`); `async/await` as sugar over Promises; concurrent patterns — `Promise.all` (fail-fast), `Promise.allSettled` (all results), `Promise.race` (first settled), `Promise.any` (first fulfilled).
**Practice:** Fetch from an API with error handling and loading state, in vanilla JS, no framework.

### 4.3 — TypeScript (Strict Mode From Day One)
Type inference; interfaces vs type aliases (interfaces extendable, prefer for objects); generics (`function identity<T>(value: T): T`); discriminated unions (the most powerful TS pattern — `type Shape = Circle | Rectangle` narrowed via a `kind` field); `unknown` vs `any` (always prefer `unknown`); utility types (`Partial`, `Required`, `Pick`, `Omit`, `ReturnType`, `Record`); declaration files (`.d.ts`); `strict: true` in `tsconfig.json` — never disable it.
**Practice:** Convert a JS file to TS strict mode; fix every error.

### 4.4 — Browser & DOM APIs
DOM tree structure; manipulation (`querySelector`, `createElement`, `appendChild`); event bubbling vs capturing, `stopPropagation`/`preventDefault`; event delegation (one listener beats a thousand); `IntersectionObserver` (lazy loading), `ResizeObserver`, `MutationObserver`; `fetch` (abort controllers, streaming responses); **Web Workers** (off-main-thread computation); **Service Workers** (intercept requests, offline, push, background sync); `LocalStorage` vs `SessionStorage` vs `IndexedDB`.

### 4.5 — React — How It Actually Works

**Core:** JSX compiles to `React.createElement`; components are pure functions; props (immutable, one-directional); reconciliation (diffing old vs new virtual DOM); **Fiber** (React's interruptible unit of work); keys and why wrong keys cause bugs; render phase vs commit phase.

**Hooks — know each one deeply:**
- `useState` — batched updates (React 18 batches everywhere, even `setTimeout`/`Promise.then`).
- `useEffect` — sync with external systems only, not for derived state or data fetching in modern React.
- `useRef` — mutable value, no re-render, DOM node access.
- `useContext` — avoid prop drilling; causes re-renders in all consumers on change.
- `useReducer` — complex state logic, an alternative to Redux for local state.
- `useMemo` / `useCallback` — memoize value/function reference; avoid overusing — adds complexity for little gain most of the time.
- `useLayoutEffect` — synchronous, runs after DOM mutation before paint — for measuring.
- Custom hooks — `useFetch`, `useLocalStorage`, `useDebounce`.

**React 18+ features:** automatic batching; `startTransition` (mark updates non-urgent); concurrent rendering; `Suspense` (declarative loading); **Server Components** (render on server, zero JS shipped); hydration and hydration errors.

**State management:**
- Local state (`useState`/`useReducer`) — default choice.
- Context API — good for infrequent updates (theme, locale, user); causes re-renders in all consumers.
- **Zustand** — lightweight, no boilerplate, selector-based to avoid unnecessary re-renders.
- **TanStack Query** — server state: caching, stale-while-revalidate, background refetch, optimistic updates.
- Redux Toolkit — `createSlice`, `createAsyncThunk`, RTK Query — the modern Redux, know it because many codebases still use it.

**Performance:** re-render causes (state, context, parent re-render); `React.memo`; bundle splitting (`React.lazy` + `Suspense`); React DevTools Profiler.

**Routing:** React Router v6 (nested routes, loaders, actions, protected routes, URL state as query params).

### 4.6 — Next.js — The React Framework
App Router vs Pages Router; SSR vs SSG vs ISR — when to use each; Server Components vs Client Components mental model; Edge runtime; middleware (intercept requests before pages); automatic image optimization.

### 4.7 — Styling
- **Tailwind CSS** — utility-first; JIT compiler (only shipped classes); responsive prefixes (`sm:`, `md:`, `lg:`); dark mode (`dark:`); state variants (`hover:`, `focus:`); config customization; `@apply` for repeating groups.
- **CSS Grid deep** — `grid-template-areas`, `minmax()`, `auto-fit` vs `auto-fill`, subgrid.
- **Flexbox deep** — `flex-grow/shrink/basis`, `align-items` vs `align-content`, `gap`.

### 4.8 — Build Tools & Dev Tooling
Vite (ESM dev server, Rollup prod build, HMR); how a bundler works (entry → dependency graph → output); tree shaking; code splitting (route/component/vendor); source maps; ESLint + Prettier; `tsc` strict mode flags; Husky + lint-staged (pre-commit checks).

### 4.9 — Frontend Performance
Core Web Vitals — LCP (<2.5s), INP (<200ms), CLS (<0.1); lazy loading (`loading="lazy"`, `React.lazy()`); code splitting (route/component/vendor chunking); tree shaking; image optimization (WebP/AVIF, `srcset`, CDN processing); bundle analysis (Webpack Bundle Analyzer, `vite-bundle-visualizer`); debounce/throttle; `React.memo`/`useMemo`/`useCallback` used judiciously, only after profiling shows a real problem; compression (Brotli > Gzip); resource hints (`preload`, `prefetch`, `preconnect`, `dns-prefetch`).

### Ecosystem Awareness — Frontend Landscape
- **Other frameworks:** Vue (Composition API, Proxy-based reactivity, Pinia), Svelte (compile-time, no virtual DOM, runes in Svelte 5), Angular (DI, RxJS, change detection, standalone components), SolidJS (fine-grained reactivity via signals, no virtual DOM).
- **State management alternatives:** Jotai/Recoil (atomic state).
- **CSS-in-JS:** Styled-components, Emotion — runtime cost, SSR hydration issues; CSS Modules (scoped, zero runtime); Sass/SCSS (variables, mixins, nesting); PostCSS (autoprefixer, future CSS today).
- **Alternative bundlers:** Webpack (loaders/plugins, more config, older default), esbuild (Go-based, very fast, used by Vite), Turbopack (Rust-based, Next.js default), Parcel (zero-config), SWC (Rust-based, Babel replacement, used by Next.js/Deno).
- **WebAssembly (WASM):** near-native browser performance for compute-heavy tasks; language-agnostic (Rust/C/Go → WASM); use cases: image/video processing, in-browser IDEs, plugin systems.
- **WebGL/WebGPU:** GPU-accelerated graphics, shaders, Three.js.
- **WebRTC:** P2P connections, STUN/TURN servers, data channels, signaling.
- **Design systems & a11y tooling:** Storybook (component isolation + visual testing), Figma-for-engineers (design tokens, Dev Mode), `axe-core` (automated a11y testing), Lighthouse CI.

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| Vite | Fast build tool — learn it deeply |
| React DevTools | Inspect component tree, spot unnecessary re-renders |
| Chrome DevTools | Performance, Network, Memory tabs |
| Storybook | Develop components in isolation |
| Lighthouse | Audit performance, accessibility, SEO |
| ESLint + Prettier | Enforce code quality automatically |

> **PROJECT:** Build a full React + TypeScript app: TanStack Query for data, Zustand for global state, React Router for navigation, Tailwind for styling, Playwright for E2E. Measure and optimize bundle size and Core Web Vitals.

---

## Phase 5 — Backend Engineering (Java + Spring Boot)

> Java forces you to understand OOP, types, and memory more deeply than most languages. Spring Boot is the enterprise/fintech industry standard.

### 5.1 — Java Fundamentals (Deep)

**OOP in Java:** encapsulation; inheritance (`extends`, overriding, `super`, why deep hierarchies are dangerous); polymorphism (`Animal` reference holding a `Dog`); abstraction (abstract classes vs interfaces); `final` (class/method/variable meaning); `static`.

**Type System:** generics (`List<T>`, `<T extends Comparable<T>>`); bounded wildcards — upper (`? extends Animal`, read-only) and lower (`? super Dog`, write-only); type erasure (why `List<String>` == `List<Integer>` at runtime); covariance (why `List<Dog>` is NOT a `List<Animal>`, but arrays are broken-covariant).

**Collections (Internals):** `ArrayList` vs `LinkedList` (ArrayList usually wins — cache locality); `HashMap` internals (bucket array, linked list or red-black tree per bucket in Java 8+); `TreeMap` (red-black tree, sorted, O(log n)); `ArrayDeque` for stack/queue; `ConcurrentHashMap` (CAS-based in Java 8+).

**Modern Java (8–21):** Streams API (`filter`/`map`/`reduce`/`collect`, lazy evaluation); `Optional` (correct use, avoid `Optional` fields); lambdas & functional interfaces (`Predicate`, `Function`, `Supplier`, `Consumer`); **Records** (immutable data carriers); **sealed classes** (controlled inheritance, exhaustive switch); pattern matching (`instanceof` with binding, switch expressions with `yield`); text blocks; **virtual threads (Java 21)** — massive I/O-bound concurrency without OS thread cost.

**Concurrency:** `synchronized`, `volatile`, `AtomicInteger`/`AtomicReference` (CAS); `ExecutorService`/`ThreadPoolExecutor` (core/max pool size, queue, rejection policy); `CompletableFuture` (composable async); `ReentrantLock`/`ReadWriteLock`; `CountDownLatch`/`Semaphore`/`CyclicBarrier`; structured concurrency (Java 21 — group related tasks, cancel on failure).

**Error Handling:** checked vs unchecked exceptions (the Java debate); custom exception hierarchies; try-with-resources.

### 5.2 — Spring Boot 3: Core Concepts

**DI & IoC:** Inversion of Control — the container creates/wires objects; bean scopes (singleton, prototype, request, session); **constructor injection always** (explicit dependencies, immutability, testability) — avoid field injection; auto-configuration (classpath scanning wires DataSource, Redis, etc. automatically); stereotypes — `@Component`, `@Service`, `@Repository`, `@Controller`.

**Spring Web (REST):** `@RestController`; `@GetMapping`/`@PostMapping`/`@PutMapping`/`@DeleteMapping`/`@PatchMapping`; `@PathVariable`, `@RequestParam`, `@RequestBody`; validation (`@Valid` + `@NotNull`/`@Size`/`@Email`); exception handling (`@ControllerAdvice` + `@ExceptionHandler`, `ProblemDetail`/RFC 9457); DTOs — never expose JPA entities directly; **MapStruct** for zero-boilerplate mapping; pagination (`Pageable`, `Page<T>`); API versioning (URL `/v1/` vs header versioning).

**Spring Data JPA:** `JpaRepository<T, ID>`; derived query methods (`findByEmailAndStatus`); `@Query` (JPQL/native); transactions (`@Transactional`, propagation, isolation); **N+1 problem** — the most common Spring perf bug — fix with `JOIN FETCH` or `@EntityGraph`; projections (interface/DTO — select only needed columns); **Flyway** migrations — never edit a deployed migration file.

**Spring Security:** authentication vs authorization; security filter chain; JWT — parse/validate in a filter, set `SecurityContext`; `@PreAuthorize("hasRole('ADMIN')")` method security; CORS via `CorsConfigurationSource` (never `@CrossOrigin` in prod); password encoding — `BCryptPasswordEncoder`/`Argon2PasswordEncoder`, never MD5/SHA-1; OAuth2 Resource Server — validate JWTs from an identity provider.

**Production features:** profiles (`application-dev.yml`/`application-prod.yml`, `SPRING_PROFILES_ACTIVE`); **Actuator** (`/actuator/health`, `/metrics`, `/info`); **Micrometer** (Prometheus/Datadog/CloudWatch integration); structured JSON logging with MDC correlation IDs; graceful shutdown (`server.shutdown=graceful` — drain in-flight requests).

**Async & Background Jobs:** `@Async` (+ `@EnableAsync`); `@Scheduled` cron jobs; **RabbitMQ + Spring AMQP** (`@RabbitListener`, `AmqpTemplate.convertAndSend()`); dead letter queues; **idempotency** — every handler must be safe to run twice.

**Testing Spring Boot:** `@SpringBootTest` (full context, expensive); `@WebMvcTest` (web layer only, `MockMvc`); `@DataJpaTest` (JPA layer, use Testcontainers for a real DB); `@MockBean`; `TestRestTemplate`; **Testcontainers** — real PostgreSQL/Redis/RabbitMQ in Docker during tests; JUnit 5 (`@Test`, `@ParameterizedTest`, `@BeforeEach`, `@Nested`); Mockito (`@Mock`, `@InjectMocks`, `when/thenReturn`, `ArgumentCaptor`, `verify`).

### 5.3 — Backend Production Concerns
- **Webhooks** — signature verification (HMAC-SHA256), retry with exponential backoff, idempotency, respond 200 fast + process async, dead-letter handling. Real examples: Stripe, GitHub, Twilio.
- **Payments** — PCI-DSS basics (never store raw card data, tokenize); Stripe PaymentIntents, idempotency keys, webhook handling; subscription billing (trials, proration, cancellation); chargebacks and refunds.
- **Full-text search** — PostgreSQL `tsvector`/`tsquery` + GIN index + `ts_rank` first; Elasticsearch when you outgrow it.
- **Multi-tenancy** — row-level security (RLS) vs schema-per-tenant vs database-per-tenant vs shared-nothing; always filter by `tenant_id`.
- **Rate limiting** — token bucket in Redis, per-user and per-IP.
- **File storage** — S3/GCS, presigned URLs, multipart uploads, content-type validation.
- **Email** — SMTP, transactional vs marketing, deliverability (SPF/DKIM/DMARC).
- **Notifications** — push (FCM/APNs), in-app, email, SMS; batching, deduplication, preferences.

### Ecosystem Awareness — Backend Landscape
- **Other backend languages worth knowing about:** Node.js/TypeScript (event loop, huge ecosystem), Python (Django/FastAPI, data/AI-friendly), Go (goroutines/channels, simple deployment, great for infra tooling), Rust (memory safety without GC, performance-critical services).
- **Job schedulers/queues in other ecosystems:** Bull/BullMQ, Sidekiq, Celery, Hangfire.
- **Search platforms:** Elasticsearch/OpenSearch (inverted index, analyzers, mappings, aggregations), Algolia (managed, typo tolerance), Meilisearch (easy self-hosted alternative).

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| IntelliJ IDEA | The best Java IDE — learn every shortcut |
| Maven / Gradle | Build tools — understand dependency management |
| Postman / Swagger UI | Test and document APIs |
| Testcontainers | Real PostgreSQL/Redis in Docker during tests |
| Spring Initializr | start.spring.io — bootstrap correctly |
| resilience4j | Circuit breaker, retry, rate limiter |

> **PROJECT:** Complete REST API with Spring Boot — user management, JWT + refresh tokens, CRUD with Spring Data JPA + PostgreSQL, Flyway migrations, async jobs with RabbitMQ, Stripe integration with webhooks, full-text search, multi-tenant RLS, full Testcontainers coverage, Swagger/OpenAPI docs.

---

## Phase 6 — Mobile Engineering (React Native)

> You already know React. React Native is React for mobile — same paradigm, different rendering target. The transition is intentionally smooth.

### 6.1 — Why React Native After React Web
Shared mental model (components, hooks, state, props); shared tooling (TypeScript, ESLint, TanStack Query, Zustand); shared backend (one Spring Boot API for web + mobile); one codebase for iOS + Android.

### 6.2 — Fundamentals
How RN works — JS thread + native thread, bridge (old architecture) vs **JSI** (new architecture); native components (`View` not `div`, `Text` not `p`, `TouchableOpacity`, `ScrollView`, `FlatList`, `Image`); `StyleSheet.create()` — CSS-like, Flexbox by default with **column** as the default direction (not row, unlike web); `Platform.OS`, `.ios.tsx`/`.android.tsx` file extensions; debugging with Flipper/React Native Debugger/Chrome DevTools; **Hermes** engine (RN's optimized JS engine).

### 6.3 — Expo — The Right Way to Start
Expo vs bare RN (start managed, eject only for unsupported native modules); **Expo Router** — file-based routing, same mental model as Next.js App Router; Expo SDK (camera, location, notifications, filesystem, secure store — without native code); **EAS Build** (cloud builds, no Mac needed) + **EAS Submit** (automated store submission); OTA updates (push JS updates without app store review).

### 6.4 — Navigation
Expo Router file-based routing (`app/index.tsx`, `app/(tabs)/home.tsx`); deep linking (universal links iOS, app links Android); URL schemes; navigation state and params.

### 6.5 — Styling for Mobile
**NativeWind** (Tailwind classes on RN); `StyleSheet.create` for perf; safe area handling (`useSafeAreaInsets` for notch/home indicator); platform design guidelines (iOS HIG, Android Material Design).

### 6.6 — Mobile Performance
`FlatList` vs `ScrollView` — always `FlatList` for lists (virtualization); `InteractionManager.runAfterInteractions` — defer heavy work until animations finish; `useCallback`/`React.memo` matter *more* here than on web (slower CPUs); `expo-image` for caching; avoid inline styles/anonymous functions in render (new references every render).

### 6.7 — Device APIs & Storage
Camera (`expo-camera`); location (`expo-location`, foreground vs background permissions); push notifications (Expo Notifications + FCM + APNs); biometrics (`expo-local-authentication`); **MMKV vs AsyncStorage** — MMKV is dramatically faster; **`expo-secure-store`** for tokens/credentials — never AsyncStorage for sensitive data.

### 6.8 — Networking & Offline
Same TanStack Query as web; `persistQueryClient` + AsyncStorage for offline cache; optimistic updates; `@react-native-community/netinfo` for network detection; certificate pinning against MITM; background sync (`expo-task-manager` + `expo-background-fetch`).

### 6.9 — App Store Deployment
App signing (iOS certs/provisioning profiles, Android keystore); EAS Build managed certificates; TestFlight (iOS beta); Google Play Console (internal/closed/production tracks); review guideline awareness; semantic versioning (`version`/`buildNumber`/`versionCode`); Fastlane for screenshots/metadata automation.

### Ecosystem Awareness — Mobile Landscape
- **Native:** iOS (Swift, SwiftUI vs UIKit, Combine, Core Data), Android (Kotlin, Jetpack Compose, coroutines, Room, WorkManager) — legacy Objective-C/Java maintenance awareness.
- **Cross-platform alternatives:** Flutter (Dart, widget tree, BLoC/Riverpod), Kotlin Multiplatform (shared business logic, `expect`/`actual`).

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| Expo Go | Run on a real device instantly during dev |
| EAS CLI | Build and submit from the terminal |
| Flipper | Debug network, layout, performance |
| Expo Snack | Share runnable RN snippets |

> **PROJECT:** Mobile companion app for your Master Project — same backend, RN + Expo frontend. Auth with secure token storage, offline support via TanStack Query, push notifications, TestFlight + Play internal testing deploy.

---

## Phase 7 — API Design

> An API is a product with users, versions, and documentation. Design it like you're building for other developers — because you are.

### 7.1 — REST Principles (Deep)
Resources not actions (`GET /orders/123` not `POST /getOrder`); method semantics — GET (safe+idempotent), POST (not idempotent), PUT (idempotent, replace), PATCH (partial), DELETE (idempotent); status codes used correctly (200/201/204, 400/401/403/404/409/422/429, 500/503); statelessness; uniform interface and consistent naming (plural, lowercase, hyphenated — `/user-profiles`).

### 7.2 — REST Best Practices
Versioning (URL `/api/v1/` — explicit, cacheable); pagination (cursor-based for large datasets — `WHERE id > :lastSeenId ORDER BY id LIMIT 20`, OFFSET only for small ones); filtering/sorting via query params; partial responses (`?fields=`); consistent error envelope:
```json
{ "error": { "code": "VALIDATION_ERROR", "message": "Email is invalid", "field": "email", "requestId": "abc-123" } }
```
Never leak internal details in errors; `requestId`/correlation ID on every response; HATEOAS at awareness level; idempotency keys for safe POST retries.

### 7.3 — WebSockets
When to use (bidirectional realtime: chat, live notifications, collaborative editing) vs when not (SSE for one-directional, polling for infrequent); handshake (`Upgrade: websocket`); lifecycle (open → messages → ping/pong heartbeat → close); auth (token in query string on connect, or first message — never in a header on the initial handshake); scaling — sticky sessions or Redis pub/sub fan-out across nodes; Spring WebSocket + STOMP (`@MessageMapping`, `SimpMessagingTemplate`).

### 7.4 — API Security
Auth on every non-public endpoint; rate limiting (per-user + per-IP, distributed in Redis); input validation at the boundary; output filtering (never return unauthorized fields); API keys (entropy, rotation, scope restriction, never in frontend code); CORS whitelisting.

### 7.5 — API Documentation
OpenAPI 3.0 as the standard; API-first design — spec before implementation, spec is the contract; **Springdoc OpenAPI** auto-generates Swagger UI from annotations (`@Operation`, `@ApiResponse`, `@Schema`); version the spec alongside the code.

### Ecosystem Awareness — API Paradigms Beyond REST
- **GraphQL** — schema/resolvers/mutations/subscriptions; the **N+1 problem** and **DataLoader** as the fix; introspection (disable in prod); complexity limiting; persisted queries; when it beats REST (many clients, differing data needs) vs when REST wins (simple CRUD, strong HTTP caching).
- **gRPC** — Protocol Buffers, HTTP/2, streaming (unary/server/client/bidirectional), code generation; best for internal service-to-service communication.
- **Server-Sent Events (SSE)** — unidirectional server→client, `EventSource` auto-reconnect, `Last-Event-ID` resume; simpler than WebSockets when you only need server push (live feeds, log streaming).
- **tRPC** — end-to-end typesafe APIs without schema generation, Zod validation, tight React integration.
- **JSON-RPC / SOAP** — simple RPC and legacy XML-based enterprise APIs — know they exist, you'll meet them in older systems.
- **AsyncAPI** — like OpenAPI but for event-driven systems (Kafka topics, WebSocket events, AMQP queues).
- **API contract testing** — Pact — consumer-driven contracts, breaking-change detection between services.

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| Swagger UI / Springdoc | Auto-generate docs from Spring Boot annotations |
| Postman / Insomnia | Test APIs, write automated collections |
| httpie | Cleaner CLI HTTP client than curl |
| Apollo Studio | GraphQL schema management (if you explore GraphQL) |

> **PROJECT:** Design and document the complete API spec for your Master Project *before* writing any controller code. Implement REST; optionally implement a GraphQL version of the same domain and add DataLoader to compare tradeoffs.

---

# Part III — Data

## Phase 8 — Databases

> Most production outages trace back to the database. Internals separate engineers who debug in minutes from those who debug for days.

### 8.1 — How Databases Actually Work
Storage on disk — pages/blocks, row format; query lifecycle — SQL text → parser → planner/optimizer (cost-based) → executor; **WAL (Write-Ahead Log)** — changes logged before applied, enables crash recovery + replication; **buffer pool** — RAM cache of hot pages; **MVCC** — readers never block writers, each transaction sees a snapshot; `VACUUM` — reclaiming dead rows from updates/deletes, autovacuum tuning; row-oriented vs column-oriented storage — when each wins.

### 8.2 — Indexing (The Most Important Topic)
What an index is — separate structure, speeds reads at the cost of writes/storage; **B-Tree** (default, O(log n) equality + range); **Hash** (O(1) exact match, no ranges); **GIN/GiST** (full-text search, JSONB, arrays, geometric types in PostgreSQL); **composite indexes** — column order matters (`(a,b)` helps `WHERE a=?` and `WHERE a=? AND b=?` but not `WHERE b=?`); **covering indexes** — index-only scans; **partial indexes** (`WHERE status='active'`); index selectivity (why indexing a boolean is nearly useless); when indexes hurt (write-heavy tables, over-indexing); reading `EXPLAIN ANALYZE` — `Seq Scan` vs `Index Scan` vs `Bitmap Heap Scan` — make this a reflex on every query you write.
**Practice:** 1M-row table. Query with no index (measure). Add B-tree index (measure). Add composite/partial/covering indexes (measure each). This single exercise teaches more than any book.

### 8.3 — ACID & Transactions
Atomicity, Consistency, Isolation, Durability (WAL ensures durability).

**Isolation levels and what they prevent:**
| Level | Dirty Reads | Non-Repeatable Reads | Phantom Reads |
|---|---|---|---|
| Read Uncommitted | Possible | Possible | Possible |
| Read Committed (Postgres default) | Prevented | Possible | Possible |
| Repeatable Read | Prevented | Prevented | Possible |
| Serializable | Prevented | Prevented | Prevented |

**Concurrency & locking:** optimistic (check on write) vs pessimistic (`SELECT FOR UPDATE`) locking; row-level vs table-level locks; deadlocks (detection, prevention, PostgreSQL kills one automatically); advisory locks (application-level, distributed-lock use cases).

### 8.4 — Query Optimization
Why `SELECT *` is harmful (over-fetch, breaks index-only scans, breaks on schema change); **N+1 problem**; JOIN types (`INNER`/`LEFT`/`RIGHT`/`FULL OUTER`/`CROSS`) and when each applies; **OFFSET vs cursor pagination** — OFFSET scans all preceding rows, cursor jumps directly; CTEs (`WITH ...`) for readability; subqueries vs JOINs — check `EXPLAIN`; materialized views for expensive aggregations; batch operations vs row-by-row.

### 8.5 — PostgreSQL in Depth
Full-text search — `tsvector`/`tsquery`, GIN index, `ts_rank`; **pgvector** — vector similarity search for AI features (`<->` distance operator, `ivfflat`/`hnsw` index); **row-level security (RLS)** — enforce tenant isolation at the DB level; `LISTEN`/`NOTIFY` — lightweight pub/sub; JSONB — `->`/`->>` operators, when to use vs when to normalize; connection pooling with **PgBouncer** (transaction pooling mode).

### 8.6 — Redis Beyond Caching
Data types — string (counters via `INCR`), list (queues/feeds), set (unique collections), **sorted set** (leaderboards, rate limiting by score, priority queues), hash (sessions/objects), stream (append-only log); cache patterns — cache-aside (most common), write-through, write-behind; invalidation — TTL, event-driven, versioned keys; **cache stampede** — mutex lock or probabilistic early expiration; pub/sub for real-time messaging; Lua scripts for atomic multi-command ops; persistence tradeoffs (RDB snapshot vs AOF append-log); Redis Cluster for horizontal scaling.

### 8.7 — Scaling Databases
Read replicas (replication lag is real); sharding (by user ID, time, geography — each has tradeoffs); connection pooling sizing (`(cores × 2) + effective spindle count`); **CAP theorem** in practice — during a partition you choose CP (refuse to stay consistent, most RDBMS) or AP (serve possibly-stale data, some NoSQL); **PACELC** — even without a partition, latency vs consistency is still a tradeoff.

### Ecosystem Awareness — The Broader Database Landscape
| Database | Type | Best For |
|---|---|---|
| PostgreSQL | Relational | Financial data, complex queries, your default choice |
| MongoDB | Document | Flexible/nested schemas |
| Redis | Key-Value | Caching, sessions, rate limiting, leaderboards |
| Cassandra | Wide-column | Massive write-heavy, time-series, eventual consistency |
| Elasticsearch/OpenSearch | Search engine | Full-text search, log analysis, inverted index |
| Neo4j | Graph | Social graphs, recommendation engines, 6+ join queries |
| InfluxDB / TimescaleDB | Time-series | Downsampling, retention policies, high cardinality |
| CockroachDB / YugabyteDB | NewSQL | Distributed SQL, serializable-by-default, geo-replication |
| DuckDB | Analytical (embedded) | Local analytical queries, no server needed |

- **Normalization** (1NF–3NF) and when to deliberately denormalize for read performance.
- **Vector databases** for AI/RAG beyond pgvector: Pinecone (managed, hybrid search), Weaviate (GraphQL + BM25 hybrid), Qdrant (Rust, on-prem friendly), Chroma (lightweight/local), Milvus (billion-scale, GPU-accelerated). **Start with pgvector** if you're already on Postgres — migrate only when you hit its limits.

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| PostgreSQL | Primary database — learn it to the internals level |
| Redis | Caching, sessions, pub/sub, rate limiting, sorted sets |
| DBeaver / pgAdmin | Visual query and schema management |
| EXPLAIN ANALYZE | Run on every query — non-negotiable habit |
| PgBouncer | Connection pooling — measure the difference |
| k6 / pgbench | Load test queries with real volume |

---

## Phase 9 — Data Engineering (Awareness)

> You don't need to be a data engineer. You need to understand the landscape well enough to design systems that feed data pipelines correctly and know when a problem is a data problem vs an application problem.

### 9.1 — ETL vs ELT
ETL — transform before loading (good when transformation is complex or data must be masked before leaving source systems). ELT — load raw, transform in the warehouse via SQL (modern, enabled by cheap columnar storage — BigQuery/Snowflake).

### 9.2 — Batch & Stream Processing
**Batch:** Apache Spark (DataFrames, RDDs, Spark SQL), Hadoop MapReduce (legacy), Databricks (managed Spark + Delta Lake); partition strategy affects job performance dramatically.
**Streaming:** Apache Kafka (topics, partitions, consumer groups, retention — the backbone of most data pipelines); Apache Flink (stateful, exactly-once, event-time vs processing-time); Spark Streaming (micro-batch); windowing — tumbling, sliding, session.

### 9.3 — Orchestration & Quality
Apache Airflow (DAG-based, Python-defined, "cron on steroids"); Prefect/Dagster (modern alternatives); backfills; sensors; idempotent tasks. Data quality — Great Expectations, dbt tests, schema validation, anomaly detection.

### 9.4 — Storage & Modeling
Data lakes — S3 + Parquet/ORC, Delta Lake/Iceberg/Hudi (ACID on object storage), schema evolution, time travel.
Data warehouses — Snowflake, BigQuery, Redshift, DuckDB; columnar storage (10-100x faster for aggregations); star schema (fact + dimension tables); slowly changing dimensions (SCD Type 1/2/3).

### 9.5 — Product Analytics
Mixpanel/Amplitude (event tracking, funnels, cohort retention); Segment (event routing, schema enforcement); event tracking design — instrument your app for analytics from day one; **data contracts** — the agreement between your app's event schema and what the data team consumes.

> **What this means for you:** when building features, think about what events your app should emit, what the data team will need from your schema, and how schema changes ripple downstream. This awareness makes you a better collaborator and system designer.

---

# Part IV — Systems at Scale

## Phase 10 — System Design & Architecture

> Real systems don't fail because of bad code. They fail because of bad design decisions made under pressure without understanding tradeoffs.

### 10.1 — The Foundation Questions
What problem are we actually solving? What scale (users, RPS, data volume)? What consistency requirements? What can we sacrifice — availability, consistency, latency, cost? What happens when each component fails?
> **Know your numbers:** 1M QPS ≈ 1KB/request ≈ 1 Gbps bandwidth.

### 10.2 — Scalability & Availability
Vertical (bigger machine, ceiling) vs horizontal (more machines, needs statelessness) scaling; auto-scaling on metrics; **availability numbers** — 99.9% = 8.7 h/year downtime, 99.99% = 52 min/year, 99.999% = 5 min/year; single points of failure — find and eliminate every one; redundancy (active-active vs active-passive); graceful degradation; liveness vs readiness health checks.

### 10.3 — Consistency Models
Strong consistency (every read reflects the latest write) vs eventual consistency (stale-but-converging reads); choose per use case — financial data needs strong, a social feed tolerates eventual.

### 10.4 — Load Balancing
Round robin, least connections, IP hash, **consistent hashing** (adding/removing a node remaps only 1/N of keys — critical for distributed caches/DBs); Layer 4 vs Layer 7; Nginx/HAProxy as reverse proxy and load balancer.

### 10.5 — Caching Architecture
What to cache (expensive computations, read-heavy/rarely-changed data, external API responses) vs what NOT to (user-specific real-time data, security-sensitive data); layers — browser → CDN → app cache (Redis) → DB buffer pool; cache-aside, write-through, write-behind; cache warming (avoid cold-start latency spikes); eviction policies (LRU, LFU, TTL); **cache stampede** protection (mutex or probabilistic early expiration).

### 10.6 — Asynchronous Communication
Message queues vs direct HTTP — decoupling, consumer can be down while messages wait; producer/consumer pattern; **at-least-once** (idempotent consumers required) vs **exactly-once** (extremely hard, approximate with idempotency keys); backpressure; **dead letter queues**; event-driven architecture (event sourcing vs event notification — know the difference); Kafka topics/partitions/consumer groups for event streaming at scale.

### 10.7 — Microservices vs Monolith
| Architecture | When to Use |
|---|---|
| Monolith | Start here — simple to develop/deploy/debug |
| Modular Monolith | Best of both — structured modules, one deployable |
| Microservices | When the pain of staying together exceeds the pain of splitting |

> Most teams adopt microservices too early. Start with a modular monolith. Split when a module has independent scaling needs, deployment cadence, or team ownership.
Service discovery; **API Gateway** (single entry — auth, routing, rate limiting); service mesh awareness (Istio, Linkerd); data ownership — each microservice owns its own data, no shared databases.

### 10.8 — Distributed Systems Fundamentals
The Fallacies of Distributed Computing (the network is not reliable/instant/infinite); **CAP theorem** — pick two during a partition; **PACELC** extension; consensus algorithms at awareness level (Raft, Paxos, ZAB — how etcd/Consul/ZooKeeper agree); **2PC** (coordinator failure can leave participants locked) vs **Saga** (compensating transactions) vs **TCC** — know when each is viable; idempotency and idempotency keys; distributed locking with Redis (`SET key value NX EX 30`); circuit breaker; retry with exponential backoff + jitter; **bulkhead pattern** (isolate workloads into separate thread pools).

### 10.9 — Common Systems to Design
For each: functional requirements, scale estimate, key components, DB choice, tradeoffs.

| System | Core Challenges |
|---|---|
| URL Shortener | Hashing, redirect performance, analytics, caching |
| Rate Limiter | Token bucket vs sliding window, distributed counters in Redis |
| Real-time Chat | WebSockets, fan-out, delivery guarantees, history |
| Notification System | Multi-channel, fan-out at scale, deduplication |
| Authentication Service | JWT, refresh tokens, revocation, session at scale |
| Payment System | Idempotency, consistency, fraud detection, webhook reliability |
| Search System | Inverted index, ranking, full-text vs vector search |
| File Upload Service | Presigned URLs, chunked upload, CDN distribution |
| Video Streaming | Object storage, CDN, adaptive bitrate, encoding pipeline |
| Distributed Cache | Consistent hashing, eviction policies |

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| Excalidraw / draw.io | Design system architectures visually — diagrams before code |
| Nginx | Load balancer, reverse proxy, rate limiting |
| Redis | Caching, pub/sub, distributed rate limiting |
| RabbitMQ / Kafka | Message queues and event streaming hands-on |
| AWS free tier | Deploy real distributed systems |

> **PROJECT:** Design and build 3 systems — URL Shortener (Postgres + Redis + rate limiting), Real-time Notification System (WebSockets + Redis pub/sub + RabbitMQ + persistence), Rate Limiter service (Redis token bucket, distributed, per-user).

---

## Phase 11 — Testing & Quality Engineering

> If you're afraid to change your code, your tests are failing you — even if they all pass.

### 11.1 — The Testing Pyramid
```
        /\
       /E2E\          ← Few, slow, expensive — critical paths only
      /------\
     /Integration\    ← Some, medium speed — component contracts
    /------------\
   /  Unit Tests  \   ← Many, fast, cheap — the foundation
  /----------------\
```
Invert this and you get a slow, brittle, expensive suite.

### 11.2 — Test-Driven Development
RED (failing test) → GREEN (minimum code to pass) → REFACTOR (clean up, tests stay green). TDD is about **design feedback** — test-first code is naturally more decoupled and modular.

### 11.3 — Unit Testing (Deep)
Arrange-Act-Assert; test doubles — **dummy** (unused placeholder), **stub** (hardcoded response), **fake** (working but unfit for prod, e.g. in-memory repo), **mock** (verifies interactions), **spy** (wraps real, records calls); Mockito (`@Mock`, `@InjectMocks`, `when/thenReturn/thenThrow`, `ArgumentCaptor`, `verify`); JUnit 5 (`@Test`, `@ParameterizedTest`, `@Nested`); naming (`should_returnUser_when_validIdProvided`).
**Anti-patterns:** testing implementation not behavior; tests depending on each other; over-mocking (testing the mock, not the code).

### 11.4 — Integration & E2E Testing
Integration — verify components work together; **Testcontainers** — real PostgreSQL/Redis/RabbitMQ in Docker during tests; `@SpringBootTest` (full context) vs `@DataJpaTest` (JPA layer). E2E — Playwright (`page.goto`, `page.click`, `expect(locator).toBeVisible()`); Page Object Model; test only critical journeys (registration, login, checkout); flaky tests are the #1 enemy — fix with explicit waits and full test isolation.

### 11.5 — Performance/Load Testing
Load (expected traffic), stress (beyond capacity — where does it break?), spike (sudden surge), soak (hours at moderate load — catches memory leaks); **k6** — JS-based virtual users and scenarios; metrics — throughput, p50/p95/p99 latency (never just average), error rate; fix, then measure again.

### 11.6 — Beyond the Basics
- **Contract testing (Pact)** — consumer-driven contracts between services, catch breaking changes before deploy.
- **Property-based testing** — define properties that must always hold ("sort then reverse == reverse then sort"); the framework generates hundreds of random inputs to find edge cases you'd never think of. Tools: **fast-check** (JS), **Hypothesis** (Python), **jqwik** (Java).
- **Mutation testing** — mutate your code automatically, verify your tests actually catch the mutation (not just hit the line). Tools: **Stryker** (JS/TS), **PIT** (Java).
- **Visual regression testing** — screenshot comparison against baselines to catch unintended CSS/component changes. Tools: **Chromatic**, **Percy**, Playwright screenshot comparison.
- **Chaos engineering** — intentionally inject failures (kill instances, add latency, drop packets) to validate resilience. Define steady state → hypothesize it holds under chaos → inject → observe. Requires mature observability first — start in staging. Tools: **Chaos Monkey**, **Gremlin**. "Game days" — scheduled chaos exercises with the whole team present.

### 11.7 — CI/CD Testing Pipeline
```
Every push triggers automatically:
1. Lint                      ← < 30 seconds
2. Unit tests                ← < 1 minute
3. Integration tests         ← < 5 minutes
4. Security dependency scan
5. Coverage threshold check  ← block merge if below threshold
6. E2E tests on critical paths ← blocking on main branch
```

### Testing Anti-Patterns (recap)
Testing implementation not behavior; order-dependent tests; over-mocking; flaky tests destroying trust; chasing coverage numbers; suites so slow nobody runs them locally.

### Tools & Practice
| Tool | Language | What to Do |
|------|----------|-----------|
| JUnit 5 | Java | Unit and integration testing |
| Mockito | Java | Mocking — Spring Boot standard |
| Testcontainers | Java | Real PostgreSQL/Redis in Docker |
| Vitest | TypeScript | Unit testing for frontend |
| Playwright | Any | E2E browser testing |
| k6 | Any | Load and performance testing |
| Pact | Any | Contract testing between services |
| fast-check / jqwik | JS / Java | Property-based testing |
| Stryker / PIT | JS/TS / Java | Mutation testing |
| Chromatic / Percy | Any | Visual regression testing |
| Gremlin / Chaos Monkey | Any | Chaos engineering (staging first) |

> **PROJECT:** Practice TDD for one full feature from scratch. Build a GitHub Actions pipeline that blocks merges on failing tests. Load test your API with k6 and find exactly where it breaks. Write 5 property-based tests for a data transformation function.

---

## Phase 12 — Performance Engineering

> Performance is a design property, not an afterthought. Measure first — never optimize what you haven't measured.

### The Golden Rule
```
Measure → Identify bottleneck → Fix → Measure again
Never optimize without data. Never guess.
```
Profiling (finds *where* time is spent) vs benchmarking (measures *how fast*); flame graphs (wide = slow); JMH for accurate Java microbenchmarks (avoids JVM warm-up bias).

### 12.1 — Database Performance
`EXPLAIN ANALYZE` — read every node (`cost`, `actual rows`, `actual time`); N+1 detection (enable Hibernate SQL logging in dev, count queries per request); connection pool sizing (`(cores × 2) + effective spindle count`); **HikariCP** tuning (`maximumPoolSize`, `connectionTimeout`, `leakDetectionThreshold`); slow query log (`log_min_duration_statement`); prepared statements (SQL-injection-safe + plan caching); cursor-based pagination over OFFSET; materialized views for expensive aggregations.

### 12.2 — Java / Spring Boot Performance
JVM tuning (`-Xmx`, `-Xms`); **G1GC vs ZGC** (throughput vs latency tradeoff); avoid object allocation in hot paths; `CompletableFuture` for parallel I/O; **virtual threads (Java 21)** for I/O-bound endpoints; Spring Boot startup time (lazy init, GraalVM native image awareness); HTTP response compression (gzip/Brotli); avoid unnecessary serialization — use projections not full entities.

### 12.3 — Frontend Performance
Core Web Vitals (LCP <2.5s, INP <200ms, CLS <0.1); bundle size analysis; route-level code splitting (`React.lazy` + `Suspense`); image optimization (WebP/AVIF, `srcset`); critical rendering path (inline critical CSS, defer non-critical JS); `React.memo`/`useMemo`/`useCallback` only when profiling shows unnecessary re-renders.

### 12.4 — Caching & Infrastructure Performance
Redis cache-aside with hit/miss monitoring; cache warming; cache stampede protection; CDN cache policies and invalidation; HTTP/2 multiplexing + header compression; **HTTP/3/QUIC** wins on high packet-loss networks (mobile).

### 12.5 — Load Testing
k6 metrics — p50/p95/p99 latency, error rate, throughput; load / stress / spike / soak test types; find the breaking point, fix it, find the next one.

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| k6 | Load test — find where it breaks |
| Java Flight Recorder / async-profiler / JProfiler | CPU and memory profiling for Java |
| Chrome DevTools Performance | Profile frontend render bottlenecks |
| Grafana | Visualize p95/p99 latency, error rates, throughput |
| Lighthouse | Frontend Core Web Vitals audit |
| pgBadger | PostgreSQL slow-query analysis |

> **PROJECT:** Load test your Master Project with k6. Profile the slowest endpoints. Optimize the top 3 bottlenecks. Measure Core Web Vitals before/after frontend optimizations. Document every change and its measured impact.

---

## Phase 13 — DevOps & Infrastructure

> Infrastructure is not someone else's problem. Engineers who understand the full stack from code to production build systems that actually work reliably.

### 13.1 — Git Mastery (Team Context)
Branching strategies — Git Flow, Trunk-based, GitHub Flow; advanced git — rebase vs merge tradeoffs, interactive rebase, `git bisect`, hooks; conventional commits.

### 13.2 — Linux & Bash for Operations
File permissions (`chmod`/`chown`/`umask`); processes (`ps`/`top`/`htop`/`kill`, `systemd` services); SSH (key auth, `ssh-agent`, bastion hosts, port forwarding); shell scripting (`set -euo pipefail`, idempotent scripts, `flock` to prevent overlapping cron runs); `strace`/`ltrace`/`perf`/eBPF for tracing syscalls and kernel behavior; `netstat`/`ss`, `tcpdump` for network debugging.

### 13.3 — Containerization — Docker
Images vs containers; Dockerfile instructions (`FROM`, `WORKDIR`, `COPY`, `RUN`, `EXPOSE`, `ENV`, `ENTRYPOINT`, `CMD`); **multi-stage builds** (build with Maven, run on JRE-only minimal image); layer caching (rarely-changing instructions first); `.dockerignore`; networking (bridge/host/none); **Docker Compose** for multi-service local dev; container security (non-root `USER`, no secrets in images, scan with Snyk/`docker scan`, minimal base images like `alpine`/`distroless`); named volumes (DB data) vs bind mounts (dev hot reload).
**Practice:** Multi-stage Dockerfile for Spring Boot: build with Maven, run with JRE only. `docker-compose.yml` for Spring Boot + PostgreSQL + Redis + RabbitMQ.

### 13.4 — CI/CD — GitHub Actions
Workflows/jobs/steps hierarchy; secrets (`${{ secrets.X }}` — never hardcode); env vars at workflow/job/step level; matrix builds (multiple Java/Node versions); dependency caching (Maven/npm); artifacts; environments + required-reviewer approvals (staging → production gates); reusable workflows.
**Build a pipeline that:** runs on every push/PR → lints → unit tests → integration tests (Testcontainers) → coverage gate → builds Docker image → pushes to registry on merge to main → deploys to staging → requires approval for production.

### 13.5 — Container Orchestration — Kubernetes
Pod, Deployment, Service, Ingress (core objects); ConfigMap & Secret (externalized config); Horizontal Pod Autoscaler; rolling deployments (zero downtime); liveness vs readiness probes; resource limits (CPU/memory).
> Awareness-to-working level is enough for most fullstack roles; go deeper (operators, CRDs, Helm charts) only if you move toward platform engineering.

### 13.6 — Infrastructure as Code — Terraform
Declarative infra; workflow — `init` → `plan` → `apply` → `destroy`; providers, resources, variables, outputs; **remote state** (S3 backend + DynamoDB lock table for teams — never local state in production); modules (reusable components); data sources.
**Practice:** Provision a full AWS stack — VPC, EC2, RDS, S3, CloudFront, IAM roles.

### 13.7 — AWS Core Services
| Service | What to Learn |
|---------|--------------|
| EC2 | Instance types, security groups, key pairs, AMIs, user data scripts |
| S3 | Bucket policies, presigned URLs, multipart uploads, static hosting, lifecycle rules |
| RDS | Managed PostgreSQL, Multi-AZ, read replicas, automatic backups |
| ElastiCache | Managed Redis, cluster mode, replication groups |
| CloudFront | CDN, distributions, cache behaviors, invalidations |
| IAM | Least privilege, roles over users, never root |
| CloudWatch | Log groups, metrics, dashboards, alarms |
| Route 53 | Hosted zones, A/CNAME/alias records, health checks, routing policies |
| SQS / SNS | Standard vs FIFO queues, visibility timeout, dead letter queues |
| ECS / Fargate | Task definitions, services, load balancer integration — containers without managing servers |
| Lambda | Cold starts, execution limits, event triggers, statelessness |
| ACM | Free auto-renewing TLS certs, integrates with ALB/CloudFront |

### 13.8 — Web Servers & Proxies
**Nginx** — reverse proxy, load balancing, SSL termination, rate limiting, static file serving; Let's Encrypt + Certbot for free auto-renewing SSL.

### 13.9 — Monitoring & Observability
**Three pillars:** logs (structured JSON, correlation IDs, centralized aggregation), metrics (RED method — Rate, Errors, Duration), distributed tracing (follow a request across services).
**Tools:** Prometheus (scrape `/actuator/prometheus`), Grafana (dashboards + alerting), Sentry (real-time error tracking), Spring Boot Actuator + Micrometer.
**SLOs/SLIs/SLAs:** SLI = the metric, SLO = the target, SLA = the customer contract, error budget = acceptable downtime before freezing releases.

### 13.10 — Deployment Strategies
| Strategy | When to Use |
|----------|-------------|
| Recreate | Dev/staging only — has downtime |
| Rolling | Zero downtime — gradual replacement |
| Blue-Green | Instant rollback — two identical environments |
| Canary | Safest — release to small % of users first |
| Feature Flags | Decouple deploy from release |

### 13.11 — Backups & Cron
3-2-1 backup rule; automated restore testing; point-in-time recovery; cron job syntax, logging, `flock` to avoid overlapping runs.

### Ecosystem Awareness — Infra Landscape
- **Other clouds:** Azure (VMs, Blob Storage, Functions, AKS), Google Cloud (Compute Engine, Cloud Storage, Cloud SQL, GKE, BigQuery).
- **Other web servers/proxies:** Apache (`.htaccess`), Caddy (automatic HTTPS), Traefik (cloud-native, auto service discovery), HAProxy (high-perf L4/L7), Varnish (HTTP accelerator/edge caching).
- **Other CI/CD:** GitLab CI, Jenkins, CircleCI; **ArgoCD** (GitOps for Kubernetes — cluster state declared in Git, auto-synced).
- **Other IaC:** Pulumi (imperative languages instead of HCL), CloudFormation (AWS-native), Ansible (agentless config management), Puppet/Chef (agent-based, enterprise legacy).
- **Serverless/edge platforms:** Vercel/Netlify (Edge Functions, ISR), Cloudflare Workers (V8 isolates, zero cold start, KV/Durable Objects), Deno Deploy, Supabase (Postgres BaaS), Firebase.
- **Observability extras:** Loki (log aggregation), Jaeger/Zipkin (distributed tracing), OpenTelemetry (vendor-neutral instrumentation standard), PagerDuty/Opsgenie (on-call, alert fatigue prevention).
- **FinOps** — see [Phase 17](#176--finops--cloud-cost-engineering).

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| Docker + Compose | Multi-service local dev environments |
| GitHub Actions | Full CI/CD pipeline with gates |
| Terraform | Provision your entire AWS stack in code |
| Prometheus + Grafana | Real dashboards and alerts |
| AWS free tier | Deploy real infrastructure |

> **PROJECT:** Deploy your Master Project to AWS using EC2 + RDS + ElastiCache + S3 + CloudFront, provisioned entirely with Terraform. GitHub Actions deploys automatically on merge to main. Prometheus + Grafana dashboards with error-rate and latency alerts.

---

## Phase 14 — Security Fundamentals

> Security is a property of every feature, not a feature itself. The question is never "will we be attacked?" — it's "when, and will we be ready?"

### 14.1 — Security Mindset
Think like an attacker; defense in depth; principle of least privilege; assume breach.

### 14.2 — OWASP Top 10
| # | Vulnerability | Prevention |
|---|--------------|-----------|
| 1 | Broken Access Control | Server-side authorization on every request; check ownership, not just auth |
| 2 | Cryptographic Failures | TLS everywhere, Argon2 for passwords, AES-256 at rest |
| 3 | Injection | Parameterized queries always — never concatenate user input |
| 4 | Insecure Design | Threat model before building (STRIDE framework) |
| 5 | Security Misconfiguration | No default creds, debug off in prod, no public S3 buckets |
| 6 | Vulnerable Components | Snyk/Dependabot in CI, patch regularly, SBOM generation |
| 7 | Authentication Failures | Rate limiting, MFA, secure session management |
| 8 | Data Integrity Failures | Code signing, verify dependency integrity |
| 9 | Logging & Monitoring Failures | Log all auth events and anomalies |
| 10 | SSRF | Whitelist URLs, block internal IP ranges |

### 14.3 — Authentication Deep Dive
Password storage — never plaintext/MD5/SHA-1, use **Argon2id** (or bcrypt); JWT structure (`header.payload.signature`); signing — HS256 (symmetric) vs RS256 (asymmetric); validation — signature, `exp`, `iss`, `aud`; **access + refresh token pattern** — short-lived access (15 min), long-lived refresh (7–30 days) with **rotation on every use** to detect theft by replay; OAuth2 flows — Authorization Code + PKCE (public clients), Client Credentials (machine-to-machine); **TOTP MFA**; session cookies vs JWT tradeoffs; store JWTs in `httpOnly` cookies, not `localStorage` (XSS can't read `httpOnly`).

### 14.4 — Authorization
RBAC (Role-Based) vs ABAC (Attribute-Based); **IDOR** — always verify ownership server-side; enforce authorization server-side, never trust the client.

### 14.5 — Common Attack Vectors (learn by doing on PortSwigger)
SQL injection (`' OR '1'='1`); XSS — stored/reflected/DOM-based (output encoding, CSP, `httpOnly` cookies); CSRF (`SameSite` cookies + CSRF tokens); **SSRF** (whitelist domains, block internal IP ranges); clickjacking (`X-Frame-Options`, CSP `frame-ancestors`); directory traversal (canonicalize + whitelist paths); mass assignment (explicit allowlists for bindable fields); insecure deserialization (never deserialize untrusted data blindly); JWT attacks (`alg:none`, weak secrets).

### 14.6 — Security Headers
```
Content-Security-Policy
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000; includeSubDomains
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy
```

### 14.7 — Cryptography Fundamentals
Symmetric (AES-256, same key both ways) vs asymmetric (RSA/ECC, public encrypts/private decrypts); hashing (SHA-256 for integrity, Argon2/bcrypt for passwords — never MD5/SHA1); digital signatures; key management (never hardcode, rotate regularly, AWS KMS/HashiCorp Vault).

### 14.8 — Secrets Management
Never commit secrets to Git; env vars for local dev; **AWS Secrets Manager**/Parameter Store or **HashiCorp Vault** for production; rotate regularly and immediately after a team departure.

### 14.9 — API Security
Auth on every endpoint, no exceptions; rate limiting (brute force + abuse prevention); input validation (whitelist not blacklist); output filtering; API keys (rotation, scope restriction); email security — SPF/DKIM/DMARC.

### Ecosystem Awareness — Security Beyond the App
- **Zero Trust Architecture** — no implicit trust even inside your own network; verify identity at every hop; micro-segmentation; identity-aware proxies.
- **WAF** — filter/monitor HTTP traffic at the edge; OWASP Core Rule Set; bot detection; DDoS mitigation. Tools: AWS WAF, Cloudflare WAF, ModSecurity.
- **SIEM** — aggregate logs across systems, correlation rules, real-time threat detection, incident-response integration. Tools: Splunk, Elastic SIEM, AWS Security Hub.
- **Penetration testing basics** — reconnaissance, vulnerability scanning, exploit validation, reporting. Tools: Burp Suite, OWASP ZAP.
- **Compliance awareness** — what each standard requires *from your code*, not full expertise:

| Standard | What It Requires from Engineering |
|----------|-----------------------------------|
| GDPR | Data minimization, right to erasure, 72h breach notification, encryption at rest/transit |
| PCI-DSS | Never store raw card data, tokenization, audit logs, quarterly scans |
| SOC2 | Availability/security/confidentiality controls, audit trail, access reviews |
| HIPAA | Encryption, access controls, audit logs, minimum-necessary access |

### Tools & Practice
| Tool | What to Do |
|------|-----------|
| PortSwigger Web Security Academy | Free hands-on labs — the best practical security resource |
| OWASP ZAP | Scan your own apps for vulnerabilities |
| Burp Suite | Intercept and manipulate HTTP requests like an attacker |
| TryHackMe | Guided security challenges |
| Snyk / Dependabot | Scan dependencies for known CVEs in CI |
| HashiCorp Vault | Production secrets management |
| AWS WAF / Cloudflare | WAF setup and rule configuration |

> **PROJECT:** Build a complete auth system (Argon2, JWT + refresh rotation, rate limiting, TOTP MFA). Intentionally build a vulnerable endpoint, attack it yourself with OWASP ZAP, then fix it. Complete the SQL injection and XSS learning paths on PortSwigger. Generate an SBOM for your project and review every dependency.

---

# Part V — Becoming Senior

## Phase 15 — The Senior Mindset & Problem Solving

> Code is the output. Thinking is the work. The best engineers ask the best questions, not type the fastest.

### 15.1 — Developer vs Engineer
| Developer | Engineer |
|-----------|----------|
| Solves the problem given | Questions whether it's the right problem |
| Writes code that works | Writes code that lasts |
| Thinks in features | Thinks in systems |
| Reacts to bugs | Anticipates failure |
| Asks "how?" | Asks "why?" and "what if?" |
| Finishes the task | Owns the outcome |

### 15.2 — First Principles Thinking
Break every problem to fundamental truths; ask "why?" five times; question every assumption before writing a line of code.
> Example: "Our API is slow." Analogy thinking adds a cache reflexively. First principles asks: *where exactly* is time spent — database, network, serialization, computation? Is it slow for all users or specific ones? Always, or only under load? Measure first, then solve the real problem.

### 15.3 — Debugging — The Scientific Method
1. **Observe** — what's *actually* happening, not what you assume.
2. **Hypothesize** — what could cause this specific behavior?
3. **Predict** — if the hypothesis is right, what else must be true?
4. **Test** — verify or disprove with evidence.
5. **Repeat** until root cause.

Core habits: reproduce consistently before anything else; change one variable at a time; read the error message fully; binary-search the problem (including `git bisect` through commit history); check your assumptions — "it must be X" is usually where the bug hides; take a break — fresh eyes catch what tired eyes miss.

**Tools:** debugger (breakpoints, conditional breakpoints, watch expressions); profiler (CPU sampling, memory allocation tracking); network tools (`curl`, `tcpdump`, Wireshark, `netstat`/`ss`); static/dynamic analysis (SonarQube, ESLint, Valgrind, AddressSanitizer/ThreadSanitizer for lower-level work).

### 15.4 — Thinking in Tradeoffs
Speed vs correctness; performance vs readability; consistency vs availability; short-term velocity vs long-term maintainability; build vs buy.
> There are no right answers in engineering — only **defensible decisions**.

### 15.5 — Estimation
Break work into small chunks, estimate pieces not wholes; reference-class forecasting (how long did similar things actually take?); three-point estimation (best/likely/worst); track estimates vs actuals; raise flags early — before the deadline, not after.

### 15.6 — Core Daily Habits
Measure before optimizing; name things precisely (vague names hide vague thinking); write before coding (pseudocode, design notes, diagrams); ask why before how; seek feedback early — fail small; read the source, not just tutorials; the Boy Scout Rule on every PR.

### Recommended Books
| Book | What You Get |
|------|-------------|
| Clean Code — Robert Martin | How to write maintainable code |
| The Pragmatic Programmer — Hunt & Thomas | Engineering mindset and craft |
| Designing Data-Intensive Applications — Kleppmann | The bible of distributed systems |
| A Philosophy of Software Design — Ousterhout | Depth vs complexity tradeoffs |
| Staff Engineer / An Elegant Puzzle — Will Larson | How senior engineers and orgs think and operate |
| System Design Interview — Alex Xu | Practical system design patterns |
| A Mind for Numbers — Barbara Oakley | How to learn hard technical concepts |
| Thinking, Fast and Slow — Kahneman | Your own cognitive biases |

---

## Phase 16 — Communication & Collaboration

> The engineers who advance fastest make everyone around them more effective. Communication is a multiplier on all your technical skills.

### 16.1 — Why Communication Is a Technical Skill
Code is written once and read dozens of times — writing clearly *is* engineering. Every misunderstood requirement costs 10x more to fix after it's built.

### 16.2 — Written Communication
**Technical documentation:** README (what/how/why); **ADR (Architecture Decision Records)** — document *why*, not just what; design documents (problem, proposed solution, alternatives, tradeoffs); RFCs (propose + invite feedback before building); post-mortems (what happened, root cause, timeline, concrete prevention steps).
**Day-to-day:** ticket descriptions with clear acceptance criteria; PR descriptions (what/why/how to test); commit messages that explain *why*.

### 16.3 — Code Reviews
**Giving:** correctness → design → style, in that order; ask questions instead of demanding ("What happens if X is null here?" not "Fix this"); distinguish blocking issues from suggestions explicitly; explain the why behind every suggestion; acknowledge what's done well.
**Receiving:** it's not criticism of you, it's improvement of the product; respond to every comment; push back respectfully with reasoning, not emotion.

### 16.4 — Meetings & Working in a Team
Come prepared with the goal; ask clarifying questions early; summarize decisions in writing afterward; **disagree and commit** — voice disagreement once clearly, then execute; update stakeholders proactively — bad news early beats bad news late; share context generously; pair programming as a fast way to level up and transfer knowledge simultaneously; mentor by asking questions, not giving answers.

### 16.5 — Communicating Technical Concepts
**Feynman Technique** — explain it as if teaching someone with zero background; the gaps reveal what you don't actually understand. Know your audience (a CTO needs tradeoff + business impact, not implementation detail). Use analogies. Visualize — a diagram communicates in seconds what paragraphs take minutes.

### 16.6 — Async-First Communication
Default to written, searchable, durable communication over synchronous meetings; batch questions; include full context in every async message (the reader has no idea what you were just thinking); set and respect response-time expectations.

---

## Phase 17 — Engineering Practices & Methodologies

> Great engineers aren't just technically strong — they know how to operate effectively inside teams, products, and organizations. This is the operating system underneath all the technical skills.

### 17.1 — Agile / Scrum / Kanban / XP

**Agile — the philosophy:** iterative delivery over big-bang releases; responding to change over following a rigid plan; individuals/interactions over process/tools. What Agile is *not*: "no documentation," "no planning," or "move fast and break things."

**Scrum:**
| Element | What It Actually Is |
|---|---|
| Sprint | Time-boxed iteration (1–2 weeks), produces a shippable increment |
| Sprint Planning | Team pulls backlog work it can complete this sprint |
| Daily Standup | 15-min sync — what I did, what I'll do, what blocks me — not a status report |
| Sprint Review | Demo working software, get real feedback |
| Retrospective | What went well/didn't/changes — must produce action items |
| Product Owner | Prioritizes backlog, represents the business |
| Scrum Master | Removes blockers, protects the team, improves process |
| Story Points | Relative complexity (Fibonacci: 1,2,3,5,8,13) — not hours |

**Kanban:** continuous flow, no sprints; **WIP limits** — the single most important Kanban concept, exposes bottlenecks; pull-based; track cycle time. Beats Scrum for support/ops/unpredictable incoming work.

**XP (Extreme Programming):** pair programming; TDD; continuous integration; collective code ownership; continuous refactoring — the origin of most modern engineering practices.

**Cargo-cult Agile to recognize:** story points used as performance metrics; retros with no action items; "we're Agile" as an excuse to skip design; sprints run as mini-waterfalls; 45-minute "standups"; velocity treated as a commitment.

### 17.2 — Lean / Lean Startup
Eliminate waste; build quality in (catch defects early); defer commitment; deliver in small batches.
**Build-Measure-Learn loop**; **MVP** — not a broken product, the smallest thing that tests a real hypothesis; validated learning (real behavior, not opinions); pivot or persevere; innovation accounting (activation/retention/referral over vanity metrics).
> Before implementing a feature, ask: what hypothesis are we testing? What's the smallest version that tests it? How will we know if it worked?

### 17.3 — Domain-Driven Design (DDD) — Deep Dive
**Strategic design:** **Bounded Context** (a boundary within which a model has one unambiguous meaning — "Order" means different things in Sales vs Fulfillment vs Accounting); **Ubiquitous Language** (same terms in code and business conversation); **Context Map**; **Anti-Corruption Layer** (translation layer that keeps an external model's mess out of your domain).

**Integration patterns between bounded contexts:**
| Pattern | Meaning |
|---|---|
| Shared Kernel | Two teams share a subset of the model — tight coupling |
| Customer/Supplier | Upstream publishes, downstream consumes |
| Conformist | Downstream adopts upstream's model as-is |
| Anti-Corruption Layer | Translate between models, isolate your domain |
| Published Language | Well-documented shared contract (JSON schema, OpenAPI) |

**Tactical design:** Entities (identity persists over time); Value Objects (no identity, immutable — `Money`, `Address`); Aggregates (a cluster with one Aggregate Root controlling access and invariants); Repository (persistence abstraction for aggregates); Domain Events (`OrderPlaced`, past tense, immutable); Domain Service (logic that doesn't belong to one entity); Application Service (thin orchestration, no business logic).

**Event Storming:** collaborative workshop — discover domain events on a timeline (orange), then commands (blue)/actors (yellow)/policies (purple)/read models (green); the output is shared understanding, not just a diagram.

### 17.4 — The 12-Factor App
| Factor | What It Means | Why It Matters |
|--------|--------------|----------------|
| 1. Codebase | One codebase, many deploys | No snowflake servers |
| 2. Dependencies | Explicitly declared and isolated | Never assume a system library is installed |
| 3. Config | In environment variables, not code | Never hardcode URLs/creds — **violated most often** |
| 4. Backing Services | Treat DB/cache/queue as attached resources | Swap local Postgres for RDS with zero code change |
| 5. Build, Release, Run | Strictly separate stages | Same artifact runs everywhere |
| 6. Processes | Stateless | State lives in backing services, not memory |
| 7. Port Binding | Self-contained service export | App doesn't plug into a container process |
| 8. Concurrency | Scale via the process model | Horizontal, not vertical |
| 9. Disposability | Fast startup, graceful shutdown | Handle `SIGTERM`, drain in-flight requests |
| 10. Dev/Prod Parity | Keep environments similar | Docker Compose with real Postgres locally, not H2 |
| 11. Logs | Treat as event streams | Write to stdout — **violated most often** |
| 12. Admin Processes | One-off, same codebase | Migrations run as part of the app, not ad-hoc scripts |

### 17.5 — Technical Debt Quadrant
```
                    DELIBERATE
    Prudent & Deliberate  |  Reckless & Deliberate
    "We know the shortcut |  "We don't have time
     and will fix it"     |   for design" — avoid
PRUDENT ------------------+------------------ RECKLESS
    Prudent & Inadvertent |  Reckless & Inadvertent
    "Now we know how it   |  "What's layering?"
     should be designed"  |   — fix via mentorship
                    INADVERTENT
```
Track debt explicitly (a backlog item, not a memory); quantify the cost; Boy Scout Rule on every PR; never let reckless debt accumulate — it compounds faster than financial debt; pay it down continuously, not in a mythical "cleanup sprint."

### 17.6 — Spikes, OKRs & FinOps
**Spikes** — time-boxed investigations that produce *knowledge*, not production code; use when the team disagrees, the tech is unfamiliar, or the estimate is wildly uncertain; always time-box it explicitly.

**OKRs** — Objective (ambitious, qualitative) + Key Results (measurable outcomes); not a task list — measures outcomes; ambitious is fine (70% of a stretch OKR often beats 100% of a safe one); 3–5 max, or it becomes noise.

**FinOps (cloud cost engineering):** right-sizing instances to actual load; reserved instances/savings plans (30–70% off for 1–3yr commit); spot instances (up to 90% off, interruptible — batch/CI only); tagging every resource (team/env/service/cost-center); cost alerts before a surprise bill, not after. Engineering decisions that drive cost: DB over-provisioning, NAT Gateway cross-AZ traffic, log retention, forgotten staging environments. Tools: AWS Cost Explorer, Infracost (cost diff on Terraform PRs), Kubecost.
> A senior engineer thinks about correctness, performance, maintainability, *and* cost in the same breath: "Lambda costs $3/month here vs $150/month on EC2 — here's the tradeoff."

---

# Part VI — Putting It Together

## The Complete Tech Stack Reference

### Languages
- **Java 21** — backend, algorithms, everything server-side
- **TypeScript** — all frontend and mobile code
- **JavaScript** — browser fundamentals, event loop understanding
- **SQL** — PostgreSQL, queries, schema design, performance
- **Bash** — terminal, scripting, Linux daily use
- **YAML** — Docker, CI/CD, Kubernetes configs
- **HCL** — Terraform infrastructure as code

### Frontend
HTML5 (semantic) → plain CSS → Tailwind CSS → JavaScript (ES6+, deeply) → TypeScript (strict) → React 18+ → Next.js → TanStack Query, Zustand, React Router, Vite → Playwright (E2E), Vitest (unit)

### Mobile
React Native, Expo, Expo Router, NativeWind, React Navigation, EAS Build + Submit, Fastlane

### Backend
Java 21, Spring Boot 3, Spring Security, Spring Data JPA, Hibernate, MapStruct, Flyway, JUnit 5, Mockito, Testcontainers, Maven, resilience4j

### Databases
PostgreSQL (primary), pgvector (AI), Redis (cache/pub-sub/queues), PgBouncer, Flyway

### Infrastructure & DevOps
Linux (Ubuntu), Git, GitHub Actions, Docker + Docker Compose, Kubernetes (working awareness), AWS (EC2, RDS, S3, ElastiCache, CloudFront, IAM, CloudWatch, Route 53, SQS, ECS), Terraform, Nginx, Let's Encrypt

### Observability
Prometheus, Grafana, Sentry, Spring Boot Actuator + Micrometer

### API & Communication
REST, WebSockets, RabbitMQ, OpenAPI/Swagger — plus GraphQL/gRPC awareness

### AI
Claude API / OpenAI API, pgvector for RAG, Langfuse for observability

### Learning Resources
LeetCode + Neetcode.io, VisuAlgo, Excalidraw, PortSwigger Web Security Academy, Refactoring.guru, nand2tetris.org, Designing Data-Intensive Applications (Kleppmann)

---

## The Master Project

> Build one serious fullstack platform that touches every phase of this roadmap — not a tutorial project, a real engineering challenge.

### What You're Building
A **Developer Tools SaaS Platform** — a real product developers would pay for.

| Feature | Engineering Challenge |
|---------|----------------------|
| User Authentication | JWT + refresh token rotation + Argon2 + TOTP MFA |
| API Key Management | Generate, rotate, revoke — scoped permissions |
| Rate Limiting | Per-user Redis token bucket, distributed across nodes |
| Background Jobs | RabbitMQ + Spring Boot workers + dead letter queue + idempotency |
| Real-time Notifications | WebSockets + Redis pub/sub |
| AI-Powered Search | RAG with pgvector — search across user's documents |
| Usage Analytics | Metrics collection + Grafana dashboards |
| Billing Integration | Stripe webhooks with idempotency keys |
| Multi-tenancy | Row-level security in PostgreSQL |
| Mobile App | React Native companion app with offline support |

### Technology Stack
| Layer | Technology |
|-------|-----------|
| Frontend | React 18 + TypeScript + TanStack Query + Zustand + Tailwind CSS |
| Mobile | React Native + Expo + NativeWind + Expo Router |
| Backend | Java 21 + Spring Boot 3 + Spring Security |
| Database | PostgreSQL + pgvector + Redis |
| Messaging | RabbitMQ |
| Infrastructure | Docker + AWS (EC2, RDS, ElastiCache, S3, CloudFront) |
| IaC | Terraform |
| CI/CD | GitHub Actions — lint, test, build, deploy on every push |
| Monitoring | Prometheus + Grafana + Sentry |
| Testing | JUnit 5 + Mockito + Testcontainers + Playwright + k6 |
| AI | Claude API + pgvector RAG |

### Build It in This Order
1. Core API — user auth, JWT, basic CRUD with PostgreSQL
2. Frontend — React + TypeScript, connect to your API
3. Testing from the start — unit, integration, E2E
4. Infrastructure — Docker Compose, then CI/CD pipeline
5. Advanced backend — RabbitMQ jobs, WebSockets, Redis caching
6. Deploy to AWS with Terraform
7. Monitoring — Prometheus, Grafana, Sentry
8. AI feature — RAG search with pgvector
9. Billing — Stripe integration
10. Mobile app — React Native + Expo

---

## Suggested Progression (No Deadlines, Just Order)

There is no clock on this. The table below is a *sequencing* guide, not a schedule — go as slow as depth requires.

| Stage | Focus |
|-------|-------|
| 1 | Foundation Reset + CS Fundamentals — Linux daily driver, build an HTTP server on raw TCP sockets, event loop mastery |
| 2 | Data Structures & Algorithms + Java fundamentals — implement every structure from scratch, one pattern problem a day |
| 3 | Software Design + Spring Boot basics — patterns in mini-projects, first real REST API + Postgres |
| 4 | Frontend deep dive + Docker + CI/CD — React/TypeScript mastery, GitHub Actions pipeline, Docker Compose full stack |
| 5 | Databases internals + System Design + AWS — the 1M-row index challenge, 2 system designs a week, Terraform deploy |
| 6 | Testing + Performance + Security — TDD discipline, load testing, PortSwigger labs, full auth system |
| 7 | Mobile (React Native) + API Design breadth — companion app, GraphQL/gRPC exploration |
| 8 | Data Engineering awareness — understand the data pipeline landscape |
| 9 | Master Project — apply everything, end to end, with monitoring, billing, and mobile |
| Ongoing | Senior mindset, communication, engineering practices — these compound for the rest of your career, they never "finish" |

---

> **The goal is not to finish this roadmap.**
>
> **The goal is to become someone who thinks in systems, designs with tradeoffs, builds with discipline, and grows without a ceiling.**
>
> *Start today. Go deep. Stay consistent. The rest follows.*
