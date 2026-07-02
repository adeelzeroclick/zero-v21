# Zero Design System — Build Spec v2

A self-contained spec for reproducing the Zero (zero.xyz) marketing-site look with pixel-level fit and finish. Every block below is **copy-paste-ready code taken verbatim from the shipped pages** — do not approximate, re-derive, or "improve" values. Where a value looks oddly specific (`.3px`, `rgba(0,0,0,.13)`, `4px/9px` dashes), that specificity *is* the design.

**Page anatomy, bottom to top:**

1. `#FAFAFA` canvas (the page background)
2. Randomized "0" glyph grid (seeded SVG layer, §4)
3. Dashed vertical rails bounding the content column (§5)
4. White containers — squared, 1px `#E5E5E5` border — floating on the canvas, separated by 48px gaps (§6)
5. Edge-to-edge grid tiles *inside* containers, sharing single 1px strokes (§7)

The atmosphere comes from layers 1–3. If you skip them the page reads as a generic SaaS template; with them it reads as Zero.

---

## 1 · Tokens

Paste as-is. Everything downstream references these.

```css
:root, [data-theme="light"]{
  --bg:#FFFFFF;             /* BACKGROUND/Primary — containers stay white */
  --canvas:#FAFAFA;         /* page background behind/around the containers */
  --surface:#F7F7F7;        /* BACKGROUND/Secondary */
  --surface-2:#E5E5E5;      /* Neutral 300 */
  --card-base:#FCFCFC;      /* bg-secondary_subtle — two-tone card base */
  --text:#191919;           /* CONTENT/Primary */
  --text-2:#616161;         /* CONTENT/Secondary */
  --text-3:#8F8F8F;         /* CONTENT/Tertiary · icon grey */
  --text-4:#A3A3A3;         /* text-quaternary — card descriptions, /unit metrics */
  --border:#E5E5E5;         /* BORDER/Subtle — every solid 1px line */
  --border-soft:rgba(0,0,0,.08);
  --line:#E5E5E5;           /* bento gap lines (solid) */
  --dashed:rgba(0,0,0,.13); /* dashed rails + dashed dividers */
  --dash-len:4px;           /* dash length…   */
  --dash-step:9px;          /* …repeat period (4 on / 5 off) */
  --accent:#1919FF;         /* THE hover blue. All interactive hovers land here */
  --btn-primary-bg:#000000;
  --btn-primary-fg:#FFFFFF;
  --radius:8px;
  --space-1:4px; --space-2:8px; --space-3:12px; --space-4:16px;
  --mono:'DM Mono',ui-monospace,'SF Mono',Menlo,Consolas,monospace;
  --sans:'Inter',-apple-system,BlinkMacSystemFont,'Segoe UI',Helvetica,sans-serif;
  --heading:'DM Sans',var(--sans);
}
```

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&family=DM+Sans:wght@400;500;600;700&family=Inter:wght@400;500;600;700&display=swap">
```

```css
body{
  font-family:var(--sans); background:var(--canvas); color:var(--text);
  line-height:1.5; -webkit-font-smoothing:antialiased;
  max-width:100vw; overflow-x:hidden;   /* Safari: marquee tracks can stretch <body> and break centering */
  position:relative; margin:0;
}
```

---

## 2 · Typography

Three families, three jobs. **DM Sans** for headings, **Inter** for body, **DM Mono** for labels/buttons/metrics. DM Mono is always `500`, `+0.3px` tracking, UPPERCASE when used as a label.

| Role | Font | Size/Line | Weight | Tracking | Color |
|---|---|---|---|---|---|
| Display D2 (closer "Make your AI better…") | DM Sans | 40/1.1 | 600 | −.01em | `--text` |
| Display D3 (page title "Get started with Zero") | DM Sans | 36/1.1 | 600 | −.01em | `--text` |
| H3 / card & row title | DM Sans | 18/24 | 500 | **+.3px** | `--text` |
| Body md | Inter | 16/24 | 400 | 0 | `--text-2` |
| Body sm (section subtext, list desc) | Inter | 14/18 | 400 | `--text-2` (sections) / `--text-3` or `--text-4` (cards) |
| Label / badge / tab / metric | DM Mono | 12/18 | 500 | +.3px, UPPERCASE | `--text` or `--text-2` |
| Button label (lg 40px) | DM Mono | 13/15 | 500 | +.3px, UPPERCASE | per button |
| Small button / SEE ALL | DM Mono | 10/11 | 500 | +.3px, UPPERCASE | `--text` |
| Price | DM Mono | 13/18 | 500 | 0 | `--text` |
| /unit metric | DM Mono | 12/18 | 500 | +.3px UPPERCASE | `--text-4` |

```css
.t-display{ font-family:var(--heading); font-weight:600; font-size:36px; letter-spacing:-.01em; line-height:1.1; color:var(--text); margin:0; }
.t-h3{ font-family:var(--heading); font-weight:500; font-size:18px; line-height:24px; letter-spacing:.3px; color:var(--text); }
.t-body{ font-family:var(--sans); font-weight:400; font-size:14px; line-height:18px; color:var(--text-2); }
.t-label{ font-family:var(--mono); font-weight:500; font-size:12px; line-height:18px; letter-spacing:.3px; text-transform:uppercase; color:var(--text-2); }
```

**Fit-and-finish rules:** headings never bold (700) at H3 scale — Medium 500 + the .3px tracking is the signature. Body copy never pure black — `#616161` on sections, `#A3A3A3` in cards. Anything mono is Medium 500, never Regular.

