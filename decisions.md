# Decision log (curated)

The project keeps a decision log: every product decision, recorded the same day in plain language, with **what** was decided, **why**, and **what it costs**. This is a curated, edited copy for public reading, newest first.

How to read it:

- **Decided** is my call as product owner.
- **Decided on my behalf** is a call the engineering team (Claude Code) made inside a scope I had already approved, logged with its cost so I could reverse it in one sentence. Anything that changes what a user sees, hears or pays comes to me.
- Internal identifiers and engineering detail are removed. The app was called Corner until it was renamed Mitts on 16 November; older entries use the name of the time.

---

## 23 November

**Intermittent test failures: diagnose first, never weaken a test.** Eight tests failed now and then. Each was diagnosed before anything changed, and in every case the app was right: the test's expectation was stricter than the approved behaviour on a rare random draw. Each check stayed at least as strict, and each failing case is now pinned as its own named test.
- Proof: 2,000 simulated workouts, five full runs back to back, and the whole test suite under 400 random seeds.

**Post-merge tidy: the retired reminder's cleanup and its notifications package are removed.** Testers had already run the cleanup. The app no longer carries a push-notification capability it doesn't use. Cost: the next test build rebuilds the app's native layer.

**The build upload leaves out design and research files.** 1.4 GB down to 49 MB.

**Process correction:** a documentation-only save went out alongside a test run that then failed. No app code was affected. A save now always waits for its checks.

**Open: trainer combos with footwork the builder doesn't offer.** They can't be rewound or saved from the paused screen. Recommended: show them with Rewind only, after release, with a spec and a mock first.

## 22 November

**The redesign passed my phone check and was merged.** A TestFlight build went through the full release check on my phone before the redesign left its branch.

**Two small workout-screen labels lightened to 5:1 contrast.** The approved design drew them at 3.8:1 and 3.4:1. Why: small text under 4.5:1 is hard to read mid-round. Cost: none visible beyond slightly brighter labels.

**The glove logo stays, with a condition.** It was traced from a freely licensed photo whose licence may require a credit. The recommendation was to retrace it; I chose to keep it and have someone qualified check the licence before the public release. Test builds aren't affected.

## 21 November

**A row's title goes on top and its details underneath, always.** One row had them the other way round. A test now holds the rule.

**Coach Mode's list says "Tap to edit, hold to reorder" under a real heading.** People need to see that the rows open and move.

**Decided on my behalf (final phase: Settings, menus, large text, VoiceOver):** at the largest text, lines sit closer together so content stays on screen, and long popups scroll as one so nothing is cut off. VoiceOver reads "Slip right", not "Slip R". Under Reduce Motion, swiped rows snap and sheets fade. The ten tap-through flows run again and pass on both phones.

## 20 November

**Our own menu sheet.** iOS 26 redrew the iPhone's menus as a small floating box with no Cancel, unlike the approved design. Mitts now draws the bottom sheet itself, the same on every iOS version.
- Cost: about half a session, and the menus no longer follow Apple's future restyles automatically.

**Decided on my behalf: bigger workouts can be shared.** A share code was refused over 4,000 characters, so a large Coach Mode workout couldn't be sent. The cap is now 100,000, enough for the largest workout the app allows.
- Cost: a friend on an older build has to update before adding one of those workouts.

**Decided on my behalf: every Undo after a delete puts the item back where it was,** whether the delete came from a swipe or a menu.

## 19 November

**Train gets big tappable cards, with Saved folded below.** I asked for it, it was mocked, and I approved it through three asks: the layout, a tap anywhere on Quick Start starts it, and Saved remembers whether it was left open.
- Cost: about half a day.

**A back arrow during the Get ready countdown.** A stray tap on Quick Start is one tap to undo, with no popup, because nothing has been trained yet. It fades at the first bell. Approved from a mock, with a little more room around it.

**Quick Start's editor ends in "Save and start" and "Start".** Changed settings can run once without overwriting Quick Start. Two standard buttons, so it was built without a mock.

**Decided on my behalf: the usage-data question becomes a card on Train after the first finished workout,** not a popup. One more on-phone yes/no records that a workout was finished. Nothing new leaves the phone.

## 18 November

**Every status page shows three columns: today, the approved design, the result.** After Phase 2 the progress page looked like rounder buttons, because only the building blocks had changed so far. A before-and-after alone hid how far the build was from what I'd approved.
- Cost: each page needs the approved picture captured beside the app's.

**The workout screen is rebuilt first.** It's the biggest change and where people spend the most time.

**The Card build's behaviour document is approved as written.** It's the agreed WHEN/THEN behaviour every phase is built and checked against.

**Decided on my behalf: readability over the picture where they disagree.** For example, Delete on a swiped row is black words on red, because the drawn grey on red measured 4.47:1, just under the 4.5:1 standard.

## 17 November

**The voice rebuild ships with round 7 as the recipe for both voices.** After seven listening rounds I approved round 7 for the male and female voice. An unvalidated eighth round for the female voice was dropped rather than shipped untested. Every line was rebuilt in both voices behind an automated quality gate and is now in the test build.
- Cost: three female combos flagged earlier ship as round 7 renders them.

