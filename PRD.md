# Product Requirements Document: BuildFitness

**Document type:** Retroactive PRD (written to document a working v1 built
iteratively; requirements below reflect the shipped product as of this
writing)
**Product name:** BuildFitness
**Format:** Command-line Python application (`muscle_exercises.py`)
**Status:** Implemented and functional

---

## 1. Purpose / Problem Statement

Most freely available exercise recommendations (fitness influencer content,
generic "top 10 exercises for X" articles) are not ranked with any
consistent methodology, do not explain *why* an exercise works a muscle,
and rarely specify *how* to actually train it (sets, reps, intensity).

BuildFitness gives a user a single, consistent, science-grounded answer to
two questions:

1. **"What are the best exercises for muscle X, and why?"**
2. **"How do I structure my week to train everything effectively?"**

It answers both from a fixed methodology (documented ranking criteria and
protocol rules) rather than opinion, and builds an actual usable program
rather than just describing options.

---

## 2. Goals

- Give a ranked (best → worst), explained list of exercises for any major
  trainable skeletal muscle.
- Make every ranking traceable to a specific, named biomechanical
  principle — never an unexplained opinion.
- Remove ambiguity in set/rep prescription — no generic "3 sets of 10."
- Let a user go from "I want to train X" to a concrete weekly training
  program with real exercises, with no manual assembly required.
- Accept natural, imprecise human input (nicknames, anatomical terms,
  typos, singular/plural, broad body-region words) without forcing the
  user through a rigid menu.

## 3. Non-Goals (Out of Scope for this version)

- No graphical UI or web front-end (explicitly deferred by product
  decision; CLI only for now).
- No persistence — the app does not save workout history, progress, or
  user profiles between runs.
- No equipment-availability filtering (e.g. "home gym only" mode).
- No nutrition, recovery, or non-resistance-training content.
- No user accounts, login, or multi-user support.
- No automated tests / CI (currently verified via manual and ad hoc
  scripted regression checks only).

---

## 4. Target User

An individual with at least basic gym familiarity who wants:
- A fast, opinionated but *explained* answer for "what should I do for
  [muscle]," and/or
- A ready-made weekly training split without having to research and
  assemble one manually.

Not intended for absolute beginners needing form instruction, or for
clinical/rehab exercise prescription.

---

## 5. Functional Requirements

### 5.1 Core Muscle Lookup

| ID | Requirement |
|----|-------------|
| FR-1 | The user can type any muscle name in free text; there is no fixed menu of choices presented for muscle selection. |
| FR-2 | Input is matched against common names, gym nicknames, and anatomical/Latin names for every muscle in the database. |
| FR-3 | Input matching tolerates typos via fuzzy string matching. |
| FR-4 | Input matching tolerates singular/plural variation automatically (e.g. "calf" ↔ "calves," "bicep" ↔ "biceps") without requiring every form to be manually aliased. |
| FR-5 | If input matches no known muscle, the program responds with "Please enter a muscle." and continues the input loop without error or exit. |

### 5.2 Muscle Coverage

The database (`MUSCLE_DB`) must include, at minimum, the following 34
muscles/muscle groups, each as an independently addressable entry:

Chest, Upper Chest, Lower Chest, Back (Lats), Traps, Serratus Anterior,
Teres Major & Minor, Rhomboids, Rotator Cuff, Shoulders (overall),
Anterior Deltoid, Lateral Deltoid, Rear Deltoid, Biceps, Brachialis,
Brachioradialis, Triceps (overall), Triceps Long Head, Triceps Short
(Lateral) Head, Triceps Medial Head, Forearms, Abs, Obliques, Lower
Back, Quadriceps, Hamstrings, Biceps Femoris, Glutes, Hip Flexors,
Adductors, Calves, Tibialis, Neck, Masseter.

| ID | Requirement |
|----|-------------|
| FR-6 | Each muscle entry includes a plain-language anatomy/function description explaining its joint action(s). |
| FR-7 | Each muscle entry includes 3–5 exercises, ordered best → worst. |

