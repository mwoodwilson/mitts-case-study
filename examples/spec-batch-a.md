# Example: a WHEN/THEN spec (Batch A)

> **What this is.** The spec for Batch A, the first set of fixes after the UX audit, as it was sent to me for approval on 15 November. It's reproduced here with light edits. The app was still called Corner, and "My Corner" was the name of the tab now called Library.
>
> **What to notice.** The one real decision is at the top, with options in a table, real examples and a recommendation. Calls already made inside approved fixes are listed separately so I can object. Every item is written as WHEN/THEN, covers large text, VoiceOver, offline and first launch where they apply, and says how it will be proved.
>
> **What's left out.** The original ended with a "For the builder" section: the root cause of each bug traced to specific code, the fix, and which existing tests would need updating. That's implementation detail from the private codebase, so it's omitted. The size estimate at the end is kept.

---

# Batch A: the broken things, fixed in today's look

Spec for the owner's approval, 2026-11-15. Nothing gets built until he approves it.

Batch A fixes what's broken today, in today's look, before the Card build starts. The app keeps the name Corner; the rename has its own plan. Words stay as they are today, except the few listed under "Words that change".

## 1 decision for you

**1. What should a workout be called when nobody names it?**

Today the automatic name is the workout's numbers, for example "2 × 1:00 · The basics" (21 characters) or "3 rounds + 2 cardio · 24:00" (27). That's too long for the 20-character name box, and in My Corner the name just repeats the grey line under it ("2 × 1:00 · The basics" over "2 × 1:00 · 15s rest · The basics"). The Card spec already approved "shorter automatic names that fit 20 characters and don't repeat the line under them". Only the wording is open.

| | Option A: the day (recommended) | Option B: the focus | Option C: shorter numbers |
|---|---|---|---|
| Rule | The weekday you trained, plus "rounds" | The focus, plus "workout"; Coach Mode is "Coach Mode workout" | Rounds × length and focus; Coach Mode is its round count, plus "+ cardio" if it has any |
| Quick Start, 2 × 1:00, The basics | Sunday rounds | Basics workout | 2 × 1:00, Basics |
| 12 × 3:00, My combos | Sunday rounds | My combos workout | 12 × 3:00, My combos |
| Coach Mode, 3 rounds and 2 cardio blocks | Sunday rounds | Coach Mode workout | 3 rounds + cardio |
| Longest possible | 16 ("Wednesday rounds") | 18 | 20 |
| Repeats the line under it in lists? | Never | The focus word | Yes, the numbers |
| In a text to a friend | "Sunday rounds", a Corner workout. | "Basics workout", a Corner workout. | "2 × 1:00, Basics", a Corner workout. |

- **Recommendation: A.** It never repeats the line under it, and it's how a leading running app names a run nobody named: "Sunday Morning Run". Cost: two workouts saved on the same weekday get the same name until you rename one, and a text to a friend says less about the workout than C does.
- **On yes (A):** automatic names read "Sunday rounds", "Tuesday rounds" and so on.
- **On B or C:** that rule is built instead. C is the most informative on its own, but it repeats the line under it everywhere, which is what the audit flagged.
- Either way, names already saved never change.

## Decided for you, inside fixes you already approved (say if any should change)

- **"Workout done" in today's look:** a plain grey bar at the top (the paused bar's grey, because nothing is live or resting), a plain dark background, and a big tick where the clock was. Not the off-white "Get ready" bar, because that's the bar that looked yellow at night.
- **Work on the end screen counts boxing rounds only.** Warm-ups, cool-downs and cardio blocks don't count. Why: it matches Build's "boxing" total and the Card spec's approved label "Boxing time".
- **"Switch to Simple?" counts every block and drops the word "scripted"** (see 5b).
- **A name that's already over 20 characters** (saved by an earlier version) can be shortened and edited freely; letters can be added once it's 20 or under. Nothing gets cut automatically.
- **Copies ("name (2)") also fit 20**, by dropping whole words from the end of a long name.
- **Toasts leave when you leave their screen.** The Card spec already says this.
- **The Stop workout? popup closes by itself at the final bell**, so it can't offer to stop a workout that has already finished.
- **The same safe minimum size goes on three other places that shrink their text to fit:** the workout clock, the end screen's numbers and the labels in pick-one bars. None of them showed the speck in the 121 captures, but they have the same hidden cause as the live call.

## Words that change (all of them)

- Automatic names (your pick above).
- "Workout done" replaces "Rest" after the final bell.
- "Switch to Simple?": "Your 3 scripted blocks stay in the draft: switch back to Coach Mode to get them." becomes "Your 3 blocks stay in the draft: switch back to Coach Mode to get them." With one block: "Your 1 block stays in the draft: switch back to Coach Mode to get it."