**Card build plan: all five open questions approved as recommended.** A way back out of a friend's notes before the countdown; three distinct lines explaining why the phone paused a workout; long combos on the iPhone SE get a smaller lit card and are never cut; deleting a round happens at once with Undo instead of a popup; eight new lines approved as listed. See [examples/plan-card-build.md](examples/plan-card-build.md).

**Status reports become decision-first summaries.** Replies lead with the decisions needed and fit a one-minute read; the long five-part report moved out of chat.
- Why: owner time is the scarcest resource on the project.
- Cost: detail lives in handoff notes, plan files or a linked page.

**A public case study, separate from the private code.** This repo: no app code and no private identifiers.

## 16 November

**Short combos: an even tone, with the strongest emphasis on the last punch; the female voice de-roboticised.** Round 6 made two- and three-punch combos sound off and the female voice robotic.
- Decided: 2- and 3-punch combos keep the voice's natural rise and fall on each number at an even level; every number is as loud as the first, and the last is 4 to 5 dB louder and held 20 percent longer. Combos of 4 or more keep the approved falling finish.
- Found: round 6 had squeezed the female voice's pitch movement to under half and made her vocal vibration unnaturally even (jitter 1.3 to 1.7 percent against a natural 2.1 to 2.6). Round 7 restored both.

**Two-punch combos stress the second punch instead of climbing in pitch.** The climb sounded artificial.
- Decided on my behalf: a single-punch call has nothing to contrast, so it keeps its natural delivery. Cost if wrong: one setting.
- Found and fixed: the word-boundary tool placed some words up to 60 ms early, which is why emphasis first landed on the wrong number.

**Ending rule by length: 1 or 2 punches rise, 3 lift and settle, 4 or more fall.** Listening showed one round's treatment suited longer combos and another suited shorter ones.
- Tried and dropped: two open-source neural audio tools. They kept the voice exactly, rasp included.

**The scratch traced to the voice engine.** Against nine coaches' recordings, the synthetic voice's hiss is roughly twice as loud to the ear, it adds a faint whine at three fixed pitches, and the male vowels are raspier. Ruled out: file quality, the app's audio player, earlier processing.
- Decided on my behalf: only punches count toward a combo's length, not slips or steps. Cost if wrong: a 3-punch combo with a defensive move ends differently.

**The app icon is the bone glove on the app's black, drawn flat.** The first icon on a device looked grey and glowing, off-brand.
- Cause: a dark glove on a bone square matched nothing in the app, and iOS 26 added its own glass shading. That shading wasn't checked before the icon reached a phone.
- Now: the launch screen's colour pair, with iOS told to leave the icon flat.

**The rename is built.** Every screen, share message, home-screen name and legal link says Mitts; the tab is Library. Verified as an update over the old build, then on a real device.
- Decided on my behalf: existing users accept the updated Terms once but aren't asked their stance again, which would have overwritten a setting they'd already chosen.
- Decided on my behalf: support-page answers updated to match today's screens, including that usage data is sent only with consent.

**Legal pages republished under the new name.** Same terms, new name; old addresses forward.
- Why: the app can't be called Mitts while its legal links name another app.

**Working files protected after an outage.** An outage wiped temporary files mid-session. No app work was lost.
- Decided on my behalf: costly working files live in the project, never in temporary folders. Three jobs that don't touch app code run in parallel, each with its own agent.

## 15 November

**"Workout done" approved as drawn.**
- Decided on my behalf: the tick is drawn about as heavy as the clock's digits, so it reads from two to three metres.

**Voice: match real coaches before rebuilding anything.** The cleaner natural voice was an improvement but not yet good enough.
- Decided: measure how real coaches call combos and match that; rebuild all 878 clips only once the delivery matches and I agree it does.
- Method: pitch, gaps, stress and endings measured from public coaching videos, used only as a measuring stick. Only tools free for commercial use; anything paid comes back to me first. See [examples/voice-research.md](examples/voice-research.md).

**Batch A approved; unnamed workouts are named by the day** ("Sunday rounds").
- Why: it never repeats the summary line under the name, and it follows how leading fitness apps name an unnamed run.
- Cost: two workouts saved on the same weekday share a name. Names already saved never change. See [examples/spec-batch-a.md](examples/spec-batch-a.md).

**The logo is scrapped and restarted from a real glove.** Four rounds of freehand gloves read as other objects, and the typeface was too playful for the brand.
- Next: trace the outline of a real glove photographed in profile; if that fails, go type-only (fallback agreed in advance).

**App work runs in parallel with the logo.** The logo is not allowed to block the build; a placeholder wordmark is used until it's final.

**The new name is Mitts.**
- Why: "let's do mitts" is what boxers call the session where a coach holds focus mitts and calls combos, which is what the app does. One short word, unused on the US App Store, no boxing brand found by it.
- Written with a capital M so it reads athletic, not playful.
- Cost: the rename touches every place the name appears, from the app to the legal pages and store listing.

**Naming criteria set.** Common boxing-gym terms tied to what the app does, not invented compound words. The first shortlist (Touch Gloves, Stick and Move, Stay Busy, On the Pads) was rejected because the names read as coaching instructions.

