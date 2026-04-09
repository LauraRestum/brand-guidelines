# Envision Brand Book — PowerPoint Build Spec

**Deliverable:** `deliverables/envision-brand-book-2025.pptx`
**Source:** This repo (brand-guidelines, deployed at https://brand-guidelines-nine.vercel.app)
**Purpose:** Click-through reference document. Not a live-presented deck. Density is acceptable; navigation is critical.
**Owner:** Laura Restum, Marketing Manager, Envision Inc.
**Approver:** Madison Neuhaus, Senior Director, Marketing and Communications

---

## 1. Build Approach

### Tooling
- **Library:** `pptxgenjs` (Node)
- **Install:** `npm install pptxgenjs` (locally in repo, not global)
- **Skill:** If `/mnt/skills/public/pptx/SKILL.md` and `/mnt/skills/public/pptx/pptxgenjs.md` exist in your environment, read them first. They contain critical pitfalls (no `#` in hex colors, no unicode bullets, shadow object reuse corruption, etc.)
- **Output path:** `deliverables/envision-brand-book-2025.pptx`
- **Build script location:** `scripts/build-brand-book-pptx.js`

### Layout
- `LAYOUT_WIDE` (13.3" × 7.5") — gives more room for the dense reference content than 16:9
- All coordinates below assume this layout

### Logo assets
- Read directly from `assets/logos/` in this repo. Files needed:
  - `Envision_PrimaryLogo_2C.png` (horizontal master)
  - `EnvisionHands_WHITE.png` (icon only — note this is the white version, will need a navy or blue background to be visible)
  - `ARTS CENTER LOGO.png`
  - `BASE SUPPLY CENTER.png`
  - `Ecdc logo.png`
  - `EVRC LOGO.png`
  - `WICHITA FOUNDATION.png`
  - `DALLAS FOUNDATION.png`
- File names contain spaces — handle accordingly in Node (`path.join` is fine, just don't string-concat with shell)
- If the icon-only mark is white-on-transparent, place it on a navy `#002855` rectangle so it's visible

### Source content
- Primary source: the rendered HTML in this repo (whatever framework it's built in — Next.js, plain HTML, etc.)
- The full text content has already been extracted and is reproduced in Section 5 of this spec — you do NOT need to re-parse the HTML for copy. Use the spec as the source of truth for slide text.
- If the HTML is updated later and the deck needs to regenerate, update this spec file alongside it.

---

## 2. Visual System

### Color tokens
Use these exact hex values. **Never** prefix with `#` in pptxgenjs — it corrupts the file.

```javascript
const COLORS = {
  // Primary
  blue:        "003087",  // PMS 287C — primary brand blue
  green:       "78BE21",  // PMS 368C — primary brand green

  // Extended palette
  navy:        "002855",  // PMS 295C — dark navy, used for impact slides
  brightBlue:  "41B6E6",  // PMS 298C
  forestGreen: "00491E",  // PMS 3537C
  goldenrod:   "FFB81C",  // PMS 1235C
  terracotta:  "EA733D",  // PMS 4012C
  violet:      "8C4799",  // PMS 258C
  charcoal:    "53565A",  // PMS Cool Gray 11C
  gray:        "D9D9D6",  // PMS Cool Gray 1C

  // Utility
  white:       "FFFFFF",
  offWhite:    "F5F6F8",  // for light section backgrounds
  lightGray:   "E8EAEE",  // for dividers and subtle backgrounds
  textDark:    "1A1F2E",  // body text on light backgrounds
  textMuted:   "6B7280",  // secondary text, captions
};
```

### Typography
- **All text:** Montserrat (web typeface — appropriate since this is a digital reference doc)
- **Never** use Gotham — it's the print primary and not web-safe
- Title weight: Bold
- Body weight: Regular (400)
- Subhead weight: Bold (700)

### Type scale (in pt)
| Element | Size | Weight | Color | Notes |
|---------|------|--------|-------|-------|
| Cover title | 60 | Bold | white | "Brand Book" |
| Cover subtitle | 20 | Regular | white | "2025 Edition" |
| Section divider title | 48 | Bold | white | On navy background |
| Section number | 14 | Bold | green | "SECTION 02" — uppercase, charSpacing 4 |
| Slide title | 28 | Bold | navy | Top of content slides |
| Slide subtitle | 14 | Regular | textMuted | Below slide title |
| H2 / card header | 16 | Bold | navy | Inside cards |
| Body text | 11 | Regular | textDark | Default body |
| Small body | 10 | Regular | textDark | Dense reference slides |
| Caption | 9 | Regular | textMuted | Image captions, footnotes |
| Footer | 8 | Regular | textMuted | Bottom of every content slide |
| Stat label | 9 | Bold | green | Uppercase, charSpacing 3 |

### Visual motif (commit to ONE thing throughout)
**The motif:** A 0.08" tall green (`78BE21`) accent rectangle sitting directly above slide titles on every content slide. Width: 0.5". Position: same x as the title, y = title.y - 0.18.

This is the single repeated visual element that ties the deck together. **Do not** add other accent lines, underline-titles, or decorative bars. Just this one.

### Background system (sandwich structure)
- **Cover:** Navy (`002855`) full bleed
- **Section dividers (one per section, 7 total):** Navy full bleed
- **Content slides:** White (`FFFFFF`) with footer bar
- **Closing slide:** Navy full bleed

### Footer (every content slide)
- Position: y = 7.1, full width
- Left text: "Envision Brand Book · 2025 Edition" — 8pt, textMuted
- Right text: "Section X · Slide Y of 35" — 8pt, textMuted
- A 0.5pt horizontal line at y = 7.05 in lightGray

### Header (every content slide)
- Small Envision primary logo top-left, sized to ~0.9" wide, y = 0.3
- Section label top-right: "SECTION 02 · BRAND IDENTITY" — 9pt, bold, charcoal, charSpacing 2

---

## 3. Critical Brand Rules (DO NOT VIOLATE)

These are non-negotiable. Violations require a rebuild.

1. **Always write "Envision's Dallas Foundation"** with apostrophe. Never "Envision Dallas Foundation."
2. **Always write "Envision's Wichita Foundation"** with apostrophe.
3. **Never use "Envision Dallas"** as a standalone term. Use "Envision" for the org generally, or "Envision, Dallas Campus" for location-specific references.
4. **No em dashes anywhere.** Use commas, periods, or colons. The brand voice rules explicitly ban em dashes. Source content in this spec may contain em dashes — if you see one in the source, convert it to a colon or period before placing it in a slide.
5. **No AI-sounding phrasing.** Specifically avoid: "delve," "leverage" (as a verb), "navigate" (as a metaphor), "robust," "comprehensive solution," "in today's fast-paced world."
6. **Never call Envision Xpress anything.** It is retired. The replacement is "Envision Base Supply Center."
7. **Sentence case for all body text and most headers.** Title case is fine for slide titles and section names. Never ALL CAPS except for: section labels in the header strip, stat labels, and the explicit cases noted in the type scale.
8. **Person-first language throughout.** "People who are blind or low vision" on first reference per section, "BVI" acceptable after.
9. **No bullet symbols in unicode.** Use pptxgenjs `bullet: true` only.
10. **Logo: never alter, recolor, rotate, stretch, or add effects.** Use the PNG as-is. Always maintain clear space (height of the "E" in Envision on all sides — practically, leave at least 0.3" padding around any logo placement).

---

## 4. Slide Architecture (35 slides)

### Section breakdown
| # | Section | Slide count | Slide range |
|---|---------|-------------|-------------|
| — | Cover | 1 | 1 |
| — | Table of Contents (hyperlinked) | 1 | 2 |
| 1 | Our Story | 5 | 3–7 |
| 2 | Brand Identity | 8 | 8–15 |
| 3 | Brand Architecture | 5 | 16–20 |
| 4 | Pillar Profiles | 9 | 21–29 |
| 5 | Audience Personas | 6 | 30–35 (grouped 2-up) |
| 6 | Brand Voice | 4 | 36–39 |
| 7 | Brand in Action | 3 | 40–42 |
| — | Governance / closing | 1 | 43 |

**Wait — that's 43 slides, not 35.** The user requested ~35 but gave permission to flex. Final target: **42–43 slides.** This is acceptable for a reference doc. If forced to compress to 35, the cuts come from: combining Brand Voice into 3 slides, combining Brand in Action into 2 slides, dropping the section dividers (saving 7 slides), and combining Our Story into 4 slides. **Default build: 43 slides with section dividers included** because they aid navigation, which is the entire point of this deck.

Section dividers (1 per section) are counted in the slide ranges above.

---

## 5. Slide-by-Slide Build Instructions

For each slide: positioning is in inches, coordinates are (x, y, w, h). Slide canvas is 13.3 × 7.5.

---

### SLIDE 1 — Cover

- Background: navy (`002855`) full bleed
- Centered horizontally:
  - Envision primary logo (white version if available, otherwise the 2C version), positioned at approximately (5.65, 1.8, 2.0, 0.9). If only the 2C color version exists, place it on a white rounded rectangle (rectRadius 0.05) sized 2.2 × 1.1 at (5.55, 1.75).
  - "BRAND BOOK" — 60pt Montserrat Bold, white, charSpacing 6, centered at (1.65, 3.2, 10, 1.0)
  - Green divider rectangle — 0.08 high, 0.6 wide, green, centered at (6.35, 4.35)
  - "2025 Edition" — 20pt Montserrat Regular, white at 80% opacity (use color `FFFFFF` and `transparency: 20`), centered at (1.65, 4.55, 10, 0.5)
- Bottom-right corner: "The complete guide to how Envision looks, speaks, and shows up in the world." — 12pt Montserrat Regular Italic, white at 70% opacity, right-aligned at (6.3, 6.7, 6.7, 0.4)
- Bottom-left corner: small green square 0.15 × 0.15 at (0.5, 6.8) as motif anchor

---

### SLIDE 2 — Table of Contents (hyperlinked)

- Background: white
- Header strip and footer per global pattern (skip the section label on this slide — there is no section)
- Title: "Contents" at (0.5, 0.9, 8, 0.6), 36pt navy bold
- Subtitle: "A click-through guide. Tap any section to jump." at (0.5, 1.55, 10, 0.4), 12pt textMuted

**Two-column TOC layout:**

Left column (x = 0.5, w = 6.0), starting y = 2.3:
1. Our Story — Section 1 — slide 3
2. Brand Identity — Section 2 — slide 9
3. Brand Architecture — Section 3 — slide 17
4. Pillar Profiles — Section 4 — slide 23

Right column (x = 6.8, w = 6.0), starting y = 2.3:
5. Audience Personas — Section 5 — slide 33
6. Brand Voice — Section 6 — slide 40
7. Brand in Action — Section 7 — slide 45
— Governance — slide 49

**Each TOC entry:**
- Row height: 0.85
- Spacing between rows: 0.2
- Entry consists of:
  - Number (large, 32pt bold green) at left of the row
  - Section name (16pt bold navy) on first line
  - Page reference (10pt textMuted) on second line
  - Hyperlink: use `hyperlink: { slide: N }` on the section name text run

**Slide number references in the TOC must match actual slide numbers in the final deck.** Recalculate after the build is complete and update if necessary. (The numbers above assume section dividers are present; verify against actual output.)

---

### SECTION 1 DIVIDER — Slide 3

- Background: navy (`002855`) full bleed
- Top-left: "SECTION 01" — 14pt bold, green, charSpacing 4, at (0.5, 3.0, 4, 0.4)
- Below: "Our Story" — 64pt Montserrat Bold, white, at (0.5, 3.4, 12, 1.5)
- Green motif rectangle — 0.08 × 1.2, green, at (0.5, 5.1)
- Bottom-right small italic: "Mission, promise, values, history, why, elevator pitch." — 11pt white at 70%, at (7, 6.9, 5.8, 0.4), right-aligned

---

### SLIDE 4 — Mission & Promise

- Standard content slide layout (header, footer, motif rect above title)
- Title: "Mission & Promise"
- Subtitle: "The two statements that anchor everything Envision says and does."

**Two-card layout:**

Left card (x = 0.5, y = 2.0, w = 6.1, h = 4.4):
- White card with subtle shadow
- Top accent: 0.08" tall blue rectangle full width of card
- Inside, padding 0.4:
  - "MISSION" — 10pt bold green charSpacing 3, at top
  - Body: "To improve the quality of life and provide inspiration for the blind or visually impaired through employment, outreach, rehabilitation, education and research." — 14pt textDark, line spacing tight

Right card (x = 6.7, y = 2.0, w = 6.1, h = 4.4):
- Same structure
- Top accent: 0.08" tall green rectangle
- "PROMISE" label
- Body: "Inspiring the community, supporting caregivers, and providing a pathway to independence for the blind or visually impaired through a continuum of opportunities so they can thrive." — 14pt textDark

---

### SLIDE 5 — Core Values

- Title: "Core Values"
- Subtitle: "Six values. One commitment to the people we serve."

**3 × 2 grid of value cards** (each card 4.0 × 1.9):

| Position | Value | Description |
|----------|-------|-------------|
| (0.5, 2.1) | Integrity | We do what's right, always. |
| (4.65, 2.1) | Curiosity | We ask questions, seek solutions, and never stop learning. |
| (8.8, 2.1) | Passion | We care deeply about our mission and the people we serve. |
| (0.5, 4.15) | Initiative | We take action and lead with purpose. |
| (4.65, 4.15) | Teamwork | We're stronger together. |
| (8.8, 4.15) | Excellence | We set the bar high and exceed expectations. |

**Each card:**
- White fill, 1pt lightGray border
- Left edge 0.08" wide green accent rectangle (full card height)
- Padding 0.3
- Value name: 18pt bold navy at top
- Description: 11pt textDark below

---

### SLIDE 6 — History

- Title: "History"
- Subtitle: "From a 1933 workshop in Wichita to a national leader in life-changing solutions."

**Layout:** Single wide content area, narrative paragraphs.

Body content (11pt, textDark, line spacing 1.4, x = 0.5, y = 2.0, w = 12.3, h = 4.8):

> Envision's story began in 1933 with a simple yet powerful mission: to create opportunities for people who are blind or visually impaired to thrive. What began as a small workshop in Wichita has evolved into a movement that empowers individuals to unlock their full potential.
>
> From crafting brooms and pillowcases to pioneering research, education, and workforce development, Envision has continually evolved to meet the needs of the community we serve.
>
> Throughout the years, we've overcome challenges, sparked innovation, and broadened our impact to help those who are blind or visually impaired live with independence, purpose, and the opportunity to thrive.
>
> Today, we are a leader in creating life-changing solutions, offering a range of services that include rehabilitation, early childhood education, professional job opportunities, business solutions, and groundbreaking research. We're not just building a brighter future. We're helping people thrive every step of the way.

**Note: Original source contains an em dash in the last sentence ("We're not just building a brighter future — we're helping people thrive every step of the way."). Per brand rules, replace with period.**

Add a small "1933 → Today" timeline graphic at the bottom: a horizontal blue line at y = 6.5, with two green dots labeled "1933" (left) and "Today" (right). 9pt charcoal labels above each dot.

---

### SLIDE 7 — Our Brand & Why

- Title: "Our Brand & Our Why"
- Subtitle: "One idea connects everything we do: Thriving."

**Two-column layout:**

Left column (x = 0.5, w = 6.1):
- Header: "OUR BRAND" (10pt bold green charSpacing 3) at y = 2.0
- Pull quote at y = 2.4: "Thriving." — 48pt bold navy
- Below at y = 3.4: "Not surviving. Not managing. Thriving. With the right tools, the right support, and the right opportunities at every stage of life." — 12pt textDark
- Below: "That idea shapes how we communicate, what we build, and who we show up for. From the earliest childhood programs to the most advanced research. From the factory floor to the arts gallery. From a first rehabilitation appointment to a first paycheck." — 11pt textDark

Right column (x = 6.8, w = 6.0):
- Header: "OUR WHY" (10pt bold green charSpacing 3) at y = 2.0
- Body at y = 2.4: "We believe that when people who are blind or low vision have access to personalized solutions in employment, education, rehabilitation, and community, everyone benefits. Healthier individuals. Stronger families. Thriving organizations." — 12pt textDark
- Divider: thin lightGray horizontal line at y = 4.6, full column width
- Header at y = 4.8: "ELEVATOR PITCH" (10pt bold green charSpacing 3)
- Body at y = 5.2: "We are dedicated to providing personalized solutions for people who are blind or low vision, enriching their lives through employment, outreach, rehabilitation, education, and research, so they can thrive." — 12pt textDark italic

**Note:** Replace em dashes in the source with commas or periods as shown.

---

### SECTION 2 DIVIDER — Slide 8

- Same divider pattern as Section 1
- Section number: "SECTION 02"
- Title: "Brand Identity"
- Italic descriptor (bottom right): "Logo system, typography, color, imagery."

---

### SLIDE 9 — Logo Guidelines: Preferred Mark

- Title: "The Preferred Mark"
- Subtitle: "Use the horizontal Envision master logo for most applications."

**Layout:**
- Left half (x = 0.5, y = 2.0, w = 6.5, h = 4.5): white card with 1pt lightGray border
  - Inside the card, centered: the primary logo (horizontal master) sized to fit with at least 0.8" of clear space on all sides
  - Caption below logo (still inside card): "Envision Master Logo · Horizontal · Two Color"
- Right half (x = 7.3, y = 2.0, w = 5.5, h = 4.5):
  - Header: "WHY THIS MATTERS" (10pt bold green charSpacing 3)
  - Body at 11pt textDark: "Our logo represents the identity of our organization. It's how people recognize and connect with who we are. Always default to this version unless space or design constraints require a variation. Approach the logo with care and consistency in every application."
  - Divider line
  - Header: "RULE" (10pt bold green charSpacing 3)
  - Body: "Never alter, rotate, stretch, recolor, or add effects to the logo. Maintain clear space equal to the height of the 'E' in Envision on all sides."

---

### SLIDE 10 — Logo Variations

- Title: "Variations"
- Subtitle: "Horizontal, vertical, and icon-only options. Use only when the preferred mark cannot be accommodated."

**Three-card horizontal layout** (each 4.0 × 4.5 at y = 2.0):

Card 1 (x = 0.5): Horizontal
- Primary logo centered inside white card
- Label below (inside card): "HORIZONTAL" (10pt bold green) and "Default for most applications." (10pt textMuted)

Card 2 (x = 4.65): Vertical
- Empty card with placeholder text "EPS / vector only" centered, 12pt textMuted italic
- Label: "VERTICAL" (10pt bold green) and "No web asset available." (10pt textMuted)

Card 3 (x = 8.8): Icon Only
- Navy filled background (`002855`) since the icon-only mark is white-on-transparent
- Hands icon centered
- Label below (in white text on the navy): "ICON ONLY" (10pt bold green) and "App icons, social avatars, favicons." (10pt white at 80%)

---

### SLIDE 11 — Clear Space & Incorrect Usage

- Title: "Clear Space & What Never To Do"
- Subtitle: "Protect the mark. The fastest way to weaken a brand is sloppy logo usage."

**Layout:**

Top half (y = 2.0 to y = 4.2): Clear space
- Left: primary logo with imaginary clear space box around it (draw a 0.5pt dashed lightGray rectangle around the logo with margin = approx height of the "E")
- Right: text explanation — "Always leave breathing room around the logo, at least the height of the 'E' in Envision on all sides. This applies to print, digital, and signage."

Bottom half (y = 4.4 to y = 6.9): Incorrect usage
- Header: "NEVER DO THIS" (10pt bold, green, charSpacing 3)
- 3 × 3 grid of small text-only "don't" callouts (each 4.0 × 0.7):
  - Don't rotate the logo.
  - Don't stretch it disproportionately.
  - Don't alter the colors.
  - Don't add effects, shadows, or outlines.
  - Don't place over busy backgrounds.
  - Don't alter internal spacing.
  - Don't change proportions.
  - Don't rearrange components.
  - Don't substitute the typeface.
- Each item: small red ✕ icon (drawn as a small shape, not a unicode character) followed by the text
  - Red color: use `EA733D` (terracotta from the palette) — never introduce a new red

---

### SLIDE 12 — Typography: The Three Faces

- Title: "Typography"
- Subtitle: "Three typefaces. Each has a specific role. Never substitute."

**Three-column layout:**

Column 1 — Gotham (x = 0.5, w = 4.0):
- Large display "Aa" sample at top, 96pt — set fontFace to Gotham if available, otherwise fall back to Arial Black with a note that Gotham renders in actual deployment
- Label: "GOTHAM"
- Role: "Primary"
- Body: "Used for headlines and body copy in print and designed materials. Confident and clear."

Column 2 — Montserrat (x = 4.65, w = 4.0):
- Large "Aa" sample, Montserrat Bold
- Label: "MONTSERRAT"
- Role: "Web"
- Body: "Used for headlines and body copy in all digital and HTML formats. Freely available via Google Fonts."

Column 3 — Humanist (x = 8.8, w = 4.0):
- Large "Aa" sample (any humanist sans, since it's logo-only)
- Label: "HUMANIST"
- Role: "Logo only"
- Body: "Reserved for the logo only. Not for everyday use. Reserving this font exclusively for the logo strengthens brand identity."

Each column's "Aa" sits inside a tinted card: Gotham card uses lightGray fill, Montserrat card uses a 10% tint of blue (use `D9E2F0` as a static approximation since pptxgenjs doesn't support runtime tinting), Humanist card uses offWhite.

---

### SLIDE 13 — Typography Specs

- Title: "Type Specs"
- Subtitle: "Precise settings for headlines, body, and UI."

**Table layout** (5 columns, 6 rows):

Headers: Element | Font | Weight | Case | Leading & Tracking

| Element | Font | Weight | Case | Leading / Tracking |
|---------|------|--------|------|---------------------|
| Headlines | Gotham or Montserrat | Bold or Black | Title case | 110% / -0.045em |
| Subheads (large) | Montserrat | Bold | Uppercase | 100–110% / 0.5em |
| Subheads (small) | Montserrat | Bold | Title case | 100–110% / -0.02em |
| Body copy | Gotham Book or Montserrat Regular | Regular | Sentence | 140–160% / 0 to -0.02em |
| Buttons | Montserrat | Bold | Uppercase | 100–110% / 0.25em |

Use pptxgenjs `addTable` with:
- Header row: navy fill, white text, bold, 11pt
- Body rows: white fill, textDark, 10pt
- Borders: 0.5pt lightGray
- Padding: 0.1
- colW: [2.0, 2.8, 1.6, 1.6, 4.3]
- Position: x = 0.5, y = 2.0, w = 12.3

---

### SLIDE 14 — Brand Colors

- Title: "Brand Colors"
- Subtitle: "Bold, vibrant, full of life. Just like the people we serve."

**Two sections:**

**Primary colors (top half, y = 2.0 to y = 4.0):**
- Header: "PRIMARY" (10pt bold green charSpacing 3) at y = 2.0
- Two large color swatches side by side:
  - Blue swatch: rectangle 5.5 × 1.7 at (0.5, 2.4), filled `003087`
    - Inside, white text: "BLUE" (14pt bold), "PMS 287C · Hex 003087" (10pt regular at 80% opacity), "RGB 0 / 48 / 135" (9pt at 70%), "CMYK 100 / 81 / 0 / 23" (9pt at 70%)
  - Green swatch: rectangle 5.5 × 1.7 at (7.3, 2.4), filled `78BE21`
    - Same structure with green specs

**Extended palette (bottom half, y = 4.3 to y = 6.9):**
- Header: "EXTENDED PALETTE" (10pt bold green charSpacing 3) at y = 4.3
- 4 × 2 grid of smaller swatches (each 3.0 × 1.1):
  - Row 1 (y = 4.7): Navy `002855`, Bright Blue `41B6E6`, Forest Green `00491E`, Goldenrod `FFB81C`
  - Row 2 (y = 5.85): Terracotta `EA733D`, Violet `8C4799`, Charcoal `53565A`, Gray `D9D9D6`
- Each swatch: filled with its color. Inside text shows name (10pt bold) and hex (9pt regular). Use white text on dark colors (navy, forest green, charcoal, violet, blue) and textDark on light colors (bright blue, goldenrod, gray, terracotta is borderline — use white).

---

### SLIDE 15 — Accessible Color Combinations

- Title: "Accessible Combinations"
- Subtitle: "All combinations meet or exceed WCAG 2.1 AA standards (4.5:1 contrast ratio)."

**4 × 2 grid of contrast samples** (each 3.0 × 2.0 at y = 2.0 and y = 4.3):

Each sample is a colored rectangle with sample text inside, plus a small "AA PASS" badge.

| Position | Background | Text color | Sample text |
|----------|------------|------------|-------------|
| (0.5, 2.0) | Blue `003087` | White | "White on Blue" |
| (3.65, 2.0) | Navy `002855` | White | "White on Navy" |
| (6.8, 2.0) | White (with 1pt charcoal border) | Blue `003087` | "Blue on White" |
| (9.95, 2.0) | White (with 1pt charcoal border) | Navy `002855` | "Navy on White" |
| (0.5, 4.3) | Blue `003087` | Green `78BE21` | "Green on Blue" |
| (3.65, 4.3) | White (with border) | Charcoal `53565A` | "Charcoal on White" |
| (6.8, 4.3) | Navy `002855` | Green `78BE21` | "Green on Navy" |
| (9.95, 4.3) | White (with border) | Forest `00491E` | "Forest on White" |

Each sample:
- Sample text centered, 16pt bold
- Small "AA PASS" pill badge in bottom-right corner of each card: green fill, white text, 8pt bold, charSpacing 2

---

### SLIDE 16 — Imagery

- Title: "Imagery"
- Subtitle: "Real people. Real moments. Never staged."

**Three-column layout** (each 4.0 × 4.5 at y = 2.0):

Column 1 (x = 0.5) — STYLE
- Header: "STYLE" (10pt bold green charSpacing 3)
- Body (12pt textDark): "Use candid, natural photography that reflects the diversity and vibrancy of the people Envision serves. Avoid staged or stock photography that feels generic."

Column 2 (x = 4.65) — TONE
- Header: "TONE"
- Body: "Empowering and optimistic, with a focus on storytelling. Highlight moments of connection, achievement, and community."

Column 3 (x = 8.8) — APPLICATION
- Header: "APPLICATION"
- Body: "Show people in action: working, learning, creating. Use creative depth of field to draw attention to the subject. Never frame people with vision loss as passive, pitied, or defined by their disability."

Each column sits in a white card with a 0.08 tall green top accent.

---

### SECTION 3 DIVIDER — Slide 17

- Section number: "SECTION 03"
- Title: "Brand Architecture"
- Descriptor: "One brand. Clear identifiers. A three-tier system."

---

### SLIDE 18 — One Brand Principle

- Title: "One Brand. Clear Identifiers."
- Subtitle: "Envision is always primary. Always first. Always dominant. No sub-brand ever leads independently."

**Layout:**
- Left half (x = 0.5, w = 6.1, y = 2.0, h = 4.8): pull quote treatment
  - Large quote: "Everything we do, our programs, services, and products, fits under the Envision umbrella." — 24pt navy bold italic
  - Below: "It's all connected, working together to reflect who we are and what we stand for." — 14pt textMuted italic
- Right half (x = 6.8, w = 6.0, y = 2.0, h = 4.8):
  - Header: "WHY ONE BRAND" (10pt bold green charSpacing 3)
  - Body (12pt textDark): "Our brand isn't just a name or a logo. It's how we tell our story. A clear, unified brand helps us amplify our mission and make a bigger impact. When we speak as one, we show the world the strength and purpose behind Envision."
  - Divider line
  - Header: "WHAT THIS MEANS IN PRACTICE"
  - Body: "Programs and services don't get their own logos. They live under the Envision master brand and are referenced by name. Only entities with a physical location get a sub-brand lock-up, and even those are location identifiers, not independent brands."

---

### SLIDE 19 — The Three-Tier System

- Title: "The Three-Tier System"
- Subtitle: "Not every program needs the same level of visual identity. We use three tiers."

**Three vertical cards** (each 4.0 × 4.8 at y = 2.0):

Card 1 — TIER 1 (x = 0.5):
- Top: "TIER 01" (12pt bold green charSpacing 3)
- Header: "Sub-Brand Lock-Ups" (16pt bold navy)
- Subhead: "Physical locations only" (10pt textMuted italic)
- Body (10pt textDark): "Sub-brand lock-ups exist only for entities with a physical location. Places someone can walk into. These use the Envision master logo with the location name as a text treatment beneath it in Alternate Gothic. They are location identifiers, not independent brands."
- Bottom of card: "Confirmed: Arts Center · Base Supply Center · Child Development Center · Vision Rehabilitation Center" (9pt textMuted)

Card 2 — TIER 2 (x = 4.65):
- "TIER 02"
- Header: "Micro-Text Identifier"
- Subhead: "Optional, contextual"
- Body: "Programs that occasionally need visual distinction may use a small green micro-text treatment beneath the Envision master logo. This mirrors the Tier 1 treatment but is used sparingly and only when context requires program distinction. It is an option, not a default."

Card 3 — TIER 3 (x = 8.8):
- "TIER 03"
- Header: "Text Only"
- Subhead: "Programs and services"
- Body: "Everything else is referenced by program name in text only. Master logo, no additional visual treatment."
- Bottom: "Includes: Research Institute · Workforce Innovation · Mission Services · Continuing Education" (9pt textMuted)

Each card: white fill, 1pt lightGray border, top accent rectangle 0.08 tall in green.

---

### SLIDE 20 — Tier 1 Lock-Ups

- Title: "Tier 1 Lock-Ups"
- Subtitle: "Confirmed sub-brand identifiers for physical locations."

**2 × 2 grid of logo cards** (each 6.1 × 2.3):

| Position | Logo file | Caption |
|----------|-----------|---------|
| (0.5, 2.0) | `ARTS CENTER LOGO.png` | Envision Arts Center · Studio, gallery, and creative community space in Wichita |
| (6.7, 2.0) | `BASE SUPPLY CENTER.png` | Envision Base Supply Center · Retail locations on military installations |
| (0.5, 4.5) | `Ecdc logo.png` | Envision Child Development Center · Campuses in Wichita and Dallas |
| (6.7, 4.5) | `EVRC LOGO.png` | Envision Vision Rehabilitation Center · Clinical rehabilitation |

Each card:
- White fill, 1pt lightGray border
- Logo image centered in left ~40% of card with appropriate sizing (preserve aspect ratio, max width 2.0", max height 1.6")
- Caption text in right ~60% of card, 11pt textDark, vertically centered

---

### SLIDE 21 — Foundations & Retired Marks

- Title: "Foundations & Retired Marks"
- Subtitle: "Foundation lock-ups are donor-facing only. Retired marks must not appear in any new materials."

**Two sections:**

**Top half — Foundations (y = 2.0 to y = 4.5):**
- Header: "FOUNDATION LOCK-UPS" (10pt bold green charSpacing 3)
- Two cards side by side:
  - Wichita (x = 0.5, w = 6.1, h = 2.0): logo `WICHITA FOUNDATION.png` left, caption "Envision's Wichita Foundation · Used for sponsorships, letterhead, and formal donor-facing materials only."
  - Dallas (x = 6.7, w = 6.1, h = 2.0): logo `DALLAS FOUNDATION.png` left, caption "Envision's Dallas Foundation · Always written with the apostrophe. Never 'Envision Dallas Foundation.'"

**Bottom half — Retired (y = 4.7 to y = 6.9):**
- Header: "RETIRED — DO NOT USE" (10pt bold terracotta charSpacing 3)
- 4-item list (each on its own row, 11pt):
  - Envision Xpress · Now Envision Base Supply Center.
  - Old colorful Envision Arts logo · Now Envision Arts Center with master logo.
  - Old ECDC circular logo · Now Envision Child Development Center with master logo.
  - All previous independently operating sub-brand logos · Retired.
- Each item gets a small terracotta ✕ shape to its left

---

### SECTION 4 DIVIDER — Slide 22

- Section number: "SECTION 04"
- Title: "Pillar Profiles"
- Descriptor: "Eight pillars. Eight audiences. One master brand."

Note: There are 9 pillar slides (8 program/service pillars + Continuing Education, which is technically part of the pillar architecture). Update descriptor to "Nine areas. Eight audiences. One master brand."

---

### SLIDES 23–31 — Pillar Profiles (one per pillar, 9 slides)

**Common layout for every pillar slide:**
- Header (per global) with section label "SECTION 04 · PILLAR PROFILES"
- Title: pillar name
- Subtitle: pillar's lead message (one sentence)
- Body in a 2-column structure:
  - Left column (x = 0.5, w = 6.5, y = 2.0, h = 4.8) — narrative content
  - Right column (x = 7.3, w = 5.5, y = 2.0, h = 4.8) — structured cards (audience, tone, content rule)

**Left column content (per pillar):**
- "WHAT IT DOES" (10pt bold green charSpacing 3)
- Body: pillar's "what it does" content (11pt textDark)
- Spacer
- "WHAT THRIVING LOOKS LIKE" (10pt bold green charSpacing 3)
- Body: italic, 11pt textDark italic

**Right column content (per pillar):**
- 3 stacked cards (each 5.5 × ~1.5):
  - Card 1: "WHO IT SERVES" header (9pt bold green) + body (10pt textDark)
  - Card 2: "TONE" header + 3 tone keywords as inline pills (small green-bordered rounded rectangles with the keyword inside)
  - Card 3: "CONTENT RULE" header + body (10pt textDark)

**Per-slide content:**

#### SLIDE 23 — Envision Arts Center
- Title: Envision Arts Center
- Subtitle: "Creative expression belongs to everyone. The Envision Arts Center is where that belief becomes a place."
- What it does: "A studio, gallery, and community space dedicated to creative expression by and for people who are blind or low vision. Currently centered on traditional visual art: exhibitions, community art initiatives, and artist showcases. With a vision that extends to music, dance, and other creative disciplines over time."
- Thriving: "An artist whose work is seen, valued, and celebrated. A community built around creative possibility."
- Who: "Artists who are blind or low vision, community members, donors, arts patrons, educators, and cultural partners."
- Tone: Expressive · Celebratory · Community-rooted
- Content rule: "Never frame arts programming as therapy or rehabilitation. These are artists. Lead with the work and the space, not the diagnosis. Language should be expansive enough to grow with the program."

#### SLIDE 24 — Envision Base Supply Center
- Title: Envision Base Supply Center
- Subtitle: "Quality products, reliable supply, operated by people who understand service."
- What it does: "Retail locations on military installations serving active duty service members, veterans, and military families. Military-facing merchandise and BSC-specific e-commerce. Operates under AbilityOne contracts. AbilityOne is the contract foundation here, not just a credential."
- Thriving: "A service member who gets what they need without friction. A veteran employee who found purpose and income after service."
- Who: "Active duty service members, veterans, military families, installation leadership, and contracting officers."
- Tone: Direct · Earned · No-nonsense
- Content rule: "Avoid 'giving back' or 'supporting our troops' framing. It reads as performative to this audience. Lead with operational reliability and shared values. US-based workforce is a strong proof point. Envision Xpress is retired. All materials must reflect Envision Base Supply Center going forward."

#### SLIDE 25 — Envision Child Development Center
- Title: Envision Child Development Center
- Subtitle: "Every child is ready to learn. ECDC builds the environment that proves it."
- What it does: "Early childhood education at two campuses: Wichita and Dallas (1801 Valley View Lane, Farmers Branch, TX). Primary focus is children who are blind or low vision, in an integrated classroom environment where sighted peers learn alongside them."
- Thriving: "A child who enters kindergarten ready: socially, emotionally, academically. A family that found a community that truly understood what their child needed from the start."
- Who: "Parents and caregivers of young children, particularly families navigating early childhood vision loss. Also serves the broader Wichita and Farmers Branch communities through an integrated classroom model."
- Tone: Warm · Community-driven · Parent-facing
- Content rule: "The integration model is the differentiator. Lead with that. Use 'future-ready' not 'forward-thinking.' Do not use 'all abilities' (overstates scope). Do not use 'difference' in reference to students. Community involvement is a proof point."

#### SLIDE 26 — Envision Commercial Solutions
- Title: Envision Commercial Solutions
- Subtitle: "We are a high-performing commercial partner. Capability, compliance, and on-time delivery are the story."
- What it does: "Print services, contact center, fulfillment, assembly, products, and e-commerce (Supply Source). Envision's primary commercial revenue engine. Serves enterprise and government buyers across any industry. Two tracks: Commercial/Enterprise (private sector, AbilityOne not relevant, capability leads) and Government (compliance and certification matter, AbilityOne important but not always the primary contract vehicle)."
- Thriving: "A workforce producing enterprise-grade output. Clients who get what they need, on time, at spec."
- Who: "Procurement officers, supply chain managers, contracting officers, GPO decision-makers, and enterprise buyers across industries."
- Tone: Business-first · Capability-led · Credibility-forward
- Content rule: "Lead always with capability, compliance, and proof points. Mission enters only when the contract requires it or the buyer is known to value nonprofit partnerships. Never open with nonprofit or charity framing. The mission-follows-credibility rule applies here more than anywhere else."

#### SLIDE 27 — Envision Mission Services
- Title: Envision Mission Services
- Subtitle: "From the earliest years to the latest ones, no one navigates this alone."
- What it does: "Community-based programs and services supporting BVI individuals across every stage of life, from early childhood through senior years. Programs span recreation, socialization, daily living support, and community connection, including but not limited to Heather's Camp, PRIDE (Adult Day Support), and Adult Support Group."
- Thriving: "A person who has community, connection, and support that fits their life at every stage. A family that isn't figuring this out alone."
- Who: "BVI individuals of all ages and their families, caregivers, social workers, and community partners."
- Tone: Compassionate · Community-centered · Dignified
- Content rule: "Person-first language is non-negotiable here. No inspiration porn. Stories should center the person's experience and choices, not Envision's role in them. Never enumerate all programs. The scope is too broad. Lead with the continuum, not a checklist."

#### SLIDE 28 — Envision Research Institute
- Title: Envision Research Institute
- Subtitle: "The future of vision health is being built here."
- What it does: "Research labs and facilities, fellowship programs, and collaborative projects with universities and industry partners. Focused on vision science and innovations that improve quality of life for people who are blind or low vision."
- Thriving: "A research breakthrough that changes how vision loss is understood or treated. A fellow who goes on to lead the field. A partnership that produces something that didn't exist before."
- Who: "Researchers, fellows, university and industry partners, healthcare and policy audiences, and donors motivated by scientific impact."
- Tone: Credible · Precise · Forward-looking
- Content rule: "Specificity is credibility here. Name the partnerships, the studies, the outcomes where possible. Vague innovation language undermines trust with this audience more than anywhere else in the brand."

#### SLIDE 29 — Envision Vision Rehabilitation Center
- Title: Envision Vision Rehabilitation Center
- Subtitle: "You don't have to figure this out alone."
- What it does: "A comprehensive vision rehabilitation center offering clinical low vision evaluations, assistive technology training, and independent living skills programs. All designed to support people at every stage of vision loss. Also home to the Everyday Store, which provides adaptive products and tools for daily living."
- Thriving: "A person who moves through the world with confidence, equipped with the right tools and skills. Not overcoming vision loss. Building a full life alongside it."
- Who: "People who are blind or low vision, newly diagnosed or experiencing progressive vision loss, referring physicians, caregivers, and family members."
- Tone: Warm · Clinically credible · Hope-forward
- Content rule: "Never use 'despite' in relation to vision loss. Never frame services as charity. Center the client's agency. Referring physicians are a key referral source. Materials targeting them lead with clinical outcomes and program credibility, not emotional storytelling."

#### SLIDE 30 — Envision Workforce Innovation
- Title: Envision Workforce Innovation
- Subtitle: "A career isn't just income. It's identity, independence, and momentum."
- What it does: "Career development, job readiness, and employment pathways for people who are blind or low vision. Youth programs include Level-Up, a national residential Pre-Employment Transition Services program for ages 14-22 held at Wichita State University. Adult programs include Pathways and the Workforce Readiness Bootcamp, an intensive assistive technology training program that equips adults with the real-world skills needed to find and keep a job."
- Thriving: "A young person with direction and the skills to pursue it. An adult who lands a job and keeps it. Employers who come back because the candidates are prepared."
- Who: "Youth ages 14-22 and their TVIs, parents, and VR counselors. Adults seeking employment or career advancement. Employers and workforce development partners."
- Tone: Motivating · Practical · Outcome-focused
- Content rule: "Lead with outcomes and proof points, not inspiration. For Level-Up, the primary marketing audience is TVIs and VR counselors. Lead with program structure and Pre-ETS compliance. For adult programs, lead with real skills and real outcomes."

#### SLIDE 31 — Continuing Education
- Title: Continuing Education
- Subtitle: "The field moves forward when practitioners do."
- What it does: "Grand Rounds, Envision Conference, and on-demand courses. Continuing education for professionals in the vision rehabilitation and blindness services fields. Known internally as Envision University. Externally referenced as Continuing Education."
- Thriving: "A clinician who leaves with tools they use Monday morning. A field that is more connected, more informed, and better equipped."
- Who: "Vision rehabilitation specialists, occupational therapists, orientation and mobility specialists, low vision clinicians, researchers, and allied health professionals."
- Tone: Authoritative · Collegial · Peer-to-peer
- Content rule: "This audience is highly credentialed. Write to their level. Lead with content quality, faculty, and networking opportunity. CEU credits are a practical proof point. No standalone logo. Appears as Envision master logo with program name as text treatment only."

---

### SECTION 5 DIVIDER — Slide 32

- Section number: "SECTION 05"
- Title: "Audience Personas"
- Descriptor: "Twelve lenses. Every message is an opportunity to help someone thrive."

---

### SLIDES 33–38 — Personas (12 personas grouped 2-up across 6 slides)

**Common layout — 2 personas per slide:**
- Header / footer per global
- Title: e.g., "Vision Rehabilitation Client & BVI Adult"
- Subtitle: "Two of twelve audience lenses." (or appropriate variant)
- Two equal-width persona cards side by side:
  - Left card: x = 0.5, y = 2.0, w = 6.1, h = 4.8
  - Right card: x = 6.7, y = 2.0, w = 6.1, h = 4.8
- Each card: white fill, 1pt lightGray border, 0.08 tall green top accent

**Per-card structure (top to bottom inside each card):**
- Persona name (16pt bold navy)
- Single-line tag (10pt italic textMuted)
- "WHO THEY ARE" (9pt bold green charSpacing 3) + body (10pt textDark)
- "WHAT THEY NEED" (9pt bold green) + body (10pt textDark)
- "PILLARS" (9pt bold green) + body (10pt textDark)
- "REACH" (9pt bold green) + body (10pt textDark)
- "THRIVING" (9pt bold green) + body (10pt textDark italic)

Keep each persona section's body to 1–2 sentences max. Compress where needed. The goal is parity between cards on the same slide.

#### SLIDE 33 — Vision Rehabilitation Client + BVI Adult
**Vision Rehabilitation Client**
- Tag: First point of contact. Often referred by an eye care professional.
- Who: An adult experiencing vision loss, newly diagnosed or facing progressive deterioration, actively seeking clinical support and rehabilitation services. Often the first point of contact with Envision, arriving at a moment of emotional vulnerability.
- Need: Clinical credibility, practical tools, and hope grounded in real outcomes. They need to know that losing vision is not losing a life.
- Pillars: Vision Rehabilitation Center primarily. Everyday Store, AT training, and Mission Services as their journey continues.
- Reach: Physician referral. Warm, credible, outcome-focused materials. Never pity, never charity framing.
- Thriving: Regaining daily function and confidence. Moving from uncertainty to independence with the right tools and support.

**BVI Adult**
- Tag: Living, working, engaging on their own terms.
- Who: An adult who is blind or low vision and has been navigating life with vision loss for some time. Not in active clinical rehabilitation. They are participating in life on their own terms.
- Need: Community, connection, and programs that meet them where they are. They are not looking to be helped. They are looking to participate.
- Pillars: Mission Services primarily. Arts Center, Workforce Innovation, and Continuing Education depending on interests and life stage.
- Reach: Peer-to-peer channels, community networks, and word of mouth. Direct and respectful. Never patronizing.
- Thriving: A full life on their own terms. Community, purpose, and the freedom to engage as a participant, not a recipient.

#### SLIDE 34 — BVI Youth + Family Member or Caregiver
**BVI Youth**
- Tag: Ages 14–22. Figuring out what's possible.
- Who: A young person who is blind or low vision navigating the transition from school to adulthood. Often the only student with vision loss in their entire school district. Figuring out who they are and what's possible.
- Need: Connection with peers who get it. Real skills, real career exploration, and a clear picture of what their future can look like. Not inspiration. Possibility.
- Pillars: Workforce Innovation and Level-Up primarily. Mission Services through Heather's Camp as a younger entry point.
- Reach: Primary marketing audience for youth programs is TVIs, VR counselors, parents, and educators. Materials targeting youth should feel peer-level, energetic, and possibility-focused.
- Thriving: A young person who arrives at adulthood with direction, skills, confidence, and a network of peers.

**Family Member or Caregiver**
- Tag: Often the decision-maker. Always the support system.
- Who: A parent, spouse, adult child, or close family member navigating vision loss alongside someone they love. Often doing the research, making the calls, and managing the logistics.
- Need: Clarity, reassurance, and practical guidance. They need to understand what's available, what to expect, and how to support without taking over. Not the client, but often the decision-maker.
- Pillars: Vision Rehabilitation Center, ECDC, Mission Services, and Workforce Innovation depending on who they support.
- Reach: Physician referral, school and TVI networks, and community word of mouth. Warm, practical, empowering. Never alarmist.
- Thriving: Confidence that their person is supported, equipped, and not alone.

#### SLIDE 35 — Referring Professional + Continuing Education Professional
**Referring Professional**
- Tag: TVIs, VR counselors, ophthalmologists, OTs, social workers.
- Who: A credentialed professional whose role connects people with vision loss to Envision's programs. Engages with Envision as a peer institution.
- Need: Confidence Envision will deliver. Clinical credibility, clear program documentation, outcome data, and materials that make the referral process easy.
- Pillars: Vision Rehabilitation Center, Workforce Innovation, ECDC, and Mission Services depending on professional focus.
- Reach: Professional networks, state AER chapters, TVI listservs, CE events, and peer referral. Collegial, outcomes-focused. Write to their credential level.
- Thriving: A client who gets the right support at the right time. A referral process that is straightforward and well-documented.

**Continuing Education Professional**
- Tag: Practitioners seeking ongoing professional development.
- Who: A credentialed professional in vision rehabilitation or blindness services seeking ongoing development. Engages as a peer and thought leader.
- Need: High-quality, relevant CE that advances their practice. CEU credits, access to emerging research, and connection with peers across the field.
- Pillars: Continuing Education primarily. Research Institute as interests intersect with applied vision science.
- Reach: Professional associations, conference networks, peer referral, TVI and rehabilitation listservs. Authoritative and collegial.
- Thriving: A field that is more connected, better informed, and more equipped to serve people who are blind or low vision.

#### SLIDE 36 — Enterprise Buyer + Military Community
**Enterprise Buyer**
- Tag: Procurement, supply chain, contracting officers.
- Who: A procurement officer, supply chain manager, operations leader, or contracting officer evaluating Envision as a commercial partner. Motivated by performance, compliance, and reliability.
- Need: Proof Envision can deliver. On-time, at spec, at scale. Certifications, compliance documentation, capability demonstrations, and references.
- Pillars: Commercial Solutions primarily across both commercial and government tracks.
- Reach: Direct sales outreach, industry events, vendor networks, and RFP processes. Professional, capability-led, credibility-forward. Never lead with nonprofit framing.
- Thriving: A vendor relationship that performs consistently, reduces friction, and delivers on every commitment.

**Military Community**
- Tag: Service members, veterans, families, installation leadership.
- Who: Active duty service members, veterans, military spouses and families who shop at Envision Base Supply Centers. Also installation leadership and contracting officers who oversee BSC operations.
- Need: Reliable products, a well-run store, and a supply chain that doesn't let them down. Contracting officers additionally need compliance documentation and AbilityOne credentials.
- Pillars: Envision Base Supply Center primarily. Commercial Solutions for contracting officers.
- Reach: On-installation presence, installation leadership relationships, and military community networks. Direct, respectful, no-nonsense. Shared values over sentimentality.
- Thriving: A Base Supply Center that runs smoothly, stocks what they need, and operates with the professionalism the military community expects.

#### SLIDE 37 — Arts Community + Research and Academic Community
**Arts Community**
- Tag: Artists, arts patrons, educators, cultural partners.
- Who: Artists who are blind or low vision, arts patrons, educators, cultural partners, and community members who engage with the Envision Arts Center.
- Need: A space that takes the work seriously. Programming that is high quality, welcoming, and expansive. Not a charity art show. A genuine creative community with real artists making real work.
- Pillars: Envision Arts Center primarily. Mission Services and Donor/Community Partner depending on engagement level.
- Reach: Local arts networks, community events, social media, and word of mouth. Expressive, celebratory, community-rooted. Never clinical or institutional.
- Thriving: An artist whose work is seen, valued, and celebrated. A community that experiences creative possibility without limitation.

**Research and Academic Community**
- Tag: Researchers, faculty, fellows, industry partners, clinicians.
- Who: Researchers, university faculty, fellows, industry partners, and clinicians who engage with Envision Research Institute. Motivated by discovery, scientific rigor, and applied outcomes.
- Need: A credible, well-resourced research environment. Access to facilities, data, fellowship opportunities, and collaborative partnerships.
- Pillars: Envision Research Institute primarily. Continuing Education and Vision Rehabilitation Center as work intersects with clinical application.
- Reach: Academic conferences, peer-reviewed publications, university partnerships, and professional networks. Precise, credible, collegial.
- Thriving: A breakthrough that changes how vision loss is understood or treated. A fellowship that launches a career. A partnership that produces something that didn't exist before.

#### SLIDE 38 — Donor or Community Partner + Envision Employee or Job Seeker
**Donor or Community Partner**
- Tag: Individual givers, major donors, foundations, corporate partners.
- Who: Individual givers, major donors, foundations, corporate partners, and community organizations who invest in Envision's mission. Motivated by impact.
- Need: Proof of impact. Inspiring stories of independence and transformation. Quantified outcomes where possible. A relationship that feels personal and reciprocal.
- Pillars: Foundations primarily for philanthropic giving. Any pillar depending on area of interest.
- Reach: Donor events, foundation relationships, personal outreach, annual reports, and impact communications. Grateful, impact-driven, relationship-focused. Lead with stories, back them with outcomes.
- Thriving: Knowing their investment created something real. A relationship with Envision that deepens over time.

**Envision Employee or Job Seeker**
- Tag: Current employees and active candidates.
- Who: Someone who works at Envision or is actively seeking employment. Includes BVI and sighted employees across all departments and roles.
- Need: A workplace that values their contribution, invests in their growth, and lives its mission from the inside out.
- Pillars: All pillars depending on role. Commercial Solutions and Base Supply Centers for manufacturing and retail. Mission Services and Workforce Innovation for BVI employees hired through the mission pipeline.
- Reach: Job listings, employee referral, workforce development partnerships, and internal communications. Motivating, mission-connected, respectful.
- Thriving: A career that grows. A workplace that sees them fully. The knowledge that their work contributes to something that matters.

---

### SECTION 6 DIVIDER — Slide 39

- Section number: "SECTION 06"
- Title: "Brand Voice"
- Descriptor: "How we speak. Confident. Purposeful. Grounded."

---

### SLIDE 40 — Voice Is / Is Not + Mission Credibility Rule

- Title: "How We Speak"
- Subtitle: "Think of Envision's voice as a knowledgeable guide, not a cheerleader."

**Three-section layout:**

**Top section — pull quote (y = 2.0 to y = 3.0):**
- Centered: "Mission follows credibility." — 32pt bold navy italic
- Below: "Establish that Envision can do the work. Then mission becomes the reason to choose Envision over an equally capable partner." — 12pt textMuted, centered

**Bottom section — two columns (y = 3.3 to y = 6.9):**

Left column (x = 0.5, w = 6.1) — OUR VOICE IS:
- White card, green left accent
- Header: "OUR VOICE IS" (10pt bold green charSpacing 3)
- Three items, each with bold lead and description:
  - **Confident.** We speak with clarity and credibility. We know our work and back it up with real stories and real results.
  - **Purposeful.** Everything we say ties back to our mission. We're not just good. We're effective.
  - **Grounded.** We meet people where they are. No jargon. Genuine conversations.

Right column (x = 6.7, w = 6.1) — OUR VOICE IS NOT:
- White card, terracotta left accent (this is the only place in the deck where terracotta is used as an accent)
- Header: "OUR VOICE IS NOT" (10pt bold terracotta charSpacing 3)
- Four items:
  - Gloomy, divisive, or arrogant.
  - Jargon-heavy or self-centered.
  - Paternalistic. We do not "make people thrive." People thrive. We enable it.
  - Vague. Claims should be measurable where possible.

---

### SLIDE 41 — Language Rules

- Title: "Language Rules"
- Subtitle: "The non-negotiables. These shape every sentence we write."

**Two-column layout (y = 2.0 to y = 6.9):**

Left column (x = 0.5, w = 6.1):
- Header: "ALWAYS"
- 8-item list, each item: small green checkmark + text:
  - Person-first language. "People who are blind or low vision" first, BVI after.
  - Use "sighted" or "typical vision," not "normal."
  - Use "guide dog," not "seeing eye dog."
  - Center the person's agency. People thrive. Tools enable.
  - Sentence case unless brand standards require otherwise.
  - Keep claims measurable where possible.
  - Drop donor names in general materials. Use only in donor-facing contexts.
  - Always "Envision's Dallas Foundation" with the apostrophe.

Right column (x = 6.7, w = 6.1):
- Header: "NEVER"
- 8-item list, each item: small terracotta ✕ + text:
  - "Suffers," "victim," "afflicted," or "wheelchair-bound."
  - "Despite vision loss." Reframe around capability.
  - Sight-based idioms in BVI-facing content ("see," "look," "watch").
  - Em dashes.
  - AI-sounding phrasing.
  - "We make people thrive." (paternalistic)
  - "We give back to the community." (performative)
  - "Envision Dallas" as a standalone term.

---

### SLIDE 42 — Word Swaps

- Title: "Word Swaps"
- Subtitle: "Quick reference for the words and phrases we replace."

**Single table** (x = 0.5, y = 2.0, w = 12.3):

| Instead of | Use |
|------------|-----|
| assist | help, support, or partner (choose by context) |
| provide accommodations | design with access from the start |
| return to a normal life | return to daily routines / independence |
| suffers from vision loss | lives with vision loss |
| the blind | people who are blind or low vision |
| normal vision | sighted / typical vision |
| seeing eye dog | guide dog |
| optimize | improve |
| initiate | start |
| despite vision loss | with the right tools and support |

Table styling per global table spec (navy header, lightGray borders, alternating row fills optional with offWhite). colW: [4.5, 7.8].

---

### SECTION 7 DIVIDER — Slide 43

- Section number: "SECTION 07"
- Title: "Brand in Action"
- Descriptor: "Before and after. The voice rules applied."

---

### SLIDES 44–46 — Brand in Action (3 slides of before/afters)

**Common layout for each B/A slide:**
- Title and subtitle per global
- Two-card layout per example pair:
  - "BEFORE" card (left, terracotta accent) — strikethrough or muted styling
  - "AFTER" card (right, green accent) — strong, confident styling
- 2 examples per slide × 3 slides = 6 examples

#### SLIDE 44 — Universal Voice
- Title: "Universal Voice"
- Subtitle: "How we replace performative language with purposeful language."

**Example 1 — Services**
- Before: "Utilize our services for improved independence."
- After: "Use our tools and training to live independently."

**Example 2 — Employment**
- Before: "We give jobs to the blind."
- After: "We create career paths and paychecks."

#### SLIDE 45 — Pillar Voice in Practice (Part 1)
- Title: "Pillar Voice in Practice"
- Subtitle: "Three pillars. Three rewrites."

**Example 1 — Commercial Solutions**
- Before: "Envision is a nonprofit organization that provides meaningful employment to people who are blind or visually impaired through our print and fulfillment services."
- After: "Envision Print Services delivers enterprise-grade print, fulfillment, and assembly solutions. HIPAA compliant, on time, and built to spec. Our workforce is proven. Our capabilities are scalable. Let's talk about what we can do for your operation."

**Example 2 — Vision Rehabilitation Center**
- Before: "Despite losing most of her vision, Maria didn't give up. Thanks to Envision, she was able to get her life back."
- After: "Maria didn't know what was possible after her diagnosis. After working with the Envision Vision Rehabilitation Center, through low vision evaluations, assistive technology training, and independent living skills programs, she had the tools to move forward on her own terms."

#### SLIDE 46 — Pillar Voice in Practice (Part 2)
- Title: "Pillar Voice in Practice"
- Subtitle: "Three more pillars. Three more rewrites."

**Example 1 — Arts Center**
- Before: "Envision's art program gives blind and visually impaired individuals a creative outlet and therapeutic activity to help them cope with vision loss."
- After: "The Envision Arts Center is where artists who are blind or low vision make, show, and sell their work. No qualifier needed. Just art, and the community that surrounds it."

**Example 2 — ECDC**
- Before: "ECDC welcomes children of all abilities in an inclusive environment where every child is accepted."
- After: "At Envision Child Development Center, kids who are blind or low vision learn alongside their sighted peers from day one. State-of-the-art facilities, future-ready curriculum, and a community built for every child to thrive."

---

### SLIDE 47 — Closing / Governance

- Background: navy (`002855`) full bleed
- Top-left: small Envision primary logo (white version on navy, or 2C on a small white card)
- Centered:
  - "Envision Brand Book" — 36pt bold white at (1, 2.5, 11.3, 0.6)
  - Green divider rectangle 0.08 × 0.6, centered at (6.35, 3.3)
  - "2025 Edition" — 16pt regular white at 80%, centered at (1, 3.5, 11.3, 0.4)
  - Body at (2, 4.3, 9.3, 1.5), centered, 12pt white at 80%:
    > "Brand questions, asset requests, and approvals route through the Marketing and Communications team. The brand book is a living document. As our work evolves, this guide evolves with it."
- Bottom strip (y = 6.4): small text in 10pt white at 60%, three items left-aligned, center-aligned, right-aligned across the slide width:
  - "marketing@envisionus.com"
  - "envisionus.com"
  - "Approved by: Marketing & Communications"

---

## 6. Build Script Skeleton

The script should follow this structure:

```javascript
// scripts/build-brand-book-pptx.js
const pptxgen = require("pptxgenjs");
const path = require("path");
const fs = require("fs");

const REPO_ROOT = path.resolve(__dirname, "..");
const LOGOS_DIR = path.join(REPO_ROOT, "assets", "logos");
const OUTPUT_DIR = path.join(REPO_ROOT, "deliverables");
const OUTPUT_FILE = path.join(OUTPUT_DIR, "envision-brand-book-2025.pptx");

if (!fs.existsSync(OUTPUT_DIR)) fs.mkdirSync(OUTPUT_DIR, { recursive: true });

const COLORS = { /* per Section 2 */ };
const FONT = "Montserrat";

const pres = new pptxgen();
pres.layout = "LAYOUT_WIDE";
pres.author = "Envision Marketing & Communications";
pres.company = "Envision Inc.";
pres.title = "Envision Brand Book — 2025 Edition";
pres.subject = "Brand reference document";

// Helper functions
function addHeader(slide, sectionLabel) { /* ... */ }
function addFooter(slide, sectionNum, slideNum, totalSlides) { /* ... */ }
function addTitle(slide, title, subtitle) { /* with green motif rect above */ }
function addSectionDivider(num, title, descriptor) { /* ... */ }
function addPillarSlide(pillarData) { /* ... */ }
function addPersonaPair(personaA, personaB) { /* ... */ }

// Build slides
addCover();
addTOC();
// Section 1
addSectionDivider("01", "Our Story", "Mission, promise, values, history, why, elevator pitch.");
addMissionPromise();
addCoreValues();
addHistory();
addBrandAndWhy();
// Section 2
addSectionDivider("02", "Brand Identity", "Logo system, typography, color, imagery.");
addPreferredMark();
addLogoVariations();
addClearSpaceAndDonts();
addTypographyFaces();
addTypeSpecs();
addBrandColors();
addAccessibleCombos();
addImagery();
// Section 3
addSectionDivider("03", "Brand Architecture", "One brand. Clear identifiers.");
addOneBrandPrinciple();
addThreeTierSystem();
addTier1LockUps();
addFoundationsAndRetired();
// Section 4
addSectionDivider("04", "Pillar Profiles", "Nine areas. Eight audiences. One master brand.");
PILLARS.forEach(p => addPillarSlide(p));
// Section 5
addSectionDivider("05", "Audience Personas", "Twelve lenses.");
PERSONA_PAIRS.forEach(pair => addPersonaPair(pair));
// Section 6
addSectionDivider("06", "Brand Voice", "How we speak.");
addVoiceIsIsNot();
addLanguageRules();
addWordSwaps();
// Section 7
addSectionDivider("07", "Brand in Action", "Before and after.");
addUniversalVoice();
addPillarVoice1();
addPillarVoice2();
// Closing
addClosing();

pres.writeFile({ fileName: OUTPUT_FILE })
  .then(f => console.log(`Built: ${f}`))
  .catch(e => { console.error(e); process.exit(1); });
```

**Important: define `PILLARS` and `PERSONA_PAIRS` as data arrays** at the top of the file so adding/editing content doesn't require touching layout code. The data-driven pattern is critical for maintainability.

---

## 7. QA Checklist (do not skip)

After the build runs, before declaring success:

### Content QA
```bash
npx markitdown deliverables/envision-brand-book-2025.pptx > /tmp/check.md
grep -i "Envision Dallas Foundation" /tmp/check.md  # should return ZERO results — must be apostrophe form
grep -i "Envision Xpress" /tmp/check.md             # should return ZERO results outside the retired section
grep "—" /tmp/check.md                              # em dash check — should return ZERO results
grep -i "lorem\|TODO\|placeholder\|xxx" /tmp/check.md  # leftover content check
```

### Visual QA (mandatory)
1. Convert to PDF: `soffice --headless --convert-to pdf deliverables/envision-brand-book-2025.pptx`
2. Convert PDF to images: `pdftoppm -jpeg -r 150 envision-brand-book-2025.pdf slide`
3. Inspect every slide image for:
   - Text overflow
   - Overlapping elements
   - Misaligned cards in grids
   - Logo distortion (check aspect ratios)
   - Accent rectangle alignment
   - Footer collisions with body content
   - Section label / header strip alignment
   - TOC slide numbers matching actual slide positions
4. **Use a fresh subagent to inspect** — do not self-review with the same context that built the slides. The build context will see what it expects, not what's there.

### Brand QA
- Verify every "Foundation" reference uses the apostrophe form
- Verify "Envision Base Supply Center" is used (not Xpress) everywhere
- Verify no em dashes
- Verify Montserrat is the only font referenced (Gotham is fine in the typography slide as a sample, but never as the actual rendered text outside that slide)
- Verify color hex values exactly match the spec — no `#` prefixes
- Verify all 9 pillars are present
- Verify all 12 personas are present
- Verify TOC hyperlinks resolve to the correct slides

---

## 8. Acceptance Criteria

The build is complete when:
1. The .pptx file exists at `deliverables/envision-brand-book-2025.pptx`
2. It opens without errors in PowerPoint and Keynote
3. Slide count is 47 (cover + TOC + 7 sections × ~6 slides each + closing)
4. All logos render correctly with preserved aspect ratios
5. The TOC hyperlinks navigate to the correct slides when clicked in presentation mode
6. Visual QA against the rendered images shows no overflow, overlap, or alignment issues
7. All brand QA grep checks return zero violations
8. The build script is committed to `scripts/build-brand-book-pptx.js`
9. A README note is added to the repo explaining how to regenerate (`node scripts/build-brand-book-pptx.js`)

---

## 9. Known Edge Cases & Decisions

- **Color discrepancy resolved:** The brand book HTML defines primary blue as `#003087` (PMS 287C). An older internal corporate deck reference used `#1B365D`. **Source of truth is the brand book: use `003087` and `002855`.** Flag this discrepancy to Marketing leadership in a build note.
- **Em dashes in source content:** The source HTML contains em dashes in several places (Mission, History, Pillar descriptions). **Replace all of them** with periods, commas, or colons before placing the text into slides. Per brand voice rules, no em dashes anywhere.
- **Slide count drift:** The spec calls for 47 slides. The original conversation targeted ~35. The expansion is intentional (section dividers + ungrouped pillar slides), and the user explicitly approved up to ~45. Final delivered count of 47 is acceptable. If a hard 35-slide cap is required, drop the 7 section dividers (leaving 40), then merge the 2 pillar voice slides into 1 (39), then merge values into Mission/Promise (38), etc. Default: ship 47.
- **The vertical logo is unavailable as a web asset.** The Variations slide should show this as a labeled empty card per the spec, not omit the variation entirely.
- **The icon-only mark is white-on-transparent.** It must be placed on a navy background to be visible.

---

## 10. Build Sequence

For Claude Code, execute in this order:

1. Verify pptxgenjs is installed: `npm list pptxgenjs` — install if missing
2. Verify all 8 logo files exist in `assets/logos/`
3. Read this entire spec file
4. If `/mnt/skills/public/pptx/` is available, read `SKILL.md` and `pptxgenjs.md` for environment-specific gotchas
5. Create `scripts/build-brand-book-pptx.js` per the skeleton in Section 6
6. Build slide-by-slide following Section 5
7. Run the build
8. Run all QA checks in Section 7
9. Fix any issues, rebuild, recheck — at least one fix-and-verify cycle is required even if the first build looks clean
10. Commit `scripts/build-brand-book-pptx.js`, `deliverables/envision-brand-book-2025.pptx`, and this spec file (`docs/pptx-build-spec.md`)

---

**End of spec. Total slide count: 47. Estimated build time in Claude Code: 30–45 minutes including QA.**
