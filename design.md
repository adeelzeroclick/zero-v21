# Zero — v21 Design System

A hand-off spec for building sites and UI that feel **on-brand with Zero v21**. Give this file to your agent (Claude, etc.) as context. It is self-contained: copy the token block, follow the rules, reuse the component recipes.

**The feel in one line:** clean, technical, and quiet. White bento containers on a near-white canvas, hairline 1px borders, monospace labels, and a single electric-blue accent that only shows on interaction. Restrained, precise, "developer-grade product."

---

## 1. Foundations

### 1.1 Fonts (load these three from Google Fonts)
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&family=DM+Sans:wght@400;500;600;700&family=Inter:wght@400;500;600;700&display=swap">
```

| Role | Family | Used for |
|---|---|---|
| **Headings** | **DM Sans** | Page/section titles, card titles, big numbers, stat values |
| **Body** | **Inter** | Paragraphs, descriptions, most UI text |
| **Labels / buttons / code / metrics** | **DM Mono** | Buttons, tags, kickers, prices, breadcrumbs, nav links, code, numeric stats |

Rule of thumb: **if it's a label, a number, a button, or "machine" text → DM Mono. If it's a heading → DM Sans. Everything else → Inter.** DM Mono labels are almost always **UPPERCASE** with positive letter-spacing.

### 1.2 Design tokens — drop this into `:root`
```css
:root{
  /* surfaces */
  --bg:#FFFFFF;            /* white — section / bento containers */
  --canvas:#FAFAFA;        /* page background behind the containers */
  --surface:#F7F7F7;       /* subtle fill: chips, hover, code blocks, "without Zero" column */
  --surface-2:#E5E5E5;     /* heavier fill / icon-tile backgrounds */

  /* text */
  --text:#191919;          /* primary text */
  --text-2:#616161;        /* secondary text / descriptions */
  --text-3:#8F8F8F;        /* tertiary text / mono labels / units */

  /* lines */
  --border:#E5E5E5;        /* every hairline stroke is 1px of this (or 0.5px on buttons) */
  --dashed:rgba(0,0,0,.13);/* dashed rails / empty-cell dividers */

  /* type families */
  --sans:'Inter',-apple-system,BlinkMacSystemFont,'Segoe UI',Helvetica,sans-serif;
  --heading:'DM Sans',var(--sans);
  --mono:'DM Mono',ui-monospace,'SF Mono',Menlo,Consolas,monospace;

  /* buttons */
  --btn-primary-bg:#000000;  --btn-primary-fg:#FFFFFF;
  --btn-secondary-bg:#FFFFFF; --btn-secondary-fg:#191919;

  /* accent + status */
  --accent:#1919FF;   /* electric blue — INTERACTION ONLY (hover/focus), never as a resting fill */
  --ok:#3E9D6E;       /* success / "shipped" / live (green) */
  --bad:#C0564B;      /* error / "stalled" (muted red) */
  --star:#E0A030;     /* rating star (gold) */
}
```

### 1.3 Dark mode (optional, set `[data-theme="dark"]`)
```css
[data-theme="dark"]{
  --bg:#000000; --canvas:#0C0F11; --surface:#191919; --surface-2:#262626;
  --text:#F7F7F7; --text-2:#8F8F8F; --text-3:#616161; --border:#363636;
  --dashed:rgba(255,255,255,.15);
  --btn-primary-bg:#F7F7F7; --btn-primary-fg:#000000;
  --btn-secondary-bg:#191919; --btn-secondary-fg:#F7F7F7;
}
```

---

## 2. Typography scale

| Token | Family / weight / size / line-height / tracking | Use |
|---|---|---|
| Display / hero | DM Sans 700 · `clamp(34px,4vw,52px)` · 1.1 · `-0.022em` | Hero headline |
| Section title | **DM Sans 600 · 29px · 1.14 · -0.022em** | "Featured services", "Services used", "How this got built" |
| Card / item title | DM Sans 600 · 15px · 1.3 · -0.005em | List item & card names |
| Body / lead | Inter 400 · 15–16px · 1.55 · `--text-2` | Section sub-copy, taglines |
| Small body | Inter 400 · 12.5–13px · 1.45–1.5 · `--text-2` | Descriptions inside cards |
| **Mono label** | **DM Mono 500 · 9–12px · UPPERCASE · `+0.06em` · `--text-3`** | Kickers, tags, nav links, "ZERO PICKS", breadcrumbs |
| Metric value | DM Mono 500–600 · 11–24px · `--text` | Prices, counts, stat numbers |

Notes:
- Headings use **DM Sans 600** (not 700) at the section level. Big hero text can go 700.
- Mono labels: weight **500** (DM Mono tops out at 500 — never request 600/700, it faux-bolds).
- Negative letter-spacing on DM Sans headings (`-0.022em`); positive on DM Mono labels (`+0.04–0.10em`).

---

## 3. Layout & surface rules

1. **Bento containers.** Content lives in white (`--bg`) boxes sitting on the `--canvas` page. Square corners (no border-radius on the big section boxes), 1px `--border` frame. Think Figma-mockup framing, not soft cards.
2. **Hairlines everywhere, exactly 1px.** All structural strokes are `1px solid var(--border)`. Buttons and chat bubbles use `0.5px`. **Never double a stroke** — where a grid meets a box edge, let ONE element own the line (drop the box border or the cell border, not both). Inconsistent/doubled strokes are the #1 thing that breaks the look.
3. **Edge-to-edge grids.** Inside a padded box, card grids and comparison tables break out to the box's inner edge (the box's own 1px border becomes the single outer frame). Headings/intro stay inset; the grid runs full-width and squared.
4. **Dashed = empty/placeholder.** Real dividers are solid `--border`; **empty/placeholder cells and "nothing here yet" dividers use `dashed`**. The outer container frame always stays solid.
5. **Generous, even whitespace.** Section padding ~`36–48px`; mobile ~`24px`. Go edge-to-edge (`padding:0`) below ~1024px so boxes are full-bleed on mobile.

---

## 4. Components (recipes)

### 4.1 Buttons — the Zero button is **uppercase DM Mono**
This is the signature. Match it exactly.
```css
.btn{
  display:inline-flex; align-items:center; justify-content:center; gap:8px; /* gap:4px if text-only */
  height:40px; padding:0 16px; border-radius:10px;
  font-family:var(--mono); font-weight:500; font-size:13px; line-height:15px;
  letter-spacing:.3px; text-transform:uppercase; white-space:nowrap;
  cursor:pointer; text-decoration:none;
  box-shadow:0 1px 2px rgba(0,0,0,.05);
  transition:background-color 140ms, color 140ms, border-color 140ms;
}
.btn-primary{ color:#fff; background:#000; border:.5px solid #000; }
.btn-primary:hover{ background:var(--accent); border-color:var(--accent); color:#fff; }  /* turns blue */
.btn-secondary{ color:var(--text); background:var(--bg); border:.5px solid var(--border); }
.btn-secondary:hover{ color:var(--accent); background:rgba(25,25,255,.10); }              /* blue text on 10% blue */
.btn:focus-visible{ outline:none; box-shadow:0 0 0 1px var(--bg), 0 0 0 3px rgba(9,104,246,.45); }
```
- **Primary** = solid black, **Secondary** = white/outline. Sizes: `sm 24px · md 32px · lg 40px`.
- **Hover is where the brand blue lives:** primary fills blue, secondary goes blue-on-light-blue. Nothing is blue at rest.
- Icons in buttons are 16px slots, but the glyph is **inset** (don't stretch a 12px glyph to 16px). Use `currentColor`.

### 4.2 Tags / pills / kickers
Small mono uppercase chips. Bordered (`1px var(--border)`, bg `--surface` or `--surface-2`), `border-radius:5–6px`, `font-size:8.5–9px`, weight 600, `+0.08em`, `--text-3`.
- A **"ZERO PICKS"** style highlight tag uses green: `color:#3E9D6E; background:rgba(62,157,110,.10)`.
- Category tags (`WEB`, `DATA`, `COMMS`, `OPS`, `MEDIA`) are this same chip.

### 4.3 Status badges
Pill, mono uppercase, 10% tint of the status color:
- **Shipped / Live / success →** `--ok` green on `rgba(62,157,110,.10)`, often with a check `✓` or a green dot.
- **Stalled / error →** `--bad` red on `rgba(192,86,75,.10)`, often with a pulsing dot.

### 4.4 Cards & list rows
- White, `1px var(--border)`, square or `8–12px` radius for inner cards, subtle `box-shadow:0 1px 2px rgba(0,0,0,.04)`.
- **Search-result row** (used for service/provider lists): `[colored letter-avatar 28px] [name (DM Sans 600)] [category tag] [✓ if picked] [price right: DM Mono, value bold + grey unit] / [description below]`. The picked row gets a `--surface` highlight; alternates are plain with thin dividers and a "+ N more on Zero →" link.
- **Letter avatar:** rounded-square (`border-radius:8px`), the provider's initial in white, background = a color hashed from the name (keep a 6–7 color palette so it's stable per provider).

### 4.5 Chat bubbles (the "ChatMessage" component)
Tail shape `border-radius:12px 12px 4px 12px`, `0.5px var(--border)`, Inter. Two variants:
- **default (assistant):** white bg + drop shadow `0 1px 1px rgba(0,0,0,.05)` + inner `inset 0 -1px 0 rgba(0,0,0,.1)`.
- **active (user / "you typed"):** black bg, white text, no shadow.

### 4.6 Stats row
A horizontal row of cells split by 1px vertical dividers (first cell no left border). Value in **DM Mono 500 ~24px** (`--text`), label below in **DM Mono ~10px UPPERCASE** (`--text-3`).

### 4.7 Build plan / timeline
Numbered steps down the left in a vertical line (mono numbers on `--canvas`), each paired with a white step card to the right. Capability cards: title + price/unit, a tag row (`CAPABILITY · x402 · ●PROVIDER · ★rating · ⤳success%`), a divided body with a blue `POST` method badge + mono endpoint + grey description.

### 4.8 Reveal motion
Sequences (e.g. a chat or plan filling in) use a staggered fade-up: each item `opacity:0; translateY(6px)` → animates in on scroll-into-view (`IntersectionObserver`), `~0.32s` stagger. **Always include a `@media (prefers-reduced-motion:reduce)` branch that shows everything with no animation.** Keep motion subtle (140ms transitions, small translateY); no bounce.

---

## 5. Iconography
- 16px line icons, ~1.8–2px stroke, `stroke="currentColor"` (or fill `currentColor`) so they inherit text color and go blue on hover.
- Inside buttons the icon glyph keeps its true size within a 16px slot — don't uniformly upscale.
- Decorative registration-mark squares and dotted/diagonal background textures are part of the Figma-mockup vibe but use them sparingly; default to clean white.

---

## 6. Voice & copy
- Plain, confident, lowercase-friendly, slightly technical. Short verb-first lines ("Discover services, call them, payments handled automatically.").
- UI labels are UPPERCASE mono ("GET ZERO", "SEE MORE CAPABILITIES", "COST / RUN").
- Prices show the unit in grey mono ("**$0.04** /call"). Counts and metrics are mono.

---

## 7. Do / Don't
**Do:** white boxes on `--canvas` · uniform 1px hairlines · DM Mono uppercase labels & buttons · blue only on hover/focus · square section corners · dashed only for empty states · even whitespace.

**Don't:** use Inter for buttons (they're DM Mono) · request DM Mono weight > 500 · make anything blue at rest · double up borders where a grid meets a box · use heavy drop shadows or rounded "bubbly" cards · introduce a second accent color.

---

## 8. Quick-start skeleton
```html
<body style="background:var(--canvas); color:var(--text); font-family:var(--sans);">
  <section class="bento" style="background:var(--bg); border:1px solid var(--border); padding:44px 48px;">
    <span class="kicker" style="font-family:var(--mono);font-size:11px;letter-spacing:.06em;text-transform:uppercase;color:var(--text-3);">Section label</span>
    <h2 style="font-family:var(--heading);font-weight:600;font-size:29px;letter-spacing:-.022em;margin:.4em 0 0;">Section title</h2>
    <p style="font-family:var(--sans);font-size:15px;line-height:1.55;color:var(--text-2);max-width:640px;">Supporting copy in Inter.</p>
    <a class="btn btn-primary" href="#">Get Zero</a>
  </section>
</body>
```

Build on these and it will read as Zero v21.
