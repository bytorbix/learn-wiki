---
name: learn
description: Turn on learning-first development in this project. The learner designs; Claude explains, checks understanding, and writes the agreed code. Uses the learner wiki to pitch teaching at the learner's real level.
disable-model-invocation: true
---

# Learn

Turn on learning mode for this conversation and keep it on through normal
development, not just this command. Read [behavior.md](behavior.md) now and
follow it for the rest of the session.

Specs:

- Learner wiki: `${CLAUDE_PLUGIN_ROOT}/reference/wiki-format.md`
- This project's state: `${CLAUDE_PLUGIN_ROOT}/reference/repo-format.md`

Use Read and Glob for these files, not shell `cat` or `ls`. A missing file or
folder is normal on first use, not an error.

## 1. The wiki

Read `${CLAUDE_PLUGIN_DATA}/wiki-path`.

- **Missing, or the wiki has no `profile.md`:** tell the learner the wiki needs
  setting up first. Read `${CLAUDE_PLUGIN_ROOT}/skills/wiki-onboarding/SKILL.md`
  and follow it, then continue here.
- **Found:** read `profile.md` and `INDEX.md`. Nothing else yet (wiki-format §8).

## 2. This project's state

Find `.learning/` using repo-format §2.

**It exists:**

1. Read `settings.md`. If `Learning mode: paused`, set it to `active`: running
   this command is how learning resumes.
2. Read `pending.md` and `project-map.md` in full.
3. If a checkpoint is pending, bring it back before anything else. Restarting
   is never approval.

**It doesn't exist:** run project onboarding (step 3).

## 3. Project onboarding

About the project only; everything about the learner comes from the wiki. Ask
one picker at a time and wait for each answer.

1. **What are we doing?** New project / Existing repo / Continuing
   (an existing repo the learner already knows).
2. **How much do you want to study here?**
   - Deep: reason through every meaningful decision
   - Balanced: major decisions
   - Light: mostly build; learn only what's new
3. **How involved will you be?**
   - Same as usual (show the profile's `Code:` value)
   - More hands-on
   - Less hands-on

Then:

- **New project:** create an empty map. Purpose and requirements come from the
  learner as you go.
- **Existing repo / Continuing:** inspect the project's guidance, entry points,
  dependencies, storage, integrations, and deployment config. Avoid secrets and
  generated files. Sketch a short map with only what the code shows; mark
  everything else `?`. Show the main flow and let the learner correct it.

Create `.learning/` with `settings.md`, `pending.md` (`None.`), and
`project-map.md`, following repo-format §2.1, §3, and §5. Recommend adding
`.learning/` to `.gitignore`; edit it only if the learner says so.

## 4. Earlier evidence

If `.learning/evidence/` has files without a `Processed:` line from **earlier**
sessions (not this session's file), merge them into the wiki with the
`wiki-process` skill. Run it in a background subagent if one is available so
the learner isn't kept waiting. Mention it in one line; don't show the report
unless something needs the learner (e.g. skipped lines).

## 5. Start

Continue the learner's task. If none was given, ask what they want to build or
change. Running this command again never resets anything.
