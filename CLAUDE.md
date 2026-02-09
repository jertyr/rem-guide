# Remodelers Guide - Project Memory

## Workflow Preferences
- **PRs**: Provide the PR link — the user will click it and handle the merge themselves
- **Pushes**: Same — just push and give the link, don't spend tokens retrying or automating merges
- **Memory**: Update this file each session with what was done so future sessions have context

## Project Overview
- Static HTML site hosted on GitHub Pages at `jertyr.github.io/rem-guide`
- Construction/remodeling reference guide with ~42 HTML pages
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
9. **Resources** — links to checklists, order forms, external guides
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
