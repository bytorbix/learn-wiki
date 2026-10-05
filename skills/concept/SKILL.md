---
name: concept
description: Resolve a concept named in learner evidence to a wiki concept entry, creating, naming, and placing it if it doesn't exist yet. Used by wiki-process for each concept heading in an evidence file.
user-invocable: false
---

# Concept

Turn a concept name from an evidence file into exactly one concept entry in the
learner wiki. Follow `${CLAUDE_PLUGIN_ROOT}/reference/wiki-format.md`; read §2,
§3.3, §3.4, and §7 before acting. If this skill and the spec disagree, the spec wins.

## Input

From the caller:

- The wiki root directory.
- One concept heading from an evidence file: either an existing slug
  (`## timestamp-alignment`) or a proposed one (`## new: clock-offset-correction`).
- The evidence lines under that heading, for context only.

## Output

Report back to the caller:

- The resolved slug and the entry's path.
- Whether the entry was **matched**, **matched + alias added**, or **created**.
- Any `possible_duplicate_of` slugs, and any domain notes created.

This skill does **not** add evidence, change levels, prune, or touch `INDEX.md`.
Those are wiki-process's job.

## Steps

### 1. Load the index

Read `<wiki>/INDEX.md`. If it doesn't exist, treat the wiki as having no concepts.

### 2. Try to match

Normalize the incoming name: lowercase, trim, spaces and underscores to hyphens.
Drop a leading `new:`.

1. **Slug match:** an entry whose slug equals the normalized name.
2. **Alias match:** an entry with an alias equal to the incoming name
   (case-insensitive).

On a match, stop here unless the evidence used wording the entry doesn't have:
then go to step 3. Otherwise report **matched**.

### 3. Add an alias (matched entries only)

If the evidence names the concept with wording that is neither the slug nor an
existing alias, and that wording is a **true synonym**, add it to the entry's
`aliases:` and set `updated:` to today. Report **matched + alias added**.

**Alias test:** could the learner use this phrase to mean *exactly this
concept*, nothing more or less? If not, it isn't an alias. This rules out:

- Sub-details: "jittered timestamps" is a detail of `timestamp-alignment`.
- Comparisons: "hls vs rtsp" is about two things, not another name for
  `hls-streaming`.
- The slug itself with spaces: "hls streaming" adds nothing to `hls-streaming`.

### 4. Check the size before creating

The concept size rule (spec §7) is still **proposed**. Apply it as a guide:

- **Too narrow** (it's really one fact or one decision about a bigger idea):
  look for an existing entry it belongs to. If one fits, treat this as a match
  with that entry and report **matched**. Don't create a narrow entry.
- **Too broad** (a whole field, like "networking"): read the evidence lines and
  pick the specific concept they actually show. Use that as the name.
- **Right size:** continue.

### 5. Create the entry

Choose, in this order:

1. **Slug:** short lowercase kebab-case noun phrase for the idea, not the
   project's use of it: `timestamp-alignment`, not `sample-app-frame-sync`.
2. **Topic folder:** reuse an existing `concepts/<topic>/` folder if one fits.
   Create a new topic folder only if none does.
3. **Tags:** one or more broad domains. Reuse existing domain tags (from
   `INDEX.md` and `domains/`) before inventing new ones.
4. **Aliases:** the original wording from the evidence if it differs from the
   slug, plus any obvious synonyms. Every alias must pass the alias test in
   step 3. Keep it short.
5. **Related:** up to 3 links to existing entries that are closely related.
6. **Similar entries:** if an existing entry looks like it could be the same
   idea but didn't match in step 2, add `possible_duplicate_of: [<slug>]`.
   Never merge them.

Write `<wiki>/concepts/<topic>/<slug>.md` using the template in spec §3.4, with:

- `understanding: none` and `practical: none`
- `created:` and `updated:` set to today
- every section present with the line `None.`
- `## Counts` set to `strong: 0 · exposure: 0`

Report **created**.

### 6. New domains

For each tag that has no `<wiki>/domains/<tag>.md`, create one using spec §3.3
with defaults: `depth: normal`, no `checkpoints` override,
`starting_experience: Not specified`, an empty `How to teach me here` (`None.`),
and an empty generated `## Summary`. Report the domain notes you created.

## Never

- Merge two existing entries, rename an existing slug, move an entry between
  folders, or delete anything. Those need a later lint pass or the learner.
- Change levels, evidence, or learner settings.
- Create more than one entry for a single heading.