---

## 3 · Buttons & badges

Two button tiers + one badge component. **Standing rule: primary (filled) buttons ALWAYS hover to blue `#1919FF` fill with white text. Secondary/tertiary hovers turn the TEXT blue with a 10% blue wash.**

```css
/* PRIMARY — lg (40px). GET ZERO / GET STARTED */
.btn-primary{
  display:inline-flex; align-items:center; justify-content:center; gap:4px;
  height:40px; padding:8px 16px; border-radius:10px;
  background:var(--btn-primary-bg); color:var(--btn-primary-fg); border:none; cursor:pointer;
  font-family:var(--mono); font-size:13px; line-height:15px; font-weight:500;
  letter-spacing:.3px; text-transform:uppercase; text-decoration:none; white-space:nowrap;
  transition:background-color 140ms ease, color 140ms ease;
}
.btn-primary:hover{ background:#1919FF; color:#fff; }

/* SECONDARY — lg. START WITH YOUR AGENT */
.btn-secondary{
  display:inline-flex; align-items:center; justify-content:center; gap:4px;
  height:40px; padding:8px 16px; border-radius:8px;
  background:var(--surface); color:var(--text); border:1px solid var(--border); cursor:pointer;
  font-family:var(--mono); font-size:13px; line-height:15px; font-weight:500;
  letter-spacing:.3px; text-transform:uppercase; text-decoration:none; white-space:nowrap;
  transition:background-color 140ms ease, color 140ms ease;
}
.btn-secondary:hover{ color:#1919FF; background-color:rgba(25,25,255,.10); }

/* SMALL secondary — 32px. TRY IT / BROWSE ALL */
.btn-sm{
  display:inline-flex; align-items:center; justify-content:center;
  height:32px; padding:0 12px; border-radius:8px;
  background:var(--bg); color:var(--text); border:1px solid var(--border); cursor:pointer;
  font-family:var(--mono); font-size:10px; line-height:11px; font-weight:600;
  letter-spacing:.02em; text-transform:uppercase; text-decoration:none; white-space:nowrap;
  transition:background-color 140ms ease, color 140ms ease;
}
.btn-sm:hover{ color:#1919FF; background-color:rgba(25,25,255,.10); }

/* BADGE — 22px, radius 6. First badge outlined (x402), rest filled (category) */
.badge{ display:inline-flex; align-items:center; height:22px; padding:2px 8px; box-sizing:border-box; border-radius:6px;
  font-family:var(--mono); font-weight:500; font-size:12px; line-height:18px; letter-spacing:.3px; text-transform:uppercase; color:var(--text); }
.badge.is-outline{ background:var(--bg); border:1px solid rgba(0,0,0,.1); }
.badge.is-fill{ background:var(--surface); }
```

Text links (tile titles, SEE ALL, nav links) hover to `color:#1919FF` — text only, no fill.

---

## 4 · Randomized zero background

The page floor is a field of tiny hand-drawn "0" glyphs (ellipse + slash) at randomized low opacities. **Seeded** randomness — the same cell always renders the same glyph, so resizes/re-renders don't shimmer. ~50% of cells are empty. 14px cell grid, opacity 0.035–0.10.

```css
.zero-grid-layer{ position:absolute; inset:0; z-index:0; pointer-events:none; }
.zero-grid-layer g{ stroke:#000; }
```

```js
// Mount on any positioned host that should show the field (usually the page shell).
(function(){
  const SVG_NS='http://www.w3.org/2000/svg', CELL=14;
  function rand(c,r,k){ let h=(c*73856093)^(r*19349663)^(k*83492791);
    h=Math.imul(h^(h>>>13),1274126177); h^=h>>>16; return (h>>>0)/4294967296; }
  function mount(host){
    const layer=document.createElementNS(SVG_NS,'svg');
    layer.setAttribute('class','zero-grid-layer'); layer.setAttribute('aria-hidden','true');
    host.insertBefore(layer, host.firstChild);
    let W=0,H=0;
    function render(){
      const w=host.clientWidth, h=host.clientHeight;
      if(!w||!h||(w===W&&h===H)) return; W=w; H=h;
      layer.setAttribute('width',w); layer.setAttribute('height',h);
      layer.setAttribute('viewBox','0 0 '+w+' '+h);
      const cols=Math.ceil(w/CELL), rows=Math.ceil(h/CELL), out=[];
      for(let r=0;r<rows;r++) for(let c=0;c<cols;c++){
        if(rand(c,r,1)>0.5) continue;                       // ~half the cells stay empty
        const op=(0.035+rand(c,r,2)*0.065).toFixed(3);      // 0.035–0.10
        const cx=c*CELL+CELL/2, cy=r*CELL+CELL/2;
        out.push('<g opacity="'+op+'" fill="none" stroke-width="0.8">'
          +'<ellipse cx="'+cx+'" cy="'+cy+'" rx="2.2" ry="3"/>'
          +'<line x1="'+(cx-1.4)+'" y1="'+(cy+2.3)+'" x2="'+(cx+1.4)+'" y2="'+(cy-2.3)+'"/></g>');
      }
      layer.innerHTML=out.join('');
    }
    render();
    if(typeof ResizeObserver!=='undefined'){ new ResizeObserver(()=>requestAnimationFrame(render)).observe(host); }
  }
  document.querySelectorAll('[data-zero-grid]').forEach(mount);
})();
```

