# Part II — Building Software (Detailed)
### Companion deep-dive to `Master_Senior_Fullstack_Roadmap.md` → Phases 4, 5, 6, 7

> This file expands **Phase 4 (Frontend Engineering)**, **Phase 5 (Backend Engineering — Track A: Node.js + NestJS now, Track B: Java + Spring Boot later)**, **Phase 6 (Mobile Engineering — React Native)**, and **Phase 7 (API Design)** into full depth.
>
> **Why this part matters the way it does:** this is where Part I's fundamentals stop being abstract. The event loop (Phase 1) is why `useEffect` timing confuses people, and it's the same event loop underneath Node's Phase 5A. The JVM (Phase 1) is why Spring Boot's startup behaves the way it does — relevant once you reach Track B. Hash maps (Phase 2) are why `HashMap` vs `LinkedHashMap` is a real decision, not trivia. SOLID and Repository (Phase 3) are why both Prisma (Track A) and Spring Data JPA (Track B) are structured the way they are. Treat every "why" in this document as a pointer back to something you already learned — if a "why" doesn't click, that's a signal to revisit Part I, not to memorize this part harder.
>
> Go in order within each phase. Frontend before backend if you're newer to the stack — but if backend is your stronger side already, it's fine to start there. Within Phase 5, do Track A (Node/NestJS) in full before touching Track B (Java/Spring Boot) — that sequencing is deliberate, see the note at the top of Phase 5. Mobile assumes Frontend (React) is already solid. API Design assumes Backend is underway, since REST design decisions are made *in* your backend's code, whichever track you're currently on.
>
> **"Under the Hood" sections:** several key subsections (4.1 JS Core, 4.2 Async JS, 4.3 TypeScript, 4.5 React, 5A.3 NestJS, 5B.1 Java, 5B.2 Spring Boot) include an extra "Under the Hood" block going one level deeper than "what to use and why" — into the actual mechanism (V8's hidden classes, React's fiber tree, TypeScript's structural typing, Nest's `reflect-metadata`-driven DI container, Java bridge methods, Spring's proxy-based `@Transactional`). These come with their own "build a toy version from scratch" exercises (a hand-rolled Promise, a minimal vDOM diff, a minimal DI container, a minimal transactional proxy) — do these once each; they're what turns "I can use this API" into "I know why this API behaves the way it does."

---

## Table of Contents

**Phase 4 — Frontend Engineering**
- 🎯 [4.1 — JavaScript: The Core Language](#41--javascript-the-core-language)
- 🎯 [4.2 — Async JavaScript](#42--async-javascript)
- 🎯 [4.3 — TypeScript, Strict Mode From Day One](#43--typescript-strict-mode-from-day-one)
- 🎯 [4.4 — Browser & DOM APIs](#44--browser--dom-apis)
- 🎯 [4.5 — React: How It Actually Works](#45--react-how-it-actually-works)
- 🎯 [4.6 — Next.js](#46--nextjs)
- 🎯 [4.7 — Styling](#47--styling)
- 🎯 [4.8 — Build Tools & Dev Tooling](#48--build-tools--dev-tooling)
- 🎯 [4.9 — Frontend Performance](#49--frontend-performance)
- [Phase 4 Capstone](#phase-4-capstone)

**Phase 5 — Backend Engineering (Track A: Node.js/NestJS now · Track B: Java/Spring Boot later)**
- 🎯 [5A.1 — Node.js Fundamentals (Deep)](#5a1--nodejs-fundamentals-deep)
- 🎯 [5A.2 — Express Fundamentals](#5a2--express-fundamentals)
- 🎯 [5A.3 — NestJS Core Concepts](#5a3--nestjs-core-concepts)
- 🎯 [5A.4 — Prisma & the Data Layer](#5a4--prisma--the-data-layer)
- 🎯 [5A.5 — Auth & Security in NestJS](#5a5--auth--security-in-nestjs)
- 🎯 [5A.6 — NestJS Production Features](#5a6--nestjs-production-features)
- 🎯 [5A.7 — Async & Background Jobs (BullMQ)](#5a7--async--background-jobs-bullmq)
- 🎯 [5A.8 — Testing NestJS](#5a8--testing-nestjs)
- 👀 [5A.9 — Backend Production Concerns](#5a9--backend-production-concerns)
- [Track A Capstone](#track-a-capstone)
- ⏸️ [5B.1 — Java Fundamentals (Deep)](#5b1--java-fundamentals-deep)
- ⏸️ [5B.2 — Spring Boot Core Concepts](#5b2--spring-boot-core-concepts)
- ⏸️ [5B.3 — Spring Data JPA](#5b3--spring-data-jpa)
- ⏸️ [5B.4 — Spring Security](#5b4--spring-security)
- ⏸️ [5B.5 — Spring Boot Production Features](#5b5--spring-boot-production-features)
- ⏸️ [5B.6 — Async & Background Jobs](#5b6--async--background-jobs)
- ⏸️ [5B.7 — Testing Spring Boot](#5b7--testing-spring-boot)
- ⏸️ [5B.8 — Backend Production Concerns](#5b8--backend-production-concerns)
- [Track B Capstone](#track-b-capstone)

**Phase 6 — Mobile Engineering (React Native)**
- ⏸️ [6.1 — Why React Native After React Web](#61--why-react-native-after-react-web)
- ⏸️ [6.2 — React Native Fundamentals](#62--react-native-fundamentals)
- ⏸️ [6.3 — Expo](#63--expo)
- ⏸️ [6.4 — Navigation](#64--navigation)
- ⏸️ [6.5 — Styling for Mobile](#65--styling-for-mobile)
- ⏸️ [6.6 — Mobile Performance](#66--mobile-performance)
- ⏸️ [6.7 — Device APIs & Storage](#67--device-apis--storage)
- ⏸️ [6.8 — Networking & Offline](#68--networking--offline)
- ⏸️ [6.9 — App Store Deployment](#69--app-store-deployment)
- [Phase 6 Capstone](#phase-6-capstone)

**Phase 7 — API Design**
- 🎯 [7.1 — REST Principles (Deep)](#71--rest-principles-deep)
- 🎯 [7.2 — REST Best Practices](#72--rest-best-practices)
- 🎯 [7.3 — WebSockets](#73--websockets)
- 🎯 [7.4 — API Security](#74--api-security)
- 🎯 [7.5 — API Documentation](#75--api-documentation)
- 👀 [7.6 — Beyond REST](#76--beyond-rest)
- [Phase 7 Capstone](#phase-7-capstone)

---

# Phase 4 — Frontend Engineering

**Phase tier: 🎯 Master — all of it, this is half of 'fullstack'**

> HTML/CSS are the foundation (Phase 0). JavaScript is the language. TypeScript is the discipline. React is the framework. This order is deliberate — skipping ahead to React without JavaScript fluency just means you'll be debugging JavaScript through a React-shaped fog.

## 4.1 — JavaScript: The Core Language

**Tier: 🎯 Master**

### What to Learn
- **Single-threaded execution** — one call stack, one thing happening at a time; this is the foundational fact that makes everything about async JavaScript (4.2) make sense — there is no "background thread" quietly running your `.then()` callback, it's all still one thread, just cleverly scheduled.
- **Execution context** — global context vs function context, and how `this` gets bound differently depending on *how* a function is called, not where it's defined.
- **Closures** — a function retains access to the variables from its enclosing scope even after that outer function has returned. This is the mechanism behind callbacks holding onto state, the module pattern (pre-ES6 encapsulation), and memoization caches.
- **Prototypal inheritance** — objects inherit from other objects via an internal `[[Prototype]]` chain; `class` syntax in modern JS is sugar over this same mechanism, not a different inheritance model. Understanding this explains why modifying `Array.prototype` affects every array in your program.
- **`this` binding** — determined at *call time*, not definition time, for regular functions (`obj.method()` binds `this` to `obj`; calling the same function unbound loses that); `call`/`apply`/`bind` let you control it explicitly; arrow functions instead capture `this` lexically from their enclosing scope, which is *why* arrow functions became the default for callbacks inside class methods and React components.
- **Scope** — `var` is function-scoped and hoisted (accessible, but `undefined`, before its declaration line); `let`/`const` are block-scoped and sit in a **temporal dead zone** until their declaration line is reached (accessing them earlier throws, rather than silently giving `undefined`).
- **Type coercion** — `==` performs type coercion before comparing (leading to surprising results like `'' == 0` being `true`); `===` does not — this is *why* `===` is the default recommendation, not a style preference. Truthy/falsy values: `0`, `''`, `null`, `undefined`, `NaN`, and `false` are falsy — everything else, including `'0'` and `[]`, is truthy.
- **Error handling** — `try/catch/finally`; custom `Error` subclasses for meaningful error types; rethrowing vs swallowing.
- **Memory leaks in JS** — detached DOM nodes still referenced from JS (the DOM node is removed from the page but JS still holds a reference, so it's never collected), and event listeners added but never removed (especially in single-page apps that never fully reload).

**ES6+ features to master:**
- Destructuring (`const { name, email } = user`, `const [first, ...rest] = arr`) — reduces boilerplate and makes intent explicit.
- Spread/rest (`{ ...defaults, ...overrides }`, `function sum(...args)`).
- Template literals — multiline strings and expression interpolation.
- Optional chaining (`user?.address?.city`) and nullish coalescing (`value ?? default`) — note `??` only falls back on `null`/`undefined`, unlike `||` which falls back on *any* falsy value (a common source of subtle bugs with `0` or `''` as legitimate values).
- Modules (`import`/`export`, named vs default, dynamic `import()` for lazy loading).
- Generators/iterators (`function*`, `yield`) — lazy sequences, the mechanism underlying custom iteration protocols.
- `WeakMap`/`WeakSet` — keys are weakly referenced, meaning they don't prevent garbage collection — useful for caching metadata about objects without causing a memory leak.

### Under the Hood — How V8 Actually Runs Your Code
This is the layer underneath everything above — it's *why* JS behaves and performs the way it does, not just *that* it does.

- **Parsing** — your source text becomes tokens (lexer), then an AST (parser). V8 doesn't fully parse everything up front: it **pre-parses** (a fast scan that just finds function boundaries and checks for syntax errors) and only does a full parse of a function's body the first time that function actually runs (**lazy parsing**). This is why a large bundle full of code paths you never hit doesn't cost as much at startup as its raw size suggests.
- **Ignition (the bytecode interpreter)** — the AST compiles to bytecode, and Ignition starts executing that bytecode immediately. There's no separate "compile the whole program" step blocking execution — this is *why* JS feels like it "just runs" instead of needing an explicit build step to start.
- **TurboFan (the optimizing JIT)** — V8 profiles bytecode as it runs. A function called enough times is considered "hot" and gets compiled by TurboFan into optimized machine code — but that optimized code is built around the *actual types V8 has observed so far*, not the full range JavaScript theoretically allows.
- **Hidden classes (a.k.a. "Shapes" or "Maps")** — V8 does not store every object as a generic hash map. When you create `{ x: 1, y: 2 }`, V8 assigns it a hidden class describing that exact shape (which properties, in which order). Objects built the same way — same constructor, same properties added in the same order — share a hidden class, which lets V8 compile property access to a fixed memory offset instead of a runtime lookup. Add properties in a different order, or add/delete properties after construction, and you fragment your objects across different hidden classes. **This is the real, mechanical reason** "always initialize every property in the constructor, in the same order every time" is genuine performance advice and not superstition.
- **Inline caching** — at a specific call site (e.g., `obj.x` inside a loop), V8 remembers which hidden class it saw last time. Same hidden class again next time → **monomorphic**, fastest path, cached offset reused directly. A handful of different hidden classes hitting the same call site → **polymorphic**, still cached but slower (a small lookup table instead of one offset). Many different shapes → **megamorphic**, V8 gives up caching and falls back to a slow generic property lookup. This is the actual mechanism behind "don't pass wildly differently-shaped objects through the same hot function."
- **Deoptimization** — TurboFan's optimized machine code is built on assumptions (e.g., "this variable has always been a small integer here"). If a later call violates that assumption, V8 throws away the optimized code and falls back to the interpreter — a **deopt**. A function that keeps getting optimized, deoptimized, and reoptimized in a hot loop is a genuine, measurable performance problem, and it's invisible unless you know to look for it.

### Practice
1. Write a closure-based counter (`function makeCounter() { let count = 0; return () => ++count; }`) and explain, in your own words, exactly why `count` survives after `makeCounter` returns.
2. Deliberately write a `this`-binding bug (pass `obj.method` as a callback without binding it) and observe/fix it three ways: `.bind(obj)`, an arrow-function wrapper, and converting the callback itself to an arrow function.
3. Write five expressions and predict their truthy/falsy/coercion outcome *before* running them: `'' == 0`, `null == undefined`, `[] == false`, `'0' == false`, `NaN === NaN`. Then verify and explain any surprises.
4. Reproduce a real memory leak: add an event listener inside a component/module that never gets removed, and confirm via a heap snapshot (Phase 0, 0.4) that the associated object is retained after it should have been eligible for collection.
5. Write two versions of a function that reads `.x` off 10 million objects in a loop: one where every object is built via the same constructor with properties added in the same order, one where property order/presence varies between objects. Time both with `console.time`/`console.timeEnd` and confirm the monomorphic version is meaningfully faster.
6. Run a small script with `node --trace-opt --trace-deopt your-script.js` where a function is called many times with consistent types, then once with an inconsistent type (e.g., a number then a string) — watch V8 optimize the function and then deoptimize it in the trace output.

### Answer This
- Why does a closure keep a variable alive after its enclosing function has already returned?
- Why does `this` inside a regular callback function often not refer to what you expect, and why do arrow functions fix that?
- Why is `??` safer than `||` for defaulting a value that might legitimately be `0` or `''`?
- Concretely, what makes a DOM node "detached" but still leak memory?
- Why does `obj.x` inside a hot loop get faster over time — what is V8 actually doing differently on the 10,000th call versus the first?
- What's a hidden class, and why does the *order* you add properties in change which hidden class an object gets?
- What's the practical difference between a monomorphic, polymorphic, and megamorphic call site, and why does it matter for performance?

---

## 4.2 — Async JavaScript

**Tier: 🎯 Master**

### What to Learn
- **The event loop** — call stack (synchronous code) + task queue (macrotasks) + microtask queue; the loop only pulls the next macrotask from the queue once the call stack is completely empty, and it drains the *entire* microtask queue between every macrotask.
- **Macrotasks vs microtasks** — `setTimeout`/`setInterval`/I/O callbacks are macrotasks; `Promise.then`/`queueMicrotask`/`MutationObserver` callbacks are microtasks. Microtasks always run before the next macrotask, which is why a `Promise.resolve().then(...)` fires before a `setTimeout(fn, 0)` even though both are "async."
- **Callbacks and callback hell** — the original async pattern, and precisely why deeply nested callbacks became unmaintainable (error handling duplicated at every level, control flow obscured by nesting).
- **Promises** — three states (pending, fulfilled, rejected) and the guarantee that a settled promise never changes state again; chaining `.then()` returns a new promise, enabling flat, readable sequencing instead of nesting; `.catch()` and `.finally()`.
- **`async/await`** — syntactic sugar over promises; `await` only pauses the *current async function*, not the whole program — the rest of your JS keeps running via the event loop while it waits.
- **Concurrent patterns** — `Promise.all` (run all in parallel, rejects immediately if any one rejects — "fail fast"), `Promise.allSettled` (run all in parallel, always resolves with each individual outcome — use when partial failure is acceptable), `Promise.race` (settles as soon as the first one settles, whether fulfilled or rejected), `Promise.any` (settles as soon as the first one *fulfills*, ignoring rejections unless all reject).

### Under the Hood — Build Your Own Promise From Scratch
`async`/`await` and `.then()` chaining feel like magic until you've built the mechanism yourself. A Promise is really just a small state machine plus two callback queues — nothing more.

The core mechanics to implement:
- **State machine** — a promise starts `pending` and can transition exactly once, either to `fulfilled` (with a value) or `rejected` (with a reason); once settled, it never changes state again, no matter how many times `resolve`/`reject` get called afterward.
- **Callback queuing** — if `.then(onFulfilled, onRejected)` is called *before* the promise settles, the callbacks must be stored and run later, once settlement happens; if `.then()` is called *after* it's already settled, the callback should still run — asynchronously, not immediately, which is why real Promises schedule their callbacks via the microtask queue (4.2 above) instead of calling them synchronously inline.
- **Chaining** — `.then()` must itself return a *new* promise, whose resolution depends on what the handler function returns: a plain value fulfills the new promise directly; a thrown error rejects it; and — the trickiest part — if the handler returns *another* promise, the new promise must wait on that inner promise's outcome instead of resolving immediately (this recursive "unwrap thenables" behavior is what makes chained `.then()` calls flatten instead of nesting into `Promise<Promise<Promise<T>>>`).

### Practice
1. Predict, on paper, the console output order of a script mixing `console.log`, `setTimeout(fn, 0)`, and `Promise.resolve().then(fn)` — then run it and confirm your prediction. Repeat until the microtask-before-macrotask rule feels automatic, not memorized.
2. Fetch data from a real API using plain `fetch` + `async/await`, with a loading state and proper `try/catch` error handling — no framework.
3. Fire off 5 independent API calls using `Promise.all` and time it against firing them sequentially with `await` in a loop — measure and explain the difference.
4. Deliberately let one of five parallel requests fail and compare the behavior of `Promise.all` (whole thing rejects) vs `Promise.allSettled` (you get all 5 outcomes, one marked rejected).
5. **Build a `MyPromise` class from scratch** — no `Promise` allowed internally — implementing `pending`/`fulfilled`/`rejected` state, a constructor that takes an `executor(resolve, reject)` function, a `.then(onFulfilled, onRejected)` that queues callbacks for a still-pending promise and returns a new chainable `MyPromise`, and correct handling of a handler that returns another `MyPromise` (must unwrap it, not nest it). Get `new MyPromise(r => setTimeout(() => r(1), 100)).then(x => x + 1).then(x => console.log(x))` printing `2` before you consider it done.
6. Once your `MyPromise` works, implement `MyPromise.all` on top of it (an array of promises in, one promise out, resolving with an array of results or rejecting on the first failure) — this forces you to actually track "how many of these have settled so far," which is the real mechanic `Promise.all` hides from you.

### Answer This
- Why does a `Promise.resolve().then()` callback run before a `setTimeout(fn, 0)` callback, even though both appear "immediate"?
- What does `await` actually pause — the whole program, or just the current function? What keeps running while it waits?
- When would you deliberately choose `Promise.allSettled` over `Promise.all`?
- In your own `MyPromise`, why does `.then()` have to return a *new* promise instead of just running the callback and returning nothing?
- Why does a `.then()` handler that returns another promise need special "unwrapping" logic, instead of just fulfilling immediately with that inner promise object?

---

## 4.3 — TypeScript, Strict Mode From Day One

**Tier: 🎯 Master**

### What to Learn
- **Type inference** — TypeScript infers types from context far more often than beginners expect; over-annotating everything is a sign of not trusting inference, not a sign of rigor.
- **Interfaces vs type aliases** — interfaces are extendable/mergeable and are the conventional choice for object shapes; type aliases are more flexible (unions, primitives, mapped types) — prefer interfaces for objects, types for everything else.
- **Generics** — `function identity<T>(value: T): T` lets you write one function that's type-safe across any type, instead of duplicating logic or falling back to `any`.
- **Discriminated unions** — `type Shape = Circle | Rectangle`, where each variant shares a common literal field (e.g., `kind: 'circle' | 'rectangle'`); TypeScript then narrows the type automatically inside an `if`/`switch` on that field — this is arguably the single most useful TypeScript pattern for modeling real domain logic safely.
- **`unknown` vs `any`** — `any` disables type checking entirely for that value (a hole in your safety net); `unknown` forces you to narrow/check the type before using it in any way — always prefer `unknown` at any boundary where the type genuinely isn't known yet (API responses, JSON.parse results).
- **Strict mode** — `"strict": true` in `tsconfig.json` turns on `noImplicitAny`, `strictNullChecks`, and several other checks together; enable it from the start of a project — retrofitting strict mode onto a large loose codebase later is far more painful than starting with it.
- **Utility types** — `Partial<T>` (all fields optional), `Required<T>` (all fields required), `Pick<T, K>` (select specific fields), `Omit<T, K>` (exclude specific fields), `ReturnType<T>` (extract a function's return type), `Record<K, V>` (a typed object/dictionary) — these let you derive new types from existing ones instead of manually duplicating shapes.
- **Declaration files (`.d.ts`)** — how TypeScript understands the shape of JavaScript libraries that weren't written in TypeScript.

### Under the Hood — Structural Typing & How Narrowing Actually Works
Most languages with static types (Java, C#) use **nominal typing** — two types are compatible only if one explicitly declares itself as (or extends/implements) the other, by name. TypeScript does not work this way, and not understanding that difference is why TS's type errors sometimes feel surprising.

- **Structural typing ("duck typing," but checked at compile time)** — TypeScript asks *"does this value have the required shape?"*, never *"was this value declared as this specific type?"*. A plain object literal `{ x: 1, y: 2 }` satisfies a `type Point = { x: number; y: number }` even though it was never declared as a `Point` anywhere — it just happens to have the right shape. This is *why* two completely unrelated interfaces with identical fields are freely interchangeable in TypeScript, and it's also why an object with *extra* properties can still fail to satisfy a type in a literal assignment (**excess property checking**) even though structurally it has everything required — TS special-cases object literals specifically to catch typos like `{ nmae: 'x' }`.
- **Type narrowing — control flow analysis** — TypeScript tracks a variable's possible type as it flows through your code, refining it at each branch. `typeof x === 'string'` narrows `x` to `string` inside that branch; `x instanceof Foo` narrows to `Foo`; a discriminated union's shared literal field (`if (shape.kind === 'circle')`) narrows the *whole object* to the matching variant, not just that one field. This isn't runtime magic — it's a compile-time-only analysis of your code's control flow graph; TypeScript is reasoning about which branch you're in and eliminating impossible types from the union at each point, then erasing all of it before the code ever runs.
- **Why this matters for `unknown`** — `unknown` is deliberately the *opposite* of a structural free pass: TypeScript won't let you access any property or call any method on an `unknown` value until you've narrowed it (via `typeof`, `instanceof`, or a custom type guard), which is exactly why it's the safe replacement for `any` at real boundaries like a parsed API response.

### Practice
1. Take a real `.js` file and convert it to TypeScript with `strict: true` from the start. Fix every single error before moving on — don't reach for `any` as an escape hatch except as a last resort, and if you do, leave a comment explaining why.
2. Model a real domain concept (e.g., an API response that can be `{ status: 'success', data: T }` or `{ status: 'error', message: string }`) as a discriminated union, then write a function that narrows it correctly in a `switch`.
3. Take a function currently typed with `any` for an incoming argument and retype it as `unknown`, then add the narrowing logic required to actually use it safely.
4. Declare two unrelated interfaces with identical fields (e.g., `interface Dog { name: string }` and `interface Company { name: string }`) and confirm TypeScript happily accepts a `Dog` value being passed where a `Company` is expected — then explain, from the structural typing rule, why that's not a bug.
5. Write a custom type guard function (`function isError(x: unknown): x is { message: string }`) and use it to narrow an `unknown` caught error into something you can safely read `.message` from.

### Answer This
- Why is over-annotating every variable a sign you're not trusting TypeScript's inference, rather than a sign of extra safety?
- What's the concrete difference in what `any` and `unknown` let you do with a value before you've checked its type?
- Why does a discriminated union let TypeScript narrow types automatically inside a `switch`, when a plain union of unrelated shapes wouldn't?
- Why does TypeScript accept an object with the right shape as satisfying an interface it was never declared to implement — what would a nominally-typed language (Java) require instead?
- What is "excess property checking," and why does it only trigger on object literals and not on a variable holding the same shape?

---

## 4.4 — Browser & DOM APIs

**Tier: 🎯 Master**

### What to Learn
- **DOM tree structure** — how the browser represents parsed HTML in memory (ties directly back to Phase 1.5); manipulation via `querySelector`/`querySelectorAll`, `createElement`, `appendChild`, `removeChild`.
- **Event bubbling vs capturing** — events propagate down (capturing phase) then back up (bubbling phase) through ancestors by default; `stopPropagation()` halts further propagation, `preventDefault()` stops the browser's default action (e.g., a link navigating) — these are two different things people frequently conflate.
- **Event delegation** — attach one listener to a common ancestor instead of one listener per child element; relies directly on bubbling, and is dramatically more efficient for long/dynamic lists.
- **`IntersectionObserver`** — efficiently detect when an element enters/leaves the viewport (the correct, non-janky way to implement lazy loading or infinite scroll, instead of polling `scroll` events).
- **`ResizeObserver`** — watch an element's size changes without polling `window.resize`.
- **`MutationObserver`** — observe DOM changes made by other code (useful for reacting to third-party scripts mutating the page).
- **`fetch` API** — request/response objects, `AbortController` for cancelling in-flight requests (critical for cancelling a request when a React component unmounts), streaming responses via `response.body.getReader()`.
- **Web Workers** — run JavaScript on a separate OS thread, off the main thread, for CPU-heavy work that would otherwise freeze the UI; communicate via `postMessage` (no shared memory, message-passing only, by default).
- **Service Workers** — a script that runs independently of any page, sitting between your app and the network; enables intercepting requests (for caching/offline support), background sync, and push notifications.

### Practice
1. Build a list with event delegation (one listener on the parent `<ul>`) instead of one listener per `<li>`, and verify it still works correctly when new `<li>` elements are added dynamically.
2. Implement lazy-loading images using `IntersectionObserver` instead of a `scroll` event listener; compare the DevTools Performance trace of each approach.
3. Use an `AbortController` to cancel an in-flight `fetch` request when a component unmounts (or when a new request supersedes an old one), and confirm in the Network tab that the aborted request actually shows as cancelled.
4. Move one genuinely CPU-heavy computation (e.g., processing a large array) into a Web Worker and confirm the main thread/UI stays responsive during it, compared to running it inline.

### Answer This
- What's the actual difference between `stopPropagation()` and `preventDefault()` — what does each one stop?
- Why is event delegation more efficient than attaching a listener to every list item, mechanically?
- Why does `IntersectionObserver` avoid the performance problems of a `scroll`-event-based visibility check?
- What do Web Workers share with the main thread, and what do they not?

---

## 4.5 — React: How It Actually Works

**Tier: 🎯 Master**

### What to Learn

**Core mechanics**
- **JSX** compiles to `React.createElement(type, props, ...children)` calls — it's not magic, it's a syntax transform; understanding this demystifies why JSX has rules that plain HTML doesn't (e.g., `className` instead of `class`, since it's really a JS object property).
- **Components** are pure functions of props → JSX; **props** are immutable and flow one direction (parent → child) — this constraint is deliberate and is what makes React's data flow predictable.
- **Reconciliation** — React compares (diffs) the newly rendered virtual DOM tree against the previous one to compute the minimal set of real DOM changes needed, rather than re-rendering the whole page on every state change.
- **Fiber** — React's internal unit of work representing each component, enabling React to pause, prioritize, and resume rendering work instead of blocking the main thread with one giant synchronous render.
- **Keys** — help the reconciler match elements across renders when rendering a list; using array index as a key when the list can reorder/filter causes real, hard-to-spot bugs (state getting attached to the wrong item) because React matches by key, not by position or content.

**Hooks — know each one's actual purpose, not just its signature**
- `useState` — triggers a re-render on update; React 18 batches multiple `setState` calls together automatically, even inside `setTimeout`/promises (not just inside event handlers, as in React 17).
- `useEffect` — synchronizes a component with an external system (subscriptions, manual DOM manipulation, logging) — it is explicitly *not* meant for computing derived state (just compute it during render) or for data fetching in modern React (TanStack Query exists specifically to replace that anti-pattern).
- `useRef` — holds a mutable value that persists across renders *without* triggering one when it changes; also the mechanism for accessing a real DOM node.
- `useContext` — consume a context value without prop drilling; every consumer re-renders when the context value changes, which is why Context is good for infrequently-changing values (theme, current user) and a poor fit for frequently-changing state.
- `useReducer` — an alternative to `useState` for state with complex transition logic, especially when the next state depends heavily on the previous one in non-trivial ways.
- `useMemo`/`useCallback` — memoize a computed value or a function reference respectively, so referential equality is preserved across renders when inputs haven't changed; genuinely useful to prevent expensive recomputation or unnecessary child re-renders, but reaching for them by default (rather than after profiling shows a real problem) adds complexity for often-negligible gain.
- `useLayoutEffect` — like `useEffect` but fires synchronously after DOM mutations, before the browser paints — use only when you need to measure/adjust the DOM before the user sees a flicker.
- Custom hooks — the mechanism for extracting and sharing stateful logic between components (`useFetch`, `useLocalStorage`, `useDebounce`) without wrapper-component nesting.

**React 18+ features**
- Automatic batching everywhere (not just event handlers).
- `startTransition` — mark a state update as non-urgent, letting React interrupt it in favor of more urgent updates (like the next keystroke).
- Concurrent rendering — React can pause a render, do something else, then resume.
- `Suspense` — a declarative way to show a fallback while a child is "not ready" (data-fetching integration, code-split components).
- **Server Components** — components that render entirely on the server, shipping zero JavaScript to the client for that component's logic — a fundamentally different tradeoff from traditional client-rendered React.
- Hydration and hydration errors — the process of attaching React's event handlers/state to server-rendered HTML that's already on the page; a hydration error means the client's first render didn't match what the server actually sent.

**State management — pick the simplest tool that solves the actual problem**
- Local state (`useState`/`useReducer`) — the default; reach past this only when a real cross-component need arises.
- Context API — fine for infrequent updates; understand its re-render cost before reaching for it as a general-purpose store.
- **Zustand** — minimal global state, no boilerplate, selector-based subscriptions so components only re-render when the specific slice they read actually changes.
- **TanStack Query** — solves *server* state specifically (data that lives on a server and can go stale) — caching, background refetching, stale-while-revalidate, optimistic updates — this is a fundamentally different problem from client state, which is why it deserves its own tool instead of being shoved into `useState`+`useEffect`.

**Performance**
- Re-renders are caused by: a component's own state changing, a consumed context changing, or its parent re-rendering (which by default re-renders all children, regardless of whether their props changed).
- `React.memo` — skip a re-render if props are shallow-equal to the previous render.
- React DevTools Profiler — the correct way to find *which* components are re-rendering unnecessarily, instead of guessing and sprinkling `useMemo` everywhere.

**Routing**
- React Router v6 — nested routes, data loaders/actions, protected routes (auth guards that redirect before rendering a protected page), and using URL query params as a legitimate form of state (shareable, back-button-friendly).

### Under the Hood — Fiber, Priority Lanes & the Actual Diffing Algorithm
"Reconciliation" and "Fiber" get named constantly and explained rarely. Here's the actual mechanism.

- **The Fiber tree** — for every element in your component tree, React keeps a corresponding **fiber**: a plain JS object holding that component's type, props, state, and — critically — pointers to its `child` (first child fiber), `sibling` (next sibling fiber), and `return` (parent fiber). This linked-list-like structure, not a recursive call stack, is what lets React's work be **interruptible**: walking `child`→`sibling`→`return` pointers can pause after any single fiber and resume later, something a plain recursive tree-walk (React's pre-Fiber, pre-16 architecture) cannot do, because you can't pause and resume a JS call stack mid-recursion.
- **Two trees, not one** — React actually keeps *two* fiber trees at once: the `current` tree (what's on screen right now) and a `workInProgress` tree being built for the next render. Reconciliation compares the new element tree against `current` while building `workInProgress`, then atomically swaps `workInProgress` in to become the new `current` once the whole tree is ready — this "double buffering" is why a render is never partially visible to the user, it's all-or-nothing at the commit step.
- **Priority lanes** — not all updates are equal. A user's keystroke is urgent; a background data refresh triggered by `startTransition` is not. React 18 assigns each update a **lane** (a priority bucket), and can pause low-priority work to let a higher-priority update (like the next keystroke) jump ahead and commit first — this is the actual mechanism behind `startTransition` and concurrent rendering, not a vague "React gets smarter about scheduling."
- **The diffing rules (why keys matter, mechanically)** — React's diff is deliberately **not** a general tree-diff algorithm (those are O(n³) and too slow to run on every render). Instead it uses two cheap heuristics: (1) elements of a **different type** at the same position (e.g., a `<div>` replaced by a `<span>`) are assumed completely unrelated — React tears down the whole old subtree (including all its state) and builds a fresh one, rather than trying to reconcile them; (2) for a **list of children**, React matches old fibers to new elements by `key`, not by position — a stable key lets React recognize "this is the same logical item, just possibly moved" and reuse its fiber (and state) instead of discarding and rebuilding it. Using the array index as the key defeats this: when the list reorders, the *index* stays put but the *item* at that index changes, so React matches the wrong old fiber to the wrong new element and state ends up attached to the wrong row — this is the mechanical root cause behind the bug in Practice item 1 below, not just "a bad practice you're supposed to remember."

### Practice
1. Deliberately use array index as a `key` in a reorderable/filterable list, introduce a bug where an input's local state ends up attached to the wrong row after reordering, then fix it with a stable ID key and explain exactly why the bug happened.
2. Build one component correctly using `useEffect` to subscribe to an external event source (e.g., `window.resize`) with proper cleanup in the returned function, and confirm — via a console log — that the cleanup actually runs on unmount and before every re-subscription.
3. Replace a `useEffect`-based data-fetching pattern (with manual loading/error state) with TanStack Query, and note concretely what code you got to delete.
4. Use the React DevTools Profiler on a real component tree to find one component re-rendering unnecessarily; fix it with `React.memo` or by moving state down, and confirm the re-render disappears in a second profiling pass.
5. **Build a minimal virtual-DOM diff function from scratch** (plain JS, no React): given an `oldTree` and `newTree` represented as simple nested objects (`{ type, props, children }`), write a `diff(oldTree, newTree)` function that returns a list of patch operations (`REPLACE`, `UPDATE_PROPS`, `ADD_CHILD`, `REMOVE_CHILD`). Implement React's actual heuristics: different `type` at the same position → `REPLACE` the whole subtree, don't try to reconcile it; same `type` → diff props and recurse into children. Then extend it to diff a **list** of children two ways — once by array position (no keys) and once by a `key` field — and reproduce, in your own tiny diff, the exact reordering bug from Practice item 1. Seeing your own function produce the bug is what makes the `key` rule click permanently.

### Answer This
- Why does using an array index as a `key` cause bugs specifically when the list can reorder or have items removed from the middle?
- Why is `useEffect` the wrong tool for computing derived state, and what should you do instead?
- Concretely, what causes a child component to re-render even when its own props haven't changed?
- Why does TanStack Query solve a different problem than `useState`, rather than just being "fancier `useState`"?
- Why does the fiber tree's `child`/`sibling`/`return` pointer structure let React pause and resume rendering, when a plain recursive tree-walk can't?
- What are React's two fiber trees (`current` and `workInProgress`) each for, and why does keeping both prevent the user from ever seeing a half-finished render?
- Why does React tear down and rebuild an entire subtree when an element's *type* changes at a given position, instead of trying to patch it in place?

---

## 4.6 — Next.js

**Tier: 🎯 Master**

### What to Learn
- **App Router vs Pages Router** — App Router (built on React Server Components) is the current direction; Pages Router is the older, still-widely-deployed model — know both exist, since you'll encounter Pages Router in real codebases.
- **SSR vs SSG vs ISR** — Server-Side Rendering (render per request, always fresh, slower per request), Static Site Generation (render at build time, fastest, can go stale until rebuilt), Incremental Static Regeneration (statically generated, but can be re-generated on a schedule or on-demand without a full rebuild) — choosing between them is a real tradeoff decision per page/route, not a framework default to accept blindly.
- **Server Components vs Client Components** — Server Components render on the server and ship no JS for their own logic; Client Components (`"use client"`) are the traditional interactive React you already know — the mental model shift is choosing, per component, whether it needs interactivity/browser APIs at all.
- **Edge runtime** — running code at CDN edge locations, closer to the user, with a more constrained runtime than full Node.js.
- **Middleware** — intercept a request before it reaches a page (auth checks, redirects, A/B test routing).
- **Image optimization** — automatic responsive image generation, format conversion (e.g., to WebP), and lazy loading built into the framework's `<Image>` component.

### Practice
1. Build the same simple page three ways — one SSR, one SSG, one ISR — and articulate, from the actual behavior you observe (rebuild needed? fresh on every request? revalidates on a timer?), which real use case each fits.
2. Add middleware that redirects unauthenticated users away from a protected route, and confirm it runs *before* the page component even starts rendering.

### Answer This
- What's the real tradeoff between SSR and SSG — what do you gain and lose by choosing one over the other for a given page?
- Why does a Server Component ship zero JavaScript for its own logic, and what does that mean for interactivity?

---

## 4.7 — Styling

**Tier: 🎯 Master**

### What to Learn
- **Tailwind CSS** — utility-first composition directly in JSX; the **JIT compiler** only ships classes you actually use in your source, keeping production CSS small regardless of how many utilities exist in the framework; responsive prefixes (`sm:`, `md:`, `lg:`) are mobile-first (a bare class applies to all sizes, a prefixed class applies from that breakpoint up); `dark:` for dark mode; state variants (`hover:`, `focus:`, `disabled:`); extending `tailwind.config` for a real design system (custom colors, spacing scale) instead of fighting the defaults; `@apply` for extracting a genuinely repeated utility group into a named class — use sparingly, since it partially defeats the "everything visible inline" benefit of the utility-first approach.
- **CSS Grid, deeper** — `grid-template-areas` for naming layout regions directly, `minmax()` for a track that can grow but not shrink below a floor, `auto-fit` vs `auto-fill` (the frequent point of confusion: `auto-fit` collapses/removes empty tracks so existing items stretch to fill the space, `auto-fill` keeps the empty tracks present, leaving gaps), subgrid for nested grids that align with their parent's tracks.
- **Flexbox, deeper** — `align-items` (cross-axis alignment for all items) vs `align-content` (spacing of the lines themselves, only relevant when wrapping), the `gap` property (spacing between flex/grid items without needing margin hacks).

### Practice
1. Build the same responsive card grid twice: once with Tailwind utilities, once with hand-written CSS Grid — write down, honestly, where each approach felt faster and where each felt more constrained.
2. Build a layout using `grid-template-areas` that visually rearranges (e.g., sidebar moves from left to bottom) at a mobile breakpoint, using only a media query that redefines the areas — no JS.
3. Deliberately misuse `auto-fill` where you meant `auto-fit` (or vice versa) in a responsive grid, observe the visual difference (gaps vs stretched items), and explain why.

### Answer This
- Why does Tailwind's production bundle stay small even though the framework itself defines thousands of utility classes?
- What's the actual visual difference between `auto-fit` and `auto-fill` in a CSS Grid, and why does it happen?
- When would `align-content` actually do something different from `align-items`?

---

## 4.8 — Build Tools & Dev Tooling

**Tier: 🎯 Master**

### What to Learn
- **Vite** — ES modules served directly (unbundled) in development for near-instant startup and HMR (Hot Module Replacement); a real bundling step (via Rollup) only happens for the production build.
- **How a bundler works, conceptually** — start from an entry point, walk the `import` graph to build a full dependency graph, then emit one or more output bundles.
- **Tree shaking** — dead code elimination based on statically-analyzable ES module imports/exports; this is *why* named imports from a large library ship less code than a default "import everything" pattern, when the library is tree-shakeable.
- **Code splitting** — splitting output into multiple bundles (by route, by component, vendor-vs-app-code) so users only download what the current page actually needs.
- **Source maps** — map minified/transpiled production code back to original source for debugging, without shipping unminified code to users.
- **ESLint + Prettier** — static analysis catching real bugs and enforcing consistency (ESLint) vs pure formatting (Prettier) — they solve different problems and are normally run together, not as alternatives to each other.
- **`tsconfig.json` strict flags** — know what `strict: true` actually turns on (see 4.3) rather than treating it as an opaque toggle.
- **Husky + lint-staged** — run linting/formatting automatically on the files about to be committed, catching problems before they ever reach a PR.

### Practice
1. Import one named export vs the entire default export from a real tree-shakeable library, build for production both ways, and compare the resulting bundle size.
2. Set up route-based code splitting (`React.lazy` + `Suspense`) on a multi-page app and confirm in the Network tab that a route's JS chunk only loads when you actually navigate to it.
3. Set up Husky + lint-staged so that a commit with a lint error is physically blocked, and confirm it by trying to commit a deliberately broken file.

### Answer This
- Why does tree shaking depend specifically on ES module `import`/`export` syntax, rather than working equally well on CommonJS `require`?
- What's the practical difference between what ESLint catches and what Prettier does?
- Why does Vite feel faster in development than a traditional bundler-based dev server, mechanically?

---

## 4.9 — Frontend Performance

**Tier: 🎯 Master**

### What to Learn
- **Core Web Vitals** — LCP (Largest Contentful Paint, target < 2.5s — how fast the main content appears), INP (Interaction to Next Paint, target < 200ms — how responsive the page feels to input), CLS (Cumulative Layout Shift, target < 0.1 — how much visible content jumps around unexpectedly).
- **Lazy loading** — images (`loading="lazy"`), components (`React.lazy()`), and data (infinite scroll / pagination) — deferring work that isn't needed for the initial view.
- **Code splitting** — route-based, component-based, and vendor chunking (separating rarely-changing third-party code from frequently-changing app code, for better browser caching).
- **Image optimization** — modern formats (WebP/AVIF vs JPEG/PNG), `srcset` for serving appropriately-sized images per device, CDN-based on-the-fly image processing.
- **Bundle analysis** — tools like Webpack Bundle Analyzer or `vite-bundle-visualizer` to actually *see* what's contributing to bundle size, instead of guessing.
- **Debounce and throttle** — debounce delays execution until input stops for a period (good for search-as-you-type); throttle guarantees execution at most once per interval regardless of how often the event fires (good for scroll/resize handlers) — these solve different problems and picking the wrong one produces a subtly wrong UX.
- **`React.memo`/`useMemo`/`useCallback` used judiciously** — only after the Profiler (4.5) has shown a real, measured re-render problem; reaching for them preemptively adds cognitive overhead for often negligible or zero benefit.
- **Compression** — Brotli generally compresses text assets smaller than Gzip; both matter far more than most micro-optimizations in the actual bundle code.
- **Resource hints** — `preload` (fetch a critical resource early), `prefetch` (fetch a likely-next-navigation resource during idle time), `preconnect` (establish a connection to a third-party origin before it's actually needed).

### Practice
1. Run Lighthouse against a real project you've built. Record the baseline LCP/INP/CLS. Fix the single worst-scoring issue. Re-run and record the new numbers — this before/after discipline (Phase 12's "golden rule": measure → fix → measure again) applies here just as much as it does to backend performance.
2. Deliberately cause a CLS problem (an image with no reserved width/height that shifts content once it loads) and fix it by reserving space up front.
3. Implement a search input with debounce vs one with throttle and compare the resulting UX side by side — articulate which one is correct for "search as you type" and why the other one feels wrong.

### Answer This
- What does INP actually measure, and how is it different from raw page-load speed?
- Why does an image without explicit width/height attributes cause layout shift, and how does reserving space fix it?
- When would throttle be the correct choice over debounce, and vice versa?

---

## Phase 4 Capstone

> **PROJECT:** Build a full React + TypeScript application (strict mode from the first commit) using TanStack Query for all server data, Zustand for any genuinely global client state, React Router for navigation (including at least one protected route), and Tailwind CSS for styling. Add Playwright E2E tests for the critical user path. Run Lighthouse, record baseline Core Web Vitals, fix the worst offender, and re-measure. Use the React DevTools Profiler at least once to find and fix a real unnecessary re-render — don't add `useMemo`/`useCallback` anywhere without first proving, via the Profiler, that it's solving an actual measured problem.

---

# Phase 5 — Backend Engineering (Two Tracks: Node.js/NestJS Now, Java/Spring Boot Later)

**Phase tier: 🎯 Track A (Node.js + NestJS) — Master now, in full. ⏸️ Track B (Java + Spring Boot) — deferred until Track A's capstone ships.**

> This sequencing is deliberate, not a downgrade of Java. Full-stack JavaScript (React from Phase 4 + Node/NestJS below) comes first because it lets you ship real freelance projects fastest with one language across the entire stack. Java + Spring Boot comes second, aimed specifically at the professional/enterprise job market, once JS is genuinely solid — not because "big projects need Java," but because that's where the job market and hiring pools for it actually are. NestJS is the deliberate bridge between the two tracks: its architecture — dependency injection, modules, decorators, guards/pipes/interceptors — is functionally the same mental model Spring Boot uses under different syntax, so mastering Track A pre-trains the hardest part of Track B rather than being a detour from it. Every Track A subsection below explicitly notes its Track B analogue for exactly this reason. Go through Track A in full, ship its capstone, then come back for Track B exactly where it's written below — untouched and waiting.

## Track A — Node.js + NestJS

**Tier: 🎯 Master — this is your near-term backend, do all of it**

## 5A.1 — Node.js Fundamentals (Deep)

**Tier: 🎯 Master**

### What to Learn
- **The runtime, not the language** — Node embeds V8 (the JS engine) plus **libuv** (the C library providing the event loop, a thread pool, and async I/O); JS syntax itself is already covered in Phase 4.1/4.2 — this section is specifically what's different about running JS outside a browser.
- **Module systems** — CommonJS (`require`/`module.exports`, resolved synchronously at runtime) vs ESM (`import`/`export`, the modern standard, statically analyzable — which is exactly what enables tree-shaking in bundlers, per Phase 4.8); Node supports both, but mixing them within one project causes real, recurring interop pain (a CJS package's default export not being what you expect under `import` syntax) — pick one as primary per project and understand why the other exists.
- **Node's event loop phases, specifically** — this is *not* the same diagram as the browser's event loop from 4.2: timers → pending callbacks → idle/prepare → poll → check → close callbacks, cycling in that order; `process.nextTick()` and Promise microtasks both drain fully between every phase, with `nextTick` taking priority over Promise microtasks — a subtlety that trips up anyone assuming the browser model carries over unchanged.
- **Streams** — Readable, Writable, Duplex, Transform; **backpressure** — a writable stream signals when its internal buffer is full via the return value of `.write()`, and ignoring that signal (writing faster than the destination can drain) is a real, common source of memory blowups in file uploads and proxying; `pipe()` handles backpressure for you automatically, which is why it's preferred over manually wiring `data`/`write` events.
- **Core built-in modules** — `fs` (sync vs async vs promise-based variants — always prefer async/promise versions in a server, since a sync `fs` call blocks the *entire* event loop for every concurrent request), `path` (cross-platform path handling — never concatenate paths with string `+`), `process` (`process.env`, `process.argv`, `process.exit`, signal handling for graceful shutdown), `Buffer` (raw binary data, relevant for file uploads and network protocols).
- **npm/package.json mechanics** — semver ranges (`^`, `~`, exact) and what each actually permits; `package-lock.json` pins the exact resolved dependency tree and must be committed; `node_modules` resolution (why a nested dependency's version can differ from a top-level one); `npm ci` vs `npm install` — CI should always use `ci`, since it installs exactly what the lockfile says and fails loudly if it can't, instead of silently updating.
- **Environment configuration** — `.env` files (via `dotenv`, or Node's built-in `--env-file` in modern versions), and why secrets live in environment variables, never committed to the repo — ties forward to Phase 14.8 Secrets Management.

### Practice
1. Write a script that reads a large file with `fs.createReadStream` and pipes it to a writable stream, then repeat it with `fs.readFileSync` on the same large file and observe the memory spike difference via `process.memoryUsage()`.
2. Deliberately call a synchronous `fs` method inside an HTTP request handler under concurrent load (fire several requests at once with `curl` or `autocannon`) and observe every other request stall until the sync call finishes — then fix it with the async version and confirm requests interleave properly.
3. Write one CommonJS module and one ESM module in the same project, deliberately hit an interop issue (e.g., a CJS package's default export not matching what `import` expects), then resolve it and explain exactly what happened.
4. Run `npm ci` on a project whose lockfile is out of sync with `package.json`, observe the failure, and explain why that failure is correct, intended CI behavior rather than a bug.

### Answer This
- Why does a synchronous `fs` call block every concurrent request on a Node server, when async JS is supposed to not block?
- What does backpressure mean concretely for a writable stream, and why does `pipe()` handle it for you automatically?
- Why does `process.nextTick()` run before a Promise microtask, and why does understanding "both run before the next event loop phase" matter more in practice than the exact ordering between them?
- Why should CI always use `npm ci` instead of `npm install`?

---

## 5A.2 — Express Fundamentals

**Tier: 🎯 Master (deliberately brief — this exists to demystify what NestJS abstracts, not to become your primary framework)**

### What to Learn
- **The middleware pattern** — `(req, res, next) => {}`, where every middleware either ends the response or calls `next()` to pass control forward; this single pattern is the entire mental model of Express, and it's also, under the hood, what NestJS's guards/interceptors/pipes are built on top of.
- **Routing** — `app.get/post/put/delete/patch`, route params (`req.params`), query strings (`req.query`), request bodies (`req.body`, which requires `express.json()` middleware to be populated at all).
- **Error handling** — a 4-argument middleware `(err, req, res, next)` is Express's special signature for centralized error handling; forgetting to call `next(err)` inside an async route handler is a classic bug that silently swallows errors, since Express doesn't automatically catch rejected promises in route handlers (pre-Express 5).
- **Router-level modularity** — `express.Router()` groups related routes into their own file/module — the crude ancestor of what Nest's module system formalizes properly.
- **Why this stays intentionally shallow** — the point here is to be able to look at NestJS's Express adapter and know exactly what it's doing underneath, not to build production apps directly on bare Express.

### Practice
1. Build 3–4 CRUD routes for one resource on bare Express, no framework beyond it — in-memory data is fine. Include one deliberately broken async route handler that throws inside an `async` function without a try/catch, and confirm the request just hangs/crashes ungracefully — exactly the failure mode Nest's exception filters exist to prevent.
2. Write one custom middleware logging method + path + response time for every request, mounted globally with `app.use()`.
3. Split your routes into an `express.Router()` module, mounted under a path prefix (`app.use('/api/users', userRouter)`).

### Answer This
- What does calling `next()` inside a middleware actually do, mechanically?
- Why doesn't a thrown error inside an `async` Express route handler get caught automatically (in Express 4), and what's the practical consequence of forgetting to handle it?
- What is `express.Router()` doing that foreshadows what a NestJS module does more formally?

---

## 5A.3 — NestJS Core Concepts

**Tier: 🎯 Master**

> This is the direct analogue of 5B.2 (Spring Boot Core Concepts) — same architecture, different syntax. Learning it well here is what makes Track B faster later.

### What to Learn
- **Modules** (`@Module()`) — the unit of organization; every Nest app has a root module and feature modules grouping related controllers/providers/imports — directly analogous to how a Spring Boot app organizes packages with `@Configuration`/component scanning, except Nest makes the module graph explicit instead of relying on classpath scanning.
- **Dependency Injection** — Nest's IoC container instantiates and wires providers (services) for you; constructor injection is the default and the only pattern to use (`constructor(private readonly usersService: UsersService) {}`) — the exact same "constructor injection always, never field injection" rule from Spring Boot (5B.2), for the exact same reason: explicit dependencies, trivial unit testing without booting the framework.
- **Providers/Services** (`@Injectable()`) — where business logic lives, injected into controllers rather than controllers doing the work themselves — the same separation Spring enforces with `@Service`.
- **Controllers** (`@Controller('users')`, `@Get()`/`@Post()`/`@Put()`/`@Delete()`/`@Patch()`) — the HTTP-facing layer, thin by design, delegating to services — same role as `@RestController` in Spring.
- **DTOs + `class-validator`/`class-transformer`** — decorate a DTO class with `@IsEmail()`, `@IsNotEmpty()`, `@MinLength()`, etc.; a global `ValidationPipe` rejects invalid requests before the handler body runs — the direct analogue of `@Valid` + Bean Validation in Spring (5B.2); never return a database entity directly from a controller here either, for the identical reason (API shape and persistence shape *will* diverge).
- **Guards** (`@UseGuards()`, implementing `CanActivate`) — run before a route handler to decide whether the request is allowed to proceed at all (authentication/authorization) — the same conceptual slot as a Spring Security filter/`@PreAuthorize`.
- **Pipes** — transform or validate incoming data before it reaches a handler (the built-in `ValidationPipe` is the main one you'll use; custom pipes handle parsing/transforming specific params).
- **Interceptors** (`NestInterceptor`, wrapping the handler call with `next.handle()`) — cross-cutting concerns wrapping a request/response (logging, response transformation, caching) — directly analogous to Spring AOP / the Proxy pattern from Phase 3.3.
- **Exception filters** (`@Catch()`, `ExceptionFilter`) — centralize error-to-HTTP-response mapping app-wide — the same role as `@ControllerAdvice` + `@ExceptionHandler` in Spring.
- **Custom decorators** (`createParamDecorator`) — extract and reuse a piece of request data (e.g., `@CurrentUser()` pulling the authenticated user off the request) across many handlers without repeating the extraction logic.

### Under the Hood — How Nest's DI Container Actually Resolves `@Injectable()`
`@Injectable()` and constructor injection look like magic the first time you use them: you declare a dependency as a constructor parameter type, and it just... appears, already instantiated. Here's the actual mechanism, and it depends on a JavaScript feature almost nobody encounters outside of Nest/Angular-style frameworks.

- **The problem decorators alone can't solve** — a decorator like `@Injectable()` runs at class-definition time and can see the class itself, but by default, plain JavaScript (even TypeScript) **erases type information at compile time** — by the time your code runs, `constructor(private usersService: UsersService)` has no runtime record that the first parameter's type was `UsersService`. Nest needs that type information *at runtime* to know what to instantiate and pass in.
- **`reflect-metadata` + `emitDecoratorMetadata`** — this is the missing piece. With `"emitDecoratorMetadata": true` in `tsconfig.json`, TypeScript's compiler emits extra calls to `Reflect.defineMetadata` alongside any decorated class, recording metadata under keys like `design:paramtypes` — literally an array of the constructor parameter types, preserved as real runtime values (the actual class references, e.g. `[UsersService]`), not erased. The `reflect-metadata` package provides the `Reflect.defineMetadata`/`Reflect.getMetadata` polyfill that makes this possible in plain JS, since it isn't (yet) a standard part of the language.
- **How the container resolves a graph from that** — when Nest instantiates `UsersController`, it calls `Reflect.getMetadata('design:paramtypes', UsersController)` to get back `[UsersService]`, recursively resolves `UsersService` the same way (which might itself depend on `PrismaService`, and so on), and only once every dependency in the chain is resolved does it call `new UsersController(usersServiceInstance)`. This is why forgetting `@Injectable()` on a provider, or forgetting to register it in a module's `providers` array, produces a runtime "cannot resolve dependency" error rather than a compile error — the whole mechanism is metadata-driven and only checked when the container actually tries to build the object graph at startup.
- **Why this is the same idea as Spring, different implementation** — Spring's container does the equivalent resolution using Java reflection over constructor parameter types directly (Java never erases this information the way compiled JS does), which is *why* Java doesn't need an equivalent of `reflect-metadata` — the runtime type information Nest has to manufacture via decorator metadata, the JVM already provides natively.

### Practice
1. Build one resource end to end — module, controller, service, DTO with `class-validator` decorators, and a global `ValidationPipe` — then send an invalid request via Postman/curl and confirm it's rejected with a 400 before your handler method body ever executes (same discipline as the Spring `@Valid` exercise in 5B.2).
2. Write a custom guard checking for a hardcoded role on the request (real JWT auth comes in 5A.5) and confirm a request without the right role never reaches the controller — verify with a breakpoint that's never hit, the same technique as the Spring Security exercise.
3. Write one global exception filter that catches at least three different exception types and converts them into the same consistent JSON error envelope defined in Phase 7.2 — the envelope shape should be identical regardless of which backend produced it, since a shared frontend will eventually consume both.
4. Write a custom `@CurrentUser()` param decorator and use it in place of manually reading `req.user` in every handler.
5. In a scratch project, add `reflect-metadata` and `"emitDecoratorMetadata": true`, write a plain `@Injectable()`-decorated class with a constructor parameter of another class type, and call `Reflect.getMetadata('design:paramtypes', YourClass)` directly yourself — print the result and confirm it's really the actual class reference, not just a string.
6. **Build a minimal DI container from scratch** — no Nest, no Angular. Write your own `@Injectable()` decorator (it can just tag the class for now) and a `Container` class with a `resolve(SomeClass)` method that: reads `design:paramtypes` via `reflect-metadata`, recursively resolves each dependency the same way, and returns a fully-constructed instance with everything wired in. Prove it works on a 3-level chain (`A` depends on `B`, `B` depends on `C`) resolved with a single `container.resolve(A)` call. This is the exercise that makes "the container just wires it up" stop being a black box.

### Answer This
- Why does constructor injection make a Nest service trivially testable without booting the Nest application context, mirroring the exact same argument for Spring's constructor injection?
- What's the actual sequence of what runs, in order, when a request with an invalid DTO hits a route protected by a `ValidationPipe` — where exactly does it get rejected?
- Where do guards run relative to pipes and the route handler, and why does that ordering matter specifically for an auth check?
- What Spring Boot concept does a Nest interceptor most directly correspond to, and why?
- Why can't Nest just read a constructor parameter's type directly at runtime the way Spring/Java can, and what mechanism does it use instead to get the same information?
- What actually goes wrong, mechanically, when you forget to add a provider to a module's `providers` array — why is it a runtime error instead of a compile-time one?

---

## 5A.4 — Prisma & the Data Layer

**Tier: 🎯 Master**

> The direct analogue of 5B.3 (Spring Data JPA) — same problems (schema management, N+1, transactions), different tool.

### What to Learn
- **`schema.prisma`** — a single declarative file defining your models, fields, relations, and DB provider; `npx prisma migrate dev` generates and applies a versioned SQL migration from a schema change, and `npx prisma generate` regenerates the fully-typed client — this type safety is compile-time, a real advantage over hand-written SQL or a loosely-typed ORM.
- **The Prisma Client** — a generated, fully-typed query builder (`prisma.user.findMany({ where: { ... } })`); unlike Hibernate/JPA's Active-Record-adjacent entity model, Prisma is a **data-mapper** — your models are plain generated types, not classes with behavior attached.
- **Relations** — `@relation` fields, `include`/`select` to control what related data a query actually fetches (analogous to JPA's lazy vs eager loading, except Prisma makes the fetch explicit per-query rather than configured on the entity).
- **The N+1 problem, Prisma's version** — calling `.findMany()` and then, in a loop, calling a related lookup per row reproduces the identical N+1 pattern from 5B.3; the fix is the same shape as `JOIN FETCH`/`@EntityGraph` — use `include` to fetch relations in the same query instead of one query per row.
- **Transactions** — `prisma.$transaction([...])` for a batch of operations that must all succeed or all fail together, and the interactive transaction API (`prisma.$transaction(async (tx) => {...})`) for logic that needs to read-then-write atomically.
- **Migrations discipline** — the same hard rule as Flyway in 5B.3: never hand-edit a migration that's already been applied in a shared environment; Prisma tracks applied migrations and flags drift — write a new migration instead.
- **Seeding** — `prisma/seed.ts`, run via `npx prisma db seed`, for reproducible local/test data.
- **Prisma Studio** — a GUI for browsing/editing real database data during development, genuinely useful for quickly checking what a query actually wrote.

### Practice
1. Model two related entities (e.g., `User` and `Post`, one-to-many) in `schema.prisma`, run a migration, then write a query that loops and fetches a related record per row — enable Prisma's query logging, count actual queries executed, and confirm the N+1 pattern; fix it with `include` and confirm the query count drops to a small constant, mirroring the exact exercise from 5B.3.
2. Write one interactive transaction that reads a value, computes something based on it, then writes back — explain why doing the read and write as two separate non-transactional calls would be unsafe under concurrent requests.
3. Apply one migration, then edit the already-applied migration file directly and try to run migrations again — observe Prisma's drift detection and understand why that's a feature, mirroring the Flyway checksum exercise from 5B.3.

### Answer This
- Concretely, why does looping over a list and calling a related-record query per item reproduce the N+1 problem, and why does `include` collapse it to one query?
- Why is Prisma described as a "data-mapper" rather than "Active Record," and what does that mean for how your model types are structured versus JPA entities?
- Why shouldn't a read-then-write sequence be split into two separate non-transactional database calls when concurrent requests are possible?

---

## 5A.5 — Auth & Security in NestJS

**Tier: 🎯 Master**

> The direct analogue of 5B.4 (Spring Security).

### What to Learn
- **Authentication vs authorization**, the same distinction as 5B.4 — identity vs permission — and the same classic bug class of checking only "is this user logged in" instead of "does this specific user own this specific resource."
- **Passport.js integration** (`@nestjs/passport`) — Nest wraps Passport strategies as guards; a `JwtStrategy` validates an incoming token and attaches the decoded payload to `request.user`, which your `@CurrentUser()` decorator (5A.3) then reads.
- **`AuthGuard('jwt')`**, applied via `@UseGuards()` on a controller or individual route — conceptually identical to a Spring Security filter chain entry, just declared per-route/controller instead of globally configured.
- **Role/permission-based guards** — a custom guard reading required roles off route metadata (`@SetMetadata`/a custom `@Roles()` decorator) and comparing against `request.user`'s roles — the direct analogue of `@PreAuthorize("hasRole('ADMIN')")`; the same discipline applies: check actual resource ownership, not just role, wherever that distinction matters.
- **Password hashing** — `bcrypt` or `argon2` npm packages, same non-negotiable rule as 5B.4: never a fast hash (MD5/SHA-1/plain SHA-256) for passwords — you want it deliberately slow.
- **Refresh token rotation** — issue a short-lived access token + a longer-lived refresh token, store the refresh token's hash (not the raw token) server-side, and rotate it on every use so a stolen, already-used refresh token is detectable.
- **CORS in Nest** — `app.enableCors({ origin: [...], credentials: true })` configured explicitly at bootstrap, same warning as 5B.4: never combine a wildcard origin with credentialed requests.
- **Helmet** (`helmet` middleware) — sets a battery of security-related HTTP headers (CSP, X-Frame-Options, etc.) with sane defaults in one line — a genuine ergonomic win of the Node ecosystem worth noting here, with no single equally-concise Spring Boot equivalent.

### Practice
1. Implement JWT auth end to end — a login endpoint issuing an access + refresh token pair, a `JwtStrategy`/`AuthGuard('jwt')` protecting a route, and confirm a request without a valid token is rejected before the controller method runs (breakpoint never hit — same verification technique as 5B.4).
2. Build a custom `@Roles()` decorator + `RolesGuard` checking actual resource ownership (not just role), and write a test proving a non-owner is correctly denied even with a valid token — the identical exercise to 5B.4's ownership-check practice, done in Nest instead of Spring.
3. Implement refresh token rotation, then simulate a stolen-and-reused refresh token scenario and confirm your system detects and rejects the reuse.
4. Deliberately configure CORS with a wildcard origin plus `credentials: true` and confirm the browser rejects the combination outright, same as the Spring exercise.

### Answer This
- Where in the request lifecycle does a Nest `AuthGuard('jwt')` run relative to the route handler, and why does that matter for what code executes after it?
- Why is checking "is this token valid" insufficient for authorization, and what specific bug does that gap produce?
- Why does rotating refresh tokens on every use let you detect a stolen, already-used refresh token?

---

## 5A.6 — NestJS Production Features

**Tier: 🎯 Master**

> The direct analogue of 5B.5.

### What to Learn
- **`@nestjs/config`** — typed, validated environment configuration (via `Joi` or `class-validator` schema validation of `process.env`), loaded once at bootstrap — the analogue of Spring profiles/`application.yml`, environment-variable-driven rather than file-per-profile by convention (though `.env.development`/`.env.production` per-environment files are also common).
- **Structured logging** — `nestjs-pino` or Winston for JSON-structured logs with a correlation/request ID attached per request (via middleware or an interceptor that generates/propagates the ID) — the direct analogue of Spring's MDC correlation ID logging in 5B.5, solving the identical problem: filtering concurrent requests' logs down to one request's full trace.
- **Health checks** — `@nestjs/terminus` provides `/health` endpoints checking DB connectivity, memory usage, disk space, etc. — the analogue of Spring Actuator's `/actuator/health`, used the same way by orchestration systems (Kubernetes liveness/readiness probes).
- **Graceful shutdown** — `app.enableShutdownHooks()` plus listening for `SIGTERM` to stop accepting new connections while letting in-flight requests finish — the same non-negotiable requirement as 5B.5 for zero-downtime rolling deploys.
- **Global prefix + versioning** — `app.setGlobalPrefix('api')`, `app.enableVersioning()` — built-in support for the URL-versioning convention from Phase 7.2.
- **Swagger/OpenAPI** — `@nestjs/swagger` generates a live Swagger UI directly from your DTOs and controller decorators (`@ApiProperty()`, `@ApiOperation()`) — the direct analogue of Springdoc in Phase 7.5; docs stay in sync with code because they're generated from it, not hand-maintained separately.

### Practice
1. Set up `@nestjs/config` with schema validation so the app refuses to start if a required environment variable is missing or malformed — confirm by deliberately omitting one and observing a clear startup failure instead of a confusing runtime error later.
2. Add correlation IDs to your logging via an interceptor, fire two concurrent requests, and confirm you can filter the combined log output down to just one request's lines — the identical exercise to 5B.5, done in Nest.
3. Set up `@nestjs/terminus` with a database health check, then stop your database and confirm `/health` correctly reports unhealthy.
4. Enable graceful shutdown, start a slow endpoint, send `SIGTERM` mid-request, and confirm the in-flight request completes instead of being cut off — same verification as the Spring exercise.

### Answer This
- Why should an app refuse to start at all if a required config value is missing, rather than failing later when that value is first used?
- What specific problem does a correlation ID solve here, identically to the Spring/MDC case in 5B.5?
- Why does graceful shutdown matter specifically for zero-downtime rolling deployments, regardless of which backend stack is doing the deploying?

---

## 5A.7 — Async & Background Jobs (BullMQ)

**Tier: 🎯 Master**

> The direct analogue of 5B.6.

### What to Learn
- **BullMQ** (Redis-backed job queue) — `Queue` for producers to add jobs, `Worker` for consumers to process them — conceptually the same producer/consumer decoupling as RabbitMQ + Spring AMQP in 5B.6, just backed by Redis instead of a dedicated message broker; a consumer being down simply means jobs wait in Redis until it's back.
- **`@nestjs/bullmq`** — Nest's integration wraps queues as injectable providers and workers as decorated processor classes (`@Processor()`, `@Process()`), the same "framework wires it for you" convenience Spring AMQP provides with `@RabbitListener`.
- **Retries and backoff** — configure a job's retry count and backoff strategy (fixed or exponential) declaratively when adding it to the queue.
- **Dead letter handling** — BullMQ has no built-in DLQ identical to RabbitMQ's, but the same outcome is achieved by moving a job to a "failed" state after exhausting retries and building a small process to inspect/requeue failed jobs — the same underlying requirement (don't silently lose or infinitely retry a failing job) implemented slightly differently than 5B.6's native DLQ.
- **Idempotency** — the identical non-negotiable requirement from 5B.6: every job handler must produce the same correct end state whether it runs once or is retried, because at-least-once delivery is the norm, not an edge case.
- **`@Cron()`** (`@nestjs/schedule`) — recurring scheduled jobs defined directly in application code, the analogue of Spring's `@Scheduled`.

### Practice
1. Set up a BullMQ producer/consumer pair via Docker Compose Redis, stop the worker, add several jobs, confirm they queue up in Redis, then restart the worker and confirm they all process — the identical exercise to 5B.6's RabbitMQ practice.
2. Configure retries with exponential backoff, force a job handler to fail every time, and confirm it eventually lands in the "failed" job state instead of retrying forever silently.
3. Write a genuinely idempotent job handler (using an idempotency key stored in the DB, exactly as in 5B.6) and deliver the same logical job twice, confirming the end state is correct either way.

### Answer This
- Why must a background job handler in any real system — regardless of which queue technology sits underneath — be designed to tolerate running more than once for the same logical event?
- What's the practical difference between RabbitMQ's native dead letter queue and BullMQ's failed-job-state approach, and why do they solve the same underlying problem?

---

## 5A.8 — Testing NestJS

**Tier: 🎯 Master**

> The direct analogue of 5B.7.

### What to Learn
- **Jest** — Nest's default test runner, configured out of the box; `describe`/`it`/`expect`, mocking via `jest.fn()`/`jest.spyOn()` — the rough analogue of JUnit 5 + Mockito combined into one tool.
- **`Test.createTestingModule()`** — Nest's testing utility for building a real (or partially mocked) DI container for a test, letting you `.overrideProvider()` swap a real service for a mock — the analogue of `@SpringBootTest`/`@MockBean`, at a similarly heavier weight than a pure unit test.
- **Pure unit tests without the Nest testing module** — since Nest services are just classes with constructor injection, you can `new UsersService(mockRepo)` directly in a plain Jest test with zero framework involvement — this is the *fast* end of the pyramid, the same argument made for why Spring's constructor injection enables trivial no-framework unit tests in 5B.2/5B.7.
- **Supertest** — HTTP-level testing against a running (in-memory) Nest app instance (`request(app.getHttpServer()).get('/users')`), the analogue of `MockMvc`/`TestRestTemplate`.
- **Testcontainers for Node** (`testcontainers` npm package) — spin up a real PostgreSQL/Redis in Docker for integration tests, the identical tool and identical motivation as 5B.7: avoid the "works on a lightweight substitute, breaks on the real database" gap.
- **e2e vs unit test file conventions** — Nest's CLI scaffolds `.spec.ts` (unit) separately from `.e2e-spec.ts` (full app, Supertest-driven) by convention, reinforcing the same testing-pyramid discipline (Phase 11) as the Java track.

### Practice
1. Write the same feature test three ways — a pure unit test with manually-constructed mocks (no Nest testing module at all), a `Test.createTestingModule()` test with one provider overridden, and a full e2e test with Supertest against a real running instance — time all three and build the same testing-pyramid intuition the equivalent 5B.7 exercise builds for Spring.
2. Set up Testcontainers for PostgreSQL in a Node test suite, write a repository-level test against it, and use a Postgres-specific feature (JSONB, `ILIKE`) to confirm why testing against the real engine matters.
3. Use `jest.spyOn()` to confirm a service called a mocked email-sending dependency with the exact expected arguments, not just that it was called at all — the Jest equivalent of Mockito's `ArgumentCaptor`.

### Answer This
- Why can a Nest service be unit-tested with a plain `new ClassName(mockDep)` call, with no framework involved at all — and why is that a direct consequence of constructor injection, the same argument as in 5B.2?
- What specific class of bug does testing against a real Testcontainers-backed PostgreSQL catch that a lighter substitute wouldn't, identically to the 5B.7 case?
- What's the practical difference between a `.spec.ts` unit test and an `.e2e-spec.ts` test in a standard Nest project layout?

---

## 5A.9 — Backend Production Concerns

**Tier: 👀 Know Only**

> The Node/NestJS equivalents of 5B.9 — same real-world concerns, situational rather than universal, so this stays a single read-through rather than a drilled section.

### What to Learn
- **Webhooks** — identical principles to 5B.9: verify signatures (HMAC-SHA256) before trusting a payload, respond `200` fast and process asynchronously, design every handler idempotently. In Node, this is typically a raw-body-parsing route (signature verification needs the *unparsed* body) handled outside Nest's default JSON body parser for that one route.
- **Payments** — Stripe's official Node SDK is arguably the most ergonomic of any language's; same rules apply regardless of language: never store raw card data, use idempotency keys on payment-creation calls, never trust a client-side success callback alone.
- **Full-text search** — same PostgreSQL `tsvector`/`tsquery`/GIN-index-first approach as 5B.9, queried through Prisma's raw query escape hatch (`$queryRaw`) since Prisma's query builder doesn't model full-text search natively.
- **Multi-tenancy** — same tradeoffs as 5B.9 (RLS vs schema-per-tenant vs database-per-tenant); Prisma middleware (`$use()`) can enforce a `tenant_id` filter on every query as an app-layer safety net, though RLS at the database layer remains the stronger guarantee for the same reason it does in Postgres regardless of which backend sits on top.
- **Rate limiting** — `@nestjs/throttler` for basic in-process limiting, or a Redis-backed token bucket (via `ioredis`) for the same per-user/per-IP distributed enforcement described in 5B.9.

### Practice
1. Build a webhook receiver with a dedicated raw-body route (bypassing global JSON parsing for just that route), verify an HMAC signature, reject anything that fails, and confirm the same payload delivered twice doesn't double-process.
2. Set up PostgreSQL full-text search via `$queryRaw` and compare its query plan against a naive `LIKE` search, mirroring the 5B.9 exercise.

### Answer This
- Why does verifying a webhook's HMAC signature require access to the *raw*, unparsed request body rather than the parsed JSON object?
- Why is database-level row-level security still the stronger multi-tenancy guarantee even when Prisma middleware can enforce the same filter at the application layer?

---

### Track A Capstone

> **PROJECT:** Build a complete REST API with NestJS — user management with JWT + refresh token rotation and Argon2/bcrypt password hashing, full CRUD via Prisma + PostgreSQL (with at least one deliberately-introduced-then-fixed N+1 query, documented with before/after query counts), Prisma migrations (including one migration added *after* the fact, never editing a previously-applied one), an async background job via BullMQ with a genuinely idempotent handler and a documented failed-job-retry path, health checks via `@nestjs/terminus`, structured logging with correlation IDs, and complete test coverage using the right test type for each layer (pure unit tests, `Test.createTestingModule()` tests, and Testcontainers-backed integration tests) — finished off with Swagger/OpenAPI documentation via `@nestjs/swagger`. **This is your near-term Master Project's backend** (see the top-level roadmap's Master Project section). When you return to Track B later, rebuild this exact same domain in Spring Boot as your natural re-entry point, and compare the two implementations directly.

---

## Track B — Java + Spring Boot

**Phase tier: ⏸️ Skip For Now — deferred until Track A's capstone ships, not neglected. The content below is complete, unchanged, and waiting exactly where it was written; come back and drill it in full once JS is solid.**

> Java forces you to understand OOP, types, and memory more deeply than most languages let you get away with skipping. Spring Boot is the enterprise/fintech industry default — this combination opens doors that a more "trendy" stack alone often doesn't. Everything you built in Track A — DI, guards/pipes as filters, DTOs, migrations, N+1 fixes, idempotent jobs, layered testing — has a named equivalent here; expect this track to move faster than it would have cold.

## 5B.1 — Java Fundamentals (Deep)

**Tier: ⏸️ Skip For Now (deferred — see the Track A/B note at the top of this phase)**

### What to Learn

**OOP in Java**
- **Encapsulation** — hide internal state behind a controlled interface; the entire point is preventing invalid states from being reachable from outside a class.
- **Inheritance** — `extends`, method overriding, `super`; deep inheritance hierarchies are dangerous because a change to a base class can ripple unpredictably through every descendant — prefer composition where it's a real option (a direct callback to the Strategy/Decorator patterns in Phase 3).
- **Polymorphism** — one interface, many implementations, resolved at runtime (`Animal ref = new Dog();` — calling `ref.makeSound()` invokes `Dog`'s implementation, not `Animal`'s).
- **Abstraction** — abstract classes (can hold shared state/implementation, single inheritance only) vs interfaces (pure contract, or default methods in modern Java, multiple "inheritance" of interface) — the decision usually comes down to whether you need shared implementation state.
- **`final`** — a final class can't be subclassed, a final method can't be overridden, a final variable can't be reassigned (but if it's a reference, the object it points to can still be mutated — a common point of confusion).
- **`static`** — belongs to the class itself, shared across all instances, not tied to any one object's state.

**Type System**
- **Generics** — `List<T>`, bounded wildcards `<? extends T>` (upper-bounded — you can *read* from it as a `T`, but can't safely add to it, since the actual runtime type could be any subtype) and `<? super T>` (lower-bounded — you can safely *add* a `T` to it, but reading only guarantees `Object`).
- **Type erasure** — generic type information is erased at runtime; `List<String>` and `List<Integer>` are literally the same class at runtime (`List`), which is why you can't do `new T()` or check `instanceof List<String>` directly.
- **Covariance** — arrays in Java are covariant (`Dog[]` is-a `Object[]`, which can cause a runtime `ArrayStoreException`), but generic collections are deliberately *not* covariant (`List<Dog>` is NOT a `List<Animal>`) specifically to catch that class of error at compile time instead of at runtime.

**Collections Framework (internals, not just the API)**
- `ArrayList` vs `LinkedList` — `ArrayList` wins in nearly all real cases due to cache locality (Phase 1.2), despite `LinkedList` having better theoretical complexity for mid-list insertion.
- `HashMap` internals — array of buckets, each bucket a linked list (or a red-black tree once a bucket exceeds 8 entries, since Java 8) — this ties directly back to Phase 2.5.
- `TreeMap` — backed by a red-black tree, keeps keys sorted, O(log n) operations — use when sorted iteration order actually matters.
- `ArrayDeque` — the correct default for stack/queue behavior in Java (see Phase 2.4).
- `ConcurrentHashMap` — thread-safe without locking the whole map; segment-based locking pre-Java 8, finer-grained CAS-based approach in Java 8+.

**Modern Java (8–21)**
- **Streams API** — a lazy, functional pipeline (`filter → map → collect`); nothing actually executes until a terminal operation (`collect`, `forEach`, `reduce`) is called — this laziness is what lets you chain operations without materializing an intermediate list at every step.
- **`Optional`** — models "value may be absent" explicitly in the type system; correct usage is `orElse`/`orElseThrow`/`map`/`flatMap` chains, not calling `.get()` unconditionally (which just reintroduces the null-pointer risk `Optional` was meant to prevent) — and avoid using `Optional` as a field type or method parameter, it's meant for return types.
- **Records** — `record Point(int x, int y) {}` auto-generates a constructor, `equals`, `hashCode`, and `toString` — the correct default for immutable data carriers, replacing a huge amount of manual boilerplate (and bug surface) that hand-written value classes used to have.
- **Sealed classes** — restrict exactly which classes may extend/implement a type, enabling the compiler to verify a `switch` over that hierarchy is exhaustive.
- **Pattern matching** — `if (obj instanceof String s)` binds `s` directly, no manual cast; switch expressions with `yield` and pattern matching make exhaustive, type-safe branching far more concise.
- **Virtual threads (Java 21)** — lightweight threads managed by the JVM rather than the OS, letting you write simple blocking-style code for I/O-bound work while still getting massive concurrency, instead of needing complex reactive/async code to avoid exhausting OS threads.

**Java Concurrency**
- `synchronized` — intrinsic lock on an object or method, the simplest mutual-exclusion mechanism.
- `volatile` — guarantees visibility of a variable's latest value across threads (no CPU-register caching of that variable), without providing mutual exclusion — solves a *different* problem than `synchronized` and is often misunderstood as a lighter-weight substitute for it, which it is not.
- `AtomicInteger`/`AtomicReference` — lock-free thread-safe operations using CAS (Compare-And-Swap) at the hardware level.
- `ExecutorService`/`ThreadPoolExecutor` — manage a bounded pool of reusable threads instead of spawning raw `Thread` objects per task; configure core/max pool size, work queue, and rejection policy deliberately, don't accept defaults blindly.
- `CompletableFuture` — composable async computation; chain, combine, and coordinate multiple async operations without manually managing callbacks.
- `ReentrantLock`/`ReadWriteLock` — more flexible than `synchronized` (tryLock with timeout, fairness policies, separate read/write locking).
- Structured concurrency (Java 21) — group related concurrent subtasks so that a failure in one automatically cancels the others, instead of leaking orphaned tasks.

**Error Handling**
- Checked vs unchecked exceptions — checked exceptions must be declared/handled at compile time (the ongoing Java community debate is whether this actually helps or just encourages exception-swallowing); unchecked (`RuntimeException`) don't require declaration.
- Custom exception hierarchies — model your own domain's failure modes explicitly instead of throwing generic exceptions everywhere.
- Try-with-resources — automatic, guaranteed resource closing (`AutoCloseable`), eliminating a whole class of "forgot to close the stream/connection" bugs.

### Under the Hood — What Generics Actually Compile To: Bridge Methods
Type erasure (above) is usually taught as "generic type info disappears at runtime" and left there. The more interesting question is: if `List<String>` and `List<Integer>` really are the same class at runtime, how does method overriding involving generics still work correctly? The answer is a compiler-generated mechanism most Java developers never see directly.

- **The problem** — suppose `class IntBox implements Comparable<Integer>` and overrides `compareTo(Integer other)`. After erasure, `Comparable`'s interface method is really `compareTo(Object other)` at the bytecode level (erasure replaces `T` with `Object`, or with the bound if one was declared). `IntBox`'s `compareTo(Integer other)` has a genuinely different method signature than `compareTo(Object other)` — by the JVM's normal rules, that's an overload, not an override, which would silently break polymorphism.
- **The fix — synthetic bridge methods** — the compiler generates an additional, hidden method on `IntBox`: `compareTo(Object other)` that casts `other` to `Integer` and delegates to your real `compareTo(Integer other)`. This bridge method is what actually satisfies the interface at the bytecode level; it's marked `synthetic` and `bridge` in the class file and is invisible in your source code and in normal IDE navigation.
- **Why this matters** — it's the mechanical reason generics + overriding "just works" despite erasure, and it's also why reflection over a class's declared methods sometimes surfaces a mysterious extra method with the erased signature that you never wrote.

### Practice
1. Write a method with an upper-bounded wildcard parameter (`List<? extends Animal>`) and attempt to add an element to it — observe the compile error, and explain in your own words exactly why the compiler can't allow it.
2. Convert a hand-written immutable value class (with manual constructor, `equals`, `hashCode`, `toString`) into a `record` and confirm the generated behavior matches.
3. Write a virtual-threads version of a program that makes 1,000 concurrent blocking I/O calls (or simulate with `Thread.sleep`), and compare thread/resource usage against doing the same with a traditional bounded `ExecutorService` thread pool.
4. Deliberately misuse `volatile` where mutual exclusion (not just visibility) was actually required, reproduce the resulting race condition, then fix it with `synchronized`/`AtomicInteger` and explain why `volatile` alone wasn't enough.
5. Write a class implementing `Comparable<Integer>`, compile it, and run `javap -p -c YourClass.class` — find the synthetic `bridge` method the compiler generated for `compareTo(Object)` and confirm it just casts and delegates to your real `compareTo(Integer)`.

### Answer This
- Why can't you safely add to a `List<? extends Animal>`, but you can safely read from it as an `Animal`?
- Why does type erasure mean `List<String>` and `List<Integer>` are the same class at runtime?
- What specific problem does `volatile` solve, and what problem does it *not* solve (that `synchronized` does)?
- Why do virtual threads let you write simple blocking-style code and still get high concurrency for I/O-bound work?
- Why does the compiler need to generate a synthetic bridge method for a class implementing `Comparable<Integer>`, and what would break if it didn't?

---

## 5B.2 — Spring Boot Core Concepts

**Tier: ⏸️ Skip For Now (deferred — see the Track A/B note at the top of this phase)**

### What to Learn
- **Dependency Injection & IoC** — Inversion of Control means the framework (not your code) creates and wires object graphs; the Spring IoC container holds every managed bean and its lifecycle.
- **Bean scopes** — singleton (one shared instance per application context — the default, and correct for stateless services), prototype (a new instance every time it's requested), request/session (web-specific scopes tied to an HTTP request or session).
- **Constructor injection, always** — makes dependencies explicit and required (a class literally cannot be constructed without them), enables immutability (`final` fields), and makes unit testing trivial (just call the constructor with test doubles — no framework needed). **Field injection** (`@Autowired` directly on a field) hides dependencies and makes a class impossible to instantiate correctly outside a Spring context, which actively hurts testability — avoid it.
- **Auto-configuration** — Spring Boot inspects the classpath and automatically wires beans it finds evidence for (a `DataSource` if a JDBC driver is present, Redis config if the Redis client is on the classpath) — genuinely useful, but you should be able to explain *what* got auto-configured and why, not treat it as unexplainable magic.
- **Stereotype annotations** — `@Component` (generic managed bean), `@Service` (business logic layer — semantically the same as `@Component` but documents intent), `@Repository` (data access layer — also enables Spring's exception translation for persistence exceptions), `@Controller`/`@RestController` (web layer).

**Spring Web (REST)**
- `@RestController` combines `@Controller` + `@ResponseBody` (every method's return value is serialized directly into the HTTP response body, instead of being resolved as a view name).
- `@GetMapping`/`@PostMapping`/`@PutMapping`/`@DeleteMapping`/`@PatchMapping` — HTTP-method-specific shortcuts over the more generic `@RequestMapping`.
- `@PathVariable`, `@RequestParam`, `@RequestBody` — binding different parts of an incoming request to method parameters.
- Validation — `@Valid` on a `@RequestBody` parameter triggers Bean Validation annotations (`@NotNull`, `@Size`, `@Email`, etc.) on the DTO *before* your handler method body even runs.
- Exception handling — `@ControllerAdvice` + `@ExceptionHandler` centralizes error handling across all controllers instead of repeating try/catch in every method; `ProblemDetail` (RFC 9457) is Spring 6's built-in structured error response format.
- **DTOs, never entities** — never return a JPA entity directly from a controller; a DTO is a deliberate boundary that decouples your API's public shape from your persistence model's internal shape (they *will* diverge over time, and coupling them means every DB schema change becomes an API breaking change).
- **MapStruct** — generates entity↔DTO mapping code at compile time, eliminating hand-written, error-prone, repetitive mapper boilerplate.
- Pagination — `Pageable` parameter + `Page<T>` return type gives you paginated results, total counts, and sort handling with minimal code.
- API versioning — URL versioning (`/v1/...` — explicit, simple, cacheable) vs header versioning (cleaner URLs, less visible/discoverable) — URL versioning is the more common practical default.

### Under the Hood — How the ApplicationContext Is Actually Built, and How `@Transactional` Actually Works
This is the direct analogue of the NestJS "how does `@Injectable()` actually resolve dependencies" deep-dive in 5A.3 — same underlying question (how does the framework build the object graph and intercept method calls), answered with Java's tools instead of TypeScript's.

- **Component scanning → `BeanDefinition`s, not beans** — on startup, Spring scans the classpath for `@Component`/`@Service`/`@Repository`/`@Controller`-annotated classes and, for each one, creates a `BeanDefinition` — metadata describing *how* to build that bean (its class, scope, constructor, dependencies) — without actually instantiating anything yet. The `ApplicationContext` only starts constructing real objects afterward, walking the dependency graph described by these definitions.
- **Why Spring doesn't need Nest's `reflect-metadata` trick** — the `BeanFactory` resolves each bean's constructor dependencies using **Java reflection directly** (`Constructor.getParameterTypes()`), because Java, unlike compiled JavaScript, never erases a constructor parameter's declared type at the bytecode level — the type information Nest has to manufacture via `emitDecoratorMetadata` is something the JVM already gives Spring for free.
- **The circular dependency problem, and why constructor injection can hit it** — if bean `A`'s constructor needs `B`, and `B`'s constructor needs `A`, there is no valid order to construct either one first — Spring throws `BeanCurrentlyInCreationException` rather than deadlocking or guessing. (Spring *can* work around circular dependencies for **singleton** beans using field/setter injection, via an early-reference cache — but this is a workaround for a design smell, not something to rely on; a real circular dependency between two services is a sign they should be refactored, not injected around.)
- **`@Transactional` is not special JVM magic — it's a dynamic proxy** — when Spring sees a bean with a `@Transactional` method, it doesn't modify your class's bytecode. Instead, at startup it wraps your real bean inside a **proxy** object and registers the proxy — not your original object — in the application context. Two proxy strategies: a **JDK dynamic proxy** (`java.lang.reflect.Proxy`) if your class implements an interface, generating a proxy that implements the same interface and forwards calls; or a **CGLIB proxy** (a runtime-generated subclass) if there's no interface to proxy. Either way, calling the method through the proxy runs the transaction-management code (begin transaction → invoke your real method → commit/rollback) *around* your actual method body — this is the exact same Proxy-pattern mechanism named in Phase 3.3, applied by the framework instead of by you.
- **The classic self-invocation gotcha, explained mechanically** — if a `@Transactional` method calls *another* `@Transactional` method on `this` from within the same class, that second call goes directly to the real object's method (`this.otherMethod()`), completely bypassing the proxy — because `this` inside your own class refers to the real instance, not the proxy wrapping it. The transaction annotation on that inner call is silently ignored. This is one of the most common real Spring bugs, and it's now a fact you can *derive* from the proxy mechanism instead of memorizing as a gotcha.

### Practice
1. Build one endpoint using constructor injection exclusively, then try to unit test the controller/service by just calling `new` on it with mock dependencies — no Spring context needed at all — and confirm how straightforward that is compared to a field-injected equivalent.
2. Trigger a validation failure on a `@Valid @RequestBody` DTO and confirm, via a debugger breakpoint, that your handler method body never actually executes — Spring rejects the request before that.
3. Build a global `@ControllerAdvice` that converts at least three different exception types into a consistent JSON error shape (matching the error envelope from Phase 7 of the master roadmap).
4. Set up MapStruct for one entity↔DTO pair and compare the generated mapper against what you'd have hand-written.
5. Inject a `@Service` bean somewhere and print `yourBean.getClass().getName()` — if the class has no interface, confirm it's actually a CGLIB-generated subclass (something like `UserService$$SpringCGLIB$$0`), not your literal class, and explain why.
6. **Reproduce the self-invocation bug on purpose** — write a service with method `a()` calling `this.b()`, where `b()` is `@Transactional`, call `a()` from outside, and (with logging or a failing assertion inside a forced rollback) confirm `b()`'s transaction never actually applies. Then fix it the standard way (move `b()` into a separate injected bean and call it through that bean instead) and confirm the transaction now applies correctly.
7. **Build a minimal transactional proxy from scratch** using `java.lang.reflect.Proxy` — given a plain interface and a real implementation, write an `InvocationHandler` that prints `"BEGIN TX"` before calling the real method and `"COMMIT TX"` (or `"ROLLBACK TX"` on exception) after, then wrap your real object with `Proxy.newProxyInstance(...)` and call methods only through the proxy. This is a small, direct model of what Spring is actually doing for every `@Transactional` bean in your app.

### Answer This
- Why does constructor injection make a class trivially testable without Spring, while field injection doesn't?
- What specifically happens, in order, when a request hits a `@Valid @RequestBody` parameter that fails validation?
- Why should a controller never return a JPA entity directly, even if the DTO would currently look identical to it?
- Why doesn't Spring need anything like Nest's `reflect-metadata` to resolve constructor dependencies?
- What's actually sitting in the `ApplicationContext` for a `@Transactional` bean — your original object, or something else? What is that something else?
- Why does calling a `@Transactional` method on `this` from inside the same class silently skip the transaction, and what does that tell you about what a Spring proxy actually wraps?

---

## 5B.3 — Spring Data JPA

**Tier: ⏸️ Skip For Now (deferred — see the Track A/B note at the top of this phase)**

### What to Learn
- `JpaRepository<T, ID>` — gives you `save`, `findById`, `findAll`, `delete`, and more, for free, plus **derived query methods** (`findByEmailAndStatus(String email, Status status)` — Spring parses the method name itself and generates the query, no SQL/JPQL written by you).
- `@Query` — write JPQL (object-model-aware query language) or native SQL directly when derived methods can't express what you need.
- **Transactions** — `@Transactional`, and *why* propagation and isolation levels (Phase 8 of the master roadmap) matter here specifically: a method marked `@Transactional` calling another `@Transactional` method on the *same* object bypasses the proxy entirely (a classic, easy-to-hit Spring pitfall, since Spring's declarative transactions are proxy-based).
- **The N+1 problem** — lazy-loaded associations trigger one query per related entity, in a loop, instead of one batched query — the single most common Spring Boot performance bug; fixed with `JOIN FETCH` in a JPQL query or `@EntityGraph`.
- **Projections** — interface-based or DTO-based projections let a query select only the columns you actually need instead of hydrating a full entity (and its lazy associations) every time.
- **Flyway** — versioned SQL migration files, applied in order, tracked in a metadata table; the hard rule: **never edit a migration that's already been applied in any shared environment** — write a new migration instead, because Flyway detects checksum mismatches on already-applied files and will refuse to proceed.

### Practice
1. Enable Hibernate SQL logging in development, call an endpoint that loads a list of entities with a lazy `@OneToMany` association, and *count* the actual queries executed in a loop — confirm the N+1 problem happens, then fix it with `JOIN FETCH` or `@EntityGraph` and confirm the query count drops to one (or a small constant).
2. Deliberately call one `@Transactional` method from another `@Transactional` method *on the same class instance* (self-invocation) and confirm — via a failing test or logged behavior — that the second annotation is silently ignored, because there's no proxy interception for a call that never leaves the object.
3. Write a projection (interface-based) that selects only 2 of an entity's 10 fields, and confirm via SQL logging that only those 2 columns are actually selected.
4. Write two Flyway migrations. After the first is "applied" (tracked), edit it and try to run migrations again — observe Flyway's checksum-mismatch failure, and understand why that's a feature, not a bug.

### Answer This
- Concretely, what query pattern does the N+1 problem produce, and why does `JOIN FETCH` collapse it to one query?
- Why does calling a `@Transactional` method from another method on the same instance silently skip the transaction, mechanically (hint: think about proxies)?
- Why does Flyway refuse to let you edit an already-applied migration, and what should you do instead when you need to fix a mistake in one?

---

## 5B.4 — Spring Security

**Tier: ⏸️ Skip For Now (deferred — see the Track A/B note at the top of this phase)**

### What to Learn
- **Authentication vs authorization** — authentication answers "who are you" (proving identity); authorization answers "what are you allowed to do" (given a proven identity) — conflating these is a common source of real security bugs (e.g., checking only that a user is logged in, not that *this* user owns *this* resource).
- **The security filter chain** — every request passes through a configurable chain of filters before it ever reaches your controller; understanding this explains where JWT parsing, CORS handling, and CSRF protection actually happen.
- **JWT authentication in Spring** — a custom filter parses and validates the incoming token, then populates the `SecurityContext` so the rest of the request pipeline (and `@PreAuthorize` checks) can see the authenticated principal.
- **`@PreAuthorize("hasRole('ADMIN')")`** — method-level authorization, evaluated before the method body runs, using Spring Expression Language for more complex rules (e.g., checking resource ownership, not just role).
- **CORS configuration** — configure a `CorsConfigurationSource` bean explicitly rather than using `@CrossOrigin` scattered across controllers, which is easy to misconfigure permissively (e.g., `allowedOrigins("*")` combined with credentials) in a way that reintroduces the exact vulnerability CORS exists to prevent.
- **Password encoding** — `BCryptPasswordEncoder` or `Argon2PasswordEncoder`; never MD5/SHA-1 for passwords (they're fast, which is exactly the wrong property for password hashing — you *want* it to be slow, to make brute-forcing expensive).
- **OAuth2 Resource Server** — validate JWTs issued by an external identity provider (Auth0, Okta, Cognito) instead of implementing your own token issuance.

### Practice
1. Build a `@PreAuthorize` rule that checks not just role but actual resource ownership (e.g., "is this user the owner of this order"), and write a test proving a non-owner admin-role-less user is correctly denied even with a valid token.
2. Deliberately misconfigure CORS with `allowedOrigins("*")` alongside `allowCredentials(true)`, observe that browsers/Spring actually reject this combination outright, and understand why that specific combination is treated as inherently unsafe.
3. Write a custom filter that extracts and validates a JWT, sets the `SecurityContext`, and confirm a request without a valid token is rejected *before* it reaches your controller — verify this with a breakpoint that never gets hit.

### Answer This
- Concretely, where in the request lifecycle does JWT validation happen in a typical Spring Security setup, and why does that matter for what code runs after it?
- Why is checking "is this user authenticated" not sufficient for authorization — what specific bug class does that miss?
- Why is `allowedOrigins("*")` combined with `allowCredentials(true)` treated as invalid/unsafe, rather than just permissive?

---

## 5B.5 — Spring Boot Production Features

**Tier: ⏸️ Skip For Now (deferred — see the Track A/B note at the top of this phase)**

### What to Learn
- **Profiles** — `application-dev.yml`/`application-prod.yml`, activated via `SPRING_PROFILES_ACTIVE`, for environment-specific configuration without code branching.
- **Actuator** — exposes operational endpoints (`/actuator/health`, `/actuator/metrics`, `/actuator/info`) for monitoring and orchestration systems (like Kubernetes health checks) to query — know which endpoints are safe to expose publicly and which need to be locked down.
- **Micrometer** — a metrics facade that lets your application code stay vendor-neutral while shipping metrics to Prometheus, Datadog, or CloudWatch depending on configuration.
- **Structured JSON logging with MDC correlation IDs** — attach a unique ID to every log line for a given request (via Mapped Diagnostic Context) so that, in a system handling concurrent requests, you can filter logs down to exactly one request's full trace across every layer it touched.
- **Graceful shutdown** (`server.shutdown=graceful`) — on receiving `SIGTERM` (Phase 1.3), stop accepting new requests but let in-flight requests finish before the process actually exits — critical for zero-downtime deploys, since without it, a rolling deployment can drop real in-flight user requests.

### Practice
1. Configure `dev` and `prod` profiles with genuinely different settings (e.g., different logging verbosity, different datasource) and confirm switching `SPRING_PROFILES_ACTIVE` changes behavior without touching code.
2. Add MDC-based correlation IDs to your logging setup, fire two concurrent requests, and confirm you can cleanly filter the combined log output down to just one request's lines.
3. Enable graceful shutdown, start a slow endpoint, send `SIGTERM` to the running process mid-request, and confirm the in-flight request completes successfully instead of being cut off.

### Answer This
- Why does exposing `/actuator/*` endpoints without any access restriction pose a real security risk in production?
- What specific problem does a correlation ID solve that plain per-line logging, without it, doesn't?
- Why does graceful shutdown matter specifically for zero-downtime rolling deployments?

---

## 5B.6 — Async & Background Jobs

**Tier: ⏸️ Skip For Now (deferred — see the Track A/B note at the top of this phase)**

### What to Learn
- `@Async` (with `@EnableAsync`) — runs a method on a separate thread pool instead of the calling thread; the caller gets a `Future`/`CompletableFuture` (or fires-and-forgets, for `void` methods) instead of blocking.
- `@Scheduled` — cron-like recurring jobs defined directly in application code (`@Scheduled(cron = "0 0 * * * *")`).
- **RabbitMQ + Spring AMQP** — `@RabbitListener` for consuming, `AmqpTemplate.convertAndSend()` for publishing; the producer/consumer relationship decouples the two sides completely (the consumer can be down, and messages simply wait).
- **Dead letter queues** — where messages go after repeatedly failing to process, so they can be inspected and manually retried instead of being silently lost or retried forever.
- **Idempotency** — every message/job handler must produce the same correct end state whether it runs once or is accidentally retried multiple times (which *will* happen in any real distributed system using at-least-once delivery) — this is a design requirement, not an edge case to handle "if it comes up."

### Practice
1. Build one `@Async` method and confirm, by logging thread names, that it genuinely runs on a different thread than the caller.
2. Set up a RabbitMQ producer/consumer pair locally (via Docker Compose), stop the consumer, publish several messages, confirm they queue up, then restart the consumer and confirm they all get processed.
3. Configure a dead letter queue, force a handler to fail repeatedly on a specific message, and confirm that message ends up in the DLQ instead of being retried forever or silently dropped.
4. Write a job handler that is genuinely idempotent (e.g., using an idempotency key stored in the DB to detect and skip a duplicate), then deliberately deliver the same message twice and confirm the end state is correct either way.

### Answer This
- Why must every message handler in a real distributed system be designed to tolerate being invoked more than once for the same logical event?
- What's the actual purpose of a dead letter queue, as opposed to just retrying forever or dropping a failed message?

---

## 5B.7 — Testing Spring Boot

**Tier: ⏸️ Skip For Now (deferred — see the Track A/B note at the top of this phase)**

### What to Learn
- `@SpringBootTest` — loads the full application context; the most realistic but also the slowest and heaviest test type — use it sparingly, for genuine end-to-end integration checks.
- `@WebMvcTest` — loads only the web layer, giving you a fast test with `MockMvc` for controller-level behavior (request mapping, validation, serialization) without booting the whole app.
- `@DataJpaTest` — loads only the JPA layer; pairs with **Testcontainers** to run tests against a real PostgreSQL instance in Docker instead of an in-memory substitute, avoiding the classic "works on H2, breaks on real Postgres" gap caused by subtle SQL dialect differences.
- `@MockBean` — replaces a real Spring-managed bean with a Mockito mock, scoped to the test's application context.
- `TestRestTemplate`/`MockMvc` — HTTP-layer testing, either against a real running embedded server or purely in-process.
- **Testcontainers** — spin up real PostgreSQL/Redis/RabbitMQ in Docker for the duration of a test suite, then tear them down — this is the concrete answer to "how do I test database-dependent code without either mocking away the database entirely or maintaining a shared test database."
- JUnit 5 (`@Test`, `@ParameterizedTest`, `@BeforeEach`, `@Nested` for organizing related test groups) and Mockito (`@Mock`, `@InjectMocks`, `when/thenReturn/thenThrow`, `ArgumentCaptor` for inspecting exactly what a mocked dependency was called with, `verify` for confirming an interaction happened).

### Practice
1. Write the same feature test three ways — `@WebMvcTest` with mocked service layer, `@DataJpaTest` with Testcontainers for the repository layer, and one full `@SpringBootTest` — and time all three; use this to build real intuition for *why* the test pyramid (Phase 11) prefers fewer, slower, full-context tests.
2. Set up Testcontainers for PostgreSQL, write a repository test against it, then intentionally use a Postgres-specific feature (e.g., a JSONB column or `ILIKE`) that would silently behave differently (or fail) on H2 — confirming why Testcontainers is worth the extra setup cost.
3. Use `ArgumentCaptor` to confirm a service called a mocked email sender with the exact expected arguments, not just that it was called at all.

### Answer This
- Why is `@SpringBootTest` the slowest test type, and why should it be used sparingly rather than as the default for everything?
- What specific class of bug does testing against Testcontainers' real PostgreSQL catch that testing against H2 wouldn't?
- What does `ArgumentCaptor` let you verify that a plain `verify(mock).method()` call doesn't?

---

## 5B.8 — Backend Production Concerns

**Tier: ⏸️ Skip For Now (deferred — see the Track A/B note at the top of this phase)**

### What to Learn
- **Webhooks** — verify every incoming webhook's signature (commonly HMAC-SHA256) before trusting its payload at all; respond `200` quickly and process the actual work asynchronously (many webhook senders will retry aggressively if your endpoint is slow or doesn't respond fast); design every handler idempotently, since retries are the normal case, not the exception; maintain a dead-letter path for payloads your app ultimately fails to process. Real examples: Stripe payment events, GitHub push events, Twilio delivery status.
- **Payments** — never store raw card data (PCI-DSS baseline — use a provider's tokenization instead); Stripe's PaymentIntents flow, idempotency keys on every payment-creation call (so a network retry can never accidentally double-charge), and webhook-driven confirmation (never trust a client-side "success" callback alone, since it can be spoofed or simply never fire due to a dropped connection); subscription billing concerns (trials, proration, cancellation timing); chargebacks and refunds as first-class flows, not afterthoughts.
- **Full-text search** — start with PostgreSQL's `tsvector`/`tsquery` + a GIN index + `ts_rank` for relevance ordering; only reach for Elasticsearch once you've actually outgrown what Postgres full-text search can do (which is further than most projects assume).
- **Multi-tenancy** — row-level security (RLS, enforced at the database level, hardest to accidentally bypass) vs schema-per-tenant vs database-per-tenant vs fully shared-nothing infrastructure — each is a real, different tradeoff in isolation, operational cost, and blast radius of a bug; whichever you choose, the iron rule is: **every single query must filter by `tenant_id`**, and RLS is attractive precisely because it enforces that at the database layer instead of trusting every developer to remember it in every query.
- **Rate limiting** — token bucket algorithm implemented in Redis, applied per-user and per-IP with different limits (a single authenticated user's legitimate high usage shouldn't be punished by an IP-wide limit meant to stop abuse from anonymous traffic).

### Practice
1. Build a webhook receiver that verifies an HMAC signature, rejects anything that fails verification, responds `200` immediately, and processes the actual business logic on a background thread/queue — then simulate the same webhook payload being delivered twice and confirm your handler's idempotency.
2. Implement a Stripe PaymentIntent flow using an idempotency key, then deliberately retry the same payment-creation request (simulating a network retry) and confirm it does not create a duplicate charge.
3. Set up PostgreSQL full-text search (`tsvector`/`tsquery`/GIN index) on a real dataset and compare the query plan/performance against a naive `LIKE '%term%'` search.
4. Implement row-level security on a multi-tenant table in PostgreSQL and confirm, by connecting as a different tenant context, that a query genuinely cannot see another tenant's rows — even if the application code "forgets" to add a `WHERE tenant_id = ?` clause.

### Answer This
- Why must every webhook handler be idempotent, given how most webhook providers actually behave on failure/timeout?
- Why does an idempotency key prevent a network-retry from double-charging a customer, mechanically?
- Why is row-level security considered a stronger multi-tenancy guarantee than "remembering to filter by `tenant_id` in every query"?

---

### Track B Capstone

> **PROJECT:** Build a complete REST API with Spring Boot: user management with JWT + refresh token authentication, full CRUD via Spring Data JPA + PostgreSQL (with at least one deliberately-introduced-then-fixed N+1 query, documented with before/after query counts), Flyway migrations (including one migration added *after* the fact, never editing a previously-applied one), an async background job via RabbitMQ with a dead letter queue and a genuinely idempotent handler, a Stripe integration with signature-verified webhooks and idempotency keys, PostgreSQL full-text search on one resource, and complete test coverage using the right test type for each layer (`@WebMvcTest`, `@DataJpaTest` + Testcontainers, and a small number of full `@SpringBootTest`s) — finished off with Swagger/OpenAPI documentation. **Rebuild the exact same domain you already shipped in the Track A Capstone** — same resources, same auth model, same job/queue behavior — so the comparison between the two stacks is direct and concrete, not from memory.

---

# Phase 6 — Mobile Engineering (React Native)

**Phase tier: ⏸️ Skip For Now — entirely, unless your goal shifts from fullstack *web* to include mobile**

> You already know React (Phase 4). React Native is React targeting a different rendering surface — the transition is deliberately smooth, but mobile has its own real constraints (slower CPUs, different navigation model, app store review) that don't exist on the web.

## 6.1 — Why React Native After React Web

**Tier: ⏸️ Skip For Now**

### What to Learn
- The mental model transfers almost entirely: components, hooks, props, state.
- Tooling transfers too: TypeScript, ESLint, TanStack Query, Zustand — none of this needs to be relearned.
- One backend (your Spring Boot API from Phase 5) can serve both the web app and the mobile app with no duplication.
- One codebase targets both iOS and Android, rather than maintaining two fully separate native apps.

### Answer This
- Concretely, which parts of your React web knowledge transfer directly to React Native, and which parts (rendering targets, navigation, styling mechanism) genuinely don't?

---

## 6.2 — React Native Fundamentals

**Tier: ⏸️ Skip For Now**

### What to Learn
- **How RN actually works** — your JS runs on a JS thread; native UI/platform APIs run on the native side; the **bridge** (older architecture) serialized messages asynchronously between the two, while **JSI** (the new architecture) allows more direct, synchronous communication — you don't need to implement either, but understanding that this boundary exists explains real RN performance characteristics that don't have a web equivalent.
- **Native components, not web ones** — `<View>` not `<div>`, `<Text>` not `<p>`/`<span>` (critically: *all* text must be inside a `<Text>` component — unlike the web, you cannot have bare text as a sibling of other elements), `<TouchableOpacity>`/`<Pressable>` for tappable elements, `<ScrollView>`, `<FlatList>`, `<Image>`.
- **`StyleSheet.create()`** — CSS-like property names, but it is not CSS — there's no cascade, no inheritance of most properties, and no external stylesheet; every component's styles are explicit, local objects.
- **Flexbox is the default layout model** — but critically, the default `flexDirection` is **`column`**, not `row` as on the web — a very common source of "why is my layout stacked weirdly" bugs for web developers new to RN.
- **Platform-specific code** — `Platform.OS === 'ios' ? x : y`, or separate `.ios.tsx`/`.android.tsx` files that RN automatically picks based on the target platform.
- **Debugging** — Flipper, React Native Debugger, or Chrome DevTools attached to the running JS context.
- **Hermes** — RN's own optimized JavaScript engine (rather than relying on the platform's system JS engine), tuned specifically for faster app startup and lower memory use on mobile hardware.

### Practice
1. Build one screen using only core RN components, deliberately get the default `flexDirection: 'column'` "wrong" for a row-like layout, observe the stacked result, then fix it explicitly with `flexDirection: 'row'` — internalize that this default differs from the web.
2. Write one component with platform-specific styling using `Platform.OS`, and a second using separate `.ios.tsx`/`.android.tsx` files — compare which approach felt cleaner for that specific case.
3. Attach a debugger (Flipper or Chrome) to a running RN app and set a real breakpoint inside a component's render logic.

### Answer This
- Why can't you place bare text as a sibling of other elements the way you can on the web?
- Why is the default flex direction different between React Native and CSS on the web, and what bug does forgetting this cause?
- At a conceptual level, what problem does the bridge/JSI boundary exist to solve, and why does it matter for performance?

---

## 6.3 — Expo

**Tier: ⏸️ Skip For Now**

### What to Learn
- **Expo (managed workflow) vs bare React Native** — start with Expo; eject (or use a bare/dev-client workflow) only once you need a native module genuinely unsupported by the Expo SDK.
- **Expo Router** — file-based routing, directly analogous to Next.js's App Router (`app/index.tsx`, `app/(tabs)/home.tsx`) — if you know Next.js routing conventions, this will feel immediately familiar.
- **Expo SDK** — camera, location, notifications, filesystem, secure storage — all accessible without writing a single line of native iOS/Android code.
- **EAS Build** — builds real iOS and Android binaries in the cloud, meaning you don't need a Mac to produce an iOS build.
- **EAS Submit** — automates the actual app store submission process.
- **OTA (Over-The-Air) updates** — push a JS-only update directly to installed apps without going through app store review at all — genuinely useful for fast-follow bug fixes, but understand its limits (native code changes still require a real store release).

### Practice
1. Scaffold a new Expo project and get it running on a real physical device via Expo Go within the same session.
2. Set up Expo Router with at least a tab layout and a nested stack, mirroring a Next.js App Router structure you're already comfortable with.
3. Ship a trivial JS-only change via an OTA update and confirm it reaches an already-installed app without a new store submission.

### Answer This
- Why would you choose to eject from Expo's managed workflow, specifically — what triggers that decision?
- What kinds of changes can an OTA update ship, and what kinds absolutely require a full store release?

---

## 6.4 — Navigation

**Tier: ⏸️ Skip For Now**

### What to Learn
- Expo Router's file-based routing model — directory structure directly determines the navigation structure (stacks, tabs, nested layouts).
- Deep linking — universal links (iOS) and app links (Android) let an external URL open directly to a specific in-app screen.
- URL schemes — a custom scheme (`myapp://...`) that lets other apps or a browser open your app directly.
- Navigation state — params passed between screens, and programmatic navigation (navigating in response to code, not just a user tap).

### Practice
1. Set up a deep link that opens the app directly to a specific detail screen with a parameter, and confirm it works both from a fresh app launch (cold start) and while the app is already running (warm start) — these are genuinely different code paths worth testing separately.

### Answer This
- Why do cold-start and warm-start deep linking need to be tested separately, rather than assuming one implies the other works?

---

## 6.5 — Styling for Mobile

**Tier: ⏸️ Skip For Now**

### What to Learn
- **NativeWind** — brings Tailwind's utility-class syntax to React Native, so the mental model and even the class names carry over directly from web Tailwind work.
- **`StyleSheet.create`** — still relevant for performance-sensitive style objects even alongside NativeWind.
- **Safe area handling** — `useSafeAreaInsets` accounts for notches, home indicators, and status bars, which have no web equivalent at all — skipping this causes real content to render underneath physical device chrome.
- **Platform design guidelines** — iOS Human Interface Guidelines vs Android Material Design — genuinely different conventions for common UI patterns (e.g., where a back button lives, how tab bars behave).

### Practice
1. Build one screen with content that would visually collide with a notch/home-indicator if safe area insets were ignored; confirm the collision, then fix it with `useSafeAreaInsets` and confirm it renders correctly on both a notched and a non-notched device/simulator.

### Answer This
- Why does safe-area handling have no real analogue in web development, and what specifically breaks if you skip it?

---

## 6.6 — Mobile Performance

**Tier: ⏸️ Skip For Now**

### What to Learn
- **`FlatList` vs `ScrollView`** — `FlatList` virtualizes (only renders items currently near the visible viewport), while `ScrollView` renders every child immediately regardless of visibility — for any list that could grow large, `FlatList` is the only correct default.
- **`InteractionManager.runAfterInteractions`** — defers non-urgent, potentially heavy work until running animations/transitions have finished, keeping those animations smooth.
- **`useCallback`/`React.memo` matter more here than on web** — mobile CPUs are meaningfully slower and more power-constrained than a typical development machine or even a typical user's laptop, so unnecessary re-renders are more visibly costly.
- **`expo-image`** — provides built-in caching and better performance characteristics than the core `Image` component for most real apps.
- **Avoid inline styles and anonymous functions in render** — both create a brand-new reference on every render, which can silently defeat `React.memo` on child components that receive them as props.

### Practice
1. Render the same large list (1,000+ items) once with `ScrollView` and once with `FlatList`; compare scroll performance and memory usage directly on a real device or a reasonably-throttled simulator.
2. Find one inline anonymous function or inline style object passed as a prop to a `React.memo`-wrapped child, and confirm — by adding a render-count log inside the child — that the memoization isn't actually preventing re-renders because the prop reference changes every time. Fix it with `useCallback`/a hoisted style object and confirm the re-render count drops.

### Answer This
- Why does `ScrollView` become a real performance problem for a long list while `FlatList` doesn't, mechanically?
- Why does an inline anonymous function passed as a prop defeat `React.memo`, even if the function's actual logic never changes?

---

## 6.7 — Device APIs & Storage

**Tier: ⏸️ Skip For Now**

### What to Learn
- Camera (`expo-camera`), location (`expo-location` — and the real distinction between foreground and background location permissions, which have very different privacy implications and review scrutiny), push notifications (Expo Notifications routed through FCM for Android and APNs for iOS), biometrics (`expo-local-authentication` for Face ID/Touch ID).
- **MMKV vs AsyncStorage** — MMKV is dramatically faster for local key-value storage; AsyncStorage is the older, slower, still-common default.
- **`expo-secure-store`** — the correct place for tokens/credentials specifically, because it uses the platform's actual secure storage (Keychain on iOS, Keystore on Android) — never store auth tokens in AsyncStorage or MMKV, which are not designed for secrets.

### Practice
1. Store a JWT in AsyncStorage and separately in `expo-secure-store`; explain, concretely, what protection the secure store provides that AsyncStorage does not (hint: think about what has access to each on a compromised or jailbroken/rooted device).
2. Request and use foreground location, then attempt background location and observe the additional, more explicit permission prompt/requirements — understand why platforms treat these differently.

### Answer This
- Why is `expo-secure-store` the correct place for auth tokens specifically, rather than AsyncStorage or MMKV?
- Why do iOS/Android treat background location permission as meaningfully more sensitive than foreground location?

---

## 6.8 — Networking & Offline

**Tier: ⏸️ Skip For Now**

### What to Learn
- The same TanStack Query patterns from Phase 4 apply directly on mobile, including `persistQueryClient` paired with AsyncStorage to persist the cache across app restarts for offline access.
- **Optimistic updates** — update the UI immediately assuming success, then reconcile (or roll back) once the real server response arrives — this matters even more on mobile, where network conditions are far less reliable than a typical web user's connection.
- `@react-native-community/netinfo` — detect current network connectivity and adjust app behavior (e.g., queue mutations instead of failing them outright when offline).
- **Certificate pinning** — hardening against man-in-the-middle attacks by trusting only a specific, known certificate instead of any certificate signed by a trusted CA — more relevant on mobile, where the app itself (not a browser with its own security model) is directly responsible for this.

### Practice
1. Set up `persistQueryClient` with AsyncStorage, then force-quit and relaunch the app while offline, and confirm previously-fetched data still renders from cache instead of showing a blank/error state.
2. Implement one optimistic mutation (e.g., "like" a post), simulate a server failure, and confirm the UI correctly rolls back to the pre-optimistic state instead of showing an incorrect success state.

### Answer This
- Why does offline resilience matter more, as a default design concern, on mobile than on a typical web app?
- What specifically should happen to the UI if an optimistic update's real server request ultimately fails?

---

## 6.9 — App Store Deployment

**Tier: ⏸️ Skip For Now**

### What to Learn
- **App signing** — iOS certificates and provisioning profiles, Android keystores — EAS Build can manage these for you, but you should understand what they actually represent (proof that a build genuinely came from you, the registered developer).
- **TestFlight** (iOS) — internal and external beta distribution before a public release.
- **Google Play Console** tracks — internal testing → closed testing → production, a staged rollout model.
- **App Store review guideline awareness** — knowing common, avoidable rejection reasons (e.g., broken links, placeholder content, missing privacy policy for data collection) saves real review-cycle time.
- **Semantic versioning** applied to `version` (user-facing) and `buildNumber`/`versionCode` (must strictly increase per submission, independent of the user-facing version string).
- **Fastlane** — automates repetitive release tasks (screenshots, metadata upload, submission) across both platforms.

### Practice
1. Take a real (even simple) app through the full pipeline: EAS Build a real binary, submit to TestFlight internal testing (iOS) and/or an internal testing track (Android), and get it running on a real device via that distribution channel rather than Expo Go.

### Answer This
- Why must `buildNumber`/`versionCode` strictly increase on every submission, independent of the semantic `version` string?
- Name two common, easily-avoidable App Store rejection reasons.

---

## Phase 6 Capstone

> **PROJECT:** Build a mobile companion app for your Phase 5 Master Project backend, using React Native + Expo. It must include: authentication with tokens stored in `expo-secure-store` (never AsyncStorage), offline support via TanStack Query's persisted cache, at least one optimistic update with a deliberately-tested rollback path, push notifications, `FlatList` for any real list view (with a documented comparison against `ScrollView` for the same data), and a deployment through EAS Build to TestFlight internal testing and/or a Google Play internal testing track.

---

# Phase 7 — API Design

**Phase tier: 🎯 Mostly Master — REST fully Master; GraphQL/gRPC/etc. Know Only**

> An API is a product with real users (other developers, including future-you), versions, and documentation. Design it like someone else is depending on it — because they are, or soon will be.

## 7.1 — REST Principles (Deep)

**Tier: 🎯 Master**

### What to Learn
- **Resources, not actions** — `GET /orders/123`, never `POST /getOrder` — the URL names a *thing*, the HTTP method describes the *action* on it.
- **Method semantics, precisely** — GET (safe: no server-side side effects at all; idempotent: repeating it changes nothing further), POST (creates a resource, generally *not* idempotent — calling it twice can create two resources, unless you deliberately add idempotency keys), PUT (idempotent — replaces a resource wholesale; calling it 1 or 100 times leaves the same end state), PATCH (partial update, idempotent only if you design it carefully — a PATCH that says "increment by 1" is *not* idempotent, one that says "set to 5" is), DELETE (idempotent — deleting an already-deleted resource is still "deleted").
- **Status codes used correctly**, not just defaulting to 200/500 — the specific code communicates real semantic information to any client, human or automated, that a generic code doesn't.
- **Statelessness** — every request must contain everything needed to process it (auth token, all relevant parameters); the server holds no per-client session state between requests, which is precisely what enables horizontal scaling (Phase 10 of the master roadmap) without sticky sessions.
- **Uniform interface** — consistent naming and consistent error format across every endpoint, so a client only has to learn your conventions once, not per-endpoint.
- **Naming convention** — plural nouns, lowercase, hyphenated (`/user-profiles`, not `/UserProfile` or `/getUserProfiles`).

### Practice
1. Take a badly-designed action-oriented API (`POST /getUser`, `POST /updateUserEmail`) and redesign it as proper resource-oriented REST.
2. Write a PATCH endpoint two ways — one that's accidentally non-idempotent (e.g., "increment the counter") and one that's genuinely idempotent (e.g., "set the counter to this value") — and articulate, precisely, why the first violates PATCH's expected semantics.

### Answer This
- Why is POST generally not idempotent while PUT is, by design?
- What specifically does "stateless" mean for a REST API, and why does it matter for horizontal scaling?
- Why can a PATCH endpoint accidentally violate idempotency in a way a PUT endpoint structurally can't?

---

## 7.2 — REST Best Practices

**Tier: 🎯 Master**

### What to Learn
- **Versioning** — URL versioning (`/api/v1/...`) is explicit, human-readable, and trivially cacheable per-version; it's the more common practical default over header-based versioning.
- **Pagination** — cursor-based (`WHERE id > :lastSeenId ORDER BY id LIMIT 20`) for large/growing datasets, since OFFSET-based pagination degrades badly at scale (Phase 8/12 of the master roadmap — OFFSET still has to scan and discard every preceding row); OFFSET is acceptable only for small, bounded datasets.
- **Filtering/sorting via query params** — `?status=active&sort=createdAt:desc` — keep the convention consistent across every endpoint that supports it.
- **Consistent error envelope**, applied everywhere, never improvised per-endpoint:
```json
{ "error": { "code": "VALIDATION_ERROR", "message": "Email is invalid", "field": "email", "requestId": "abc-123" } }
```
- **Never leak internal details** in an error response — no stack traces, no raw database error messages, no internal object IDs a client shouldn't know exist.
- **`requestId`/correlation ID on every response** — ties directly back to Phase 5's correlation-ID logging (5A.6 now, 5B.5 later): the same ID that traces a request through your logs should be visible to the client so a bug report can reference it directly.
- **Idempotency keys** — a client-supplied unique key on a POST request that lets the server recognize and safely no-op a retried request instead of creating a duplicate resource (directly relevant to Phase 5's payment example, 5A.9 now / 5B.8 later).

### Practice
1. Implement cursor-based pagination on a genuinely large table and compare its performance at a "deep" page against OFFSET-based pagination on the same query, at the same depth.
2. Standardize error responses across at least 3 different endpoints (a validation error, a not-found error, and an unexpected server error) into the exact same envelope shape.
3. Add idempotency key support to one write endpoint and prove, with a test, that submitting the same request twice with the same key produces only one resource, not two.

### Answer This
- Why does OFFSET pagination get slower as you page deeper, while cursor-based pagination doesn't?
- What specific harm can leaking a stack trace or raw DB error message in an API response actually cause?
- How does an idempotency key let a server distinguish "this is a retry of a request I already handled" from "this is a genuinely new request"?

---

## 7.3 — WebSockets

**Tier: 🎯 Master**

### What to Learn
- **When to use WebSockets** — genuine bidirectional, low-latency communication (chat, live notifications, collaborative editing) — not just "anything that updates in real time" (Server-Sent Events, 7.6, is often a simpler correct choice for one-directional server-push cases).
- **Handshake** — starts as a normal HTTP request that upgrades via the `Upgrade: websocket` header — it's not a separate protocol from scratch, it piggybacks on HTTP just for the initial negotiation.
- **Connection lifecycle** — open → messages flow in both directions → periodic ping/pong heartbeats (to detect a silently-dead connection that TCP alone won't reliably notice) → close.
- **Authentication** — a WebSocket handshake can't carry custom headers as easily as a normal HTTP request in every client environment, so auth is typically passed as a token in the initial query string or as the very first message after connecting, rather than in a header.
- **Scaling** — WebSocket connections are inherently stateful (a specific server instance holds a specific open connection), which breaks the simple stateless-horizontal-scaling model from 7.1; the fix is either sticky sessions (route a client's reconnects back to the same server instance) or a Redis pub/sub backplane (any server instance can publish a message, and it fans out to whichever instance actually holds the relevant connection).
- **Spring WebSocket + STOMP** — `@MessageMapping` for handling incoming messages, `SimpMessagingTemplate.convertAndSend()` for pushing messages out.

### Practice
1. Build a minimal real-time chat feature using WebSockets, including a heartbeat mechanism, and deliberately kill the network mid-session to confirm the heartbeat actually detects the dead connection (rather than the app appearing to hang silently).
2. Explain, in writing, why a naive horizontally-scaled WebSocket deployment (multiple server instances behind a load balancer, no sticky sessions or pub/sub backplane) would silently fail to deliver messages between users connected to different instances — then fix it with a Redis pub/sub backplane.

### Answer This
- Why is a WebSocket connection's initial handshake still, technically, an HTTP request?
- Why do WebSocket connections need a heartbeat mechanism when TCP already has its own keep-alive concept?
- Why does horizontal scaling break naively for WebSockets in a way it doesn't for stateless REST endpoints?

---

## 7.4 — API Security

**Tier: 🎯 Master**

### What to Learn
- Authentication required on every non-public endpoint, with no silent exceptions.
- Rate limiting — per-user and per-IP, with different limits for each (an authenticated user's legitimate traffic shouldn't be capped by an anonymous-abuse-focused IP limit, and vice versa) — implemented in Redis for distributed enforcement across multiple server instances.
- Input validation at the API boundary — validate everything arriving from outside, never trust it because "the frontend already validates it" (the frontend is not a security boundary; anyone can call your API directly).
- Output filtering — never return fields the caller isn't authorized to see, even if it would be "convenient" to just return the full entity.
- API keys — sufficient entropy, rotation support, scope restriction (a key should be able to do only what it needs to, not everything your API supports), and never embedded in frontend/client code where it's trivially extractable.
- CORS — an explicit allow-list of origins, never a wildcard combined with credentialed requests (see 5.4).

### Practice
1. Build rate limiting in Redis with genuinely separate per-user and per-IP limits, and write a test proving a single abusive IP can't exhaust a legitimate authenticated user's quota, and vice versa.
2. Deliberately skip server-side validation on one endpoint (trusting "the frontend already checks this"), then bypass the frontend entirely with a direct `curl`/Postman request carrying invalid data, and confirm the bad data gets through — then add server-side validation and confirm it's now rejected regardless of how the request arrives.

### Answer This
- Why is frontend-only input validation not a real security boundary, concretely?
- Why should per-user and per-IP rate limits be tracked and enforced separately rather than as one combined limit?

---

## 7.5 — API Documentation

**Tier: 🎯 Master**

### What to Learn
- **OpenAPI 3.0** — the industry-standard way to describe every endpoint, parameter, request/response schema, and auth requirement in a machine-readable spec.
- **API-first design** — write the spec *before* the implementation; the spec becomes the actual contract that both frontend and backend teams build against independently and in parallel, rather than the frontend waiting on a finished backend (or reverse-engineering behavior from whatever the backend happens to currently do).
- **Springdoc OpenAPI** — auto-generates a live Swagger UI directly from your Spring Boot annotations (`@Operation`, `@ApiResponse`, `@Schema`), keeping docs from silently drifting out of sync with the actual code.
- **Documenting error responses** — every endpoint's spec should include its realistic 4xx/5xx responses, not just the happy-path 200.

### Practice
1. Write a complete OpenAPI spec for one resource *before* implementing any of its controller code, then implement strictly against that spec.
2. Set up Springdoc so your Swagger UI is generated directly from annotations on real controller code, and confirm that changing a DTO field is immediately reflected in the generated docs without any manual doc update.

### Answer This
- What real coordination problem does API-first design solve for a frontend and backend team working in parallel?
- Why does generating docs directly from code annotations (Springdoc) avoid a failure mode that hand-written, separately-maintained documentation doesn't?

---

## 7.6 — Beyond REST

**Tier: 👀 Know Only**

> Ecosystem awareness — know when each of these is the better tool, even if REST remains your default.

### What to Learn
- **GraphQL** — a schema plus resolvers, letting clients request exactly the fields they need across related resources in a single round trip; the **N+1 problem** here is even easier to accidentally trigger than in a REST+ORM setup, since a naive resolver can fire one query per related field per result row — **DataLoader** batches and caches those resolver calls within a single request to collapse them back down; disable introspection in production (it exposes your entire schema to anyone); add complexity limiting so a maliciously deep/broad query can't be used to overload your server. GraphQL wins when many different clients need different slices of the same underlying data; REST wins for simple CRUD and when strong HTTP-level caching matters.
- **gRPC** — Protocol Buffers (a compact binary format) over HTTP/2, with generated client/server code per language and native support for all four streaming modes (unary, server-streaming, client-streaming, bidirectional) — the standard choice for internal service-to-service communication where both ends are under your control and raw performance/type-safety matter more than human-readability.
- **Server-Sent Events (SSE)** — a simpler alternative to WebSockets for the common case where only the server needs to push updates (not the client); built-in automatic reconnection via `EventSource`, and `Last-Event-ID` lets a reconnecting client resume exactly where it left off — genuinely the right choice more often than people default to WebSockets for.
- **tRPC** — end-to-end type-safe APIs without a separate schema-generation step, by sharing TypeScript types directly between a Node backend and a frontend in the same monorepo — not applicable to a Java backend, but worth knowing if you ever work in a full-TypeScript stack.
- **AsyncAPI** — the OpenAPI-equivalent for event-driven systems, documenting Kafka topics/WebSocket events/AMQP queues as a real, versioned contract instead of tribal knowledge.
- **Contract testing (Pact)** — consumer-driven contracts let a frontend/consumer team define exactly what they expect from an API, and that contract gets verified automatically against the real backend in CI — catching a breaking API change *before* it reaches a shared environment, not after.

### Practice
1. Implement the same small domain (e.g., a blog with posts and comments) as both a REST API and a GraphQL API, and add DataLoader to the GraphQL version after first reproducing its N+1 problem without it.
2. Build one feature using SSE instead of a WebSocket for a one-directional "live update" case, and write down concretely why it required less code and less operational complexity for that specific case.

### Answer This
- Why can GraphQL's N+1 problem be *easier* to accidentally trigger than a typical REST+ORM N+1, and what does DataLoader do to fix it?
- When is SSE the objectively simpler correct choice over a WebSocket, and why?
- What real failure does consumer-driven contract testing (Pact) catch that a passing backend test suite alone wouldn't?

---

## Phase 7 Capstone

> **PROJECT:** Design the complete API spec (OpenAPI) for your Master Project's core domain *before* writing any controller code. Implement it as REST, following every convention in 7.1/7.2 (cursor pagination, a consistent error envelope, idempotency keys on at least one write endpoint). Add a real-time feature using either WebSockets or SSE — whichever is the objectively correct choice for that specific feature, and be ready to justify the choice. Optionally, implement one resource as GraphQL as well, deliberately reproduce its N+1 problem, then fix it with DataLoader, and write a short comparison of the REST and GraphQL versions of the same domain.
