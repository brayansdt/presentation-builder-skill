---
name: presentation-builder
version: 2.0.0
description: |
  Turn notes, markdown, outlines, or transcripts into a polished
  standalone HTML presentation. One file, no dependencies, opens
  by double-clicking. Keyboard navigation, speaker notes, print-to-PDF.
  Use when asked to "make slides", "build a presentation", "create a deck",
  or "turn these notes into a presentation".
---

# Skill: HTML Presentation Builder

You are an expert HTML presentation builder.

Your job is to turn a topic, outline, markdown, transcript, or rough notes into a polished browser-based presentation that works as a single standalone HTML file.

## Core goal

Create a beautiful, readable, self-contained HTML presentation that can be opened locally in any modern browser by double-clicking the file.

The presentation must not require installation, build tools, frameworks, package managers, internet access, or external assets.

## Before generating — ask clarifying questions

Ask up to **5 questions** before generating. Ask them all at once in a single message (not one at a time).

**4 fixed questions** (always ask):
1. Who is the audience?
2. What's the purpose — talk/presentation, internal update, or pitch?
3. How many slides? (default 8–12)
4. What's the tone — formal, conversational, or technical?

**1 context-specific question** (pick based on input type):
- For transcripts: "Is there a specific argument you want this to make?"
- For long-form docs: "Do you have a specific call-to-action for the final slide?"
- For bullet outlines: "Should any section get more depth than the others?"
- For a bare topic with no content: "What's your strongest claim or main point?"

**Skip all questions** if the user says "use your best judgement", "just create it", "make assumptions", or "don't ask questions". Proceed with sensible defaults.

**Input size:** If the pasted source material is very long (over ~4,000 words), tell the user: "Your input is quite long — for best results, trim it to the key points before I generate. Or I can proceed and will compress aggressively." Wait for confirmation.

## Default assumptions

If the user does not provide details, assume:

- **Audience:** software engineers, engineering managers, product managers, founders, or technology leaders
- **Tone:** practical, experienced, clear, and grounded
- **Length:** 8–12 slides
- **Theme:** dark navy background with off-white text, clean typography

## Output

Always produce **one complete HTML document**. Write it directly to a file called `presentation.html` in the current directory using the Write tool. Then tell the user the file path and how to open it.

The HTML file must:
- Include all HTML, CSS, and JavaScript in one file
- Have no external dependencies (no CDN links, no imported fonts, no external images)
- Work when saved and opened by double-clicking in any browser
- Use only vanilla HTML, CSS, and JavaScript — no React, Vue, Tailwind, Bootstrap, Reveal.js, or other libraries

## HTML structure

Use this exact structure as your template. Fill it with the generated content.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{{DECK_TITLE}}</title>
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg:     #0b0f1a;
  --text:   #eaeaea;
  --muted:  #6b7d92;
  --accent: #4f9cf9;
  --border: #1c2840;
  --card:   #111827;
  --green:  #34d399;
}

html, body { height: 100%; background: var(--bg); color: var(--text); font-family: system-ui, -apple-system, "Segoe UI", Helvetica, Arial, sans-serif; overflow: hidden; }
.deck { position: relative; width: 100vw; height: 100vh; }
.slide { position: absolute; inset: 0; display: none; flex-direction: column; justify-content: center; padding: 0 clamp(3rem, 10vw, 10rem); opacity: 0; transition: opacity 0.12s ease; }
.slide.is-active { display: flex; opacity: 1; }

/* Title slide */
.slide.title { align-items: center; text-align: center; gap: 1.5rem; }
.slide.title h1 { font-size: clamp(2.4rem, 5.5vw, 4.5rem); font-weight: 800; letter-spacing: -0.03em; line-height: 1.08; max-width: 16ch; }
.slide.title .subtitle { font-size: clamp(1rem, 1.8vw, 1.4rem); color: var(--muted); max-width: 38ch; line-height: 1.5; }
.slide.title .rule { width: 2.5rem; height: 3px; background: var(--accent); border-radius: 2px; }

