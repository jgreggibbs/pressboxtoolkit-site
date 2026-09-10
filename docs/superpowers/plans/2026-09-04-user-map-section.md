# User Map Section Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a non-interactive US map of Press Box Toolkit user locations to the home page as a social-proof section.

**Architecture:** The map is extracted from the standalone Drive page into a static `images/user-map.svg` with the site's colors baked in and hover titles removed. A new photo-backed section on `index.html` shows it inside a floating panel, styled by a handful of new rules in `styles.css`. No JavaScript changes.

**Tech Stack:** Static HTML/CSS site, Git Bash with perl for the one-off extraction. No build step, no test framework. Verification is done in the browser pane.

## Global Constraints

- Site orange is `#F26A1B` (`--orange` in `styles.css`). Do not use the source file's `#f97316`.
- Panel background is `rgba(18,23,40,.94)` (`--panel`). SVG colors must read against it.
- Source file: `H:\My Drive\Gibbs Technology Group Apps\0_PressBoxToolkit\pbt-user-map.html`. Do not modify it.
- State paths and dot positions are copied exactly. No geometry changes.
- No em dashes in any copy.
- Section goes between the "What it does" section and the Contact section on `index.html` only.
- Photo background is `images/turf4.webp`, currently unused.

---

### Task 1: Extract the map into `images/user-map.svg`

**Files:**
- Create: `images/user-map.svg`

**Interfaces:**
- Produces: `images/user-map.svg`, a self-contained SVG with `viewBox="16.1 -1.1 1030.5 611.3"`, 51 state paths, 46 city circles, no `<title>` elements, no CSS variable references.

- [ ] **Step 1: Confirm the source structure**

Run:
```bash
f="H:/My Drive/Gibbs Technology Group Apps/0_PressBoxToolkit/pbt-user-map.html"; sed -n '59p' "$f" | grep -o '<path' | wc -l; sed -n '59p' "$f" | grep -o '<circle' | wc -l; sed -n '59p' "$f" | grep -o '<title' | wc -l
```
Expected output (one number per line):
```
51
46
97
```
If the counts differ, the Drive file has been regenerated. Stop and re-check the line number holding the `<svg` element before continuing.

- [ ] **Step 2: Write the extraction script to the scratchpad**

Create `extract-map.sh` in the session scratchpad directory with this content:

```bash
#!/usr/bin/env bash
set -euo pipefail
src="H:/My Drive/Gibbs Technology Group Apps/0_PressBoxToolkit/pbt-user-map.html"
out="C:/projects/pressboxtoolkit-site/images/user-map.svg"

# Pull the <svg ...>...</svg> element off the single long line that holds it.
svg=$(grep -o '<svg.*</svg>' "$src")

printf '%s\n' "$svg" | perl -pe '
  # Drop hover tooltips.
  s{<title>[^<]*</title>}{}g;
  # Drop the aria-label and role; the <img> alt carries the description.
  s{ role="img" aria-label="[^"]*"}{};
  # Bake state fills. Shades read against the panel background rgba(18,23,40,.94).
  s{fill="var\(--land\)"}{fill="#1c2236"}g;
  s{fill="var\(--land-1\)"}{fill="#3d2f27"}g;
  s{fill="var\(--land-2\)"}{fill="#63391f"}g;
  s{fill="var\(--land-3\)"}{fill="#9a4b17"}g;
  # Bake the state hairline in place of class="st".
  s{class="st"}{stroke="#3a4058" stroke-width="0.9" stroke-linejoin="round"}g;
  # Bake the dot style in place of class="dot".
  s{class="dot"}{fill="#F26A1B" stroke="#121728" stroke-width="1.3"}g;
' > "$out"

echo "wrote $out ($(wc -c < "$out") bytes)"
```

- [ ] **Step 3: Run the script**

Run:
```bash
bash "C:/Users/jgreg/AppData/Local/Temp/claude/C--projects-pressboxtoolkit-site/ff0a1604-80d9-4e02-a933-a6b1c29bf32d/scratchpad/extract-map.sh"
```
Expected: `wrote C:/projects/pressboxtoolkit-site/images/user-map.svg (NNNNN bytes)` with a size in the 40 to 50 KB range.

