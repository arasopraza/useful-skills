# Carousel Generation Skill
**Platform:** Instagram | **Last updated:** June 2026

---

## Brand Identity

**Vibe:** Bold. Architectural. Smart. Casual. Actionable.
**Audience:** General Indonesian audience — practical, relatable, zero jargon.
**Language:** Bahasa Indonesia (conversational, not formal)

---

## Color Palette

| Role | Name | Hex |
|---|---|---|
| Primary background | Warm parchment | `#F2E8D9` |
| Secondary background | Dusty sand | `#D9C4A8` |
| Accent | Terracotta / burnt sienna | `#C4622D` |
| Dark text | Rich espresso | `#2C1A0E` |
| Light text / on-dark | Warm cream | `#FDF6EC` |
| Cover / CTA background | Deep walnut | `#3D2314` |

**Usage rules:**
- Default slide background: warm parchment `#F2E8D9`
- Cover and CTA slide background: deep walnut `#3D2314` (bookend effect)
- Dusty sand `#D9C4A8` used on every 3rd–4th content slide to break rhythm
- Terracotta used on 1–2 words per slide ONLY — never as full background
- Never use cool grays, stark whites, or sterile neutrals

---

## Typography System

| Role | Style | Notes |
|---|---|---|
| Hero headline | Bold geometric sans-serif | Heavy weight, high contrast |
| Script accent | Handwritten / script font | 1–3 key words per slide only |
| Body / supporting text | Clean geometric sans-serif | Regular weight, relaxed leading |
| Labels / tags | Bold sans, all-caps, tracked | Small, used for categories or badges |

**Typography rules:**
- Script font is an accent — never use for full sentences
- Key stat or number = oversized, terracotta colored, hero treatment
- Headline hierarchy: one dominant line, everything else secondary
- Never stack two script elements on the same slide

---

## Slide Format

- **Aspect ratio:** 4:5 portrait (1080 × 1350px)
- **Text density:** Moderate — headline + 2–3 supporting points per slide
- **Progress indicator:** Arrow (→) bottom-right on all content slides
- **Branding:** Handle/name top-left, every slide, small and unobtrusive
- **Structure:** Varied — Cover, Content, and CTA slides each have distinct layouts

---

## Slide Layout Templates

### Cover Slide
- Background: deep walnut `#3D2314`
- Large bold sans headline in warm cream
- 1–3 key words in script font, terracotta accent
- Handle top-left in cream
- Optional: 3D illustration or bold graphic element
- No progress arrow

### Content Slides
- Background: warm parchment `#F2E8D9` (or dusty sand for rhythm variation)
- Bold sans headline — one dominant line
- Script accent on 1–2 key words in terracotta
- 2–3 supporting points in clean sans, espresso color
- Slide number or large stat as hero element when relevant
- Arrow (→) bottom-right

### CTA Slide (always last)
- Background: deep walnut `#3D2314` (mirrors cover)
- Main line in large cream script
- Supporting action line in small cream sans
- Handle bottom-center, prominent
- No arrow

---

## Visual Style Rules

- Generous whitespace — never crowd the slide
- One visual hero per slide (stat, number, icon, or 3D element)
- Device mockups or 3D illustrations as visual anchors on content slides
- Strong grid structure — bold and architectural, not chaotic
- Dot or minimal progress at bottom only on carousel series (arrows preferred)
- Never use gradient backgrounds or drop shadows on text

---

## Copy Rules

**Tone:** Smart. Casual. Actionable. Sounds like a knowledgeable friend, not a corporate account.

**Do:**
- Start from a pain point, situation, or surprising fact
- Use short punchy sentences — one idea per line
- Let stats or numbers carry visual weight ("10x", "30 detik", "5 hal")
- End content slides with a single concrete takeaway
- CTA slide: always save + follow, phrased conversationally

**Never:**
- ❌ "Bukan X tapi Y" constructions
- ❌ "X kasih kamu ini, Y langsung kasih kamu itu" parallel comparisons
- ❌ Copy that sounds like a listicle or PowerPoint bullet
- ❌ Overly formal or academic language
- ❌ AI slop patterns — no formulaic sentence structures
- ❌ Filler words ("ternyata", "jadi", "kelihatannya", "sebenarnya")
- ❌ Binary contrast structures used as a crutch
- ❌ Vague statements — always be specific
- ❌ Passive voice — always use active voice
- ❌ Uniform sentence lengths — vary rhythm across a slide
- ❌ Over-explaining — trust the reader, state facts directly

---

## Workflow (every carousel session)

1. **User provides topic or link**
2. **Claude proposes an angle** — user approves or requests tweak
3. **Claude proposes slide structure** — number of slides + content direction per slide
4. **User approves structure** — Claude writes full slide-by-slide copy with visual direction
5. **User iterates on copy** — per slide if needed
6. **Claude generates images** once connected

**Default slide count:** Flexible — determined by topic complexity
**CTA rule:** Always include a CTA on the final slide
**Output format:** Pure markdown — copy + visual direction per slide, formatted with headers (## SLIDE N — Title), bold for headlines, and bullet points for visual direction

---

## Image Generation Instructions

**Model:** GPT Image 2 (OpenAI) — recommended
**Alt model:** Nano Banana (Google)

For each slide, generate with:
- Exact hex colors from palette above
- 1080 × 1350px (4:5 portrait)
- Typography direction as specified per slide type
- Visual anchor element (3D object, device mockup, or minimal illustration)
- Handle/branding positioned top-left
- Arrow positioned bottom-right (content slides only)

**Prompt structure:**
```
Instagram carousel slide, 4:5 portrait, warm parchment background #F2E8D9,
bold sans-serif headline in espresso #2C1A0E, [KEY WORD] in terracotta script #C4622D,
[VISUAL ELEMENT] as hero, handle "@[name]" top-left small, arrow → bottom-right,
generous whitespace, architectural layout, no gradients, no drop shadows
```

---

## Anti-Patterns (never do these)

- Cool or neutral color palettes
- Purple gradients, stark white backgrounds
- Copy with "bukan X tapi Y" or parallel comparison formulas
- Cluttered slides with too many elements
- Script font for full sentences
- Generic stock photo backgrounds
- Terracotta used as full background color
- Formulaic AI-sounding copy
- Two hero elements competing on the same slide
- Filler words and unnecessary qualifiers
- Passive voice constructions
- Over-explained points — let facts speak directly

---
