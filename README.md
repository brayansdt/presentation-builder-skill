# presentation-builder

A skill that turns notes, markdown, outlines, or transcripts into polished standalone HTML presentations.

## What it does

1. **Clarifies** the audience, purpose, slide count, tone, and source-specific intent when needed
2. **Builds** an opinionated slide narrative where each slide makes one clear claim
3. **Generates** a complete `presentation.html` file with embedded HTML, CSS, and JavaScript
4. **Runs locally** with no build tools, package managers, frameworks, CDNs, or external assets
5. **Supports** keyboard navigation, speaker notes, progress, and print-to-PDF export

## Install

### Claude Code
```bash
claude plugin marketplace add brayansdt/presentation-builder-skill
claude plugin install presentation-builder@presentation-builder-skill
```

### Gemini CLI
```bash
gemini extensions install https://github.com/brayansdt/presentation-builder-skill
```

### Codex / OpenClaw
```bash
git clone https://github.com/brayansdt/presentation-builder-skill
```
Then add to your project's `AGENTS.md`:
```
@./presentation-builder-skill/SKILL.md
```

### Cursor / Windsurf / Cline / Copilot
```bash
npx skills add brayansdt/presentation-builder-skill
```

---

Once installed, just say:

> "make slides from these notes"  
> "build a presentation about this topic"  
> "turn this transcript into a deck"  
> "create a pitch deck from this outline"

The skill triggers automatically.

## Output

The skill writes a single self-contained file:

```
presentation.html
```

Open it in any modern browser. Use the arrow keys to navigate, press `N` for speaker notes, and use browser print to save as PDF.

## Key behaviors

- **Asks targeted setup questions** unless the user asks it to make assumptions
- **Never invents statistics or quotes** when source material is vague
- **Keeps every slide focused** on one takeaway
- **Avoids external dependencies** so the presentation opens by double-clicking
- **Uses speaker notes** for detail that should not clutter the slide

## Requirements

- Any AI coding agent that can use local skill files
- A modern browser to view the generated HTML presentation
