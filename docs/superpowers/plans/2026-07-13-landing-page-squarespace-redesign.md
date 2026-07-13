# Landing Page Squarespace-Style Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rebuild index.html and styles.css so the site matches the former Squarespace design: light turquoise theme, Anton/Epilogue fonts, orange pill buttons, three animated statement sections, three-column links row, Formspree contact form.

**Architecture:** Pure static HTML + CSS. Each statement section is a `position:relative` flex section with an inline SVG "scene" as an absolutely-positioned background layer, a scrim div for text contrast, and an Anton headline. All motion is CSS keyframes targeting SVG groups; no JavaScript (the existing nav-toggle onclick is the only JS and stays). CSS gradient backgrounds on each section double as the fallback if SVG fails and as the reduced-motion-friendly base.

**Tech Stack:** HTML5, CSS3 (keyframes, backdrop-filter), inline SVG, Google Fonts (Anton, Epilogue), Formspree (form POST endpoint), GitHub Pages hosting.

## Global Constraints

- Colors: background `#CCE6EA`, text `#020617`, accent orange `#F97415`, button text `#F8FAFC`.
- Fonts: headlines **Anton** (weight 400 only, always uppercase), body **Epilogue**.
- Buttons are pills: `border-radius:999px`.
- Logo in nav/footer is `PBT-logo-black.png` (already in repo). Favicon becomes `PBT-logo-black.png` on both pages.
- No video files, no new JS files, no external assets beyond Google Fonts and Formspree.
- Formspree form ID is not yet known: use literal `YOUR_FORM_ID` in the action URL with an HTML comment noting it must be replaced. This is intentional, not a placeholder failure.
- Respect `prefers-reduced-motion: reduce` — all scene animations disabled, static composition remains.
- `details.html` keeps its structure; it inherits the new theme through styles.css. Its class names (`hero`, `section`, `alt`, `eyebrow`, `lede`, `product`, `tag`, `feat`, `why`, `item`, `cta-band`, `btn-dark`, `foot`) must all remain styled.
- This is a visual project with no test framework; each task's test cycle is a concrete browser verification step against a local server.

## Local preview server

All verification steps use the Browser pane. First-time setup (Task 1 creates this):

`.claude/launch.json`:
```json
{
  "version": "0.0.1",
  "configurations": [
    {
      "name": "site",
      "runtimeExecutable": "python",
      "runtimeArgs": ["-m", "http.server", "8080"],
      "port": 8080
    }
  ]
}
```

Start with the preview_start tool (`name: "site"`), then navigate to `http://localhost:8080`.

---

### Task 1: Rewrite styles.css to the light Squarespace theme

**Files:**
- Modify: `styles.css` (full replacement)
- Create: `.claude/launch.json` (content above)

**Interfaces:**
- Produces: all class names used by Tasks 2-6: `.nav`, `.nav-inner`, `.nav-logo`, `.nav-links`, `.nav-toggle`, `.cta`, `.btn`, `.btn-primary`, `.btn-dark`, `.wrap`, `.stmt`, `.stmt-1`, `.stmt-2`, `.stmt-3`, `.scene`, `.scrim`, `.stmt-title`, `.trio`, `.trio-item`, `.section`, `.alt`, `.eyebrow`, `.lede`, `.form-grid`, `.field`, `.full`, `.hero`, `.sub`, `.accent`, `.hero-cta`, `.product`, `.tag`, `.feat`, `.why`, `.item`, `.cta-band`, `.foot`, `.foot-logo`, `.links`, `.small`, plus scene animation classes `.snow`, `.snow-fast`, `.lamp`, `.ball2`, `.net2`, `.ball3`, `.net3`.

- [ ] **Step 1: Replace the entire contents of styles.css**

