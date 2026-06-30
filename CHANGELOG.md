# Changelog — install-v17 clone → v18

All changes from the original `site.withzero.ai/s/install-v17` clone to the current state.
Deployed as **https://site.withzero.ai/s/install-v18-claude**.

## Design system foundation (from ZER0-Design-System Figma)

1. **Color tokens** — text remapped to the neutral ramp (`#191919` / `#616161` / `#8F8F8F`), surfaces `#F7F7F7` / `#E5E5E5`, borders Subtle `#E5E5E5` + Medium `#8F8F8F`; dark-mode tokens aligned (`#000` / `#191919` backgrounds, `#F7F7F7` / `#8F8F8F` content).
2. **Spacing tokens** — PADDING scale (4/8/12/16/24/32/48/96/128) and `--radius: 8` added as CSS variables.
3. **Typography** — DM Sans added as the heading family; all headings (hero, every section title, h1–h4) set to DM Sans Semibold, large titles at −0.3px tracking; "Featured services" title upsized to match the education title (29px, 26px on mobile).
4. **Button system** (Figma 123-393) — three tiers (Large 40px/13px, Medium 32px/10px, Small 24px) in DM Mono uppercase; Primary black with white-overlay hover, Secondary white with 0.5px border, Ghost transparent. Later reassignments: nav Get Zero → Large, "Copy prompt" → Large primary, "More projects" → Large secondary, "Browse all" → Medium secondary, "Try it" → Medium ghost.
5. **Modal** (4092-1641) — white panel, 0.5px subtle border, radius 16, 24px padding, layered elevation-4 shadow, DM Sans 20/24 title, 24px circular close button.
6. **Code block** (4125-5282) — prompt snippets restyled to `#F7F7F7` fill, 0.5px border, radius 12, 16px padding, DM Mono 12/18.
7. **Tab groups** (4056-2999 / 4056-3138) — HUMAN/MACHINE toggle and footer theme toggle rebuilt as tab groups (grey pill group, white active tab with drop + inset shadows); theme toggle uses design-system icons (Laptop / Light / Asleep).
8. **Badges** (4177-6738) — service tag chips became the Badge component (white pill, 22px, 1px 10%-black stroke, DM Mono 12/18).
9. **Design-system icons** (179-662) — chat header laptop icon, FAQ +/− (Add/Subtract geometry, collapse animation preserved), education checkmarks (in `#1919FF`), project-card Copy icon.

## Layout & chrome

10. Corner registration squares: first reduced to outer corners only, later removed entirely; banner edge caps hidden.
11. Bento dividers made dashed (45° repeating-gradient trick); education section's internal divider removed entirely.
12. Hero: "works with" row optically centered (−16px pull), then hero bottom padding cut to 24px so the row hugs the box edge.
13. Nav: vertical divider between social icons and GET LISTED; camera icon removed.
14. Page canvas split from section fills — `#F9F9F7` warm canvas (incl. services bar, via new `--canvas` token) with sections staying white; dark-mode canvas tuned to `#0C0F11`.
15. Footer: pinned black → then white-in-light / black-in-dark; inset shadow removed; tagline removed; "All systems operational" removed; dashed side rails end below the closer section (none on the footer); SOC 2 badge matched to the theme-toggle height (35px).

## Hero chat window

16. Header reads "Project / <category>" synced to the cycling story (was "Claude Code"), with the laptop icon; header divider line removed.
17. Panel: white → `#FDFDFD`; drop shadow removed; Basepoint-style outer ring added (stroke → solid grey mat fill, 7px inset).
18. User bubble: border removed, fill lightened to `#F7F7F7`.
19. Project-card thumbnail stroke removed; prompt picker flattened (no shadow → no border; selected item stroke removed in favor of a grey `--surface` fill).
20. Composer send button + AI reply avatar in Claude orange `#D97757`.
21. "See all capabilities" — copy shortened, styled as a chip pill matching the animation chips (exact 22px size match), moved up beside the stream (`50% + 102px`), arrow removed; all capability chips made white.
22. Animation tiles for Zero Man / Recreate Scroll Motion / Sales Leads replaced with new Figma artwork (embedded as PNG data URIs in `GEN_THUMBS`).

## Zero-grid background

23. Diagonal hatch replaced with a randomized slashed-zero pattern (seeded per-cell hash, 3.5–10% opacity); densified (14px cells, 50% fill, smaller glyphs); converted to a live SVG layer with `#1919FF` hover (55% hot glyph + 22% 3×3 neighbors). Fixed twice: hover persistence through re-renders, and a self-measurement bug that froze layers at 300×150.
24. Footer zero band (88px → 168px tall) with the same hover, plus the Zero Man easter egg: hover the band center (±70px zone) and glyphs repel (85px radius) while the pixel-art mascot reveals at 18% opacity; zeros overlapping the mascot fade out. Asset at `assets/zeroman.png`.

## Interactions

25. `#1919FF` hover accents: Try it / Browse all / capability titles (text), Install-Zero primaries (fill, both themes), FAQ icons — scoped in dark mode to the primaries only.
26. Capability/FAQ row hover grey: halved, then halved again (25% `color-mix` toward bg).
27. Try it / Browse all arrows are hover-only (fade + slide); Browse all collapses the arrow slot at rest so the button hugs its label.

## Content (copy from Zero-search-website Figma 6821-2)

28. Full copy pass: "Unblock your AI." + new hero subhead; "Get Zero" everywhere (was "Install Zero"); "Featured services" + "Try one with your AI to see it in action"; "Browse all 9,999 services"; education section "Your AI stops saying “I can't”" + new body + checklist (works with your AI / install once / transactions automatic); closer "Make your AI better with Zero" / "Your first $5 is on us."; search placeholder "search zero services". Footer tagline intentionally left removed despite appearing in the Figma.
29. Service icons pulled from zero.xyz (provider favicons via its `/api/favicon/<domain>` endpoint: StablePhone, Suno, fal.ai, PostalForm, RapidRuntime, Judge0) on white tiles, replacing emoji. Saved under `assets/svc-*.png`.
30. Education video emptied to a bare box (elements hidden in CSS, kept in DOM for the player JS); its drop shadow removed.

## Deploy

31. Published via Zero's "Host Website (ZeroClick)" capability to **site.withzero.ai/s/install-v18-claude** (0.005 USDC via MPP on Tempo, 365-day TTL, expires 2027-06-10). Deploy build inlines all assets as data URIs and downscales Zero Man to 216px (nearest-neighbor) to fit the 1MB limit. Re-publish: `POST /run` with slug `install-v18-claude` ($0.005, refreshes TTL); the owner-only free `PUT` rejected our payment proof, so use the POST route. Re-deployed 2026-06-10 with the video-box shadow removal — the live copy matches local.
