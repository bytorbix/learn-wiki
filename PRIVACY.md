# Privacy

learn-wiki is a Claude Code plugin made of instructions, two Markdown specs, and
one read-only Python hook. It has no server, no account, no analytics, and no
telemetry. The plugin itself never sends data anywhere.

## What it stores

Everything is plain Markdown on your own machine.

| Where | What |
| --- | --- |
| Your wiki folder (default `~/.learn-wiki`, you choose it) | Your overall experience, study preferences, interests, and a self-reported starting experience per interest. For each concept: short paraphrased evidence of what you explained, designed, or implemented, open and resolved gaps, counts, and levels |
| `<project>/.learning/` in each project where you run `/learn` | That project's learning settings, pending checkpoints, a map of the system as you understand it, and the session's evidence |
| `~/.claude/plugins/data/<plugin>/wiki-path` | One line: where your wiki is |

## What it never stores

- Transcripts or quotes from your conversations. Evidence is a one-sentence paraphrase.
- Credentials, keys, tokens, or other secrets.
- Pronouns or other personal details about you.

## How the data is used

Claude Code reads these files during your sessions so Claude can pitch
explanations at your level and remember pending decisions. Like anything else
in a Claude Code session, the content Claude reads is processed under your
existing Anthropic account and data settings. learn-wiki adds no other
processing and no other recipients.

The session hook only reads `.learning/settings.md` to check whether learning
is on, and tells Claude which files to open. It writes nothing.

## Your control

- **See it:** every file is readable Markdown.
- **Keep it out of git:** learn-wiki recommends adding `.learning/` to your
  project's `.gitignore` and never edits `.gitignore` unless you ask.
- **Pause:** say "pause learning" in a project. Nothing is recorded while paused.
- **Reset a project:** ask Claude to reset this project's learning. Settings,
  pending checkpoints, and the map are backed up and cleared.
- **Delete everything:** delete your wiki folder and any `.learning/` folders.
  Uninstalling the plugin removes the `wiki-path` pointer.

## Contact

Questions or concerns: [open an issue](https://github.com/bytorbix/learn-wiki/issues).
