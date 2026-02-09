# Remodelers Guide - Project Memory

## Project Overview
- Static HTML site hosted on GitHub Pages at `jertyr.github.io/rem-guide`
- Construction/remodeling reference guide with ~40+ HTML pages
- Single shared `styles.css` for all styling
- No build system or framework — plain HTML, CSS, and vanilla JS

## Architecture
- **Layout**: Fixed header + fixed sidebar nav + scrollable main content
- **Sidebar**: 280px wide, lists all pages as nav links. Each HTML file has its own copy of the full nav (no templating)
- **Mobile**: Sidebar hidden by default, slides in from left via hamburger button (`.mobile-nav-toggle`). Toggled with `.active` class. Backdrop overlay via `::before` pseudo-element
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

## Work Log

### Session: 2025-02-09 — Fix mobile nav styling
- **Issue**: Mobile sidebar had a gray (`#F8F6F3`) background that looked dull/wrong
- **Fix**: Added `background: #FFFFFF` to `.sidebar` inside `@media (max-width: 768px)` block in `styles.css`
- **Result**: Clean white sidebar on mobile; active page highlight (light blue + orange left border) stands out clearly
- **Branch**: `claude/fix-mobile-nav-styling-gR5zk`
