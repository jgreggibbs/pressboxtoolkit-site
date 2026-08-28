# Footer Social Icons Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the footer's text LinkedIn/email line with an icon row (LinkedIn, Instagram, Facebook, email) on all three pages and correct the LinkedIn URL everywhere.

**Architecture:** Static HTML site, no build step. Inline SVG icons with `fill="currentColor"` inside anchor tags; one small CSS block styles the row. Metadata (schema.org `sameAs`, llms.txt) updated to match.

**Tech Stack:** Plain HTML/CSS. No test framework exists; verification is via browser preview.

## Global Constraints

- LinkedIn URL: `https://www.linkedin.com/company/press-box-toolkit/` (the old `press-box-toolkit-llc` slug must not survive anywhere)
- Instagram URL: `https://www.instagram.com/pressboxtoolkit/`
- Facebook URL: `https://www.facebook.com/profile.php?id=61593398079838`
- Email: `mailto:info@pressboxtoolkit.com`
- Social links: `target="_blank" rel="noopener"`. Mailto link: no target.
- SVGs: `viewBox="0 0 24 24"`, `fill="currentColor"`, `aria-hidden="true"`; accessible name lives on the `<a>` via `aria-label`.
- Do not touch the `.links` nav row or the copyright line (other than the line being replaced sits between them).

---

### Task 1: Icon row markup + CSS on all three pages

**Files:**
- Modify: `styles.css` (after the `.foot .small` rule, ~line 338)
- Modify: `index.html:146`
- Modify: `details.html:202`
- Modify: `404.html:46`

**Interfaces:**
- Produces: `.foot .social` markup pattern used identically on all three pages.

- [ ] **Step 1: Add CSS**

In `styles.css`, directly after the line `.foot .small{color:var(--mute);font-size:14px}`, add:

```css
.foot .social{display:flex;gap:20px;justify-content:center;margin:2px 0 4px}
.foot .social a{color:var(--orange-lt);display:inline-flex}
.foot .social a:hover{color:#fff}
.foot .social svg{width:22px;height:22px}
```

- [ ] **Step 2: Replace the footer line on each page**

In each of `index.html`, `details.html`, and `404.html`, replace this exact line:

```html
    <p class="small"><a href="https://www.linkedin.com/company/press-box-toolkit-llc">LinkedIn</a> &nbsp;·&nbsp; info@pressboxtoolkit.com</p>
```

with:

```html
    <div class="social">
      <a href="https://www.linkedin.com/company/press-box-toolkit/" target="_blank" rel="noopener" aria-label="Press Box Toolkit on LinkedIn"><svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 1 1 0-4.124 2.062 2.062 0 0 1 0 4.124zM7.119 20.452H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg></a>
      <a href="https://www.instagram.com/pressboxtoolkit/" target="_blank" rel="noopener" aria-label="Press Box Toolkit on Instagram"><svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838a6.162 6.162 0 1 0 0 12.324 6.162 6.162 0 0 0 0-12.324zM12 16a4 4 0 1 1 0-8 4 4 0 0 1 0 8zm6.406-11.845a1.44 1.44 0 1 0 0 2.881 1.44 1.44 0 0 0 0-2.881z"/></svg></a>
      <a href="https://www.facebook.com/profile.php?id=61593398079838" target="_blank" rel="noopener" aria-label="Press Box Toolkit on Facebook"><svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"><path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/></svg></a>
      <a href="mailto:info@pressboxtoolkit.com" aria-label="Email Press Box Toolkit"><svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"><path d="M12 12.713L.015 4.5h23.97L12 12.713zM12 15.13L0 6.916V19.5h24V6.916L12 15.13z"/></svg></a>
    </div>
```

The surrounding lines (the `.links` div above, the copyright `<p class="small" style="margin-top:10px">` below) stay unchanged.

- [ ] **Step 3: Verify no old-slug references remain in the three HTML footers**

Run (Grep tool or):

```bash
grep -rn "press-box-toolkit-llc" --include="*.html" .
```

Expected: only the `index.html` schema.org `sameAs` line remains (handled in Task 2).

- [ ] **Step 4: Commit**

```bash
git add styles.css index.html details.html 404.html
git commit -m "Replace footer text links with social media icon row"
```

---

### Task 2: Metadata updates (schema.org sameAs + llms.txt)

**Files:**
- Modify: `index.html:32`
- Modify: `llms.txt:26`

**Interfaces:**
- Consumes: URLs from Global Constraints. No code interfaces.

- [ ] **Step 1: Update schema.org sameAs in index.html**

Replace:

```json
      "sameAs": ["https://www.linkedin.com/company/press-box-toolkit-llc"]
```

with:

```json
      "sameAs": [
        "https://www.linkedin.com/company/press-box-toolkit/",
        "https://www.instagram.com/pressboxtoolkit/",
        "https://www.facebook.com/profile.php?id=61593398079838"
      ]
```

- [ ] **Step 2: Update llms.txt Contact section**

Replace:

```
- LinkedIn: https://www.linkedin.com/company/press-box-toolkit-llc
```

with:

```
- LinkedIn: https://www.linkedin.com/company/press-box-toolkit/
- Instagram: https://www.instagram.com/pressboxtoolkit/
- Facebook: https://www.facebook.com/profile.php?id=61593398079838
```

- [ ] **Step 3: Verify old slug is fully gone**

```bash
grep -rn "press-box-toolkit-llc" .
```

Expected: no matches (spec files in docs/ may mention it descriptively; that is acceptable — check only site files: *.html, *.css, llms.txt).

- [ ] **Step 4: Validate the JSON-LD still parses**

Extract the `<script type="application/ld+json">` block content and run it through a JSON parser (e.g. paste into `python -m json.tool` or use the browser console). Expected: parses without error.

- [ ] **Step 5: Commit**

```bash
git add index.html llms.txt
git commit -m "Update social URLs in schema.org and llms.txt"
```

---

### Task 3: Browser verification

**Files:** none modified (verification only; fix-and-recheck if issues found)

**Interfaces:**
- Consumes: the deployed markup/CSS from Tasks 1-2.

- [ ] **Step 1: Open the site in the browser preview**

Serve the site root (any static server) and open `index.html`.

- [ ] **Step 2: Check the footer icon row**

Scroll to the footer. Confirm: four icons render centered, 22px, orange (`--orange-lt`); hover turns an icon white; each anchor's href matches the Global Constraints URLs (verify via read_page/accessibility tree, which also confirms the aria-labels read "Press Box Toolkit on LinkedIn" etc.).

- [ ] **Step 3: Repeat the footer check on details.html and 404.html**

Navigate to each page; confirm the identical icon row renders.

- [ ] **Step 4: Mobile width check**

Resize to 375px wide. Expected: the icon row stays on one line, centered, no horizontal overflow.

- [ ] **Step 5: Screenshot proof**

Take a footer screenshot for the user.
