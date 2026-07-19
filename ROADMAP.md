# Remodelers Guide - Improvement Roadmap

Last updated: 2026-07-19

This is a running list of areas to improve the site. It is a planning document, not a commitment. Items are grouped by theme and roughly ordered by value-to-effort within each group. Nothing here changes the workflow rules in `CLAUDE.md` (small PRs, new branch per round, red-flag added content, no em-dashes, no unsolicited cleanup). Pick items off this list as you want them; each becomes its own scoped PR.

## How to use this document
- Treat each unchecked box as a candidate, not a queued task. Wait for the user to call a shot.
- When an item ships, check the box and note the PR number, or move it to `CLAUDE.md` history and delete it here.
- Add new gaps as they surface during page work so they are not lost between sessions.
- Each full-site review pass gets a dated entry in the **Review Log** below, separate from the thematic backlog. The backlog itself gets refreshed (items closed out, new ones added) as part of that pass.

---

## Review Log
Dated record of full-site review passes, kept so we don't re-discover the same gaps or lose track of what's already been checked.

### 2026-07-19 - Full-site review (automated)
Read through the live site starting from `about.html`, checked internal links, red-flag spans, footer/nav consistency, and the existing backlog below against the current file state.
- **No broken internal links** found across all 77 HTML files (checked every `href="*.html"` against the file list).
- **Nav is consistent**: every content page carries `remediation.html`, `air-sealing.html`, and `safety.html` links; no drift.
- **Redirect stubs confirmed clean**: `roofing-order-sheet-2.html`, `siding-and-trim-order-sheat.html`, `tile-order-sheet.html`, and `safety-reminders.html` all redirect correctly and have zero remaining inbound links from other pages (bookmarks-only at this point).
- **`CLAUDE.md` work log has a gap**: PRs #224-227 (folding the Safety Reminders card into the PPE checklist, then dropping `safety-reminders.html` entirely) landed on `main` but were never added to the Work Log. The Redundancy Map note in `CLAUDE.md` still describes the reminders card as a separate artifact. Next session touching safety pages should backfill this entry.
- **New red-flag content found that predates this log**: `remediation.html` (added earlier, never logged) carries six unapproved `color:red` spans covering PPE minimums, containment/negative-air setup, and a moisture-content threshold for treating mold, i.e. safety-relevant procedure, not just a spec citation. See item below.
- Rechecked items 1-6 below against current file state and updated statuses.

---

## 1. Content gaps (missing pages, forms, and checklists)
These are referenced in field work or implied by existing pages but do not yet exist.

- [ ] **Flatwork / concrete order form** - `flatwork-concrete.html` exists as a page but has no companion order form. The `example-concrete-price-sheet.html` is a frozen "fossil" (do not reformat per user decision), so a real fillable order form is still missing.
- [ ] **Fireblocking & draftstopping checklist** - `fireblocking-and-draftstopping.html` is a strong page but has no printable jobsite checklist companion like the other trades have.
- [ ] **Rough-trade cross-linking** - Plumbing, Electrical, Mechanical, and HVAC rough pages do not link each other in their Resources sections. A "Related rough trades" block on each would help crews jump between coordinated trades. (Sidebar nav links exist; in-content cross-links do not.)
- [ ] **Neighbor letter fill-in blanks** - `neighbor-letter.html` reads as static prose. Add fill-in blanks (project address, start/end dates, contact name and phone, work hours) so it is actually usable as a template, matching the fillable-form direction of the rest of the site.
- [ ] **Tile order consolidation follow-through** - `tile-order-sheet.html` is now a redirect stub superseded by the prep/finish split. Confirm no remaining inbound links point at the old sheet expecting content.

## 2. Order forms - finish the Westbury-model pass
Standing direction: present real, orderable options and constrain clients to common choices instead of open write-in lines; keep each form to a single print page where possible.

