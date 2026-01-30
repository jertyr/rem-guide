# Update Page Skill

Transform Remodelers Guide pages from the early format to a clean, professional format that serves contractors and experienced homeowners who need quick, reliable reference material.

## Audience

The target audience is contractors and homeowners undertaking construction projects. These are people who already operate at the 80th or 90th percentile—they know the work but want key reminders and guidance to achieve top-level performance. The content should instruct but be concise and to the point. This is not a teaching resource for beginners.

## Core Principles

### Preserve the Details

The information on each page has been accumulated through hard work and real-world experience. When reorganizing or rewriting:

- Do not lose fidelity or technical detail
- Keep all code references, specifications, and measurements
- Keep all photos and resource links
- Rewriting for clarity and conciseness is encouraged
- Reorganizing content within the page is fine if it makes sense

### Let Content Drive Structure

Early versions of this site tried to force every page into identical sections. That approach was too limiting—some topics need different sections, and forcing content into a rigid template created redundancy.

When updating a page:

- Use sections that fit the content, not the other way around
- Add sections when the material calls for it
- Omit sections that would be empty or redundant
- Combine or split sections as needed for clarity

### Write for Quick Reference

Contractors use these pages on the jobsite. The format should be scannable:

- Bullet points over paragraphs where practical
- Key information up front
- Technical specs easy to find
- No filler or redundant explanation

## HTML Cleanup

### Remove WordPress Artifacts

Early pages were exported from WordPress and contain unnecessary classes and inline styles. Remove:

- Classes like `wp-block-heading`, `wp-block-list`
- Custom CSS beyond print styles (submenu styles, sidebar styles, article list indentation)
- Redundant text in meta descriptions (e.g., "- The Remodelers Guide" suffix)

### Standard Print Styles

Replace any custom CSS with print-only styles:

```html
<style>
    @media print {
        header,
        .sidebar,
        footer,
        .mobile-nav-toggle,
        .breadcrumb,
        button,
        #share-btn,
        #print-btn,
        small {
            display: none !important;
        }
        .main-wrapper {
            margin: 0 !important;
            padding: 0 !important;
        }
        main {
            margin: 0 !important;
            padding: 0 !important;
        }
        article {
            margin: 0 !important;
            padding: 2rem !important;
        }
        body {
            margin: 0;
            padding: 0;
        }
        li {
            page-break-inside: avoid;
            margin-bottom: 0.5em;
        }
        ul {
            list-style-type: none !important;
        }
    }
</style>
```

### Clean Formatting

- Use proper HTML indentation (4 spaces per level)
- Break long content lines into readable chunks
- Use `<figure><img /></figure>` for photos
- Include the footer element

## Common Section Patterns

These sections appear frequently but are not required. Use what fits the content.

**Quick Reference** - Key reminders at the top for people who know the work but want a checklist before starting.

**Materials and Tools** - What to have on hand. Can use nested lists for categories.

**Process** - Workflow steps. Nested `<ul>` lists work well for grouping by phase. Numbered lists are fine when sequence matters.

**Best Practices** - Tips that prevent common mistakes. Positive framing works better than "issues to avoid."

**Inspections** - What inspectors look for, permit requirements, documentation needed.

**Client Communication** - What to discuss with clients, expectations to set, advice to give.

**Resources** - Links to checklists, order forms, code references, external guides.

## Section Naming

When a page uses inconsistent or unclear section names, consider these alternatives:

| Instead of | Consider |
|------------|----------|
| TLDR | Quick Reference |
| Materials / Tools Needed | Materials and Tools |
| Common Issues & How to Avoid Them | Best Practices |
| Client Interaction / Communication Notes | Client Communication |
| Inspections & Documentation | Inspections |

But don't rename sections mechanically. Use whatever name is clearest for the content.

## Page Footer

Every page should end with share/print buttons and a revision date:

```html
<p>
    <button id="share-btn" type="button" style="padding:0.4rem 0.9rem; border-radius:4px; border:1px solid #ccc; background:#f5f5f5; cursor:pointer;">Share</button>
    <button id="print-btn" type="button" style="padding:0.4rem 0.9rem; border-radius:4px; border:1px solid #ccc; background:#f5f5f5; cursor:pointer; margin-left:0.5rem;">Print</button>
    <br>
    <small>Last revised: MM/DD/YYYY</small>
</p>
```

And a site footer:

```html
<footer>
    &copy; 2025 Remodelers Guide. All rights reserved. | Contact: <a href="mailto:geraldatyrrell@gmail.com">geraldatyrrell@gmail.com</a>
</footer>