**Corner gets a new name.** The App Store already had an app called "Corner" in Health & Fitness and several boxing apps with Corner in the name.
- Why: a name people can find and remember.
- What stays: the internal App Store identity (nobody sees it) and the recorded line "This is your corner. Let's get to work."

**The rename protects everything users already have.** Decided on my behalf:
- Saved workouts, combos and settings stay where they are. Moving them risks loss for no visible gain.
- New share links use the new name; links already sent keep working.
- Share codes keep their format, so older builds can still read them.

**Copy and popups reworked from a boxer's point of view.** With the Card look approved, a review showed every line had been carried over from the old app unexamined.
- How: every screen walked as two boxer personas, plus a second independent walk, then checked against Apple's writing and alert guidance.
- Result: seven behaviour changes and about 30 wording changes, approved as a set.
- **Correction, decided on my behalf:** popups follow Apple's guidance (the likely choice on top, Cancel at the bottom, red for anything destructive), replacing an earlier rule that contradicted it. Cost: none; nothing was built yet.

**Card approved as the new design system; sequence set.**
- Why Card: brightness tells round from rest at a glance (12 to 1, against 1.3 to 1 in the old look).
- Sequence: final visual sign-off; then broken things fixed in the old look (Batch A); then the Card build with remaining audit fixes built inside it. Nothing is drawn twice.

**Design phase: audit, then a new design system.**
- Decided on my behalf: every past direction ruled out first; seven fresh starting points drawn up and one picked at random, so the proposal isn't just the obvious idea.
- The design audit found the live combo call shrinking to a speck on standard-size iPhones.
- **Process change:** every decision needed from me goes in one numbered list, with the count first, each item self-contained and naming exactly where to look.

## 13 November

**Native audio plan approved with every recommendation.**
- A call, Siri or an alarm pauses the workout until Resume. Cost: after Siri on a locked phone, the user unlocks to resume.
- Headphones disconnecting pause the workout, as Apple expects; combos shouldn't suddenly blare from the phone.
- Paused on screen: nothing plays and music stays lowered. Cost: a long pause keeps music low.
- Bluetooth delay is measured first and corrected only if noticeable.

**Two engineering rulings during the build.**
- A line not ready within 1.5 s of its moment is skipped, not spoken late.
- Lines are scheduled 50 ms ahead instead of 10 so the measured delay stays honest. Cost: about 40 ms, inaudible.

**Sequence: audio fix, then audit fixes by priority, then a redesign.** Audio first because it blocks resubmission. Purely visual findings wait for the new design system.

**End-of-workout screen and empty Saved state.**
- End of workout: Save, Run again and Send as three equal buttons; Done is the one primary. After saving, Save becomes "Saved" in place.
- Empty Saved: a short line and a Paste a code button.
- Why: one obvious next step, large targets for gloved hands, and new users learn that saving and receiving workouts exist.

**All 20 UX audit findings marked Fix, including six polish items.** Cost: each polish item joins the batch that touches the same screen.

**The count-in is spoken aloud.** Gloved up with the phone across the room, you hear the count rather than read it.

**Pausing makes no sound.** The user pressed it, so they already know.

**A full UX audit before more design work; mockups before code for anything big.** An earlier redesign went straight to code and was rejected.

**Working process adopted.** WHEN/THEN specs before screen changes, screenshots before "done", tap-through tests, save at every passing step, tests never weakened, one phase per session, an independent reviewer for every task. See [process.md](process.md).

**The new engine stays on its own branch until the release check passes.**

## 12 November

**Rebuild the training engine before fixing audio or design.**
- Why: App Review rejected the build under guideline 2.5.4 (background audio). The root cause was the audio library switching off the audio session after every clip. Patching would have stacked fixes on a timer design that drifted and double-fired.
- Plan: four plans in order (engine core, native audio, tailored sessions, background coaching and resubmission), each task built and independently reviewed.

**One "Keep coaching in the background" switch, off by default,** for locking and switching apps alike. iOS can't reliably tell them apart.

**Our own native audio player, playing the existing recorded voice.** Fixes the rejection's cause at the root.

**Music is lowered for the whole workout,** rather than dipping for each line.

## 11 November

**No AI in the engine; the trainer stays rule-based.** A model would cost roughly $0.01 to $0.94 per active user per month and need a server, a connection and an extra consent step under Apple's rules.

**The trainer's own combos never slip inside** (toward the opponent's power hand). Combos users build play exactly as built.

**Boxing rounds have no ending block.** Combos run to the bell.

**Eval rubric v2.** Three pass/fail gates and six scores, never averaged. Code checks anything with a right answer; the AI proposes judgment calls; I have the final word. The AI grades alone only after 80 percent agreement with me. See [examples/eval-rubric.md](examples/eval-rubric.md).

## Before 11 November (summary)

- No accounts and no ads.
- Works fully offline.
- Recorded voice instead of the phone's system speech.
- A combos-only trainer (form advice between combos removed after grading).
- Coach Mode: build a session block by block and script each round.
- Analytics opt-in.
- Terms and training-disclaimer agreement on first launch.