```css
/* ===== Press Box Toolkit — landing site (light Squarespace-style theme) ===== */
@import url('https://fonts.googleapis.com/css2?family=Anton&family=Epilogue:wght@400;500;600;700&display=swap');

:root{
  --bg:#CCE6EA;
  --bg-soft:#DCF0F3;
  --ink:#020617;
  --orange:#F97415;
  --orange-deep:#DD5F04;
  --paper:#F8FAFC;
  --mute:#44565C;
  --line:#A8CDD4;
  --disp:'Anton',system-ui,sans-serif;
  --body:'Epilogue',system-ui,sans-serif;
}

*{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth}
body{
  background:var(--bg);
  color:var(--ink);
  font-family:var(--body);
  font-size:17px;
  line-height:1.6;
  -webkit-font-smoothing:antialiased;
}
a{color:inherit;text-decoration:none}
img{max-width:100%;display:block}

.wrap{max-width:1080px;margin:0 auto;padding:0 24px}

/* ---- nav ---- */
.nav{
  position:sticky;top:0;z-index:50;
  background:rgba(204,230,234,.92);
  backdrop-filter:blur(8px);
  border-bottom:1px solid var(--line);
}
.nav-inner{display:flex;align-items:center;justify-content:space-between;height:72px}
.nav-logo{height:40px;width:auto}
.nav-links{display:flex;gap:30px;align-items:center}
.nav-links a{
  font-weight:600;font-size:14px;
  letter-spacing:1.2px;text-transform:uppercase;
  transition:color .15s;
}
.nav-links a:hover{color:var(--orange)}
.nav-links a.cta{
  color:var(--paper);background:var(--orange);
  padding:10px 22px;border-radius:999px;
}
.nav-links a.cta:hover{background:var(--orange-deep);color:var(--paper)}
.nav-toggle{display:none;background:none;border:0;color:var(--ink);font-size:26px;cursor:pointer}

/* ---- buttons ---- */
.btn{
  font-family:var(--body);font-weight:600;letter-spacing:1.2px;text-transform:uppercase;
  font-size:14px;padding:14px 32px;border-radius:999px;cursor:pointer;border:0;
  transition:transform .12s, background .15s;display:inline-block;
}
.btn-primary{background:var(--orange);color:var(--paper)}
.btn-primary:hover{background:var(--orange-deep);transform:translateY(-2px)}
.btn-dark{background:var(--ink);color:var(--paper)}
.btn-dark:hover{transform:translateY(-2px)}

/* ---- statement sections (landing) ---- */
.stmt{
  position:relative;min-height:76vh;
  display:flex;align-items:center;justify-content:center;
  text-align:center;overflow:hidden;padding:90px 24px;
}
.stmt-1{background:linear-gradient(180deg,#BFD9E8,#8FB4CD 55%,#5E86A6)}
.stmt-2{background:linear-gradient(180deg,#F2D8BE,#E0A878 55%,#BC6E48)}
.stmt-3{background:linear-gradient(180deg,#C9C6EA,#8D86C9 55%,#57517E)}
.scene{position:absolute;inset:0;width:100%;height:100%}
.scrim{
  position:absolute;inset:0;
  background:radial-gradient(ellipse 72% 58% at 50% 50%, rgba(220,240,243,.68), rgba(220,240,243,.10) 72%);
}
.stmt-title{
  position:relative;max-width:1000px;
  font-family:var(--disp);font-weight:400;text-transform:uppercase;
  font-size:clamp(36px,6.5vw,80px);line-height:1.12;letter-spacing:1px;
}

/* ---- scene 1: stadium night ---- */
.snow{animation:fall 16s linear infinite}
.snow-fast{animation:fall 9s linear infinite;animation-delay:-4s}
@keyframes fall{to{transform:translateY(810px)}}
.lamp{animation:lampGlow 4s ease-in-out infinite}
.lamp:nth-of-type(2n){animation-delay:1.3s}
.lamp:nth-of-type(3n){animation-delay:2.2s}
@keyframes lampGlow{0%,100%{opacity:.7}50%{opacity:1}}

/* ---- scene 2: soccer goal ---- */
.ball2{transform-box:fill-box;transform-origin:center;animation:kick 6s ease-in-out infinite}
@keyframes kick{
  0%{transform:translate(560px,-60px) scale(.45);opacity:0}
  6%{opacity:1}
  22%{transform:translate(0,0) scale(1)}
  26%{transform:translate(-16px,8px) scale(1)}
  30%{transform:translate(0,0) scale(1)}
  80%{transform:translate(0,0) scale(1);opacity:1}
  88%{opacity:0}
  100%{transform:translate(560px,-60px) scale(.45);opacity:0}
}
.net2{transform-box:fill-box;transform-origin:center;animation:netBulge 6s ease-in-out infinite}
@keyframes netBulge{
  0%,21%{transform:skewX(0)}
  25%{transform:skewX(-3deg) scaleX(1.03)}
  29%{transform:skewX(1.5deg)}
  34%,100%{transform:skewX(0)}
}

/* ---- scene 3: basketball swish ---- */
.ball3{transform-box:fill-box;transform-origin:center;animation:drop 5.5s cubic-bezier(.55,0,.75,.5) infinite}
@keyframes drop{
  0%{transform:translate(24px,-330px) scale(.92);opacity:0}
  8%{opacity:1}
  30%{transform:translate(0,-16px) scale(1)}
  44%{transform:translate(0,150px) scale(.96)}
  56%{transform:translate(0,250px);opacity:0}
  100%{transform:translate(24px,-330px) scale(.92);opacity:0}
}
.net3{transform-box:fill-box;transform-origin:50% 0;animation:netStretch 5.5s ease-in-out infinite}
@keyframes netStretch{
  0%,32%{transform:scaleY(1)}
  42%{transform:scaleY(1.22)}
  52%{transform:scaleY(.97)}
  60%,100%{transform:scaleY(1)}
}

/* ---- section scaffolding ---- */
.section{padding:76px 0}
.section.alt{background:var(--bg-soft)}
.eyebrow{
  font-weight:700;letter-spacing:3px;text-transform:uppercase;
  color:var(--orange);font-size:13px;text-align:center;margin-bottom:12px;
}
.section h2{
  font-family:var(--disp);font-weight:400;text-transform:uppercase;text-align:center;
  font-size:clamp(28px,4.5vw,44px);line-height:1.15;margin-bottom:14px;letter-spacing:1px;
}
.section .lede{text-align:center;color:var(--mute);max-width:680px;margin:0 auto 44px;font-size:18px}

/* ---- trio (landing three columns) ---- */
.trio{display:grid;grid-template-columns:repeat(3,1fr);gap:44px;text-align:center}
.trio-item h3{
  font-family:var(--disp);font-weight:400;text-transform:uppercase;
  font-size:clamp(22px,2.6vw,30px);letter-spacing:1px;margin-bottom:14px;
}
.trio-item p{color:var(--mute);margin-bottom:6px}
.trio-item a{color:var(--orange);font-weight:600;text-decoration:underline;text-underline-offset:3px}
.trio-item a:hover{color:var(--orange-deep)}

/* ---- contact form ---- */
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:18px;max-width:680px;margin:0 auto}
.form-grid .full{grid-column:1 / -1}
.field label{display:block;font-weight:600;font-size:14px;margin-bottom:6px}
.field input,.field textarea{
  width:100%;padding:13px 16px;border:1px solid var(--line);border-radius:10px;
  background:#fff;color:var(--ink);font-family:var(--body);font-size:16px;
}
.field input:focus,.field textarea:focus{outline:2px solid var(--orange);outline-offset:1px;border-color:var(--orange)}
.field textarea{min-height:140px;resize:vertical}
.form-send{grid-column:1 / -1;text-align:center;margin-top:6px}

/* ---- hero (details page) ---- */
.hero{position:relative;text-align:center;padding:96px 24px 72px}
.hero h1{
  font-family:var(--disp);font-weight:400;text-transform:uppercase;
  font-size:clamp(32px,5.5vw,60px);line-height:1.12;letter-spacing:1px;
}
.hero h1 .accent{color:var(--orange)}
.hero .sub{margin:22px auto 0;max-width:680px;color:var(--mute);font-size:clamp(16px,2.2vw,19px)}
.hero-cta{margin-top:34px;display:flex;gap:14px;justify-content:center;flex-wrap:wrap}

/* ---- product family (details) ---- */
.product{
  background:#fff;border:1px solid var(--line);border-radius:14px;
  padding:30px;margin-bottom:18px;
}
.product .tag{font-weight:700;letter-spacing:2px;text-transform:uppercase;color:var(--orange);font-size:13px;margin-bottom:6px}
.product h3{font-family:var(--disp);font-weight:400;text-transform:uppercase;font-size:21px;letter-spacing:.5px;margin-bottom:10px}
.product p{color:#26343a;margin-bottom:10px}
.product .feat{color:var(--mute);font-size:15px}
.product .feat strong{color:var(--ink)}

/* ---- why pillars (details) ---- */
.why{display:grid;grid-template-columns:repeat(2,1fr);gap:26px}
.why .item{border-left:3px solid var(--orange);padding:4px 0 4px 20px}
.why h3{font-family:var(--disp);font-weight:400;text-transform:uppercase;letter-spacing:.5px;font-size:18px;margin-bottom:8px}
.why p{color:#26343a;font-size:16px}

/* ---- final cta band (details) ---- */
.cta-band{
  text-align:center;padding:64px 24px;
  background:linear-gradient(120deg,var(--orange-deep),var(--orange) 60%,#FF9A4D);
}
.cta-band h2{
  font-family:var(--disp);font-weight:400;text-transform:uppercase;color:var(--paper);
  font-size:clamp(28px,4.5vw,44px);letter-spacing:1px;margin-bottom:22px;
}

/* ---- footer ---- */
.foot{border-top:1px solid var(--line);padding:42px 0;text-align:center;background:var(--bg-soft)}
.foot-logo{height:44px;width:auto;margin:0 auto 18px}
.foot a:hover{color:var(--orange)}
.foot .links{
  display:flex;gap:24px;justify-content:center;flex-wrap:wrap;margin-bottom:14px;
  font-weight:600;letter-spacing:1px;text-transform:uppercase;font-size:13px;
}
.foot .small{color:var(--mute);font-size:14px}
.foot .small a{color:var(--orange-deep);text-decoration:underline;text-underline-offset:3px}

/* ---- responsive ---- */
@media(max-width:780px){
  .nav-links{
    position:absolute;top:72px;left:0;right:0;flex-direction:column;gap:0;
    background:var(--bg);border-bottom:1px solid var(--line);
    max-height:0;overflow:hidden;transition:max-height .25s ease;
  }
  .nav-links.open{max-height:340px}
  .nav-links a{padding:16px 24px;width:100%;border-top:1px solid var(--line)}
  .nav-links a.cta{border-radius:0}
  .nav-toggle{display:block}
  .trio,.why,.form-grid{grid-template-columns:1fr}
  .stmt{min-height:62vh}
}

@media(prefers-reduced-motion:reduce){
  *{transition:none!important;scroll-behavior:auto!important}
  .snow,.snow-fast,.lamp,.ball2,.net2,.ball3,.net3{animation:none!important}
}
```

