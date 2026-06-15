# Lucidity Design — Web/CSS Recipes

The source Liquid Glass system is SwiftUI-only; these are the web translations, plus the
Lucidity-specific square-bokeh field and the Marcellus-capitals trick. All verified in the
canonical `showcase.html`.

## 1. Liquid Glass material (chrome only)

```css
.lx-glass {
  background: var(--glass-fill);                 /* rgba(10,30,38,0.32) dark */
  -webkit-backdrop-filter: blur(var(--glass-blur)) saturate(var(--glass-saturate));
  backdrop-filter: blur(var(--glass-blur)) saturate(var(--glass-saturate));
  border: 1px solid var(--glass-border);
  border-top-color: var(--glass-specular);       /* brighter top edge = the light catch */
  border-radius: var(--radius);                  /* SHARP: 4px */
}
```
Rules: glass ONLY on chrome (rail, toolbars, command bar, floating controls, sheets) — never
on content. Glass needs the bokeh field (or content) behind it to refract; never glass over a
flat solid.

## 2. Focal-plane blur dial (attention = inverse of blur)

Glassiness is a focus dial across three stops. State drives it; transition on the fog curve.

```css
.lx-card { backdrop-filter: blur(var(--glass-blur)) saturate(1.3); opacity:.82;
  transition: backdrop-filter var(--dur-fog) var(--ease-out),
              -webkit-backdrop-filter var(--dur-fog) var(--ease-out),
              opacity var(--dur-fog), transform var(--dur-fog); }
/* recede everything that is NOT the hovered/focal element */
.lx-cards:has(.lx-card:hover) .lx-card:not(:hover){
  backdrop-filter: blur(var(--glass-blur-deep)) saturate(1.1); opacity:.4; transform:scale(.985); }
/* resolve the focal element */
.lx-card:hover{ backdrop-filter: blur(var(--glass-blur-clear)) saturate(1.5); opacity:1; }
/* the resting "alarm": de-frosted + accent, owns the eye with nothing hovered */
.lx-card.alarm{ backdrop-filter: blur(var(--glass-blur-clear)); opacity:1;
  background: color-mix(in srgb, var(--accent) 16%, transparent);
  border-color: color-mix(in srgb, var(--accent) 50%, transparent); }
```

## 3. The square-bokeh "waking clarity" field (the signature atmosphere)

A depth-of-field field of soft squares (pixels = samples). **Crisp/small at the lucid focal
center; large/soft into the dream periphery. Blur eases off toward the bottom (grounded);
color goes glassier toward the top. Most squares are faded shades of the background; a few
warm-beige / cool-glass squares bloom through. Additive (screen) blend.**

```html
<div class="lx-stage">
  <div class="lx-warm"></div>     <!-- warm beige light bleed, one corner -->
  <div class="lx-field" id="field"></div>
  <!-- glass chrome + flat content go here, above the field -->
</div>
```
```css
.lx-stage{ position:relative; overflow:hidden;
  background: radial-gradient(130% 115% at 70% 14%, #0c2a39 0%, #07181f 55%, #040d12 100%); }
.lx-warm{ position:absolute; inset:0; pointer-events:none; filter:blur(12px);
  background: radial-gradient(56% 64% at 4% 2%, rgba(232,200,156,.5), transparent 66%); }
.lx-field{ position:absolute; inset:0; pointer-events:none; }
.lx-sq{ position:absolute; transform:translate(-50%,-50%); mix-blend-mode:screen; border-radius:2px; }
```
```js
// generate the field — focal point fx,fy (%); blur ∝ distance²; vertical bias top↔bottom.
(function(){
  var f=document.getElementById('field'); if(!f) return;
  var shade=['38,86,100','30,74,88','46,98,110','52,84,92','34,80,96'];   // shades of the bg
  var beige=['232,200,156','240,214,172','246,224,188'];                  // warm poke-through
  var glassy=['168,232,240','140,215,232','120,205,228'];                 // cool glass bloom
  var fx=46, fy=34;
  for(var i=0;i<66;i++){
    var x=Math.random()*106-3, y=Math.random()*106-3;
    var t=Math.max(0,Math.min(1,y/100));                       // 0 top → 1 bottom
    var dx=(x-fx)/58, dy=(y-fy)/52, d=Math.min(1,Math.sqrt(dx*dx+dy*dy));
    var size=4+d*d*30+Math.random()*5;                          // small focal → large periphery
    var blur=(0.6+d*d*13)*(1-0.5*t);                            // less blur toward bottom
    var bloomChance=0.12+0.20*(1-t);                            // glassier blooms toward top
    var c, op;
    if(Math.random()<bloomChance){
      var warmward=(x<46 && y<48);
      c=warmward?beige[(Math.random()*beige.length)|0]:glassy[(Math.random()*glassy.length)|0];
      op=Math.max(0.15,Math.min(0.72,(0.6-d*0.25)*(0.75+0.3*(1-t))*(0.7+Math.random()*0.4)));
    } else {
      c=shade[(Math.random()*shade.length)|0];                  // faded, melts into the bg
      op=Math.max(0.06,Math.min(0.3,(0.32-d*0.15)*(0.6+Math.random()*0.5)));
    }
    var s=document.createElement('div'); s.className='lx-sq';
    s.style.cssText='left:'+x+'%;top:'+y+'%;width:'+size.toFixed(1)+'px;height:'+size.toFixed(1)
      +'px;filter:blur('+blur.toFixed(1)+'px);opacity:'+op.toFixed(2)+';background:rgba('+c+',0.9)';
    f.appendChild(s);
  }
})();
```
Tuning knobs: `fx,fy` (focal point), particle count, `size`/`blur` multipliers (DoF depth),
`bloomChance` (how many poke through), the `t` factors (vertical blur/glassiness gradient).
Per product, swap `beige`/`glassy`/the warm-bleed toward the product accent if desired — but
the cool teal field is the constant family signature.

## 4. Accent = the one event
Use `--accent` (warm, per-product) ONLY for the primary action, the single interrupt, and the
active/alarm state. Everything else is monochrome over the field.

## 5. Marcellus capitals via `unicode-range` (automatic — no per-letter markup)

Map capital glyphs (U+0041–005A) to Marcellus, everything else to Space Grotesk, in one
composite family. Any capital then renders in the hand automatically.

```css
/* self-host the two faces, then: */
@font-face{ font-family:'Lucidity Display'; src:url('/fonts/Marcellus.woff2') format('woff2');
  unicode-range:U+0041-005A; }                         /* capitals → Marcellus */
@font-face{ font-family:'Lucidity Display'; src:url('/fonts/SpaceGrotesk.woff2') format('woff2');
  unicode-range:U+0061-007A, U+0030-0039, U+0020-0040, U+005B-007E; }  /* the rest → grotesk */
.display{ font-family:'Lucidity Display', sans-serif; }
```
Scope: apply `Lucidity Display` to brand / headings / hero ONLY. **Never** to Space Mono
instrument labels — those stay machine-cut. (In a quick mockup you may instead wrap capitals
in `<span class="caps">` with `font-family:'Marcellus'`; production uses unicode-range.)

## 6. Accessibility (required)

```css
@media (prefers-reduced-transparency: reduce){
  .lx-glass{ background:var(--surface); backdrop-filter:none; -webkit-backdrop-filter:none; }
}
@media (prefers-reduced-motion: reduce){
  .lx-card{ transition:none; }      /* and skip any field animation */
}
```
Also: text on glass must hold AA against the darkest patch of field behind it; cap simultaneous
live-glass layers (~≤6 large surfaces); dense lists/tables use the solid `--surface` tier, not
glass.
