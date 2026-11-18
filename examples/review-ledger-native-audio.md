# Example: a review ledger (the native audio plan, excerpt)

> **What this is.** While a plan is being built, every engineering call made without me is written down as a ruling, with its cost if wrong. When the plan finishes, a fresh reviewer checks the whole branch, and anything not fixed is listed with the reason. This excerpt is from the plan that replaced the app's audio library with our own native iPhone audio player (13 to 14 November). The rulings are paraphrased into plain language; code references are removed.
>
> **What to notice.** Each ruling names the trade-off. The final review is a verdict ("with fixes"), not a vibe. Deferred findings aren't dropped: they're written down with the reason they can wait, and some are tied to a later check that will measure them.

---

## Rulings, in the order made (selected)

- **The order of operations when a line starts was changed from the brief.** The builder measured the brief's order slipping by one audio buffer in 30 of 30 offline runs, and the reordered version on time in 30 of 30. The brief's order was an implementation hint; the spec wants the line on time. *Cost if wrong: none observed.*
- **Lines are scheduled at least 50 ms ahead, not 10.** On the development Mac, shorter leads started lines 9 to 14 ms late without the delay log reporting it; 35 ms or more was on time. 50 ms keeps the reported delay honest and is inaudible as a constant offset. The device check measures it on a real phone. *Cost if wrong: every line starts about 40 ms later than strictly needed (inaudible), or on device the lead is still short and lines start a little later than reported.*
- **A clip that exists but missed its 1.5 s window is dropped, not handed to the system voice.** A late cue is a late cue, and the engine's rule drops late cues rather than speaking them late. The never-silent rule covers lines with no recording at all. *Cost if wrong: a line can be silent if its clip took over 1.5 s to load, plausible only on a development build.*
- **A reviewer's minor finding was promoted to a must-fix:** a voice preview stopping from another screen could, in a narrow timing window, cut off the first line of a workout that had just started. That is exactly the class of bug this plan exists to remove. *Cost if wrong: a few lines and two tests.*
- **Another minor finding promoted:** an unhandled error in the play step could mean silence. The never-silent rule is absolute for spoken lines, so it must fall back to the system voice. *Cost if wrong: one error handler.*
- **The push to the private backup happens after the final whole-branch review,** not inside the last task. The process backs up after each batch, and the batch isn't done until the final review. *Cost if wrong: none.*
- **The audio buffer size stays small for now.** A reviewer suggested a larger one for battery life. There's no device measurement either way yet; the later soak test measures battery. *Cost if wrong: some extra battery on long locked workouts until then.*

## Final review

- **Verdict: with fixes.** Important findings: a configuration change could cut off a live line; wired USB-C headphones unplugging weren't detected; two user-facing rulings were missing from the owner's decision log. Several minor findings were promoted into the fix wave (a preview playing over a workout, a crash window, a failed release with no retry, among others).
- **Fix wave:** every finding addressed, each with a test where testable. **Re-review: clean.**

## Deferred findings (selected)

About 40 minor findings were written down rather than fixed, grouped by task. A few, to show the shape:

- An audio-format conversion branch is untested, because no real asset reaches it.
- A delayed "app was suspended" interruption is forwarded as a real interruption (Apple advises ignoring those). Low impact; noted for later.
- USB-C wired headphones may report themselves as a generic audio device, so unplugging them might not pause. Added to the release checklist's headphones check.
- A timing test assumes 30 ms of slack and could flake under load. Watched, not changed.
