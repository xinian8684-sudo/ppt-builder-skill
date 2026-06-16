# PPT Builder Skill

An AI assistant workflow skill for turning scripts, outlines, training material, or product notes into editable PowerPoint-oriented presentations.

The skill is written as a portable `SKILL.md` workflow. It can be used with Codex-style folder skills or adapted for other coding agents that can follow a skill instruction file.

## What It Does

- Turns a speech script, document, or outline into a slide-by-slide deck plan
- Guides the user through outline, storyboard, design system, generation, visual QA, and delivery
- Uses `pptxgenjs`-oriented generation patterns for editable `.pptx` output
- Includes guardrails for image sizing and visual QA so generated decks avoid obvious layout mistakes

## Workflow

The skill follows a six-step process:

1. Extract an outline from the source material
2. Confirm a page-by-page storyboard
3. Define the visual style and design system
4. Generate the PPTX build script
5. Render slides for visual QA
6. Fix layout issues and deliver the final deck

This confirmation-heavy flow is intentional. Presentation work has many subjective design decisions, and confirming the outline and storyboard early prevents full-deck rewrites later.

## Install

For Codex-style folder skills, create a skill folder and copy `SKILL.md` into it:

```bash
mkdir -p ~/.codex/skills/ppt-builder
cp SKILL.md ~/.codex/skills/ppt-builder/SKILL.md
```

For other AI coding-agent workflows, keep `SKILL.md` available as the instruction file and ask the agent to follow it when building a presentation.

## Example Prompts

```text
Use the PPT builder skill to turn this training document into a 15-slide presentation.
```

```text
Use the PPT builder skill to create a dark, technology-style product launch deck from this outline.
```

```text
Use the PPT builder skill to make an editable PPTX based on this speech script.
```

## Dependencies

The exact tools depend on the environment and the final deck workflow:

| Tool | Purpose |
|---|---|
| `pptxgenjs` | Generate editable PPTX files |
| Python 3 + Pillow | Preprocess images |
| `markitdown` | Extract text from source documents |
| LibreOffice or PowerPoint | Export slides for visual QA |
| Vision-capable model or manual review | Check slide rendering and layout |

## Typical Project Structure

```text
your-ppt-project/
  material/          # Source assets such as logos, screenshots, product images
  build_deck_v1.js   # PPTX generation script
  slide_v1/          # Rendered slide images for QA
  output_v1.pptx     # Final editable deck
```

## Notes

- This project is a practical workflow skill, not a hosted PPT generation service.
- Visual QA depends on the tools available in your environment. Use rendered slide screenshots and either a vision-capable model or careful manual review.
- The skill is designed for editable output and iterative review, not one-shot image-only slide generation.

## License

MIT
