# Article Reader — Local Configuration

This file documents the configuration values the skill needs. **Copy it to
`config.local.md`** in the same directory and fill in your own paths.

`config.local.md` is gitignored — your personal paths stay on your machine
and are never committed.

## VAULT_PATH

The absolute path the skill is scoped to. Reads and writes happen **only**
within this directory.

You can point this at:

- Your entire Obsidian vault root (broadest scope)
- A specific sub-folder you've set aside for this skill (recommended for
  first use — best isolation, easiest to delete if something goes wrong)

```
~/Documents/Obsidian/article-reader-helper
```

## (Optional) Anchor files

If you want forcing questions calibrated to you specifically, create either
or both of these files inside `VAULT_PATH`:

- `about.md` — a paragraph or two on who you are, what you care about, how
  you think. The skill reads this to calibrate question angles.
- `beliefs.md` — your core "I've thought about this and reached a position"
  views. The skill uses these to detect when an article challenges them.

Both files are optional. If absent, the skill skips anchoring silently and
asks more generic forcing questions.

## First run

The skill will create `{VAULT_PATH}/inbox/articles/` on first run if it
doesn't exist. Notes are written there as a quarantine — you review and
manually promote them to permanent vault locations afterwards.

That's the whole configuration. No other paths are needed.
