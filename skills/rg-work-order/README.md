# rg-work-order

Turns an **existing** RoosterGrin audit into an internal **Requests Team Work Order** — a
no-explanation action sheet the support/requests team executes top to bottom.

- **Page 1:** domain + WHMCS Client ID + severity summary.
- **Each task:** what is wrong · the exact fix · paste-ready content when a new header, title,
  meta, schema block, sitemap, or alt text is required.
- **Never** runs a new audit. **Never** invents a fix.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The generator spec (single + batch mode). |
| `work-order-template.html` | The styled, theme-aware, print-ready HTML shell with `{{TOKENS}}` and a clone-per-finding task block. |
| `examples/chathamorthonc.com-work-order.html` | Reference output built from the Chatham findings. `CRITICAL-1/2/3` are real; `HIGH`/`MEDIUM` and any address/phone/title values are illustrative sample data to show the format. |

Live rendered reference (same file, published):
https://claude.ai/code/artifact/38673e4e-6024-478f-b28b-5b7803e23033

## Status — read before running the batch

The generator is built and works on a single audit. **Batch generation over "every audit" is
blocked on two inputs that are not reachable from the releases/reports repos:**

1. **The technical audit deliverables are not in a reachable repo.** `ctkubik/roostergrin-reports`
   holds the **client-facing** Digital Evaluation Reports (rankings, traffic, AI visibility,
   Google Ads) for 51 clients — not the technical audits. A scan of all 131 report files found
   zero sitemap / title-tag / canonical / alt-text findings. The 300+ technical audits ran in
   Skill Studio and their outputs live in the app's local data, not in git. **A work order
   cannot be built from a marketing report** — the technical findings simply are not in it.

2. **No WHMCS ID source.** `clients.json` has an internal 12-char id and domains, but no WHMCS
   ID. Until a `domain → WHMCS ID` map exists, every cover renders the flagged placeholder.

Neither gap is worked around by guessing — that would ship invented fixes to live client sites.

## To run the batch (once the inputs are reachable)

Point the skill at the folder of **technical audit** deliverables, with the WHMCS map, in
Skill Studio (where the audits live) or in a session that can reach them:

```
rg-work-order  source=<folder of rg-master-audit / rg-site-audit outputs>
               whmcs_map=<domain-to-WHMCS-ID csv or json>
```

Output: `work-orders/<client-slug>/work-order-<YYYY-MM>.html` (+ `.md`) per client, plus
`work-orders/index.html` sorted by Critical count, with every "Needs WHMCS ID" client listed
at the top.

## Destination

This skill belongs in the team skill catalog (`ctkubik/skill-studio-team-skills`) so Skill
Studio installs it like the other `rg-*` skills. It is staged here on
`claude/version-2158-main-xneqs9` for review first.