**The field is bounded by the rails — it never bleeds outside them.** It lives only in the region the rails frame: between the two vertical rails, below the horizontal dashed rail (under the nav/sub-nav), and above the footer (which has its own §10 band). Do **not** mount it on `<body>` — mount it on a clipped host inside the content wrapper:

```css
/* main content wrapper starts BELOW the horizontal dashed rail */
main{ position:relative; }
.zero-field{ position:absolute; top:0; bottom:0;
  left:max(16px, calc(50% - 656px)); right:max(16px, calc(50% - 656px));   /* = the vertical rails */
  overflow:hidden; z-index:0; pointer-events:none; }
```

```html
<main>
  <div class="zero-field" data-zero-grid></div>
  <!-- containers etc., all position:relative; z-index:1 -->
</main>
```

Only bleed it full-viewport when a screen explicitly calls for it. Optional interactivity: recolor the glyph under the cursor `stroke:#1919FF` at `opacity:.55` with its 8 neighbors at `.22`.

---

## 5 · Dashed rails

Two page-height dashed 1px verticals bound the content column, plus dashed horizontal separators. Built with `linear-gradient` dashes — **not** `border-style:dashed` (browser dashes are the wrong rhythm). 4px dash / 5px gap.

```css
/* vertical rails — 656px from center on wide screens, clamped to 16px inset on narrow */
body::before, body::after{
  content:''; position:absolute; top:0; bottom:0; width:1px; z-index:50; pointer-events:none;
  background-image:linear-gradient(to bottom, var(--dashed) var(--dash-len), transparent var(--dash-len));
  background-size:1px var(--dash-step); background-repeat:repeat-y;
}
body::before{ left:max(16px, calc(50% - 656px)); }
body::after{ left:auto; right:max(16px, calc(50% - 656px)); }

/* dashed horizontal separators (below the nav/section bars) */
.hline-bottom{ position:relative; }
.hline-bottom::after{
  content:''; position:absolute; left:0; right:0; bottom:0; height:1px; pointer-events:none;
  background-image:linear-gradient(to right, var(--dashed) var(--dash-len), transparent var(--dash-len));
  background-size:var(--dash-step) 1px; background-repeat:repeat-x;
}
```

The content column itself is `max-width:1440px; margin:0 auto; padding:0 64px`. Nav/section bars use `padding-left/right:max(28px, calc(50% - 616px))` so their content aligns 40px inside the rails.

---

## 6 · Bento white containers

Sections are **squared white boxes** (no radius, no shadow) with a 1px `#E5E5E5` border, floating on the canvas + zero field, separated by **48px** gaps. Multi-cell bentos are CSS grids with **1px gaps whose background paints the lines** — cells never carry their own borders, so lines are always exactly 1px.

```css
.container{ background:var(--bg); border:1px solid var(--border); position:relative; z-index:1; }
.bento-gap{ height:48px; }   /* the canvas + zero field shows through here */

/* multi-cell bento: the 1px gap IS the divider */
.bento{ position:relative; display:grid; grid-template-columns:repeat(4,1fr); gap:1px;
  background:var(--line); border:1px solid var(--line); }
.bento-cell{ position:relative; background:var(--bg); padding:22px 24px; min-width:0; display:flex; flex-direction:column; }
.bento-span2{ grid-column:span 2; } .bento-span4{ grid-column:span 4; }
```

Container headers (title + tabs + description) get `padding:24px` all around.

**The first container on every screen sits 24px below the horizontal dashed rail** — give the content column `padding-top:24px`. The container must never touch the hline.

**Stroke ownership — every visible line has exactly ONE owner.** The classic mistake that makes bentos look "thick": a grid painting its own outer border while sitting inside a bordered `.container` → 2px edges. The rule: the **container owns the outer frame; anything inside paints only *internal* lines.** See §7 for the tile-grid version of this rule.

---

## 7 · Grid tiles inside containers

Tile grids run **edge-to-edge** inside their container, and every stroke has exactly one owner so every line is exactly 1px:

- **Inside a bordered `.container`** (the normal case): the container's border is the outer frame. The grid paints only **internal** lines — `border-top` on the grid (the divider against the header above), tiles paint `border-right` + `border-bottom`, and the **last column drops its right border, the last row drops its bottom border**. If you skip these two drops the container edges render 2px thick.
- **Standalone** (no container): the grid owns `border-top` + `border-left`, tiles own `border-right` + `border-bottom`.