### 5.3 Exercise Ranking Methodology

| ID | Requirement |
|----|-------------|
| FR-8 | Every exercise entry includes a **mechanism** explanation: the biomechanical reason it trains the target muscle. |
| FR-9 | Every exercise entry includes a **why-ranked** explanation: the reasoning for its position relative to other exercises for that muscle. |
| FR-10 | Rankings are produced using seven named, consistently applied criteria: (1) stretch-mediated tension, (2) resistance profile across the range of motion, (3) direct vs. shared tension with synergists, (4) range of motion available, (5) stability demands, (6) motor unit recruitment potential, (7) progressive overload potential. |
| FR-11 | These seven criteria are displayed to the user alongside every exercise list so the methodology is transparent, not hidden. |

### 5.4 Training Protocol ("Best Method")

| ID | Requirement |
|----|-------------|
| FR-12 | Every exercise displays a specific training protocol — never a generic "3 sets of X." |
| FR-13 | Protocol is one of three types, chosen by the injury risk of training that exercise to true failure: (a) 2 sets to true failure (safe, supported/machine path), (b) 1 set to true failure (free-weight isolation/unilateral/bodyweight), (c) 1 set × 5–8 reps at 1 rep in reserve (heavy free-weight compound lifts). |
| FR-14 | A small set of exercises that don't fit a standard rep scheme (e.g. Farmer's Carry, Nordic Hamstring Curl) receive a manually tailored protocol string instead of the automatic classification. |

### 5.5 Ambiguous Term Clarification

| ID | Requirement |
|----|-------------|
| FR-15 | If the user enters a broad, multi-muscle body-region word — `legs`, `arms`, `torso`, `chest`, `back`, `shoulders`, or `triceps` (including singular forms and the synonym `trunk`) — the program does not guess; it asks which specific muscle within that region is meant, and shows the valid options for that region only. |
| FR-16 | Fully specific muscle names/aliases (e.g. `lats`, `delts`, `triceps brachii`) bypass this clarification and resolve directly, even though they overlap semantically with a broader category. |
| FR-17 | Clarification option sets, by category: <br>• **Chest** → overall chest, upper chest, lower chest <br>• **Back** → lats, traps, lower back, serratus anterior, teres major & minor <br>• **Shoulders** → anterior deltoid, lateral deltoid, rear delt, rotator cuff <br>• **Triceps** → long head, short (lateral) head, medial head <br>• **Legs** → quadriceps, hamstrings, glutes, calves, adductors, hip flexors, tibialis, biceps femoris <br>• **Arms** → biceps, triceps, brachialis, brachioradialis, forearms <br>• **Torso** → chest, upper chest, lower chest, back, traps, abs, obliques, lower back, serratus anterior, teres major & minor, rotator cuff |

### 5.6 Workout Split Builder

| ID | Requirement |
|----|-------------|
| FR-18 | The user can request a workout split via natural trigger phrases (`split`, `splits`, `workout split`, `training schedule`, etc.), including full natural-language sentences ("can you give me a split"). |
| FR-19 | The system provides seven split families, each defined by a repeating day-type pattern rather than a fixed frequency: Full Body, Upper/Lower, Anterior/Posterior, Push/Pull/Legs, Bro Split, Arnold Split, and Push/Pull/Legs x Arnold Split. |
| FR-20 | Each family displays a description of its training philosophy (how its pattern works and why). No family has a fixed or preset frequency — frequency is entirely determined by the user's day-count answer (see FR-23). |
| FR-21 | The user can name a split family directly (`upper`, `ppl`, `full body`, `anterior`, `bro split`, `arnold`, `ppl x arnold`) to identify it directly, with typo tolerance. |
| FR-22 | Split-related words always take priority over any identically- or similarly-spelled muscle name (e.g. bare `upper` always resolves to the Upper/Lower split, never to "Upper Chest"; "push pull legs x arnold" resolves to the hybrid family, not the plain Push/Pull/Legs family). |
| FR-23 | Once a single family is identified, the system always asks "How many days a week do you want to workout?" (accepting a bare number or a natural phrase like "4 days a week"), retaining conversational state so the next input is interpreted as the answer to that question. Valid range is 1-7 days; out-of-range or non-numeric answers are asked again without losing the chosen family. |
| FR-24 | Once a day count is given, the system **constructs an actual program**: the family's day-type pattern is cycled for exactly that many training days, distributed as evenly as possible across the calendar week starting from Monday, with every remaining day marked as rest. For each training day, the system selects the #1-ranked exercise for every muscle assigned to that day (pulled from the same exercise database used for individual lookups) and displays it with its training protocol. |

