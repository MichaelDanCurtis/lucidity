# Lucidity Design — Tokens

## 1. FONTS (declare + load first)

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600&family=Space+Mono:wght@400;700&family=Marcellus&display=swap" rel="stylesheet">
```

| Role | Font | Use |
|------|------|-----|
| **UI / body** | `Space Grotesk` | Headings, body, controls. Weights 400/500/600. |
| **Data / labels** | `Space Mono` | ALL-CAPS instrument labels, numerals, metadata. 0.10–0.16em tracking. |
| **Display capitals** | `Marcellus` | The "one surprise" — capital letters on brand/display/headings only. Applied to capitals via `unicode-range` (see liquid-glass-web.md §5), never to mono instrument labels. |

`--font-ui: 'Space Grotesk', system-ui, sans-serif;`
`--font-mono: 'Space Mono', ui-monospace, monospace;`
`--font-display-caps: 'Marcellus', serif;`

## 2. TYPE SCALE (compact)

| Token | Size | Tracking | Use |
|-------|------|----------|-----|
| `--t-hero` | 40–48px | -0.02em | Greeting / hero (caps in Marcellus) |
| `--t-display` | 30px | -0.02em | Numerals (Space Mono), section heroes |
| `--t-title` | 22px | -0.01em | Brand wordmark, page titles |
| `--t-heading` | 17px | -0.01em | Section headings |
| `--t-body` | 14px | 0 | Body |
| `--t-label` | 10–11px | 0.14em | Space Mono ALL-CAPS instrument labels |
| `--t-micro` | 9px | 0.10em | Sub-labels |

Budget per screen: ≤3 sizes, ≤2 weights. Distinguish with spacing/opacity before adding a size.

## 3. CONTENT TOKENS — flat (Nothing)

Opacity hierarchy is the content hierarchy (max 4 levels).

**Dark (native):**
```
--bg:            #07181f;   /* deep teal-black field base */
--bg-deep:       #040d12;
--surface:       #0b2029;   /* solid content surface (Command tier) */
--text-display:  #ffffff;
--text-primary:  rgba(234,253,255,0.90);
--text-secondary:rgba(190,235,240,0.55);
--text-disabled: rgba(190,235,240,0.32);
--hairline:      rgba(180,240,245,0.12);
```

**Light:**
```
--bg:            #eef4f5;   /* cool off-white, NOT warm paper */
--bg-deep:       #e3edee;
--surface:       #ffffff;
--text-display:  #07171d;
--text-primary:  #16323a;
--text-secondary:#5a7178;
--text-disabled: #9bb0b5;
--hairline:      #d6e2e4;
```

Content is FLAT: no shadow, no blur, no gradient. Light mode = printed technical manual on
cool stock; dark = instrument panel in a dark room with data glowing.

## 4. GLASS / CHROME TOKENS — Liquid Glass

```
--glass-fill:        rgba(10,30,38,0.32);   /* dark; light mode ≈ rgba(255,255,255,0.5) */
--glass-fill-strong: rgba(10,30,38,0.5);
--glass-border:      rgba(180,240,245,0.14);
--glass-specular:    rgba(205,250,255,0.34); /* brighter top edge */
--glass-saturate:    1.4;
/* focal-plane blur dial (the 3 stops) */
--glass-blur-deep:   24px;   /* recede / out of focus */
--glass-blur:        13px;   /* base chrome haze */
--glass-blur-clear:  2px;    /* resolved / focal (→0 for sharpest) */
```

## 5. PALETTE — atmosphere vs accent

**Atmosphere (constant family signature):** cool teal data-field + warm beige light bleed.
```
--field-base:   #0c2a39;          /* teal field (radial, see liquid-glass-web.md §3) */
--field-warm:   232,200,156;      /* warm BEIGE light bleed (rgb) */
--field-glass:  168,232,240;      /* cool glass bloom (rgb) */
--field-shade:  38,86,100;        /* a representative "shade of the bg" square (rgb) */
```

**Accent (per-product, warm — the ONE event):**
```
--accent:        #ff6a4d;   /* Lucidity/Design default (red-coral). Agent #38a8f0, Vision #9b6bf0 */
--accent-strong: color-mix(in srgb, var(--accent) 84%, var(--text-display));
--accent-tint:   color-mix(in srgb, var(--accent) 16%, var(--surface));
```
Accent is used ONLY for: primary action, the single interrupt, the active/alarm state.
Everything else is monochrome over the field.

## 6. SHAPE, SPACING, MOTION

```
--radius-xs: 3px;  --radius: 4px;  --radius-lg: 6px;  --radius-pill: 999px;  /* SHARP */
--space: 2,4,8,11,16,24,32,48,64,96 px   /* 8px-ish base; spacing IS hierarchy */
--ease-out: cubic-bezier(0.25, 0.1, 0.25, 1);   /* percussive, no bounce */
--dur-quick: 120ms; --dur: 200ms; --dur-fog: 400ms;  /* fog→focus transition */
```
Corners are sharp (3–4px). Motion prefers opacity over position; the signature move is
blur animating down/up as focus is gained/lost (fog→focus), on the `--ease-out` curve.
