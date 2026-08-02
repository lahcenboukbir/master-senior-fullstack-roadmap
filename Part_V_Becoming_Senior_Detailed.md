# Part V — Becoming Senior (Detailed)
### Companion deep-dive to `Master_Senior_Fullstack_Roadmap.md` → Phases 15, 16, 17

> This file expands **Phase 15 (The Senior Mindset & Problem Solving)**, **Phase 16 (Communication & Collaboration)**, and **Phase 17 (Engineering Practices & Methodologies)** into full depth.
>
> **Why this part is genuinely different from Parts I–IV:** everything before this point was about what you know. This part is about how you operate — and it's the part that has no finish line. You can "complete" learning B-Trees. You never finish getting better at debugging under pressure, writing a design doc a stakeholder actually reads, or knowing when a piece of technical debt is prudent versus reckless. Treat the exercises here as habits to install, not boxes to check once. Revisit this file periodically — the same section will mean something different to you after a year of real experience than it does on a first read.
>
> There's also a real risk in this part that doesn't exist in the others: it's easy to *read* about first-principles thinking or code review culture and feel like you've absorbed it, without ever actually changing a single behavior. The "Practice" sections here are deliberately built around real, recurring work situations — not toy exercises — because that's the only place this material actually gets learned.

---

## Table of Contents

