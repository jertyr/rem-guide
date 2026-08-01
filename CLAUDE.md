# Remodelers Guide - Project Memory

## Workflow Preferences
- **PRs**: Provide the PR link — the user will click it and handle the merge themselves
- **Pushes**: Same — just push and give the link, don't spend tokens retrying or automating merges
- **Memory**: Update this file each session with what was done so future sessions have context

## Working Style and Relationship
- **Iterative review**: User reviews output and gives feedback; Claude applies it in follow-up PRs. Expect multiple rounds per page — this is normal, not a failure
- **Red-flag pattern**: When Claude adds content that wasn't in the user's notes (researched specs, code citations, product details), wrap it in `<span style="color:red">` so the user can spot and approve it before it goes live. Remove flags once approved
- **Field guide workflow**: User provides raw notes or bullet points; Claude builds the full page to match site format. Claude fills gaps with research but flags everything added
- **No em-dashes**: User dislikes em-dashes. Use periods, semicolons, or conjunctions instead. This is a standing rule for all content
- **Small PRs preferred**: One logical change per PR. Don't bundle unrelated fixes
- **Always a new PR**: Every round of changes gets a fresh branch and PR, even when responding to feedback on a just-merged PR. Never push to an existing open PR after the user has reviewed it
- **No unsolicited cleanup**: Don't refactor, reorganize, or "improve" things the user didn't ask about. Stay in scope
- **Trust the user's field knowledge**: User is a working contractor. Don't hedge or over-explain trade basics. Flag code/spec details for review, not common practice
- **Concise responses**: Short answers. Don't recap what you just did at length — the PR diff speaks for itself

## Project Overview
- Static HTML site hosted on GitHub Pages at `jertyr.github.io/rem-guide`
- Construction/remodeling reference guide with ~48 HTML pages
- Single shared `styles.css` for all styling
- No build system or framework — plain HTML, CSS, and vanilla JS
- Migrated from AWS to GitHub Pages in PRs #1–6

## Architecture
- **Layout**: Fixed header + fixed sidebar nav + scrollable main content
- **Sidebar**: 280px wide, lists all pages as nav links. Each HTML file has its own copy of the full nav (no templating)
- **Mobile**: Sidebar hidden by default, slides in from left via hamburger button (`.mobile-nav-toggle`). Toggled with `.active` class. Backdrop overlay via large `box-shadow` spread on `.sidebar.active`
- **Breakpoint**: 768px for mobile layout
- **Active page**: `.active` class on the `<a>` + `.active-section` on parent `<li>` (only `.active` is styled)
- **JS**: Each HTML file has identical inline `<script>` for mobile nav toggle (no shared JS file)
- **Adding nav links**: Must update every HTML file; use a script to insert after a known nav item

## Key Files
- `styles.css` — All site styling including responsive/mobile rules
- `about.html` — Home page
- `index.html` — Entry point (redirects or mirrors home)
- `update-page.md` — Skill document for standardizing page rewrites