- [ ] **Step 2: Create `.claude/launch.json`** with the JSON shown in the "Local preview server" section above.

- [ ] **Step 3: Verify details.html against the new theme**

Start the preview server (`preview_start` with `name: "site"`), navigate to `http://localhost:8080/details.html`. Check:
- Page background is light turquoise, all body text dark and readable.
- Product cards are white with turquoise borders; headings render in Anton uppercase.
- CTA band is orange with white Anton heading and a dark pill button.
- The only known-broken elements at this stage are the two white logos (invisible on light background) — that is expected; Task 6 fixes them.

- [ ] **Step 4: Commit**

```bash
git add styles.css .claude/launch.json
git commit -m "Rewrite styles.css to light Squarespace-style theme"
```

---

### Task 2: Rebuild index.html structure

**Files:**
- Modify: `index.html` (full replacement)

**Interfaces:**
- Consumes: all classes from Task 1.
- Produces: three section elements `#stmt-1`, `#stmt-2`, `#stmt-3`, each beginning with a `<!-- scene -->` comment marker where Tasks 3-5 insert their SVG as the first child of the section (before the `.scrim` div).

- [ ] **Step 1: Replace the entire contents of index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Press Box Toolkit | Enhance Game Day Operations</title>
<meta name="description" content="Press Box Toolkit is the all-in-one platform for high school game-day operations: rosters, sponsors, scripts, and audio in one place. Streamline your press box.">
<meta property="og:title" content="Press Box Toolkit | Enhance Game Day Operations">
<meta property="og:description" content="The all-in-one platform for high school press box crews: rosters, sponsors, scripts, and audio in one place.">
<meta property="og:type" content="website">
<meta property="og:url" content="https://www.pressboxtoolkit.com">
<meta property="og:image" content="https://www.pressboxtoolkit.com/PBT-logo-black.png">
<link rel="icon" href="PBT-logo-black.png">
<link rel="stylesheet" href="styles.css">
</head>
<body>

