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
