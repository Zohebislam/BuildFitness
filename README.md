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

The program asks "What would you like to do -- name a muscle or ask for
a split?" and you type whatever you want — there's no menu to pick from.
Common names, gym nicknames, and anatomical/Latin names all work, and
your input is matched against 34 major trainable muscles and muscle
groups covering essentially the whole body.

If the program doesn't recognize what you typed — a typo (even of
"quit"), a stray number, anything it can't match to a muscle or a
split — it always gives the same response and asks again:

> Hmm, I didn't catch that -- try naming a muscle, or ask for a workout
> split.

The one place a number is expected is the "how many days a week do you
want to workout?" question: 1-7 builds your program, and anything
outside that range gets "Please pick a number of days between 1 and 7."

See [Ending your session](#ending-your-session) for how to finish.

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

How much you're told to do depends on where you are in the program.

### Exercise mode (looking up a muscle)

Every exercise shows exactly how to train it — always **1-2 sets**,
never a generic "3 sets of X." Each one gets one of three protocols,
chosen by injury risk if pushed to true failure:

- **2 sets to true failure x 8-15 reps** — for machine/cable movements
  on a fixed, supported path where failure just means the weight stops
  moving.
- **1 set to true failure** — for free-weight isolation, unilateral, and
  bodyweight movements (10-15 reps for free weights, 15-25 for
  bodyweight).
- **1 set x 5-8 reps, 1 rep in reserve (1 RIR)** — for heavy free-weight
  compound lifts (squats, deadlifts, heavy presses) where grinding to
  true failure carries real injury risk.

A few exercises (Farmer's Carry, Nordic Hamstring Curl, etc.) get a
tailored protocol instead, since they don't fit a normal rep scheme.

### Split mode (inside a built program)

When the program builds a split for you, sets and reps are tailored to
*that* split and *your* day count instead — with two hard limits:
**sets never exceed 2 and reps never exceed 10.**

- **Sets follow how often each muscle is trained that week:** once or
  twice → 2 sets, three or more times → 1 set. More frequent training
  means less per session, so weekly volume stays balanced.
- **Heavy compound lifts** (squats, deadlifts, heavy presses) get 2 sets
  only if that muscle is trained just once a week; otherwise 1 set,
  since they cost the most recovery.
- **Long sessions** (8+ exercises in one day) use 1 set per exercise.
- **6-7 training days a week** uses 1 set per exercise, since there's
  less recovery time between sessions.
- **Reps:** 5-8 for heavy compounds (every set stopping 1 rep short of
  failure); 8-10 for everything else (every set to true failure). For
  bodyweight movements, add weight or slow the tempo if you can pass 10.

Example: Bro Split at 5 days trains each muscle once, so every exercise
gets 2 sets. Push/Pull/Legs at 4 days trains the push muscles twice, so
most get 2 sets but heavy presses drop to 1. Push/Pull/Legs at 6 days
uses 1 set per exercise.

## Workout split builder

Ask for a "split" (or "workout split," "training schedule," etc.) to see
the full list of split families — **Full Body, Upper/Lower,
Anterior/Posterior, Push/Pull/Legs, Bro Split, Arnold Split, and
Push/Pull/Legs x Arnold Split** — each with a description of how it
works.

You can also name a specific family directly (e.g. `upper`, `ppl`,
`full body`, `anterior`, `bro split`, `arnold`, `ppl x arnold`), typos
included (`fullbdoy`, `aterior`, `bro splt` all still work). These
split-related words always take priority over any similarly-spelled
muscle name — typing `upper` always means the Upper/Lower split, never
"upper chest."

**Every split works the same way now: there's no fixed frequency.**
Once you name a family, the program asks **"How many days a week do you
want to workout?"** — any number from 1 to 7 — and builds the week
around exactly that. It cycles through the family's day-type pattern
(e.g. Upper/Lower alternates Upper, Lower, Upper, Lower...) for however
many training days you asked for, then spreads the remaining days as
rest as evenly as possible across the week, starting from Monday.

Once you give a day count, the program builds an **actual program**:
for every training day, it picks the #1-ranked exercise for each muscle
trained that day (straight from the same database used for individual
muscle lookups) along with sets and reps tailored to that split and
day count — a complete,
ready-to-follow week, not just a description of the split.

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

## Ending your session

The program keeps running until you decide you're finished — there's no
automatic cutoff, since you may want to look up another muscle or try a
different split. To finish, type any of `quit`, `exit`, `q`, `done`,
`bye`, `goodbye`, `finished`, or `I'm done` (case-insensitive, at any
prompt — even while it's waiting on an answer). `Ctrl+C` or `Ctrl+D`
also end the session cleanly, with no error message.

You won't be reminded about this on every prompt. Instead, a short hint
appears only at natural stopping points:

- After an exercise report: *"Done with this muscle? Type 'quit' to end
  your session, or keep exploring."*
- After a split program is built: *"Happy with your plan? Type 'quit'
  to end your session, or keep exploring."*

It does not appear at startup, on the split overview, while a
clarifying question is pending (such as "how many days a week do you
want to workout?"), or after an unrecognized input.

## Extending it

All exercise data lives in the `MUSCLE_DB` dictionary at the top of
`muscle_exercises.py`. To add a new muscle or exercise, add a new
`Muscle`/`Exercise` entry following the existing pattern, and (if adding a
new muscle) add any aliases to `MUSCLE_ALIASES`.