### 5.7 Conversational UX

| ID | Requirement |
|----|-------------|
| FR-25 | On startup, the program displays a welcome message identifying the product as "BuildFitness" and briefly describing what it can do (muscle lookup and split building), without listing every option as a menu. |
| FR-26 | Outside of the two explicitly-designed exceptions (region/category clarification, and the split overview), the program never presents the user with a list of options to choose from — all other input is free text. |
| FR-27 | The user can chain unlimited lookups/requests in a single session (muscle → muscle, muscle → split, split → muscle, etc.) without needing to restart or confirm continuation between them. |
| FR-28 | Typing `quit`, `exit`, or `q` ends the session at any time. |

---

## 6. Non-Functional Requirements

| ID | Requirement |
|----|-------------|
| NFR-1 | Runs with the Python 3 standard library only — no third-party dependencies required. |
| NFR-2 | Single-file implementation (`muscle_exercises.py`) for ease of distribution and review. |
| NFR-3 | All exercise/muscle content is stored as structured data (`MUSCLE_DB`, `SPLITS`, alias dictionaries) separate from program logic, so new muscles, exercises, or splits can be added without altering control flow. |
| NFR-4 | Every user-facing claim (mechanism, ranking rationale, protocol assignment, split rationale) must be traceable to one of the documented methodologies (7 ranking criteria; 3 protocol types; split-frequency research framing) — no unexplained assertions. |

---

## 7. Deferred / Candidate Features (Not Yet Built)

Identified during development but intentionally not implemented in this
version:

- Save/export a generated program or muscle report to a file.
- A `list` command to browse all muscles currently in the database.
- Equipment-availability filtering (e.g. "bodyweight/home only" mode).
- Per-exercise common-mistake / form-cue notes.
- Injury/contraindication flags on higher-risk exercises.
- Separating the content database out of the `.py` file into JSON/YAML
  for non-developer editing.
- Automated unit tests covering the alias/fuzzy-matching logic.
- A graphical or web front-end.

---

## 8. Open Questions

- Should the split builder eventually let a user customize which
  muscles are assigned to a given day (e.g. swap glutes from Lower to a
  dedicated day)?
- Should protocol assignment (2×failure / 1×failure / 1×6 @1RIR) become
  user-configurable, or remain a fixed system rule?
- Is there demand for exporting a built program to a shareable format
  (text file, calendar invite, etc.)?

---

## 9. Appendix: Full Split Family List

Frequency is no longer fixed per split — every family below is defined
by a repeating day-type **pattern**, cycled for however many training
days (1-7) the user requests, with rest evenly distributed across the
remaining days starting from Monday.

| Split Family | Pattern (repeats to fill the chosen day count) |
|---|---|
| Full Body | Full Body A, Full Body B (alternating) |
| Upper/Lower | Upper, Lower |
| Anterior/Posterior | Anterior, Posterior |
| Push/Pull/Legs | Push, Pull, Legs |
| Bro Split | Chest, Back, Shoulders, Legs, Arms |
| Arnold Split | Chest & Back, Shoulders & Arms, Legs |
| Push/Pull/Legs x Arnold Split | Push, Pull, Legs, Shoulders & Arms, Chest & Back, Legs |

Example: choosing Push/Pull/Legs at 5 days/week produces Push, Pull,
Legs, Push, Pull (pattern repeats mid-cycle) with the remaining 2 days
as rest, spread evenly rather than clustered at the end of the week.