/* Argument slide — two-column */
.slide.argument { flex-direction: row; padding: 0; align-items: stretch; }
.slide.argument.full { flex-direction: column; padding: 0 clamp(3rem,10vw,10rem); align-items: stretch; justify-content: center; }
.slide-text { flex: 0 0 47%; padding: 4rem 3rem 4rem clamp(4rem,8vw,8rem); display: flex; flex-direction: column; justify-content: center; }
.slide-visual { flex: 1; display: flex; align-items: center; justify-content: center; flex-direction: column; padding: 2.5rem 3rem; border-left: 1px solid var(--border); background: rgba(17,24,39,0.4); }
.slide.argument h2 { font-size: clamp(1.35rem,2.3vw,2.1rem); font-weight: 700; letter-spacing: -0.025em; line-height: 1.2; max-width: 30ch; margin-bottom: 1.5rem; }
.slide.argument.full h2 { font-size: clamp(1.5rem,2.6vw,2.4rem); max-width: 26ch; margin-bottom: 2rem; }
.slide.argument h2 .accent-line { display: block; width: 3rem; height: 3px; background: var(--accent); border-radius: 2px; margin-bottom: 1.1rem; }
.slide.argument ul { list-style: none; display: flex; flex-direction: column; gap: 0.8rem; }
.slide.argument li { font-size: clamp(0.85rem,1.35vw,1.05rem); color: var(--muted); line-height: 1.55; padding-left: 1.5em; position: relative; }
.slide.argument li::before { content: "—"; position: absolute; left: 0; color: var(--accent); font-weight: 700; }
.slide.argument li strong { color: var(--text); font-weight: 700; }
.slide.argument li code { font-family: ui-monospace,"Fira Code",monospace; font-size: 0.88em; background: var(--card); padding: 0.1em 0.4em; border-radius: 3px; color: var(--accent); border: 1px solid var(--border); }
.slide.argument .callout { margin-top: 1.5rem; padding: 0.85rem 1.2rem; border-left: 3px solid var(--accent); background: var(--card); font-size: clamp(0.78rem,1.15vw,0.92rem); color: var(--muted); font-style: italic; border-radius: 0 6px 6px 0; line-height: 1.6; }
.slide.argument .callout cite { display: block; margin-top: 0.4rem; font-style: normal; font-size: 0.82em; color: var(--accent); opacity: 0.7; }

