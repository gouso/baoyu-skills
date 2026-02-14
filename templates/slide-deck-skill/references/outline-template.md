# Outline Template

Standard structure for slide deck outlines with style instructions.

## Outline Format

```markdown
# Slide Deck Outline

**Topic**: [topic description]
**Style**: [preset name OR "custom"]
**Dimensions**: [texture] + [mood] + [typography] + [density]
**Audience**: [target audience]
**Language**: [output language]
**Slide Count**: N slides
**Generated**: YYYY-MM-DD HH:mm

---

<STYLE_INSTRUCTIONS>
Design Aesthetic: [2-3 sentence description combining dimension characteristics]

Background:
  Texture: [from texture dimension]
  Base Color: [from mood dimension palette]

Typography:
  Headlines: [from typography dimension - describe visual appearance]
  Body: [from typography dimension - describe visual appearance]

Color Palette:
  Primary Text: [Name] ([Hex]) - [usage]
  Background: [Name] ([Hex]) - [usage]
  Accent 1: [Name] ([Hex]) - [usage]
  Accent 2: [Name] ([Hex]) - [usage]

Visual Elements:
  - [element 1 from texture + mood combination]
  - [element 2 with rendering guidance]
  - ...

Density Guidelines:
  - Content per slide: [from density dimension]
  - Whitespace: [from density dimension]

Style Rules:
  Do: [guidelines from dimension combinations]
  Don't: [anti-patterns from dimension combinations]
</STYLE_INSTRUCTIONS>

---

[Slide entries follow...]
```

## Building STYLE_INSTRUCTIONS from Dimensions

Combine characteristics from all four dimensions:

### 1. Design Aesthetic

| Texture | Contribution |
|---------|--------------|
| clean | "Clean, digital precision with crisp edges" |
| grid | "Technical grid overlay with engineering precision" |
| organic | "Hand-drawn feel with soft textures" |
| pixel | "Chunky pixel aesthetic with 8-bit charm" |
| paper | "Aged paper texture with vintage character" |

| Mood | Contribution |
|------|--------------|
| professional | "Professional navy and gold palette" |
| warm | "Warm earth tones creating approachable atmosphere" |
| cool | "Cool analytical blues and grays" |
| vibrant | "Bold, high-saturation colors with energy" |
| dark | "Deep cinematic backgrounds with glowing accents" |
| neutral | "Minimal grayscale sophistication" |

### 2. Typography

**Important**: Describe visual appearance for image generation:
- "bold geometric sans-serif with perfect circular O shapes" NOT "Inter font"
- "rounded humanist serif with warm curves" NOT "Georgia font"

### 3. Color Palette

From mood dimension. Include hex codes and usage notes for each role.

### 4. Style Rules

Combine dimension-specific Do/Don't rules.

## Slide Templates

### Cover Slide

```markdown
## Slide 1 of N

**Type**: Cover
**Filename**: 01-slide-cover.png

// NARRATIVE GOAL
[What this slide achieves in the story arc]

// KEY CONTENT
Headline: [main title]
Sub-headline: [supporting tagline]

// VISUAL
[Detailed visual description - specific elements, composition, mood]

// LAYOUT
Layout: [optional: layout name from gallery, e.g., title-hero]
[Composition, hierarchy, spatial arrangement]
```

### Content Slide

```markdown
## Slide X of N

**Type**: Content
**Filename**: {NN}-slide-{slug}.png

// NARRATIVE GOAL
[What this slide achieves in the story arc]

// KEY CONTENT
Headline: [main message - narrative, not label]
Sub-headline: [supporting context]
Body:
- [point 1 with specific detail]
- [point 2 with specific detail]
- [point 3 with specific detail]

// VISUAL
[Detailed visual description]

// LAYOUT
Layout: [optional: layout name from gallery]
[Composition, hierarchy, spatial arrangement]
```

### Back Cover Slide

```markdown
## Slide N of N

**Type**: Back Cover
**Filename**: {NN}-slide-back-cover.png

// NARRATIVE GOAL
[Meaningful closing - not just "thank you"]

// KEY CONTENT
Headline: [memorable closing statement or call-to-action]
Body: [optional summary points or next steps]

// VISUAL
[Visual that reinforces the core message]

// LAYOUT
Layout: [optional: layout name from gallery]
[Clean, impactful composition]
```

## Key Rules

- `<STYLE_INSTRUCTIONS>` block is the SINGLE SOURCE OF TRUTH for style
- Typography descriptions must describe visual appearance (not font names)
- Prompts extract STYLE_INSTRUCTIONS from outline, NOT re-read style files
- Cover is always Slide 1, Back Cover is always final slide
- Filename prefix matches slide position: `01-`, `02-`, etc.
- Slugs: kebab-case, max 30 chars, unique within deck