- [ ] **Step 4: Verify the output**

Run:
```bash
cd C:/projects/pressboxtoolkit-site && grep -o '<path' images/user-map.svg | wc -l; grep -o '<circle' images/user-map.svg | wc -l; grep -c '<title' images/user-map.svg; grep -c 'var(--' images/user-map.svg; grep -c 'class=' images/user-map.svg; head -c 120 images/user-map.svg
```
Expected:
```
51
46
0
0
0
<svg viewBox="16.1 -1.1 1030.5 611.3" xmlns="http://www.w3.org/2000/svg"><g class="states">...
```
Note: `grep -c` on a single-line file prints `0` or `1`. All three must print `0`. The `<g class="states">` wrapper is the one `class=` allowed; if the count prints `1`, run `grep -o 'class="[^"]*"' images/user-map.svg` and confirm the only match is `class="states"`. Then it is fine.

- [ ] **Step 5: Render it in the browser pane to eyeball colors**

Open the raw file in the Browser pane: `preview_start` with `{url: "file:///C:/projects/pressboxtoolkit-site/images/user-map.svg"}`, then take a screenshot. Expected: a US map with dark blue-gray states, warmer brown-orange states where users are (Georgia darkest), and orange dots. If the browser refuses `file://`, skip this step; Task 2 renders it on the page.

- [ ] **Step 6: Commit**

```bash
cd C:/projects/pressboxtoolkit-site && git add images/user-map.svg && git commit -m "Add static user-location map SVG

Extracted from the Drive user map page with hover titles removed and
site colors baked in.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>"
```

---

### Task 2: Add the home page section and styles

