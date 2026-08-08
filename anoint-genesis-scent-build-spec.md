# ANOINT : GENESIS SCENT — Website Build Spec

Single-page site for a 5-fragrance men's collection. Target market: Bitcoiner /
AnCap men. The brand fuses sacred/masculine-journey language with Bitcoin
culture vocabulary (Genesis, Consensus, Sovereign, Encode, Singularity) —
lean into that fusion in supporting copy, but don't overdo the keyword-spotting.

## Brand concept

Five fragrances trace a masculine journey in five stages: vision → awakening
→ flow → maturation → transcendence. Each stage has a name, a virtue, a tone
color, and a real-perfume comparison the buyer can orient against.

Colour arc (this is the spine of the whole design):
**White → Amber → Teal → Tobacco → Black**

| # | Name | Stage | Tone | Notes (top / middle / base) | Comparable |
|---|------|-------|------|------------------------------|------------|
| 1 | Prophet Encode | Vision | White `#f2ede4` | Grapefruit, Lemon, Mint, Bergamot, Pink Pepper, Coriander / Ginger, Jasmine, Nutmeg, Melon / Incense, Amber, Cedar, Sandalwood, Amberwood, Patchouli, Labdanum | Bleu de Chanel EDP |
| 2 | Dawn Aurora | Awakening | Amber `#b8863a` | Grapefruit, Bergamot, Pink Pepper, Black Currant, Pineapple, Nutmeg, Cloves / Ginger, Cinnamon, Citron, Cardamom, Rose / Patchouli, Vetiver, Oakmoss, Sandalwood, Musk, Tonka Bean | Creed Aventus Absolu |
| 3 | Sovereign Nomad | Flow | Teal `#1f4d47` | Sea Notes, Green Mandarin / Rosemary, Lavender / Mineral Notes, Ambergris, Musk, Cedar, Patchouli | Acqua di Giò Profondo EDP |
| 4 | Consensus Unity | Maturation | Tobacco `#5c3a24` | Tobacco Leaf, Spicy Notes / Vanilla, Cacao, Tonka Bean, Tobacco Blossom / Dried Fruits, Woody Notes | Tom Ford Tobacco Vanille |
| 5 | Ascent Singularity | Transcendence | Black `#0b0a08` | Pineapple, Grapefruit, Bergamot / Cedar, Patchouli, Jasmine / Oakmoss, Woody Notes | Nishane Hacivat |

Note pyramids above are sourced from Fragrantica's public listings for each reference fragrance (accessed Aug 2026) — treat them as directional inspiration for the brief, not a formulation spec. A real perfumer/fragrance house will need to develop each juice's actual formula; these note lists exist to keep the copy (and the design's sensory language) honest to what each reference actually smells like.

One-line hooks (use as section straplines, don't invent new ones):
1. Prophet Encode — "Every man begins with a vision."
2. Dawn Aurora — "Awaken. Illuminate. Rise."
3. Sovereign Nomad — "Own yourself. Move with purpose."
4. Consensus Unity — "Refine what you've built. Reflect what you've become."
5. Ascent Singularity — "Light becomes timeless."

Closing line: "Five scents. Five virtues. One timeless expression of man."
Tagline: "Timeless. Elemental. Masculine."

## Design tokens

**Color**
- `--ink` `#0b0a08` — base background, doubles as Ascent's tone
- `--bone` `#f2ede4` — Prophet's tone, primary light text on dark sections
- `--amber` `#b8863a` — Dawn's tone
- `--teal` `#1f4d47` — Sovereign's tone
- `--tobacco` `#5c3a24` — Consensus's tone
- `--gold` `#c9a227` — anointing-oil accent: CTA, dividers, the signature thread (not one of the 5 tones, used sparingly as the connective metal)

**Type**
- Display: a serif with an inscriptional/liturgical weight (e.g. Fraunces or Cormorant, set with generous tracking on eyebrows) — carries the "sacred" half of the brand
- Body: a clean geometric/technical sans (e.g. Space Grotesk or Inter) — carries the "protocol/ledger" half of the brand
- Load both via Google Fonts `<link>` tags (this runs in the user's browser, not a sandboxed environment, so this is fine)

**Signature element**
A vertical "anointing thread" — a thin gold line running down the page (desktop: fixed left rail; mobile: behind the section markers) with a small orb/node at each of the 5 fragrance sections. As the user scrolls into a fragrance's section, the page background transitions to that fragrance's tone color (via IntersectionObserver toggling a `data-tone` attribute on `<body>`, driving a CSS transition — do NOT try continuous scroll-interpolation, it's flaky across browsers). The thread's orb at the active section lights up gold. This makes the color arc something the visitor *travels through*, not just a swatch they're told about.

Avoid the generic-AI defaults: no cream-background/terracotta-accent combo, no near-black-with-single-neon-accent as the *whole* palette (the 5 named tones need to actually appear), no numbered-marker broadsheet template unless it's doing real work (here numbering IS meaningful — it's a sequence/journey — so 01–05 style markers are appropriate).

## Layout (single page, vertical scroll)

```
┌─────────────────────────────────────┐
│  HERO (full viewport, --ink bg)      │
│  eyebrow: ANOINT · GENESIS SCENT      │
│  headline (display serif, large):     │
│    "Five Anointings.                  │
│     One Becoming."                    │
│  subhead: brand concept, 1-2 lines    │
│  scroll cue                           │
└─────────────────────────────────────┘
┌─────────────────────────────────────┐
│  5× FRAGRANCE SECTION (repeat)        │
│  bg transitions to that tone on entry │
│  eyebrow: 0N — STAGE NAME             │
│  name (display serif, huge)           │
│  strapline                            │
│  notes: top / middle / base (3 cols   │
│    on desktop, stacked mobile)        │
│  "comparable" line, small/muted       │
│  thread orb lights up in left rail    │
└─────────────────────────────────────┘
┌─────────────────────────────────────┐
│  CLOSING (--ink bg, gold thread ends) │
│  "Five scents. Five virtues.          │
│   One timeless expression of man."    │
│  tagline: Timeless. Elemental.        │
│    Masculine.                         │
│  CTA button (gold): e.g. "View the    │
│    Collection" / "Get Notified"       │
└─────────────────────────────────────┘
```

## Technical notes for build

- Single HTML file, CSS + vanilla JS inline (no framework needed)
- IntersectionObserver per fragrance section → set `document.body.dataset.tone`
- CSS custom property transition on `background-color` (~600–800ms ease) driven by `data-tone`
- Fully responsive down to mobile: rail collapses, notes stack, type scale steps down
- Respect `prefers-reduced-motion`: disable the background transition animation (snap instead) and any other motion
- Visible keyboard focus states on the CTA and any interactive elements
- Copy is final as written above — don't rewrite the hooks/tagline, but you can write the hero subhead and CTA microcopy
