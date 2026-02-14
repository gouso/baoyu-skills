---
name: slide-deck
description: Generates professional slide deck images from content. Creates outlines with style instructions, then generates individual slide images. Use when user asks to "create slides", "make a presentation", "generate deck", "slide deck", or "PPT".
---

# Slide Deck Generator

Transform content into professional slide deck images.

## Usage

```bash
/slide-deck path/to/content.md
/slide-deck path/to/content.md --style sketch-notes
/slide-deck path/to/content.md --audience executives
/slide-deck path/to/content.md --lang zh
/slide-deck path/to/content.md --slides 10
/slide-deck path/to/content.md --outline-only
/slide-deck  # Then paste content
```

## Script Directory

**Agent Execution Instructions**:
1. Determine this SKILL.md file's directory path as `SKILL_DIR`
2. Script path = `${SKILL_DIR}/scripts/<script-name>.ts`

| Script | Purpose |
|--------|---------|
| `scripts/merge-to-pptx.ts` | Merge slides into PowerPoint |
| `scripts/merge-to-pdf.ts` | Merge slides into PDF |

## Options

| Option | Description |
|--------|-------------|
| `--style <name>` | Visual style: preset name, `custom`, or custom style name |
| `--audience <type>` | Target: beginners, intermediate, experts, executives, general |
| `--lang <code>` | Output language (en, zh, ja, ko, etc.) |
| `--slides <number>` | Target slide count (8-25 recommended, max 30) |
| `--outline-only` | Generate outline only, skip image generation |
| `--prompts-only` | Generate outline + prompts, skip images |
| `--images-only` | Generate images from existing prompts directory |
| `--regenerate <N>` | Regenerate specific slide(s): `--regenerate 3` or `--regenerate 2,5,8` |

**Slide Count by Content Length**:
| Content | Slides |
|---------|--------|
| < 1000 words | 5-10 |
| 1000-3000 words | 10-18 |
| 3000-5000 words | 15-25 |
| > 5000 words | 20-30 (consider splitting) |

## Style System

### Presets

| Preset | Dimensions | Best For |
|--------|------------|----------|
| `blueprint` (Default) | grid + cool + technical + balanced | Architecture, system design |
| `corporate` | clean + professional + geometric + balanced | Investor decks, proposals |
| `minimal` | clean + neutral + geometric + minimal | Executive briefings |
| `sketch-notes` | organic + warm + handwritten + balanced | Educational, tutorials |
| `dark-atmospheric` | clean + dark + editorial + balanced | Entertainment, gaming |
| `bold-editorial` | clean + vibrant + editorial + balanced | Product launches, keynotes |

### Style Dimensions

| Dimension | Options | Description |
|-----------|---------|-------------|
| **Texture** | clean, grid, organic, pixel, paper | Visual texture and background treatment |
| **Mood** | professional, warm, cool, vibrant, dark, neutral | Color temperature and palette style |
| **Typography** | geometric, humanist, handwritten, editorial, technical | Headline and body text styling |
| **Density** | minimal, balanced, dense | Information density per slide |

Full specs: `references/dimensions/*.md`

### Auto Style Selection

| Content Signals | Preset |
|-----------------|--------|
| tutorial, learn, education, guide | `sketch-notes` |
| architecture, system, data, technical | `blueprint` |
| executive, minimal, clean | `minimal` |
| investor, quarterly, business, corporate | `corporate` |
| launch, marketing, keynote | `bold-editorial` |
| entertainment, music, gaming | `dark-atmospheric` |
| Default | `blueprint` |

## Design Philosophy

Decks designed for **reading and sharing**, not live presentation:
- Each slide self-explanatory without verbal commentary
- Logical flow when scrolling
- All necessary context within each slide
- Optimized for social media sharing

See `references/design-guidelines.md` for:
- Audience-specific principles
- Visual hierarchy
- Content density guidelines
- Color and typography selection

See `references/layouts.md` for layout options.

## File Management

### Output Directory

```
slide-deck/{topic-slug}/
├── source-{slug}.{ext}
├── outline.md
├── prompts/
│   └── 01-slide-cover.md, 02-slide-{slug}.md, ...
├── 01-slide-cover.png, 02-slide-{slug}.png, ...
├── {topic-slug}.pptx
└── {topic-slug}.pdf
```

## Language Handling

**Detection Priority**:
1. `--lang` flag (explicit)
2. User's conversation language
3. Source content language

## Workflow

```
Slide Deck Progress:
- [ ] Step 1: Setup & Analyze
  - [ ] 1.1 Analyze content
  - [ ] 1.2 Check existing
- [ ] Step 2: Confirmation (style, audience, slides, review preferences)
- [ ] Step 3: Generate outline
- [ ] Step 4: Review outline (conditional)
- [ ] Step 5: Generate prompts
- [ ] Step 6: Review prompts (conditional)
- [ ] Step 7: Generate images
- [ ] Step 8: Merge to PPTX/PDF
- [ ] Step 9: Output summary
```

### Flow

```
Input → Analyze → Confirm (1-2 rounds) → Outline → [Review?] → Prompts → [Review?] → Images → Merge → Complete
```

### Step 1: Setup & Analyze

