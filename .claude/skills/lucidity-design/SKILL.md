---
name: lucidity-design
description: The Lucidity design language — the family-wide look for OmniVyx / Galvo / Lucidity products. Use ONLY when the user explicitly says "Lucidity design", "Lucidity Design skill", "/lucidity-design", or asks to apply the Lucidity look. NEVER trigger automatically for generic UI work. Fuses Nothing-style typographic discipline (flat content) with Apple Liquid Glass (chrome) over a square-bokeh "waking clarity" field.
version: 1.0.0
allowed-tools: [Read, Write, Edit, Glob, Grep]
---

# Lucidity Design Language

A cross-product design language for the Lucidity family. It fuses three things:
- **Nothing-style discipline** for everything you *read* (flat, monochrome, typographic).
- **Apple Liquid Glass** for everything you *act through* (translucent, refractive chrome).
- A **square-bokeh "waking clarity" field** as the atmosphere everything floats on.

**Before any work: declare the fonts** (Space Grotesk, Space Mono, Marcellus — see
`references/tokens.md` §1) and tell the user they load from Google Fonts. **Ask: dark or
light?** Dark is the native mode (waking in a dark room); light is first-class too.

See `showcase.html` in this skill for the canonical reference implementation.

---

## 1. PHILOSOPHY — the soul

**Lucidity is the moment of waking clarity** — the fuzz that fades exactly as you become
fully awake. The whole language is that transition made structural:

> **Blur belongs to the periphery; clarity belongs to the center.**

- **Chrome is the haze** — rail, toolbars, command surfaces float as soft Liquid Glass.
- **Content is the clarity** — data, type, hierarchy are flat, monochrome, razor-sharp.
- You move *through* fog to land on *focus*. The name is the mechanic.

**The field is a dream being sampled into reality.** The background is a depth-of-field
**square-bokeh** field: squares are pixels — *samples* of reality. Out of focus they bloom
into big soft squares (the compressed dream, "a remembered dream is a low-sampled song"); in
focus they resolve to small crisp squares (lucid reality). Most squares are faded *shades of
the background*; only a few **bloom through** (warm beige + cool glass). Blur eases off
toward the bottom (grounded), color goes glassier toward the top (the dream-glow rises).

**Clarity is a spotlight — the focal plane.** Glassiness is a *focus dial*, not a constant.
To pull the eye, **reduce the glass on what matters**: it resolves out of the fog while the
rest stays soft. The UI has a focal plane like a camera's — the alarm-clock-in-the-morning
effect. **Attention = the inverse of blur.** Focus stacks two ways: **de-frost** (clarity)
and the **accent** (color event); the loudest "alarm" does both.

Inherited from Nothing: *subtract don't add; structure is ornament; monochrome is the
canvas, color is an event; type does the heavy lifting; industrial warmth; one moment of
surprise per screen.*

## 2. THE LAW (one inviolable rule)

**Content is flat; chrome is glass; never cross.** Everything you *read* is flat (Nothing).
Everything you *act through* is glass (Liquid Glass), floating over the bokeh field. The
**accent is the single color event** per screen. (Both parents already agree: Apple — "glass
= navigation/chrome only, never content"; Nothing — "content is flat.")

## 3. ATMOSPHERE & PALETTE

- **The field is the family signature** — a **cool teal data-field** (the cold digital
  dream) with a **warm beige light bleed** in one corner. Square-bokeh, depth-of-field.
  Recipe in `references/liquid-glass-web.md` §3.
- **The accent is per-product and warm** — the *one event*: primary action, the single
  interrupt, the active/alarm state. Default leans **red/coral** (`#ff6a4d`/`#e5484d`);
  OmniVyx Agent = blue, Vision = purple. Warmth against the cold field = attention.
- **Atmosphere ≠ accent.** The cool field stays constant across products; only the warm
  accent swaps. Data-status colors (success/warning) are exempt — applied to the *value*,
  never the label or row.

## 4. CRAFT RULES

- **Three-layer hierarchy** per screen (primary / secondary / tertiary). Squint test: one
  thing must clearly win. Make the primary absurdly large, the tertiary absurdly small.
- **Font budget per screen:** ≤2 families in play, ≤3 sizes, ≤2 weights. Reach for spacing
  or opacity before a new size.
- **Spacing IS meaning** (8px base): tight = belongs-together, vast = new context. A divider
  means the spacing was wrong. Never box the most important element — let it float.
- **Opacity is the hierarchy** in content: display → primary → secondary → disabled.
- **The one surprise** = the **Marcellus capitals** (elegant classical Roman capitals — a
  human, inscriptional grace against the machine) + a bloom poking through the field. Once per
  screen. Everything else is machine-cut (Space Grotesk / Space Mono ALL-CAPS instrument labels).
- **Sharp corners** (3–4px; pill only for true pills). Compact density.
- **Percussive motion**: opacity over position, `--ease-out`, 150–250ms. The one signature
  move is **fog→focus** — blur animates *down* as an element gains focus, *up* as it loses
  it. Never on every element.

## 5. ANTI-PATTERNS — never do

- Never put **glass on content** (only chrome). Never put **flat opaque chrome** where glass
  belongs.
- Glass needs the **bokeh field (or content) behind it to refract** — never glass on a flat
  solid.
- No glass without a **`prefers-reduced-transparency` → solid fallback** and
  `prefers-reduced-motion` → kill field/fog motion.
- No gradients in content chrome; no drop shadows on content (Nothing). No skeuomorphism.
- No toast popups — inline status (`[SAVED]`, `[ERROR]`). No filled/multi-color icons, no
  emoji-as-UI. Monoline 1.5px icons (Lucide/Phosphor thin).
- No spring/bounce. No border-radius > 16px on cards.
- The bokeh field is **atmosphere, not decoration on content** — it lives behind the whole
  surface, faded; most squares are shades of the background, not bright confetti.

## 6. WORKFLOW

1. **Declare fonts** (Space Grotesk, Space Mono, Marcellus) — `references/tokens.md` §1.
2. **Ask mode** — dark (native) or light.
3. **Sketch the 3 layers**, then **assign each region a tier: content (flat) or chrome
   (glass)** — that decides flat vs glass before any CSS.
4. **Lay the field** — the square-bokeh atmosphere behind everything
   (`references/liquid-glass-web.md` §3).
5. **Compose** with the craft rules (§4); accent is the one event.
6. **Check tokens** (`references/tokens.md`), **build components** by layer
   (`references/components.md`), **adapt to platform** (`references/platform-mapping.md`).

## 7. REFERENCE FILES

- `references/tokens.md` — fonts, type scale, the two token families (content + glass),
  palette (cool field / warm accent), spacing, radii, motion.
- `references/liquid-glass-web.md` — the web/CSS recipes: Liquid Glass material, the
  square-bokeh depth-of-field field, the focal-plane blur dial, the `unicode-range`
  Marcellus-capitals trick, accessibility fallbacks.
- `references/components.md` — components split by layer (glass chrome vs flat content).
- `references/platform-mapping.md` — HTML/CSS + React; SwiftUI `.glassEffect()` note.
- `showcase.html` — the canonical reference implementation (open it in a browser).
