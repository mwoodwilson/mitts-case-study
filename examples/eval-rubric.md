# Example: the trainer eval rubric (v2)

> **What this is.** The rubric used to grade what the trainer says, version 2 (5 November). Reproduced with light edits; internal file references are removed. "The owner" is me.
>
> **What to notice.** Hard gates before scores. Scores reported separately, never averaged. Code does everything with a right answer; the AI proposes judgment calls with reasons; the owner has the final word. The AI earns the right to grade alone with a measured agreement rate. Versions aren't comparable across a rubric change, and the rubric says so.

---

> v2 replaces v1.1. The trainer changed under v1.1: advice lines were removed, the round's ending block was removed, the throwing room after a big combo dropped from 8 s to 6 s, and the never-slip-inside rule arrived. v1.1 scores are not comparable with v2 scores.

**What gets evaluated:** one simulated session, meaning a transcript of everything the trainer says, with timestamps, for one setup (level, stance, focus, Simple or Coach Mode). Sessions come from a fixed list of setups and fixed random seeds, so the same session can be re-run after a fix and compared line for line.

**Scoring order:** gates first, then six scores. A session that fails a gate fails, whatever it scores.

## 1. Gates (pass/fail, all checked by code)

| Gate | Fails when |
|---|---|
| **G1 Voice** | Any line has no recorded clip (the user would hear the robot voice) |
| **G2 Rules** | An owner rule is broken. **R1** a generated combo slips inside (orthodox: slip right; southpaw: slip left). Built combos are exempt. **R2** a Basics round calls a 4, 5 or 6. **R3** a beginner combo throws the same hand twice in a row (the double jab aside). **R4** a banned phrase ("level change"). **R5** a Coach Mode round doesn't call its script exactly, in order |
| **G3 Shape** | A round's skeleton is wrong: no opener; a missing 60 s or 30 s warning; a call talking over a warning; a call still talking at the bell; a dead ending (more than 10 s of silence before the bell); a 4+ punch combo with under 6 s before the next call; a rest without its opener, ten-second line and 3-2-1; no finish line |

## 2. Scores (1 to 5 each, reported separately, never averaged)

Code scores D1 to D3 from countable facts. The owner scores D4 and D5, and can override any code score.

| | Dimension | 5 | 3 | 1 |
|---|---|---|---|---|
| **D1** | **Focus fit:** does the round drill what was picked? | 70%+ of calls carry the focus | 50 to 59% | under 40%: the focus had no visible effect |
| **D2** | **Level fit:** right difficulty for the level? | No calls out of level | One out-of-level call per round | The level had no visible effect |
| **D3** | **Variety:** does it keep moving? | No unplanned repeats within 4 calls; no combo over 25% of a round | One unplanned repeat per round, or one combo over 25% | A round cycles 3 or fewer combos |
| **D4** | **Pacing:** does the round flow like a real round? | Even, breathable rhythm; nothing crammed at the bell | One awkward spot | No sense of a round |
| **D5** | **Line quality:** does every line sound like a real cornerman? | Every line is a clean, sayable call | Two or three lines tagged | Six or more tagged |
| **D6** | **Built content:** is a user-built round called exactly as built? | Every built unit called in order | n/a | (any miss already fails R5) |

Scores 2 and 4 sit between the anchors.

## 3. Line tags

| Tag | Means | Set by |
|---|---|---|
| `INSIDE` | Generated slip toward the power hand (R1) | code |
| `LEVEL` | Out of level (D2) | code |
| `FOCUS` | Doesn't serve the chosen focus (D1) | code |
| `REPEAT` | Same combo again within 4 calls, not an intentional "Again" (D3) | code |
| `TIMING` | Talks over a warning or the bell, or a big combo without throwing room (G3) | code |
| `VOICE` | No recorded clip (G1) | code |
| `NOTCOMBO` | An instruction or advice, not a combo | judgment |
| `WORDING` | A real cornerman wouldn't say it that way | judgment |
| `CUT` | Shouldn't exist in any session | judgment |
| `PACE` | Right line, wrong moment (D4) | judgment |

## 4. Who judges

1. **Code** runs the gates, D1 to D3 and the code tags on every session. It never gets tired, gives the same answer every time and runs again on every change.
2. **Claude** reads every transcript and proposes judgment tags and D4/D5 scores, each with a one-line reason.
3. **The owner** agrees or disagrees with each proposal, adds anything missed, and has the final word.

Agreement between Claude and the owner is tracked per round. Once Claude matches the owner on at least 80% of tags and within 1 point on 80% of scores for two rounds running, Claude's judgment can run on its own across many seeds, with the owner spot-checking. Every judgment the owner makes that code could have made becomes a new code check.

## 5. What a round of evals produces

- Every transcript with its tags, gates and scores.
- The owner's verdicts, the fixes they led to, and the Claude/owner agreement rate.
- Fixes go in, the same seeds re-run, and the round is done when every gate passes and no score has dropped.
