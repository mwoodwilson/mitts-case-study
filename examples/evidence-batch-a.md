# Example: an evidence list (Batch A, excerpt)

> **What this is.** After Batch A was built, the team wrote an evidence list: for every item in the spec, the before shot, the after shots, the automated tests, the tap-through flows and any gap. It was corrected the same day after the final review's fix wave. This excerpt keeps the structure and the parts most useful to a reader; file names and code references are removed.
>
> **What to notice.** Every item names its gap, even when the gap is "none". Every existing test that changed is listed with why it isn't weaker. And it ends with what still looks wrong on screen, which nobody asked for.

---

**Phones:** a large iPhone and a 390-point iPhone, both on iOS 26, at normal text and the largest accessibility text size. An older iOS version for the Settings switch check only.

## Item 4: the live combo call shrinks to a speck (excerpt)

- **Before:** the 390-point phone at normal text (the speck); the large phone at the largest text.
- **After:** the live call on the 390-point phone at normal text (six shots); the large phone at the largest text (four shots); the longest combo the builder allows; a cardio block live.
- **The clock, the end screen's numbers and pick-one labels.** The first build gave these a "safe minimum size" through a setting that, it turned out, the framework never sends to the phone, so it did nothing. The fix wave removed shrink-to-fit from all three and sizes them instead. After shots: a live round clock at full size, the same at normal and largest text; a 20-minute warm-up clock; the end screen at normal and largest text; Build's pick-one bars at three text sizes, stacking at the largest.
- **Automated:** the size rule for every call length; the longest call shown whole; the clock fits a 375-point phone for every time up to 20:00; pick-one labels wrap to two lines and keep the 44-point target; guards that nothing in the app uses shrink-to-fit.
- **Gap:** no 375-point phone was checked on screen. The clock and the end-screen numbers are proved for 375 points by arithmetic in the tests; for pick-one labels see "Things that look wrong" below.

## Item 5c: Coach Mode's end screen counts the warm-up as work

- **Before:** Work 2:00 with a 1-minute warm-up and one finished round.
- **After:** a 2:00 warm-up plus two 1:00 rounds, stopped during round 2. The end screen reads Rounds 1 / 2 and Work 1:00. The warm-up isn't counted. (The spec's example used a 1:00 warm-up; the builder only offers warm-ups in steps from 3:00, so 2:00 was used. The result is the same.)
- **Automated:** warm-up, rounds, cardio and cool-down give Work equal to the rounds alone, finished and stopped early, plus the stop-near-the-end case.
- **Gap:** none.

## Tap-through results

| Flow | Phone | Result |
|---|---|---|
| Automatic names | Large | pass |
| Toasts | Large | pass |
| Notes above the keyboard | Large | pass |
| Notes above the keyboard | 390-point | pass |
| Workout audio across background, pause and resume | Large | pass |

## Existing tests that changed

Every one updates a test for an approved change; none was weakened.

| Test | What changed |
|---|---|
| Coach Mode's default name (2 tests) | Replaced by tests of the new one name maker. Intent kept: Coach Mode and Simple get their default from one place. |
| Share and Save keep the name the workout ran under | Two source checks became one, for the new name maker. The other checks are untouched. |
| Typed names are capped at 20; stored names keep up to 60 | Checks the new editable-down rule. The 20 and 60 limits are still asserted. |
| Session rule 12 (work time) | Sums completed boxing rounds instead of all work phases, per the approved change. |
| Three workout screen tests about the final bell | "Rest" became "Workout done". In one, "Pause is disabled and there's no Stop" became "no buttons at all", which is stricter. |
| The longest call test (title only) | The title now says exactly what the test asserts. The assertions are untouched. |

## Check results

- Type check: clean.
- Automated tests after the fix wave: 789 passed, 2 skipped (769 before the fix wave; 704 at the start of the batch). No flaky test failed.

## Things that look wrong on screen (not fixed here)

- **390-point phone, largest text, end screen:** two stat tiles start below the fold, under the three stacked buttons, so their labels look cut off. One swipe brings them into view. The real problems: only about the top 60 percent of the screen is left for content, nothing hints that it scrolls, and scrolled content passes under the status bar with no background behind it.
- **Pick-one labels on a 375-point phone at a middle text size** (arithmetic, not seen): "Intermediate" is about 4 points too wide for its third of the control, so it will break inside the word rather than shrink.
- **390-point phone, largest text, Save screen:** with the keyboard up, the summary is hidden behind the save bar, whose label wraps onto two lines.
- **A stepper hint at a middle text size** is cut off ("30 sec ste..."). Not changed by Batch A.
- **Changing the text size while a screen is open** briefly shows old heights. A live-resize effect, not what a person sees after changing the size in Settings.

*These went into the Card build plan as requirements.*