## For every item

- **Offline:** nothing in batch A uses a connection, so all of it works in airplane mode.
- **First launch:** nothing behaves differently on first launch.
- **Proof:** before-and-after simulator screenshots of every changed screen, on a large iPhone and a 390-point iPhone, at normal and at the largest text size. Automated tests where listed. All tests and the type check pass at every step.

---

## 1. Automatic names

**Today:** a workout run without a name gets a name built from its numbers, often over 20 characters. Save and Send on the end screen open with that name already in the box, and the box freezes: you can't type, and you can't delete one letter at a time either, only select a chunk and delete it. Rename on a workout's page freezes the same way for names saved like that (it shows "21 / 20").

- WHEN a workout with no name of its own ends, THEN the end screen's title is the automatic name, never more than 20 characters.
- WHEN I tap Save or Send on the end screen, THEN the name box opens with that name in it, and I can type, delete or change it straight away.
- WHEN I send from Build without typing a name, THEN the grey hint in the name box is the automatic name and the workout goes out under it.
- WHEN I save from Build, THEN the name box starts empty and Save stays greyed until I type, as today.
- WHEN a workout already has a name (saved, received, or typed in Coach Mode), THEN that name is used, as today. An automatic name never replaces a real one, and names already saved never change.
- WHEN I duplicate a workout, or add a copy of one I already have, THEN the copy is called "name (2)" and still fits 20. A long name drops whole words from its end to make room: "Saturday pad rounds" becomes "Saturday pad (2)".
- WHEN I open Rename or Send for a workout whose name is already over 20, THEN I can delete and change letters freely, and add letters once it's 20 or under. The counter reads "21 / 20" until then. Saving without changes keeps the whole name.
- WHEN I type a name the word filter blocks, THEN it's refused, as today.
- Large text: the end screen's title wraps onto a second line if it needs to; the name box scrolls sideways, as today.

**Tested by:**
- Automated: every Simple setting and a few thousand random Coach Mode workouts give a name of 20 or fewer; the exact example names; copy names; the "delete freely, add only once it's under 20" rule; a guard that the end screen, Send and Build all use the one name maker.
- Tap-through: a Quick Start run, then Save, then type a letter, which is accepted. Today typing is refused at this exact spot.
- Screenshots: the end screen's title, Save with the name filled in, and Rename on "2 × 1:00 · The basics".

## 2. "Workout done" after the final bell

**Today:** after the final bell, while the trainer signs off, the screen says "Rest" in steel blue, a clock counts the sign-off down ("0:08"), and Pause sits greyed out at the bottom.

- WHEN the final bell rings, THEN the bar at the top says "Workout done", with the workout's name under it when it has one. The bar is plain grey and the background goes plain dark, not rest blue.
- THEN a big tick stands where the clock was. There's no countdown.
- THEN there's no Pause or Notes button, and the bottom of the screen is empty. The progress bar shows every block done.
- THEN the trainer signs off as today, and the end screen opens when he finishes, as today.
- WHEN the last block is cardio or a cool-down, THEN the same.
- WHEN the Stop workout? popup is open as the final bell rings, THEN it closes by itself and Workout done shows.
- WHEN the phone locks or I switch apps during the sign-off, THEN the workout finishes as today and the end screen is waiting when I come back.
- WHEN VoiceOver is on, THEN it reads "Workout done" and the name.
- WHEN Reduce Motion is on, THEN the bar changes with today's short fade.
- Large text: "Workout done" grows up to the workout bar's limit (1.35 times) and fits on one line on a 390-point iPhone. The tick doesn't grow, like the clock.
- WHEN I stop early, THEN I go to the end screen (or back) as today. Workout done only ever follows the final bell.

**Picture first:** one picture of this screen in today's look before it's built (the process rule: mock anything visual first).

**Tested by:**
- Automated: the workout screen test, which runs the real screen on a simulated clock, checks that after the final bell the bar says Workout done, there's no clock and no Pause, the stop popup closes, and the end screen follows the sign-off.
- Screenshots: the sign-off on both phones, at normal and largest text.

## 3. Toasts stay above buttons

**Today:** a toast is the dark strip at the bottom ("Saved to My Corner, View", "Workout deleted, Undo"). It follows you to the next screen and stays for its full time. On some screens it sits right over the main button. Delete a workout in My Corner, open Paste a code, and "Workout deleted, Undo" covers Check code.

