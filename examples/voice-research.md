# Example: coach-voice research (summary)

> **What this is.** A plain-language summary of the research behind the trainer voice rebuild (15 to 17 November), written for this repo from the working notes. The coach recordings were used only as a measuring stick: measured, never reused as audio.
>
> **What to notice.** "Make it sound natural" was turned into measurable targets from real coaches. Candidate methods were scored against those targets before anyone listened. Then the owner's ear made the final calls, round by round, and each issue was traced to a measurable cause before it was fixed.

---

## The question

The trainer's voice is synthetic: 439 lines rendered in two voices (878 clips) by an open-source text-to-speech model, played from recordings so the app works offline. I flagged two problems: the voice sounded scratchy, and combo calls didn't sound like a coach. I set the direction to match real coaches before rebuilding anything, so the team first had to define what "like a coach" means.

## How real coaches call combos

**Sample:** 280 combo calls from nine coaches in public coaching videos, plus two audio-only training products. Medians per coach.

- **Every number sits nearly level.** The first number is a fraction of a semitone below the call's average and the last a fraction above. No staircase.
- **Inside each number:** the first rises and eases; middle numbers dip gently; the last rises briefly, then falls.
- **Endings:** level 48 percent of the time, falling 40 percent, rising 9 percent. No question-like lifts.
- **Loudness is even.** The last number is, on median, exactly as loud as the first.
- **Almost no gap between numbers** (about 20 ms), about 3.5 items a second, with the last number held about a quarter longer.
- **Two styles:** coaches who climb through a combo, and coaches who finish falling. The audio-only products both use the falling finish.
- Consistent with published research on how people read digit groups and calling contours, and on low endings signalling finality.

## How our voice compared

- The natural render read combos **like a list:** starting about 4 semitones high and stepping down to about 3.5 below, fading about 5 dB, with pauses of 120 to 880 ms between numbers.
- The shipped clips had been flattened to fix that, which made them level but buzzier.
- The scratch was in the voice model's own output: hiss on "t", "th" and breaths roughly twice as loud to the ear as the coaches', a faint whine at three fixed pitches, and raspier male vowels.

## Candidates, scored before listening

| Method | Result |
|---|---|
| **Reshape the natural render's pitch to the coach pattern** (a standard signal-processing technique) | Closest on every measure that separated good from bad; every word right; no added buzz. Remaining gaps: narrower pitch range than coaches, a few leftover pauses. **Chosen.** |
| The same, with a different vocoder | Same shape, measurably buzzier. Not for production. |
| Steering the model with text or phoneme tweaks | No change. |
| Rendering each number separately | Choppy, and one wrong word. |
| A newer open-source voice model | Slow, still list-like, wrong words in 11 of 40 takes, identity drift, many hours for the full set. Dropped. |
| Voice conversion from coach audio | Demo only. Production would need a recording we have the rights to. Not pursued. |

**Constraints set by me:** only tools that allow commercial use at no cost; anything paid comes back to me first. The chosen rebuild was estimated at one to one and a half hours of machine time for all 878 clips, with no spend.

## Seven listening rounds

Each round was a page of side-by-side samples. Each issue I raised was traced to a measurable cause before it was fixed.

| Issue raised | What was found | What changed |
|---|---|---|
| Residual scratchiness | Hiss, whine and rasp in the model's output, measured against the coaches | A cleaning pass that brings hiss to the coaches' level and removes the whine |
| Short combos didn't end high | The model drops its pitch sharply on the last word of every call, whatever the punctuation; the earlier fix only softened the drop | Short combos rise and finish high |
| Different combo lengths suited different rounds' treatments | Longer and shorter combos want different endings | A length-based ending rule; only punches count toward the length |
| Two-punch combos climbed artificially instead of stressing the second punch | Also: the tool that locates words placed some up to 60 ms early, so emphasis landed on the wrong number | Two-punch combos stress the second punch (louder, longer) instead of climbing; word boundaries moved into the real gap between words |
| The female voice sounded robotic | Her natural pitch movement had been squeezed to under half, and her vocal vibration made unnaturally even (jitter 1.3 to 1.7 percent against a natural 2.1 to 2.6) | Round 7 restores both to her natural levels |
| (Tried along the way) | A well-known neural audio tool kept the voice exactly, rasp included | Dropped |

**Outcome: shipped.** I approved round 7 for both voices. Every line was rebuilt in both voices (878 clips) by a resumable pipeline behind an automated quality gate: word accuracy (by speech recognition), glitch and robotic-jitter checks, emphasis on the final punch, and peak level and loudness against the previous clip. A clip that failed its checks fell back to its natural recording instead of shipping a bad reshaped version, and every fallback was listed. Clips that only "failed" because speech recognition misheard a very short line (which the old clips failed the same way) were accepted and listed. The rebuilt voice is in the test build for both voices.

**Known limit:** combos people build themselves are stitched from single-word recordings, so they keep a list-like delivery until a later change to how the app joins them.