<!-- NAV -->
<header class="nav">
  <div class="wrap nav-inner">
    <a href="index.html"><img class="nav-logo" src="PBT-logo-black.png" alt="Press Box Toolkit"></a>
    <button class="nav-toggle" aria-label="Menu" onclick="document.getElementById('navlinks').classList.toggle('open')">☰</button>
    <nav class="nav-links" id="navlinks">
      <a href="index.html">Home</a>
      <a href="details.html">Details</a>
      <a href="https://jgreggibbs.github.io/press-box-toolkit/">Try It</a>
      <a class="cta" href="#contact">Contact Us</a>
    </nav>
  </div>
</header>

<!-- STATEMENT 1: stadium night -->
<section class="stmt stmt-1" id="stmt-1">
  <!-- scene -->
  <div class="scrim"></div>
  <h1 class="stmt-title">We're building the tools to<br>streamline game day<br>planning &amp; operations</h1>
</section>

<!-- STATEMENT 2: soccer goal -->
<section class="stmt stmt-2" id="stmt-2">
  <!-- scene -->
  <div class="scrim"></div>
  <h2 class="stmt-title">An intuitive platform to<br>manage rosters, sponsors,<br>audio, and more</h2>
</section>

<!-- STATEMENT 3: basketball swish -->
<section class="stmt stmt-3" id="stmt-3">
  <!-- scene -->
  <div class="scrim"></div>
  <h2 class="stmt-title">Every game<br>executed flawlessly</h2>
</section>

<!-- THREE COLUMNS -->
<section class="section">
  <div class="wrap">
    <div class="trio">
      <div class="trio-item">
        <h3>Tell Me More</h3>
        <p>Interested in learning more?</p>
        <p>Browse the <a href="details.html">Details</a> page for an overview.</p>
      </div>
      <div class="trio-item">
        <h3>Try It Out</h3>
        <p>Ready to dive in?</p>
        <p>Create an account and try it out on the <a href="https://jgreggibbs.github.io/press-box-toolkit/">Demo</a> page.</p>
      </div>
      <div class="trio-item">
        <h3>Contact Us</h3>
        <p>Questions or feedback?</p>
        <p><a href="#contact">Send us a message</a> and we will be in touch shortly.</p>
      </div>
    </div>
  </div>
</section>

