# Example: a reviewed build plan (the Card build, excerpt)

> **What this is.** Excerpts from the plan to rebuild every screen of Mitts in the new Card design system, written on 16 November. One helper wrote it; a second reviewed it, and it was revised with that review and with the findings of an iPhone SE check run the same evening. The full plan has 40 tasks in seven phases, each with exact files, tests and evidence, which is engineering detail from the private codebase and is omitted here.
>
> **What to notice.** The plan opens with the decisions only the owner can make, each written to stand on its own (what it is, where to look, a recommendation, what yes and no each mean). Then the calls made on the owner's behalf. Then a "review focus" naming the risks no single task's tests would catch, and who owns each. The app is one coherent look at every phase boundary, and the owner's waiting time is shown separately from build time.
>
> **Outcome.** I approved all five recommendations on 17 November.

---

**Goal:** rebuild every screen of Mitts in the signed-off Card design system, with its reworked words, its seven behaviour changes and the remaining audit fixes, without changing the engine, saved data or spoken lines.

## Open questions for the owner (5)

These go to the owner in one numbered list, with the WHEN/THEN document, before any code changes. Each has a recommendation; one approval accepts all five.

1. **A friend's notes before the countdown: a way back out.**
   What it is: a workout with notes from a friend now opens on the notes, with one button, "Start countdown". The picture has no way to back out if you opened the workout by mistake.
   Where to look: spec page, section "Workout", phone "Notes before the countdown".
   Recommendation: add a plain "Not now" under the button. It takes you back where you were, with no popup, because nothing has started.
   Yes: "Not now" is added. No: the only way out is to start the countdown, pause, then stop.

2. **The line that says why the phone paused the workout.**
   What it is: the approved line for a lock is "To keep going with the phone locked, turn on Keep coaching in the background in Settings." Two things make it untrue as written. Settings can't be reached from a workout, and a workout uses the settings it started with, so turning it on mid-workout changes nothing until the next one. And iOS can still freeze Mitts for people who already have that switch on, so they would be told to turn on something that's already on.
   Where to look: spec page, section "Workout", phone "Paused by the phone".
   Recommendation: three lines, picked by what happened:
   - a call, an alarm or headphones: "Paused for a call, an alarm or your headphones. Resume when you're ready."
   - locking or leaving with the switch off: "Paused when you left Mitts. To keep going with the phone locked next time, turn on Keep coaching in the background in Settings."
   - locking or leaving with the switch on, when iOS froze Mitts anyway: "Your iPhone paused Mitts while it was in the background. Resume when you're ready."
   Yes: those three lines. No: one line for both lock cases, exactly as approved.

3. **The iPhone SE, and the longest combos.**
   What it is: the spec draws the large iPhone and the 390-point phones. The SE's screen is shorter, and tonight's SE check found today's app already breaks there: a 7-line combo puts the clock over the progress bar and the previous combo inside the Pause button; a 9-line one cuts its last letter; the paused list runs behind Stop and Resume. Card's bigger card leaves even less room. The spec also says a combo "never shrinks below 64 points", which the longest combos people can build (ten moves like "Roll under") can't meet on any phone while the full card is showing.
   Where to look: spec page, section "Workout", phones "Round" and "Round, on a 390pt iPhone"; the worst three SE screenshots on the ask's page.
   Recommendation:
   - On every phone, when a combo is too long to fit under the full card, the card shrinks to its small height, stays lit, and shows a smaller clock. The combo gets the room and is shown whole at 64 points.
   - On the SE the full card is shorter, with the 390-point phone's clock size.
   - Only when a combo still doesn't fit at 64 with the small card (only saved combos of seven or more word moves, on the SE), it steps down to 48 points. It's never cut and never under the buttons.
   - The trainer's own combos are three lines or fewer and never reach any of this.
   Yes: built that way, and you see every case on the SE in Phase 3's screenshots. No: tell me which part to change, or I draw the SE screens on the spec page first (about half a day) and wait for your yes.

4. **Deleting a round or a cardio part while building.**
   What it is: today this asks "Delete this round? Its combos go with it." The approved popup rule is "no popup for anything you can undo", but the approved list of changes only names workouts and combos.
   Where to look: spec page, section "Words and popups", panel "Popup rules", first line.
   Recommendation: follow the rule. It deletes straight away, you land back on Build, and a toast says "Round deleted" with Undo.
   Yes: the popup goes and Undo replaces it. No: the popup stays, restyled, with its buttons reworded to "Delete round" and "Cancel".