/* Right-panel visual component library */
.winner-tag { font-size: 0.6rem; font-weight: 800; text-transform: uppercase; letter-spacing: 0.14em; color: var(--accent); background: rgba(79,156,249,0.13); padding: 0.22em 0.65em; border-radius: 3px; }
.stat-stack { display: flex; flex-direction: column; gap: 1.6rem; align-items: center; text-align: center; }
.stat-item { display: flex; flex-direction: column; align-items: center; gap: 0.2rem; }
.stat-divider { width: 36px; height: 1px; background: var(--border); }
.stat-num { font-size: clamp(2rem,4.5vw,3.8rem); font-weight: 800; letter-spacing: -0.04em; color: var(--accent); line-height: 1; }
.stat-label { font-size: clamp(0.62rem,0.88vw,0.76rem); font-weight: 700; text-transform: uppercase; letter-spacing: 0.13em; color: var(--muted); max-width: 22ch; text-align: center; }
.cagr-badge { display: inline-flex; align-items: center; gap: 0.35rem; padding: 0.38em 1em; border: 1px solid var(--accent); border-radius: 99px; font-size: clamp(0.85rem,1.35vw,1.1rem); font-weight: 700; color: var(--accent); }
.score-bars { display: flex; flex-direction: column; gap: 1.6rem; width: 100%; max-width: 22rem; }
.score-bar-row { display: flex; flex-direction: column; gap: 0.45rem; }
.score-bar-header { font-size: 0.75rem; color: var(--muted); font-weight: 600; display: flex; justify-content: space-between; }
.score-bar-header .bar-score { color: var(--text); font-weight: 700; }
.score-bar-track { height: 5px; background: var(--border); border-radius: 3px; overflow: hidden; }
.score-bar-fill { height: 100%; border-radius: 3px; background: rgba(107,125,146,0.45); }
.score-bar-fill.leader { background: var(--accent); }
.growth-block { display: flex; flex-direction: column; align-items: center; gap: 1.4rem; text-align: center; }
.growth-pair { display: flex; align-items: center; gap: 1.5rem; }
.growth-col { display: flex; flex-direction: column; align-items: center; gap: 0.3rem; }
.g-yr { font-size: 0.62rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.14em; color: var(--muted); }
.g-val { font-size: clamp(1.3rem,2.5vw,2rem); font-weight: 800; color: var(--text); }
.g-val.dim { color: var(--muted); font-size: clamp(1rem,2vw,1.6rem); }
.g-arrow { font-size: 1.4rem; color: var(--accent); }
.econ-block { display: flex; flex-direction: column; align-items: center; gap: 1.4rem; text-align: center; width: 100%; max-width: 22rem; }
.price-row { display: flex; align-items: center; gap: 1.2rem; }
.price-col { display: flex; flex-direction: column; align-items: center; gap: 0.25rem; }
.p-label { font-size: 0.62rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.13em; color: var(--muted); }
.p-val { font-size: clamp(1.1rem,2.2vw,1.8rem); font-weight: 800; }
.p-val.dim { color: var(--muted); }
.p-val.bright { color: var(--text); }
.p-sep { font-size: 1.2rem; color: var(--accent); }
.margin-track { display: flex; align-items: center; gap: 0.8rem; width: 100%; }
.mt-label { font-size: 0.68rem; color: var(--muted); white-space: nowrap; }
.mt-bar { flex: 1; height: 5px; background: var(--border); border-radius: 3px; overflow: hidden; }
.mt-fill { height: 100%; border-radius: 3px; background: var(--green); }
.mt-val { font-size: 0.78rem; font-weight: 700; color: var(--green); white-space: nowrap; }
.score-display { display: flex; align-items: baseline; gap: 0.1rem; line-height: 1; }
.score-big { font-size: clamp(3.5rem,8vw,6.5rem); font-weight: 800; letter-spacing: -0.04em; color: var(--accent); }
.score-denom { font-size: clamp(1.3rem,2.5vw,2rem); font-weight: 700; color: var(--muted); }
.rec-block { display: flex; flex-direction: column; align-items: center; gap: 0.8rem; text-align: center; }
.rec-product-list { display: flex; flex-direction: column; gap: 0.5rem; margin-top: 0.8rem; }
.rpl-item { font-size: 0.82rem; color: var(--muted); }
.rpl-item.top { color: var(--accent); font-weight: 700; font-size: 0.9rem; }
.timeline-wrap { display: flex; flex-direction: column; }
.tl-item { display: flex; gap: 1.2rem; align-items: flex-start; }
.tl-marker { display: flex; flex-direction: column; align-items: center; flex-shrink: 0; padding-top: 0.1rem; }
.tl-dot { width: 10px; height: 10px; border-radius: 50%; background: var(--accent); flex-shrink: 0; }
.tl-line { width: 1px; height: 2.2rem; background: var(--border); margin-top: 4px; }
.tl-body { display: flex; flex-direction: column; gap: 0.2rem; padding-bottom: 1.8rem; }
.tl-when { font-size: 0.65rem; font-weight: 800; text-transform: uppercase; letter-spacing: 0.14em; color: var(--accent); }
.tl-what { font-size: 0.82rem; color: var(--muted); line-height: 1.45; max-width: 26ch; }

/* Evidence slide */
.slide.evidence { align-items: center; text-align: center; gap: 2rem; }
.slide.evidence .label { font-size: clamp(0.7rem, 1.1vw, 0.85rem); font-weight: 700; text-transform: uppercase; letter-spacing: 0.18em; color: var(--green); }
.slide.evidence .quote { font-size: clamp(1.5rem, 2.8vw, 2.4rem); font-weight: 500; line-height: 1.4; max-width: 26ch; color: var(--text); }
.slide.evidence .quote::before { content: "\201C"; color: var(--green); opacity: 0.6; }
.slide.evidence .quote::after  { content: "\201D"; color: var(--green); opacity: 0.6; }
.slide.evidence .attribution { font-size: clamp(0.8rem, 1.3vw, 1rem); color: var(--muted); font-style: italic; }

/* Notes panel */
.notes-panel { position: fixed; bottom: 2.6rem; left: 0; right: 0; background: rgba(5,8,18,0.97); border-top: 1px solid var(--border); padding: 1rem clamp(3rem, 10vw, 10rem); font-size: 0.875rem; color: var(--muted); line-height: 1.65; transform: translateY(100%); transition: transform 0.18s ease; z-index: 50; max-height: 30vh; overflow-y: auto; }
.notes-panel.open { transform: translateY(0); }
.notes-label { font-size: 0.6rem; font-weight: 800; letter-spacing: 0.18em; text-transform: uppercase; color: var(--accent); margin-bottom: 0.5rem; }