**1.1 Analyze Content**

1. Save source content (if pasted, save as `source.md`)
2. Follow `references/analysis-framework.md` for content analysis
3. Analyze content signals for style recommendations
4. Detect source language
5. Determine recommended slide count
6. Generate topic slug from content

**1.2 Check Existing Content**

Use Bash to check if output directory exists:

```bash
test -d "slide-deck/{topic-slug}" && echo "exists"
```

**If directory exists**, use AskUserQuestion:

```
header: "Existing"
question: "Existing content found. How to proceed?"
options:
  - label: "Regenerate outline"
    description: "Keep images, regenerate outline only"
  - label: "Regenerate images"
    description: "Keep outline, regenerate images only"
  - label: "Backup and regenerate"
    description: "Backup to {slug}-backup-{timestamp}, then regenerate all"
  - label: "Exit"
    description: "Cancel, keep existing content unchanged"
```

**Save to `analysis.md`** with:
- Topic, audience, content signals
- Recommended style (based on Auto Style Selection)
- Recommended slide count
- Language detection

### Step 2: Confirmation

**Two-round confirmation**: Round 1 always, Round 2 only if "Custom dimensions" selected.

#### Round 1 (Always)

**Use AskUserQuestion** for all 5 questions:

**Q1: Style** → Recommended preset + alternative + custom dimensions option
**Q2: Audience** → General, Beginners, Experts, Executives
**Q3: Slide Count** → Recommended ± 3
**Q4: Review Outline** → Yes/No
**Q5: Review Prompts** → Yes/No

#### Round 2 (Only if "Custom dimensions" selected)

**Use AskUserQuestion** for 4 dimensions: Texture, Mood, Typography, Density

### Step 3: Generate Outline

1. Read style spec: `references/styles/{preset}.md` or combine from `references/dimensions/`
2. Follow `references/outline-template.md` for structure
3. Build `<STYLE_INSTRUCTIONS>` block (single source of truth for style)
4. Apply confirmed audience, language, slide count
5. Save as `outline.md`

### Step 4: Review Outline (Conditional)

**Skip** if user selected "No, skip outline review" in Step 2.

Display slide-by-slide summary table → Ask proceed/edit/regenerate.

### Step 5: Generate Prompts

For each slide in outline:
1. Read `references/base-prompt.md` (template)
2. Copy `<STYLE_INSTRUCTIONS>` from outline (NOT from style file again)
3. Add slide-specific content (headline, body, visual, layout)
4. Save to `prompts/NN-slide-{slug}.md`

**This is the key step**: base-prompt template + STYLE_INSTRUCTIONS + slide content = final image generation prompt.

### Step 6: Review Prompts (Conditional)

**Skip** if user selected "No, skip prompt review" in Step 2.

### Step 7: Generate Images

**IMAGE GENERATION BACKEND**: This skill requires an image generation tool/API.

Configure your image generation command in this section:

```bash
# Example: using a local image generation script
npx -y bun path/to/image-gen.ts --promptfiles prompts/01-slide-cover.md --image 01-slide-cover.png

# Example: using an API-based generator
# Adapt to your available image generation tool
```

**Flow**:
1. For each slide prompt file in `prompts/`:
   - Generate image sequentially
   - Report progress: "Generated X/N"
   - Auto-retry once on failure
2. All images saved as `NN-slide-{slug}.png`

### Step 8: Merge to PPTX and PDF

```bash
npx -y bun ${SKILL_DIR}/scripts/merge-to-pptx.ts <slide-deck-dir>
npx -y bun ${SKILL_DIR}/scripts/merge-to-pdf.ts <slide-deck-dir>
```

### Step 9: Output Summary

```
Slide Deck Complete!

Topic: [topic]
Style: [preset name or custom dimensions]
Location: [directory path]
Slides: N total

- 01-slide-cover.png - Cover
- 02-slide-intro.png - Content
- ...
- {NN}-slide-back-cover.png - Back Cover

Outline: outline.md
PPTX: {topic-slug}.pptx
PDF: {topic-slug}.pdf
```

## Partial Workflows

| Option | Workflow |
|--------|----------|
| `--outline-only` | Steps 1-3 only |
| `--prompts-only` | Steps 1-5 |
| `--images-only` | Skip to Step 7 |
| `--regenerate N` | Regenerate specific slide(s) only |

## Slide Modification

| Action | Steps |
|--------|-------|
| **Edit** | Update prompt → `--regenerate N` → Regenerate PDF |
| **Add** | Create prompt → Generate image → Renumber → Update outline → Regenerate PDF |
| **Delete** | Remove files → Renumber → Update outline → Regenerate PDF |

## References

| File | Content |
|------|---------|
| `references/analysis-framework.md` | Content analysis for presentations |
| `references/outline-template.md` | Outline structure and format |
| `references/content-rules.md` | Content and style guidelines |
| `references/design-guidelines.md` | Audience, typography, colors |
| `references/layouts.md` | Layout options and selection tips |
| `references/base-prompt.md` | Base prompt for image generation |
| `references/dimensions/*.md` | Dimension specifications |
| `references/dimensions/presets.md` | Preset to dimension mapping |
| `references/styles/<style>.md` | Full style specifications |
