# Part 0 — Foundation Reset (Detailed)
### Companion deep-dive to `Master_Senior_Fullstack_Roadmap.md` → Phase 0

> This file expands **Phase 0 — Foundation Reset** from the master roadmap into full depth: every sub-skill broken down into what to learn, why it matters, how to practice it, and what you should be able to answer afterward.
>
> **Why Phase 0 exists at all:** almost every "senior" gap you'll hit later — a slow debugging session, a broken deploy, a misread stack trace, a CSS bug that eats an afternoon — traces back to a shaky foundation in one of the topics below. Fixing these now is cheap. Fixing them later, under pressure, mid-project, is expensive. Nothing here is optional "review" — treat it as load-bearing.
>
> Go in order. Each section assumes the previous one. Don't skip to Phase 1 until you can answer every "Answer This" question below without looking it up.

---

## Table of Contents

- [0.1 — Your Operating Environment (Linux as Daily Driver)](#01--your-operating-environment-linux-as-daily-driver)
- [0.2 — Terminal Mastery](#02--terminal-mastery)
- [0.3 — IDE Mastery](#03--ide-mastery)
- [0.4 — Chrome DevTools Mastery](#04--chrome-devtools-mastery)
- [0.5 — Git Internals & Discipline](#05--git-internals--discipline)
- [0.6 — HTML Semantics](#06--html-semantics)
- [0.7 — CSS Fundamentals (Before Any Framework)](#07--css-fundamentals-before-any-framework)
- [0.8 — How HTTP Really Works](#08--how-http-really-works)
- [0.9 — Environment & Habits Checklist](#09--environment--habits-checklist)
- [Phase 0 Capstone](#phase-0-capstone)

---

## 0.1 — Your Operating Environment (Linux as Daily Driver)

> You don't need Linux to write good code, but you need to stop treating your OS as a black box. Most engineers who are weak here are weak because they've never had to be strong — the GUI always saved them. Remove the GUI safety net deliberately.

### If you're on Windows right now
You have three realistic paths — pick one, don't agonize over it:
1. **WSL2 (recommended to start)** — a real Linux kernel inside Windows, near-zero setup friction, keeps your Windows apps. Install via `wsl --install -d Ubuntu` in an elevated PowerShell.
2. **Dual boot** — a real Ubuntu install alongside Windows. More commitment, more realism (real filesystem, real hardware interaction).
3. **A cloud/VM box** — a small VPS (DigitalOcean, AWS EC2 free tier) or local VM (VirtualBox/UTM) you SSH into daily.

Whichever you pick, the rule is the same: **do your daily development work, git, and terminal tasks from inside Linux**, not from PowerShell muscle memory.

### What to Learn
- **Filesystem hierarchy** — `/etc` (system config), `/var` (logs, variable data), `/usr` (installed software), `/home` (your files), `/tmp` (ephemeral), `/proc` (a virtual filesystem exposing running kernel/process state — `cat /proc/cpuinfo`, `/proc/meminfo`), `/sys` (kernel device/driver info).
- **Users, groups, permissions** — the `rwx` bit triplet for owner/group/other; `chmod 755` vs `chmod +x`; `chown user:group file`; `sudo` vs root login; `umask` and default permission calculation.
- **Shells** — `bash` vs `zsh`; shell config files (`.bashrc`, `.zshrc`, `.bash_profile`) and when each loads; environment variables (`export VAR=value`); `PATH` and why "command not found" usually means a `PATH` problem, not a missing binary.
- **Process management** — `ps aux`, `top`/`htop`, `kill <pid>` vs `kill -9 <pid>` (SIGTERM vs SIGKILL), `killall`, `systemctl status/start/stop/enable` for services.
- **Package management** — `apt update && apt upgrade`, `apt install`, `snap`; the difference between a package manager install and building from source; why you should never blindly `sudo` a script from the internet without reading it first.
- **Dotfiles** — the convention of prefixing config files with `.` so they're hidden by default; keeping your own dotfiles in a Git repo is a real, common senior-engineer habit.

### Practice
1. Fresh Ubuntu (or WSL2 Ubuntu) install. Do not install a GUI file manager or GUI git client.
2. Write a shell script (`setup.sh`) that, run on a brand-new machine, installs your entire toolchain: Java, Node, Git, Docker, your editor's CLI — from zero to "ready to code" with one command.
3. Create a dotfiles repo. Track `.bashrc`/`.zshrc`, `.gitconfig`, and your editor settings in it. Clone it onto a second machine (or a fresh VM) and confirm it reproduces your setup.
4. Break something on purpose: chmod a script to remove execute permission, try to run it, understand the error, fix it.

### Answer This
- What's the actual difference between a process and a thread at the OS level (not just "a thread is lighter")?
- What does `chmod 755` mean, bit by bit, in binary?
- How do you make a script run automatically on machine startup?
- Why does editing `~/.bashrc` not affect a terminal window that's already open?

---

## 0.2 — Terminal Mastery

> The terminal is the one interface that hasn't meaningfully changed in 40 years — and won't. Every hour invested here compounds for the rest of your career.

### Core Tools to Learn Deeply

| Tool | What to Master |
|------|-----------------|
| `grep` | Pattern search; `-r` recursive; `-i` case-insensitive; `-n` line numbers; `-v` invert match; `-E` extended regex; `-c` count matches |
| `awk` | Field-based processing (`awk '{print $1}'`); filtering rows (`awk '$3 > 100'`); field separators (`-F","`) |
| `sed` | Stream editing; find-and-replace (`sed 's/foo/bar/g'`); in-place edits (`sed -i`); deleting lines matching a pattern |
| `find` | By name (`-name "*.java"`), type (`-type f`/`-type d`), size (`-size +10M`), modification time (`-mtime -1`); combine with `-exec` |
| `curl` | GET/POST, custom headers (`-H`), JSON bodies (`-d`), verbose mode (`-v`) to see raw request/response, following redirects (`-L`), saving output (`-o`) |
| `ssh` | Key-based auth, `~/.ssh/config` host aliases, agent forwarding, port forwarding (`-L`/`-R`), bastion/jump hosts (`-J`) |
| `tmux` | Sessions that survive disconnects, panes/splits, detach/reattach (`tmux attach`), a `.tmux.conf` you actually understand |
| `htop` | Sorting by CPU/memory, killing processes interactively, understanding load average |
| Piping & redirection | `\|`, `>`, `>>`, `2>&1`, `/dev/null`, `xargs`, command substitution `$(...)` |

### Practice
1. Take a real (or generated) web server access log. Use `grep` + `awk` + `sed` to extract the count of 5xx errors per endpoint, sorted descending.
2. Write a `curl` command that POSTs a JSON body with a Bearer auth header to a local API, and pretty-prints the JSON response (pipe into `jq` if available).
3. Set up `~/.ssh/config` with a named alias for a remote box so `ssh myserver` just works, no flags.
4. Start a `tmux` session, split it into 3 panes (editor, server, logs), detach, close the terminal entirely, reopen the terminal, and reattach — confirm nothing died.
5. Chain 4+ commands with pipes to answer a real question about your own filesystem (e.g., "what are my 10 largest files modified in the last week?").

### Answer This
- How do you find all `.java` files modified in the last 24 hours?
- How do you redirect both stdout and stderr into the same file?
- What's the difference between `>` and `>>`?
- Why would a `curl` request work in verbose mode but fail silently without `-v`?

---

## 0.3 — IDE Mastery

> Your editor is where you'll spend 80% of your working hours. Mouse-dependency here is a permanent tax on your speed — pay it down once.

### What to Learn (IntelliJ IDEA — primary, for Java/Spring work)
- **Navigation without the mouse** — Go to Class/File/Symbol (`Ctrl+N`/`Ctrl+Shift+N`), Go to Declaration, Find Usages, Recent Files, Navigate Back/Forward.
- **Editing** — multi-cursor editing, column selection, Extend/Shrink Selection, Duplicate Line, Move Line Up/Down, Surround With.
- **Refactoring built-ins** — Rename (`Shift+F6`), Extract Method/Variable/Constant/Field, Inline, Change Signature, Safe Delete — these are the same refactorings from Phase 3 of the master roadmap, except IntelliJ automates the mechanical part so you can focus on the design decision.
- **Live Templates** — typing `psvm`, `sout`, `iter` and expanding boilerplate instantly; learn to create your own.
- **Debugger** — breakpoints, conditional breakpoints, "Evaluate Expression" mid-debug, watches, step into/over/out, the difference between "Force Step Into" and regular step-into (library vs your code).
- **Run configurations** — understanding what actually gets executed (main class, VM options, environment variables, working directory) instead of just clicking the green triangle blindly.
- **Version control panel** — viewing diffs, staging hunks, resolving conflicts, browsing history — all without leaving the IDE, while still understanding what Git commands this maps to underneath (see 0.5).
- **Inspections** — turn on all warnings, understand each one before disabling it, use "Analyze → Inspect Code" on a whole project to find dead code and smells.

### If your primary editor is VS Code instead (frontend/TypeScript work)
The same categories apply — Command Palette (`Ctrl+Shift+P`) mastery, multi-cursor, Go to Definition/References, integrated debugger with `launch.json`, built-in Git panel, and extensions that matter for this stack: ESLint, Prettier, Tailwind CSS IntelliSense, Error Lens.

### Practice
1. For one full day, physically disconnect or ignore your mouse while coding. Force yourself to find the keyboard shortcut instead of clicking.
2. Debug a real bug using only breakpoints, watches, and "Evaluate Expression" — no `System.out.println`/`console.log` statements.
3. Set up 3 custom Live Templates (IntelliJ) or snippets (VS Code) for boilerplate you personally write often.
4. Run "Inspect Code" (or ESLint with all rules on) across an existing project and fix 10 real warnings.

### Answer This
- What is the actual difference between "Step Into" and "Step Into (Force)" in a debugger?
- Why is a conditional breakpoint often better than a manually-placed `if` + print statement?
- What does "Safe Delete" actually check before letting you delete a method?

---

## 0.4 — Chrome DevTools Mastery

> Every frontend bug, every "why is this slow," and every "why is this request failing" question gets answered here first — before you touch code.

### What to Learn

**Elements panel**
- Live-editing the DOM and CSS to test hypotheses before writing code.
- The "Computed" tab to see final resolved styles after cascade/specificity/inheritance.
- Force element state (`:hover`, `:focus`) to debug interaction styles.

**Console**
- `console.log/table/group/time/trace`; using the console as a live REPL against the running page; `$0` (last inspected element), `$$('selector')` (querySelectorAll shortcut).

**Network tab**
- Reading the waterfall — DNS lookup, initial connection, TLS, TTFB (time to first byte), content download.
- Filtering by type (XHR/Fetch, JS, CSS, Img); inspecting request/response headers, payload, and preview.
- Throttling (simulate slow 3G) and offline mode.
- "Disable cache" while DevTools is open, and understanding why forgetting this causes confusing debugging sessions.

**Performance tab**
- Recording a real interaction and reading the flame chart — spotting long tasks, layout thrashing, excessive scripting.
- Distinguishing **Loading**, **Scripting**, **Rendering**, and **Painting** time in the summary.
- Correlating a janky scroll/animation with a specific function in the flame chart.

**Application tab**
- Inspecting Local Storage, Session Storage, IndexedDB, and Cookies directly.
- Service Worker status and manually unregistering/updating one.
- Cache Storage inspection.

**Memory tab**
- Taking a heap snapshot before/after an action to spot detached DOM nodes (a classic memory leak signature).
- Comparing two snapshots to see what got allocated and never freed.

**Lighthouse (built into DevTools)**
- Running an audit and reading Performance, Accessibility, Best Practices, and SEO scores with actionable line items.

### Practice
1. Open any real website. Use the Network tab to find the single largest asset being downloaded and figure out why it's that size.
2. Record a Performance trace while scrolling a heavy page. Identify one specific function causing jank.
3. Take a heap snapshot, click a button that should clean up after itself (e.g., closes a modal), take a second snapshot, and check whether the modal's DOM nodes were actually freed.
4. Throttle your network to "Slow 3G" and observe how your own project degrades. Fix the worst offender.
5. Run Lighthouse against a project you built and fix the top 3 flagged issues.

### Answer This
- What's the difference between TTFB and "DOMContentLoaded" in the Network waterfall?
- What does a detached DOM node in a heap snapshot actually indicate?
- Why would disabling cache in DevTools change your debugging results?

---

## 0.5 — Git Internals & Discipline

> Most developers use Git as a magic "save" button. Understanding what it actually stores turns every scary situation (bad rebase, lost commit, conflict hell) into a solvable puzzle instead of a panic.

### What to Learn
- **What Git actually stores** — not diffs. Blobs (file contents), trees (directory structure), commits (snapshot + metadata + parent pointer(s)), and refs (branches/tags are just pointers to a commit hash). A branch is *not* a container of commits — it's a movable label.
- **The staging area (index)** — the actual third state between working tree and last commit; why `git add` matters beyond "select files."
- **`merge` vs `rebase`** — merge creates a new commit with two parents, preserving history exactly as it happened; rebase replays your commits on top of a new base, producing a linear (but rewritten) history. Know the real tradeoff: rebase rewrites commit hashes — never rebase commits that are already pushed and shared.
- **`git reset --soft / --mixed / --hard`** — precisely what happens to HEAD, the index, and the working tree at each level. This is the single most misunderstood Git command.
- **`git reflog`** — your safety net; almost nothing in Git is truly unrecoverable as long as you haven't garbage-collected, because the reflog remembers where HEAD has been.
- **`git bisect`** — binary search through commit history to find the exact commit that introduced a regression; understand it as an application of binary search (Phase 2 of the master roadmap) to a real engineering problem.
- **`git stash`** — save/restore work-in-progress; `stash pop` vs `stash apply`; stashing untracked files with `-u`.
- **Interactive rebase (`git rebase -i HEAD~N`)** — squash, reorder, reword, and edit commits before they're shared; cleaning up a messy WIP history into a coherent, reviewable one.
- **Conventional commits** — `feat:`, `fix:`, `refactor:`, `chore:`, `docs:`, `test:`; why this convention enables automated changelogs and semantic versioning.
- **Commit messages** — explain *why*, not *what* (the diff already shows what). A good commit message answers "why did this change need to happen" for someone with zero context, six months from now — possibly you.
- **Branching strategies** — Git Flow (feature/develop/release/hotfix — heavier, less common now), Trunk-Based Development (everyone commits to `main` frequently, short-lived branches), GitHub Flow (simple, PR-based — the default for most teams today).

### Practice
1. Make 5 messy, "wip", "fix typo", "actually fix it" commits on a branch. Use interactive rebase to squash them into one clean, well-described commit before it would be merged.
2. Intentionally create a merge conflict (edit the same line on two branches) and resolve it properly — don't just accept "theirs" or "ours" blindly, understand what each side intended.
3. Delete a commit with `git reset --hard`, panic, then recover it using `git reflog` and `git reset --hard <sha>`. Do this on purpose, once, so it's never scary again.
4. Use `git bisect` on a repo with an intentionally-introduced bug across several commits — find the exact culprit commit via binary search instead of manual inspection.
5. Adopt conventional commits for one week on a real project and generate a changelog from the commit history.

### Answer This
- What's the actual difference between `git merge` and `git rebase`, in terms of the resulting commit graph?
- What happens to HEAD, the index, and the working tree with `git reset --hard HEAD~1`? How do you undo it?
- What is a detached HEAD state, and how do you get out of one safely?
- Why should you never rebase commits that have already been pushed to a shared branch?

---

## 0.6 — HTML Semantics

> If you've been reaching for `<div>` for everything, this is the single highest-leverage half-day you can spend on frontend fundamentals.

### What to Learn
- **Why semantics matter** — accessibility (screen readers announce structure), SEO (search engines weight semantic content differently), and free browser behavior (a `<button>` is focusable, keyboard-activatable, and announced as a button with zero extra code — a `<div onClick>` gives you none of that for free).
- **Block vs inline elements** — how each participates in layout by default, and why this still matters even though Flexbox/Grid override display behavior constantly.
- **Structural elements** — `<header>`, `<nav>`, `<main>` (exactly one per page), `<article>` (independently distributable content), `<section>` (thematic grouping, usually with a heading), `<aside>`, `<footer>`. Know precisely when `<article>` is correct and `<section>` is not, and vice versa.
- **Forms done right** — every `<input>` type (`email`, `tel`, `date`, `number`, etc. — each gives you free validation and the right mobile keyboard); `<label for="id">` association (clicking the label focuses the input — free, and required for accessibility); `required`, `pattern`; `<fieldset>`/`<legend>` for grouping related inputs.
- **Tables for data, never for layout** — `<thead>`, `<tbody>`, `<th scope="col">` — `scope` is what lets a screen reader announce which column/row a cell belongs to.
- **Images** — `alt` text that's actually descriptive (not "image123.png"), `loading="lazy"` for below-the-fold images, `srcset` for responsive image delivery.
- **Meta tags** — `viewport` (mobile rendering), `charset`, Open Graph tags (`og:title`, `og:image`) for how links preview when shared on social platforms.
- **How the browser parses HTML into the DOM** — tokenization → tree construction; understanding that malformed HTML gets "corrected" by the browser's error-recovery rules, which is why validating your HTML matters even though it usually still renders.

### Practice
1. Take any page you previously built with `<div>` for everything and rebuild it with correct semantic elements — no CSS changes, structure only.
2. Test that rebuilt page with a real screen reader (NVDA on Windows, free; VoiceOver on Mac, built-in). Navigate it using only the keyboard and the screen reader's shortcuts. Note every place it's confusing.
3. Build a form with proper `<label>` associations, correct `input` types, and a `<fieldset>` for a grouped set of related fields (e.g., a shipping address).
4. Validate a real page through the W3C HTML validator and fix every structural error.

### Answer This
- What's the concrete difference between `<article>` and `<section>`, and how do you decide which one applies to a blog post's comment section?
- Why does a missing `<label>` break more than just "looking a bit worse" — what actually fails for a real user?
- Why is `<button>` strictly better than `<div onClick={...}>` for anything clickable, beyond "it's more correct"?

---

## 0.7 — CSS Fundamentals (Before Any Framework)

> Tailwind, and every other CSS framework, is a shorthand for real CSS. If you don't know what the shorthand expands to, you will eventually hit a bug you cannot fix — only work around.

### What to Learn
- **The box model** — `content → padding → border → margin`; `box-sizing: content-box` (default, painful) vs `box-sizing: border-box` (width includes padding+border — set this globally, always).
- **Specificity** — the actual calculation: inline styles > IDs > classes/attributes/pseudo-classes > elements/pseudo-elements. Learn to calculate specificity for a real selector by hand, not just "vibe" which rule wins.
- **The cascade** — how specificity, source order, and `!important` interact when multiple rules target the same property; why `!important` is usually a symptom of a specificity problem, not a fix for one.
- **Inheritance** — which properties inherit by default (`color`, `font-family`, `line-height`) and which don't (`margin`, `padding`, `border`, `width`) — and why that split exists (typography inherits because it should flow through a document; box-model properties don't because every element needs its own).
- **Stacking context & `z-index`** — `z-index` only matters *within* a stacking context; a new stacking context is created by `position` + `z-index` together, `opacity < 1`, `transform`, `filter`, and several other properties — this is why a modal with `z-index: 9999` can still render behind something else if a parent creates its own stacking context first.
- **Positioning** — `static` (default, in normal flow), `relative` (offset from its own normal position, still takes up space), `absolute` (removed from flow, positioned relative to nearest positioned ancestor), `fixed` (relative to the viewport), `sticky` (hybrid — relative until a scroll threshold, then fixed).
- **Flexbox** — `flex-direction`, `justify-content` (main axis), `align-items` (cross axis), and the three growth properties: `flex-grow` (how much extra space to claim), `flex-shrink` (how much to shrink when space is tight), `flex-basis` (the starting size before grow/shrink apply).
- **CSS Grid** — `grid-template-columns`/`grid-template-areas`, `minmax()`, `auto-fit` vs `auto-fill` (the classic point of confusion: `auto-fit` collapses empty tracks, `auto-fill` keeps them).
- **Responsive design** — mobile-first media queries (`min-width` breakpoints, not `max-width`); fluid typography with `clamp(min, preferred, max)`.
- **Custom properties (CSS variables)** — `:root { --brand-color: ... }`, `var(--brand-color, fallback)`, and how they differ from Sass variables (they're live in the DOM and can be changed at runtime via JS or media queries — Sass variables are compile-time only).
- **Animations** — `transition` for state changes, `@keyframes` for multi-step animations; **only animate `transform` and `opacity`** for 60fps performance — animating `width`/`top`/`left` forces layout recalculation on every frame.

### Practice
1. Build one non-trivial responsive layout (a dashboard or a card grid) from a design mockup using **zero** frameworks — plain CSS only.
2. Deliberately break a modal's `z-index` by giving an ancestor `opacity: 0.99` or a `transform`, observe the modal disappear behind content despite a huge `z-index`, then explain exactly why before fixing it.
3. Build the same 3-column responsive layout twice: once with Flexbox, once with Grid. Write down which one was actually the right tool and why.
4. Take a design with a heading, body text, and a button. Predict — on paper, before opening DevTools — which CSS properties inherit down to nested `<span>`s and which don't. Then verify in DevTools' Computed tab.

### Answer This
- What specifically creates a new stacking context, and why does that make `z-index` "not work" in a way that looks like a bug?
- Why does `margin: auto` center a block element horizontally but not vertically, by default?
- What's the practical difference between `flex-grow` and `flex-basis` when both are set on the same element?
- Why should you only animate `transform`/`opacity` instead of `top`/`left`/`width`?

---

## 0.8 — How HTTP Really Works

> Every API you'll ever build or consume sits on top of this. If HTTP itself is fuzzy, every layer above it (REST design, caching, auth headers, CORS errors) will feel like memorized trivia instead of understood mechanics.

### What to Learn
- **HTTP as a text protocol** — a request is: request line (method + path + version), headers, a blank line, then an optional body. Seeing this raw (via `curl -v` or `nc`) demystifies "the request" from an abstract concept into literal text over a socket.
- **Method semantics** — GET (safe: no server-side side effects; idempotent: repeating it changes nothing further), POST (creates, not idempotent — calling it twice can create two resources), PUT (idempotent — replaces a resource wholesale, calling it twice has the same end state), PATCH (partial update, idempotent only if designed carefully), DELETE (idempotent).
- **Status code families** — 1xx (informational, rare), 2xx (success — know 200/201/204 precisely), 3xx (redirection — 301 permanent vs 302/307 temporary), 4xx (client error — know 400/401/403/404/409/422/429 and the real distinction between 401 "who are you" and 403 "I know who you are, and no"), 5xx (server error).
- **Key headers** — `Content-Type` (tells the receiver how to parse the body), `Authorization` (credentials, usually `Bearer <token>`), `Cache-Control` (caching directives — `no-cache` does NOT mean "don't cache," it means "revalidate before using the cached copy"), `ETag`/`If-None-Match` (conditional requests — "give me the new version only if it's different from this hash"), `Location` (where a redirect or a newly created resource lives), `Set-Cookie`.
- **Keep-alive** — why reusing a TCP connection across multiple requests avoids repeated handshake overhead, and why this matters for performance at scale.
- **HTTP/1.1 vs HTTP/2 vs HTTP/3** — HTTP/1.1 is one request in flight per connection (without pipelining, which is barely used); HTTP/2 adds multiplexing (many requests over one connection) and header compression (HPACK); HTTP/3 replaces TCP with QUIC (over UDP) to solve TCP-level head-of-line blocking entirely.
- **HTTPS/TLS** — HTTPS is HTTP running over an encrypted, authenticated TLS connection; understand this as "same protocol, different transport," not a separate protocol.
- **Cookies** — `HttpOnly` (JavaScript cannot read this cookie — mitigates XSS token theft), `Secure` (only sent over HTTPS), `SameSite` (`Strict`/`Lax`/`None` — controls whether the cookie is sent on cross-site requests, the core defense against CSRF).
- **Caching** — `Cache-Control` directives (`max-age`, `no-store`, `no-cache`, `private`/`public`), `ETag` + `If-None-Match` for revalidation, `Last-Modified` + `If-Modified-Since` as the older/simpler alternative.

### Practice
1. Run `curl -v https://example.com` and read every line of the raw request and response. Identify the request line, every header, and where the body begins.
2. Use Wireshark to capture a real HTTP request from your own browser and find the same information at the packet level.
3. Deliberately send a GET request with a body using `curl` and observe what actually happens (many servers ignore or reject it) — understand why this is a semantic violation, not a hard protocol error.
4. Trigger a 401 and a 403 from a real or toy API and articulate, out loud, the difference in what each communicates to the client.
5. Set `Cache-Control: no-cache` on a response, then `Cache-Control: no-store`, and observe the different real caching behavior in DevTools' Network tab for each.

### Answer This
- What actually happens if you send a GET request with a body?
- What is the precise difference between `401 Unauthorized` and `403 Forbidden`?
- What does `Cache-Control: no-cache` actually mean (hint: it is not "don't cache")?
- Why does HTTP/2's multiplexing reduce the need for the old "domain sharding" performance trick?

---

## 0.9 — Environment & Habits Checklist

> These aren't topics to study — they're behaviors to install. Revisit this list monthly for the first year; it should become automatic, not something you have to remember to do.

### Environment Checklist
- [ ] Daily-driving Linux (native, WSL2, or VM) for development work.
- [ ] A dotfiles repo tracking your shell config, Git config, and editor settings.
- [ ] IDE/editor configured with your own Live Templates/snippets, all inspections/lint rules enabled.
- [ ] `EXPLAIN ANALYZE` (or equivalent) is a reflex, not an afterthought, on every query you write (this becomes fully relevant in Phase 8, but the habit starts now).
- [ ] A learning journal — even two lines per session — tracking what you learned and what confused you.

### Habits Checklist
- [ ] **Read error messages fully** before searching for the fix. The stack trace almost always contains the answer.
- [ ] **Measure before optimizing.** Never guess where a bottleneck is — profile it.
- [ ] **Write before you code.** Pseudocode, a design note, or a quick diagram before the first line of implementation.
- [ ] **Reproduce before you fix.** If you can't reliably reproduce a bug, you can't verify you actually fixed it.
- [ ] **Change one variable at a time** when debugging.
- [ ] **Explain the why, not the what**, in commit messages and comments — the diff already shows what changed.

---

## Phase 0 Capstone

> One project that forces you to touch every section above.

> **PROJECT — "Zero-to-Running":** Starting from a bare Linux install (WSL2, VM, or dual boot), write a single provisioning script that installs your entire toolchain (Git, a JDK, Node, Docker, your editor's CLI tools). Use it to clone a small existing project of yours. Configure Git properly (your name/email, a useful `.gitignore`, aliases you'll actually use). Open the project in your IDE and navigate it for 15 minutes using **only the keyboard**. Rebuild one real page's markup with correct semantic HTML and validate it. Rebuild its layout in plain CSS, no framework, and intentionally trigger and then fix a stacking-context bug. Serve it locally and inspect the raw request/response with `curl -v`, then in the Network and Performance tabs of Chrome DevTools. Make a deliberate mess of Git history on a feature branch, then clean it up with interactive rebase before treating it as "done."
>
> If you can do all of that comfortably, Phase 0 is complete — move on to **Phase 1 — Computer Science Fundamentals** in `Master_Senior_Fullstack_Roadmap.md`.
