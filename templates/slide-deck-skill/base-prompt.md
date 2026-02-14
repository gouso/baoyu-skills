Create a single presentation slide image.

---

## Image Specifications

- **Format**: Presentation slide, 16:9 landscape
- **Rendering**: Illustrated / hand-crafted quality — NOT photographic, NOT 3D render, NOT stock photo
- **Output**: One complete slide image ready for a deck

---

## Visual Design System

### Composition & Layout

Every slide follows a strict visual hierarchy:

1. **Focal Point**: ONE dominant element draws the eye first. This is the headline or key visual — never both competing equally.
2. **Z-Pattern Reading Flow**: Position elements so the eye travels: top-left (headline) → top-right (supporting visual) → bottom-left (body text) → bottom-right (accent/CTA).
3. **Rule of Thirds**: Divide the slide into a 3×3 grid. Place key elements at intersection points, not dead center (exception: cover slides may center).
4. **Breathing Room**: Minimum 8% margin from all edges. Elements never touch the slide boundary. Between elements, maintain spacing equal to at least 1.5× the body text size.
5. **Visual Weight Balance**: If the left side has a heavy visual (chart, icon cluster), balance the right with text or whitespace — never stack heavy elements on one side.

### Layout Types

When a layout is specified in `// LAYOUT`, follow these rules:

**`title-hero`** — Cover slides, section breaks
- Headline dominates 60%+ of visual space
- Sub-headline below, smaller by 40-50%
- Optional: abstract visual elements in background or bottom third
- Center-aligned or left-aligned with right visual

**`split-screen`** — Two concepts side by side
- Exact 50/50 or 60/40 vertical split
- Left: text content / Right: visual (or vice versa)
- Clear divider: color block, thin line, or whitespace gap
- Both halves have their own visual hierarchy

**`icon-grid`** — Features, capabilities, benefits
- 2×2, 2×3, or 3×3 grid of icon+label pairs
- Icons same size, same style, evenly spaced
- Labels directly below or beside each icon
- Grid centered in slide with generous outer margins

**`two-columns`** — Paired information
- Two equal columns with consistent formatting
- Column headers in accent color or bold
- Same number of items per column when possible
- Clear gutter (whitespace) between columns

**`three-columns`** — Triple comparison
- Three equal columns
- Visual or number at top of each column
- Supporting text below
- Highlight one column (accent border or background) if there's a recommended option

**`key-stat`** — Single impactful number
- One large number (60-80pt equivalent) as focal point
- Unit or label in smaller text beside/below the number
- Contextual sentence in body text
- Number in accent color, rest in primary text color

**`quote-callout`** — Testimonials, key insights
- Large quotation marks as decorative element
- Quote text in larger-than-body size, slightly different style (italic or lighter weight)
- Attribution below in smaller text with em-dash
- Generous whitespace around quote

**`bullet-list`** — Structured content
- Headline at top (20% of space)
- 3-5 bullet points with consistent markers
- Each bullet: bold lead phrase + supporting detail
- Left-aligned, consistent indentation

**`linear-progression`** — Timeline, steps
- Horizontal flow: left to right
- Connected nodes (circles, arrows, or stepping stones)
- Labels above or below each node
- Progress indicator (color change or size growth)

**`binary-comparison`** — Before/after, pros/cons
- Clear A vs B layout
- "VS" or divider in center
- Matching structure on both sides
- Color coding: warm for A, cool for B (or vice versa)

**`hub-spoke`** — Concept maps, ecosystems
- Central node (largest, accent colored)
- Radiating connections to 4-8 outer nodes
- Lines/arrows showing relationships
- Labels on or beside each node

**`dashboard`** — KPIs, data display
- 3-6 metric cards in bento-style grid
- Each card: number + label + optional sparkline
- Varied card sizes for importance hierarchy
- Clean borders or subtle shadows between cards

**`funnel`** — Conversion, filtering stages
- Trapezoidal or triangular narrowing shape
- 3-5 stages, labeled on or beside each level
- Numbers/percentages showing reduction
- Color gradient from top (light) to bottom (dark/accent)

**`hierarchical-layers`** — Priority, importance
- Pyramid or stacked horizontal bars
- Top = most important, bottom = base/foundation
- Clear level labels
- Color intensity increases toward top

**`winding-roadmap`** — Journey, milestones
- S-curve or winding path from left to right (or top to bottom)
- Milestone markers along the path
- Labels beside each milestone
- Current position highlighted

### Typography Rules