5. **Approve eight lines the spec doesn't cover.**
   What it is: lines that today say "block", or that only exist because of a new behaviour. For example:
   - Paused in a warm-up, cardio or cool-down, the button on the card reads "Restart warm-up", "Restart cardio" or "Restart cool-down".
   - Saving a combo mid-workout when My combos is full: "My combos is full (50). Delete one in your library to save more."
   - A combo the trainer called that's already in My combos shows "Saved" from the start, so it can't be saved twice.
   - VoiceOver's reading of the row of small cards: "3 of 6 rounds done".
   Recommendation: approve all eight.
   Yes: built as listed. No: tell me which lines to change.

## Decided for you (selected)

Calls that don't change what people see, hear or pay, or that the owner has already made. Each is logged in the decision log by name.

- **Menus, switches and sliders are the iPhone's own.** On iOS 26 the iPhone draws them in its glass style, so they won't match the pictures exactly. They're tinted, not reshaped.
- **When you pause yourself and then a call or headphones change takes the sound,** the line under the card switches to the call line, because the sound route may have changed and the line explains why.
- **A workout with notes counts as started (in usage data) when the countdown starts,** not when its screen opens, so backing out with "Not now" isn't counted. Nothing new is sent, and it still passes through the usage-data consent.
- **The design tokens are swapped in place,** with today's names kept as aliases until the last phase, so the app is never two looks at once. Cost: the first two phases touch every screen at once, so each ends with a screenshot sweep of the whole app.
- **"Has finished a workout" is one new on-phone yes/no,** used only to decide when to ask about usage data. It isn't workout history: no date, no count.
- **There is no phone check and no TestFlight build until the last phase is finished.** The owner can see the in-between app on his phone, but nothing is asked of him there before then.

## Review focus

The five things most likely to hurt a person using the app that no single task's tests would catch on their own. Each is pinned to the task that owns the code.

1. **A phone left on an older build's data.** A saved workout with a rest length no longer offered, a 60-character name, a retired focus or a retired move must still open, read correctly everywhere, and run.
2. **The workout screen under a suspension or an interruption.** After a lock, a call or headphones coming out, the card must show paused with the right reason line, never a lit running card over a stopped clock.
3. **The SE at the largest text.** Every screen scrolls, no label breaks mid-word or is cut, nothing sits under a bottom bar, and the workout card keeps its numerals.
4. **The one-text share message pasted whole.** A friend on any build since the rename, or on the old build, must be able to paste the entire message and get the workout, including when Messages turns line breaks into spaces.
5. **The usage-data ask never asked twice and never lost.** Someone who answered under the old popup never sees the new card. Someone who hasn't answered sees it after their next finished workout until they answer. A crash-resume launch doesn't lose it.

## What the app looks like at each phase boundary

| After | The app |
|---|---|
| Phase 1 | Every screen in Card's colours and typefaces, still in today's shapes and layouts. One look, usable, clearly unfinished. |
| Phase 2 | Every button, bar, field, switch, popup, toast and row is Card's. Screens keep today's arrangement. |
| Phase 3 | The workout screen is finished on all three phone sizes. |
| Phase 4 | The whole everyday path is finished: Welcome, Train, a workout, Complete. |
| Phase 5 | Building is finished too. |
| Phase 6 | Saving, sending, receiving, Library and Workout detail are finished. |
| Phase 7 | Everything is Card. Old names removed, documents rewritten, release checks run. The first build that may go to TestFlight. |

## Phases and estimates

Build time is measured in sessions, from batch A's pace: 8 tasks, each built and reviewed, in about 4 hours. Owner-wait time is shown separately; it isn't work, but it decides the calendar.

| Phase | What | Tasks | Build sessions | Owner waits |
|---|---|---|---|---|
| 0 | Approvals: the WHEN/THEN document, the five questions | 2 | 0.5 | **Blocking:** one answer before Phase 1 code starts |
| 1 | Foundations: colours, type, icons, whole-app sweep | 5 | 1 | None blocking; evidence page |
| 2 | The kit: every shared component, sweep | 7 | 1 to 1.5 | None blocking; evidence page |
| 3 | The workout screen | 7 | 2 | None blocking; evidence page with the SE cases |
| 4 | Complete, Train, the usage-data card, Welcome and first launch | 4 | 1 | None blocking; evidence page |
| 5 | Build, the round editor, Combo Builder | 4 | 1.5 | None blocking; evidence page |
| 6 | Save, Paste a code, Send, Add this workout?, Library, Workout detail | 5 | 1 to 1.5 | None blocking; evidence page |
| 7 | Settings; large text, motion and VoiceOver; old names removed; tap-through flows; documents; finish review | 6 | 1.5 | **Blocking:** yes to a TestFlight build, then the owner's phone check |
| | **Total** | **40** | **9.5 to 11.5 sessions** | **Two blocking waits** |

If the owner's feedback on a phase asks for changes, they fold into the next phase's first task. They don't reopen the finished phase unless something is broken.
