# Part III — Data (Detailed)
### Companion deep-dive to `Master_Senior_Fullstack_Roadmap.md` → Phases 8, 9

> This file expands **Phase 8 (Databases)** and **Phase 9 (Data Engineering — Awareness)** into full depth.
>
> **Why this part is disproportionately important:** most production incidents that look like "the app is broken" are actually "the database is doing something the developer didn't expect." A slow endpoint is almost always a query problem, not a code problem. A data-corruption bug is almost always a transaction-isolation problem, not a logic bug. Phase 8 is where Part I's fundamentals (Phase 1.2's memory hierarchy, Phase 2's trees/hashing) stop being abstract and become the literal explanation for why an index works the way it does. Phase 9 exists so that when a data team asks you a question about your schema or your event emission, you understand what they actually need and why — not because you're becoming a data engineer.
>
> Go in order within Phase 8 — indexing (8.2) depends on understanding storage (8.1), and query optimization (8.4) depends on understanding indexing. Phase 9 is lighter — it's awareness, not mastery — so its "Practice" sections are lighter too, deliberately.

---

## Table of Contents

**Phase 8 — Databases**
- 🎯 [8.1 — How Databases Actually Work](#81--how-databases-actually-work)
- 🎯 [8.2 — Indexing (The Most Important Topic)](#82--indexing-the-most-important-topic)
- 🎯 [8.3 — ACID & Transactions](#83--acid--transactions)
- 🎯 [8.4 — Query Optimization](#84--query-optimization)
- 🎯 [8.5 — PostgreSQL in Depth](#85--postgresql-in-depth)
- 🎯 [8.6 — Redis Beyond Caching](#86--redis-beyond-caching)
- 👀 [8.7 — Scaling Databases](#87--scaling-databases)
- 👀 [8.8 — The Broader Database Landscape](#88--the-broader-database-landscape)
- [Phase 8 Capstone](#phase-8-capstone)

**Phase 9 — Data Engineering (Awareness)**
- 👀 [9.1 — ETL vs ELT](#91--etl-vs-elt)
- 👀 [9.2 — Batch & Stream Processing](#92--batch--stream-processing)
- 👀 [9.3 — Orchestration & Data Quality](#93--orchestration--data-quality)
- 👀 [9.4 — Data Lakes, Warehouses & Modeling](#94--data-lakes-warehouses--modeling)
- 👀 [9.5 — Product Analytics & Data Contracts](#95--product-analytics--data-contracts)
- [Phase 9 Capstone](#phase-9-capstone)

---

# Phase 8 — Databases

**Phase tier: 🎯 Mostly Master — core PostgreSQL/Redis Master; scaling and the broader NoSQL landscape Know Only**

> Most production bugs and outages trace back to the database. Understanding internals is what separates an engineer who debugs a slow endpoint in ten minutes from one who spends a full day guessing.

## 8.1 — How Databases Actually Work

**Tier: 🎯 Master**

### What to Learn
- **Storage on disk** — data lives in fixed-size **pages** (commonly 8KB in PostgreSQL), each holding multiple rows; this directly connects to Phase 1.2's memory hierarchy — a page is the unit the database moves between disk and its in-memory buffer pool, chosen specifically to amortize the enormous cost gap between disk and RAM access.
- **Query lifecycle** — SQL text → **parser** (turns text into an internal representation, closely related to the AST concept from Phase 1.6) → **query planner/optimizer** (generates multiple possible execution strategies and estimates the cost of each using statistics about your actual data) → **executor** (runs the chosen plan).
- **Cost-based optimization** — the planner doesn't run your query one obvious way; it considers several candidate plans (different join orders, different index usage) and picks the one it estimates will be cheapest, based on table statistics it maintains (row counts, value distributions) — which is *why* stale statistics (e.g., right after a huge bulk load) can cause the planner to pick a genuinely bad plan until statistics are refreshed.
- **Row-oriented vs column-oriented storage** — row storage (PostgreSQL, MySQL) keeps a full row together on disk, which is efficient for typical transactional workloads that read/write whole records; column storage (used by analytical databases, see 9.4) keeps each column together, which is dramatically faster for "aggregate one column across millions of rows" queries but worse for "give me this one full record" queries — this is a foundational tradeoff, not just an implementation detail.
- **WAL (Write-Ahead Log)** — every change is written to an append-only log *before* it's applied to the actual data pages; if the database crashes mid-write, it can replay the WAL on restart to recover to a consistent state — this is the actual mechanism that makes "Durability" (8.3) a real guarantee rather than a promise.
- **Buffer pool** — the database's own in-memory cache of frequently-used pages, existing specifically to avoid the enormous latency cost of a disk read (Phase 1.2) on every single query; a "cold" database (freshly restarted, empty buffer pool) is measurably slower until commonly-used pages get cached again.
- **MVCC (Multi-Version Concurrency Control)** — instead of readers blocking writers (or vice versa), each transaction sees a consistent **snapshot** of the data as of when it started; this is *why* a long-running read query in PostgreSQL doesn't get blocked by a concurrent write, and also *why* old row versions need to be cleaned up afterward (see `VACUUM`, below).
- **`VACUUM`** — reclaims space from "dead" row versions left behind by updates/deletes under MVCC (an UPDATE in Postgres, under MVCC, doesn't overwrite a row in place — it writes a new version and marks the old one dead); `autovacuum` handles this automatically in the background, but an under-tuned autovacuum on a write-heavy table can let dead rows ("bloat") accumulate faster than they're reclaimed, degrading performance over time in a way that looks mysterious if you don't know MVCC exists.

### Practice
1. Run `EXPLAIN` (not `ANALYZE` yet — that's 8.4) on a simple query and identify the plan's shape before you've learned to fully read it — just notice that the planner chose *a* specific strategy, not "the only possible" one.
2. Load a large amount of data into a table in one bulk operation without ever running `ANALYZE`, then run a query that should clearly benefit from an index and observe the planner potentially making a poor choice due to stale statistics; run `ANALYZE` manually and re-check the plan.
3. Open two `psql` sessions. In session A, start a transaction and read a row. In session B, update and commit a change to that same row. Confirm session A still sees the old value until it starts a *new* transaction — this is MVCC's snapshot isolation made directly visible.
4. On a table you've done many updates/deletes against, check `pg_stat_user_tables` for dead tuple count, then run `VACUUM` manually and observe the number drop.

### Answer This
- Why does a page-based storage model exist at all, rather than the database just reading/writing individual rows directly?
- What specifically does the WAL guarantee, and why does that guarantee break down if you disable it (or ignore `fsync`, from Phase 1.3)?
- Why doesn't a concurrent write block an already-running read transaction under MVCC — what is the reader actually looking at?
- Why can stale table statistics cause the query planner to choose a genuinely bad plan?

---

## 8.2 — Indexing (The Most Important Topic)

**Tier: 🎯 Master**

### What to Learn
- **What an index actually is** — a separate, additional data structure (not a copy of your table) that trades write cost and storage space for dramatically faster reads on the columns it covers.
- **B-Tree indexes (the default)** — a direct, practical application of the balanced-tree concept from Phase 2.6; supports both equality (`WHERE id = ?`) and range queries (`WHERE created_at > ?`) in O(log n).
- **Hash indexes** — O(1) exact-match lookup (Phase 2.5's hash map concept, applied on disk), but structurally cannot support range queries at all — a narrower, more specialized tool than B-Tree.
- **GIN/GiST indexes** — GIN (Generalized Inverted Index) is built for values that contain multiple searchable "things" inside them — full-text search tokens, array elements, JSONB keys; GiST supports geometric/range types and nearest-neighbor-style queries.
- **Composite indexes** — an index on `(a, b)` is genuinely useful for `WHERE a = ?` alone *and* `WHERE a = ? AND b = ?`, but **not** for `WHERE b = ?` alone — column order in a composite index determines which query shapes it can actually serve, because a composite B-Tree index is fundamentally sorted by the first column first, then the second within each value of the first.
- **Covering indexes** — an index that includes every column a query needs lets Postgres answer the query via an **index-only scan**, never touching the actual table's heap pages at all — a meaningfully faster path than a regular index scan.
- **Partial indexes** — `CREATE INDEX ON orders (user_id) WHERE status = 'active'` indexes only the subset of rows matching the condition, keeping the index small and fast when queries only ever care about that subset.
- **When indexes hurt** — every additional index adds overhead to every `INSERT`/`UPDATE`/`DELETE` (the index has to be maintained too), and consumes storage; a table with 15 indexes "just in case" is a real, common anti-pattern, not a safety net.
- **Index selectivity** — an index on a low-cardinality column (e.g., a boolean, or a status field with only 3 possible values) is often nearly useless, because the planner may correctly decide a sequential scan is cheaper than jumping through an index that doesn't narrow the result set down much.
- **Reading `EXPLAIN ANALYZE`** — distinguishing `Seq Scan` (full table scan), `Index Scan` (uses an index, then fetches matching rows from the table), and `Bitmap Heap Scan` (builds a bitmap of matching pages from an index, then reads them — often chosen over a plain Index Scan when many rows match) — this needs to become a reflex, run on every non-trivial query you write, not something you reach for only when something already feels slow.

### Practice
1. **The canonical exercise:** create a PostgreSQL table with 1,000,000+ rows. Run a query filtering on an unindexed column and record the `EXPLAIN ANALYZE` output and timing (expect a `Seq Scan`). Add a B-Tree index on that column, re-run, and record the new plan and timing (expect an `Index Scan` or `Bitmap Heap Scan`). Do this again with a composite index matching a two-column `WHERE` clause. Do it again with a covering index and confirm you get an index-only scan. This single exercise, done honestly with real before/after numbers, teaches more than any amount of reading.
2. Create an index on a boolean or 2-3-value status column and confirm — by trying to force its use and comparing costs — that the planner often (correctly) ignores it in favor of a sequential scan, due to low selectivity.
3. Create a composite index on `(a, b)` and confirm via `EXPLAIN` that it's used for `WHERE a = ?` and `WHERE a = ? AND b = ?`, but *not* for `WHERE b = ?` alone.

### Answer This
- Why does a composite index on `(a, b)` not help a query filtering only on `b`?
- What makes an index-only scan faster than a regular index scan, mechanically?
- Why can adding an index to a low-cardinality column actually be pointless or even counterproductive?
- What's the real cost of "just add an index" as a default reflex, beyond storage?

---

## 8.3 — ACID & Transactions

**Tier: 🎯 Master**

### What to Learn
- **Atomicity** — a transaction's operations all succeed or all roll back together; there is no partial-completion state ever visible to the rest of the system.
- **Consistency** — every transaction moves the database from one valid state to another, respecting every constraint (foreign keys, unique constraints, check constraints) along the way.
- **Isolation** — concurrent transactions don't observe each other's uncommitted, in-progress work — but *how much* isolation you get is a genuine dial, not a fixed guarantee (see isolation levels below).
- **Durability** — once a transaction commits, it survives a crash — guaranteed specifically by the WAL (8.1) and `fsync` (Phase 1.3).

**Isolation levels — where most real concurrency bugs hide:**

| Level | Dirty Reads | Non-Repeatable Reads | Phantom Reads |
|---|---|---|---|
| Read Uncommitted | Possible | Possible | Possible |
| Read Committed (Postgres default) | Prevented | Possible | Possible |
| Repeatable Read | Prevented | Prevented | Possible |
| Serializable | Prevented | Prevented | Prevented |

- **Dirty read** — reading another transaction's uncommitted changes (which might still be rolled back).
- **Non-repeatable read** — running the exact same query twice within one transaction and getting different results, because another transaction committed a change in between.
- **Phantom read** — running the exact same `WHERE` clause twice within one transaction and getting a different *set of rows* (not just different values), because another transaction inserted or deleted rows matching the condition in between.
- Higher isolation levels prevent more anomalies but cost more (more locking, more chance of a transaction having to abort and retry under `SERIALIZABLE`) — choosing an isolation level is a genuine tradeoff decision per use case, not a "just use the strictest one always" default.

**Concurrency & locking**
- **Optimistic locking** — assume conflicts are rare; check for a conflict only at write time (commonly via a version number column) and reject/retry if the version has changed — good for low-contention scenarios.
- **Pessimistic locking** — lock the row the moment you read it with intent to modify it (`SELECT ... FOR UPDATE`), so no other transaction can touch it until you're done — good for high-contention scenarios where retries would be expensive or frequent.
- **Deadlocks** — two transactions each hold a lock the other is waiting for; PostgreSQL detects this automatically and kills one of the transactions to break the cycle (this is the exact graph-cycle-detection concept from Phase 2.8, applied to a real "waits-for" graph of transactions).
- **Advisory locks** — application-defined locks that PostgreSQL will track for you, useful for building your own distributed-lock-like coordination without inventing a new mechanism from scratch.

### Practice
1. Reproduce a **dirty read** at `READ UNCOMMITTED` (if your database/driver allows it — Postgres doesn't actually implement true Read Uncommitted, which is itself worth discovering) — or reproduce a **non-repeatable read** at `READ COMMITTED` by opening two sessions, having session A read a row twice with a commit from session B in between, and observing the value change within session A's single logical unit of work.
2. Reproduce a **phantom read**: at `REPEATABLE READ`, run a `WHERE` query, have another session insert a new matching row and commit, then re-run the exact same query in the first session and check whether the new row appears (behavior differs meaningfully between `REPEATABLE READ` and `SERIALIZABLE` here — confirm which anomalies your isolation level actually prevents versus which it doesn't, directly).
3. Force a deadlock on purpose: two sessions, each locks one row then tries to lock the other's row, in opposite order. Confirm Postgres detects it and kills one transaction with a deadlock error, rather than both sessions hanging forever.
4. Implement the same "prevent overselling stock" scenario two ways — once with optimistic locking (a version column, retry on conflict) and once with pessimistic locking (`SELECT ... FOR UPDATE`) — and write down which one you'd choose for a high-traffic checkout flow and why.

### Answer This
- What's the precise difference between a non-repeatable read and a phantom read?
- Why does Postgres need to actively detect and break a deadlock, rather than the deadlock simply resolving itself?
- When would optimistic locking be the wrong choice, and pessimistic locking the right one?
- Why is "just always use SERIALIZABLE" not a free lunch?

---

## 8.4 — Query Optimization

**Tier: 🎯 Master**

### What to Learn
- **Why `SELECT *` is harmful** — it transfers more data than needed over the network, can prevent an index-only scan (8.2) from being possible at all, and silently breaks when the schema changes underneath it (a new column appears in results the calling code never asked for or expected).
- **The N+1 problem** — one query to fetch N parent records, then N additional queries to fetch each one's related data in a loop, instead of one batched query — this is the single most common real-world performance bug in web applications, and directly connects to Phase 5's N+1 fix in whichever backend track is current — Prisma's `include` (5A.4) now, Spring Data JPA's `JOIN FETCH`/`@EntityGraph` (5B.3) later.
- **JOIN types** — `INNER` (only matching rows on both sides), `LEFT`/`RIGHT` (all rows from one side, matched rows from the other, `NULL` where no match), `FULL OUTER` (all rows from both sides), `CROSS` (every combination — rarely intentional) — know precisely which one a given business question calls for, since picking the wrong one silently drops or duplicates real data.
- **OFFSET vs cursor-based pagination** — `OFFSET 100000` still requires the database to scan and discard 100,000 rows before returning your page, so cost grows with page depth; cursor-based pagination (`WHERE id > :lastSeenId ORDER BY id LIMIT 20`) jumps directly to the right spot using an index, with roughly constant cost regardless of depth.
- **CTEs (`WITH ... AS (...)`)** — improve readability and let you reference the same computed result multiple times; know that in Postgres, CTEs are not always automatically inlined/optimized the same way a subquery would be (this has changed across Postgres versions — check the behavior of the version you're actually using rather than assuming).
- **Subqueries vs JOINs** — often logically equivalent, but not always equivalent in the plan the optimizer chooses — always verify with `EXPLAIN`, don't assume one is always faster.
- **Materialized views** — precompute and store the result of an expensive query, refreshed on a schedule (or on demand) — the right tool when a report/aggregation is read far more often than the underlying data changes.
- **Batch operations vs row-by-row** — a single `INSERT INTO ... VALUES (...), (...), (...)` is dramatically faster than the same number of individual `INSERT` statements, because it avoids per-statement round-trip and parsing/planning overhead.

### Practice
1. Enable query logging, call an endpoint that exhibits the N+1 pattern, count the actual queries fired, then fix it and confirm the count collapses to a small constant (directly reusing the Phase 5A.4 Prisma exercise, but this time focusing purely on the SQL/EXPLAIN side rather than the ORM-specific syntax).
2. Compare `EXPLAIN ANALYZE` output and timing for the same "give me page 5000" query implemented with OFFSET vs with a cursor, on a table with a few million rows.
3. Write the same report query as a subquery and as a JOIN, compare their `EXPLAIN ANALYZE` plans, and note whether the optimizer actually treated them identically or not.
4. Insert 10,000 rows via a loop of individual `INSERT` statements, time it, then insert the same 10,000 rows via one batched multi-row `INSERT`, and compare.

### Answer This
- Why does `SELECT *` potentially prevent an index-only scan that a column-specific `SELECT` wouldn't?
- Why does OFFSET pagination get progressively slower at deeper pages, while cursor-based pagination doesn't?
- Concretely, what real data-correctness bug can result from using `INNER JOIN` where the business question actually required `LEFT JOIN`?
- Why is a single batched multi-row `INSERT` faster than the same number of individual `INSERT` statements?

---

## 8.5 — PostgreSQL in Depth

**Tier: 🎯 Master**

### What to Learn
- **Full-text search** — `tsvector` (a preprocessed, searchable representation of a document — normalized, stemmed, stripped of stop words) and `tsquery` (the parsed search expression), matched against each other and backed by a **GIN** index (8.2) for real performance at scale; `ts_rank` orders results by textual relevance instead of just returning an unordered matching set.
- **`pgvector`** — adds a `vector` column type and nearest-neighbor search operators (`<->` for distance) directly inside Postgres, with `ivfflat`/`hnsw` index types for approximate-nearest-neighbor search at scale — the practical, low-friction starting point for AI-adjacent similarity search (embeddings) without standing up a separate specialized vector database.
- **Row-level security (RLS)** — define a policy directly on a table that transparently filters every query (even ones a developer "forgot" to add a `WHERE tenant_id = ?` to) based on the current session/user context — the strongest practical enforcement of multi-tenant data isolation, because it's enforced by the database itself, not by application-layer discipline (directly connects to Phase 5's multi-tenancy discussion — 5A.9 now, 5B.8 later).
- **`LISTEN`/`NOTIFY`** — a lightweight, built-in pub/sub mechanism inside Postgres itself, useful for simple cases like cache invalidation signals without introducing a separate message broker.
- **JSONB** — a binary, indexable, queryable JSON column type (`->`/`->>` operators for field access); genuinely useful for semi-structured or highly variable data, but a real anti-pattern when used to avoid doing proper schema design for data that's actually structured and known in advance.
- **Connection pooling with PgBouncer** — Postgres connections are relatively expensive (each is a full OS process, not a lightweight thread); opening a fresh connection per web request at any real scale exhausts the database, so a pooler sits in front, multiplexing many application-level "connections" onto a smaller number of real Postgres connections — **transaction pooling mode** is the common choice for typical web workloads.

### Practice
1. Build full-text search on a real text column: create a `tsvector` (generated column or trigger-maintained), add a GIN index, and query with `tsquery` + `ts_rank`, comparing results and performance against a naive `LIKE '%term%'` search on the same data.
2. Enable RLS on a multi-tenant table, define a policy filtering by the current session's tenant context, and confirm — by deliberately writing a query with no `WHERE tenant_id` clause at all — that RLS still correctly prevents cross-tenant data from ever being returned.
3. Store a JSONB column and query into a nested field with `->>`, then add a GIN index on the JSONB column and confirm via `EXPLAIN` that it's actually used.
4. Set up PgBouncer in front of a local Postgres instance in transaction pooling mode, and measure connection-handling behavior under a burst of concurrent short-lived connections, with and without the pooler.

### Answer This
- Why does RLS provide a stronger multi-tenancy guarantee than "every developer remembers to filter by tenant_id"?
- Why can JSONB be a legitimate tool in one case and an anti-pattern in another — what's the actual distinguishing factor?
- Why do Postgres connections need pooling at scale, when a typical application-server thread pool doesn't need the equivalent for, say, in-memory objects?

---

## 8.6 — Redis Beyond Caching

**Tier: 🎯 Master**

### What to Learn
- **Data types and their real use cases** — string (simple values, atomic counters via `INCR`), list (queues, recent-activity feeds), set (unique membership collections, tagging), **sorted set** (leaderboards, and rate limiting — using the score as a timestamp and counting entries within a sliding time window, from Phase 2.7's priority-queue concept applied practically), hash (compact object/session storage), stream (an append-only log, closer in spirit to a lightweight Kafka).
- **Cache patterns** — cache-aside (the application checks cache first, falls back to the source of truth on a miss, then populates the cache — the most common pattern), write-through (write to cache and the source of truth together, synchronously), write-behind (write to cache immediately, asynchronously persist to the source of truth later — faster, but risks losing the write if the async persistence fails before it happens).
- **Cache invalidation** — TTL-based expiration (simple, but can serve stale data until it expires), event-driven invalidation (more precise, more complex to get right), versioned keys (embed a version in the key itself, so "invalidating" is just starting to use a new version).
- **Cache stampede** — when a popular, expensive-to-recompute cache entry expires, many concurrent requests can all simultaneously miss and all hit the underlying source of truth at once, overwhelming it — mitigated with a mutex/lock (only one request recomputes, others wait) or probabilistic early expiration (recompute slightly before actual expiry, staggered across requests, so they don't all expire in the same instant).
- **Pub/Sub** — real-time message fan-out between services or connections, directly relevant to the WebSocket-scaling problem from Phase 7.3.
- **Lua scripting** — lets you execute multiple Redis commands as a single atomic operation, avoiding race conditions between separate commands that would otherwise need to be combined (e.g., "check a value, then conditionally modify it" needs to be atomic to be race-free).
- **Persistence tradeoffs** — RDB (periodic point-in-time snapshots — fast to restore from, but can lose recent writes since the last snapshot) vs AOF (append-only log of every write — more durable, larger, slower to replay) — you can also run both together for a better durability/performance balance.

### Practice
1. Implement a sliding-window rate limiter using a Redis sorted set (score = timestamp), and write a test proving it correctly allows N requests per window and rejects the (N+1)th within the same window.
2. Reproduce a cache stampede on purpose: have many concurrent "requests" (threads/processes) all miss an expired cache key simultaneously and all hit a simulated slow "source of truth" at once; then fix it with a simple mutex-based recomputation guard and confirm only one actual recomputation happens.
3. Write a small Lua script that atomically checks a value and conditionally updates it, and confirm — by running many concurrent clients against it — that it doesn't suffer the race condition an equivalent two-step (non-atomic) command sequence would.

### Answer This
- Why is a sorted set a natural fit for both leaderboards and sliding-window rate limiting — what property do they share?
- What specifically causes a cache stampede, and why does a mutex-based guard fix it?
- Why does combining two separate Redis commands (a `GET` then a conditional `SET`) risk a race condition that a single Lua script avoids?

---

## 8.7 — Scaling Databases

**Tier: 👀 Know Only**

### What to Learn
- **Read replicas** — a primary handles writes, one or more replicas handle reads, replicating data asynchronously (usually) from the primary; **replication lag** is real and means a read from a replica can be milliseconds-to-seconds stale — directly relevant to "why did I just write something and then immediately not see it when I read it back" bugs (a classic "read-your-own-writes" consistency problem).
- **Sharding** — partitioning data across multiple independent database instances, by user ID (hash-based — spreads load evenly but makes cross-shard queries hard), by time (range-based — good for time-series-heavy workloads, but can create "hot" shards for recent data), or by geography (good for data-residency and latency requirements, but uneven load if usage isn't evenly distributed geographically) — every sharding strategy is a real tradeoff, not a free scaling lever.
- **Connection pooling** at the database-instance level — the sizing formula `(number_of_cores × 2) + effective_spindle_count` is a rough starting heuristic, not a law — the real point is that more connections than the database can efficiently context-switch between makes things *slower*, not faster, which is counterintuitive to engineers used to thinking "more concurrency = more throughput" without limit.
- **CAP theorem in practice** — during an actual network partition, a system must choose: **CP** (remain Consistent by refusing some requests until the partition heals — most traditional RDBMS setups lean this way) or **AP** (remain Available by serving possibly-stale data during the partition — many NoSQL systems, by design, lean this way); outside of an actual partition, this tradeoff doesn't force a choice — it's specifically a during-partition decision.

### Practice
1. Set up a primary/replica Postgres pair (even locally via Docker), write to the primary, and immediately read from the replica in a tight loop — measure how long replication lag actually takes to catch up under a real (even light) write load.
2. Design (on paper, no need to fully build) a sharding strategy for a hypothetical multi-million-user application by user ID vs by signup date, and write down one real query that becomes hard or impossible to answer efficiently under each strategy (e.g., "total users across all shards" under hash sharding, "all of one user's data" under time-based sharding if their data spans shards).

### Answer This
- Why can reading from a replica immediately after writing to the primary return stale data, and what real bug does that cause if the application doesn't account for it?
- Why isn't "just add more connections" a valid scaling strategy for the database tier, in the way it might seem to be for a stateless application server?
- What does "choosing CP or AP" actually mean it applies to — all the time, or specifically during a network partition?

---

## 8.8 — The Broader Database Landscape

**Tier: 👀 Know Only**

> Ecosystem awareness — you don't need production mastery of all of these, but you should recognize the shape of a problem that calls for one of them instead of forcing everything into PostgreSQL.

| Database | Type | Best For |
|---|---|---|
| PostgreSQL | Relational | Financial data, complex queries — your default choice |
| MongoDB | Document | Flexible/nested schemas that don't fit a rigid relational shape well |
| Redis | Key-Value | Caching, sessions, rate limiting, leaderboards |
| Cassandra | Wide-column | Massive write-heavy workloads, time-series, tolerant of eventual consistency |
| Elasticsearch / OpenSearch | Search engine | Full-text search at a scale/complexity beyond Postgres's built-in FTS, log analysis |
| Neo4j | Graph | Social graphs, recommendation engines — queries needing 6+ joins in a relational model |
| InfluxDB / TimescaleDB | Time-series | Downsampling, retention policies, high-cardinality metrics |
| CockroachDB / YugabyteDB | NewSQL | Distributed SQL with serializable-by-default consistency and geo-replication |
| DuckDB | Analytical (embedded) | Local, in-process analytical queries — no server to run at all |

### Vector Databases (AI/RAG use cases)
| Database | Best For |
|---|---|
| pgvector | Start here if already on Postgres — no new infrastructure, ACID guarantees |
| Pinecone | Managed, production-ready, hybrid search, metadata filtering |
| Weaviate | GraphQL interface, vector + BM25 hybrid search |
| Qdrant | Rust-based, high performance, on-premise friendly |
| Chroma | Lightweight, local development, tight LangChain integration |

### Normalization Awareness
- **1NF–3NF** — the standard normalization progression (eliminate repeating groups, eliminate partial dependencies on a composite key, eliminate transitive dependencies) — the point isn't to recite the definitions, it's to recognize when a schema has genuine redundancy that will cause update anomalies (the same fact stored in two places that can drift out of sync).
- **Deliberate denormalization** — sometimes correct, specifically to avoid expensive joins on a hot read path, but it should be a conscious, documented tradeoff (accepting some duplication and the responsibility of keeping it in sync) — not something that happens by accident from not knowing normalization in the first place.

### Practice
1. For three different hypothetical scenarios (a social graph with deep "friend of friend" queries, a fleet of IoT devices reporting metrics every second, a product catalog needing typo-tolerant search), write down which database type from the table above you'd reach for and why — specifically naming what about PostgreSQL alone would make each scenario painful.
2. Take a normalized schema and deliberately denormalize one relationship for read performance (e.g., storing a `user_name` snapshot directly on an `order` row instead of always joining to `users`), then write down, explicitly, the tradeoff you just accepted (a name change won't retroactively update old orders) and whether that's actually the correct business behavior for that field.

### Answer This
- What specifically makes Cassandra a better fit than PostgreSQL for a massive, write-heavy, time-series-like workload?
- Why might a deep, "friend of friend of friend" relationship query become painful in a relational database, and why does a graph database handle it more naturally?
- What real risk do you accept when you deliberately denormalize data for read performance?

---

## Phase 8 Capstone

> **PROJECT:** Create a PostgreSQL table with 1,000,000+ rows and run the complete indexing exercise from 8.2, recording real before/after `EXPLAIN ANALYZE` output at every step (no index, B-Tree index, composite index, covering index). Reproduce and fix an N+1 query. Reproduce a non-repeatable read and a deadlock deliberately, on purpose, in two separate `psql` sessions, and explain both in writing. Build full-text search on a real dataset using `tsvector`/`tsquery`/GIN. Implement a Redis-backed rate limiter using a sorted set, and deliberately reproduce and then fix a cache stampede. Set up a primary/replica pair and measure real replication lag under load.

---

# Phase 9 — Data Engineering (Awareness)

**Phase tier: 👀 Know Only — entirely, this phase was already designed as awareness-level**

> You do not need to become a data engineer. You need to understand this landscape well enough to design systems that feed data pipelines correctly, collaborate effectively with a data team, and recognize whether a given problem is a data-engineering problem or an application-engineering problem. This phase is intentionally lighter than Phase 8 — awareness, not mastery.

## 9.1 — ETL vs ELT

**Tier: 👀 Know Only**

### What to Learn
- **ETL (Extract, Transform, Load)** — transform data *before* loading it into the destination warehouse; the traditional approach, and still the right choice when transformation must happen before data leaves a sensitive source system (e.g., masking PII before it's ever copied anywhere else).
- **ELT (Extract, Load, Transform)** — load raw data first, then transform it *inside* the warehouse using SQL; the modern default, enabled by cheap, fast columnar warehouses (Snowflake, BigQuery) that make it practical to keep raw data around and re-transform it repeatedly as requirements evolve, rather than committing to one transformation shape up front.

### Answer This
- Why would a team choose ETL over ELT specifically for data containing sensitive PII?
- What made ELT practical as a default in a way it wasn't a decade ago?

---

## 9.2 — Batch & Stream Processing

**Tier: 👀 Know Only**

### What to Learn
- **Batch processing** — Apache Spark (DataFrames, distributed processing across a cluster), Hadoop MapReduce (older, largely superseded by Spark but still found in legacy systems), Databricks (a managed Spark platform); partitioning strategy has a dramatic effect on job performance, since poorly partitioned data forces expensive data shuffling across the cluster; batch wins for large historical datasets, scheduled reports, and ML training runs where near-real-time isn't required.
- **Stream processing** — **Apache Kafka** is the backbone of most real data pipelines (topics, partitions for parallelism, consumer groups for scaling out consumption, configurable retention); Apache Flink (stateful stream processing with exactly-once semantics and explicit handling of event-time vs processing-time); Spark Streaming (a micro-batch approach to streaming, reusing existing Spark infrastructure). **Windowing** — tumbling (fixed, non-overlapping time buckets), sliding (overlapping buckets), session (gap-based — a window closes after a period of inactivity) — the choice of windowing strategy changes what question you're actually answering about the stream. Streaming wins for real-time dashboards, fraud detection, and live recommendations, where the value of the data decays quickly with delay.

### Answer This
- Why does poor data partitioning hurt a Spark job's performance specifically, in terms of what has to happen across the cluster?
- What's the practical difference between event-time and processing-time in a stream processor, and why does that distinction matter for correctness (not just performance)?

---

## 9.3 — Orchestration & Data Quality

**Tier: 👀 Know Only**

### What to Learn
- **Apache Airflow** — DAG-based workflow orchestration defined in Python — effectively "cron on steroids," with dependency management, retries, and backfills (re-running historical pipeline periods when the transformation logic changes) built in; **sensors** wait for an external condition (a file arriving, an API becoming available) before triggering the next step. Prefect/Dagster are more modern alternatives focused on developer experience.
- **Idempotent pipeline tasks** — every step must be safe to re-run without producing duplicate or incorrect results, since retries and backfills are the normal operating mode of a real pipeline, not an edge case (directly the same principle as Phase 5's idempotent job handlers — BullMQ in 5A.7 now, RabbitMQ in 5B.6 later — applied at pipeline scale).
- **Data quality tooling** — Great Expectations and dbt tests let a team define explicit expectations about data (uniqueness, non-nullness, value ranges, referential integrity) and run them as automated tests against real data, catching upstream schema/data changes before they silently break downstream reports.

### Answer This
- Why must every task in a data pipeline be idempotent, given how backfills and retries actually work in practice?
- What real failure does automated data-quality testing (Great Expectations/dbt tests) catch that a passing application test suite wouldn't?

---

## 9.4 — Data Lakes, Warehouses & Modeling

**Tier: 👀 Know Only**

### What to Learn
- **Data lakes** — raw data stored cheaply in object storage (S3) using columnar formats like Parquet/ORC; **Delta Lake/Apache Iceberg/Apache Hudi** add ACID transactions, schema evolution (adding columns without rewriting all existing data), and "time travel" (querying data as it looked at a past point in time) on top of otherwise-simple object storage.
- **Data warehouses** — Snowflake, BigQuery, Redshift, DuckDB — built on **columnar storage**, which is why analytical "aggregate this one column across billions of rows" queries can be 10-100x faster than the same query against a row-oriented database like Postgres (directly connects to the row-vs-column storage tradeoff from 8.1).
- **Star schema** — a central fact table (the events/transactions themselves) surrounded by dimension tables (the descriptive context — customers, products, time) — the standard, well-understood data warehouse modeling approach.
- **Slowly Changing Dimensions (SCD)** — how a warehouse tracks a dimension's value changing over time (e.g., a customer's address changing) — Type 1 (overwrite, lose history), Type 2 (keep every historical version as a new row with validity date ranges), Type 3 (keep only the previous value in an extra column) — each is a real, deliberate tradeoff between simplicity and historical accuracy.

### Answer This
- Why is columnar storage dramatically faster than row storage specifically for "aggregate one column across many rows" queries?
- What real business question can Type 2 SCD answer that Type 1 fundamentally cannot?

---

## 9.5 — Product Analytics & Data Contracts

**Tier: 👀 Know Only**

### What to Learn
- **Product analytics tools** — Mixpanel/Amplitude (event tracking, funnel analysis, cohort retention), Segment (routes a single instrumented event to multiple downstream analytics tools without your application needing to know about each one individually).
- **Event tracking design** — deciding, deliberately, what events your application should emit for analytics from the start of a feature's design, not retrofitted afterward once someone asks "why don't we have data on this."
- **Data contracts** — an explicit agreement between your application (the producer of an event schema) and the data team (the consumer of it) about what fields exist, their types, and what changing them means; **schema enforcement** matters because inconsistent or silently-changed event schemas are one of the most common ways downstream analytics quietly breaks without anyone noticing until a report looks wrong weeks later.

### Practice
1. For one real feature you've built (or plan to build), write down explicitly what analytics events it should emit, what fields each event needs, and what a data team would actually be able to answer using them — treat this as a design step, not an afterthought.

### Answer This
- Why does an unannounced change to an event's schema (renaming or removing a field) cause damage that isn't immediately visible?
- What does Segment let you avoid duplicating across every analytics tool you use?

---

## Phase 9 Capstone

> **PROJECT (lighter, awareness-level):** Take your Phase 5 Master Project and, without necessarily implementing a full pipeline, design what a real data pipeline around it would look like: which events it should emit and why, what a star-schema model of its core business activity would look like (fact table + dimensions), and whether a Type 1 or Type 2 SCD approach makes sense for one specific piece of changing dimension data in your domain (e.g., a user's plan tier). Write this up as a short design doc — the goal is demonstrating you can think like a good collaborator to a data team, not that you've built Kafka/Airflow/Spark infrastructure yourself.
