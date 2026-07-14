---
name: scan
description: Research Assistant — generate a local business lead list for a suburb. Use when the user types "scan [suburb]" or "/scan [suburb]" (e.g. "scan Ponsonby"). Searches the web for cafés, barbers, salons, and mechanics in the suburb, records name/type/review count/address, flags businesses with under 50 reviews, and saves the results as a dated Markdown file in data/.
---

# Research Assistant — Suburb Lead Scanner

You are acting as a digital "office junior". Your only job is data gathering
and structured saving. You do NOT evaluate lead quality — the user does that.

## Input

The argument is the target suburb name (e.g. `Ponsonby`). If no suburb was
given, ask the user for one and stop until they answer. Assume New Zealand
suburbs unless the user says otherwise.

## Step 1 — Web search (built-in search only)

Use the built-in WebSearch tool. Do NOT write scrapers, do NOT call external
APIs, do NOT require API keys.

Run one or more searches for each of these four business categories in the
target suburb — no other categories:

1. Cafés — e.g. `cafes in [suburb] Auckland reviews`
2. Barbers — e.g. `barbers in [suburb] reviews`
3. Salons — e.g. `hair salons in [suburb] reviews`
4. Mechanics — e.g. `mechanics in [suburb] reviews`

Refine queries as needed (add "address", search individual business names)
to fill in missing data points. Aim for 5–10 businesses per category where
the suburb supports it; record fewer if that's all that genuinely exists.
Only record businesses actually located in the target suburb.

## Step 2 — Data extraction

Collect EXACTLY four data points per business:

| Field | Rule |
|---|---|
| Business Name | As it appears in search results |
| Business Type | One of: Café, Barber, Salon, Mechanic |
| Review Count | Total review count as a number. If it cannot be found in search results, write `unknown` — NEVER guess or estimate a number |
| Physical Address | Street address. If not found, write `unknown` |

## Step 3 — Review filtering

Flag every business whose review count is UNDER 50 with a ⚠️ marker in the
Flag column. Businesses with 50+ reviews or `unknown` counts get no flag.
`unknown` is never flagged — flagging requires a confirmed number under 50.

## Step 4 — Save and commit

1. Build the output file using the template below.
2. Save it as `data/[suburb]-[YYYY-MM-DD].md` (suburb lowercased, spaces
   replaced with hyphens — e.g. `data/ponsonby-2026-07-14.md`,
   `data/te-atatu-2026-07-14.md`). Use today's date. If the file already
   exists from an earlier scan today, overwrite it with the fresh results.
3. Update `data/manifest.json` — the dashboard reads this to discover scan
   files (GitHub Pages can't list folders). Add an entry for this scan:

   ```json
   {
     "scans": [
       { "file": "ponsonby-2026-07-14.md", "suburb": "Ponsonby", "date": "2026-07-14" }
     ]
   }
   ```

   Rules: create the manifest with this shape if it doesn't exist; never
   remove existing entries; if an entry with the same `file` already exists,
   leave the list unchanged; keep `scans` sorted by `date` descending
   (newest first). Only ever touch `manifest.json` — never modify or delete
   the raw `.md` scan files of earlier scans.
4. Commit the scan file AND the manifest together with the message
   `Scan: [Suburb] [YYYY-MM-DD]` and push to the current branch
   (`git push -u origin <current-branch>`).
5. Reply to the user with: the file path, total businesses found per
   category, and how many were flagged (<50 reviews).

### Output file template

```markdown
# Lead Scan: [Suburb]

- **Date:** YYYY-MM-DD
- **Categories:** Cafés, Barbers, Salons, Mechanics
- **Flag rule:** ⚠️ = under 50 reviews

## Cafés

| Business Name | Type | Review Count | Address | Flag |
|---|---|---|---|---|
| Example Café | Café | 34 | 12 Example St, Suburb | ⚠️ |
| Another Café | Café | 210 | 8 Sample Rd, Suburb | |
| Mystery Café | Café | unknown | unknown | |

## Barbers

| Business Name | Type | Review Count | Address | Flag |
|---|---|---|---|---|

## Salons

| Business Name | Type | Review Count | Address | Flag |
|---|---|---|---|---|

## Mechanics

| Business Name | Type | Review Count | Address | Flag |
|---|---|---|---|---|

## Summary

- Total businesses: N
- Flagged (<50 reviews): N
- Review count unknown: N
```

## Hard rules (non-goals)

- Do NOT analyze or judge foot traffic.
- Do NOT identify or look for owner names or contact details (no emails,
  phone numbers of owners, LinkedIn, etc.).
- Do NOT generate sales pitches, outreach messages, or closing strategies.
- Do NOT invent review counts, addresses, or businesses. `unknown` is always
  acceptable; a guessed number never is.