Text rendering must match the style aesthetic. These rules apply to ALL styles:

**Headline Treatment**:
- 2.5-3× larger than body text
- Bold or semi-bold weight
- Max 2 lines, ideally 1 line
- Narrative tone: tells the story, doesn't label ("Teams save 10 hours weekly" not "Benefits")

**Body Text Treatment**:
- Regular weight, comfortable reading size
- Max 4-5 lines for balanced density, 2-3 for minimal
- Line height: 1.4-1.6× font size
- Left-aligned (never justified in slides)

**Data & Numbers**:
- Key statistics: 2-3× body text size, in accent color
- Units/labels: 60% of the number's size, in secondary text color
- Use tabular/monospace rendering for aligned numbers

**Text Hierarchy** (size ratio from largest to smallest):
```
Headline ████████████████ (100%)
Sub-headline ██████████ (60-70%)
Body text ██████ (40-45%)
Caption/label ████ (30-35%)
```

**Prohibited**:
- No more than 3-4 distinct text elements per slide
- No paragraphs longer than 3 lines
- No text smaller than caption size
- No decorative fonts that sacrifice readability

### Color Application

Colors from the STYLE_INSTRUCTIONS palette must be applied consistently:

| Element | Color Role |
|---------|-----------|
| Background | Background color (solid, no gradients unless style specifies) |
| Headlines | Primary Text color |
| Body text | Secondary Text color |
| Key numbers, highlights | Accent 1 color |
| Secondary accents, borders | Accent 2 color |
| Positive indicators | Success/green (if in palette) |
| Negative indicators | Alert/red (if in palette) |
| Section backgrounds | Neutral color at 20-40% opacity |

**Color Rules**:
- Maximum 4 colors visible on any single slide (excluding grayscale)
- Accent color used for ONE key element per slide — never everything
- Background color covers 60%+ of slide area
- Ensure WCAG AA contrast between text and background (4.5:1 minimum)
- If dark background: use light text. If light background: use dark text. Never low-contrast combos.

### Visual Elements

All visual elements must match the style aesthetic:

**Charts & Data Viz**:
- Simple and clear — no 3D effects, no unnecessary gridlines
- Label data directly (no legends if possible)
- Use accent colors for the key data point, muted colors for context
- Bar charts: horizontal for comparisons, vertical for time series
- Max 5-7 data points per chart

**Icons & Illustrations**:
- Consistent style throughout entire deck (all outlined OR all filled OR all hand-drawn)
- Same stroke width, same level of detail
- Sized consistently: all icons same dimensions within a layout
- Never mix icon styles (no outlined + filled on same slide)

**Decorative Elements**:
- Subtle — never compete with content for attention
- Consistent with texture dimension (grid lines for `grid`, brush strokes for `organic`, etc.)
- Corner accents, section dividers, or background patterns only
- Must enhance readability, never reduce it

**Connectors & Arrows**:
- Consistent style (straight, curved, or hand-drawn — match texture)
- Clear directionality
- Color: primary text at 40-60% opacity, or accent color for emphasis

### Density-Specific Rendering

**minimal density**:
- ONE idea, ONE visual focus
- Headline + 0-2 lines max
- 50%+ of slide is whitespace
- Let the single element breathe

**balanced density**:
- 2-3 key points
- Headline + 3-5 supporting items
- 30-40% whitespace
- Structured but not crowded

**dense density**:
- Multiple data points, annotations
- Headline + multiple sections
- 15-25% whitespace
- Every element earns its space, no decoration for decoration's sake

---

## Absolute Rules (Never Violate)

1. **NO slide numbers, page numbers, footers, headers, or logos** — ever
2. **NO photographic or realistic imagery** — illustrated/stylized only
3. **NO AI cliche language** in text: "dive into", "explore", "journey", "exciting", "revolutionary"
4. **ALL text must be readable** — if you can't read it at normal viewing distance, it's too small
5. **ONE language only** per slide — match the content language below
6. **Self-contained** — every detail in this prompt, no references to "previous slide" or "like slide 2"
7. **Every text element fully specified** — no "[insert here]", no "TBD", no placeholders
8. If content involves sensitive or copyrighted figures, create **stylistically similar alternatives** — DO NOT refuse to generate

---

## STYLE_INSTRUCTIONS

[STYLE_INSTRUCTIONS_HERE]

---

## SLIDE CONTENT

[SLIDE_CONTENT_HERE]

---

## Language

All text elements on this slide MUST be in: [LANGUAGE_HERE]

---

Generate the slide image based on all specifications above.