<!-- CONTACT -->
<section class="section alt" id="contact">
  <div class="wrap">
    <p class="eyebrow">Contact Us</p>
    <h2>Questions or feedback?</h2>
    <p class="lede">Thanks for your interest in Press Box Toolkit! Send us a message and we will be in touch shortly.</p>
    <!-- Replace YOUR_FORM_ID with the real Formspree form ID once the account exists -->
    <form class="form-grid" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
      <div class="field">
        <label for="first-name">First Name</label>
        <input id="first-name" name="first-name" type="text" autocomplete="given-name" required>
      </div>
      <div class="field">
        <label for="last-name">Last Name</label>
        <input id="last-name" name="last-name" type="text" autocomplete="family-name" required>
      </div>
      <div class="field full">
        <label for="email">Email</label>
        <input id="email" name="email" type="email" autocomplete="email" required>
      </div>
      <div class="field full">
        <label for="message">Message</label>
        <textarea id="message" name="message" required></textarea>
      </div>
      <div class="form-send">
        <button class="btn btn-primary" type="submit">Send</button>
      </div>
    </form>
  </div>
</section>

<!-- FOOTER -->
<footer class="foot">
  <div class="wrap">
    <img class="foot-logo" src="PBT-logo-black.png" alt="Press Box Toolkit">
    <div class="links">
      <a href="index.html">Home</a>
      <a href="details.html">Details</a>
      <a href="https://jgreggibbs.github.io/press-box-toolkit/">Try It</a>
      <a href="#contact">Contact</a>
    </div>
    <p class="small"><a href="https://www.linkedin.com/company/press-box-toolkit-llc">LinkedIn</a> &nbsp;·&nbsp; info@pressboxtoolkit.com</p>
    <p class="small" style="margin-top:10px">© 2026 Press Box Toolkit LLC</p>
  </div>
</footer>

</body>
</html>
```

- [ ] **Step 2: Verify in browser**

Navigate to `http://localhost:8080/`. Check:
- Three tall statement sections show gradient backgrounds (blue, warm orange, purple) with centered dark Anton uppercase headlines, readable through the scrim.
- Nav: black logo, uppercase links, orange pill "Contact Us" that scrolls to the form.
- Trio row: three Anton headings with orange underlined links.
- Contact form: two-column name row, full-width email and message, orange pill Send button; submitting empty shows native validation.
- Footer: black logo, light background.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Rebuild landing page to Squarespace-style structure"
```

---

### Task 3: Scene 1 — stadium night SVG

**Files:**
- Modify: `index.html` (replace the `<!-- scene -->` comment inside `#stmt-1`)

**Interfaces:**
- Consumes: `.scene`, `.snow`, `.snow-fast`, `.lamp` styles/keyframes from Task 1.

- [ ] **Step 1: Insert the SVG**

Replace `<!-- scene -->` inside `#stmt-1` with:

```html
<svg class="scene" viewBox="0 0 1440 810" preserveAspectRatio="xMidYMid slice" aria-hidden="true" focusable="false">
  <defs>
    <linearGradient id="sky1" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0" stop-color="#BFD9E8"/>
      <stop offset=".55" stop-color="#8FB4CD"/>
      <stop offset="1" stop-color="#5E86A6"/>
    </linearGradient>
    <filter id="blurL" x="-50%" y="-50%" width="200%" height="200%"><feGaussianBlur stdDeviation="18"/></filter>
    <filter id="blurS" x="-50%" y="-50%" width="200%" height="200%"><feGaussianBlur stdDeviation="7"/></filter>
    <g id="flakes1" fill="#fff">
      <circle cx="60" cy="40" r="3" opacity=".9"/><circle cx="210" cy="150" r="2" opacity=".6"/>
      <circle cx="330" cy="70" r="4" opacity=".8"/><circle cx="470" cy="230" r="2.5" opacity=".7"/>
      <circle cx="590" cy="110" r="3" opacity=".9"/><circle cx="700" cy="320" r="2" opacity=".5"/>
      <circle cx="820" cy="60" r="3.5" opacity=".8"/><circle cx="960" cy="200" r="2.5" opacity=".6"/>
      <circle cx="1100" cy="90" r="3" opacity=".9"/><circle cx="1240" cy="260" r="2" opacity=".7"/>
      <circle cx="1370" cy="140" r="4" opacity=".8"/><circle cx="120" cy="420" r="2.5" opacity=".6"/>
      <circle cx="280" cy="520" r="3" opacity=".8"/><circle cx="430" cy="610" r="2" opacity=".5"/>
      <circle cx="560" cy="470" r="3.5" opacity=".9"/><circle cx="690" cy="580" r="2.5" opacity=".7"/>
      <circle cx="850" cy="500" r="3" opacity=".8"/><circle cx="990" cy="640" r="2" opacity=".6"/>
      <circle cx="1130" cy="450" r="3" opacity=".9"/><circle cx="1290" cy="560" r="2.5" opacity=".7"/>
      <circle cx="1410" cy="680" r="3" opacity=".8"/><circle cx="160" cy="720" r="2" opacity=".5"/>
      <circle cx="520" cy="760" r="3" opacity=".8"/><circle cx="900" cy="740" r="2.5" opacity=".6"/>
    </g>
  </defs>
  <rect width="1440" height="810" fill="url(#sky1)"/>
  <!-- stand structure -->
  <ellipse cx="720" cy="590" rx="1020" ry="330" fill="#3F638A" opacity=".38" filter="url(#blurL)"/>
  <!-- floodlights -->
  <g filter="url(#blurS)" fill="#FFF6D8">
    <circle class="lamp" cx="130" cy="170" r="24"/>
    <circle class="lamp" cx="320" cy="140" r="20"/>
    <circle class="lamp" cx="510" cy="120" r="26"/>
    <circle class="lamp" cx="720" cy="110" r="22"/>
    <circle class="lamp" cx="930" cy="120" r="26"/>
    <circle class="lamp" cx="1120" cy="140" r="20"/>
    <circle class="lamp" cx="1310" cy="170" r="24"/>
  </g>
  <!-- crowd bokeh -->
  <g filter="url(#blurL)" opacity=".8">
    <circle cx="180" cy="520" r="34" fill="#F97415" opacity=".55"/>
    <circle cx="340" cy="600" r="26" fill="#FFD9A8" opacity=".6"/>
    <circle cx="520" cy="540" r="40" fill="#DFF3F6" opacity=".5"/>
    <circle cx="700" cy="620" r="30" fill="#F97415" opacity=".4"/>
    <circle cx="880" cy="560" r="36" fill="#FFD9A8" opacity=".55"/>
    <circle cx="1060" cy="630" r="28" fill="#DFF3F6" opacity=".5"/>
    <circle cx="1240" cy="550" r="38" fill="#F97415" opacity=".45"/>
    <circle cx="1390" cy="640" r="26" fill="#FFD9A8" opacity=".5"/>
    <circle cx="90" cy="660" r="30" fill="#DFF3F6" opacity=".45"/>
    <circle cx="610" cy="700" r="34" fill="#FFD9A8" opacity=".45"/>
    <circle cx="990" cy="710" r="30" fill="#F97415" opacity=".35"/>
  </g>
  <!-- snow: two layers, second offset and faster -->
  <g class="snow"><use href="#flakes1"/><use href="#flakes1" y="-810"/></g>
  <g class="snow-fast" opacity=".65" transform="translate(37,0)"><use href="#flakes1"/><use href="#flakes1" y="-810"/></g>
</svg>
```

