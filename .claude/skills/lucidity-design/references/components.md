# Lucidity Design — Components (by layer)

The Law: **chrome = glass, content = flat.** Decide each element's layer first, then style.

## CHROME (glass — `.lx-glass`, sits over the bokeh field)

- **Rail / sidebar** — slim glass column, sharp 4px corners, monoline icons (1.5px, Lucide/
  Phosphor thin), active item = accent dot/glow. The active app/region de-frosts (focal).
- **Top bar / toolbar** — glass strip, specular top edge. Brand wordmark left (Marcellus caps),
  Space Mono status label right. No title chrome clutter.
- **Command bar / palette** — glass, the focal element when open (blur → clear), everything
  behind recedes (blur → deep).
- **Floating controls / buttons** — glass; the primary action takes accent-tinted glass
  (`background: color-mix(in srgb, var(--accent) 22%, transparent)`), others plain glass.
- **Sheets / popovers** — glass, partial height.

Buttons: pill only for true pills; otherwise sharp 4px. Hover = de-frost + specular lift, not
scale/shadow.

## CONTENT (flat — over `--surface`/`--bg`, NO glass, NO shadow)

- **Cards / panels** — flat `--surface`, 1px `--hairline`, sharp corners. The lightest
  container that works: prefer spacing → hairline → border → surface, in that order. Never box
  the most important element; let the hero float on the field.
- **Hero numerals / metrics** — Space Mono at `--t-display`/`--t-hero`, `--text-display`.
  Units as `--t-label` adjacent. One hero per screen.
- **Instrument labels** — Space Mono, ALL CAPS, 0.14em, `--text-secondary`. Machine-cut —
  never Marcellus.
- **Display / brand / greeting** — Space Grotesk with **Marcellus capitals** (the one surprise).
- **Lists / tables** — flat, dense; differentiate rows by opacity/spacing before color; no
  zebra. High-density tables = the solid tier (glass behind 200 rows hurts legibility + perf).
- **Data viz** — differentiate by **opacity (100/60/30)** or **pattern (solid/striped/dotted)**
  before introducing color. Status color on the *value* only.

## STATE → focus (the dial)

Map interaction to the blur dial: `hover/active/selected/urgent` → toward **clear**;
`idle/disabled/background` → toward **deep**. The one urgent thing (the "alarm") rests
de-frosted **and** accented — clarity + color = the loudest signal. See
`liquid-glass-web.md` §2.

## THE ONE SURPRISE (once per screen)

Marcellus capitals on the wordmark/greeting **or** a bloom poking through the field behind the
hero — not both competing. Everything else machine-precise.
