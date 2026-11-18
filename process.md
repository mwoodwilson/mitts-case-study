# How I run an AI engineering team

I don't write the code. Claude Code does the engineering: auditing, planning, building, testing, capturing evidence. My job is the same as on any team I've led: set direction, make the product calls, hold the quality bar, and keep decisions cheap and reversible.

The process below is written into the project's standing instructions, so every new session starts from it. It wasn't designed up front. Most rules exist because something went wrong without them, and the reason is given beside each one.

---

## 1. Roles and decision rights

| Who | Decides |
|---|---|
| Me (product owner) | Anything a user sees, hears or pays for. Direction, priorities, scope, sequencing. When something is good enough to ship. |
| Claude Code (the team) | How to build what I've approved. Engineering calls inside an approved scope, each logged with its cost if wrong. |
| An independent reviewer (a fresh Claude instance) | Whether a task is actually done: correct, tested, and matching the spec. The builder never grades its own work. |

**"Decided on my behalf" is a first-class category.** When the team makes a call inside a scope I approved, it goes in the decision log with *why* and *what it costs if wrong*. Most of these I never touch. When one is wrong, I reverse it in a sentence. This keeps me out of the weeds without losing visibility.

Examples from the log: a single-punch call keeps its natural delivery because it has nothing to contrast (cost if wrong: one setting); existing users accept the updated Terms after the rename but aren't asked their stance again.

## 2. Decision hygiene

- **Log every decision the same day,** in plain language, with what, why and the cost. The log is the project's memory and the source for this case study ([decisions.md](decisions.md)).
- **Every ask comes with a recommendation.** Never "what do you want?"; always "I recommend X because Y; on yes, this happens; on no, that happens."
- **Asks go in one numbered list in chat, with the count on the first line.** Each ask stands on its own: the question, what it is, exactly where to look (page and section heading), the recommendation, and what yes and no each mean. A page with visuals carries the same list at its top.
  - Why: early on, asks were spread across report sections and review pages and had to be hunted for. Since then, every decision is either in that one list or linked to the exact place it needs to be made.
- **One reply can approve everything.** Because every ask carries a recommendation, a single-word approval accepts them all, and I only write more when I disagree.
- **Replies are decidable in under a minute.** Critical detail only, decision or result first. The full detail lives in the plan files, the handoff notes or a linked page.
  - Why: an earlier default was a five-part report (feedback, what's done, evidence, decisions, next steps). It was thorough but too slow to act on. The five parts still exist; they moved into linked documents.
- **Corrections are logged as corrections.** When a later check showed that an earlier popup rule contradicted Apple's guidance, the log says so and points to the entry it replaces.

## 3. Specs before screens

- **Before any screen changes, a short WHEN/THEN spec,** covering the empty, offline, error and first-run states, approved by me. Example: [examples/spec-batch-a.md](examples/spec-batch-a.md).
- **Anything visual is mocked before code.** Why: an early redesign went straight to code and was rejected, and the build was wasted.
- **The spec separates "1 decision for you" from "decided for you".** I see the one real choice at the top, with options in a table and real examples. Everything else is a call already made inside approved fixes, listed so I can object.
- **Each item says how it will be proved:** which automated tests, which tap-through flow, which screenshots.

## 4. Plans, and review between every task

- **Big work becomes a written plan** with tasks small enough to build and review one at a time. Each plan opens with the owner's open questions, then "decided for you", global constraints, a "review focus" (the risks no single task's tests would catch), phases and estimates. Example: [examples/plan-card-build.md](examples/plan-card-build.md).
- **One helper writes each task; a fresh reviewer checks it before the next starts.** At most three helpers at once. Why: I don't review code line by line, so independent review is the safety net. On the day this rule was adopted, a reviewer caught a bug that would have silenced the trainer's count-in.
- **Plans are reviewed before they reach me.** The Card build plan was written by one helper, reviewed and revised by another, and folded in the findings of an iPhone SE check run the same evening.
- **Rulings are written down as they're made.** During a build, engineering calls go in a ledger with the cost if wrong; user-facing ones are copied to the decision log. A final whole-branch review closes each plan, and anything deferred is listed with the reason. Example: [examples/review-ledger-native-audio.md](examples/review-ledger-native-audio.md).

## 5. Evidence before "done"

- **Screenshots of every changed screen** before anything is called done, on two phones (a large one and a standard 390-point one) at normal text and at the largest accessibility text size.
  - Why: the worst bug in the project (the live combo call shrinking to a speck) was invisible on my phone at normal text and broken on most other iPhones.
- **Tap-through flows** (Maestro) cover the main paths on the simulator.
- **Plain results:** type check clean, test counts before and after, which flows passed.
- **What still looks wrong is listed,** not hidden. Batch A's evidence ended with a section called "Things that look wrong on screen (not fixed here)". Example: [examples/evidence-batch-a.md](examples/evidence-batch-a.md).

## 6. Tests are never weakened

- **Never weaken or delete an existing test.** A test that encodes the old version of a change I approved may be updated, and every one is listed in the report: which test, what changed, and why it's not weaker.
- **Every past bug becomes a test.** Randomised tests also run hundreds of simulated workouts full of pauses, locks, rewinds and crashes against a list of rules that must always hold.
- **Rules I care about become guards.** For example: no text in the app may shrink to fit; the old product name may not return to on-screen text; every line the trainer can say must have a recording before release.

## 7. Grading the subjective part

Some quality can't be unit-tested: does a round *flow*, does a line sound like a real cornerman? For the trainer's output there's a versioned rubric ([examples/eval-rubric.md](examples/eval-rubric.md)):

- Code runs the pass/fail gates and the countable scores.
- Claude proposes the judgment calls, each with a one-line reason.
- I agree or disagree, and have the final word.
- Claude grades alone only after matching me on 80 percent of calls for two rounds running.
- Any judgment I make that code could have made becomes a new code check.

The same shape worked for the voice: measure what can be measured (pitch, loudness, jitter, the right words), present side-by-side samples, and let my ear make the call. See [examples/voice-research.md](examples/voice-research.md).

## 8. Sessions, saving and recovery

- **One phase per session** (an audit, an audio fix, one design batch). A session never mixes phases. Why: context stays focused, and each phase ends in a reviewable state.
- **A handoff file is kept current as work goes,** not only at the end, so a new session picks up exactly where the last stopped.
- **Save at every step that passes the type check and the tests;** back up to a private repository after each batch.
- **Working files that are costly to redo live in the project,** never in a temporary folder. Why: an outage once wiped the temporary folders mid-session. No app work was lost, but the voice tools had to be rebuilt from the session record.

## 9. Release discipline

The release check runs on a TestFlight build on my own phone: a clean install, permissions allowed and denied, airplane mode, lock and unlock, headphones, the silent switch, large text, and force-quit and reopen. Before release: a scan for placeholder text and dead buttons, and App Review notes that explain what makes Mitts more than a timer.
