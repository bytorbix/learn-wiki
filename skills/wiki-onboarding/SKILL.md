---
name: wiki-onboarding
description: First-time onboarding for the learner wiki. Asks where to create it and a few questions about the learner, then creates the wiki. Run once; never overwrites an existing wiki.
disable-model-invocation: true
---

# Wiki onboarding

Create the learner wiki and its profile. This is onboarding about the **learner
only**; project onboarding belongs to the learning skill. Follow
`${CLAUDE_PLUGIN_ROOT}/reference/wiki-format.md`; read §1–§3 and §10 first.

Ask **one question at a time** and wait for each answer. Use AskUserQuestion for
choices (one question, 2–4 short options, `multiSelect: false`); ask free-text
questions in chat. If the learner wants to skip, use defaults and mark unknown
answers `Not specified`.

## 1. Check for an existing wiki

Read `${CLAUDE_PLUGIN_DATA}/wiki-path`. It holds one line: the wiki root.

- If it exists and `<path>/profile.md` exists: a wiki is already set up. Tell
  the learner where it is and stop. Never overwrite it.
- If it exists but the wiki is missing: tell the learner, and ask whether to
  create a new wiki there or choose another location. Continue with step 2.
- If it doesn't exist: continue with step 2.

## 2. Choose the location

Ask where to create the wiki. Options: **`~/.learn-wiki` (Recommended)** and
**Somewhere else**. For "somewhere else", ask for the path in chat. Expand `~`
to the absolute home path.

Check the chosen folder:

- **Doesn't exist, or is empty:** use it.
- **Already contains `profile.md`:** it's an existing wiki (e.g. copied from
  another device). Tell the learner, save the path (step 5), and stop without
  onboarding.
- **Contains other files:** don't use it. Ask for another location.

## 3. Ask about the learner

1. **Overall experience** (picker): Beginner / Intermediate / Advanced.
2. **Top interests** (chat): "Which areas do you want to learn or dig into?"
   Turn the answer into domain tags (lowercase kebab-case, e.g.
   `binary-exploitation`) and show them in order. Let the learner fix names or
   order before continuing.
3. **Starting experience** (chat): for each interest, one short line about
   their experience so far. Record each as `self-reported`. Ask about all
   interests in one message; don't make it one question per domain.
4. **Preferences** (picker):
   - **Use defaults**: open-ended questions, short context, Normal checkpoints,
     AI writes the code from the learner's design.
   - **Customize**: then ask, one picker each:
     - Checkpoints: Light / Normal / Frequent
     - Question style: Open-ended / Multiple choice / Mixed
     - Code: AI writes / A mix / I write it, guided

## 4. Create the wiki

Create the layout from spec §2:

- `profile.md` (spec §3.1): overall experience, code, checkpoints, the brief
  study preferences, and top interests as `[[tag]]` links in the learner's order.
- `study-preferences.md` (spec §3.2): the four headings, filled from step 3.4.
  Write `Not specified` under headings with no answer. It holds only **how to
  teach** (explanations, questions, diagrams, pace). Never repeat checkpoints
  or who writes code here; those live only in `profile.md`.
- `domains/<tag>.md` for each interest (spec §3.3): `depth: normal`, no
  `checkpoints` override, `starting_experience: <answer> (self-reported)`,
  `How to teach me here` set to `None.`, and an empty generated `## Summary`.
- `concepts/`: empty.
- `INDEX.md`: a title only. It's filled by wiki-process.

Don't create concept entries or set any levels. Levels come only from evidence.

## 5. Save the location

Write the absolute wiki path as the only line of `${CLAUDE_PLUGIN_DATA}/wiki-path`.

## 6. Finish

Summarize in two or three lines: where the wiki is, the experience and interests
recorded, and that defaults can be changed any time by asking (e.g. "go deeper
on binary exploitation").