- [ ] **Step 2: Verify in browser**

Reload `http://localhost:8080/`. Check section 1:
- Blue stadium mood: glowing floodlight row, soft warm/teal bokeh in the lower half.
- Two snow layers drift downward continuously with no visible jump at the loop point (watch for at least 20 seconds).
- Headline remains clearly readable; if not, raise `.scrim` first alpha from .68 toward .8.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add animated stadium-night scene to statement section 1"
```

---

### Task 4: Scene 2 — soccer goal SVG

**Files:**
- Modify: `index.html` (replace the `<!-- scene -->` comment inside `#stmt-2`)

**Interfaces:**
- Consumes: `.scene`, `.ball2`, `.net2` styles/keyframes from Task 1.

- [ ] **Step 1: Insert the SVG**

Replace `<!-- scene -->` inside `#stmt-2` with:

```html
<svg class="scene" viewBox="0 0 1440 810" preserveAspectRatio="xMidYMid slice" aria-hidden="true" focusable="false">
  <defs>
    <linearGradient id="sky2" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0" stop-color="#F2D8BE"/>
      <stop offset=".55" stop-color="#E0A878"/>
      <stop offset="1" stop-color="#BC6E48"/>
    </linearGradient>
    <filter id="blur2L" x="-50%" y="-50%" width="200%" height="200%"><feGaussianBlur stdDeviation="20"/></filter>
  </defs>
  <rect width="1440" height="810" fill="url(#sky2)"/>
  <!-- warm crowd bokeh -->
  <g filter="url(#blur2L)" opacity=".75">
    <circle cx="150" cy="220" r="40" fill="#C4402A" opacity=".5"/>
    <circle cx="380" cy="150" r="30" fill="#F97415" opacity=".5"/>
    <circle cx="640" cy="240" r="36" fill="#FFE3C2" opacity=".6"/>
    <circle cx="900" cy="160" r="32" fill="#C4402A" opacity=".45"/>
    <circle cx="1150" cy="230" r="42" fill="#F97415" opacity=".5"/>
    <circle cx="1360" cy="150" r="28" fill="#FFE3C2" opacity=".55"/>
    <circle cx="260" cy="640" r="38" fill="#F97415" opacity=".4"/>
    <circle cx="1080" cy="660" r="40" fill="#C4402A" opacity=".4"/>
  </g>
  <!-- goal frame -->
  <g stroke="#FDF8F2" stroke-width="14" fill="none" opacity=".95">
    <path d="M240,700 L240,120 L1200,120"/>
  </g>
  <!-- net -->
  <g class="net2" stroke="#FDF8F2" stroke-width="2.5" opacity=".85">
    <path d="M260,140 L260,690 M310,140 L310,690 M360,140 L360,690 M410,140 L410,690 M460,140 L460,690 M510,140 L510,690 M560,140 L560,690 M610,140 L610,690 M660,140 L660,690 M710,140 L710,690 M760,140 L760,690 M810,140 L810,690 M860,140 L860,690 M910,140 L910,690 M960,140 L960,690 M1010,140 L1010,690 M1060,140 L1060,690 M1110,140 L1110,690 M1160,140 L1160,690"/>
    <path d="M250,180 L1180,180 M250,230 L1180,230 M250,280 L1180,280 M250,330 L1180,330 M250,380 L1180,380 M250,430 L1180,430 M250,480 L1180,480 M250,530 L1180,530 M250,580 L1180,580 M250,630 L1180,630"/>
  </g>
  <!-- ball: outer g positions it, inner g animates relative -->
  <g transform="translate(470,430)">
    <g class="ball2">
      <circle r="52" fill="#FAFAF7" stroke="#D8D3C8" stroke-width="2"/>
      <polygon points="0,-20 19,-6 12,16 -12,16 -19,-6" fill="#1E293B"/>
      <polygon points="-38,-28 -22,-38 -12,-24 -26,-12 -42,-16" fill="#1E293B" opacity=".85"/>
      <polygon points="38,-28 22,-38 12,-24 26,-12 42,-16" fill="#1E293B" opacity=".85"/>
      <polygon points="-30,30 -14,24 -10,40 -24,48" fill="#1E293B" opacity=".85"/>
      <polygon points="30,30 14,24 10,40 24,48" fill="#1E293B" opacity=".85"/>
    </g>
  </g>
</svg>
```

