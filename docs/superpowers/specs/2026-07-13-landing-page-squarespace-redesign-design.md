# Landing Page Redesign: Match Former Squarespace Site

**Date:** 2026-07-13
**Status:** Approved pending user review

## Goal

Rebuild the landing page (index.html) of pressboxtoolkit.com to visually match the
former Squarespace site (dory-turquoise-ykl3.squarespace.com), which the user prefers.
The site is static HTML/CSS hosted on GitHub Pages with a custom domain.

## Design tokens (from the Squarespace site)

- Background: light turquoise `#CCE6EA`
- Text: near-black `#020617`
- Headline font: **Anton** (Google Fonts), all-caps, centered
- Body font: **Epilogue** (Google Fonts)
- Accent: orange `#F97415`; buttons are fully-rounded pills with white text
- Logo: black-and-orange version (`PBT-logo-black.png`) in nav and footer
- Header: light, transparent-feeling, not dark sticky

These tokens live in `styles.css`, which `details.html` shares. The details page is
not being restructured, but it inherits the light theme; a legibility pass is in scope.

## Landing page structure (top to bottom, mirroring the old site)

1. **Three tall statement sections**, each with a full-bleed animated background and
   a centered Anton headline:
   - "We're building the tools to streamline game day planning & operations"
   - "An intuitive platform to manage rosters, sponsors, audio, and more"
   - "Every game executed flawlessly"
2. **Three-column links section:** Tell Me More (Details page) / Try It Out (demo) /
   Contact Us (anchor), same copy as the old site.
3. **Contact section** with a real form: First Name, Last Name, Email, Message, orange
   pill Send button. Posts to Formspree. Wired with a placeholder form ID until the
   user creates the Formspree account; activation is a one-line change.
4. **Minimal footer:** email + LinkedIn, like the old site.

Removed from the current page: dark hero with logo, the four "What it does" cards
(that content lives on Details).

## Animated backgrounds (replacing the old stock videos)

The old site used three licensed stock MP4s the user can no longer publish. We
recreate each scene as an original, stylized CSS/SVG/canvas animation - same mood
and motion, clearly graphic-art rather than photoreal. No video files.

1. **Stadium night** (section 1): cool blue blurred-stadium feel - floodlight glows,
   bokeh crowd dots, particles drifting like snow.
2. **Soccer goal** (section 2): warm red/orange blurred-crowd backdrop, stylized goal
   net with a ball hitting it.
3. **Basketball swish** (section 3): purple-blue arena glow with floodlights, stylized
   hoop with a ball dropping through on a loop.

Constraints:
- Loop seamlessly; subtle enough that the Anton headlines stay dominant and legible.
- Respect `prefers-reduced-motion`: animations pause/settle to a static composition.
- Text over these backgrounds may need a light scrim for contrast.

## Out of scope

- Restructuring details.html (legibility pass only)
- Downloading or reusing any Squarespace-hosted asset other than referencing the
  already-in-repo logos
- Form backend beyond Formspree wiring

## Error handling / fallbacks

- If canvas/JS is unavailable, sections fall back to their base gradient backgrounds.
- Form: native HTML validation (required fields, email type); Formspree handles the rest.

## Testing

- Visual check in the browser pane at desktop and mobile widths.
- Verify details.html is legible after the theme flip.
- Verify reduced-motion behavior via emulation.
- Form posts to Formspree endpoint (placeholder ID until user provides one).
