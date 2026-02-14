---
name: slide-deck
description: Generates professional slide deck images from content. Creates outlines with style instructions, then generates individual slide images via AI image generation. Use when user asks to "create slides", "make a presentation", "generate deck", "slide deck", or "PPT".
---

# Slide Deck Generator

Transform content into professional slide deck images.

## Usage

```bash
/slide-deck path/to/content.md
/slide-deck path/to/content.md --style corporate
/slide-deck path/to/content.md --audience executives
/slide-deck path/to/content.md --slides 15
/slide-deck  # Then paste content
```

## Script Directory

**Agent Execution Instructions**:
1. Determine this SKILL.md file's directory path as `SKILL_DIR`
2. Script path = `${SKILL_DIR}/scripts/<script-name>.ts`

| Script | Purpose |
|--------|---------|
| `scripts/merge-to-pptx.ts` | Merge slide images into PowerPoint |
| `scripts/merge-to-pdf.ts` | Merge slide images into PDF |

## Options

| Option | Description |
|--------|-------------|
| `--style <name>` | Visual style preset or `custom` |
| `--audience <type>` | beginners, intermediate, experts, executives, general |
| `--lang <code>` | Output language (en, zh, ja, ko, etc.) |
| `--slides <number>` | Target slide count |
| `--outline-only` | Generate outline only, skip images |
| `--prompts-only` | Generate outline + prompts, skip images |
| `--images-only` | Generate images from existing prompts/ directory |
| `--regenerate <N>` | Regenerate specific slide(s): `3` or `2,5,8` |

## Workflow

```
Input → Analyze → Confirm → Outline → [Review?] → Prompts → [Review?] → Images → Merge → Done
```

### Step 1: Analyze Content

1. Save source content (if pasted, save as `source.md`)
2. Extract core message (one sentence, <=15 words)
3. Identify 3-5 supporting points
4. Detect content signals for style recommendation:

| Content Signals | Recommended Style |
|-----------------|-------------------|
| tutorial, learn, education, guide | `sketch-notes` |
| architecture, system, data, technical | `blueprint` |
| investor, quarterly, business, corporate | `corporate` |
| executive, minimal, clean, simple | `minimal` |
| launch, marketing, keynote, magazine | `bold-editorial` |
| entertainment, music, gaming | `dark-atmospheric` |
| Default | `blueprint` |

5. Determine slide count by content length:

| Content Length | Slides |
|---------------|--------|
| < 1000 words | 5-10 |
| 1000-3000 words | 10-18 |
| 3000-5000 words | 15-25 |
| > 5000 words | 20-30 |

6. Detect source language, generate topic slug (2-4 words, kebab-case)
7. Check existing: `test -d "slide-deck/{topic-slug}" && echo "exists"`

### Step 2: Confirmation

Use AskUserQuestion for all questions at once:

**Q1: Style** → Recommended preset + alternative + "Custom dimensions" option
**Q2: Audience** → General (default), Beginners, Experts, Executives
**Q3: Slide Count** → Recommended, Fewer (N-3), More (N+3)
**Q4: Review Outline?** → Yes (recommended) / No
**Q5: Review Prompts?** → Yes (recommended) / No

If "Custom dimensions" selected, Round 2: ask Texture, Mood, Typography, Density.

### Step 3: Generate Outline

Save as `slide-deck/{topic-slug}/outline.md`. The outline has two parts:

**Part 1: `<STYLE_INSTRUCTIONS>` block** — Single Source of Truth for style across all prompts. Build from the selected preset or custom dimensions. Contains: Design Aesthetic, Background, Typography, Color Palette (hex codes), Visual Elements, Density Guidelines, Style Rules (Do/Don't).

**Part 2: Slide entries** — Each slide has these sections:

```markdown
## Slide X of N

**Type**: Cover | Content | Back Cover
**Filename**: NN-slide-{slug}.png

// NARRATIVE GOAL
[What this slide achieves in the story arc]

// KEY CONTENT
Headline: [narrative headline, not label]
Sub-headline: [supporting context]
Body:
- [point with specific detail]
- [point with specific detail]

// VISUAL
[Detailed visual description - specific elements, composition, mood]

// LAYOUT
Layout: [layout name from gallery]
[Composition and spatial arrangement]
```

**Outline Rules**:
- Headlines are narrative ("Usage doubled in 6 months"), not labels ("Key Statistics")
- Each slide = ONE clear message
- Every detail fully specified, no placeholders
- Back Cover: meaningful close (CTA, takeaway), not just "Thank you"

### Step 4: Review Outline (Conditional)

Skip if user chose "No" in Q4. Display slide summary table → proceed / edit / regenerate.

### Step 5: Generate Prompts

For each slide in outline:
1. Read `${SKILL_DIR}/base-prompt.md` (image generation template)
2. Replace `[STYLE_INSTRUCTIONS_HERE]` with `<STYLE_INSTRUCTIONS>` block from outline
3. Replace `[SLIDE_CONTENT_HERE]` with the slide's content section
4. Replace `[LANGUAGE_HERE]` with target language
5. Save to `slide-deck/{topic-slug}/prompts/NN-slide-{slug}.md`

### Step 6: Review Prompts (Conditional)

Skip if user chose "No" in Q5.

### Step 7: Generate Images

For each prompt file in `prompts/`:
```bash
# Configure your image generation backend here
npx -y bun path/to/image-gen.ts --promptfiles prompts/NN-slide-{slug}.md --image NN-slide-{slug}.png
```
Generate sequentially, report "Generated X/N", auto-retry once on failure.

### Step 8: Merge

```bash
npx -y bun ${SKILL_DIR}/scripts/merge-to-pptx.ts slide-deck/{topic-slug}/
npx -y bun ${SKILL_DIR}/scripts/merge-to-pdf.ts slide-deck/{topic-slug}/
```

### Step 9: Summary

Report topic, style, location, file list.

## Style Presets Quick Reference

| Preset | Texture | Mood | Typography | Density |
|--------|---------|------|------------|---------|
| `blueprint` | grid | cool | technical | balanced |
| `corporate` | clean | professional | geometric | balanced |
| `minimal` | clean | neutral | geometric | minimal |
| `sketch-notes` | organic | warm | handwritten | balanced |
| `bold-editorial` | clean | vibrant | editorial | balanced |
| `dark-atmospheric` | clean | dark | editorial | balanced |

## Layout Gallery

| Layout | Best For |
|--------|----------|
| `title-hero` | Cover slides, section breaks |
| `split-screen` | Comparisons, feature highlights |
| `icon-grid` | Features, capabilities |
| `two-columns` / `three-columns` | Paired/triple info |
| `key-stat` | Single impactful metric |
| `quote-callout` | Testimonials, key insights |
| `bullet-list` | Simple content |
| `linear-progression` | Timelines, steps |
| `binary-comparison` | Before/after, pros-cons |
| `hub-spoke` | Concept maps, ecosystems |
| `dashboard` | KPIs, data display |
| `funnel` | Conversion stages |
| `winding-roadmap` | Journey, milestones |
| `hierarchical-layers` | Priority, importance |

## File Structure

```
slide-deck/{topic-slug}/
├── source-{slug}.{ext}
├── outline.md
├── prompts/
│   ├── 01-slide-cover.md
│   ├── 02-slide-{slug}.md
│   └── ...
├── 01-slide-cover.png
├── 02-slide-{slug}.png
├── {topic-slug}.pptx
└── {topic-slug}.pdf
```
