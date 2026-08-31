# Remodelers Guide - Improvement Roadmap

Last updated: 2026-08-31

This is a running list of areas to improve the site. It is a planning document, not a commitment. Items are grouped by theme and roughly ordered by value-to-effort within each group. Nothing here changes the workflow rules in `CLAUDE.md` (small PRs, new branch per round, red-flag added content, no em-dashes, no unsolicited cleanup). Pick items off this list as you want them; each becomes its own scoped PR.

## How to use this document
- Treat each unchecked box as a candidate, not a queued task. Wait for the user to call a shot.
- When an item ships, check the box and note the PR number, or move it to `CLAUDE.md` history and delete it here.
- Add new gaps as they surface during page work so they are not lost between sessions.
- Each full-site review pass gets a dated entry in the **Review Log** below, separate from the thematic backlog. The backlog itself gets refreshed (items closed out, new ones added) as part of that pass.

---

## Review Log
Dated record of full-site review passes, kept so we don't re-discover the same gaps or lose track of what's already been checked.

### 2026-08-31 - Full-site review (automated)
Since the 2026-08-10 pass, `main` gained: the demolition downspout-drainage field note (PR #239), Interior Trim page + Interior Door Order Form (PRs #241/#243/#244), a Tile Prep content merge (PR #247), and the Foundation Order Sheet replacing the foundation checklist (PRs #250/#252/#254, `foundation-order-sheet.html`) with its red flags cleared. Two automated review passes dated 2026-08-17 and 2026-08-24 were also run in this window but never made it into this log - see the process finding below.

**Process finding (most important item this pass, not a content issue):** three review/memory PRs are sitting open and unmerged against `main`, some for two weeks or more: **#248** ("2026-08-17 review pass"), **#249** ("2026-08-24 review pass"), and **#253** (a `CLAUDE.md` memory update documenting the Foundation Order Sheet's 08-27 revision session, which is already live on `main` but undocumented). Because #248 and #249 both branched from the same pre-Foundation-Order-Sheet commit rather than from each other, they duplicate parts of the same findings without building on one another, and neither is mergeable cleanly anymore now that `main` has moved on. Their still-accurate findings (the footer-date/colon fixes below, and the note that `interior-trim.html`'s red flags have now been open since 08-14) are folded into this entry so nothing is lost. **Recommend the user close #248 and #249 without merging** (superseded by this entry) **and merge or close #253** on its own merits (it only touches `CLAUDE.md`, not site content, so it's low-risk either way). Going forward, each automated pass should branch fresh from `main` rather than stacking on a prior pass's branch, so this doesn't recur.

**Implemented this pass (mechanical, no user decision needed):**
1. **Stale `Last revised` footers, five files** - `demolition.html` and `demolition-checklist.html` still read their original build dates despite the 08/13/2026 downspout-drainage note (PR #239); updated both to `08/13/2026`. `foundations.html` still read `01/01/2026` despite the 08/07/2026 sump-crock/radon field note (PR #234); updated to `08/07/2026`. `flatwork-concrete.html` and `neighbor-letter.html` were missing the colon after "Last revised" (`neighbor-letter.html` also had an unpadded month); fixed formatting only, no evidence of a recent content edit so the dates themselves were left as-is.
2. **`foundation-order-sheet.html` had a stale `Last revised` date** - the page still read `08/14/2026` (its creation date) despite substantive field-revision and red-flag-approval passes on 08/27/2026 (PRs #252, #254). Updated to `08/27/2026`.
3. Checked off **"Tile order consolidation follow-through"** and **"Deck order sheet - resolve open hardware flag"** in the backlog below (both reverified clean, matching what PR #248 had already found).

**Reverified and still accurate:**
- No broken internal links across all 79 HTML files (scripted check of every `href="*.html"` against the file list).
- Nav is consistent: Interior Trim, Tile, and Painting links present on all 69 sidebar-carrying files; the one file missing all three (`diagnostic-tools.html`) is the known parked orphan, not a miss.
- `foundation-checklist.html` redirects cleanly to `foundation-order-sheet.html`; zero remaining inbound links expect the old checklist. `foundations.html` and `resources.html` both correctly link the new sheet.
- Red-flag files and ages: `remediation.html` (8 spans, open since before 07-19 - **over six weeks**, still the single oldest and highest-priority open item on the site, safety-relevant), `painting.html` (10 spans, open since 08-08 - over three weeks), `interior-trim.html` (13 spans, open since 08-14 - over two weeks, the largest single batch), `tile.html` (3 spans, open since 07-31 - a month), `insulation.html` (2 spans), `excavation.html` (2 spans), `products.html` (1 span). `foundation-order-sheet.html`'s own flags are now clear (approved in PR #254).
- `about.html` footer still reads `&copy; 2025` with no Share/Print/`Last revised` chrome; still parked as a discuss-first item, unchanged across many passes now.
- Footer copyright text inconsistency (`decks.html` "Licensed under CC BY 4.0"; `wage-calculator.html` "Built by Jerry Tyrrell", no Last revised/Share/Print) unchanged, still needs a one-line answer from the user.
- README still describes the pre-GitHub-Pages AWS/S3 setup, unchanged.
- Content-gap items unchanged: no fireblocking checklist, no flatwork/concrete order form, `neighbor-letter.html` still has no fill-in blanks beyond today's colon fix, rough-trade pages still don't cross-link each other in Resources sections.
- Orphan pages unchanged, still parked pending personal-site repo access.

**Suggested priority for next session:** (1) the process fix above - close the two stale review PRs, resolve #253 - is a five-minute GitHub cleanup that stops the review log from drifting further. (2) A `remediation.html` red-flag review is now overdue by a wide margin (six-plus weeks, safety-relevant content); this should be the next content session regardless of what else gets picked. (3) After that, the flag backlog on `interior-trim.html`, `painting.html`, and `tile.html` can reasonably be bundled into one review conversation with the user, since it's the same approve/correct/remove decision three times over. See the plan below for a concrete run order.

### 2026-08-10 - Full-site review (automated)
Since the 2026-07-27 pass: tile print-table-width fix (PR #233), the foundations.html sump crock/radon field note (PR #234), and a new Painting page + worksheet (PR #236, `painting.html` / `painting-worksheet.html`) landed on `main`.

**Implemented this pass (mechanical, no user decision needed):**
1. **`products.html` and `resources.html` were missing all standard footer chrome** - the single biggest finding of this pass. Both pages had no `@media print` stylesheet at all (so printing either page would include the sidebar, header, and buttons), no Share/Print buttons, no `Last revised` line, and no sitewide `<footer>` copyright/contact block - every other content page on the site has all four. This is notable because `resources.html` is the master index linked from every other page's Resources section, and `products.html` is the site's product/SKU reference; both are pages a contractor would actually try to print on a jobsite. Added the standard print stylesheet, Share/Print buttons + `Last revised: 08/10/2026` line, and sitewide footer to both, copied from the current standard (`painting.html`, the most recently built page). No content changed, only the missing chrome.
2. `CLAUDE.md` - backfilled the Work Log gap for PR #236 (Painting page + worksheet, merged 2026-08-08, was never logged).

**Reverified and still accurate:**
- No broken internal links across all 80 HTML files (scripted check of every `href="*.html"` against the file list).
- Nav is consistent: "Painting" link present on all 66 files that carry the sidebar; the 14 files without it are all legitimate exclusions (orphan pages, redirect stubs, `index.html`, and the two deliberately nav-less safety pages `safety-signs.html` / `safety-training-presentation.html`).
- Red-flag files: `products.html` (1 span, unchanged), `remediation.html` (8 spans, unchanged, still the top-priority open item), `insulation.html` (2 spans, unchanged), `excavation.html` (2 spans, unchanged). **New since last pass**: `painting.html` (10 spans - PCA P1/P4, GA-214 finish levels, primer/temperature requirements) and `tile.html` (3 spans, carried over from the 2026-07-31 tile split, still open). Both new pages' flags are recent enough (added 2026-08-08 and 2026-07-31) that they likely just haven't been reviewed yet rather than being stuck.
- `about.html` footer inconsistency still present, still parked as a discuss-first item (unchanged from prior passes).
- `safety-signs.html` / `safety-training-presentation.html` confirmed to have no sidebar nav at all (by design, not a bug) - this is why they don't carry the Painting link either; not a new finding.

**New minor finding, not fixed this pass (low priority, needs a judgment call, not mechanical):**
- **Footer copyright text is inconsistent site-wide** - most pages read "&copy; [year] Remodelers Guide. All rights reserved. | Contact: ...", but `decks.html` alone reads "Licensed under CC BY 4.0" instead, and `wage-calculator.html` uses a different structure entirely ("Built by Jerry Tyrrell", no `Last revised`/Share/Print). Unclear which is intentional (a real licensing decision vs. a stray edit), so left alone. Worth a one-line answer from the user on which text is correct before touching it.

**Still flagged for user decision (needs judgment, unchanged from prior passes):**
- `remediation.html` red-flag review - 8 spans covering PPE minimums, containment/negative-air setup, and the ~19% wood moisture-content treatment threshold. Still the single highest-priority open item on the site: procedural safety content live on the page without approval.
- `painting.html` red-flag review (new) - 10 spans of researched specs (PCA P1/P4, GA-214, primer/temp requirements) pending the same approve/correct/remove decision as any other new page.

**Suggested priority for next session:** with the `products.html`/`resources.html` chrome gap closed, the two highest-value remaining moves are (1) a `remediation.html` red-flag review (oldest open item, safety-relevant) and (2) a `painting.html` red-flag review now that it's had time to settle, both one-page/one-PR sessions. After that, the Roadmap section 1 content gaps (fireblocking checklist, flatwork/concrete order form) remain the two trade areas with no companion printable at all.

### 2026-08-03 - Full-site review (automated)
One content change landed since the 2026-07-27 pass: PR #231 split Tile out of Flooring into its own page (`tile.html`) and got both tile order sheets down to one print page. This pass checked that work plus reverified everything still open.

**Implemented this pass (mechanical, no user decision needed):**
1. `flooring.html` and `tile-finish-order-sheet.html` carried stale `Last revised` dates (`02/16/2026` and `05/03/2026`) despite being substantively rewritten on 07/31/2026 for the tile split. Updated both to `07/31/2026`, matching the pattern of the 07/20 and 07/27 date fixes on other pages. `tile-prep-order-sheet.html` had the same stale date and is corrected alongside its All-Set content change in a companion PR.

**New finding - unreviewed red flags on the new Tile page:**
- `tile.html` carries 3 unapproved `color:red` spans from its 07/31 creation (per the `CLAUDE.md` work log): two Best Practices bullets (dry-lay complex patterns before setting; keep tile from one lot together in a room) and one Client Communication bullet (the flood test holds the shower for a day or more). These are reasonable field-practice notes, not safety-critical like the `remediation.html` flags, but they're still live, unflagged-to-the-user content per the red-flag workflow.

**Reverified and still accurate:**
- No broken internal links across all HTML files (scripted check of every `href="*.html"` against the file list).
- Nav is consistent: `tile.html` was correctly added to all 63 sidebar-carrying files that needed it; the one file missing it (`diagnostic-tools.html`) is a parked orphan page out of scope, not a miss.
- Red-flag files unchanged otherwise: `products.html` (1 span), `remediation.html` (8 spans), `insulation.html` (2 spans), `excavation.html` (2 spans) - all still open, same counts as 07/27.
- `about.html` footer inconsistency still present, still parked as a discuss-first item. Noted in passing: its copyright line also reads "© 2025" - one year stale in addition to lacking Share/Print/Last-revised chrome.
- README still describes the old "Active Navigation Version" / pre-GitHub-Pages setup; still open per section 5.
- Content-gap items unchanged and confirmed still missing: no `flatwork-concrete` order form, no fireblocking/draftstopping checklist, `neighbor-letter.html` still has no fill-in blanks, rough-trade pages (plumbing/electrical/mechanical) still only cross-link via sidebar nav, not in their Resources sections.
- Orphan pages (`construction-dashboard.html`, `building-diagnostics.html`, `diagnostic-tools.html`, `cte-presentation.html`, `newsletter_prompt.html`) unchanged, still parked pending personal-site repo access.

**Still flagged for user decision (not mechanical, needs judgment - unchanged from prior passes):**
- `remediation.html` red-flag review - 8 spans covering PPE minimums, containment/negative-air setup, and the ~19% wood moisture-content treatment threshold. Still the single highest-priority open item on the site: procedural safety content live without approval.
- `tile.html` red-flag review (new, see above) - lower stakes than remediation but open the same way.

### 2026-07-27 - Full-site review (automated)
No content-page files changed since the 2026-07-20 pass landed (`main` only gained that pass's own ROADMAP.md commit), so this pass re-verified prior findings and implemented the zero-judgment mechanical cleanup that had been sitting ready since 2026-07-20.

**Implemented this pass (mechanical, no user decision needed):**
1. `hazardous-materials.html` - fixed footer typo, now reads `Last revised: 05/01/2026`.
2. `air-sealing.html` - added the standard Share/Print buttons and `Last revised: 06/04/2026` line (matching every other content page's footer chrome), plus the corresponding button JS. The print stylesheet already anticipated `#share-btn`/`#print-btn` but the buttons themselves were never added.
3. `safety.html` and `safety-equipment-checklist.html` - updated `Last revised` from `06/04/2026` to `07/10/2026` to match the actual last-edit date (the Safety Reminders fold-in, PRs #225-226).
4. `CLAUDE.md` - backfilled the Work Log gap for PRs #224-227 (folding the Safety Reminders card into the PPE/equipment checklist line-by-line, then dropping the standalone `safety-reminders.html` page).

**Reverified and still accurate:**
- No broken internal links across all HTML files (scripted check of every `href="*.html"` against the file list).
- Nav is still consistent: every content page carries `remediation.html`, `air-sealing.html`, and `safety.html` links; no drift since 2026-07-19.
- Red-flag files unchanged: `products.html` (1 span), `remediation.html` (8 spans), `insulation.html` (2 spans), `excavation.html` (2 spans) - all still open, matching the 2026-07-19/07-20 findings. Note: `remediation.html` was previously described as "six spans"; a precise recount finds 8 (the small-area DIY threshold and the "no dust mask" line were undercounted before, not newly added). No new red-flag files found; `construction-dashboard.html` also carries flags but is a parked orphan page, not in scope.
- `about.html` footer inconsistency still present, still parked as a discuss-first item (not fixed this pass).
- `safety-signs.html` / `safety-training-presentation.html` still intentionally use the stripped-down footer with no date line; not a bug.

**Still flagged for user decision (not mechanical, needs judgment - unchanged from 2026-07-19/20):**
- `remediation.html` red-flag review - 8 spans (recount, see above) covering PPE minimums, containment/negative-air setup, and the ~19% wood moisture-content treatment threshold. This is procedural safety content already live on the page without approval. This is the single highest-priority open item on the site: recommend the user reviews and either approves, corrects, or asks for removal/citation before it sits any longer unflagged-but-unapproved.

**Suggested priority for next session:** with the mechanical debt now cleared, the next highest-value move is a `remediation.html` red-flag review session (get user sign-off, one page, one PR), followed by picking an item from Roadmap section 1 (Content gaps) - the fireblocking checklist or flatwork/concrete order form are the two with no companion artifact at all yet.

### 2026-07-20 - Full-site review (automated)
No content-page files changed since the 2026-07-19 pass (only that pass's own commit landed in between), so this was a verification and deeper-check pass rather than a fresh discovery pass.
- **All 2026-07-19 findings reverified and still open**: red-flag files unchanged (`products.html`, `remediation.html`, `insulation.html`, `excavation.html`); `air-sealing.html` still missing Share/Print buttons and a `Last revised` line; `hazardous-materials.html` still has the missing-colon footer typo; `CLAUDE.md` Work Log still has not been backfilled for PRs #224-227.
- **New finding - stale `Last revised` dates on two actively-edited pages**: `safety.html` and `safety-equipment-checklist.html` both display `Last revised: 06/04/2026`, but git history shows both were substantively edited as recently as 07/10/2026 (the Safety Reminders card merge and later removal). Checked via `git log --follow` against the footer date on every page; these two are the only mismatches found. Low-risk one-line fixes.
- **Confirmed not a bug**: `safety-signs.html` and `safety-training-presentation.html` use a deliberately different, stripped-down `footer.sitefoot` (no Last revised, no Share/Print) with no date line at all. This matches documented user intent for those two pages ("no clutter," presenter/date lines stripped per user review in PRs #210-218), so it's left alone rather than flagged as missing chrome.
- **Suggested priority for next session** (see plan below): this session recommends, in order, (1) surface the `remediation.html` red flags to the user for content approval since they're safety-procedure guidance already live and unapproved, not formatting, so it needs a decision rather than a fix. This is *not* implemented in this pass. (2) A single bundled "mechanical cleanup" PR (in the tradition of PR #198) covering the four zero-judgment fixes below - typo, missing footer chrome, two stale dates, and the `CLAUDE.md` backfill - none of which need user input since none touch content, red-flag, or spec-level judgment calls.

**Proposed mechanical cleanup PR (no user decision needed, ready to implement on request):**
1. `hazardous-materials.html` - fix footer to `Last revised: 05/01/2026` (add missing colon).
2. `air-sealing.html` - add the standard footer chrome (Share/Print buttons + `Last revised` line) to match every other content page.
3. `safety.html` and `safety-equipment-checklist.html` - update `Last revised` to `07/10/2026` to match actual last-edit date.
4. `CLAUDE.md` - backfill Work Log entries for PRs #224-227 (fold Safety Reminders card into PPE checklist line-by-line, then drop the standalone `safety-reminders.html` page), matching the pattern of prior Work Log entries.

**Flagged for user decision (not a mechanical fix, needs judgment):**
- `remediation.html` red-flag review - six spans covering PPE minimums, containment/negative-air setup, and the ~19% wood moisture-content treatment threshold. This is procedural safety content already live on the page without approval. Recommend the user reviews and either approves, corrects, or asks for removal/citation before it sits any longer unflagged-but-unapproved.

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
- [x] **Tile order consolidation follow-through** - Reverified 2026-08-31: `tile-order-sheet.html` is a clean redirect stub with zero remaining inbound links.

## 2. Order forms - finish the Westbury-model pass
Standing direction: present real, orderable options and constrain clients to common choices instead of open write-in lines; keep each form to a single print page where possible.

- [ ] **Audit remaining open write-ins** - Sweep every order form for fields that come in fixed sizes/SKUs but are still open lines, and convert to pick lists (the siding reveal and countertop material/edge work in PR #206 is the model).
- [x] **Deck order sheet - resolve open hardware flag** - Reverified 2026-08-31: no `HDU5-SDS2.5` reference remains; `DTT2Z` is present carrying the lateral tie.
- [ ] **Quantity-note verification** - Deck fastener pack/count notes (clips/bag, pcs/lb) were generalized to "supplier scales" reminders; revisit if the user wants hard counts for any specific product.
- [ ] **Consistent form chrome** - Confirm every order form uses the standard `h1.form-title` "Remodelers Guide:" prefix, job-header fill lines, print-credit line, `.form-actions` wrapper, and `@page` rule. A few legacy sheets may predate the standard.

## 3. Citation and spec verification (red-flag cleanup)
Per workflow, researched specs stay red until the user approves them. These are the open flags, current as of the 2026-07-19 sweep.

- [ ] **Remediation page PPE/containment red flags (highest priority in this group)** - `remediation.html` has six unflagged-nowhere spans covering: small-area (under ~10 sq ft) EPA DIY threshold, PPE minimums for small vs. large cleanup (N95 vs. half-face P100/Tyvek), the "no dust mask" line, HVAC register sealing and negative-air containment setup, bagging demo material, and a ~19% wood moisture-content threshold (USDA Forest Products Lab) before treating. This is procedural safety guidance a crew could act on directly, not a background spec, so it's worth prioritizing over the others in this section.
- [ ] **Insulation soffit NFA rows** - Aluminum and vinyl soffit net-free-area numbers are still flagged; aluminum is likely understated (manufacturer full-vent panels run roughly 14-19.6 sq in/ft). Verify against current manufacturer data and unflag or correct.
- [ ] **Excavation utility-coordination notes** - Two flags on `excavation.html`: electrical service drop relocation lead time ("sometimes weeks out") and the refrigerant-recovery-requires-licensed-tech line for AC condensers in the dig zone. Both read as reasonable but are unconfirmed against the user's actual field experience.
- [ ] **Products page respirator note** - One flag on `products.html` on the Uqezagpa full-face respirator listing, asking to confirm the cartridge rating (organic vapor vs. P100) matches intended task use.
- [ ] **Painting page red flags** - `painting.html` has 10 spans of researched specs (PCA P1/P4, GA-214 finish levels, primer/temperature requirements) pending approval, open since 2026-08-08.
- [ ] **Interior Trim page red flags** - `interior-trim.html` has 13 spans (door sizes, RO rule of thumb, hinge fastening, bore/backset, casing reveal, nail gauges, acclimation time, garage-to-house door code items) pending approval, open since 2026-08-14. The garage door self-closing/self-latching span specifically needs a citation source, not just an approve/reject, since the exact code-cycle text couldn't be verified past the 2021 edition.
- [ ] **Standing red-flag sweep** - Grep the site for `color:red` spans and triage each: approve, correct, or surface to the user. Keeps the "added content" debt visible. (As of 2026-08-31: `products.html`, `remediation.html`, `insulation.html`, `excavation.html`, `painting.html`, `tile.html`, `interior-trim.html` are the files with open flags. `foundation-order-sheet.html`'s flags were cleared in PR #254.)

## 4. Structural / maintainability (the big one)
The site has no templating: every one of the 70+ HTML files carries its own full copy of the sidebar nav, the inline mobile-nav script, and the print-style block. Any nav change touches every file.

- [ ] **Evaluate a lightweight build or include step** - Options worth weighing: (a) a small static-site generator or partial-include build, (b) a tiny client-side JS include for the shared nav, (c) keep hand-edited HTML but standardize a generator script. Each has tradeoffs against the "plain HTML, no build system" simplicity the user values. This is a discussion item, not a do-it item - get a decision before touching it.
- [ ] **Shared CSS/JS extraction** - The print-style block and mobile-nav script are duplicated inline in every file. If a build step is rejected, at minimum consider moving the script to a shared file and the print rules into `styles.css`.
- [ ] **Nav-update script in repo** - Adding a nav link currently relies on an ad-hoc Python/sed script written fresh each time. Commit a reusable, documented `update-nav` script so the next addition is one command and indentation stays consistent.

## 5. Housekeeping and small polish
Low-risk, high-tidiness items.

- [ ] **Redirect-stub inventory** - `roofing-order-sheet-2.html`, `siding-and-trim-order-sheat.html` (note the misspelling), `tile-order-sheet.html`, and `safety-reminders.html` are all redirect stubs with zero remaining inbound links (verified 2026-07-19). Decide whether to keep them indefinitely for old bookmarks/print copies or sunset them on a date.
- [x] **resources.html completeness** - Re-verified 2026-07-19: every current order sheet, checklist, and safety printable is indexed, including the recent PPE checklist consolidation.
- [x] **`air-sealing.html` missing standard footer chrome** - Fixed 2026-07-27: added Share/Print buttons and a `Last revised: 06/04/2026` line.
- [x] **`hazardous-materials.html` footer typo** - Fixed 2026-07-27: now reads "Last revised: 05/01/2026".
- [ ] **`about.html` footer inconsistency (discuss, don't just fix)** - The home page uses an older, simpler footer with no Share/Print buttons, no feedback-form link, and no `Last revised` line, while all other pages (including utility pages like `wage-calculator.html`) share those elements. Could be intentional for the homepage; confirm with the user before changing.
- [x] **`CLAUDE.md` work log gap** - Backfilled 2026-07-27: PRs #224-227 (fold Safety Reminders card into PPE checklist, then drop `safety-reminders.html`) now recorded in the Work Log.
- [ ] **README is stale** - `README.md` still describes the AWS/S3 upload process and an "Active Navigation" version. The site is on GitHub Pages now. Rewrite it to reflect the current hosting, structure, and the conventions in `CLAUDE.md`.
- [x] **Last revised dates** - Spot-checked 2026-07-27: `safety.html` and `safety-equipment-checklist.html` were stale (`06/04/2026` vs. actual last edit `07/10/2026`); fixed. No other mismatches found.
- [x] **`products.html` / `resources.html` missing all footer chrome** - Fixed 2026-08-10: both pages had no print stylesheet, no Share/Print buttons, no `Last revised` line, and no sitewide footer at all. Added the standard set matching every other content page.
- [ ] **Footer copyright text inconsistency** - `decks.html` alone uses "Licensed under CC BY 4.0" instead of the standard "All rights reserved" text used on every other page; `wage-calculator.html` uses a different footer structure entirely ("Built by Jerry Tyrrell", no Last revised/Share/Print). Confirm with the user which is intentional before changing either.

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