- [ ] **Step 2: Verify in browser**

Reload and check section 2:
- Warm orange/red mood with a white goal frame and mesh filling the frame.
- Ball flies in from upper right every 6 seconds, hits the net, net visibly ripples, ball fades and repeats.
- Loop repeats cleanly; headline stays readable over the net (raise scrim alpha if needed).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add animated soccer-goal scene to statement section 2"
```

---

### Task 5: Scene 3 — basketball swish SVG

**Files:**
- Modify: `index.html` (replace the `<!-- scene -->` comment inside `#stmt-3`)

**Interfaces:**
- Consumes: `.scene`, `.ball3`, `.net3`, `.lamp` styles/keyframes from Task 1.

- [ ] **Step 1: Insert the SVG**

Replace `<!-- scene -->` inside `#stmt-3` with:

```html
<svg class="scene" viewBox="0 0 1440 810" preserveAspectRatio="xMidYMid slice" aria-hidden="true" focusable="false">
  <defs>
    <linearGradient id="sky3" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0" stop-color="#C9C6EA"/>
      <stop offset=".55" stop-color="#8D86C9"/>
      <stop offset="1" stop-color="#57517E"/>
    </linearGradient>
    <filter id="blur3L" x="-50%" y="-50%" width="200%" height="200%"><feGaussianBlur stdDeviation="18"/></filter>
    <filter id="blur3S" x="-50%" y="-50%" width="200%" height="200%"><feGaussianBlur stdDeviation="7"/></filter>
  </defs>
  <rect width="1440" height="810" fill="url(#sky3)"/>
  <!-- arena floodlights -->
  <g filter="url(#blur3S)" fill="#FFFDF2">
    <circle class="lamp" cx="180" cy="120" r="26"/>
    <circle class="lamp" cx="420" cy="90" r="20"/>
    <circle class="lamp" cx="660" cy="110" r="24"/>
    <circle class="lamp" cx="1300" cy="100" r="22"/>
  </g>
  <!-- bokeh -->
  <g filter="url(#blur3L)" opacity=".75">
    <circle cx="160" cy="420" r="38" fill="#6C63B5" opacity=".6"/>
    <circle cx="360" cy="560" r="30" fill="#F97415" opacity=".4"/>
    <circle cx="560" cy="470" r="36" fill="#E4E0FF" opacity=".5"/>
    <circle cx="300" cy="700" r="34" fill="#6C63B5" opacity=".5"/>
    <circle cx="640" cy="680" r="28" fill="#F97415" opacity=".35"/>
    <circle cx="1350" cy="500" r="34" fill="#E4E0FF" opacity=".45"/>
  </g>
  <!-- backboard -->
  <rect x="790" y="130" width="330" height="230" rx="14" fill="#F5F7FB" opacity=".92"/>
  <rect x="880" y="200" width="150" height="110" rx="6" fill="none" stroke="#F97415" stroke-width="7" opacity=".9"/>
  <!-- support arm -->
  <rect x="1120" y="200" width="180" height="26" rx="10" fill="#2E2A45" opacity=".7"/>
  <!-- net (drawn before ball so ball passes in front, rim drawn after ball) -->
  <g class="net3" stroke="#F2F4F8" stroke-width="4" fill="none" opacity=".95">
    <path d="M888,362 L910,470 M920,368 L932,474 M955,370 L955,476 M990,368 L978,474 M1022,362 L1000,470"/>
    <path d="M895,400 L1015,400 M902,436 L1008,436"/>
  </g>
  <!-- ball: outer g positions the through-the-hoop point -->
  <g transform="translate(955,350)">
    <g class="ball3">
      <circle r="50" fill="#F97415" stroke="#C2570A" stroke-width="3"/>
      <path d="M-50,0 L50,0 M0,-50 L0,50 M-35,-35 Q0,-10 35,-35 M-35,35 Q0,10 35,35" stroke="#7C3A08" stroke-width="3.5" fill="none"/>
    </g>
  </g>
  <!-- rim front, over the ball -->
  <ellipse cx="955" cy="352" rx="72" ry="16" fill="none" stroke="#F97415" stroke-width="10"/>
</svg>
```