```css
.tile-grid{ display:grid; grid-template-columns:repeat(4,1fr);
  border-top:1px solid var(--border); }                             /* divider vs the header above */
/* inside a container: suppress the outer strokes the container already draws */
.container .tile-grid .tile:nth-child(4n){ border-right:0; }        /* last column (match n to column count) */
.container .tile-grid .tile:nth-last-child(-n+4){ border-bottom:0; }/* last row — ONLY when the grid is the container's last child */
/* standalone grid: add border-left to .tile-grid and keep all tile borders */
.tile{ border-right:1px solid var(--border); border-bottom:1px solid var(--border);
  background:var(--card-base); display:flex; flex-direction:column; min-height:212px;
  text-decoration:none; color:inherit; transition:background 140ms; }
.tile-body{ flex:1; background:var(--bg); border-bottom:1px solid var(--border);
  padding:20px; display:flex; flex-direction:column; gap:16px; }
.tile-title{ font-family:var(--heading); font-weight:500; font-size:18px; line-height:24px;
  letter-spacing:.3px; color:var(--text); transition:color 140ms; }
.tile-desc{ font-family:var(--sans); font-weight:400; font-size:14px; line-height:18px; color:var(--text-4); margin:0; }
.tile-foot{ padding:16px; display:flex; align-items:center; justify-content:flex-end; gap:16px; }
.tile-metric{ display:inline-flex; align-items:center; gap:4px; font-family:var(--mono);
  font-weight:500; font-size:12px; line-height:18px; color:var(--text-2); white-space:nowrap; }
.tile-metric svg{ width:16px; height:16px; flex:none; }

/* hover — blue-tinted white wash + blue title (identical across ALL tiles/rows sitewide) */
.tile[href]:hover{ background:linear-gradient(rgba(25,25,255,.01), rgba(25,25,255,.01)) var(--bg); }
.tile[href]:hover .tile-title{ color:#1919FF; }

/* EMPTY cells — white, with fine DASHED internal dividers (1px, rgba(0,0,0,.10), 4 on / 5 off).
   Only INTERNAL verticals dash; the outer frame stays solid. */
.tile.is-empty{ pointer-events:none; background:var(--bg); }
.tile.is-empty:not(:last-child){
  border-right-color:transparent;
  background-image:repeating-linear-gradient(to bottom, rgba(0,0,0,.10) 0 4px, transparent 4px 9px);
  background-repeat:no-repeat; background-size:1px 100%; background-position:100% 0;
  background-origin:border-box; background-clip:border-box;
}
```

Pad the last row with `<span class="tile is-empty"></span>` so the grid always closes as a full rectangle. Icons in tiles come from **Zero Design System 1.2** only — see §8 for the sourcing workflow and the starter set.

---

## 8 · Icons — Zero Design System 1.2

**Single source of truth:** the Figma file **"Zero Design System 1.2"** — file key `63NBbuyvIdvN9o8Gt0mw20`. Icons are 16×16 components named `Icons / <Name>` (e.g. `Icons / Star`, `Icons / Grid`, `Icons / Currency--dollar`, `Icons / Activity`, `Icons / Chevron--right`). Never redraw an icon from memory, never substitute Feather/Lucide/Heroicons lookalikes — the DS shapes are visibly different (compare the DS `$` glyph or the four rounded squares of Grid against any generic set).

### Getting an icon (hosted mirror — no Figma needed)

**All 2,052 DS icons are extracted, normalized, and hosted.** This is the fastest path and requires nothing but HTTP:

- **Browse:** https://adeelzeroclick.github.io/zero-v21/icons/ — gallery of every icon by category, click any to open its SVG
- **Manifest:** https://adeelzeroclick.github.io/zero-v21/icons/icons.json — `[{name, slug, category}]` for programmatic lookup
- **Fetch one:** `https://adeelzeroclick.github.io/zero-v21/icons/<slug>.svg` (e.g. `star`, `grid`, `currency--dollar`, `activity`, `chevron--right`, `alarm--add`). Slugs are usually the lowercase DS name, but a few are irregular (`Edit with ai` → `edit-with-ai`, `Moon --filled` → `moon---filled`) — **when in doubt, look the exact slug up in the manifest instead of guessing.**

```bash
curl -s https://adeelzeroclick.github.io/zero-v21/icons/star.svg
```

Files are **already normalized** — `viewBox="0 0 16 16"`, `fill="currentColor"`, no hardcoded colors — so inline the response directly into your HTML (as an `<svg>` or `<symbol>`) and it inherits the surrounding text color. Don't hotlink via `<img>` (that renders literal black and can't inherit color); fetch and inline.

Caveats: 6 icons have non-16×16 viewBoxes (`gear`, `windows-hosting`, `automation--decision`, `subsecond` are 32×32; `batch-job--step`, `summarize-with-ai` are off-grid) — keep their own viewBox. `apple` is the fruit (Commerce); the Apple logo is `apple-logo`.

### Getting an icon (Figma MCP connected)

1. **Locate the node.** Either select the icon in Figma and use its node URL, or call `get_design_context` on a component/screen that uses it — the response lists each icon as an asset constant:
   ```
   const imgIconsStar = "https://www.figma.com/api/mcp/asset/<id>";
   ```
   (You can also find icons by name via the design-system search tool if your client has it.)
2. **Fetch the SVG.** The asset URL returns raw SVG — `curl -s <url> > star.svg`. URLs are short-lived (~7 days); fetch and inline, don't hotlink.
3. **Normalize it.** The raw export looks like:
   ```svg
   <svg preserveAspectRatio="none" width="100%" height="100%" overflow="visible" style="display: block;"
        viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
     <g id="Icons / Star"><path id="icon" d="…" fill="var(--fill-0, #8F8F8F)"/></g>
   </svg>
   ```
   Keep only the `viewBox` and the `<path>` data; drop the sizing/style attributes and the wrapper `<g>`; replace every hardcoded fill (`var(--fill-0, …)`, `#8F8F8F`, `black`) with **`fill="currentColor"`**:
   ```svg
   <svg viewBox="0 0 16 16" fill="currentColor" xmlns="http://www.w3.org/2000/svg" aria-hidden="true"><path d="…"/></svg>
   ```
4. **Size with CSS, color with `color`.** Icons render at 16×16 (`svg{ width:16px; height:16px; }`) and inherit the metric/label color (`--text-3` next to `--text-2` text is the standard pairing). Reused icons go in one hidden `<symbol>` sheet and are placed with `<use href="#ic-star"/>`.

### Starter set (already extracted and normalized)

These four cover tiles, lists, and metrics — copy as-is if Figma isn't connected:

