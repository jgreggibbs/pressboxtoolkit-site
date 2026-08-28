# Footer Social Icons — Design

Date: 2026-08-27
Status: Approved (pending final spec review)

## Goal

Replace the plain-text `LinkedIn · info@pressboxtoolkit.com` line in the site footers with a row of icon links for LinkedIn, Instagram, Facebook, and email. Correct the outdated LinkedIn company URL everywhere it appears.

## Scope

Files changed:

- `index.html` — footer icon row; schema.org `sameAs` update
- `details.html` — footer icon row
- `404.html` — footer icon row
- `styles.css` — one small `.foot .social` rule block
- `llms.txt` — corrected LinkedIn URL; add Instagram and Facebook

Out of scope: header/nav changes (footer only, per decision), any visual redesign of the footer beyond the icon row.

## URLs

- LinkedIn: `https://www.linkedin.com/company/press-box-toolkit/` (replaces the old `press-box-toolkit-llc` slug in all files)
- Instagram: `https://www.instagram.com/pressboxtoolkit/`
- Facebook: `https://www.facebook.com/profile.php?id=61593398079838` (numeric ID; page is new and has no vanity username yet. The ID URL remains valid permanently, so a future username claim is a cosmetic swap, not a breakage risk.)
- Email: `mailto:info@pressboxtoolkit.com`

## Design

### Icon row (all three footers)

The `<p class="small">` line containing the LinkedIn text link and email address is replaced with:

```html
<div class="social">
  <a href="https://www.linkedin.com/company/press-box-toolkit/" target="_blank" rel="noopener" aria-label="Press Box Toolkit on LinkedIn">[LinkedIn SVG]</a>
  <a href="https://www.instagram.com/pressboxtoolkit/" target="_blank" rel="noopener" aria-label="Press Box Toolkit on Instagram">[Instagram SVG]</a>
  <a href="https://www.facebook.com/profile.php?id=61593398079838" target="_blank" rel="noopener" aria-label="Press Box Toolkit on Facebook">[Facebook SVG]</a>
  <a href="mailto:info@pressboxtoolkit.com" aria-label="Email Press Box Toolkit">[envelope SVG]</a>
</div>
```

- Icons are inline SVGs, standard brand glyph shapes, `viewBox="0 0 24 24"`, `fill="currentColor"`, rendered at 22px, with `aria-hidden="true"` on the SVG element (the link's `aria-label` carries the accessible name).
- Social links open in a new tab with `rel="noopener"`; the mailto link does not use `target="_blank"`.
- The `© 2026 Press Box Toolkit LLC` line stays below the icon row.
- The footer's existing "Contact" text link (mailto) is unchanged, so the email address remains reachable as text.

### CSS (`styles.css`)

One rule block near the existing `.foot` styles:

```css
.foot .social{display:flex;gap:20px;justify-content:center;margin-bottom:4px}
.foot .social a{color:var(--orange-lt);display:inline-flex}
.foot .social a:hover{color:#fff}
.foot .social svg{width:22px;height:22px}
```

Hover color is `#fff` (existing footer links have no hover rule, so this is the icons' own treatment).

### Metadata updates

- `index.html` schema.org Organization `sameAs` becomes an array of all three social URLs.
- `llms.txt`: LinkedIn line updated to the new slug; Instagram and Facebook lines added in the same list.
- All other occurrences of `press-box-toolkit-llc` in LinkedIn URLs are replaced (footers in all three pages).

## Verification

- Open the site in the browser preview; confirm the icon row renders centered in the footer on all three pages, hover state works, and each link points to the right destination.
- Confirm mobile width (375px) does not wrap or overflow the row.
- Grep confirms no remaining `press-box-toolkit-llc` references.