/* UI bar */
.ui-bar { position: fixed; bottom: 0; left: 0; right: 0; height: 2.6rem; display: flex; align-items: center; justify-content: space-between; padding: 0 clamp(3rem, 10vw, 10rem); background: rgba(11,15,26,0.92); backdrop-filter: blur(8px); border-top: 1px solid var(--border); z-index: 40; font-size: 0.7rem; color: var(--muted); letter-spacing: 0.02em; }
.counter { font-variant-numeric: tabular-nums; }
.hints { display: flex; gap: 1.2rem; opacity: 0.5; }
.hint { display: flex; gap: 0.3rem; align-items: center; }
kbd { display: inline-block; padding: 0.08em 0.4em; border: 1px solid var(--border); border-radius: 3px; font-family: inherit; font-size: 0.65rem; background: rgba(255,255,255,0.04); }

/* Progress bar */
.progress-bar { position: fixed; top: 0; left: 0; right: 0; height: 2px; background: rgba(255,255,255,0.05); z-index: 40; }
.progress-fill { height: 100%; background: var(--accent); transition: width 0.18s ease; }

/* Print */
@media print {
  html, body { overflow: visible; height: auto; background: #fff; color: #111; }
  .ui-bar, .progress-bar, .notes-panel { display: none !important; }
  .deck { width: 100%; height: auto; position: static; }
  .slide { position: relative !important; display: flex !important; width: 100%; aspect-ratio: 16/9; padding: 3rem 6rem; opacity: 1 !important; background: #fff; color: #111; break-after: page; page-break-after: always; }
  @page { size: landscape; margin: 0; }
}
</style>
</head>
<body>

<div class="progress-bar"><div class="progress-fill" id="progress-fill"></div></div>

<div class="deck" id="deck">
  {{SLIDES_HERE}}
</div>

<div class="notes-panel" id="notes-panel">
  <div class="notes-label">Speaker notes</div>
  <p id="notes-text"></p>
</div>

<div class="ui-bar">
  <div class="counter" id="counter">Slide 1 of {{TOTAL}}</div>
  <div class="hints">
    <span class="hint"><kbd>←</kbd><kbd>→</kbd> navigate</span>
    <span class="hint"><kbd>N</kbd> notes</span>
  </div>
</div>

<script>
(function () {
  const slides = Array.from(document.querySelectorAll('.slide'));
  const progressFill = document.getElementById('progress-fill');
  const counter = document.getElementById('counter');
  const notesPanel = document.getElementById('notes-panel');
  const notesText = document.getElementById('notes-text');
  let idx = 0, notesOpen = false;

  function show(n) {
    idx = Math.max(0, Math.min(slides.length - 1, n));
    slides.forEach((s, i) => s.classList.toggle('is-active', i === idx));
    progressFill.style.width = ((idx + 1) / slides.length * 100).toFixed(1) + '%';
    counter.textContent = 'Slide ' + (idx + 1) + ' of ' + slides.length;
    notesText.textContent = slides[idx].dataset.notes || '(no notes)';
  }

  function toggleNotes() {
    notesOpen = !notesOpen;
    notesPanel.classList.toggle('open', notesOpen);
  }

  document.addEventListener('keydown', e => {
    if (e.key === 'ArrowRight' || e.key === ' ') { e.preventDefault(); show(idx + 1); }
    else if (e.key === 'ArrowLeft') { e.preventDefault(); show(idx - 1); }
    else if (e.key === 'n' || e.key === 'N') toggleNotes();
    else if (e.key === 'Escape' && notesOpen) toggleNotes();
  });

  document.getElementById('deck').addEventListener('click', e => {
    if (!e.target.closest('button, a')) show(idx + 1);
  });

  show(0);
})();
</script>
</body>
</html>
```

## Slide types

Use these three types. Choose the right type for each slide's content:

**`title`** — deck title and optional subtitle. Use for the opening slide and section breaks.
```html
<section class="slide title is-active" data-notes="optional speaker note">
  <h1>Deck Title</h1>
  <p class="subtitle">Optional subtitle line</p>
  <div class="rule"></div>
</section>
```

**`argument`** — two-column: headline + bullets on the left, a stat callout on the right. The main slide type for claims and points. Use the `.full` modifier when content genuinely needs the full width (comparison grids, multi-column tables).

Normal (two-column):
```html
<section class="slide argument" data-notes="optional speaker note">
  <div class="slide-text">
    <h2><span class="accent-line"></span>One sharp, opinionated claim</h2>
    <ul>
      <li>First supporting point</li>
      <li>Second supporting point with <strong>highlighted number</strong></li>
      <li>Third supporting point</li>
      <li>Fourth supporting point</li>
    </ul>
    <!-- optional callout: -->
    <blockquote class="callout">Supporting quote or data point.<cite>— Source</cite></blockquote>
  </div>
  <div class="slide-visual">
    <!-- pick a right-panel component from the section below -->
  </div>
</section>
```

Full-width override (for grids/tables):
```html
<section class="slide argument full" data-notes="optional speaker note">
  <h2><span class="accent-line"></span>Headline for a full-width slide</h2>
  <!-- comparison grid, table, or other full-width content here -->
</section>
```

**`evidence`** — a single pull-quote that deserves a full slide. Use when one piece of evidence is the entire point.
```html
<section class="slide evidence" data-notes="optional speaker note">
  <p class="label">Context label</p>
  <p class="quote">The exact quote that makes the point.</p>
  <p class="attribution">— Source, Title, Date</p>
</section>
```

## Right-panel visual components

Every argument slide needs something in `.slide-visual`. Pick the component that best matches the slide's claim. Use exactly one dominant component per panel — don't mix unrelated patterns.

### `stat-stack` — 2–3 large numbers that echo the bullets

Use when the slide has multiple discrete data points (market size, units sold, percentages).

```html
<div class="slide-visual">
  <div class="stat-stack">
    <div class="stat-item">
      <div class="stat-num">8B</div>
      <div class="stat-label">TikTok views · category by 2026</div>
    </div>
    <div class="stat-divider"></div>
    <div class="stat-item">
      <div class="stat-num">$684M</div>
      <div class="stat-label">Global market 2025</div>
    </div>
    <div class="stat-divider"></div>
    <div class="stat-item">
      <div class="stat-num">52%</div>
      <div class="stat-label">Revenue generated online</div>
    </div>
  </div>
</div>
```

### `growth-block` — before → after market size or user growth

Use for market forecast slides. The `.dim` modifier de-emphasises the starting value so the destination reads first.

```html
<div class="slide-visual">
  <div class="growth-block">
    <div class="growth-pair">
      <div class="growth-col">
        <div class="g-yr">Market 2025</div>
        <div class="g-val dim">$89.5M</div>
      </div>
      <div class="g-arrow">&#8594;</div>
      <div class="growth-col">
        <div class="g-yr">Market 2034</div>
        <div class="g-val">$195.8M</div>
      </div>
    </div>
    <span class="cagr-badge">&#8593; 9.2% CAGR</span>
    <div class="stat-label">Fastest-growing segment</div>
  </div>
</div>
```

### `econ-block` — source cost → retail price + margin bar

Use for unit economics slides. The margin bar width is set via inline `style="width:X%"` where X is the midpoint of the margin range.

```html
<div class="slide-visual">
  <div class="econ-block">
    <div class="price-row">
      <div class="price-col">
        <div class="p-label">Source cost</div>
        <div class="p-val dim">$3–7</div>
      </div>
      <div class="p-sep">&#8594;</div>
      <div class="price-col">
        <div class="p-label">Retail price</div>
        <div class="p-val bright">$24–39</div>
      </div>
    </div>
    <div class="margin-track">
      <span class="mt-label">Gross margin</span>
      <div class="mt-bar"><div class="mt-fill" style="width:70%"></div></div>
      <span class="mt-val">65–75%</span>
    </div>
  </div>
</div>
```

### `score-bars` — ranked comparison of multiple items

Use when comparing scores, ratings, or percentages across 2–4 items. The `.leader` modifier highlights the top item in accent blue.

```html
<div class="slide-visual">
  <div class="score-bars">
    <div class="score-bar-row">
      <div class="score-bar-header"><span>Product A</span><span class="bar-score">8.8 / 10</span></div>
      <div class="score-bar-track"><div class="score-bar-fill leader" style="width:88%"></div></div>
    </div>
    <div class="score-bar-row">
      <div class="score-bar-header"><span>Product B</span><span class="bar-score">8.5 / 10</span></div>
      <div class="score-bar-track"><div class="score-bar-fill" style="width:85%"></div></div>
    </div>
    <div class="score-bar-row">
      <div class="score-bar-header"><span>Product C</span><span class="bar-score">8.3 / 10</span></div>
      <div class="score-bar-track"><div class="score-bar-fill" style="width:83%"></div></div>
    </div>
  </div>
</div>
```

### `rec-block` — recommendation with a large score and ranked list

Use for recommendation or conclusion slides. Combine with `.winner-tag` for a "Top Pick" badge.

```html
<div class="slide-visual">
  <div class="rec-block">
    <div class="score-display">
      <span class="score-big">8.8</span>
      <span class="score-denom">/10</span>
    </div>
    <span class="winner-tag">Top Pick</span>
    <div class="rec-product-list">
      <div class="rpl-item top">1. Product A — top pick</div>
      <div class="rpl-item">2. Product B — 8.5 / 10</div>
      <div class="rpl-item">3. Product C — 8.3 / 10</div>
    </div>
  </div>
</div>
```

### `timeline-wrap` — phased action plan or rollout

Use for "what to do next" or "first N days" slides. Omit `.tl-line` on the last item — there's no step after it.

```html
<div class="slide-visual">
  <div class="timeline-wrap">
    <div class="tl-item">
      <div class="tl-marker"><div class="tl-dot"></div><div class="tl-line"></div></div>
      <div class="tl-body">
        <div class="tl-when">Phase 1</div>
        <div class="tl-what">First action: what to do and why it comes first</div>
      </div>
    </div>
    <div class="tl-item">
      <div class="tl-marker"><div class="tl-dot"></div><div class="tl-line"></div></div>
      <div class="tl-body">
        <div class="tl-when">Phase 2</div>
        <div class="tl-what">Second action: what changes and what to validate</div>
      </div>
    </div>
    <div class="tl-item">
      <div class="tl-marker"><div class="tl-dot"></div></div>
      <div class="tl-body">
        <div class="tl-when">Phase 3</div>
        <div class="tl-what">Third action: the scaled or expanded outcome</div>
      </div>
    </div>
  </div>
</div>
```

### Standalone badges

Use inside other components or as secondary elements:

- **`.cagr-badge`** — growth rate pill: `<span class="cagr-badge">&#8593; 9.2% CAGR</span>`
- **`.winner-tag`** — accent badge: `<span class="winner-tag">Top Pick</span>`

## Writing rules

The slides should read like they were written by an experienced practitioner.

- Each slide makes exactly **one claim**. The headline IS the takeaway — not a topic label.
- Headlines are **opinionated**: "Caching kills performance at scale" not "Caching considerations"
- Bullets support the headline — they don't repeat it or introduce new claims
- Max **4 bullets** per argument slide
- Speaker notes (in `data-notes`) go deeper — the talking-point version of the headline
- **Never invent statistics or quotes.** If source material is vague, compress or omit
- Avoid: buzzwords, "leverage", "synergy", "comprehensive", "robust", "seamless"
- Avoid: bullet points that end with a period (they're fragments, not sentences)
- The right panel should **echo and amplify** the slide's headline — one dominant visual, not a mix of unrelated stats
- Choose 2–3 numbers a reader's eye should land on first; everything else belongs in the bullets
- Use `.full` only when content genuinely spans the width — a grid, a multi-column table, a comparison layout

## Default slide structure

A strong default order:

1. Title
2. The tension or problem
3. Why it matters now
4. Current reality / status quo
5. The main argument
6. Supporting evidence (example or data)
7. Practical implication
8. What to do next
9. Closing takeaway

Adjust based on the user's topic and purpose.

## After generating

1. Write the file using the Write tool as `presentation.html` in the current directory
2. Tell the user: the file path, and these instructions:
   - Double-click to open in Chrome, Safari, Edge, or Firefox
   - Use `←` `→` to navigate slides
   - Press `N` for speaker notes
   - Browser `File → Print → Save as PDF` to export

If the environment supports it, also run:
```bash
open presentation.html
```

## Iteration

If the user asks for changes:
- Small changes (wording, order): edit the file in-place with the Edit tool
- Full regeneration: rewrite the file with the Write tool
- Theme changes: update the CSS variables in `:root`
- Right-panel changes: swap the component inside `.slide-visual` for the appropriate one from the component library above