```html
<svg style="display:none" aria-hidden="true">
  <symbol id="ic-grid" viewBox="0 0 16 16"><path fill="currentColor" d="M6 2H3C2.73478 2 2.48043 2.10536 2.29289 2.29289C2.10536 2.48043 2 2.73478 2 3V6C2 6.26522 2.10536 6.51957 2.29289 6.70711C2.48043 6.89464 2.73478 7 3 7H6C6.26522 7 6.51957 6.89464 6.70711 6.70711C6.89464 6.51957 7 6.26522 7 6V3C7 2.73478 6.89464 2.48043 6.70711 2.29289C6.51957 2.10536 6.26522 2 6 2ZM6 6H3V3H6V6Z"/><path fill="currentColor" d="M13 2H10C9.73478 2 9.48043 2.10536 9.29289 2.29289C9.10536 2.48043 9 2.73478 9 3V6C9 6.26522 9.10536 6.51957 9.29289 6.70711C9.48043 6.89464 9.73478 7 10 7H13C13.2652 7 13.5196 6.89464 13.7071 6.70711C13.8946 6.51957 14 6.26522 14 6V3C14 2.73478 13.8946 2.48043 13.7071 2.29289C13.5196 2.10536 13.2652 2 13 2ZM13 6H10V3H13V6Z"/><path fill="currentColor" d="M6 9H3C2.73478 9 2.48043 9.10536 2.29289 9.29289C2.10536 9.48043 2 9.73478 2 10V13C2 13.2652 2.10536 13.5196 2.29289 13.7071C2.48043 13.8946 2.73478 14 3 14H6C6.26522 14 6.51957 13.8946 6.70711 13.7071C6.89464 13.5196 7 13.2652 7 13V10C7 9.73478 6.89464 9.48043 6.70711 9.29289C6.51957 9.10536 6.26522 9 6 9ZM6 13H3V10H6V13Z"/><path fill="currentColor" d="M13 9H10C9.73478 9 9.48043 9.10536 9.29289 9.29289C9.10536 9.48043 9 9.73478 9 10V13C9 13.2652 9.10536 13.5196 9.29289 13.7071C9.48043 13.8946 9.73478 14 10 14H13C13.2652 14 13.5196 13.8946 13.7071 13.7071C13.8946 13.5196 14 13.2652 14 13V10C14 9.73478 13.8946 9.48043 13.7071 9.29289C13.5196 9.10536 13.2652 9 13 9ZM13 13H10V10H13V13Z"/></symbol>
  <symbol id="ic-dollar" viewBox="0 0 16 16"><path fill="currentColor" d="M11.5 10.2576C11.5 7.94995 9.61 7.687 8.09145 7.47605C6.43645 7.2456 5.5 7.04605 5.5 5.62105C5.5 4.42455 6.75355 4 7.8269 4C8.36188 3.9827 8.89331 4.09281 9.37737 4.32126C9.86143 4.54972 10.2842 4.88997 10.611 5.31395L11.3891 4.68605C11.0392 4.23615 10.6038 3.85984 10.108 3.57875C9.61219 3.29765 9.06573 3.11732 8.5 3.0481V1.5H7.5V3.011C5.6924 3.1206 4.5 4.141 4.5 5.621C4.5 7.986 6.415 8.25235 7.95375 8.466C9.58 8.6924 10.5 8.8872 10.5 10.2576C10.5 11.7737 8.9337 12 8 12C6.2853 12 5.5609 11.5181 4.88905 10.6861L4.11095 11.3139C4.50735 11.8363 5.02004 12.259 5.60832 12.5486C6.19661 12.8383 6.8443 12.9868 7.5 12.9824V14.5H8.5V12.9775C10.3628 12.8254 11.5 11.814 11.5 10.2576Z"/></symbol>
  <symbol id="ic-star" viewBox="0 0 16 16"><path fill="currentColor" d="M7.99999 3.26L9.37999 6.05L9.60998 6.55L10.11 6.625L13.19 7.07L11 9.22L10.625 9.585L10.715 10.085L11.24 13.15L8.48499 11.705L7.99999 11.5L7.53499 11.745L4.77999 13.17L5.27999 10.105L5.36999 9.605L4.99999 9.22L2.78999 7.045L5.86999 6.6L6.36999 6.525L6.59999 6.025L7.99999 3.26ZM7.99999 1L5.72499 5.61L0.639985 6.345L4.31999 9.935L3.44999 15L7.99999 12.61L12.55 15L11.68 9.935L15.36 6.35L10.275 5.61L7.99999 1Z"/></symbol>
  <symbol id="ic-act" viewBox="0 0 16 16"><path fill="currentColor" d="M6 14.5C5.90129 14.4995 5.80493 14.4698 5.72307 14.4146C5.64121 14.3595 5.57751 14.2813 5.54 14.19L3.165 8.5H1V7.5H3.5C3.59871 7.50049 3.69507 7.53019 3.77693 7.58536C3.85879 7.64052 3.92249 7.71869 3.96 7.81L6 12.64L10.03 1.825C10.0658 1.7293 10.13 1.64685 10.214 1.58874C10.298 1.53064 10.3978 1.49967 10.5 1.5C10.6031 1.5018 10.7031 1.53544 10.7864 1.59631C10.8696 1.65717 10.932 1.74229 10.965 1.84L12.86 7.5H15V8.5H12.5C12.3952 8.50027 12.2929 8.46759 12.2077 8.40658C12.1224 8.34557 12.0585 8.25931 12.025 8.16L10.5 3.5L6.47 14.175C6.43424 14.2707 6.37003 14.3531 6.286 14.4113C6.20197 14.4694 6.10216 14.5003 6 14.5Z"/></symbol>
</svg>
```

