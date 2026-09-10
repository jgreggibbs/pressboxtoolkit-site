# User Map Section Design

Date: 2026-09-04

## Goal

Add a non-interactive map of where Press Box Toolkit users are located to the home page as a social-proof section.

## Source

`H:\My Drive\Gibbs Technology Group Apps\0_PressBoxToolkit\pbt-user-map.html` is a standalone page containing an inline SVG US map (51 state paths shaded into four buckets by user count, 46 city dots sized by user count, hover titles on each), a swatch legend, a ranked bar list of 24 states, and a footnote. It has no scripts. Fonts and orange differ from the site.

## Placement

New section on `index.html` between the "What it does" section and the Contact section.

## Section content

- Eyebrow: "Where we are"
- Heading: "Press boxes in 24 states"
- Lede: one sentence along the lines of "From Friday night football in Georgia to hoops in Oregon, crews across the country run game day on Press Box Toolkit."
- Map in a floating panel that matches the existing `.wrap.panel` style.
- Caption under the map: "Each dot is a city with Press Box Toolkit users. Brighter orange states have more."
- Not included: swatch legend, state bar list, account total, footnote.

## Map asset

- Saved as `images/user-map.svg`, loaded with an `img` tag with `loading="lazy"` and descriptive alt text.
- Paths and dot positions are copied exactly from the source. Hover `title` elements are removed.
- Colors are baked into the SVG (external SVGs cannot read page CSS variables):
  - Dots: site orange `#F26A1B` with a dark stroke matching the panel background.
  - State fills: four shades from neutral dark to warm orange-brown, adjusted to read against the panel background `rgba(18,23,40,.94)`.
  - State borders: a subtle hairline.
- `viewBox` and `role="img"` are kept. The root `aria-label` is dropped in favor of the `img` alt.

## Styling

- Section classes: `section alt photo-bg photo-users-home`.
- Background: the currently unused `images/turf4.webp` via a new `--photo-users-home` variable, with darkening gradients matching the neighboring sections, applied only once `.bg-ready` is set by the existing lazy-load script.
- The CSS comment marking turf4 as spare is updated.
- Map panel padding is tighter than the contact panel so the map fills the width. Caption is small, muted, centered.
- Responsive: panel and padding follow the existing 780px breakpoint rules.

## Files touched

- `index.html`: new section.
- `styles.css`: new photo variable, new `.photo-users-home.bg-ready` rule, map panel and caption styles.
- `images/user-map.svg`: new.

## Not changed

Nav, footer, details page, sitemap, schema.

## Staleness

The map is a snapshot. The Drive file is the source of truth. To refresh: regenerate the Drive file, re-run the extraction (strip titles, recolor), replace the SVG, and update the state count in the heading by hand.

## Verification

- Open the home page in the browser pane at desktop and mobile widths.
- Confirm the SVG renders, dots are site orange, state shading is visible against the panel, and no console errors.
- Confirm the turf4 background loads when the section scrolls into view.
- Screenshot for the record.