- WHEN a toast shows on a screen with buttons along the bottom, THEN it sits just above them and never covers them.
- WHEN a toast shows on a tab screen, THEN it sits above the tab bar, as today.
- WHEN I leave the screen while a toast is showing, THEN the toast goes with that screen. It never follows me.
- WHEN the toast comes from the step that moved me (Saved to My Corner after Save, Sent, Workout deleted from a workout's page), THEN it shows on the screen I land on, for its full time.
- WHEN the keyboard is up, THEN the toast sits above the keyboard.
- WHEN text is at the largest size, THEN the toast grows upward and still clears the buttons.
- WHEN VoiceOver is on, THEN the toast stays 15 seconds and its button gets focus, as today.
- Errors never show as toasts, as today.

**Tested by:** placement and ownership rules tested as plain logic; a guard that every screen with a bottom button bar lifts toasts above it; a tap-through of the Paste a code case; screenshots.

## 4. The live combo call shrinks to a speck

**Today:** on a 390-point iPhone (like most current iPhones), the combo the trainer just called is drawn a few points tall, while the grey previous call under it reads fine. The same happens on a large iPhone at the largest text size. The call is meant to shrink for long combos but never below 44 points. iPhones ignore that floor, so once it starts shrinking it goes all the way down.

- WHEN the trainer calls a combo, on any iPhone, THEN it shows at full size: 56 points (72 on the biggest phones).
- WHEN the combo is long (more than 18 characters), THEN it steps down to 48 points and wraps, never below 44 points, centred, with the previous call still under it.
- WHEN the longest combo a round can hold is called (10 moves with slips and rolls), THEN all of it shows, at 44 points or more, on a 390-point iPhone.
- WHEN text is at the largest size, THEN the combo stays exactly as big as at normal text, and it still never goes below 44.
- WHEN a round has just started and nothing's been called yet, THEN the space is empty and nothing jumps, as today.
- WHEN a cardio block shows its activity in the same place ("Jump rope"), THEN the same size rules apply.
- WHEN the workout clock, the end screen's numbers or a pick-one bar's label run out of room, THEN they stop shrinking at a safe minimum instead of collapsing.

**Tested by:** the size rule as plain logic (every call length gives 44 points or more, and short calls get full size); a guard that no screen relies on the floor setting iPhones ignore; screenshots on both phones, with a 10-move scripted combo and a cardio block.

## 5. Five small bugs found while capturing

### 5a. The Coach Mode notes box hides behind the keyboard

- WHEN I tap the notes box, THEN the page scrolls so the whole box sits just above the keyboard.
- WHEN I type more lines, THEN the box grows and the line I'm typing stays above the keyboard, up to the 300-character limit.
- WHEN I close the keyboard, THEN the page settles with Start and Save at the bottom, as before.
- WHEN the notes break the word filter, THEN the red line under the box shows when I leave the box, as today.

### 5b. "Switch to Simple?" miscounts

- WHEN I switch to Simple with a Coach Mode draft, THEN the popup counts every block and doesn't call them scripted.
- WHEN there's one block, THEN it reads "Your 1 block stays in the draft: switch back to Coach Mode to get it."
- WHEN the draft has no blocks, THEN there's no popup, as today.

### 5c. Coach Mode's end screen counts the warm-up as work

- WHEN a Coach Mode workout ends or stops early, THEN Work counts boxing rounds only.
- WHEN I stop within 5 seconds of the final bell and the last block is a cool-down or cardio, THEN it still counts as finished, and that block adds nothing to Work.
- WHEN a workout is resumed after the app was closed, THEN Work carries on counting the same way.

### 5d. The locked My combos chip jumps the page to the top

- WHEN I tap the greyed My combos chip and its note is already on screen, THEN the page stays where it is.
- WHEN the note is below the bottom of the screen, THEN the page scrolls down just enough to show it.
- The page never scrolls up for this.

### 5e. Settings switches sit 4 points from the edge

- WHEN I open Settings, THEN every switch's right edge lines up with the right margin every other row uses.
- WHEN the phone runs an older iOS, which draws the smaller switch, THEN the switches line up there too.

---

## Not in batch A

- Anything in the Card look, and the remaining audit batches (they're built inside the Card build).
- The rename.
- Card's new words, such as "Boxing time" for Work and "Go again" for Run again.

## Size

| Item | Estimate |
|---|---|
| 1. Automatic names, copies and the name box | half a day |
| 2. Workout done (after its picture is approved) | 2 to 3 hours |
| 3. Toasts | half a day |
| 4. The live call | 2 to 4 hours, most of it simulator proof |
| 5a to 5e | half a day together |
| Screenshots on two phones at two text sizes, and the report | half a day |
| **Total** | **about 2 to 2.5 working days** |

*Outcome: approved with Option A. The build ran as eight tasks, each built and independently reviewed, in about four hours.*