Usage: `<span class="tile-metric"><svg><use href="#ic-star"/></svg>4.7</span>`

---

## 9 · Navigation

64px bar, white, solid 1px bottom border. Left: the Zero wordmark (real SVG below, 21px tall, `#191919`). Right: DM Mono uppercase links, a 22px social divider, and the GET ZERO primary button (which hovers blue like every primary).

```css
.nav{ height:64px; flex-shrink:0; display:flex; align-items:center; justify-content:space-between;
  padding-left:max(28px, calc(50% - 616px)); padding-right:max(28px, calc(50% - 616px));
  background:var(--bg); border-bottom:1px solid var(--border); position:relative; z-index:2; }
.nav-logo{ display:inline-flex; align-items:center; color:#191919; text-decoration:none; }
.nav-logo .brand-mark{ height:21px; width:auto; display:block; }
.nav-right{ display:flex; align-items:center; gap:24px; }
.nav-link{ font-family:var(--mono); font-size:12px; font-weight:500; letter-spacing:.06em;
  text-transform:uppercase; color:var(--text); text-decoration:none; transition:color 140ms ease; }
.nav-link:hover{ color:#1919FF; }
```

The logo **must** be the real wordmark — the interlocking double-O mark plus ZERO letterforms:

```html
<a class="nav-logo" href="index.html" aria-label="Zero">
  <svg class="brand-mark" viewBox="0 0 469 126" fill="currentColor" role="img" aria-label="Zero" xmlns="http://www.w3.org/2000/svg">
    <g clip-path="url(#zlogo-clip)"><path d="M113.535 9.38561C109.723 5.5733 105.234 2.70617 100.269 0.749792C119.711 20.255 113.484 58.0421 86.3412 85.1848C59.1793 112.346 21.3577 118.564 1.86476 99.0708C1.00352 98.2095 0.193973 97.3108 -0.567303 96.3805C1.3075 102.901 4.60263 108.75 9.38674 113.534C29.8384 133.986 69.7321 127.251 98.4919 98.4914C127.252 69.7317 133.987 29.8373 113.535 9.38561ZM90.3919 16.3303C76.9704 2.90934 49.5113 8.60859 29.0601 29.0596C8.60889 49.5108 2.90974 76.9699 16.3308 90.3915C22.5393 96.6 31.7515 98.7145 41.8847 97.1725C39.0476 96.0075 36.4702 94.3324 34.266 92.1282C22.4425 80.3045 25.8104 57.7665 41.788 41.7889C57.7656 25.8115 80.3029 22.4441 92.1265 34.2676C94.3312 36.4723 96.0072 39.0499 97.1722 41.8877C98.7152 31.7533 96.6011 22.5395 90.3919 16.3303Z"/></g>
    <path d="M427.083 88.2917C430.063 88.2917 432.878 87.7399 435.527 86.6361C438.286 85.5324 440.714 83.9319 442.811 81.8348C444.908 79.6274 446.564 76.9232 447.778 73.7223C448.992 70.5215 449.599 66.824 449.599 62.6297C449.599 58.4355 448.992 54.7932 447.778 51.7027C446.564 48.5018 444.908 45.8529 442.811 43.7557C440.714 41.5483 438.286 39.9478 435.527 38.9545C432.878 37.8507 430.063 37.2989 427.083 37.2989C424.103 37.2989 421.233 37.8507 418.474 38.9545C415.825 39.9478 413.452 41.5483 411.355 43.7557C409.368 45.8529 407.712 48.5018 406.388 51.7027C405.174 54.7932 404.567 58.4355 404.567 62.6297C404.567 66.824 405.174 70.5215 406.388 73.7223C407.712 76.9232 409.368 79.6274 411.355 81.8348C413.452 83.9319 415.825 85.5324 418.474 86.6361C421.233 87.7399 424.103 88.2917 427.083 88.2917ZM427.083 19.9149C433.153 19.9149 438.727 21.0187 443.805 23.2261C448.882 25.3233 453.297 28.3034 457.049 32.1664C460.802 35.9192 463.727 40.3893 465.824 45.5769C467.921 50.7645 468.97 56.4488 468.97 62.6297C468.97 68.8107 467.921 74.5501 465.824 79.8481C463.727 85.0357 460.802 89.561 457.049 93.4241C453.297 97.1769 448.882 100.157 443.805 102.364C438.727 104.462 433.153 105.51 427.083 105.51C421.123 105.51 415.549 104.462 410.361 102.364C405.284 100.157 400.869 97.1769 397.116 93.4241C393.474 89.561 390.604 85.0357 388.507 79.8481C386.41 74.5501 385.361 68.8107 385.361 62.6297C385.361 56.4488 386.41 50.7645 388.507 45.5769C390.604 40.3893 393.474 35.9192 397.116 32.1664C400.869 28.3034 405.284 25.3233 410.361 23.2261C415.549 21.0187 421.123 19.9149 427.083 19.9149Z"/>
    <path d="M378.084 41.4377C375.877 41.1066 373.78 40.941 371.793 40.941C364.287 40.941 358.769 42.9829 355.237 47.0668C351.815 51.1506 350.104 57.0005 350.104 64.6163V103.027H330.899V22.3982H349.608V35.3119C351.484 30.897 354.519 27.4754 358.714 25.0471C362.908 22.6189 367.654 21.4048 372.952 21.4048C374.166 21.4048 375.215 21.46 376.098 21.5704C376.981 21.6807 377.643 21.7911 378.084 21.9015V41.4377Z"/>
    <path d="M294.831 53.855C294.721 51.5371 294.224 49.3296 293.341 47.2325C292.569 45.025 291.355 43.0935 289.699 41.4379C288.043 39.7823 286.002 38.4578 283.573 37.4644C281.145 36.471 278.275 35.9744 274.964 35.9744C271.984 35.9744 269.28 36.5262 266.852 37.63C264.534 38.6233 262.547 40.003 260.891 41.769C259.236 43.4246 257.911 45.3562 256.918 47.5637C255.925 49.6608 255.373 51.7579 255.262 53.855H294.831ZM313.209 80.0137C312.105 83.5456 310.505 86.8569 308.408 89.9473C306.31 93.0378 303.717 95.742 300.626 98.0599C297.536 100.378 294.004 102.199 290.03 103.523C286.057 104.848 281.642 105.51 276.785 105.51C271.267 105.51 266.024 104.572 261.057 102.696C256.09 100.709 251.73 97.8943 247.978 94.2519C244.225 90.4992 241.19 85.9739 238.872 80.6759C236.664 75.2676 235.561 69.1418 235.561 62.2986C235.561 55.8969 236.609 50.1023 238.706 44.9147C240.914 39.7271 243.839 35.3121 247.481 31.6698C251.123 27.917 255.318 25.0473 260.064 23.0606C264.81 20.9635 269.721 19.9149 274.799 19.9149C280.98 19.9149 286.498 20.9083 291.355 22.895C296.322 24.8818 300.461 27.6963 303.772 31.3386C307.193 34.981 309.787 39.396 311.553 44.5836C313.319 49.6608 314.202 55.4002 314.202 61.8019C314.202 63.3472 314.147 64.7268 314.037 65.941C313.926 67.0447 313.816 67.7069 313.705 67.9277H254.766C254.876 71.0182 255.538 73.8327 256.752 76.3713C257.967 78.9099 259.567 81.1174 261.554 82.9938C263.54 84.8701 265.803 86.3602 268.342 87.4639C270.991 88.4573 273.805 88.954 276.785 88.954C282.635 88.954 287.105 87.6295 290.196 84.9805C293.397 82.2212 295.659 78.8547 296.984 74.8813L313.209 80.0137Z"/>
    <path d="M159.29 103.027V86.305L200.184 39.2856H160.118V22.3983H224.356V38.6233L182.965 86.1394H225.183V103.027H159.29Z"/>
    <defs><clipPath id="zlogo-clip"><rect width="125.425" height="125.425" fill="white"/></clipPath></defs>
  </svg>
</a>
```

