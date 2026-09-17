# Muscle Exercise Recommender

A command-line Python tool. You type in a muscle, and it prints exercises
for that muscle **ranked from best to worst**, each with:

- **Mechanism** — the biomechanical/physiological reason the exercise
  works the muscle (muscle length, joint action, resistance profile, etc.)
- **Why it's ranked there** — the reasoning for its position relative to
  the other exercises on the list.

## Run it

```
python3 muscle_exercises.py
```

Then type ANY muscle name when prompted. The program never shows a menu
or list to pick from — it just asks "Enter a muscle to train" and you
type whatever you want (common name, gym nickname, or anatomical/Latin
name all work). Behind the scenes it matches your input against ~20
major trainable muscle groups covering essentially the whole body.

Type `quit` to exit.

## Broad body-region / multi-part words

If you type `legs`, `arms`, `torso`, `chest`, `back`, `shoulders`, or
`triceps` (or singular forms like `leg`, `shoulder`, `tricep`, or `trunk`
for torso), the program won't guess which specific muscle you mean — it
asks first, then you type your choice next. Specific names you already
know (like `lats`, `delts`, or `triceps brachii`) still work as direct
answers, bypassing the prompt.

- **Chest** asks: overall chest, upper chest, or lower chest
- **Back** asks: lats, traps, lower back, serratus anterior, or teres
  major & minor
- **Shoulders** asks: front delt, side delt, rear delt, or rotator cuff
- **Triceps** asks: long head, short (lateral) head, or medial head

## How the ranking works

Exercises are ordered using seven established exercise-science
principles:

1. **Stretch-mediated tension** — loading a muscle while it's lengthened
   tends to drive the strongest growth stimulus.
2. **Resistance profile** — whether tension stays high through the full
   range (e.g. cables) or drops off at certain joint angles (e.g. free
   weights at "easy" points in the arc).
3. **Direct vs. shared tension** — how much of the work goes to the
   target muscle vs. synergists/stabilizers.
4. **Range of motion** available at the joint(s) involved.
5. **Stability demands** — instability can divert effort away from the
   target muscle.
6. **Motor unit recruitment** — heavier, higher-force movements recruit
   more high-threshold (fast-twitch) motor units, which have the
   greatest growth/strength potential.
7. **Progressive overload potential** — how far an exercise's load, reps,
   or range can keep increasing over time before it's capped (e.g. by
   bodyweight or a light cable stack).

## Training protocol ("best method")

Every exercise now shows exactly how to train it — never a generic
"3 sets of X." Each one gets one of three protocols, chosen by injury
risk if pushed to true failure:

- **2 sets to true failure** — for machine/cable movements on a fixed,
  supported path where failure just means the weight stops moving.
- **1 set to true failure** — for free-weight isolation, unilateral, and
  bodyweight movements.
- **1 set x 5-8 reps, 1 rep in reserve (1 RIR)** — for heavy free-weight
  compound lifts (squats, deadlifts, heavy presses) where grinding to
  true failure carries real injury risk.

A few exercises (Farmer's Carry, Nordic Hamstring Curl, etc.) get a
tailored protocol instead, since they don't fit a normal rep scheme.

## Workout split builder

Ask for a "split" (or "workout split," "training schedule," etc.) to see
the full list of research-supported weekly training splits — Full Body,
Upper/Lower, Anterior/Posterior, Push/Pull/Legs, and Bro Split — at
different weekly frequencies, each with an example schedule and the
reasoning behind it.

You can also name a specific split family directly (e.g. `upper`,
`lower`, `ppl`, `full body`, `anterior`, `bro split`), typos included
(`fullbdoy`, `aterior`, `bro splt` all still work). These split-related
words always take priority over any similarly-spelled muscle name —
typing `upper` always means the Upper/Lower split, never "upper chest."

If a family has more than one frequency variant (e.g. Upper/Lower has a
4x/week and a 6x/week version), the program asks which one you mean and
remembers that you're mid-question — so your next answer (just `4x`,
`6`, `3.5`, `x2`, etc.) resolves correctly on its own, without needing
to repeat the split name.

Once a single specific split is identified, the program builds an
**actual program**: for every training day in that split's schedule, it
picks the #1-ranked exercise for each muscle trained that day (straight
from the same database used for individual muscle lookups) along with
its training protocol — a complete, ready-to-follow week, not just a
description of the split.

## Coverage

The database now includes every major trainable muscle, including
specific entries for: upper chest, lower chest, abs, obliques, serratus
anterior, spinal erectors (lower back), teres major & minor, rhomboids,
lats, traps, all three deltoid heads (front/side/rear), rotator cuff,
triceps overall plus all three triceps heads (long/short/medial),
biceps, brachialis, brachioradialis, forearms, glutes, hip flexors,
hamstrings, biceps femoris, tibialis, calves, quadriceps, adductors,
neck, and masseter — plus common nicknames, typos, and anatomical names
for all of them.

## Extending it

All exercise data lives in the `MUSCLE_DB` dictionary at the top of
`muscle_exercises.py`. To add a new muscle or exercise, add a new
`Muscle`/`Exercise` entry following the existing pattern, and (if adding a
new muscle) add any aliases to `MUSCLE_ALIASES`.