**Files:**
- Modify: `index.html:126-128` (insert new section between the "What it does" section's closing tag and the Contact comment)
- Modify: `styles.css:24-36` (photo variables block), `styles.css:155-162` (after the contact photo rule), `styles.css:221-225` (after `.wrap.panel`)

**Interfaces:**
- Consumes: `images/user-map.svg` from Task 1.
- Produces: section classes `photo-users-home`, `map-panel`, `map-caption`.

- [ ] **Step 1: Add the photo variable**

In `styles.css`, replace these two lines:
```css
  --photo-contact-home:  url('images/empty-seats.webp'); /* index: contact */
  /* spare, currently unused: images/turf4.webp */
```
with:
```css
  --photo-users-home:    url('images/turf4.webp');       /* index: where we are (user map) */
  --photo-contact-home:  url('images/empty-seats.webp'); /* index: contact */
```

- [ ] **Step 2: Add the background rule**

In `styles.css`, directly after the `.photo-contact-home.bg-ready{...}` block (ends at the line `background-position:center 65%;` followed by `}`), insert:
```css
/* index: where we are — turf close-up */
.photo-users-home.bg-ready{
  background-image:
    linear-gradient(180deg,rgba(var(--shade),.82),rgba(var(--shade),.6) 45%,rgba(var(--shade),.8)),
    linear-gradient(rgba(var(--shade),.45),rgba(var(--shade),.45)),
    var(--photo-users-home);
  background-position:center;
}
```

- [ ] **Step 3: Add the map panel and caption styles**

In `styles.css`, directly after the `.wrap.panel{...}` block (ends with `max-width:1032px; /* aligns with cards inside a .wrap */` then `}`), insert:
```css
/* user map (home): tighter panel so the map fills the width */
.wrap.panel.map-panel{padding:28px 28px 20px}
.map-panel img{width:100%;height:auto;display:block}
.map-caption{
  text-align:center;color:var(--mute);font-size:.85rem;
  margin-top:16px;
}
```

Then in the `@media(max-width:780px)` block, directly after the line `.wrap.panel{width:calc(100% - 48px);padding:32px 20px}`, insert:
```css
  .wrap.panel.map-panel{padding:16px 12px 14px}
```

- [ ] **Step 4: Add the section markup**

In `index.html`, find:
```html
    </div>
  </div>
</section>

<!-- CONTACT -->
```
and replace with:
```html
    </div>
  </div>
</section>

<!-- WHERE WE ARE (user map) -->
<section class="section alt photo-bg photo-users-home" id="where">
  <div class="wrap">
    <p class="eyebrow">Where we are</p>
    <h2>Press boxes in 24 states</h2>
    <p class="lede">From Friday night football in Georgia to hoops in Oregon, crews across the country run game day on Press Box Toolkit.</p>
  </div>
  <div class="wrap panel map-panel">
    <img src="images/user-map.svg" width="1031" height="612" loading="lazy" alt="Map of the United States with orange dots marking cities where Press Box Toolkit is used, concentrated in the Southeast and Midwest">
    <p class="map-caption">Each dot is a city with Press Box Toolkit users. Brighter orange states have more.</p>
  </div>
</section>

<!-- CONTACT -->
```

Note: the `.wrap.panel` rule uses `width:calc(100% - 48px)` and `max-width:1032px`, so the panel centers itself under the heading `.wrap` at the same effective width the contact panel uses.

- [ ] **Step 5: Verify in the browser pane**

Start the site: `preview_start` with `{name: "site"}`. If `.claude/launch.json` does not exist, create it first:
```json
{
  "version": "0.0.1",
  "configurations": [
    {
      "name": "site",
      "runtimeExecutable": "python",
      "runtimeArgs": ["-m", "http.server", "8765"],
      "port": 8765
    }
  ]
}
```
If `python` is not on PATH, use `npx` with `["--yes", "serve", "-l", "8765", "."]` instead.

Then:
1. `navigate` to `http://localhost:8765/index.html`.
2. `read_console_messages` with `onlyErrors: true`. Expected: no errors.
3. `read_network_requests` with `urlPattern: "user-map.svg"`. Expected: one request with status 200 after the section scrolls into view (use `find` for "Press boxes in 24 states" and `scroll_to` its ref).
4. `read_network_requests` with `urlPattern: "turf4"`. Expected: status 200.
5. `javascript_tool`: `getComputedStyle(document.querySelector('.photo-users-home')).backgroundImage.includes('turf4')` Expected: `true`.
6. Screenshot the section at desktop width.
7. `resize_window` with `preset: "mobile"`, reload, scroll to the section, screenshot. Expected: panel has 24px side gutters, map fills panel, caption wraps cleanly.
8. `resize_window` with `preset: "desktop"` to reset.

If the section's top or bottom spacing looks different from the "What it does" and Contact sections, the `.section.photo-bg{padding:88px 0}` rule applies to all three, so check that the section has both `section` and `photo-bg` classes.

- [ ] **Step 6: Commit**

```bash
cd C:/projects/pressboxtoolkit-site && git add index.html styles.css && git commit -m "Add user-location map section to home page

New photo-backed section between What it does and Contact showing the
static US map in a floating panel with a headline state count.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>"
```

---

### Task 3: Update site metadata that describes the home page

**Files:**
- Modify: `llms.txt`

**Interfaces:**
- Consumes: nothing.
- Produces: nothing.

- [ ] **Step 1: Check whether llms.txt lists home page sections**

Run:
```bash
cd C:/projects/pressboxtoolkit-site && cat llms.txt
```
If the file describes page sections (for example lists "What it does" or "Contact"), add one line in the same style after the "What it does" entry:
```
- Where we are: map of user locations across 24 states
```
If the file only describes the product and links, make no change and skip to Step 3.

- [ ] **Step 2: Commit (only if changed)**

```bash
cd C:/projects/pressboxtoolkit-site && git add llms.txt && git commit -m "Mention user map section in llms.txt

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>"
```

- [ ] **Step 3: Confirm clean tree**

Run:
```bash
cd C:/projects/pressboxtoolkit-site && git status --short && git log --oneline -4
```
Expected: no uncommitted files, and the top commits are the map section, the SVG, and the spec.