**Sub-navigation** (section bar, below the nav, dashed bottom line): left `9,999 SERVICES` (DM Mono 500 13px +.3px uppercase, `--text`) + `SEE ALL ›` (10px, chevron icon, hover `#1919FF`); right a terminal-style search: 298×38, `--surface` fill, 1px `rgba(0,0,0,.1)` border, radius 8, `>_` DM Mono prompt; **goes white on focus** and only then reveals a 24×24 black rounded-4 arrow submit button.

---

## 10 · Footer

White strip with a **solid** 1px top border (the one place a section divider is solid, not dashed). Three zones: brand row, a dashed-divider bottom row, and — the signature — a **168px zero-glyph band** at the very bottom where the randomized "0" field runs between the rails. Give the footer `padding-bottom:200px` to make room for it.

```css
.footer-strip{ position:relative; background:var(--bg); border-top:1px solid var(--border);
  padding:40px max(28px, calc(50% - 616px)) 200px; overflow:hidden; }

/* brand row: logo + Product Hunt badge (left) · link columns (right) */
.footer-main{ display:flex; justify-content:space-between; gap:48px; flex-wrap:wrap; position:relative; z-index:1; }
.footer-brand{ display:flex; flex-direction:column; gap:12px; max-width:320px; }
.footer-logo{ color:var(--text); text-decoration:none; display:inline-flex; }
.footer-logo .brand-mark{ height:22px; width:auto; display:block; }   /* same wordmark SVG as the nav, 1px taller */
.footer-cols{ display:flex; gap:56px; flex-wrap:wrap; }
.footer-col{ display:flex; flex-direction:column; gap:11px; }
.footer-col-h{ font-family:var(--mono); font-size:10px; font-weight:600; letter-spacing:.12em; text-transform:uppercase; color:var(--text-3); margin-bottom:3px; }
.footer-col a{ font-size:13px; color:var(--text-2); text-decoration:none; transition:color 140ms ease; }
.footer-col a:hover{ color:#1919FF; }

/* Product Hunt badge */
.footer-ph{ display:inline-flex; align-items:center; gap:10px; align-self:flex-start;
  margin-top:4px; padding:7px 14px 7px 10px; border-radius:8px;
  border:1px solid var(--border); background:var(--surface);
  text-decoration:none; transition:border-color 140ms ease, background 140ms ease; }
.footer-ph:hover{ border-color:var(--text-3); background:var(--surface-2); }
.footer-ph-label{ font-family:var(--mono); font-size:8px; font-weight:700; letter-spacing:.14em; text-transform:uppercase; color:var(--text-3); }
.footer-ph-rank{ font-family:var(--sans); font-size:13px; font-weight:700; color:var(--text); letter-spacing:-.01em; }
.footer-ph-text{ display:flex; flex-direction:column; line-height:1.15; }

/* bottom row — DASHED top divider, mono microcopy, SOC 2 pill */
.footer-bottom{ position:relative; display:flex; align-items:center; justify-content:space-between;
  gap:20px; margin-top:34px; padding-top:18px; z-index:1; }
.footer-bottom::before{ content:''; position:absolute; top:0; left:0; right:0; height:1px;
  background-image:linear-gradient(to right, var(--dashed) var(--dash-len), transparent var(--dash-len));
  background-size:var(--dash-step) 1px; background-repeat:repeat-x; }
.footer-copy{ font-family:var(--mono); font-size:10px; letter-spacing:.05em; color:var(--text-3); text-transform:uppercase; }
.footer-end{ display:inline-flex; align-items:center; gap:18px; flex:none; }
.footer-legal{ font-family:var(--mono); font-size:10px; font-weight:400; letter-spacing:.06em; text-transform:uppercase; color:var(--text-2); text-decoration:none; transition:color 140ms ease; }
.footer-legal:hover{ color:#1919FF; }
.footer-soc2{ display:inline-flex; align-items:center; gap:7px; height:35px; box-sizing:border-box;
  font-family:var(--mono); font-size:10px; font-weight:600; letter-spacing:.06em; text-transform:uppercase;
  color:var(--text-2); background:var(--bg); border:1px solid var(--border); border-radius:999px;
  padding:0 13px; white-space:nowrap; text-decoration:none; transition:color 140ms ease, border-color 140ms ease; }
.footer-soc2:hover{ color:var(--text); border-color:var(--text-3); }

/* the zero band — §4's glyph field clipped to a 168px strip between the rails */
.zero-grid-footer{ position:absolute; inset:auto max(16px, calc(50% - 656px)) 0 max(16px, calc(50% - 656px));
  height:168px; overflow:hidden; z-index:0; pointer-events:none; }
```

