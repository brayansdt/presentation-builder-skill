---
name: presentation-builder
version: 1.0.0
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

/* Argument slide */
.slide.argument h2 { font-size: clamp(1.7rem, 3vw, 2.8rem); font-weight: 700; letter-spacing: -0.025em; line-height: 1.18; max-width: 22ch; margin-bottom: 2.2rem; }
.slide.argument h2 .accent-line { display: block; width: 3rem; height: 3px; background: var(--accent); border-radius: 2px; margin-bottom: 1.2rem; }
.slide.argument ul { list-style: none; display: flex; flex-direction: column; gap: 1rem; max-width: 52ch; }
.slide.argument li { font-size: clamp(1rem, 1.6vw, 1.25rem); color: var(--muted); line-height: 1.55; padding-left: 1.6em; position: relative; }
.slide.argument li::before { content: "—"; position: absolute; left: 0; color: var(--accent); font-weight: 700; }
.slide.argument li code { font-family: ui-monospace, "Fira Code", monospace; font-size: 0.88em; background: var(--card); padding: 0.1em 0.4em; border-radius: 3px; color: var(--accent); border: 1px solid var(--border); }
.slide.argument .callout { margin-top: 2.2rem; padding: 1rem 1.4rem; border-left: 3px solid var(--accent); background: var(--card); font-size: clamp(0.88rem, 1.4vw, 1.05rem); color: var(--muted); font-style: italic; max-width: 52ch; border-radius: 0 6px 6px 0; line-height: 1.6; }
.slide.argument .callout cite { display: block; margin-top: 0.5rem; font-style: normal; font-size: 0.82em; color: var(--accent); opacity: 0.7; }

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

**`argument`** — headline + bullet list. The main slide type for claims and points. Optional evidence callout below the list.
```html
<section class="slide argument" data-notes="optional speaker note">
  <h2><span class="accent-line"></span>One sharp, opinionated claim</h2>
  <ul>
    <li>First supporting point</li>
    <li>Second supporting point</li>
    <li>Third supporting point</li>
  </ul>
  <!-- optional callout: -->
  <blockquote class="callout">Supporting quote or data point.<cite>— Source</cite></blockquote>
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
