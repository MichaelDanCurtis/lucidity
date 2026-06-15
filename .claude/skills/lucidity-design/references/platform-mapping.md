# Lucidity Design — Platform Mapping

The language is **web-first** (the Lucidity/Galvo product stack is React/Next/Vite). Native is
noted for any future Apple target.

## HTML / CSS (primary)
- Define the tokens (`tokens.md`) as CSS custom properties on `:root` + `[data-theme="dark"]`.
- Chrome = `backdrop-filter` glass; content = flat. Bokeh field = the JS generator in
  `liquid-glass-web.md` §3 behind everything.
- Fonts via Google Fonts (declare first) or self-hosted woff2 (required for the `unicode-range`
  Marcellus-capitals trick, §5).
- Ship the accessibility media queries (§6).

## React / Next / Vite
- Token sheet imported once globally (a `lucidity-tokens.css`); per-product theme file overrides
  only `--accent` + the field warm/glass hues.
- Components: `<GlassPanel>`, `<GlassButton>`, `<CommandBar>`, `<Rail>` (chrome) +
  `<Field>` (the bokeh generator as a mount-effect component) + flat content components.
- `<Field>` renders the squares in a `useEffect` (or pre-rendered for SSR), `pointer-events:none`,
  behind a `position:relative` shell. Honor `prefers-reduced-motion`/`-transparency`.
- This is a *contract*, not a shared runtime library: vendor the token sheet + the field
  recipe into each product (matching the Lucidity "contract not library" philosophy); each app
  implements its own glass components.

## SwiftUI (Apple targets — future)
- Chrome glass → native `.glassEffect(.regular, in: .rect(cornerRadius: 4))`,
  `.buttonStyle(.glass)`, `GlassEffectContainer`; never on content.
- The bokeh field → a `Canvas`/`TimelineView` drawing soft squares with blur, behind content.
- Fonts → bundle Space Grotesk / Space Mono / Marcellus; apply Marcellus to capitals via a
  custom `AttributedString` pass (no `unicode-range` equivalent).
- Honor Reduce Transparency / Reduce Motion (`.glassEffect(.identity)` fallback).

## Paper / export
- Drop the field + glass entirely; content is already flat and print-ready (cool off-white,
  black ink, Space Mono labels). Marcellus capitals survive as the one expressive mark.