```html
<footer class="footer-strip">
  <div class="footer-main">
    <div class="footer-brand">
      <a class="footer-logo" href="index.html" aria-label="Zero"><!-- §9 wordmark SVG, .brand-mark --></a>
      <a class="footer-ph" href="https://www.producthunt.com/products/zero" target="_blank" rel="noopener">
        <span class="footer-ph-text">
          <span class="footer-ph-label">Product Hunt</span>
          <span class="footer-ph-rank">#3 Product of the Day</span>
        </span>
      </a>
    </div>
    <nav class="footer-cols" aria-label="Footer">
      <div class="footer-col">
        <span class="footer-col-h">Social</span>
        <a href="https://x.com/zeroxyz">X</a><a href="https://zero.xyz/discord">Discord</a><a href="https://zero.xyz/call">Call</a>
      </div>
      <div class="footer-col">
        <span class="footer-col-h">Resources</span>
        <a href="https://www.zero.xyz/getlisted">Get Listed</a><a href="https://www.zero.xyz/faq">FAQ</a><a href="https://www.zero.xyz/SKILL.md">SKILL.md</a>
      </div>
    </nav>
  </div>
  <div class="footer-bottom">
    <span class="footer-copy">A product of the people's internet experiment&nbsp;©&nbsp;2026</span>
    <div class="footer-end">
      <a class="footer-legal" href="#">Legal</a>
      <a class="footer-soc2" href="#">SOC&nbsp;2 · In&nbsp;Progress</a>
    </div>
  </div>
  <div class="zero-grid-footer" data-zero-grid></div>   <!-- §4 script mounts the glyph band here -->
</footer>
```

**Optional (the shipped pages do this):** the footer is `position:fixed; bottom:0; z-index:0` behind the content column (`z-index:1`), with a spacer equal to the footer's height at the end of the content — scrolling to the bottom *reveals* the footer as the page slides off it. For simpler pages a static footer with the same anatomy is fine.

---

## 11 · Fit-and-finish checklist

- [ ] Every 1px line is exactly 1px — one owner per stroke. Inside a bordered container, inner grids/lists paint only INTERNAL lines (last column: no right border; last row / last list row: no bottom border). Check every container edge at zoom — 2px edges are the #1 giveaway.
- [ ] First container on the screen sits 24px below the horizontal dashed rail — never touching it.
- [ ] Zero-glyph field stays INSIDE the rail-bounded region (between the vertical rails, below the hline, above the footer) — no full-bleed unless the screen explicitly calls for it.
- [ ] Dashes are 4px on / 5px off (`--dash-len:4px; --dash-step:9px`), color `rgba(0,0,0,.13)` for rails, `rgba(0,0,0,.10)` inside grids.
- [ ] All hovers land on `#1919FF`: filled buttons → blue fill; text/secondary → blue text (+10% blue wash on buttons); tiles → blue-tinted white + blue title.
- [ ] All mono text is DM Mono **500**, `+.3px`, uppercase when a label.
- [ ] White containers are squared — **no border-radius, no shadows** on section boxes. Radius lives only on buttons (8–10), badges (6), icons (10–11).
- [ ] Zero-glyph field visible in the canvas gaps between containers and behind the page.
- [ ] Transitions: 140ms ease on color/background, 160ms on transform nudges. Nothing slower on hover.
- [ ] Icons only from Zero Design System 1.2, sourced per §8 (16×16, `currentColor`, never redrawn).
- [ ] Footer: solid top border, dashed internal divider, mono-uppercase microcopy, and the 168px zero-glyph band at the very bottom between the rails.
