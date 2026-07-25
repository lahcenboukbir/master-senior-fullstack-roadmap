# Part I — Core Engineering Foundations (Detailed)
### Companion deep-dive to `Master_Senior_Fullstack_Roadmap.md` → Phases 1, 2, 3

> This file expands **Phase 1 (Computer Science Fundamentals)**, **Phase 2 (Data Structures & Algorithms)**, and **Phase 3 (Software Design & Clean Code)** into full depth — the mental models that separate an engineer from someone who only knows how to make code run.
>
> **Why this matters more than it looks like it should:** almost nothing in Phase 1 shows up directly in your day job as syntax. But it shows up constantly as *intuition* — why a query is slow, why a thread pool is starving, why a cache invalidation bug happened, why a recursive function blew the stack. Phase 2 is not about passing interviews; it's about having the right tool ready when a real problem needs it. Phase 3 is about writing code that a team — including future-you — can still change safely a year from now.
>
> Go in order within each phase. Every "Answer This" should be answerable without looking it up before you move on.

---

## Table of Contents

**Phase 1 — Computer Science Fundamentals**
- 👀 [1.1 — How a CPU Works](#11--how-a-cpu-works)
- 🎯 [1.2 — Memory Hierarchy](#12--memory-hierarchy)
- 🎯 [1.3 — Operating Systems Fundamentals](#13--operating-systems-fundamentals)
- 👀 [1.4 — Networking Deep Dive](#14--networking-deep-dive)
- 🎯 [1.5 — How the Browser Works](#15--how-the-browser-works)
- 👀 [1.6 — Compilers, Interpreters & the JVM](#16--compilers-interpreters--the-jvm)
- [Phase 1 Capstone](#phase-1-capstone)

**Phase 2 — Data Structures & Algorithms**
- 🎯 [2.1 — Complexity Analysis](#21--complexity-analysis)
- 🎯 [2.2 — Arrays & Strings](#22--arrays--strings)
- 🎯 [2.3 — Linked Lists](#23--linked-lists)
- 🎯 [2.4 — Stacks & Queues](#24--stacks--queues)
- 🎯 [2.5 — Hash Maps & Hash Sets](#25--hash-maps--hash-sets)
- 🎯 [2.6 — Trees](#26--trees)
- 👀 [2.7 — Heaps](#27--heaps)
- 👀 [2.8 — Graphs](#28--graphs)
- ⏸️ [2.9 — Tries](#29--tries)
- ⏸️ [2.10 — Union-Find (Disjoint Set Union)](#210--union-find-disjoint-set-union)
- 🎯 [2.11 — Sorting Algorithms](#211--sorting-algorithms)
- 🎯 [2.12 — Searching & Shortest Paths](#212--searching--shortest-paths)
- 👀 [2.13 — Dynamic Programming](#213--dynamic-programming)
- ⏸️ [2.14 — Advanced & Niche DS&A](#214--advanced--niche-dsa)
- 🎯 [2.15 — Problem-Solving Patterns Reference](#215--problem-solving-patterns-reference)
- [Phase 2 Capstone](#phase-2-capstone)

**Phase 3 — Software Design & Clean Code**
- 🎯 [3.1 — Clean Code Principles](#31--clean-code-principles)
- 🎯 [3.2 — SOLID Principles](#32--solid-principles)
- 🎯 [3.3 — Design Patterns](#33--design-patterns)
- 🎯 [3.4 — Architecture Patterns](#34--architecture-patterns)
- 🎯 [3.5 — Refactoring](#35--refactoring)
- [Phase 3 Capstone](#phase-3-capstone)

---

# Phase 1 — Computer Science Fundamentals

**Phase tier: 🎯👀 Mixed — half Master (memory, OS, browser), half Know Only (CPU internals, deep networking, compilers)**

> Most coders use computers without understanding them. This is the mental model layer.

## 1.1 — How a CPU Works

**Tier: 👀 Know Only**

### What to Learn
- **Fetch-decode-execute cycle** — the CPU fetches an instruction from memory, decodes what it means, executes it, and repeats — billions of times per second. Everything your code does eventually reduces to this loop.
- **Registers** — the CPU's own tiny, ultra-fast storage (dozens of bytes total), where the actual arithmetic happens; every value that gets computed on has to be loaded into a register first.
- **ALU (Arithmetic Logic Unit)** — where addition, comparison, and bitwise operations physically happen.
- **Clock speed vs instruction count** — why a 3GHz CPU doesn't do "3 billion of your operations" per second: modern CPUs use pipelining, superscalar execution (multiple instructions per cycle), and out-of-order execution, so raw GHz is a poor proxy for real performance.
- **Binary representation** — two's complement for negative integers (why it makes addition/subtraction "just work" in hardware without special-casing sign); IEEE 754 for floating point (why `0.1 + 0.2 !== 0.3` in every mainstream language — it's not a language bug, it's the representation).
- **Hexadecimal** — why engineers use it (each hex digit maps cleanly to exactly 4 bits, unlike decimal) — memory addresses, color codes, and byte dumps are hex for this reason.
- **Instruction sets** — x86-64 (CISC, complex instructions) vs ARM (RISC, simpler instructions, dominant in mobile/Apple Silicon/cloud ARM instances) — awareness level, not a deep dive.

### Practice
1. Convert 20 decimal numbers to binary and hex by hand (no calculator) until it's fast and automatic.
2. Represent `-5` in 8-bit two's complement by hand, then verify by adding it to `5` and confirming you get `0` (with overflow discarded).
3. Run `0.1 + 0.2` in a JS console or Java program, observe the result isn't exactly `0.3`, and explain why using IEEE 754 in your own words.
4. Compile a trivial Java method and inspect it with `javap -c ClassName` — identify the load/store/arithmetic bytecode instructions corresponding to your source.

### Answer This
- Why is `0.1 + 0.2 !== 0.3` not a bug in your language, but a property of how floating point is represented?
- Why doesn't a CPU's clock speed alone tell you how fast it will run your program?
- Why does two's complement let hardware use the same circuit for addition regardless of sign?

---

## 1.2 — Memory Hierarchy

**Tier: 🎯 Master**

### What to Learn
- **The hierarchy and real latency numbers** — Registers (~1 cycle) → L1 cache (~4 cycles) → L2 cache (~12 cycles) → L3 cache (~40 cycles) → RAM (~100-300 cycles) → SSD (~100,000 cycles) → HDD (~10,000,000 cycles). Each step down is roughly an order of magnitude slower — internalize this ratio, not the exact numbers.
- **Cache lines** — memory is fetched from RAM into cache in fixed-size chunks (typically 64 bytes), not one byte at a time; this is *why* sequential access patterns are dramatically faster than random ones.
- **Temporal locality** — if you access a piece of data once, you're likely to access it again soon (loop variables, hot function state).
- **Spatial locality** — if you access one memory address, you're likely to access nearby addresses soon (array iteration) — this is what cache lines are designed to exploit.
- **Why this matters in real code** — an `ArrayList`/array beats a `LinkedList` for iteration not because of Big-O (both are O(n)) but because of cache locality: contiguous memory means fewer cache misses.

### Practice
1. Write two Java programs operating on the same large 2D array (e.g., 5000×5000 `int[][]`): one iterates row-by-row (`arr[i][j]` with `j` innermost), one column-by-column (`arr[j][i]` with `j` outermost... i.e., swap the loop order). Time both. Explain the difference using cache lines, not vague "it's just faster."
2. Compare iterating and summing an `ArrayList<Integer>` vs a `LinkedList<Integer>` of the same size — measure and explain the gap.

### Answer This
- Why can two algorithms with identical Big-O complexity have wildly different real-world performance?
- What does a "cache miss" actually cost, roughly, compared to a cache hit?
- Why does row-major iteration of a 2D array in Java beat column-major iteration, concretely?

---

## 1.3 — Operating Systems Fundamentals

**Tier: 🎯 Master**

### What to Learn

**Processes vs Threads**
- A process has its own isolated address space; a thread shares its process's memory with sibling threads.
- Why threads are cheaper to create than processes (no new address space, no new page tables).
- Thread-local storage vs shared memory — and why shared mutable state is where all the danger lives.

**Stack vs Heap**
- The stack holds function call frames (local variables, return addresses) — LIFO, fixed size per thread, extremely fast to allocate/deallocate (just moving a pointer).
- The heap holds dynamically allocated objects, managed by the runtime/GC — slower to allocate, lives until nothing references it (or until explicitly freed in non-GC languages).
- `StackOverflowError` — what it actually is: unbounded (or too-deep) recursion exhausting the fixed stack space.
- In Java specifically: primitives and object references live on the stack (per method frame); the actual objects and arrays live on the heap.

**Virtual Memory & Paging**
- Every process believes it owns the entire address space — the OS + MMU (Memory Management Unit) maintain the illusion via a **page table** mapping virtual addresses to physical ones.
- Pages are fixed-size chunks (commonly 4KB).
- A **page fault** occurs when a requested page isn't currently in RAM — the OS loads it from disk, which is why the first access to something can be much slower than subsequent ones.
- This is *why* processes are isolated from each other by default — one process cannot see another's physical memory without an explicit shared-memory mechanism.
- **Memory-mapped files (`mmap`)** — how databases and the JVM treat a file on disk as if it were addressable memory, letting the OS handle paging it in/out.

**Scheduling**
- Preemptive scheduling (the OS can interrupt a running thread) vs cooperative (a thread must yield voluntarily) — modern OSes are preemptive.
- CPU-bound tasks (spend most time computing) vs I/O-bound tasks (spend most time waiting on disk/network) behave completely differently under load — an I/O-bound thread should yield the CPU while waiting, a CPU-bound one should get a full time slice.
- **Context switching** — what the CPU has to save/restore (registers, program counter, memory mappings) to switch which thread runs — this has real, measurable overhead, which is *why* spawning 10,000 OS threads tanks performance instead of helping it.

**Concurrency Primitives**
- **Race condition** — two threads reading/writing shared data simultaneously with an unpredictable, incorrect outcome.
- **Critical section** — the code that must run under mutual exclusion.
- **Mutex (lock)** — only one thread may hold it at a time.
- **Semaphore** — generalizes a mutex to allow N concurrent holders (a mutex is a semaphore with count 1).
- **Deadlock** — two or more threads each hold a lock the other needs, and neither will release; prevention via consistent lock ordering, detection via cycle detection in a "waits-for" graph (this is the graph theory from Phase 2 showing up in a real OS concept).
- **Livelock** — threads keep actively yielding to each other in response to one another, but no actual progress happens.
- **Starvation** — a thread never gets scheduled because other threads are consistently prioritized.
- **The Java Memory Model** — why `volatile` and `synchronized` exist: without them, one thread's write to a variable is not guaranteed to ever become visible to another thread (due to CPU caching and compiler reordering), independent of the race-condition problem itself.

**Garbage Collection**
- **Reference counting** — simplest GC strategy, cannot handle reference cycles on its own (two objects referencing each other, neither reachable from the root, never get collected) — used by Python (with a cycle detector bolted on) and Swift (ARC).
- **Mark-and-sweep** — traverse from GC roots, mark everything reachable, sweep (free) everything unmarked.
- **Generational GC** — the empirical observation that most objects die young; segregating memory into a young generation (collected often, cheaply) and an old generation (collected rarely, more expensively) exploits this.
- **Stop-the-world pauses** — the GC must (at least briefly) halt all application threads to safely collect, which is why GC pause time is a first-class performance metric, not just throughput.
- **Java GC algorithms** — Serial (single-threaded, tiny apps), Parallel (multi-threaded, throughput-focused), **G1GC** (default since Java 9 — divides the heap into regions, aims for predictable pause targets), **ZGC** (Java 21+ — sub-millisecond pauses even on huge heaps, via concurrent marking/relocation).
- **JVM heap tuning basics** — `-Xms`/`-Xmx` (initial/max heap size), `-XX:+UseG1GC`.
- **Memory leaks in a GC'd language** — they still happen! Static collections that grow forever, unclosed streams/connections, and (in UI code) event listeners that are never unregistered are the classic culprits — the GC can't free something that's still technically reachable.

**File Systems, IPC & Signals**
- File systems store data via **inodes** (metadata + pointers to data blocks) — this is why renaming a file within the same filesystem is instant (metadata-only change) but moving across filesystems is a real copy.
- `fsync` — forces the OS to actually flush data to physical disk instead of leaving it in a buffer — this is *why* it matters for database durability: without it, a "committed" transaction could vanish on a power loss.
- **IPC (Inter-Process Communication)** — pipes (one-directional byte stream between related processes), message queues, shared memory (fastest, but requires manual synchronization), Unix domain sockets (socket API, but local-machine-only, no network stack overhead).
- **Signals** — `SIGKILL` (immediate, un-catchable termination) vs `SIGTERM` (a polite request to terminate that a process can catch and respond to) — this distinction is exactly why "graceful shutdown" logic exists: your app catches `SIGTERM`, finishes in-flight work, then exits cleanly, whereas `SIGKILL` gives it no chance to do anything.

### Practice
1. Write a Java program where multiple threads increment a shared (non-atomic) `int` counter with no synchronization. Run it, observe the final value is wrong, then fix it with `synchronized`/`AtomicInteger` and confirm correctness.
2. Deliberately construct a two-thread deadlock (Thread A locks X then wants Y; Thread B locks Y then wants X) and reproduce the hang. Fix it by enforcing a consistent lock-acquisition order.
3. Use VisualVM or Java Mission Control to watch GC activity on a running app; force allocation pressure (allocate large short-lived objects in a loop) and watch young-gen collections spike.
4. Send `SIGTERM` (`kill <pid>`, not `kill -9`) to a long-running script and add a handler that logs "shutting down gracefully" before exiting; compare against `kill -9` which gives it no chance to run anything.

### Answer This
- Why does spawning 10,000 OS threads hurt performance instead of helping it, mechanically?
- What is the actual difference between a deadlock and a livelock?
- Why can a memory leak still happen in a garbage-collected language?
- Why does `fsync` matter for database durability specifically, not just "for safety in general"?
- What's the real difference in what `SIGKILL` and `SIGTERM` allow a process to do?

---

## 1.4 — Networking Deep Dive

**Tier: 👀 Know Only**

### What to Learn
- **OSI layers 3–7** — Network (IP — addressing/routing), Transport (TCP/UDP — end-to-end delivery), Session/Presentation (mostly folded into practice today), Application (HTTP, DNS, etc.) — you don't need to recite all 7 layers, but you need the mental model of "each layer only cares about its own job and hands off to the next."
- **IP addressing** — IPv4 vs IPv6, subnets, CIDR notation (`/24` etc.) at a working level.
- **TCP** — the **three-way handshake** (SYN → SYN-ACK → ACK) and why two isn't enough (both sides need to confirm they can both send *and* receive); reliability and ordering guarantees; **congestion control** (slow start, then AIMD — additive increase, multiplicative decrease — the algorithm that prevents one connection from saturating a shared link); **Nagle's algorithm** vs `TCP_NODELAY` (batching small writes for efficiency vs disabling that for latency-sensitive traffic); teardown (FIN/FIN-ACK/ACK) and why `TIME_WAIT` exists (to handle any stray packets from the old connection safely).
- **UDP** — connectionless, no delivery/ordering guarantees, but far lower overhead — why DNS, video streaming, gaming, and VoIP prefer it; datagram boundaries (each `send()` is a discrete message, unlike TCP's byte stream).
- **DNS** — recursive resolution (your resolver does the legwork) vs iterative (a resolver only ever gives you the next hop, forcing the client to keep asking); **TTL** and why "DNS propagation" isn't really propagation — it's just cached records expiring at different times across the internet.
- **TLS 1.3** — the handshake (now effectively 1-RTT), certificate chain validation (root CA → intermediate → leaf, and why your OS/browser ships a list of trusted root CAs), **mTLS** (mutual TLS — both client and server present certificates, common for service-to-service auth).
- **CORS** — exists because of the same-origin policy; a **preflight** `OPTIONS` request is sent by the browser (not your code) before "unsafe" cross-origin requests to ask permission first; most CORS bugs are misconfigured allowed-origins/headers on the server, not a client problem.
- **HTTP/2 vs HTTP/3** — HTTP/2 multiplexes many requests over one TCP connection (fixing HTTP/1.1's head-of-line blocking at the request level) using HPACK header compression; HTTP/3 replaces TCP with **QUIC** (built on UDP) to fix head-of-line blocking *at the transport level* too (one lost packet in HTTP/2 stalls the whole TCP connection; in QUIC it only stalls its own stream).
- **Non-blocking I/O** — `select`/`poll` (older, O(n) scanning of file descriptors) vs `epoll` (Linux, scales far better for many connections) vs `io_uring` (newest, minimizes syscall overhead) — this is *how* a single-threaded event loop (Node.js, Redis) can handle tens of thousands of concurrent connections.
- **Zero-copy (`sendfile`)** — normally, sending a file over a socket means copying it kernel→user space→kernel space again; `sendfile` lets the kernel send it directly, avoiding the redundant copy — a real, measurable win for file-serving workloads.

### Practice
1. Install Wireshark, capture your own traffic while visiting an HTTPS site, and identify the TCP three-way handshake, the TLS handshake, and the actual HTTP request/response — all in the same capture.
2. Use `dig` or `nslookup` against a real domain and trace resolution step by step (or use `dig +trace`) to see recursive resolution happen in front of you.
3. Trigger a real CORS error on purpose (fetch from a different-origin API without proper headers configured) and read the exact browser console message; then fix it server-side and confirm the preflight `OPTIONS` request in the Network tab.
4. Compare, conceptually, what happens to a single HTTP/2 connection vs a single HTTP/3 connection when one packet is lost mid-stream — write down the difference in your own words.

### Answer This
- Why does the TCP handshake need three steps instead of two?
- Why do DNS, gaming, and video streaming prefer UDP over TCP?
- What specifically does a CORS preflight request check, and who sends it?
- Why does HTTP/3 solve head-of-line blocking more completely than HTTP/2?
- What problem does `epoll` solve that `select`/`poll` don't scale well for?

---

## 1.5 — How the Browser Works

**Tier: 🎯 Master**

### What to Learn
- **URL to rendered page, step by step** — DNS lookup → TCP connection → TLS handshake → HTTP request sent → response received → parsing begins.
- **The rendering pipeline** — HTML parsing produces the **DOM**; CSS parsing produces the **CSSOM**; DOM + CSSOM combine into the **render tree** (only visible, styled nodes); **layout** (aka reflow) computes exact size/position of every render-tree node; **paint** fills in pixels; **composite** lets the GPU assemble separately-painted layers into the final frame.
- **Render-blocking resources** — a `<script>` in `<head>` without `defer`/`async` halts HTML parsing until it downloads and executes; `@import` in CSS is similarly blocking.
- **Reflow vs repaint** — reflow (layout) is expensive because it can cascade to the entire page; repaint (just pixels, no layout change) is cheaper; this is exactly why Phase 4/12 in the master roadmap insist on only animating `transform`/`opacity` — those are composite-only, skipping layout and paint entirely.
- **The critical rendering path** — the minimum sequence of steps needed before the user sees anything meaningful; optimizing it is most of what "frontend performance" (Phase 12) actually means at the browser-mechanics level.
- **Why JavaScript execution blocks the HTML parser** — the parser can't know in advance whether a `<script>` will use `document.write()` to inject more HTML, so by default it must stop and wait; `defer` (execute after parsing, in order) and `async` (execute whenever it's ready, out of order) exist specifically to opt out of this.

### Practice
1. Open DevTools' Performance tab, record a page load, and identify the DNS/TCP/TLS/request phases followed by parsing, layout, and paint in the trace.
2. Add a large inline `<script>` (no `defer`/`async`) in `<head>` before visible content, reload, and observe the delay in first paint; then move it to the end of `<body>` or add `defer` and compare.
3. Animate an element's `left`/`top` property vs its `transform: translate()` for the same visual movement; record both in the Performance tab and compare how much layout work each triggers.

### Answer This
- In what order do DOM and CSSOM combine, and what happens to nodes that are styled `display: none`?
- Why does animating `transform` avoid both layout and paint, while animating `top`/`left` doesn't?
- Why does a `<script>` tag without `defer`/`async` block HTML parsing by default?

---

## 1.6 — Compilers, Interpreters & the JVM

**Tier: 👀 Know Only**

### What to Learn
- **Lexing/tokenization** — breaking source text into meaningful tokens (keywords, identifiers, literals, operators) before anything about structure is understood.
- **Parsing & AST** — tokens get assembled into an Abstract Syntax Tree representing the code's actual structure — this is the same tree-traversal knowledge from Phase 2, applied to how your own source code is understood by the tools you use every day.
- **Compilation vs interpretation vs JIT** — a compiler translates source to machine code ahead of time; an interpreter executes source (or an intermediate form) directly, line by line, at runtime; **JIT (Just-In-Time) compilation** is a hybrid — start by interpreting, then compile the "hot" (frequently executed) parts to native machine code on the fly.
- **How Java specifically runs** — your `.java` source compiles to **bytecode** (not machine code) via `javac`; the JVM's **class loader** (bootstrap → extension → application, using a delegation model where each loader asks its parent first) loads that bytecode; the JVM initially **interprets** the bytecode (slow but starts immediately); **HotSpot** then identifies hot methods and JIT-compiles them to real native machine code, applying optimizations like **inlining** (replacing a method call with its body to avoid call overhead), **escape analysis** (detecting an object never "escapes" a method and can be stack-allocated instead of heap-allocated), and **loop unrolling**.
- **Why this matters practically** — it explains real-world JVM behavior that otherwise looks mysterious: startup being slower than steady-state throughput (interpretation before JIT kicks in), benchmarks needing a "warm-up" period to be meaningful, and why a method called once is never as fast as one called a million times in a hot loop.

### Practice
1. Compile a small Java class and inspect its bytecode with `javap -c YourClass` — match specific bytecode instructions (`iload`, `invokevirtual`, etc.) to lines of your source.
2. Write a microbenchmark loop that calls the same method a million times, printing elapsed time for the first 1,000 calls vs the last 1,000 — observe the JIT warm-up effect directly (a real JMH benchmark, from Phase 12, does this properly — this is the quick-and-dirty version to build intuition first).

### Answer This
- What's the actual difference between what `javac` produces and what the CPU ultimately executes?
- Why does a JVM-based application typically get *faster* the longer it runs, up to a point?
- What does "escape analysis" let the JVM safely do that it otherwise couldn't?

---

## Phase 1 Capstone

> **PROJECT:** Build an HTTP server from scratch using raw TCP sockets (Java, no frameworks — just `ServerSocket`/`Socket`). It must: parse a raw HTTP request line and headers by hand (no library parsing), respond with correct status lines and headers, and handle at least GET and POST. Then: capture your own server's traffic in Wireshark and identify the handshake and the raw request/response bytes. Load it with a handful of concurrent clients and watch thread/connection behavior in `htop`. Write up, in your own words, everything that happens from a client typing the URL to your server sending back bytes — you should be able to name every concept from 1.1–1.6 somewhere in that explanation.

---

# Phase 2 — Data Structures & Algorithms

**Phase tier: 🎯 Mostly Master — core structures/patterns Master; heaps/graphs/DP Know Only; tries/union-find/advanced topics Skip**

> Implement everything in Java, from scratch, no built-in collections for the "build" exercises. Understanding comes from building, not from reading someone else's implementation.

## 2.1 — Complexity Analysis

**Tier: 🎯 Master**

### What to Learn
- **Big O** measures growth rate as input size grows — not actual wall-clock speed. An O(n²) algorithm can beat an O(n log n) one for small n; Big O only tells you what happens as n → ∞.
- **Time vs space complexity** — every algorithm has both; sometimes you trade one for the other (memoization spends memory to save time).
- **Analyzing loops** — a single loop over n elements is O(n); nested loops over the same input are O(n²); a loop that halves its range each iteration is O(log n).
- **Best / average / worst case** — and why worst case is what you design for in production (an attacker or an unlucky data distribution *will* eventually hit it).
- **Amortized analysis** — why `ArrayList.add()` is O(1) *amortized* even though any individual call might trigger an O(n) resize: the cost of resizing, spread across all the cheap calls between resizes, averages out to constant time per call.
- **Common complexities with concrete examples** — O(1) HashMap lookup, O(log n) binary search, O(n) linear scan, O(n log n) merge sort, O(n²) nested loops over the same array, O(2ⁿ) naive recursive Fibonacci without memoization.

### Practice
1. Take five functions you've written in the past (any language) and write down the Big-O of each, out loud, explaining why.
2. Write naive recursive Fibonacci and time `fib(35)`. Add memoization and time it again. Explain the difference in terms of complexity class, not just "it's faster."
3. Prove to yourself that `ArrayList.add()` is amortized O(1): instrument a dynamic array's resize operations, log every resize, and count total element-copies across N adds — confirm it grows linearly with N, not quadratically.

### Answer This
- What is the time complexity of a HashMap lookup in the average case, and when does it degrade — and to what?
- Why is O(n log n) the accepted lower bound for comparison-based sorting?
- What does "amortized" mean, precisely, and why doesn't it mean "usually fast"?

---

## 2.2 — Arrays & Strings

**Tier: 🎯 Master**

### What to Learn
- Static arrays — contiguous memory, O(1) index access, fixed size.
- Dynamic arrays — resize by doubling when full, which is *why* `add()` is amortized O(1) (see 2.1).
- Why arrays beat linked lists for random access and iteration — cache locality (see 1.2).
- String immutability in Java — `s = s + "x"` inside a loop is O(n²) overall because each concatenation allocates a new string; `StringBuilder` mutates an internal buffer instead.
- Techniques: **two pointers** (converge from both ends, or move at different speeds), **sliding window** (a variable- or fixed-size window swept across the array, tracking a running aggregate instead of recomputing from scratch), **prefix sums** (precompute cumulative sums so any range-sum query becomes O(1)).

### Build From Scratch
A dynamic array with `get`, `set`, `add` (auto-resize by doubling), and `remove`.

### Practice
1. Benchmark string concatenation in a loop using `+` vs `StringBuilder` for 100,000 iterations — measure the gap directly.
2. Solve one sliding-window problem (max sum of a k-size subarray) and one two-pointer problem (remove duplicates from a sorted array in-place) without looking up a solution.

### Answer This
- Why is repeated `String` concatenation in a loop O(n²), concretely?
- What's the actual growth strategy that makes dynamic array resizing amortized O(1) instead of O(n) per call?

---

## 2.3 — Linked Lists

**Tier: 🎯 Master**

### What to Learn
- Singly (`next` only) vs doubly (`next` + `prev`) linked lists.
- When linked lists genuinely beat arrays: frequent insertion/deletion in the middle when you already hold a reference to the node — O(1) instead of array's O(n) shift.
- When arrays win: basically everything else, due to cache locality (1.2) — this is why in practice, arrays/`ArrayList` are the default and linked lists are a niche tool, not "the next data structure after arrays."
- **Fast & slow pointers (Floyd's cycle detection)** — a slow pointer advances one node per step, a fast one advances two; if there's a cycle, they *will* eventually meet.
- Reversing a linked list in-place; finding the middle node in a single pass using fast/slow pointers.

### Build From Scratch
A singly linked list with `addFirst`, `addLast`, `removeFirst`, `get`, `contains`, `reverse`, and a cycle-detection method using Floyd's algorithm.

### Answer This
- In what specific scenario does a linked list actually outperform an array in practice, given equal Big-O?
- How does Floyd's cycle detection guarantee the two pointers will meet if — and only if — a cycle exists?

---

## 2.4 — Stacks & Queues

**Tier: 🎯 Master**

### What to Learn
- Stack (LIFO): `push`/`pop`/`peek`. Queue (FIFO): `enqueue`/`dequeue`.
- In Java, use **`ArrayDeque`** for both — it's backed by a resizable array and outperforms the legacy `Stack` class (which is needlessly synchronized) and `LinkedList` (worse cache locality) for this purpose.
- Real uses: the call stack itself (recursion — see 1.3), undo/redo, BFS traversal (queue), task queues, browser history (stack-like).
- **Monotonic stack** — a stack kept in strictly increasing or decreasing order by discarding elements that violate the order as you go; the classic tool for "next greater/smaller element" problems in O(n) instead of the naive O(n²).

### Build From Scratch
A stack backed by a dynamic array; a queue implemented using two stacks (a classic exercise that forces you to actually understand amortized analysis from 2.1).

### Answer This
- Why is `ArrayDeque` the right default in Java for both stack and queue use cases, over `Stack` and `LinkedList`?
- How does a monotonic stack solve "next greater element" in O(n) when the naive approach is O(n²)?

---

## 2.5 — Hash Maps & Hash Sets

**Tier: 🎯 Master**

### What to Learn
- `hashCode()` compresses a key into an index, mapped into a bucket.
- **Collision handling** — separate chaining (each bucket holds a small linked list/tree of entries) vs open addressing (probe for the next empty slot — linear or quadratic probing).
- **Load factor** — the ratio of stored elements to bucket count; once it crosses a threshold (commonly 0.75), the table resizes and rehashes everything.
- Why HashMap lookup is O(1) *average* but O(n) *worst case* — if every key collides into the same bucket, you've degraded to a linked-list scan.
- Java 8+ **treeification** — once a single bucket's chain exceeds 8 entries, Java converts that bucket from a linked list to a red-black tree, capping worst-case lookup at O(log n) instead of O(n) even under pathological hashing.
- `HashMap` (no order guarantee) vs `LinkedHashMap` (insertion order preserved) vs `TreeMap` (sorted by key, backed by a red-black tree, O(log n) operations).
- `ConcurrentHashMap` — thread-safe without locking the entire map (segment-based locking pre-Java 8, CAS-based finer-grained approach in Java 8+).

### Build From Scratch
A HashMap using separate chaining: `put`, `get`, `remove`, and automatic `resize` when the load factor threshold is crossed.

### Answer This
- What specifically makes HashMap lookup degrade from O(1) toward O(n), and how does Java 8's treeification cap that degradation?
- Why does a resize require rehashing every existing entry, not just inserting new ones into a bigger table?

---

## 2.6 — Trees

**Tier: 🎯 Master**

### What to Learn
- Binary tree — each node has at most two children. BST property — left subtree < node < right subtree, which is what enables O(log n) search *when the tree is balanced*.
- Traversals — **inorder** (left, node, right — produces sorted output for a BST), **preorder** (node, left, right — useful for copying/serializing a tree), **postorder** (left, right, node — useful for safely deleting a tree bottom-up), **BFS/level-order** (queue-based, level by level).
- Why self-balancing matters — an unbalanced BST (e.g., built by inserting already-sorted data) degenerates into a linked list, turning O(log n) operations into O(n); **AVL trees** and **Red-Black trees** rebalance automatically on insert/delete to guarantee O(log n).
- **B-trees** — why databases and filesystems use them instead of binary trees: a B-tree node holds many keys/children (not just 2), matching a disk page's size, minimizing the number of disk reads (each of which is enormously expensive relative to memory — see 1.2) needed to find a value.

### Build From Scratch
A BST with `insert`, `search`, `delete`, all four traversals (both recursively and iteratively using an explicit stack/queue), and a "validate BST" check.

### Answer This
- Why does inserting already-sorted data into a naive BST turn every operation into O(n)?
- Why do databases use B-trees (wide, shallow) instead of binary trees (narrow, deep) for on-disk indexes specifically?

---

## 2.7 — Heaps

**Tier: 👀 Know Only**

### What to Learn
- A binary heap is a complete binary tree stored as a flat array (child/parent indices computed arithmetically, no pointers needed).
- Max-heap (parent ≥ children) vs min-heap (parent ≤ children).
- `siftUp` (bubble a newly-inserted element up until heap order is restored) and `siftDown` (bubble the root down after removing it) — both O(log n).
- Building a heap from an already-populated array is O(n) overall, not O(n log n) — a subtlety worth understanding (it comes from the math of how many elements are near the bottom vs the top).
- Priority queue use cases — Dijkstra's algorithm (2.12), task scheduling, "K-th largest element" problems. Java's `PriorityQueue` is a min-heap by default; pass a custom `Comparator` for max-heap behavior.
- **Why heapsort isn't used in practice** despite being O(n log n) worst-case guaranteed — its access pattern jumps around the array (poor cache locality, see 1.2), whereas quicksort's partitioning is far more cache-friendly, so quicksort wins in practice despite a worse theoretical worst case.

### Build From Scratch
A min-heap with `insert`, `extractMin`, `peek`, and a `heapify` (build-from-array) method.

### Answer This
- Why is building a heap from an array O(n) instead of the more intuitive-seeming O(n log n)?
- Why does quicksort typically outperform heapsort in practice despite a worse worst-case complexity?

---

## 2.8 — Graphs

**Tier: 👀 Know Only**

### What to Learn
- Directed vs undirected; weighted vs unweighted.
- **Adjacency list** (map of node → list of neighbors — good for sparse graphs, most real-world graphs) vs **adjacency matrix** (2D array — O(1) edge-existence lookup, but O(V²) space, good only for dense graphs).
- **BFS** — queue-based, explores level by level; the only correct way to find shortest path in an *unweighted* graph.
- **DFS** — stack-based (or recursive), explores as deep as possible before backtracking; used for cycle detection and topological sort.
- **Topological sort** — an ordering of nodes such that every edge points forward — only possible on a DAG (Directed Acyclic Graph); real use case: resolving build/dependency order.
- **Cycle detection** — using three-color/three-state marking during DFS (white = unvisited, gray = in current recursion stack, black = fully processed) — a gray node reached again means a cycle.
- **Connected components / Union-Find** — grouping nodes that are reachable from each other (see 2.10).

### Build From Scratch
A graph via adjacency list supporting `addEdge`, BFS, DFS, cycle detection, and topological sort.

### Answer This
- Why does BFS (not DFS) guarantee shortest path in an unweighted graph?
- Why is topological sort only well-defined on a DAG, and what does its existence tell you about the graph's cycle-freedom?
- When would you actually choose an adjacency matrix over an adjacency list, despite the worse space complexity?

---

## 2.9 — Tries

**Tier: ⏸️ Skip For Now**

### What to Learn
- A prefix tree — each edge represents one character, and a path from the root spells out a prefix (or full word, marked with an `isEndOfWord` flag).
- Insert, search, and prefix-search are all O(m), where m is the length of the word/prefix — independent of how many words are stored, which is the key advantage over scanning a list of strings.
- Real use cases — autocomplete, spell checkers, IP routing tables (longest-prefix matching).
- Tradeoff — higher memory overhead per character than a hash set of strings, but wins decisively whenever prefix-based queries matter.

### Build From Scratch
A trie with `insert`, `search`, `startsWith`, and `getAllWordsWithPrefix`.

### Answer This
- Why is a trie's prefix search O(m) regardless of how many total words are stored, while a hash-set-based approach to "all words starting with X" would require scanning every entry?

---

## 2.10 — Union-Find (Disjoint Set Union)

**Tier: ⏸️ Skip For Now**

### What to Learn
- Tracks a partition of elements into disjoint sets, supporting `union` (merge two sets) and `find` (which set does this element belong to) efficiently.
- **Path compression** — during `find`, flatten the tree so future lookups are near-instant.
- **Union by rank/size** — always attach the smaller tree under the bigger one's root, keeping trees shallow.
- Together, these two optimizations bring both operations to near-O(1) amortized (technically O(α(n)), the inverse Ackermann function — for all practical n, this is a small constant).
- Real use cases — connected components in a graph, Kruskal's Minimum Spanning Tree algorithm, cycle detection in an undirected graph.

### Answer This
- Why does combining path compression *and* union by rank/size get you to near-constant time, when either alone is only a partial improvement?

---

## 2.11 — Sorting Algorithms

**Tier: 🎯 Master**

### What to Learn
- Bubble, Selection, Insertion — all O(n²); understand *why* they're slow (each does redundant comparisons/swaps), not just that they are. Insertion sort is genuinely good for small or nearly-sorted inputs.
- **Merge Sort** — O(n log n) always (no worst-case degradation), stable (equal elements keep their relative order), needs O(n) extra space; classic divide-and-conquer.
- **Quick Sort** — O(n log n) average, O(n²) worst case (e.g., already-sorted input with a naive pivot choice), in-place; the partition strategy is what determines real-world performance.
- Why **O(n log n)** is the proven lower bound for *comparison-based* sorting (an information-theory argument: you need at least log₂(n!) comparisons to distinguish all possible orderings).
- Non-comparison sorts — **Counting sort** (O(n+k), when the value range k is small and known) and **Radix sort** (sorts by digit/character position) — these beat the O(n log n) bound because they don't rely on pairwise comparisons at all.
- Java's `Arrays.sort()` — dual-pivot quicksort for primitive arrays, TimSort (a hybrid of merge sort and insertion sort, exploiting existing runs of order) for object arrays — know that Java deliberately uses *different* algorithms for primitives vs objects, and roughly why (stability matters for objects with custom `equals`, doesn't matter for primitives).

### Build From Scratch
Merge sort and quicksort, from memory, with no reference material.

### Answer This
- Why is merge sort's worst case guaranteed O(n log n) while quicksort's isn't, despite both averaging O(n log n)?
- Why can counting sort and radix sort beat the O(n log n) comparison-sort lower bound?
- Why does Java use a different sorting algorithm for `int[]` than for `Integer[]`?

---

## 2.12 — Searching & Shortest Paths

**Tier: 🎯 Master**

### What to Learn
- **Binary search** — O(log n) on sorted (or otherwise monotonic) data; the standard template uses `mid = left + (right - left) / 2` specifically to avoid integer overflow that `(left + right) / 2` risks on large inputs; off-by-one errors between `left <= right` and `left < right` are the most common bug source here.
- Binary search generalizes beyond "find a value in a sorted array" to **binary search on the answer space** — e.g., "find the minimum capacity such that X is achievable" — recognizing when a problem is monotonic enough for this is a genuinely valuable skill.
- **BFS** for shortest path in unweighted graphs (2.8).
- **Dijkstra's algorithm** — shortest path in a weighted graph with non-negative edges, using a priority queue (2.7) to always expand the currently-cheapest frontier node; it fails silently (produces wrong answers) in the presence of negative edge weights.
- **Bellman-Ford** — handles negative weights correctly (and can detect negative cycles), at the cost of being slower than Dijkstra.
- **A\*** — Dijkstra plus a heuristic estimate of remaining distance to the goal, letting it prioritize promising paths first — used heavily in pathfinding (games, maps).

### Practice
1. Implement binary search with both `left <= right` and `left < right` loop conditions on the same problem and deliberately introduce/fix off-by-one bugs until you can explain exactly why each variant needs its own careful boundary handling.
2. Implement Dijkstra's algorithm using Java's `PriorityQueue`, then feed it a graph with a negative edge weight and observe it produce a wrong answer — understand precisely why the greedy assumption breaks.

### Answer This
- Why does `mid = left + (right - left) / 2` avoid a bug that `(left + right) / 2` doesn't?
- Why does Dijkstra's algorithm break with negative edge weights specifically, in terms of its greedy assumption?
- What does A* add on top of Dijkstra, and why does that make it faster in practice for point-to-point pathfinding?

---

## 2.13 — Dynamic Programming

**Tier: 👀 Know Only**

### What to Learn
- Recognize **optimal substructure** (the optimal solution to the whole problem is built from optimal solutions to subproblems) and **overlapping subproblems** (the same subproblem gets solved repeatedly in a naive recursive approach) — these two properties together are the actual signal that DP applies.
- **Memoization (top-down)** — write the natural recursive solution, then cache results so each unique subproblem is computed once.
- **Tabulation (bottom-up)** — build a table of solutions starting from the base cases upward, avoiding recursion overhead entirely.
- 1D DP examples — Fibonacci, climbing stairs, house robber.
- 2D DP examples — longest common subsequence, edit distance, 0/1 knapsack.
- The genuinely hard and valuable skill is **deriving the recurrence relation** — expressing the solution to a problem of size n in terms of solutions to smaller sub-problems — not memorizing specific problems.

### Build From Scratch
Fibonacci with and without memoization (measure and compare the performance gap directly — this should feel dramatic, not subtle); coin change (minimum coins for a target); longest increasing subsequence; 0/1 knapsack.

### Answer This
- What two properties, together, actually signal that a problem is a good DP candidate?
- Why does memoized recursive Fibonacci run in O(n) time when the naive version is O(2ⁿ)?
- What's the actual mechanical difference between top-down memoization and bottom-up tabulation, beyond "one uses recursion"?

---

## 2.14 — Advanced & Niche DS&A

**Tier: ⏸️ Skip For Now**

> Awareness-to-working level. You won't use these daily, but you should recognize the problem shape that calls for them.

- **Segment Tree / Fenwick Tree (Binary Indexed Tree)** — support range queries (sum/min/max over a range) and point updates in O(log n), where a naive approach would be O(n) per query. Used in competitive programming and some real analytics/metrics systems.
- **String matching** — **KMP** (avoids re-scanning the text on a partial match by precomputing a failure function), **Rabin-Karp** (rolling hash comparison), **suffix arrays** (support many string queries efficiently) — this is what powers real regex engines and text search under the hood.
- **Computational geometry** — convex hull, line sweep, closest-pair-of-points algorithms — relevant in game development, GIS, and computer graphics.

### Answer This
- What does a Fenwick Tree let you do in O(log n) that a plain prefix-sum array can't (hint: think about updates, not just queries)?
- Why is KMP string matching faster than the naive approach on inputs with partial matches?

---

## 2.15 — Problem-Solving Patterns Reference

**Tier: 🎯 Master**

| Pattern | Recognize It When | Classic Example |
|---|---|---|
| Sliding Window | Subarray/substring with a size or sum constraint | Max sum of k-size window |
| Two Pointers | Sorted input, or searching from both ends | Remove duplicates in-place |
| Fast & Slow Pointers | Cycle or midpoint in a linked structure | Linked list cycle detection |
| Binary Search | Sorted space, or a monotonic condition on the answer | First bad version |
| BFS | Shortest path, level-by-level exploration | Shortest path in a maze |
| DFS | Explore all paths, backtrack | All permutations |
| Backtracking | Generate all valid combinations under constraints | N-Queens, Sudoku solver |
| Dynamic Programming | Overlapping subproblems, optimize a value | Knapsack, LCS |
| Divide & Conquer | Problem splits into independent halves | Merge sort, closest pair |
| Monotonic Stack | Next greater/smaller element | Daily temperatures |
| Union-Find | Connected components, grouping | Number of islands |
| Topological Sort | Dependency ordering | Course schedule |

---

## Phase 2 Capstone

> **PROJECT:** Implement — entirely from scratch, no built-in Java collections — a dynamic array, singly linked list, stack, queue, hash map (separate chaining), binary search tree, min-heap, and graph (adjacency list). Implement BFS, DFS, merge sort, and quicksort from memory. Then solve 8–10 problems, one from each row of the pattern table in 2.15, and for each one write a single sentence identifying *which pattern* applies and *why* before writing any code.

---

# Phase 3 — Software Design & Clean Code

**Phase tier: 🎯 Master — all of it, this is core craft, not over-engineering**

> The true cost of software isn't writing it — it's maintaining it. This phase is about controlling that cost.

## 3.1 — Clean Code Principles

**Tier: 🎯 Master**

### What to Learn
- **Meaningful naming** — a name should make a comment unnecessary; `validateAndEnrichUserOrder()` tells you what happens, `processData()` tells you nothing.
- **Functions do one thing** — if you can't describe a function's purpose in one sentence without "and," it's doing too much.
- **Short functions** — under ~20 lines is a good heuristic, not a hard rule; the real test is whether it fits in your head at once.
- **No magic numbers** — `private static final int MAX_RETRY_ATTEMPTS = 3` instead of a bare `3` scattered through the code — the name documents *why* the number exists.
- **Avoid deep nesting** — guard clauses and early returns flatten a function's structure and make the "happy path" visually obvious.
- **Comments explain WHY, not WHAT** — if a comment just restates what the next line of code obviously does, delete it; keep comments for surprising constraints, workarounds, or non-obvious decisions.
- **DRY** (Don't Repeat Yourself) — every piece of knowledge should have one authoritative representation in the system; the risk of *not* following this is that two copies drift apart and only one gets fixed.
- **KISS** (Keep It Simple) — the simplest solution that actually satisfies the requirement is usually correct; complexity should be justified, not defaulted to.
- **YAGNI** (You Aren't Gonna Need It) — don't build abstraction or configurability for a future requirement that may never arrive; it's easier to add generality later than to remove premature generality now.

### Practice
1. Take a real function you wrote more than a month ago. Cover the name. Read only the body. Guess what it does. Compare your guess to what it's actually named — if they don't match, that's a naming problem, not a "you forgot" problem.
2. Refactor one function that's doing 3+ things into 3+ functions, each describable in one sentence.
3. Find and eliminate one piece of duplicated logic in a real project by extracting it to a single shared location.

### Answer This
- Why is a good name considered "documentation," not just style?
- What's the actual cost of violating DRY, beyond "it's more typing"?
- Why is YAGNI not the same as "never think ahead" — where's the line?

---

## 3.2 — SOLID Principles

**Tier: 🎯 Master**

### What to Learn (with concrete bad/good pairs — don't memorize the acronym, learn to spot violations)

- **Single Responsibility** — a class should have one reason to change.
  *Bad:* a `UserService` that validates input, saves to the database, sends a welcome email, and generates a usage report — four unrelated reasons to change one class.
  *Good:* separate `UserValidator`, `UserRepository`, `EmailService`, `ReportGenerator`, each independently testable and changeable.

- **Open/Closed** — open for extension, closed for modification.
  *Bad:* a giant `if/else` or `switch` on a "type" field that you must edit every time a new type is added.
  *Good:* polymorphism or the Strategy pattern (3.3) — adding a new type means adding a new class, not editing existing ones.

- **Liskov Substitution** — a subclass must be usable anywhere its parent is expected, without breaking correctness.
  *Bad:* `Square extends Rectangle`, overriding `setWidth`/`setHeight` to keep both sides equal — this silently breaks any code that assumes setting width alone doesn't affect height.
  *Good:* don't force an inheritance relationship where the "is-a" claim doesn't actually hold behaviorally.

- **Interface Segregation** — don't force an implementer to provide methods it doesn't need.
  *Bad:* one fat `Worker` interface with `work()` and `eat()`, forced onto a `RobotWorker` that has no meaningful `eat()` implementation.
  *Good:* split into smaller, focused interfaces (`Workable`, `Eatable`) and implement only what applies.

- **Dependency Inversion** — depend on abstractions, not concrete implementations.
  *Bad:* `UserService` directly instantiates `new PostgresUserRepository()` inside its constructor.
  *Good:* `UserService` depends on a `UserRepository` interface, and the concrete implementation is injected — this is *why* dependency injection (Phase 5 of the master roadmap) exists as a pattern, not just a Spring feature.

> Don't memorize the definitions — the actual skill is **spotting violations in real code you didn't write**, including your own from six months ago.

### Practice
1. Find one real Single Responsibility violation in an existing codebase (yours or open source) and split it.
2. Find one real Open/Closed violation (a growing `switch`/`if-else` chain on a type) and refactor it to polymorphism or Strategy.
3. Write down, from a real class you use often (in your language's standard library or a library you depend on), which SOLID principles it seems to honor and which it doesn't — and whether that seems like a deliberate tradeoff.

### Answer This
- Why is "one reason to change" a better test for Single Responsibility than "does one thing" (which can be ambiguous)?
- Concretely, why does `Square extends Rectangle` violate Liskov Substitution — what real code breaks, and how?
- Why does Dependency Inversion make unit testing dramatically easier?

---

## 3.3 — Design Patterns

**Tier: 🎯 Master**

### Creational Patterns
- **Singleton** — exactly one instance for the application's lifetime; implemented via a private constructor + static instance + `getInstance()`; thread-safe versions use `synchronized`, double-checked locking, or (the cleanest in Java) an `enum` singleton. **Why it's often abused:** it's frequently just global mutable state wearing a design-pattern costume, which reintroduces every problem OOP encapsulation was meant to solve.
- **Factory Method** — create objects through a method instead of `new`, when the concrete type to instantiate depends on runtime information (e.g., `NotificationFactory.create("email")` returns an `EmailNotification`).
- **Builder** — construct a complex object step by step; reach for this when a constructor would otherwise need 4+ parameters (especially several of the same type, where positional arguments become error-prone).

### Structural Patterns
- **Adapter** — wraps an incompatible interface (often third-party) to match the interface your code expects, isolating the rest of your codebase from that third party's quirks.
- **Decorator** — adds behavior to an object dynamically without subclassing it — e.g., `LoggingUserRepository` wraps a real `UserRepository` and logs every call, transparently to callers.
- **Proxy** — controls access to an object; this is literally what Spring AOP (`@Transactional`, `@Cached`, etc.) is built on under the hood — a proxy object intercepts calls to add behavior before/after delegating to the real object.
- **Facade** — simplifies a complex subsystem behind one clean interface, e.g., an `OrderFacade` that internally coordinates `InventoryService`, `PaymentService`, and `ShippingService` so callers don't need to know about any of them individually.

### Behavioral Patterns
- **Observer / Pub-Sub** — one-to-many notification when state changes, fully decoupling the publisher from its subscribers (message queues and React state updates both rely on this idea).
- **Strategy** — encapsulate interchangeable algorithms behind a common interface, selected at runtime (payment processor selection, tax calculation by region).
- **Command** — encapsulate a request as an object, enabling undo/redo, queuing, and logging of actions uniformly.
- **Repository** — abstracts data access so the domain/business layer never directly touches SQL or an ORM API.
- **Circuit Breaker** — CLOSED (calls flow normally) → OPEN (after N consecutive failures, stop calling and fail fast) → HALF_OPEN (after a cooldown, cautiously probe with one call) → back to CLOSED or OPEN depending on the result. Prevents a struggling downstream service from being hammered further and prevents your own service from wasting resources waiting on doomed calls.

### Distributed System Patterns (relevant once you reach Phase 10 — System Design)
- **Saga** — manage a multi-step distributed transaction via a sequence of local transactions, each with a compensating action if a later step fails (Order → Payment → Shipping; if Shipping fails, compensate by refunding Payment and cancelling the Order).
- **Outbox Pattern** — write your business data change and an "event to publish" row into the same local database transaction, then have a separate worker relay outbox rows to the message broker — this avoids the "dual write" problem where a DB write succeeds but the corresponding message publish fails (or vice versa).
- **Event Sourcing** — persist a log of events (what happened), not the current state directly; current state is derived by replaying events — enables full audit trails and "what did this look like at time T" queries.
- **CQRS** — separate the write model (optimized for consistency and business rules) from the read model (optimized for query performance), often paired with Event Sourcing.

### Anti-Patterns to Recognize
God object (one class that knows/does everything), Singleton abuse (global mutable state), premature abstraction (an interface with exactly one implementation, added "just in case"), cargo cult programming (using a pattern because it's a pattern, without the problem it solves actually being present).

### Practice
> **PROJECT:** Implement 5 patterns as small, real, working mini-projects (not toy examples copied from a tutorial):
> 1. **Observer** — a small event emitter from scratch (`on`/`emit`), no library.
> 2. **Repository** — a data-access abstraction layer with at least two swappable implementations (e.g., in-memory and a real database).
> 3. **Strategy** — a payment system supporting multiple providers, selected at runtime.
> 4. **Factory** — a notification system that creates email/SMS/push senders from a config string.
> 5. **Circuit Breaker** — wrap a deliberately-flaky external API call (simulate random failures) with resilience4j and observe the OPEN/HALF_OPEN/CLOSED transitions happen live.

### Answer This
- Why is Singleton considered risky when it's essentially "just" global state with extra ceremony?
- What specifically does the Outbox pattern prevent that a naive "write to DB, then publish an event" sequence doesn't?
- Why does a Circuit Breaker's HALF_OPEN state exist — what would go wrong if it jumped straight from OPEN back to CLOSED?

---

## 3.4 — Architecture Patterns

**Tier: 🎯 Master**

### What to Learn
- **Layered Architecture** — Presentation → Business Logic → Data Access, each layer only talking to the one below; the common failure mode is business logic leaking into controllers or repositories because there's no structural enforcement preventing it.
- **Clean Architecture** — dependencies always point inward; core business rules know nothing about frameworks, databases, or HTTP — they're defined purely in terms of your domain, with outer layers (infrastructure, presentation) implementing interfaces the core defines.
- **Domain-Driven Design (DDD) — the basics** — Ubiquitous Language (the same vocabulary in code and business conversation); Entities (objects with identity that persists across state changes, e.g., a `User` that can change its email but is still "the same" user); Value Objects (defined purely by their attributes, no identity — e.g., `Money`, `Address` — two `Money(100, USD)` instances are simply equal, there's no "which one"); Aggregates (a cluster of entities/value objects with one root entity controlling all access and enforcing invariants — you never mutate an `OrderLine` directly, you ask the `Order` aggregate root to do it); Domain Events (`OrderPlaced`, `PaymentFailed` — past tense, immutable, decoupled). *(The full deep dive — Bounded Contexts, Context Maps, tactical patterns like Domain Services, and Event Storming — lives in Phase 17 of the master roadmap, once you're ready for the organizational/strategic side of DDD.)*
- **Hexagonal Architecture (Ports & Adapters)** — "ports" are interfaces defining how the outside world talks to your core logic; "adapters" are concrete implementations plugging specific technology into those ports (`UserRepository` is a port, `JpaUserRepository` is an adapter) — this is really the same idea as Clean Architecture and Dependency Inversion (3.2), viewed from a different angle.
- **MVC** — separation of concerns across Model/View/Controller; the practically important thing to internalize is that business logic frequently does *not* belong in the "Model" as commonly implemented (it often ends up as thin data classes) — and that MVC as a pattern starts to strain in SPA/micro-frontend architectures where "the view" isn't a single server-rendered thing anymore.

**Spring Boot Clean Architecture project layout** (you'll use this directly in Phase 5):
```
src/main/java/com/company/app/
├── domain/          ← Entities, domain services, repository interfaces
├── application/     ← Use cases, application services, DTOs
├── infrastructure/  ← Repository implementations, external integrations
└── presentation/    ← Controllers, request/response models
```

### Practice
1. Take an existing small project structured as one flat package and reorganize it into `domain`/`application`/`infrastructure`/`presentation`, moving each class based on what it actually does, not its current name.
2. Identify one Value Object hiding in your code as a primitive (e.g., a `String email` field that's validated in five different places) and promote it to a real `Email` value object with validation built into its constructor.

### Answer This
- What does "dependencies always point inward" concretely forbid your domain layer from doing?
- Why is a `Money` amount a Value Object rather than an Entity — what would break if you gave it an identity?
- Concretely, what's the difference between a Port and an Adapter, using `UserRepository`/`JpaUserRepository` as the example?

---

## 3.5 — Refactoring

**Tier: 🎯 Master**

### What to Learn
- **What refactoring is** — changing internal structure without changing external behavior; if behavior changes, it's not a refactor, it's a feature change or a bug fix wearing a refactor's clothes.
- **Code smells** — long method (doing too much), large class (too many responsibilities), feature envy (a method more interested in another class's data than its own — a signal it belongs on that other class), data clumps (the same group of variables always traveling together — promote them to a class), primitive obsession (using a bare `String`/`int` where a small dedicated type would prevent misuse — see the `Email` example in 3.4), switch statements on a type field (often a sign polymorphism should replace it — see Open/Closed in 3.2), god object.
- **Core techniques** — extract method (pull a block of code into its own named function), extract class (split an overloaded class into two cohesive ones), rename (names should reveal intent — see 3.1), move method/field (put behavior next to the data it actually operates on), replace conditional with polymorphism.
- **The Boy Scout Rule** — leave the code a little cleaner than you found it, every time you touch it — this is how large codebases improve continuously instead of only during rare, risky "big rewrite" efforts.
- **Refactoring safely** — write or confirm tests exist *first*, then change structure, keeping tests green throughout; refactoring without a safety net is just risky rewriting with extra steps.

### Practice
1. Identify one "long method" in a real codebase (yours or open source) and apply Extract Method until each piece is independently understandable.
2. Find one instance of primitive obsession (a bare `String`/`int` standing in for a real domain concept) and promote it to a small dedicated type.
3. Practice the discipline explicitly: before refactoring anything in a real project, confirm test coverage exists for the behavior you're about to touch; if it doesn't, write it first.

### Answer This
- Why does "refactoring" specifically exclude any change in observable behavior — what's the actual risk if that line gets blurred?
- Why is "feature envy" a signal that a method is on the wrong class?
- Why is writing tests *before* refactoring non-negotiable, rather than just a nice-to-have?

---

## Phase 3 Capstone

> **PROJECT:** Take one real, moderately-sized existing codebase (yours, or a small open-source project). Over several passes: (1) identify and fix 3 SOLID violations, explaining each one in a sentence before changing it; (2) implement the 5 design patterns from the 3.3 capstone as real, working pieces of this project rather than isolated toy examples, where they genuinely fit; (3) reorganize the project into a Clean Architecture layout (`domain`/`application`/`infrastructure`/`presentation`); (4) find and fix 3 real code smells using the refactoring techniques from 3.5, with tests in place *before* each refactor. Write a short before/after note for each change explaining what specifically improved and why.
