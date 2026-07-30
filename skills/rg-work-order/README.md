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

2. **WHMCS IDs — SOLVED.** The source is the CRM (The Roost) Supabase table `client_profiles`:
   the numeric `whmcs_id` keyed by `website` / `websites[]`. Coverage as of 2026-07-30:
   **1,209 clients, 970 with a WHMCS ID (1,505 domain aliases mapped).** Validated end to end:
   `chathamorthonc.com → 1765`, and the wrong-practice domain from Chatham's CRITICAL-1,
   `baybirdorthodontics.com → 1456`. The exact export query is in `SKILL.md` →
   "WHMCS ID resolution". The materialized map is client data and is **not** committed here
   (this repo is public); regenerate it from the CRM at run time.

Gap 1 is the only remaining blocker, and it clears by running where the audits live.

## To run the batch (in Skill Studio, where the audits are)

1. Install this skill into the catalog (`skill-studio-team-skills`) → Skill Studio picks it up.
2. Regenerate the WHMCS map from the CRM with the query in `SKILL.md` (or let the skill query
   the Roost Supabase directly if that MCP is configured locally).
3. Run against the folder of **technical audit** deliverables (rg-master-audit / rg-site-audit
   output) — not the client-facing reports:

```
rg-work-order  source=<folder of technical audit outputs>
               whmcs_map=<domain-to-WHMCS-ID json from the query above>
```

Output: `work-orders/<client-slug>/work-order-<YYYY-MM>.html` (+ `.md`) per client, plus
`work-orders/index.html` sorted by Critical count, with every "Needs WHMCS ID" client listed
at the top.

## Destination

This skill belongs in the team skill catalog (`ctkubik/skill-studio-team-skills`) so Skill
Studio installs it like the other `rg-*` skills. It is staged here on
`claude/version-2158-main-xneqs9` for review first.
