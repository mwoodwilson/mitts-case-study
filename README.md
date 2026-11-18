# Mitts: a product case study

**Matthew Wilson**, Staff Product Manager

**Mitts** is an iPhone boxing trainer. A coach's voice calls punch combinations ("1, 2, 3 to the body") through timed rounds, and users can build workouts and share them with a friend. No accounts, no ads, works offline, analytics opt-in.

**My role:** product owner and sole decision-maker. AI coding agents (Claude Code) do the engineering: audit, plan, build, test. I set direction and the quality bar, and I make every product call. The app's code is private; this repo shows the product work.

## Competencies shown

| Competency | Where it shows |
|---|---|
| **Diagnosis before solution** | An App Store rejection and three unexplained bugs traced to one root cause by an audit that required a reproduction test for every claim. Chose a rebuild over patching symptoms. |
| **Scoping and prioritisation** | Sequenced the fix that blocked resubmission first, then broken screens, then the redesign. Purely visual fixes waited for the new design system, so nothing was built twice. |
| **Spec writing** | WHEN/THEN specs covering empty, offline, error, first-launch and large-text states, with the one real decision at the top in an options table. |
| **Quality bars and evals** | A versioned rubric for what the trainer says: three code-checked pass/fail gates, six scores never averaged. The AI grades alone only after matching my judgment 80% of the time, two rounds running. |
| **Directing AI agents with review gates** | Every task checked by an independent reviewer. Calls made on my behalf logged with their cost if wrong. Nothing is "done" without screenshots on two phones at two text sizes. |
| **Privacy and compliance judgment** | Kept AI out of the engine to stay offline and private with no per-user cost. Analytics off until the user agrees; no advertising identifiers. Resolved the App Store rejection at its root. |
| **Data-informed iteration** | Turned a subjective goal, a coach voice that sounds natural, into targets measured from 280 real coach calls, then shipped a rebuild of all 878 voice clips behind an automated quality gate. |
| **Designing for the user's context** | A design system built for someone mid-round across the room: round and rest now differ 12 to 1 in brightness (was 1.3 to 1), with 72-point targets for gloved hands. |

## Headline decisions

| Decision | Trade-off accepted |
|---|---|
| Rebuild the training engine and replace the audio player, rather than patch | Weeks before any visible feature, in exchange for fixing the rejection and its related bugs at the root. |
| No AI model in the engine; hand-written coaching rules | Less variety in what the coach says, in exchange for offline use, privacy, no server and no per-user cost (an estimated $0.01 to $0.94 per user per month). |
| Fix all 20 UX audit findings, broken items first | A longer road to the new look, in exchange for fixing what users hit today and drawing each screen only once. |
| One "Keep coaching in the background" switch, off by default | Less fine-grained control, in exchange for predictable behaviour (iOS can't reliably tell locking the phone from switching apps). |
| Rename the product without migrating any user data | Internal names keep the old word, in exchange for zero risk to saved workouts and share links already sent. |

## Before and after

The live combo call on a standard 390-point iPhone. A UX audit found it shrinking to a speck on most current iPhones, a bug that never appeared on my own phone.

| Before | After |
|---|---|
| <img src="images/live-call-before.webp" width="240" alt="Workout screen with the combo call shrunk to a speck"> | <img src="images/live-call-after.webp" width="240" alt="Workout screen with the combo call at full size"> |

## Read more

| | |
|---|---|
| [case-study.md](case-study.md) | The full story: the rejection, the rebuild, the audit, the design system, the rename, the voice |
| [decisions.md](decisions.md) | Curated decision log: what was decided, why, and what it cost |
| [process.md](process.md) | How I run an AI engineering team: decision rights, specs, review, evidence |
| [examples/](examples/) | Real artifacts: a WHEN/THEN spec, a reviewed build plan, a review ledger, an evidence list, the eval rubric, the voice research |

## Status

Built: the engine rebuild, the native audio player, the audit fixes and the rename. The rebuilt coach voice is in the test build in both voices. The new design system is signed off and its build is next, followed by background coaching and App Store resubmission. Not yet released; nothing here claims users or revenue.
