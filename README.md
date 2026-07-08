# PPT Builder Skill

**Turn ideas into polished, editable presentations in minutes — not hours.**

An AI assistant workflow skill for turning scripts, outlines, training materials, product notes, or minimal briefs into **production-ready, editable PowerPoint presentations**. No design experience needed.

---

## Why This Skill Exists

Creating presentation decks is a bottleneck:
- **Designers** spend hours on layout and visual hierarchy
- **Subject matter experts** waste time on formatting instead of content
- **Teams** can't iterate quickly — each revision means manual rework
- **Non-designers** avoid presentations because the tooling is intimidating

This skill solves it: **give Claude your idea → get a complete, layered PPTX in 20 minutes**.

---

## What It Does

The skill guides you through a **6-step workflow**:

1. **Extract outline** — Parse source material into a slide-by-slide plan
2. **Confirm storyboard** — Review and adjust page flow before design starts
3. **Define visual style** — Set tone, color, typography, imagery approach
4. **Generate PPTX** — Auto-build using `pptxgenjs` patterns
5. **Visual QA** — Render slides and review for layout issues
6. **Deliver final deck** — Hand off an editable, layer-organized PPTX

This confirmation-heavy flow is intentional. Presentations have subjective design choices — confirming early prevents full-deck rewrites.

---

## Real-World Example: Product Launch Deck

**Project:** Female wellness product launch presentation (丁教授女性私护指南)

| Metric | Result |
|--------|--------|
| **Slide count** | 22 slides |
| **Skill automation** | 90% (outline → design → layout) |
| **Manual effort** | 10% (copywriting polish) |
| **Skill execution time** | ~20 minutes |
| **Total project time** | 1 hour 20 minutes |
| **Use case** | B2B/B2C product pitch to customers |
| **Output** | Delivered to real clients ✓ |

**Time saved:** Traditional hand-design of a 22-slide deck averages 4–8 hours. This approach: **67–85% faster**.

**The workflow:** Minimal input (product brief + 3 key messages) → Claude follows the skill → Complete deck with professional design, proper typography, image spacing, and ready-to-edit text layers.

---

## Workflow Details

### Input Formats
- Speech scripts
- Bulleted outlines
- Product briefs
- Training documentation
- Strategic pitches
- Any text describing your idea

### Output
- **Editable `.pptx`** with text layers, image placeholders, grouped design elements
- **Slide renderings (PNG)** for QA review
- **Build script (`*.js`)** for reproducibility and tweaking

### Customization Examples
```
"Create a dark, tech-forward product launch deck..."
"Turn this fundraising one-pager into a 15-slide investor deck..."
"Make an educational slide deck with warm colors and illustrations..."
```

The skill adapts design, tone, and layout to your brief.

---

## Installation

### Codex-Style Folder Skills
```bash
mkdir -p ~/.codex/skills/ppt-builder
cp SKILL.md ~/.codex/skills/ppt-builder/SKILL.md
```

### Other AI Coding Agents
Keep `SKILL.md` available and ask the agent to follow it when building presentations.

### Dependencies

| Tool | Purpose |
|------|---------|
| `pptxgenjs` (npm) | Generate editable PPTX files |
| Python 3 + Pillow | Preprocess and validate images |
| `markitdown` | Extract text from source documents |
| LibreOffice or PowerPoint | Export rendered slides for QA |
| Vision-capable AI (optional) | Automated visual QA of layouts |

---

## Example Prompts

```
Use the PPT builder skill to turn this product launch brief into a 20-slide pitch deck with modern, minimal design.
```

```
Use the PPT builder skill to convert this training manual into a 12-slide educational presentation. Make it warm and approachable.
```

```
Use the PPT builder skill to create an investor deck from this startup one-pager. Use professional, data-forward styling.
```

```
Take this customer case study and build it into a 15-slide success story deck using brand colors and typography guidelines I'll provide.
```

---

## Project Structure

```
your-ppt-project/
├── material/              # Source assets (logos, screenshots, product images)
├── source.md              # Your input: outline, script, or brief
├── build_deck_v1.js       # Generated pptxgenjs build script
├── slides_v1/             # Rendered slide images (PNG) for QA
└── output_v1.pptx         # Final editable PowerPoint deck
```

---

## Design Principles

- **Editable, not image-only** — All output is layered PPTX with real text, not flattened screenshots
- **Iterative by default** — Built for 2–3 review cycles, not one-shot generation
- **Guardrails included** — Image sizing rules, text overflow checks, readability validation
- **Minimal dependencies** — Works in any environment with Node.js + Python
- **Customizable output** — Adapt design, tone, and layout through brief changes

---

## Known Strengths & Limitations

### ✅ Strengths
- **Speed** — 20-minute delivery from brief to presentation
- **Accessibility** — Non-designers can create professional decks
- **Editability** — Full layer control; easy post-generation tweaks
- **Scalability** — Same workflow scales from 8 to 50+ slides
- **Versatility** — Works across industries (product, education, sales, internal comms)

### ⚠️ Considerations
- **AI copywriting tone** — Generated text can sound "AI-like"; 10–30 min of human polish recommended
- **Image sourcing** — Works best when you provide reference images or detailed descriptions
- **Custom branding** — Requires clear design briefs; complex brand guidelines need review cycles

---

## Community & Contribution

This is a **living workflow**. If you:
- Use it with interesting results, share your output (anonymized) as a case study
- Find edge cases or pain points, open an issue
- Extend the skill (add 3D elements, add video intros, etc.), submit a PR

---

## License

MIT — Use freely in personal projects, commercial work, and adapted workflows.

---

## Next Steps

1. **Install** the skill into your Claude Code environment
2. **Test** with a simple 5-slide brief
3. **Iterate** — the confirmation steps mean you're never surprised by the output
4. **Share** — if you get great results, let the community know how you used it

---

*This skill was created for Claude Code and Codex agents, but adapts to any AI assistant that can follow structured instruction files.*
