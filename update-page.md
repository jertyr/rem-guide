# Update Page Skill

This skill describes how to redesign a Remodelers Guide page from the early format to the polished, consistent format used across the site.

## Goals

- Create a consistent, professional look across all pages
- Make content scannable and easy to use on the jobsite
- Remove redundant information while preserving all valuable technical content
- Ensure pages print well for field use
- Maintain all photos, resources, and links

## Identifying Pages That Need Updates

Pages needing updates typically have these characteristics:

- WordPress classes like `wp-block-heading`, `wp-block-list`
- Custom CSS beyond print styles (submenu styles, sidebar navigation styles, article list styles)
- Section names like "TLDR" instead of "Quick Reference"
- Numbered lists (`<ol>`) for the Process section instead of nested `<ul>`
- Content compressed into single long lines
- Missing `<footer>` element
- Meta description containing redundant text like "- The Remodelers Guide"
- Inconsistent section ordering

## Standard Page Structure

Every polished page follows this structure:

### 1. Head Section

```html
<meta name="description" content="Page Title | Remodelers Guide">
<title>Page Title | Remodelers Guide</title>
<link rel="stylesheet" href="styles.css">
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

The `<style>` block should ONLY contain print styles. Remove any custom CSS for submenus, sidebars, or article list formatting.

### 2. Content Sections (in order)

1. **Title** (`<h1>`) - Page name only
2. **Intro Paragraph** - One or two sentences explaining the topic scope
3. **Quick Reference** (`<h2>`) - Bullet list of key reminders and critical points
4. **Materials and Tools** (`<h2>`) - Flat list with optional nested sub-items for categories
5. **Process** (`<h2>`) - Nested `<ul>` structure with parent categories and child steps
6. **Best Practices** (`<h2>`) - Short tips list (optional, when applicable)
7. **Inspections** (`<h2>`) - Inspection requirements and what inspectors look for
8. **Client Communication** (`<h2>`) - What to tell clients, expectations to set
9. **Photo Examples** - `<figure>` elements with images (keep all existing photos)
10. **Resources** (`<h2>`) - Links to checklists, order forms, and external references

Not every page needs every section. Use what's appropriate for the content.

### 3. Process Section Format

The Process section uses nested unordered lists, not numbered lists. Organize by workflow phase with sub-steps:

```html
<h2>Process</h2>

<ul>
    <li>Phase Name
        <ul>
            <li>First step in this phase</li>
            <li>Second step with sub-details:
                <ul>
                    <li>Sub-detail one</li>
                    <li>Sub-detail two</li>
                </ul>
            </li>
        </ul>
    </li>
    <li>Next Phase Name
        <ul>
            <li>Steps for this phase</li>
        </ul>
    </li>
</ul>
```

### 4. Footer Elements

Every page ends with:

```html
<p>
    <button id="share-btn" type="button" style="padding:0.4rem 0.9rem; border-radius:4px; border:1px solid #ccc; background:#f5f5f5; cursor:pointer;">Share</button>
    <button id="print-btn" type="button" style="padding:0.4rem 0.9rem; border-radius:4px; border:1px solid #ccc; background:#f5f5f5; cursor:pointer; margin-left:0.5rem;">Print</button>
    <br>
    <small>Last revised: MM/DD/YYYY</small>
</p>

<script>
    // Share button
    const shareBtn = document.getElementById('share-btn');
    if (shareBtn && navigator.share) {
        shareBtn.addEventListener('click', () => {
            navigator.share({
                title: document.title || 'Page Title',
                url: window.location.href
            }).catch(() => {
                // user cancelled or share failed; do nothing
            });
        });
    } else if (shareBtn) {
        shareBtn.style.display = 'none';
    }
    // Print button
    const printBtn = document.getElementById('print-btn');
    if (printBtn) {
        printBtn.addEventListener('click', () => {
            window.print();
        });
    }
</script>
```

And the page footer:

```html
<footer>
    &copy; 2025 Remodelers Guide. All rights reserved. | Contact: <a href="mailto:geraldatyrrell@gmail.com">geraldatyrrell@gmail.com</a>
</footer>
```

## Transformation Steps

### Step 1: Clean the HTML

1. Remove all WordPress classes (`wp-block-heading`, `wp-block-list`, etc.)
2. Replace custom CSS in `<style>` with print-only styles
3. Fix the meta description (remove "- The Remodelers Guide" suffix)
4. Ensure proper HTML indentation (4 spaces per level)
5. Break long content lines into readable chunks

### Step 2: Restructure Sections

1. Rename "TLDR" to "Quick Reference"
2. Rename "Materials / Tools Needed" to "Materials and Tools"
3. Rename "Common Issues & How to Avoid Them" to "Best Practices" (reframe positively)
4. Rename "Client Interaction / Communication Notes" to "Client Communication"
5. Ensure sections appear in the standard order

### Step 3: Convert Process Section

Convert numbered `<ol>` lists to nested `<ul>` with phase groupings:

**Before:**
```html
<ol>
    <li><strong>Step One</strong> - Details about step one</li>
    <li><strong>Step Two</strong> - Details about step two</li>
</ol>
```

**After:**
```html
<ul>
    <li>Phase Name
        <ul>
            <li>Step one details</li>
            <li>Step two details</li>
        </ul>
    </li>
</ul>
```

### Step 4: Add Missing Elements

1. Add a brief intro paragraph after the `<h1>` if missing
2. Add `<footer>` element if missing
3. Update the "Last revised" date to today's date

### Step 5: Preserve Content

1. Keep all technical information, code references, and specifications
2. Keep all photos (convert to simple `<figure><img>` format if needed)
3. Keep all resource links
4. Keep all code section numbers (R905.1.2, IRC Table R602.3(1), etc.)

### Step 6: Verify Sidebar Navigation

Ensure the sidebar marks the current page as active:

```html
<li class="nav-item active-section"><a href="current-page.html" class="active">Current Page</a></li>
```

## Quality Checklist

Before committing changes, verify:

- [ ] No WordPress classes remain in the HTML
- [ ] Only print styles in the `<style>` block
- [ ] Meta description is clean (no redundant suffix)
- [ ] Brief intro paragraph exists
- [ ] Sections are in standard order
- [ ] Process section uses nested `<ul>` structure
- [ ] All photos preserved with `<figure>` tags
- [ ] All resource links preserved
- [ ] Footer element exists
- [ ] Last revised date updated
- [ ] HTML is properly indented
- [ ] Share/Print buttons and scripts present

## Section Naming Reference

| Old Name | New Name |
|----------|----------|
| TLDR | Quick Reference |
| Materials / Tools Needed | Materials and Tools |
| Materials or Tools Needed | Materials and Tools |
| Materials / Tools Checklist | Materials and Tools |
| Prerequisites | (merge into Process > Preparation) |
| Common Issues & How to Avoid Them | Best Practices |
| Client Interaction / Communication Notes | Client Communication |
| Client Communication & Warranty | Client Communication |
| Inspections & Documentation | Inspections |
| Photo Examples | (no heading, just `<figure>` elements before Resources) |

## Example Transformation

See the following pages for reference implementations:

- Windows (windows.html)
- Exterior Doors (exterior-doors.html)
- Roofing (roofing.html)
- Framing (framing.html)
- Foundations (foundations.html)
- Demolition (demolition.html)