- [ ] **Audit remaining open write-ins** - Sweep every order form for fields that come in fixed sizes/SKUs but are still open lines, and convert to pick lists (the siding reveal and countertop material/edge work in PR #206 is the model).
- [ ] **Deck order sheet - resolve open hardware flag** - The HDU5-SDS2.5 hold-down was dropped in PR #206; re-verify the deck sheet reflects DTT2Z carrying lateral tie and that no stale SKUs remain.
- [ ] **Quantity-note verification** - Deck fastener pack/count notes (clips/bag, pcs/lb) were generalized to "supplier scales" reminders; revisit if the user wants hard counts for any specific product.
- [ ] **Consistent form chrome** - Confirm every order form uses the standard `h1.form-title` "Remodelers Guide:" prefix, job-header fill lines, print-credit line, `.form-actions` wrapper, and `@page` rule. A few legacy sheets may predate the standard.

## 3. Citation and spec verification (red-flag cleanup)
Per workflow, researched specs stay red until the user approves them. These are the open flags, current as of the 2026-07-19 sweep.

- [ ] **Remediation page PPE/containment red flags (highest priority in this group)** - `remediation.html` has six unflagged-nowhere spans covering: small-area (under ~10 sq ft) EPA DIY threshold, PPE minimums for small vs. large cleanup (N95 vs. half-face P100/Tyvek), the "no dust mask" line, HVAC register sealing and negative-air containment setup, bagging demo material, and a ~19% wood moisture-content threshold (USDA Forest Products Lab) before treating. This is procedural safety guidance a crew could act on directly, not a background spec, so it's worth prioritizing over the others in this section.
- [ ] **Insulation soffit NFA rows** - Aluminum and vinyl soffit net-free-area numbers are still flagged; aluminum is likely understated (manufacturer full-vent panels run roughly 14-19.6 sq in/ft). Verify against current manufacturer data and unflag or correct.
- [ ] **Excavation utility-coordination notes** - Two flags on `excavation.html`: electrical service drop relocation lead time ("sometimes weeks out") and the refrigerant-recovery-requires-licensed-tech line for AC condensers in the dig zone. Both read as reasonable but are unconfirmed against the user's actual field experience.
- [ ] **Products page respirator note** - One flag on `products.html` on the Uqezagpa full-face respirator listing, asking to confirm the cartridge rating (organic vapor vs. P100) matches intended task use.
- [ ] **Standing red-flag sweep** - Grep the site for `color:red` spans and triage each: approve, correct, or surface to the user. Keeps the "added content" debt visible. (As of this pass: `products.html`, `remediation.html`, `insulation.html`, `excavation.html` are the only files with open flags.)

## 4. Structural / maintainability (the big one)
The site has no templating: every one of the 70+ HTML files carries its own full copy of the sidebar nav, the inline mobile-nav script, and the print-style block. Any nav change touches every file.

- [ ] **Evaluate a lightweight build or include step** - Options worth weighing: (a) a small static-site generator or partial-include build, (b) a tiny client-side JS include for the shared nav, (c) keep hand-edited HTML but standardize a generator script. Each has tradeoffs against the "plain HTML, no build system" simplicity the user values. This is a discussion item, not a do-it item - get a decision before touching it.
- [ ] **Shared CSS/JS extraction** - The print-style block and mobile-nav script are duplicated inline in every file. If a build step is rejected, at minimum consider moving the script to a shared file and the print rules into `styles.css`.
- [ ] **Nav-update script in repo** - Adding a nav link currently relies on an ad-hoc Python/sed script written fresh each time. Commit a reusable, documented `update-nav` script so the next addition is one command and indentation stays consistent.

## 5. Housekeeping and small polish
Low-risk, high-tidiness items.

- [ ] **Redirect-stub inventory** - `roofing-order-sheet-2.html`, `siding-and-trim-order-sheat.html` (note the misspelling), `tile-order-sheet.html`, and `safety-reminders.html` are all redirect stubs with zero remaining inbound links (verified 2026-07-19). Decide whether to keep them indefinitely for old bookmarks/print copies or sunset them on a date.
- [x] **resources.html completeness** - Re-verified 2026-07-19: every current order sheet, checklist, and safety printable is indexed, including the recent PPE checklist consolidation.
- [ ] **`air-sealing.html` missing standard footer chrome** - Unlike every other content page, it has no Share/Print buttons and no `Last revised` line. Found 2026-07-19; page was added in an earlier session without full parity to the standard footer.
- [ ] **`hazardous-materials.html` footer typo** - Reads "Last revised 05/01/2026" with no colon; every other page uses "Last revised: MM/DD/YYYY". One-line fix.
- [ ] **`about.html` footer inconsistency (discuss, don't just fix)** - The home page uses an older, simpler footer with no Share/Print buttons, no feedback-form link, and no `Last revised` line, while all other pages (including utility pages like `wage-calculator.html`) share those elements. Could be intentional for the homepage; confirm with the user before changing.
- [ ] **`CLAUDE.md` work log gap** - PRs #224-227 (fold Safety Reminders card into PPE checklist, then drop `safety-reminders.html`) are not recorded in the Work Log; the Redundancy Map note there is now stale. Backfill next time safety pages are touched.
- [ ] **README is stale** - `README.md` still describes the AWS/S3 upload process and an "Active Navigation" version. The site is on GitHub Pages now. Rewrite it to reflect the current hosting, structure, and the conventions in `CLAUDE.md`.
- [ ] **Last revised dates** - Spot-check that pages edited recently have current `Last revised:` footers.

## 6. Orphan pages (parked - do not delete)
Per user decision, these are NOT to be deleted; the user wants them migrated to his personal website repo, which is not yet accessible from these sessions.

- `construction-dashboard.html`
- `building-diagnostics.html`
- `diagnostic-tools.html`
- `cte-presentation.html`
- `newsletter_prompt.html`

- [ ] **Migration when repo access lands** - When the personal website repo becomes reachable, move these out of `rem-guide` and leave redirect stubs only if any are publicly linked.

## 7. Possible future enhancements (not yet requested)
Ideas to float with the user, not assumptions to act on.

- [ ] **Site search** - 70+ pages with no search. A small client-side index (e.g., a static JSON + tiny JS) would help crews find a spec fast on a phone.
- [ ] **Print-all / packet export** - A way to print a curated set of forms (the "van packet") in one shot rather than page by page.
- [ ] **Mobile field-use review** - The audience increasingly uses this on phones on-site. A dedicated pass on tap targets, table overflow, and form-on-phone legibility could pay off.
- [ ] **Per-page "last verified" provenance** - For code-citation-heavy pages, a small note on when citations were last checked against the adopted code edition would build trust and flag staleness.

---

*Keep this list honest: if an item stops being worth doing, delete it. A roadmap full of things nobody will do is just noise.*