## CSS Variables (from `:root`)
- `--primary-color: #2C3E50` (deep slate blue)
- `--secondary-color: #E67E22` (burnt orange, used for CTAs/highlights)
- `--sidebar-bg: #F8F6F3` (warm off-white, desktop only — mobile overridden to #FFFFFF)
- `--active-bg: #E8F4F8` (light blue for active nav item)
- `--header-height: 58px`
- `--sidebar-width: 280px`

## Page Format (Standard Structure)
Pages follow a flexible but consistent format (see `update-page.md` for full skill):

1. **H1 Title** + 1–2 sentence intro paragraph (scope and context)
2. **Quick Reference** — bulleted key reminders for jobsite scanning
3. **Prerequisites** — what needs to be done/on-site before this phase (used on some pages)
4. **Materials and Tools** — bulleted, nested by category (used on some pages)
5. **Process** — nested bullet lists, 2–3 levels deep, organized by phase
6. **Best Practices** — positive framing, preventing common mistakes
7. **Inspections** — code citations, what inspectors check
8. **Client Communication** — expectations, warranties, timing
9. **Resources** — links to checklists, order forms, external guides. Always the last section before the footer
10. **Footer** — Share/Print buttons + `Last revised: MM/DD/YYYY`

Not every page needs every section — content drives structure.

## Content Conventions
- **Audience**: Contractors and homeowners at 80th–90th percentile skill; people who know the work but want key reminders
- **Writing style**: Bullet points over paragraphs, active voice, no filler
- **Checklist items**: Concise — just the device/item name, not verbose descriptions with specs
- **Code citations**: In parentheses — `(R905.1.2)`, `(IRC Table R602.3(1))`
- **Product names**: Use real names (James Hardie, LP SmartSide, Azek, Trex, etc.)
- **Checkbox format** (used on Pre-Trade Checklist): `☐` character + `list-style-type: none`
- **Images**: `<figure><img /></figure>` tags
- **Print styles**: Identical template in every page `<style>` tag (hides nav, sidebar, buttons, footer)
- **Share button**: Uses Web Share API; hidden if unavailable
- **Standalone checklists/references**: Not in sidebar nav — linked from parent page (e.g., `demolition-checklist.html`, `electrical-device-checklist.html`)

## Lessons Learned
- **`transform` breaks `position: fixed` children**: The sidebar uses `transform: translateX()` for slide-in. This makes the sidebar a containing block for `position: fixed` descendants (like `::before`), positioning them relative to the sidebar instead of the viewport. Fix: use `box-shadow: 0 0 0 9999px rgba(0,0,0,0.5)` instead of a `::before` pseudo-element for backdrop.
- **Process section structure**: Evolved through multiple iterations on the Windows page (narrative → ordered list → nested unordered lists). Settled on 2–3 level nested `<ul>` as the standard.
- **Complex pages need iterative refinement**: Siding & Trim took 7 PRs (#34–40) to finalize — initial rewrite, then fastener specs, resource links, and section reorganization in follow-ups. Budget for this.
- **Nav changes touch every file**: Adding a new nav link requires updating all 42+ HTML files. Use a script (Python or sed) to insert after a known anchor line. Always verify indentation matches each file.
- **Decks page had 9+ PRs**: Product tables, navigation, and share button fixes cascaded across sessions. Lesson: test new pages end-to-end before moving on.

## Project History (PR Summary)
- **PRs #1–6**: Migrated from AWS to GitHub Pages; streamlined nav, added breadcrumbs/checkboxes to resource files, redesigned stylesheet, added share/print buttons
- **PRs #7–8**: Framing page rewrite and checklist updates (blocking list, materials, terminology)
- **PRs #9–17**: Created Decks page with Trex product tables; multiple rounds of table refinement
- **PRs #18**: Framing checklist update (blockers→blocks, raised floors)
- **PRs #19–25**: Fixed navigation across all pages after Decks addition; fixed share button; mobile layout fixes (overlay sidebar, fixed button, full-width content)
- **PR #26**: Windows page rewrite per update-page skill
- **PRs #27–29**: Windows page iteration (process section structure evolution)
- **PR #30**: Exterior Doors page redesign to match Windows format
- **PRs #31–33**: Roofing page redesign; added update-page skill document; insurance notification tip
- **PRs #34–40**: Siding & Trim page (7 PRs: initial rewrite, tools/fasteners integration, external links, spec approvals); Gutters page rewrite; Pre-Trade converted to checkbox format
- **PR #41**: HVAC page rewrite per update-page skill
- **PRs #42–43**: HVAC combustion air section (IRC G2407), gas pipe sizing table corrections
- **PR #44**: Plumbing Rough Install page update
- **PRs #45–50**: Electrical Rough Install page + device checklist; mobile nav fix (white sidebar, box-shadow backdrop); nav update across all files
- **PRs #51–54**: Drywall, Insulation, Flooring, and Countertops pages created from field guide notes; nav links added across all files
- **PRs #55–63**: Insulation page — iterative approval of red-flagged researched content; timing clarification (rough building inspection, not all trade inspections); attic exception; zone 5/6 table updates; fiberglass R-value corrections; em-dashes removed
- **PRs #64–66**: Drywall page refinements — intro/quick reference/materials/prerequisites streamlined; coordination and stocking merged into process; em-dashes removed; flooring page tightened; countertops feedback applied
- **PR #67**: Insulation timing final pass
- **PRs #68–69**: Deck maintenance section added (composite + IPE/tropical hardwood); drywall process reordered (cleanup before prime); drywall order sheet added and linked from drywall page
- **PRs #70–75**: Exterior Door Order Form created (`exterior-door-order-form.html`); iterated on layout (2×2 bottom grid for Storm Door alignment), print whitespace fixes (notes-box, button paragraph, hr dividers), copyright credit line, casing/extension/shoe fields, label bold removed. Insulation attic ventilation section (R806) added.

## Work Log

### Session: 2026-02-09 — Fix mobile nav styling
- **Issue 1**: Mobile sidebar had gray (`#F8F6F3`) background — user wanted white
- **Issue 2**: `::before` backdrop overlay rendered incorrectly due to `transform` on sidebar creating a containing block — caused dark gray overlay on top portion of nav items
- **Fix**: Set `background: #FFFFFF` on mobile `.sidebar`; replaced `::before` backdrop with `box-shadow: 0 0 0 9999px rgba(0,0,0,0.5)` on `.sidebar.active`
- **Branch**: `claude/fix-mobile-nav-styling-gR5zk`

### Session: 2026-02-09 — Add Electrical Rough Install page
- **New page**: `electrical-rough-install.html` — matches format of HVAC and Plumbing pages
- **Content**: Quick reference, prerequisites, process (includes temp power/lighting step), room-by-room "What Needs Power?" checklist (concise item names only), inspections, best practices, client communication
- **Standalone reference**: `electrical-device-checklist.html` — printable device list by room, linked from the electrical rough page (not in nav, same pattern as other checklist pages)
- **Nav**: Added "Electrical" link to all 41 HTML files, positioned between Plumbing and Stairs
- **Style note**: User prefers concise checklist items (just the device/item name), not verbose descriptions with specs
- **Branch**: `claude/fix-mobile-nav-styling-gR5zk`

### Session: 2026-02-09 — Clean up Electrical Rough Install page
- **Removed**: Quick Reference section (redundant with Prerequisites)
- **Prerequisites cleanup**: Removed framing inspection note (doesn't happen before electrical), removed HVAC/plumbing coordination note, removed USB/dimmer selection notes
- **Process reorganization**: Moved temporary power/lighting to end of Process section (last step in typical workflow); removed spider box reference
- **Dedicated circuits**: Separated into code-required (with NEC citations) vs recommended; fixed items that don't actually require dedicated circuits
- **Inspections**: Removed "schedule early" reminder
- **Branch**: `claude/cleanup-electrical-rough-l3eCO`

### Session: 2026-02-10 — Add Drywall, Insulation, and Flooring pages
- **New pages created from user's field guide notes**: `drywall.html`, `insulation.html`, `flooring.html`, `countertops.html`
- **Pattern**: User provides extracted notes; Claude creates page matching site format and flags researched/added content in `<span style="color:red">` for user review
- **Drywall**: Coordination, stocking/delivery logistics, hanging (glue + screw per MRC 702.3.5), finishing (3-coat), screw inspection, finish levels 0–5, sequencing into paint
- **Insulation**: Material options (fiberglass, mineral wool, cellulose, spray foam, foam board), Michigan code requirements zones 5 & 6, fire code clearances (R302.13), ASTM E84 exposure limits, spray foam certificate requirement
- **Flooring**: Wood floor best practices (moisture, vapor retarder, joist direction), engineered + solid fastening specs by plank width, LVP (floating vs glue-down), tile process + shower coordination + flood test inspection
- **Countertops**: Quartz-focused page — template/fabrication workflow, build-to-print option, other stone pieces (niches, thresholds, curbs, benches), Goo Gone vs Goof Off distinction, quartz vs quartzite, cleaning/care guide. Heavier research needed — user notes were thin; researched cleaning products, heat damage thresholds, templating process
- **Nav order**: ...Electrical → Insulation → Drywall → Flooring → Countertops → Stairs
- **Nav updates**: Used Python script to insert after known anchor line in all HTML files (same pattern as previous nav additions)
- **Branch**: `claude/create-drywall-page-8VgCU`

### Session: 2026-02-15 — Update Insulation Timing
- **Changed**: Insulation timing clarification from "after all rough trade inspections pass" to "after the rough building inspection"
- **Added**: Exception for attic insulation, which can be installed after drywall as long as it's accessible for inspection
- **Updated sections**: Intro paragraph, Quick Reference (clarified wall/ceiling vs attic timing, added note about depth markers for loose fill), Process (Pre-Insulation Check), and Inspections
- **Scope clarification**: Changed intro from Michigan-specific focus to broader applicability; notes content applies across US and IRC-based jurisdictions while maintaining Michigan zones 5 & 6 as primary examples
- **Key clarification**: Wall/ceiling insulation must be inspected before drywall; attic insulation can go in after drywall lid is up as long as it remains accessible for inspection (loose fill installers use depth markers/measuring sticks for easy verification)
- **Code Requirements table**: Added paragraph explaining Michigan climate zone 5/6 split (south vs north of I-96/Lansing area) with link to DOE Building America climate zone map; added code citations linking to 2015 MRC Chapter 11 and 2015 IECC Table R402.1.2
- **Fiberglass Materials**: Removed R-15 and R-21 fiberglass options; not locally available; builders use R-13 for 2×4 walls and R-19 for 2×6 walls, and switch to Rockwool for higher R-values
- **Em-dashes removed**: Replaced all em-dashes throughout page with periods, semicolons, or conjunctions as appropriate for clearer readability
- **Reasoning**: More accurately reflects typical construction sequencing where insulation happens after rough building inspection rather than waiting for all individual trade inspections
- **Branch**: `claude/update-insulation-timing-B1z2k`

### Sessions: 2026-02-15 to 2026-02-21 — Insulation + Drywall iterations, Deck maintenance
- **Insulation (PRs #55–63, #67)**: Multiple passes approving red-flagged researched content; attic exception added; zone split paragraph added; R-15/R-21 removed; em-dashes replaced; final timing pass
- **Drywall (PRs #64–66)**: Streamlined intro, quick reference, materials, prerequisites; merged coordination/stocking into process; em-dashes removed; process reordered (cleanup before prime in PRs #68–69)
- **Flooring (PR #65)**: Tightened copy, removed em-dashes, restructured
- **Countertops (PR #66)**: Applied user feedback
- **Drywall order sheet (PR #69)**: Created standalone order sheet, linked from drywall page
- **Deck maintenance (PRs #68–69)**: Added maintenance section to decks page covering composite (Trex) and IPE/tropical hardwood care
- **Branch**: `claude/add-deck-maintenance-OVKWe`

### Session: 2026-02-25 — Exterior Door Order Form created and iterated
- **New file**: `exterior-door-order-form.html` — printable one-page order form linked from exterior-doors.html (not in sidebar nav)
- **Layout**: Two-column top grid (Configuration + Draw box left / Sizing + Wall & Installation right); 2×2 bottom grid (Glass & Lites / Screens top row, Hardware & Color / Storm Door bottom row — forced into same grid row so headings align vertically)
- **Fields**: Job header (Location, Brand, Line); Configuration (Type, Hand, Swing); Draw box; Sizing (RO, Frame, Brickmold); Wall & Installation (Wall depth, Install type, Casing, Extension, Shoe — each on own line); Glass & Lites; Hardware & Color; Screens; Storm Door; Notes / Special Instructions
- **Print behavior**: Single-page letter portrait; sidebar/header/footer/buttons all hidden; draw-box forced to 2.4in; notes-box hidden in print (heading acts as end-of-form, blank page space is writing area); copyright credit line visible only in print
- **Copyright**: `© 2026 Remodelers Guide — jertyr.github.io/rem-guide` — print-only, centered, below notes heading
- **Styling**: h3 headings bold, all label/row text not bold (removed `<strong>` from option-row labels); no hr dividers between sections
- **Print bugs fixed**: Three hr dividers removed; notes-box screen CSS `height: 1.5in` explicitly hidden in print (was bleeding through); button `<p>` wrapped in `.form-actions` and hidden in print; print-credit margin-top trimmed from 1.5rem to 0.5rem
- **Branch**: `claude/add-ventilation-section-O0wc0`

### Session: 2026-02-25 — Insulation page: attic ventilation section
- **Added**: New "Attic Ventilation (R806)" section covering the 1:150 standard rule with a worked example, the 1:300 exception conditions, and installation requirements (baffles, 1" clear channel, pest screening)
- **Added**: NFA reference table for the most common products contractors use: OC VentSure strip ridge vent (20 sq. in./LF), fully lanced aluminum soffit vent 8×16 (50–65 sq. in./vent), James Hardie vented soffit (5 sq. in./LF), vented vinyl soffit (6–9 sq. in./LF). Table is red-flagged pending user approval
- **Added**: Ventilation baffle bullet to Best Practices — install baffles in every rafter/truss bay before blowing
- **Condensed**: Two fire code clearance bullets (combustible insulation + kraft facing) merged into one
- **Removed**: Remodeling Watch Points section, Unvented (Hot Roof) Assemblies section, Quick Ventilation Math section (user wanted to keep only the practical field reference content)
- **Site philosophy note**: Goal is practical cheat-sheet for builders directing crews — give them the numbers and code basis to be the expert on site, not background theory
- **Branch**: `claude/add-ventilation-section-O0wc0`
- **PR**: #75 (pending merge)

### Session: 2026-04-29 — Checklist reformats (job-start and jobsite-protection)
- **Checklist style standard established**: Reformatted `job-start-materials-checklist.html` and `jobsite-protection-checklist.html` to match Westbury order sheet style
- **Changes**: `h1.form-title` with "Remodelers Guide: [Title]" prefix; job-header with Job Name / Date fill lines; `h3.form-section` section headings with border-bottom; 2-column grid layout on screen and in print; print-credit line; `.form-actions` wrapper; `@page` print rule
- **Job Start layout**: General + Misc in left column, Lead Demolition in right
- **Jobsite Protection layout**: Process in left column, Materials / Tools in right
- **Jobsite Protection item splits**: "Blue and white painters tape" → two lines; painters plastic split by mil weight into four lines; "Floor protection (Albert Floorotex, reinforced builders paper)" → two lines
- **Branch**: `claude/print-friendly-checklist-qZF1L`

### Session: 2026-06-04 — New Safety page + printables (post OSHA inspection)
- **Context**: Had an OSHA inspector on site; building a no-nonsense, citation-backed safety reference that is implementation-agnostic (not tied to one company's strategy). Future: van copies live in each carpenter's van
- **New page** `safety.html` (nav: after Jobsite Protection): Required PPE & Equipment checklist with OSHA 29 CFR 1926 citations per item; "When Is a Hard Hat Required?" exposure-based section (1926.100, ANSI Z89.1 types/classes); Written Safety Plan & Emergency Contacts; Site Safety Walks (1926.20(b)(2) competent person); Training & Certifications (first aid/CPR, respirator fit test, silica 1926.1153, ladder 1926.1060, safety coordinator/competent person 1926.32(f))
- **Key citations researched**: eye 1926.102 (Z87/Z87.1); head 1926.100 (Z89.1); hearing 1926.101/1926.52 (90 dBA TWA, 85 make-available); respirators 1910.134 (1926.103 defers to it; fit test annual per (f)(2); medical eval NOT fixed-annual); first aid 1926.50, supplies (d), posted emergency #s (f); fire ext 1926.150(c) (2A/3000sqft/100ft, 10B within 50ft of >5gal flammable); GFCI 1926.404(b)(1) (temp 120V 15/20A receptacles, or AEGCP); HazCom/SDS 1910.1200 (adopted by 1926.59); EAP 1926.35; foot 1926.96; gloves/general PPE 1926.95; hi-vis 1926.201/MUTCD; ladders 1926.1053 (4:1 ratio, extend 3ft above landing) + training 1926.1060; silica 1926.1153 (Table 1, written exposure control plan (g), training (i)); accident prevention program 1926.20(b), competent person def 1926.32(f)
- **New printables (not in nav, linked from safety.html + resources.html)**: `safety-equipment-checklist.html` (van PPE/equipment list, order-sheet style, citations), `safety-inspection-form.html` (competent-person site safety walk short form with corrective-actions box + signature, per 1926.20(b)(2))
- **Red flags**: Per workflow, all researched OSHA citations + items added beyond user's notes (gloves, foot protection, hi-vis) wrapped in `<span style="color:red">` for approval
- **Future requests noted**: common safety-reminders resource (ladder setup 4:1 / 3ft above roofline, etc.); signage resource (pull/create site signage)
- **Branch**: `claude/remodelers-guide-safety-2aKIG` (PR #187)
- **Follow-up (same branch/PR #187)**: Added two more resources requested next:
  - `safety-reminders.html`: ONE-page at-a-glance card (dead-simple memorable phrases, defensible to an OSHA official), van copy. Ladder phrased "4 in 1 and 3 ft above the roofline" (ladder line was just user's example, not the only item). Includes stepladder top-step rule (1926.1053(b)(13): top/top step not used), fall protection at 6 ft (1926.501(b)(1)), cover/mark holes (501(b)(4)), 3 points of contact, belt buckle between rails. Citations kept small/muted (red-flagged) since carpenters don't need them but they make lines defensible
  - `safety-signage.html`: which signs to post + ANSI Z535 signal-word/color system (1926.200), and a fillable Emergency Information posting template (1926.50(f) required posting). Links to OSHA + suppliers (pull or create signs)
  - Both linked from safety.html Resources and resources.html Safety section
  - User's intent for reminders card: pithy, easy-to-remember, fits one page, carried with safety gear

### Session: 2026-06-12 — Full-site review pass (5 PRs)
- **PR #198** (`claude/remodelers-guide-review-3nsleq`): mechanical cleanup. Typos (undistrurbed, dop not, Lets), unclosed `</li>` x2, prose em-dashes removed from 16 pages (form-table label dashes and print-credit em-dash left as designed), renamed `siding-and-trim-order-sheat.html` -> `-sheet.html` with redirect stub, fixed stale navs (products, example-concrete), indexed 10 missing order sheets/checklists on resources.html
- **PR #199** (`claude/checklist-retrofits-3nsleq`, base #198): retrofitted the 4 old flat checklists (start framing, complete framing, site set up, window/door install) to the standard form template; window/door checklist gained an Install Verification section sourced from the windows/exterior-doors pages
- **PR #200** (`claude/stairs-page-3nsleq`, base #198): stairs.html restructured to standard format (was h2-only WordPress export); new `stair-parts-order-sheet.html` linked from stairs + resources
- **PR #201** (`claude/pre-construction-3nsleq`, base #198): Site Set Up page rewritten from paragraphs to bullets with Quick Reference; all content kept
- **PR #202** (`claude/citation-pass-3nsleq`, base #198): web-verified citations. Approved/unflagged: all OSHA cites on safety pages, products respirator note, excavation notes, VentSure 20 sq in/LF + Hardie 5 sq in/LF NFA rows, verified Simpson SKUs. Corrected (re-flagged): LUS212Z->HUS212, LSCA28->LSSR28Z, CBE44/66->ABU44Z/66Z, LSCA212->HUS212, HHUSS212->LSCZ, angle bracket size labels, MISS DIG 3-working-day legal minimum (MCL 460.725). Added flagged MRC plumbing citations (P3005.3, P2503.5.1 5-ft head, P2503.7, Table P3105.1) + plumbing Resources section. Drywall cite fixed to Table R702.3.5
- **User decisions this session**: feedback email `jefferson.claw.11@gmail.com` is correct (a bot that closes the improvement loop); orphan pages (construction-dashboard, building-diagnostics, diagnostic-tools, cte-presentation, newsletter_prompt) are NOT to be deleted — user wants them migrated to his personal website repo, which is not accessible from these sessions yet
- **Still flagged pending verification**: insulation aluminum/vinyl soffit NFA rows (aluminum likely understated; manufacturer full-vent panels run 14-19.6 sq in/ft), deck order sheet fastener quantity notes (pcs/lb, clips/bag)
- **Known remaining gaps (not yet requested)**: flatwork order form, fireblocking checklist, gutter price sheet is static July 2022 data, roofing-order-sheet-2.html ugly filename, neighbor letter has no fill-in blanks, rough-trade pages don't cross-link each other, tile-order-sheet.html orphan superseded by prep/finish split

### Session: 2026-06-24 — Order form improvement pass (PR #206)
- **Context**: User wants all order forms moved toward the Westbury model: present real, orderable options and limit clients to common choices, rather than open write-in lines for things that come in fixed sizes/SKUs. Standing quality bar going forward: keep each form to a single print page if possible (don't lose fidelity over it), dense and useful
- **Branch/PR**: `claude/order-forms-improvements-8g0ei9` (PR #206) — single working branch for the whole pass
- **Review findings (per-form state)**: window/door/tile-prep/tile-finish already on the Westbury model; roofing good content but ugly `-2` filename; siding/countertop had open fill-ins to constrain; deck had wrong/red-flagged Simpson SKUs; wood-lvp and tile-order use `<strong>` bold labels (against form convention); tile-order is an orphan superseded by prep/finish; gutter + concrete price sheets still old WordPress format
- **Done this session**:
  - **Siding**: Reveal -> pick list (4"/5"/7"/8" + Other). User decisions: leave F/J/UT rows and Brand/Style/Color open; window/door casing stay write-in; roof color stays open and metal-accessory brand doesn't matter (only shingle brand)
  - **Deck**: applied the PR #202 SKU corrections that had never actually been written to the file (LSCA28->LSSR28Z, CBE44/66->ABU44Z/66Z, HHUSS212->LSCZ); fixed angle-bracket leg labels (A34 = 2½", A35 = 4½"; old 3"/4"/5" wrong, web-verified). GRK RSS = 100/box (ships by count not by lb, verified). HeadLok = 50/pack (user-confirmed). Clip/joist-tape counts -> generic order reminders (user uses different clip kinds; supplier scales). All deck red flags cleared
  - **GALVANIZATION CORRECTION (important)**: the PR #202 note "LUS212Z->HUS212" and "LSCA212->HUS212" was WRONG for a deck. **HUS212 only comes in G90 standard galvanizing, NOT ZMAX**, so it is under-spec'd for pressure-treated/exterior. Reverted both the 2×12 joist hanger and the stringer-to-header to **LUS212Z (ZMAX)**, which is the part actually used on PT deck joists. Rule of thumb confirmed with user: **decks need galvanized (ZMAX/HDG/Z suffix), interior framing generally does not** (G90 standard is fine indoors)
  - **Framing sheet audit**: correct as-is. Interior connectors are standard finish without Z (LUS26, H2.5A, LSTA18, BC46, HUC28-2, LSC); the one galvanized item, post base ABA44Z, is right because it sits on concrete. Anchor bolts/sill are PT-contact so galvanized by default
  - **Open flag for user**: deck HDU5-SDS2.5 hold-down is not a ZMAX part; on a typical deck the lateral load is handled by DTT2Z (already listed). Asked user whether to drop HDU5 or keep it
  - **Lumber**: added stocked-length reminder line (dimensional 8–16'; precut studs 92⅝"/104⅝"). Supplier scales quantities
  - **Countertop**: constrained Material / Thickness / Sheen / Edge profile to pick lists (Color + dimensions left open). NOTE: this form's `.opts` class is `white-space:nowrap`, so long lists (Material, Profile) need inline `white-space:normal` to wrap
- **Hardware follow-up (same PR)**: user asked whether framing vs deck hardware still make sense (decks need galvanized, framing generally not). Audited both. Dropped deck HDU5-SDS2.5 hold-down (not ZMAX; DTT2Z carries the lateral tie) and the rarely-used A23Z angle, both per user
- **Mechanical cleanup done (same PR)**: renamed `roofing-order-sheet-2.html` -> `roofing-order-sheet.html` with redirect stub at the old name (updated inbound links on roofing.html + resources.html); removed `<strong>` bold labels from wood-lvp-order-sheet job header / field rows (left the table Total bold); converted orphaned `tile-order-sheet.html` to a redirect stub (refresh to flooring.html, body links both prep + finish sheets)
- **USER DECISION**: gutter + concrete price sheets stay as-is, "fossils for education only" — do NOT reformat or update. Removed from the queue

### Sessions: 2026-07-02 to 2026-07-08 — Safety training presentation, printable signs, streamlining pass
- **Safety Training Presentation** (`safety-training-presentation.html`, PRs #210/#211/#215/#216): scrollable in-house training page (NOT a slide deck; user rejected deck format). Six modules: Ladder (Inspect / Set up / Climbing), Silica (Hazard incl. drywall dust / Dust control / On Site), Fall Protection, Hazard Communication, PPE Basics. Fire extinguisher module removed per user. Style: quiet page, plain title hero, tight bullet cards, NO clutter (no agenda, module numbers, intro lines, memory-hook callouts, presenter/date lines; user stripped all of these in review). ALL citations removed per user decision: "all or nothing" — training page carries zero citations; safety.html is the citation reference
- **Printable signs** (`safety-signs.html`, PRs #212/#213/#218): replaced the old about-signage page (`safety-signage.html` -> redirect stub) with actual printable signs, one per landscape page, ANSI Z535 colors, no imagery. Screen-only gray meta notes under each sign (post location + OSHA cite) do not print. Single general-purpose `construction-site-sign.html` (Caution / Construction Site / Appropriate PPE / fillable emergency block) created because user finds the individual signs mostly unnecessary on residential remodels ("we don't always wear the full getup")
- **GitHub Pages outage (~24h, 07-02 to 07-04)**: every deploy failed at syncing_files ("Deployment failed, try again later"); one run stuck in `queued` held the deploy lock. Self-cleared eventually. Playbook: check run list via API, re-run failed jobs, Settings->Pages source reset, delete github-pages environment, empty-commit PR to force rebuild; if all fails it's GitHub-side, wait or contact support. Site serves last SUCCESSFUL deploy, so "merged but page not updating" usually = failed/missing deploy, not content
- **PHILOSOPHY DECISION (2026-07-08)**: the Remodelers Guide is a national reference; generic unfilled forms only. Company/location-specific resources (pre-filled hospital signs, Buildertrend mentions) get printed to PDF for the user (delivered via chat) and then removed from the site. Sushi-menu principle: one artifact, multiple functions (menu + order form + kitchen ticket), prefer consolidating redundant printables
- **Streamlining pass (5 PRs, 2026-07-08)**: #219 remove Dexter hospital/clinic signs (Chelsea/UM/Concentra, PDFs delivered first); #220 (base #219) fold construction-site-sign into safety-signs as page one, portrait via named @page rule (`@page gpage { size: letter portrait }` inside @media print works in Chromium; verified 1 portrait + 15 landscape), stub at old URL; #221 (base #220) safety.html Resources split into Printables / OSHA References; #222 PPE checklist gains Van/Checked-by/Date header + "one sheet, three uses" line (master list + van check + restock order), safety.html PPE section collapsed to one when-required rules list keeping all citations; #223 genericize Buildertrend SDS line in training presentation. Merge order: #219 -> #220 -> #221; #222 and #223 independent
- **Redundancy map (safety printables)**: equipment checklist = THE inventory artifact; safety.html = rules + citations; reminders card = behavior rules van card; inspection form = walk record; confined-space permit = distinct. No further overlap worth killing

### Session: 2026-07-10 — Fold Safety Reminders card into the PPE checklist (PRs #224-227)
- **Context**: further redundancy cleanup after the streamlining pass; the standalone `safety-reminders.html` card and the PPE/equipment checklist covered overlapping ground
- **PR #225**: merged the Safety Reminders card content into `safety-equipment-checklist.html` rather than keeping it as a separate page
- **PR #226**: folded each reminder's behavioral context into its corresponding checklist line (rather than a separate callout block), so the checklist reads as one list with the "why" attached per item
- **PR #227**: dropped `safety-reminders.html` entirely once its content lived fully inside the checklist; removed its links from `safety.html` and `resources.html`
- **Result**: `safety-equipment-checklist.html` is now single-purpose "one sheet, three uses" (master list + van check + restock order) carrying the citations and the behavioral reminders together; no separate reminders card remains on the site
- **Branch**: `claude/land-fold-and-split` (PR #224 bundled the Resources split, see PR #221 note above)

### Session: 2026-07-31 — Split Tile out of Flooring; tile order sheets to one page
- **New page** `tile.html`: the Tile section was pulled out of `flooring.html` and built into a standalone page in the standard format (intro, Quick Reference, Prerequisites, Process, Best Practices, Inspections, Client Communication, Resources). All existing tile copy carried over verbatim; the Process sub-lists were reordered to job sequence (Planning, Shower Coordination, Substrate Prep, Flood Test, Install)
- **Red-flagged additions** (new content beyond what was already on the site, pending approval): two Best Practices bullets (dry-lay complex patterns; keep tile from one lot in one room) and one Client Communication bullet (flood test holds the shower for a day or more)
- **Flooring page**: now wood + LVP only; intro points to Tile, tile flood-test bullet in Quick Reference replaced with a transitions bullet, Resources point to the Tile page and keep the Wood/LVP sheet
- **Nav**: "Tile" added after "Flooring" in all 63 files that carry the sidebar. Both tile order sheets now show Tile as the active section and breadcrumb to `tile.html` instead of `flooring.html`
- **Order sheets, one page each**: both `tile-prep-order-sheet.html` and `tile-finish-order-sheet.html` printed to 2 pages; both now fit one letter portrait page with room to spare (prep ~9.6in of 10in printable, finish ~9.5in)
- **Print fixes applied to both** (matching the Westbury / window / exterior-door forms, which are the one-page reference standard): `.main-wrapper { display:block }`, `main { margin-left:0; min-height:0 }`, `article { box-shadow:none }`, `body { display:block; min-height:0 }`, `@page margin 0.5in`, tighter font sizes / line-heights / section margins
- **Prep sheet content fixes**: removed the duplicate Kerdi-band line (it was listed under both Floor Substrate and Shower Waterproofing), Shower Tray and Shower Niches promoted from bold paragraphs to real `h3.form-section` headings, job header labels un-bolded per form convention, size notes given a muted `.size-note` class, `.option-row`/`.opts` styles defined (they were being used but never declared), Stone Pieces trimmed 5 rows to 4
- **Finish sheet**: added the print-only copyright credit line it was missing, and an `.area-title` class so each Room / Location block reads as a section divider
- **Print-measurement technique (reusable)**: headless Chromium `--print-to-pdf` plus a page count, then binary-search `@page { size: 8.5in Xin }` to find the exact content height. Tells you how much slack a form has instead of just pass/fail
- **Branch**: `claude/tile-page-order-sheet-6rzeck`