**Phase 15 — The Senior Mindset & Problem Solving**
- 🎯 [15.1 — Developer vs Engineer](#151--developer-vs-engineer)
- 🎯 [15.2 — First Principles Thinking](#152--first-principles-thinking)
- 🎯 [15.3 — Debugging: The Scientific Method](#153--debugging-the-scientific-method)
- 🎯 [15.4 — Thinking in Tradeoffs](#154--thinking-in-tradeoffs)
- 🎯 [15.5 — Estimation](#155--estimation)
- 🎯 [15.6 — Core Daily Habits](#156--core-daily-habits)
- [Phase 15 Capstone](#phase-15-capstone)

**Phase 16 — Communication & Collaboration**
- 🎯 [16.1 — Why Communication Is a Technical Skill](#161--why-communication-is-a-technical-skill)
- 🎯 [16.2 — Written Communication](#162--written-communication)
- 🎯 [16.3 — Code Reviews](#163--code-reviews)
- 🎯 [16.4 — Meetings & Working in a Team](#164--meetings--working-in-a-team)
- 🎯 [16.5 — Communicating Technical Concepts](#165--communicating-technical-concepts)
- 🎯 [16.6 — Async-First Communication](#166--async-first-communication)
- [Phase 16 Capstone](#phase-16-capstone)

**Phase 17 — Engineering Practices & Methodologies**
- 🎯 [17.1 — Agile / Scrum / Kanban / XP](#171--agile--scrum--kanban--xp)
- 👀 [17.2 — Lean / Lean Startup](#172--lean--lean-startup)
- 👀 [17.3 — Domain-Driven Design (DDD) — Deep Dive](#173--domain-driven-design-ddd--deep-dive)
- 🎯 [17.4 — The 12-Factor App](#174--the-12-factor-app)
- 🎯 [17.5 — Technical Debt Quadrant](#175--technical-debt-quadrant)
- 👀 [17.6 — Spikes, OKRs & FinOps](#176--spikes-okrs--finops)
- [Phase 17 Capstone](#phase-17-capstone)

---

# Phase 15 — The Senior Mindset & Problem Solving

**Phase tier: 🎯 Master — all of it, free and high-leverage**

> Code is the output. Thinking is the work. The best engineers aren't the fastest typists — they ask the best questions.

## 15.1 — Developer vs Engineer

**Tier: 🎯 Master**

### What to Learn

| Developer | Engineer |
|-----------|----------|
| Solves the problem given | Questions whether it is the right problem |
| Writes code that works | Writes code that lasts |
| Thinks in features | Thinks in systems |
| Reacts to bugs | Anticipates failure |
| Asks "how?" | Asks "why?" and "what if?" |
| Finishes the task | Owns the outcome |

- This table isn't a value judgment on "developers" as people — everyone starts on the left column for topics they're new to, including a senior engineer encountering an unfamiliar domain. It's a description of a *mode of operating*, and the goal is recognizing which mode you're in on any given task, and deliberately shifting to the right column when the situation calls for it (a genuinely novel, ambiguous, or high-stakes problem) rather than defaulting to the left column out of habit.
- The most important single shift is **"owns the outcome"** — a developer's job is often considered done when the ticket is closed; an engineer's job is done when the actual problem is solved, which sometimes means going back after "done" because the outcome didn't hold up.

### Practice
1. Pick one recent task you completed. Honestly re-answer, for that specific task: did you question whether it was the right problem before starting, or did you take the ticket description at face value? Would the outcome have been different if you had?
2. For your next non-trivial ticket, before writing any code, write down one sentence answering "what is this actually trying to achieve, and is the requested approach the best way to get there?" — even if your answer ends up being "yes, proceed as specified," make the check deliberate rather than skipped.

### Answer This
- Why is this table not really about people, but about a mode of operating that anyone can be in on a given task?
- What's the practical difference between "the ticket is closed" and "the outcome held up"?

---

## 15.2 — First Principles Thinking

**Tier: 🎯 Master**

### What to Learn
- **Break every problem down to its fundamental truths** — rather than reasoning by analogy ("we always add a cache when things are slow") or by convention ("this is just how it's done here"), ask what is actually, verifiably true about *this specific* situation.
- **The Five Whys** — ask "why?" repeatedly (traditionally five times, though the exact count isn't the point) until you reach an actual root cause, rather than stopping at the first plausible-sounding explanation.
- **Question every assumption before writing a line of code** — including assumptions so embedded in a request that they don't even feel like assumptions ("we need to add pagination" might really mean "the current experience is too slow," which pagination is only one possible fix for).
- **The concrete example from the master roadmap, worth internalizing fully:** "Our API is slow." Reasoning by analogy: everyone adds a cache when an API is slow, so add a cache. First-principles reasoning: *where, specifically,* is time actually being spent? Database? Network? Serialization? Is it slow for all users, or a specific subset? Is it slow constantly, or only under load? Measure first (Phase 12's Golden Rule), then solve the *actual* bottleneck — which might not be helped by a cache at all, and might even be made worse by adding one if the real problem is somewhere a cache can't touch.

### Practice
1. Take a recent "obvious" fix you or a teammate applied to a problem (a cache, a retry, an index, a timeout increase) and retroactively ask the Five Whys about the *original* symptom — did the fix address the actual root cause, or a plausible-looking one that happened to make the symptom go away?
2. Next time you're about to reach for a familiar "this usually works" fix, pause and write down, in one sentence, the actual measured evidence for where the problem lives — before applying the fix. If you don't have that evidence yet, get it first.

### Answer This
- Why is "everyone adds a cache when things are slow" reasoning by analogy rather than first principles, and what real risk does that carry?
- What's the actual purpose of asking "why" repeatedly — is the goal literally five iterations, or something else?

---

## 15.3 — Debugging: The Scientific Method

**Tier: 🎯 Master**

### What to Learn
1. **Observe** — determine what is *actually* happening, not what you assume is happening; this often means adding logging/instrumentation *before* forming a theory, not after.
2. **Hypothesize** — form a specific, falsifiable explanation for the observed behavior.
3. **Predict** — if the hypothesis is correct, what *else* should be true? A good hypothesis makes a prediction beyond just "the bug goes away."
4. **Test** — gather evidence that would confirm or disprove the hypothesis specifically, not just evidence that the bug is gone (which can happen for reasons unrelated to your actual hypothesis).
5. **Repeat** until you've found the actual root cause, not just a point where the symptom stopped appearing.

**Core supporting habits:**
- **Reproduce first** — if you can't reliably reproduce a bug, you cannot verify a fix; "it seems fixed" without reproduction is not evidence.
- **Change one variable at a time** — changing multiple things and then observing the bug is gone tells you nothing about *which* change actually mattered, which matters both for understanding the bug and for not accidentally reverting the real fix later.
- **Read the error message fully** — most of the actual answer is already there, in the exact line, the exact exception type, the exact stack frame — skimming past it to search online is often slower than reading it carefully first.
- **Binary-search the problem** — narrow down *where* the bug lives systematically (this is `git bisect`, Phase 0.5/13.1, applied as a mental model even without using the tool literally — e.g., commenting out half a function to see if the bug persists in the remaining half).
- **Check your assumptions specifically** — "it must be X" is usually exactly where the real bug is hiding, precisely because you've stopped questioning it.
- **Take a break when stuck** — a genuinely useful debugging technique, not a cop-out; fresh eyes catch what tired eyes, staring at the same five lines for the twentieth time, do not.

### Practice
1. Next time you hit a real, non-trivial bug, explicitly write down each of the five steps as you go — your specific hypothesis, your specific prediction, and the specific evidence you gathered — rather than debugging "by feel." Compare, afterward, how this differs from your usual process.
2. Deliberately practice "reproduce first": the next time someone reports a bug you can't immediately reproduce, resist the urge to guess-fix it; spend the time establishing a reliable reproduction *before* touching any code.
3. The next time you fix something by changing several things at once because you're not sure which one matters, go back afterward and revert them one at a time to actually identify which single change fixed it — build the discipline of knowing, not just fixing.

### Answer This
- Why does a good hypothesis need to make a prediction *beyond* "the bug will go away"?
- Why is changing multiple things at once and observing the bug disappear not the same as understanding — or even correctly fixing — the bug?
- Why is "it must be X" often exactly the place a real bug is hiding?

---

## 15.4 — Thinking in Tradeoffs

**Tier: 🎯 Master**

### What to Learn
- Every real engineering decision has a genuine cost somewhere: speed vs correctness, performance vs readability, consistency vs availability (Phase 10.3), short-term velocity vs long-term maintainability, build vs buy.
- **There are no right answers in engineering — only defensible decisions.** The skill isn't finding the option with no downside (it usually doesn't exist); it's correctly identifying what you're trading away, deciding that tradeoff is acceptable *for this specific context*, and being able to explain that reasoning to someone else later.
- A decision made without acknowledging its tradeoff isn't actually a decision — it's an unexamined default, which is a different (and riskier) thing.

### Practice
1. For one real technical decision you're currently facing (or recently made), write down explicitly: what did you gain, what did you give up, and why was that trade acceptable *in this specific context* — not "because it's best practice" as a stand-alone justification, since best practices themselves usually encode a tradeoff that may or may not apply to your situation.
2. Find one decision in an existing codebase that looks, on the surface, like it was made "just because" — and try to reverse-engineer the tradeoff that might have justified it at the time. Sometimes you'll conclude it genuinely was arbitrary; the exercise is in learning to distinguish an examined tradeoff from an unexamined default, even in someone else's old code.

### Answer This
- Why is "there's no right answer, only a defensible decision" not the same thing as "any answer is fine"?
- What's the practical difference between a decision made with an acknowledged tradeoff and one made as an unexamined default?

---

## 15.5 — Estimation

**Tier: 🎯 Master**

### What to Learn
- **Break work into small chunks and estimate the pieces, not the whole** — a large estimate is really a sum of many smaller, more reliably estimable ones; estimating the whole directly tends to hide the pieces you haven't actually thought through yet.
- **Reference class forecasting** — instead of estimating from first principles every time ("how long *should* this take"), ask "how long did similar things actually take, historically" — actual past data beats optimistic first-time reasoning almost every time.
- **Three-point estimation** — best case, likely case, worst case — communicates the real uncertainty explicitly, instead of a single number that implies false precision.
- **Track estimates vs actuals over time** — the only way to actually get better at estimating is to close the feedback loop and learn, systematically, where your specific biases tend to be (usually: underestimating integration/testing/edge-case time, not the core "happy path" implementation).
- **Raise flags early** — if a piece of work is trending over its estimate, say so as soon as you know, not near the deadline; early bad news is a manageable problem, late bad news is a crisis.

### Practice
1. For your next 5 tasks, give a three-point estimate (best/likely/worst) *before* starting, then record the actual time taken. After 5 data points, look for a pattern in the gap — are you consistently optimistic about a particular kind of work (integration, testing, "simple" UI polish)?
2. Practice raising a flag early on purpose: the next time you notice you're trending over an estimate, communicate it the same day you notice — not "when I'm more sure," and not at the deadline.

### Answer This
- Why does reference-class forecasting (using real historical data) tend to beat first-principles time estimation for familiar kinds of work?
- Why is a single-number estimate less honest, in a real sense, than a three-point range?
- Why is raising a flag early "manageable" while raising it late is "a crisis" — what actually changes between those two moments?

---

## 15.6 — Core Daily Habits

**Tier: 🎯 Master**

### What to Learn
- Measure before optimizing — never guess (Phase 12's Golden Rule, restated as a personal habit, not just a performance-engineering rule).
- Name things precisely — a vague name (`data`, `handleThing`, `process`) usually reflects genuinely vague thinking about what the thing actually is; forcing yourself to name something precisely often surfaces a design problem you hadn't consciously noticed yet.
- Write before you code — pseudocode, a short design note, or even a quick diagram, before the first line of real implementation.
- Ask why before how — the First Principles habit (15.2), made into a daily default rather than a special occasion.
- Seek feedback early — showing a rough draft/approach *before* it's polished lets you fail small and cheaply, rather than discovering a fundamental problem only after a large investment.
- Read the source — actual documentation, actual library code, actual RFC/spec text — not just the first summarized blog post or AI-generated explanation you find.
- The Boy Scout Rule — leave code a little cleaner than you found it, on every single PR, as a continuous, small habit rather than a rare "big cleanup" event.

### Practice
1. For one week, before writing any non-trivial function, spend 60 seconds writing its name and its intended one-sentence purpose *before* the implementation — if you can't write a clean one-sentence purpose, that's a signal worth pausing on, not skipping past.
2. Pick one PR this week and deliberately apply the Boy Scout Rule — leave one small thing (a name, a redundant comment, a slightly awkward structure) better than you found it, even though it wasn't part of your actual task.

### Answer This
- Why does struggling to name something precisely often indicate a design problem, rather than just "not being good with names"?
- Why is showing a rough, unfinished draft for early feedback often more valuable than waiting until it's polished?

---

## Phase 15 Capstone

> **PROJECT (a habit-installation exercise, not a coding project):** Over the next 10 real, non-trivial tasks in your actual work (not toy exercises), do the following for each: (1) write a one-sentence check on whether the requested approach is actually the right problem to solve (15.1); (2) if you hit a genuine bug, explicitly write down your hypothesis and prediction before testing it, rather than debugging "by feel" (15.3); (3) give a three-point estimate before starting and record the actual time after (15.5). At the end of the 10 tasks, write a short retrospective: what pattern did you notice in your own estimation bias, and what's one specific habit from this phase you're keeping going forward.

---

# Phase 16 — Communication & Collaboration

**Phase tier: 🎯 Master — all of it, free and high-leverage**

> The engineers who advance fastest make everyone around them more effective. Communication is a multiplier on every technical skill you have — a brilliant solution nobody can understand or act on has close to zero realized value.

## 16.1 — Why Communication Is a Technical Skill

**Tier: 🎯 Master**

### What to Learn
- Code is written once and read dozens of times afterward — writing it clearly *is* engineering work, not a "soft skill" bolted on separately from it.
- Every misunderstood requirement costs dramatically more to fix once it's already built than it would have cost to clarify up front — communication failures are frequently the *actual* root cause behind what gets logged as a "requirements bug" or "scope miss."

### Answer This
- Why is "communication is a soft skill, separate from real engineering" a misleading framing?

---

## 16.2 — Written Communication

**Tier: 🎯 Master**

### What to Learn
- **README** — what the project is, how to actually run it, and *why* key decisions were made — not just installation steps.
- **ADR (Architecture Decision Record)** — documents *why* a decision was made (the context, the alternatives considered, the reasoning) — not just what the final decision was; the "why" is the part that's actually valuable to a future reader who's wondering whether the decision still makes sense.
- **Design documents** — problem, proposed solution, alternatives considered, and explicit tradeoffs — written *before* building, so disagreement happens on paper (cheap) rather than in code (expensive).
- **RFC (Request for Comments)** — proposes a change and explicitly invites feedback before committing to it, for changes big enough to warrant broader input.
- **Post-mortems** — what happened, the actual root cause (not just the proximate trigger), a timeline, and concrete prevention steps — blameless by convention, since a post-mortem focused on blame produces defensive, incomplete accounts instead of an honest one.
- **Day-to-day writing** — ticket descriptions with clear acceptance criteria and context; PR descriptions covering what changed, why, and how to test it; commit messages that explain *why*, since the diff already shows *what*.

### Practice
1. Write a real ADR for one decision you've already made on a real project (even retroactively) — context, alternatives you actually considered (or should have), and your reasoning — and notice whether writing it down surfaces a gap in your original reasoning that you hadn't consciously examined.
2. Write one post-mortem (blameless, focused on root cause and prevention) for a real bug or incident you've dealt with, even a minor one, using the four-part structure above.

### Answer This
- Why is the "why" in an ADR more valuable long-term than the "what"?
- Why does a blameless post-mortem tend to produce a more accurate account of what happened than one focused on identifying fault?

---

## 16.3 — Code Reviews

**Tier: 🎯 Master**

### What to Learn

**Giving reviews**
- Review for correctness first, then design, then style — in that order of priority, so a review doesn't get bogged down in formatting nitpicks while missing an actual bug.
- Ask questions instead of making demands — "What happens if X is null here?" surfaces the same issue as "Fix this" while leaving room for the author to have a reason you're missing, and invites a conversation instead of a directive.
- Distinguish blocking issues from suggestions explicitly — an unmarked pile of comments forces the author to guess which ones actually need addressing before merge.
- Explain the *why* behind every suggestion — the same principle as commit messages and ADRs, applied to reviews.
- Acknowledge what's done well, not only what needs changing — a review that's 100% correction, even when technically accurate, reads as harsher than intended and doesn't help calibrate what the author should keep doing.

**Receiving reviews**
- A code review is feedback on the code, not a judgment on you — internalizing this distinction is what makes reviews something you can act on efficiently rather than something to get defensive about.
- Respond to every comment, even a simple acknowledgment — an unaddressed comment leaves the reviewer unsure whether it was seen, missed, or silently dismissed.
- Push back respectfully when you disagree, with reasoning — not silence (which reads as either agreement or resentment) and not emotion.

### Practice
1. On your next code review (giving one), explicitly separate your comments into "blocking" and "suggestion/nit," and include the *why* for at least the blocking ones.
2. On your next code review (receiving one), respond to every single comment, even ones you agree with immediately and are just fixing — practice this as a discipline, not just when you happen to remember.
3. Find one review comment (given or received) that was purely a directive ("fix this") with no reasoning, and rewrite it as a question or an explained suggestion instead.

### Answer This
- Why does reviewing for correctness before style matter, in terms of what a reviewer's attention and time actually get spent on?
- Why is an unanswered review comment a real problem, even if the author quietly agrees and fixes it?

---

## 16.4 — Meetings & Working in a Team

**Tier: 🎯 Master**

### What to Learn
- Come prepared — know the actual goal of a meeting before it starts, rather than treating it as a place to figure out what you think in real time at everyone else's expense.
- Ask clarifying questions early — ambiguity left unresolved compounds the longer it goes unaddressed, since decisions get built on top of a shared misunderstanding.
- Summarize decisions in writing afterward — verbal alignment degrades surprisingly fast without a written record that everyone can refer back to later.
- **Disagree and commit** — voice a disagreement once, clearly, with your actual reasoning; once a decision is made, execute it fully rather than re-litigating it repeatedly or quietly under-investing in it.
- Update stakeholders proactively — delivering bad news early is always better received than delivering it late, even though it's the harder thing to do in the moment.
- Share context generously — never assume others already know what you know; what feels obvious to you is frequently not obvious to someone who wasn't in the room for the earlier context.
- Pair programming — one of the fastest ways to transfer knowledge in both directions simultaneously, not just a way to "help someone stuck."
- Mentor by asking questions, not by giving answers — a question that leads someone to their own correct conclusion teaches the underlying reasoning; a direct answer only solves that one instance.

### Practice
1. After your next meeting where a real decision was made, write a short summary (a few sentences) and share it with the people involved, even if it feels redundant — notice whether anyone's understanding of the decision actually differed from yours.
2. The next time you disagree with a team decision, voice it once, clearly, with your specific reasoning — then commit fully to the decision once made, and notice whether you're actually able to let go of relitigating it.
3. The next time someone asks you a question you could answer directly in ten seconds, try instead asking them one guiding question first, and see if they arrive at the answer themselves.

### Answer This
- Why does verbal alignment in a meeting degrade faster than people expect, without a written summary?
- What's the actual difference between disagreeing-and-committing and simply staying quiet?
- Why might teaching through a guiding question produce more durable understanding than giving the direct answer?

---

## 16.5 — Communicating Technical Concepts

**Tier: 🎯 Master**

### What to Learn
- **The Feynman Technique** — explain a concept as if teaching someone with zero background in it; the specific places where your explanation gets vague, hand-wavy, or stuck are exactly the places you don't actually understand it as well as you thought.
- **Know your audience** — a CTO or product stakeholder generally needs the tradeoff and the business impact, not the implementation detail; leading with implementation detail to a non-technical audience often loses the point you're actually trying to make.
- **Use analogies** — a complex system explained through a familiar, well-chosen analogy is both understood faster and retained longer than the same explanation given in purely technical terms.
- **Visualize** — a diagram frequently communicates an architecture in seconds what an equivalent paragraph of prose takes minutes to convey, and is far less likely to be misread.

### Practice
1. Pick one technical concept you use regularly (from any part of this roadmap) and explain it out loud, or in writing, to an imagined complete beginner — without using any of its own jargon. Notice exactly where you get stuck or start hand-waving; that's the actual gap in your own understanding, not a communication failure.
2. Take a system you've designed or worked on and explain it twice — once as you naturally would to another engineer, once as you'd explain it to a non-technical stakeholder focused only on tradeoffs and business impact — and notice how much of the first explanation had to change, not just simplify.

### Answer This
- Why does getting stuck while doing a Feynman-style explanation reveal a real gap in understanding, rather than just a communication difficulty?
- Why does leading with implementation detail often fail with a non-technical audience specifically?

---

## 16.6 — Async-First Communication

**Tier: 🎯 Master**

### What to Learn
- Default to written, searchable, durable communication over a synchronous meeting whenever the content allows it — a well-written message can be read, understood, and acted on by someone in a different timezone without ever needing to schedule a meeting.
- Batch questions rather than interrupting someone for one question at a time when several are likely to come up.
- Include full context in every async message — the reader has no idea what you were just thinking about, so a message assuming shared context that doesn't actually exist yet forces a slow back-and-forth just to establish it.
- Set and respect response-time expectations explicitly — not everything is urgent, and treating everything as equally urgent trains people to either constantly context-switch or to ignore your messages.

### Practice
1. The next time you're tempted to schedule a meeting to convey information (not to make a decision requiring real discussion), write it up instead and share it async — notice whether the meeting was actually necessary.
2. The next time you have multiple small questions for the same person, batch them into a single message instead of sending them as they occur to you.

### Answer This
- Why is a meeting sometimes the wrong tool even when the content itself isn't controversial or complex?
- Why does including full context in an async message matter more than it would in a live conversation?

---

## Phase 16 Capstone

> **PROJECT (an applied-communication exercise):** Write one real ADR and one real post-mortem (or design doc, if no incident is available) for actual work you've done, using the structures in 16.2. Give at least 3 real code reviews (or re-review 3 past ones you gave) explicitly separating blocking issues from suggestions and explaining the *why* behind each. Explain one real system you've built twice in writing — once for a fellow engineer, once for a non-technical stakeholder — and compare the two versions side by side, noting specifically what changed beyond just "simpler words."

---

# Phase 17 — Engineering Practices & Methodologies

**Phase tier: 🎯 Mostly Master — Agile/12-Factor/Tech Debt Master; Lean/deep DDD/FinOps Know Only**

> Great engineers aren't just technically strong — they know how to operate effectively inside teams, products, and organizations. This is the operating system running underneath every technical skill in Parts I–IV.

## 17.1 — Agile / Scrum / Kanban / XP

**Tier: 🎯 Master**

### What to Learn

**Agile — the philosophy, not the ceremony**
- Iterative delivery over big-bang releases; responding to change over rigidly following an upfront plan; individuals and interactions valued over process and tooling for their own sake.
- **What Agile explicitly is not** — "no documentation," "no planning," or "move fast and break things"; a common, real dysfunction is using "we're Agile" as an excuse to skip design thinking entirely, which is a misreading of the philosophy, not an application of it.

**Scrum**

| Element | What It Actually Is |
|---|---|
| Sprint | A time-boxed iteration (1–2 weeks) producing a genuinely shippable increment |
| Sprint Planning | The team pulls backlog work it can realistically complete this sprint |
| Daily Standup | A 15-minute sync — what I did, what I'll do, what's blocking me — not a status report to a manager |
| Sprint Review | A demo of real working software to stakeholders, to get genuine feedback |
| Retrospective | What went well, what didn't, what changes — must produce real, tracked action items or it's theater |
| Product Owner | Prioritizes the backlog, represents the business's actual needs |
| Scrum Master | Removes blockers, protects the team's focus, improves the process itself |
| Story Points | Relative complexity (Fibonacci-like: 1, 2, 3, 5, 8, 13) — deliberately *not* hours |

**Kanban**
- Continuous flow, no sprints — work moves through To Do → In Progress → Done continuously.
- **WIP (Work In Progress) limits** — the single most important Kanban concept: capping how much work can be "in progress" at once forces bottlenecks to become visible instead of being hidden behind everyone looking individually busy.
- Pull-based — the team pulls new work only when it genuinely has capacity, rather than work being pushed onto people regardless of current load.
- Cycle time (how long from start to done) is tracked and used to drive real process improvement.
- Kanban tends to beat Scrum for support/ops work and other genuinely unpredictable incoming-work streams, where a fixed sprint commitment doesn't fit the actual nature of the work.

**XP (Extreme Programming)**
- Pair programming, Test-Driven Development (Phase 11.2), Continuous Integration, collective code ownership, and continuous (not "big cleanup sprint") refactoring — XP is genuinely the origin of most of the modern engineering practices covered across this entire roadmap, not a separate, outdated methodology.

**Recognizing Agile as cargo cult**
- Story points used as an individual performance metric — this directly defeats the entire purpose of relative, team-level estimation.
- Retrospectives that never produce real action items — theater, not actual improvement.
- "We're Agile" used as a justification for skipping design or documentation entirely.
- Sprints run as mini-waterfalls (design in sprint 1, build in sprint 2, test in sprint 3) — this is Agile in name only.
- A "standup" that's actually a 45-minute status meeting.
- Velocity treated as a binding commitment rather than a rough, historical measurement.

### Practice
1. Attend (or run) one real retrospective and insist on at least one genuinely tracked, followed-up action item — check back on it at the next retrospective and see whether it actually happened.
2. Audit your own team's current process against the cargo-cult list above — honestly identify if any of these dysfunctions are present, and pick one to raise (constructively) with the team.
3. If your team uses Kanban or a Kanban-like board, check whether WIP limits are actually enforced or just theoretically defined — try enforcing one explicitly for a week and see what bottleneck it exposes.

### Answer This
- Why does using story points as an individual performance metric undermine the entire point of relative estimation?
- Why do WIP limits specifically make bottlenecks visible, when nothing about the work itself changes by adding the limit?
- Why is XP considered the origin of practices like CI and TDD, rather than a separate methodology from them?

---

## 17.2 — Lean / Lean Startup

**Tier: 👀 Know Only**

### What to Learn
- **Lean's core ideas** — eliminate waste (anything not adding real value to the customer), build quality in from the start (catch defects early rather than inspecting for them at the end), defer commitment (keep real options open as long as reasonably possible before locking in a decision), deliver in small batches for fast feedback.
- **Build-Measure-Learn loop** — build the smallest thing that tests a real assumption, measure actual behavior, learn from it, and adjust.
- **MVP (Minimum Viable Product)** — explicitly *not* a broken or low-quality product; it's the smallest thing that genuinely tests a real hypothesis and produces real learning — an MVP can be a landing page, a manual/concierge process, or a working prototype, whichever is the smallest thing that still produces valid evidence.
- **Validated learning** — evidence drawn from real, observed user behavior, not opinions, assumptions, or stakeholder intuition, however confident.
- **Pivot or persevere** — a decision made based on validated learning, not on sunk cost or personal attachment to the original idea.
- **Innovation accounting** — measuring what actually matters (activation, retention, referral) rather than vanity metrics (raw page views, raw signups) that look good but don't predict real business outcomes.

### Practice
1. Before building your next non-trivial feature, write down explicitly: what hypothesis is this feature actually testing, and what's the smallest version that would produce real evidence either way? Compare that to what you were about to build by default.
2. Identify one vanity metric currently used to evaluate a project or feature you're involved in, and propose (even just to yourself) a metric from the activation/retention/referral family that would actually predict whether it's working.

### Answer This
- Why is an MVP not the same thing as "a low-quality version of the product"?
- Why can a vanity metric look like clear success while telling you almost nothing useful about whether a feature is actually working?

---

## 17.3 — Domain-Driven Design (DDD) — Deep Dive

**Tier: 👀 Know Only**

> Phase 3.4 covered DDD's basics (Entities, Value Objects, Aggregates). This section is the strategic and organizational layer that makes DDD actually usable across a real, larger system — not just within a single class design.

### What to Learn

**Strategic design — the big picture**
- **Bounded Context** — a boundary within which a domain model has one single, unambiguous meaning; the same word ("Order") can mean genuinely different things in different bounded contexts (Sales vs Fulfillment vs Accounting), and trying to force one unified "Order" model across all of them usually produces a bloated, contradictory mess that satisfies none of the contexts well.
- **Ubiquitous Language** — the same terms used by developers *and* domain experts, in both conversation and code; if the business calls something an "Engagement" and the code calls the equivalent thing a "UserInteraction," that mismatch is a constant, low-grade source of miscommunication and translation-induced bugs.
- **Context Map** — a diagram showing how different bounded contexts relate to and integrate with each other — making implicit organizational/system boundaries explicit.
- **Anti-Corruption Layer** — a deliberate translation layer that prevents a poorly-designed (or simply differently-modeled) external system's concepts from leaking into and polluting your own domain model.

**Integration patterns between bounded contexts**

| Pattern | What It Means |
|---|---|
| Shared Kernel | Two teams deliberately share a subset of the domain model — real, tight coupling that requires coordinated changes |
| Customer/Supplier | An upstream team publishes a model/contract; a downstream team consumes it |
| Conformist | The downstream team adopts the upstream model as-is, with no translation — simpler, but fully dependent on upstream |
| Anti-Corruption Layer | Translate between models explicitly, isolating your domain from an external model's mess |
| Published Language | A well-documented, explicit shared contract (e.g., JSON schema, OpenAPI) used for integration |

**Tactical design — building the model (recap + extension of Phase 3.4)**
- **Entities** — identity that persists across state changes.
- **Value Objects** — no identity, defined purely by attributes, immutable.
- **Aggregates** and the **Aggregate Root** — a cluster of entities/value objects with one root controlling all external access and enforcing every invariant; external code never reaches into an aggregate's internals directly, it only ever talks to the root.
- **Repository** — abstracts persistence specifically for aggregates.
- **Domain Events** — something that happened, expressed in past tense, immutable (`OrderPlaced`, `PaymentFailed`).
- **Domain Service** — business logic that genuinely doesn't belong to any single entity or value object (`PricingService.calculateDiscount(order, customer)`).
- **Application Service** — thin orchestration of domain objects and infrastructure to fulfill one specific use case, deliberately containing no real business logic itself.

**Event Storming**
- A collaborative workshop bringing developers, domain experts, and product people into the same room to discover the actual domain together.
- Big-picture event storming — surface domain events on a long timeline (orange stickies) without worrying yet about structure.
- Process modeling — layer in commands (blue), actors (yellow), policies (purple), and read models (green) on top of the event timeline.
- Design-level event storming — use the discovered events to identify real aggregate and bounded-context boundaries.
- The actual output of an Event Storming session is shared understanding among the room, not primarily the sticky-note diagram itself — the diagram is a byproduct of the conversation, not the goal.

### Practice
1. For a real domain you work in, identify one word that means genuinely different things in two different parts of the organization (e.g., different teams, different systems) — write down both meanings explicitly, and consider whether that's evidence of two real bounded contexts being forced into one shared model.
2. Run (or simulate solo, if a real session isn't available) a small big-picture event-storming exercise for one feature area: write out the real domain events on a timeline, then layer in the commands and actors that produce them.
3. Identify one place in a real codebase where an external system's model has clearly leaked directly into your own domain code without any translation layer, and sketch what an Anti-Corruption Layer for it would look like.

### Answer This
- Why can forcing one unified model of "Order" across Sales, Fulfillment, and Accounting produce a worse result than accepting three different bounded-context-specific models?
- What's the actual output of an Event Storming session — the diagram, or something else?
- Why does an Aggregate Root, specifically, need to be the only thing external code talks to?

---

## 17.4 — The 12-Factor App

**Tier: 🎯 Master**

### What to Learn

| Factor | What It Means | Why It Matters |
|--------|--------------|----------------|
| 1. Codebase | One codebase in version control, many deploys | No snowflake servers; every environment traces back to the same source |
| 2. Dependencies | Explicitly declare and isolate dependencies | Never assume a system-level library happens to be installed |
| 3. Config | Store config in environment variables, not in code | Never hardcode URLs/credentials/environment-specific values — **violated most often** |
| 4. Backing Services | Treat databases, caches, queues as attached resources | Swap a local Postgres for RDS without any code change |
| 5. Build, Release, Run | Strictly separate build and run stages | The same build artifact runs unchanged in staging and production |
| 6. Processes | Execute the app as stateless processes | No sticky sessions, no in-memory state — state lives in backing services (Phase 10.2) |
| 7. Port Binding | Export services via port binding | The app is self-contained; a reverse proxy routes to it, not the reverse |
| 8. Concurrency | Scale out via the process model | Horizontal scaling (Phase 10.2) — more processes, not bigger ones |
| 9. Disposability | Fast startup, graceful shutdown | Handle `SIGTERM` (Phase 1.3, 5A.6 now / 5B.5 later) — drain in-flight requests, release locks |
| 10. Dev/Prod Parity | Keep development and production as similar as possible | Docker Compose with a real Postgres locally (Phase 5A.8 now / 5B.7 later), not an in-memory substitute |
| 11. Logs | Treat logs as event streams | Write to stdout, let the platform aggregate — **violated most often** |
| 12. Admin Processes | Run admin tasks as one-off processes | Migrations and data cleanups run from the same codebase, not ad-hoc scripts |

- **Factors 3 (Config) and 11 (Logs) are violated most often in real systems** — config hardcoded into source is a real, recurring security-incident cause; writing logs to local files instead of stdout makes debugging inside containers/orchestrated environments genuinely painful, since the file may not even be accessible after the container is gone.

### Practice
1. Audit a real project you've worked on against all 12 factors, explicitly, one by one — mark each as compliant, partially compliant, or violated, with a one-line note on why.
2. For the worst violation you find (most likely Config or Logs), actually fix it — move a hardcoded value to an environment variable, or redirect file-based logging to stdout — and confirm the app still behaves correctly afterward.

### Answer This
- Why are Config and Logs the two factors most commonly violated in real systems, specifically?
- Why does "the same build artifact runs unchanged in staging and production" (Factor 5) matter as much as it does — what real bug class does violating it cause?

---

## 17.5 — Technical Debt Quadrant

**Tier: 🎯 Master**

### What to Learn
```
                    DELIBERATE
                        |
    Prudent &           |        Reckless &
    Deliberate          |        Deliberate
    "We know we're      |        "We don't have
    taking a shortcut   |        time for design"
    and will fix it     |
    after launch"       |
                        |
PRUDENT ----------------+---------------- RECKLESS
                        |
    Prudent &           |        Reckless &
    Inadvertent         |        Inadvertent
    "Now we know how    |        "What's layering?"
    we should have      |
    designed it"        |
                        |
                   INADVERTENT
```
- **Prudent & Deliberate** — a conscious, acceptable shortcut made under a real deadline, with a genuine, tracked plan to fix it afterward.
- **Reckless & Deliberate** — "we don't have time for proper design," made *knowingly* — this produces the worst kind of debt, because it's a known-bad decision made under a manufactured or poorly-examined time pressure; this should be actively avoided, not just tolerated as "part of the job."
- **Prudent & Inadvertent** — "now that we've built it, we understand how it should have been designed" — genuinely healthy learning, and a normal, expected part of building anything non-trivial; refactor once the better understanding is clear (Phase 3.5).
- **Reckless & Inadvertent** — writing code without knowing there was a better way, out of inexperience rather than a deliberate choice — prevented over time by code review, learning, and mentorship, not by blame.
- **Managing debt in practice** — track it *explicitly*, in a real backlog; debt that lives only in one person's head effectively doesn't exist to the rest of the team, and disappears entirely if that person leaves. Quantify the actual cost where possible ("this coupling costs us roughly 2 extra hours per sprint"). Apply the Boy Scout Rule (Phase 3.5/15.6) continuously. Never let reckless debt accumulate — unlike prudent debt, it compounds in a way that gets actively worse, not just static. Schedule debt paydown as ongoing, continuous work, not a mythical future "cleanup sprint" that in practice almost never actually happens.

### Practice
1. Identify one piece of real technical debt in a codebase you work in, and classify it honestly into one of the four quadrants — was it a conscious tradeoff, or something nobody realized at the time?
2. Add that piece of debt explicitly to a real, visible backlog (even if it's just a personal note-to-team), with a rough note on its actual cost, rather than letting it continue to live only as tribal knowledge.

### Answer This
- Why is "Reckless & Deliberate" considered the worst quadrant, worse even than debt from genuine inexperience?
- Why does debt tracked only in one person's head effectively not exist from the team's perspective?
- Why does a "cleanup sprint" as the plan for paying down debt tend to fail in practice?

---

## 17.6 — Spikes, OKRs & FinOps

**Tier: 👀 Know Only**

### What to Learn

**Spikes**
- A **spike** is a strictly time-boxed investigation whose output is *knowledge*, not production code.
- Use a spike when the team genuinely disagrees on the right approach, the technology involved is unfamiliar, or an estimate is wildly uncertain and needs to be de-risked before being committed to.
- Always time-box it explicitly ("we spend 2 days on this, then decide") — an un-timeboxed spike tends to quietly turn into unplanned, unaccountable production work.
- **Technical spike** — can we actually integrate with this API? How does this library really perform under our load? **Functional spike** — what does the user actually need here, tested with a quick prototype rather than assumed.
- The output of a spike is a recommendation, a proof of concept, or a written summary — explicitly not a pull request meant to be merged as-is.

**OKRs (Objectives and Key Results)**
- **Objective** — an ambitious, qualitative goal ("Make our platform the most reliable in the industry").
- **Key Results** — measurable outcomes that tell you whether the objective was actually achieved ("Reduce p99 latency from 800ms to 200ms," "Achieve 99.95% uptime").
- OKRs measure *outcomes*, not activities or task lists — "ship feature X" is not a well-formed Key Result.
- OKRs are meant to be ambitious; achieving 70% of a genuinely stretch OKR is often more valuable than achieving 100% of a deliberately conservative one.
- Understanding company-level OKRs helps an engineer prioritize technical work correctly and communicate its impact in terms that actually matter to the business.
- Common anti-patterns — OKRs that are just KPIs relabeled, OKRs used as individual performance-review inputs (which incentivizes sandbagging them), and simply having too many at once (3–5 is a reasonable practical maximum).

**FinOps (cloud cost engineering)**
- **Right-sizing** — matching instance size to the *actual* observed workload, not a worst-case theoretical peak that rarely or never occurs.
- **Reserved instances/savings plans** — committing to 1-3 years of usage in exchange for a real 30-70% discount versus on-demand pricing.
- **Spot instances** — up to ~90% cheaper, but can be interrupted with little notice — appropriate for batch jobs, CI/CD runners, and other genuinely non-critical, interruption-tolerant workloads.
- **Tagging strategy** — tagging every resource with team/environment/service/cost-center; without consistent tagging, a question like "what does the payments service actually cost us monthly" is often literally unanswerable from the billing data alone.
- **Chargeback/showback** — allocating cloud costs back to the specific teams/products actually incurring them, making cost a visible, ownable metric rather than an abstract shared line item.
- **Cost alerts** — configured proactively, before you need them, not discovered reactively via a surprise bill.
- **Real engineering decisions that drive cost** — an over-provisioned database instance (check Performance Insights before assuming you need a bigger one); NAT Gateway data-transfer costs specifically for cross-AZ traffic; CloudWatch Logs costs scaling with both ingestion and retention (filter before storing, don't just log everything at maximum verbosity forever); forgotten, still-running staging environments and orphaned snapshots.

### Practice
1. The next time your team faces a genuinely uncertain technical decision, propose a time-boxed spike explicitly, with a stated deadline and a stated deliverable (a recommendation or proof of concept) — rather than letting the uncertainty quietly turn into unplanned, un-timeboxed exploration.
2. Write one real OKR for a piece of technical work you're doing or planning, with Key Results that are genuinely measurable outcomes (not just "ship X") — and check whether it would survive someone asking "how would we actually know if this Key Result was hit?"
3. Pick one piece of real cloud infrastructure you have access to (or a hypothetical one) and estimate: is it right-sized for its actual observed load, is it tagged well enough to attribute its cost to a specific team/service, and would a spot instance be appropriate for any part of its workload?

### Answer This
- Why does an un-timeboxed spike tend to quietly become unplanned production work?
- Why is "ship feature X" not a well-formed Key Result, even though it's clearly measurable as done-or-not-done?
- Why is achieving 70% of an ambitious OKR sometimes considered more valuable than achieving 100% of a conservative one?
- Why can a specific service's cloud cost be literally unanswerable from billing data alone, without a consistent tagging strategy?

---

## Phase 17 Capstone

> **PROJECT (an organizational-practices exercise):** Audit one real project against the full 12-Factor App checklist and fix the worst violation you find. Classify 3 real pieces of technical debt from a codebase you work in into the debt quadrant, and add the most important one to a real, tracked backlog with an estimated cost. Run (or simulate) a big-picture Event Storming session for one real feature area, producing an explicit timeline of domain events, commands, and actors. Propose and time-box one real spike for a genuinely uncertain decision your team is facing, and see it through to an actual recommendation.

---

> **A closing note on this Part specifically:** unlike Parts I–IV, there is no clean "capstone project" that proves you're done with Part V, because you're never done with it. The habits here (questioning the problem, debugging scientifically, writing the ADR, running the honest retro, tracking debt explicitly) are meant to run continuously, in the background, for the rest of your career — revisit this file every few months and notice which sections read differently now that you have more real experience behind them.
