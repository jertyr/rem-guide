# Remodelers Guide - Project Memory

## Workflow Preferences
- **PRs**: Provide the PR link — the user will click it and handle the merge themselves
- **Pushes**: Same — just push and give the link, don't spend tokens retrying or automating merges
- **Memory**: Update this file each session with what was done so future sessions have context

## Project Overview
- Static HTML site hosted on GitHub Pages at `jertyr.github.io/rem-guide`
- Construction/remodeling reference guide with ~40+ HTML pages
- Single shared `styles.css` for all styling
- No build system or framework — plain HTML, CSS, and vanilla JS

## Architecture
- **Layout**: Fixed header + fixed sidebar nav + scrollable main content
- **Sidebar**: 280px wide, lists all pages as nav links. Each HTML file has its own copy of the full nav (no templating)
- **Mobile**: Sidebar hidden by default, slides in from left via hamburger button (`.mobile-nav-toggle`). Toggled with `.active` class. Backdrop overlay via large `box-shadow` spread on `.sidebar.active`
- **Breakpoint**: 768px for mobile layout
- **Active page**: Indicated by `.active` class on the nav link and `.active-section` on the parent `<li>` (note: `.active-section` has no CSS rule — only `.active` on the `<a>` is styled)
- **JS**: Each HTML file has identical inline `<script>` for mobile nav toggle (no shared JS file)

## Key Files
- `styles.css` — All site styling including responsive/mobile rules
- `about.html` — Home page
- `index.html` — Entry point (redirects or mirrors home)
- `update-page.md` — Documentation for updating pages

## CSS Variables (from `:root`)
- `--primary-color: #2C3E50` (deep slate blue)
- `--secondary-color: #E67E22` (burnt orange, used for CTAs/highlights)
- `--sidebar-bg: #F8F6F3` (warm off-white, desktop only)
- `--active-bg: #E8F4F8` (light blue for active nav item)
- `--header-height: 58px`
- `--sidebar-width: 280px`

## Lessons Learned
- **`transform` breaks `position: fixed` children**: The sidebar uses `transform: translateX()` for the slide-in animation. This makes the sidebar a containing block for any `position: fixed` descendants (like `::before`), so they position relative to the sidebar instead of the viewport. This caused a dark overlay to appear over part of the nav items. Fix: use `box-shadow: 0 0 0 9999px rgba(0,0,0,0.5)` instead of a `::before` pseudo-element for the backdrop.

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
