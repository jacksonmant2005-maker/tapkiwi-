# tapkiwi-

## Research Assistant (suburb lead scanner)

In any Claude Code session connected to this repo, run:

```
/scan Ponsonby
```

(or just type `scan Ponsonby`). Claude searches the web for cafés, barbers,
salons, and mechanics in the suburb, records business name, type, review
count, and address, flags anything with under 50 reviews with ⚠️, and saves
the result as `data/[suburb]-[date].md` — then commits and pushes it.

Unverifiable review counts are written as `unknown`, never guessed. The
`data/` folder is plain Markdown, ready to sync into an Obsidian vault.

The workflow definition lives in [.claude/skills/scan/SKILL.md](.claude/skills/scan/SKILL.md).
Every scan also registers itself in [data/manifest.json](data/manifest.json),
which the dashboard uses to discover scan files.

## Dashboard v2 (sales tracker)

A single-file dashboard at [dashboard/index.html](dashboard/index.html), served
by GitHub Pages at:

**https://jacksonmant2005-maker.github.io/tapkiwi-/dashboard/**

It renders every scanned business as a lead card (name, category, review
count, address), badges low-review leads (⚠️ under 50, ❓ unknown), and gives
each card a four-state pipeline toggle: Not pitched → Pitched → Sold →
Follow-up. Marking a lead Sold prompts for the sale tier ($49 / $99 / $179)
and feeds the Pitches Made / Sales Closed / Cash Earned counters at the top.
Pipeline state lives in `localStorage` (per-device); the raw `data/*.md`
files are never modified, so the folder stays Obsidian-ready.