- [ ] **Step 2: Verify in browser**

Reload and check section 3:
- Purple-blue arena mood, white backboard with orange target square, orange rim with white net.
- Ball drops from above through the rim every 5.5 seconds; net stretches as it passes; ball fades below.
- Ball passes visually in front of the net but behind the front rim ellipse.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add animated basketball-swish scene to statement section 3"
```

---

### Task 6: details.html legibility pass

**Files:**
- Modify: `details.html`

**Interfaces:**
- Consumes: light-theme styles from Task 1.

- [ ] **Step 1: Swap logos and favicon to the black version**

Three edits in `details.html`:

1. Line 13: `<link rel="icon" href="PBT-logo-white.png">` → `<link rel="icon" href="PBT-logo-black.png">`
2. Line 21 (nav): `src="PBT-logo-white.png"` → `src="PBT-logo-black.png"`
3. Line 152 (footer): `src="PBT-logo-white.png"` → `src="PBT-logo-black.png"`

- [ ] **Step 2: Update the nav to match the landing page**

Replace the nav links block (lines 23-28) with:

```html
    <nav class="nav-links" id="navlinks">
      <a href="index.html">Home</a>
      <a href="details.html">Details</a>
      <a href="https://jgreggibbs.github.io/press-box-toolkit/">Try It</a>
      <a class="cta" href="index.html#contact">Contact Us</a>
    </nav>
```

- [ ] **Step 3: Verify in browser**

Navigate to `http://localhost:8080/details.html`. Check top to bottom:
- Logos visible in nav and footer.
- Hero headline renders in Anton uppercase, dark on turquoise, "upgraded" in orange.
- All product cards, why-pillars, CTA band, and footer are legible; no white-on-white or dark-on-dark text anywhere.

- [ ] **Step 4: Commit**

```bash
git add details.html
git commit -m "Switch details page to black logos and shared nav"
```

---

### Task 7: Responsive, reduced-motion, and final verification

**Files:**
- Modify: `index.html`, `styles.css`, `details.html` (only if fixes are needed)

- [ ] **Step 1: Mobile check**

Resize the Browser pane to the mobile preset (375x812). On `http://localhost:8080/`:
- Statement headlines wrap without overflowing; sections shrink to ~62vh.
- Hamburger menu opens and closes; links stack full-width.
- Trio stacks to one column; form fields stack to one column.
- Repeat quickly for details.html.

- [ ] **Step 2: Reduced-motion check**

Run in the browser console via the JavaScript tool: `matchMedia('(prefers-reduced-motion: reduce)').matches` is expected `false` normally; emulate reduced motion via CSS override instead — temporarily add `.snow,.snow-fast,.lamp,.ball2,.net2,.ball3,.net3{animation:none!important}` in devtools or trust the media query, and verify each scene still composes sensibly as a static image (ball resting at net/rim positions defined by their outer `translate` transforms, snow visible at initial positions).

- [ ] **Step 3: Form validation check**

Click Send with empty fields: native validation blocks submission. Fill the fields and confirm the form would POST to `https://formspree.io/f/YOUR_FORM_ID` (do NOT actually submit; the ID is a placeholder until the user supplies the real one).

- [ ] **Step 4: Desktop screenshot review**

Resize back to desktop preset. Screenshot all of index.html section by section and compare against the design tokens: turquoise body, Anton headlines, orange pills, black logo. Fix any visual defects found (overlap, contrast, spacing) and re-verify.

- [ ] **Step 5: Commit any fixes**

```bash
git add -A
git commit -m "Responsive and reduced-motion polish for redesigned pages"
```

Only commit if fixes were made; otherwise skip.

---

## Post-plan follow-ups (user actions, not tasks)

1. Create a free Formspree account with info@pressboxtoolkit.com, create a form, and give me the form ID; I replace `YOUR_FORM_ID` in index.html (one line) and commit.
2. Push to GitHub (`git push`) to deploy via GitHub Pages when you're satisfied with the local preview.
