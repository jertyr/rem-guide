# Remodelers Guide - Active Navigation Version

## ✅ NEW: Active Navigation Highlighting

This version includes smart navigation that:

1. **Highlights the current page** - The link for the page you're on is highlighted in green
2. **Keeps sections expanded** - If you're on "Site Setup" or any of its sub-pages, the Site Setup dropdown stays open
3. **Shows where you are** - Both main pages and sub-pages get highlighted when active

### Example:
- On "Site Setup" page → "Site Setup" link is highlighted, dropdown is visible
- On "Site Set Up Checklist" → "Site Setup" section is expanded, "Site Set Up Checklist" is highlighted

## ✅ All Previous Fixes Included

- ✓ Corrected image paths (images/filename.jpg)
- ✓ Email: geraldatyrrell@gmail.com
- ✓ Dexter Builders removed
- ✓ Sidebar navigation with dropdowns

## Upload to S3

1. Extract this ZIP
2. Upload all HTML files to your S3 bucket root
3. Upload styles.css to bucket root
4. Make sure your images are in images/ folder

## How It Works

The navigation uses CSS classes:
- `active` class = current page link (highlighted)
- `active-section` class = current section (dropdown stays open)

No JavaScript needed for the highlighting - it's built into each page's HTML!

## Contact

Email: geraldatyrrell@gmail.com
Built by Jerry Tyrrell
