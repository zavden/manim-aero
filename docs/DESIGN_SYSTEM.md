# Neo / Old design system

This is the implementation reference for the approved design. It documents the runtime CSS at code checkpoint `d47806a`; it does not propose a redesign. See [COMPONENTS.md](COMPONENTS.md) for DOM and interaction contracts and [design-tokens.json](design-tokens.json) for a machine-readable reference.

## 1. The two themes

| | Neo | Old |
| --- | --- | --- |
| Role | Default dark theme | Optional light theme |
| Visual direction | Contemporary Aero with diffuse light | Classic Vista / Windows 7 Aero |
| Main canvas | Open; no full-page window border | Framed window with lavender chrome |
| Desktop lesson navigation | Horizontal segmented strip | Vertical left sidebar |
| Surface lighting | Continuous glow gradients | Deliberate reflection bands and bevels |
| Accent treatment | Selective neon at illuminated edges | Pale lavender gloss and white highlights |
| Primary action | Dark sage glass, pale text | Glossy light lavender button |
| Play control | Dark rounded square, 30 × 30 px | Glossy circle, 25 × 25 px |

Neo is selected on a first visit, even on an OS using light mode. A saved choice overrides that default. The theme keys are `neo` and `old`; use these exact values in `body[data-theme]` and the controls.

The design is calm and educational. Code and learning content remain the primary focus. Glow accents should establish depth and hierarchy, not make every surface equally luminous.

## 2. Sources and cascade

Styles load in this order:

1. [styles.css](../dist/assets/styles.css): common components, early theme rules, responsive rules, then the final Neo materials and component refinements.
2. [old.css](../dist/assets/old.css): the restored Prisma theme and its compatibility/responsive rules.

Theme-specific selectors have higher specificity than unscoped component selectors. A later generic media rule does not necessarily override an earlier theme rule. For example, Neo keeps a 65 px desktop hero heading even though the generic large-screen rule specifies 55 px.

Search for **every occurrence** of a selector before editing it. Useful final block markers in `styles.css` are:

- `Neo: open canvas, glass panels with smooth light and selective neon edges.`
- `Lesson navigation: quiet indigo glass, with sage light marking the current step.`
- `Action controls share the dark glass and restrained sage light of the cards.`
- `Logo: the same smooth glass light, with a continuous graduated neon outline.`

The [material recipes](material-recipes.css) copy the final relevant declarations from these sources. They are documentation, not an additional stylesheet to load.

## 3. Color system

### Runtime foundation

All hex values below are CSS colors. Eight-digit values use `#RRGGBBAA`, where the final two digits are alpha, not a second color channel.

| Role / CSS variable | Neo | Old |
| --- | --- | --- |
| `--bg` | `#191E29` | `#CDCDDF` |
| `--surface` | **`#202637`** | `#F4F3FA` |
| `--text` | `#EAE8E3` | `#30334E` |
| `--muted` | `#A1A9BE` | `#818099` |
| `--accent` | `#B5AFF2` | `#55598E` |
| `--border` | `#3D455B` | `#D2D0E3` |
| `--panel` | `#232B3D` | `#EFEDF7` |
| `--code` | `#151B28` | `#25283B` |
| `--inline` | `#FFFFFF0C` | `#8984B312` |
| Sage brand accent | `#8BBDA3` | `#8BAF9B` |
| Coral brand accent | `#DF866B` | `#CD7F66` |

The page backgrounds are layered gradients. Old's final background layer is `#BFC1DA`, even though its nominal `--bg` and theme metadata are `#CDCDDF`. Neo's main reading canvas is transparent over the body background; `--surface` is used by components such as the style dialog, not as an opaque fill over the entire lesson.

### Neo accents and hierarchy

| Use | Color |
| --- | --- |
| Mint neon / code strings | `#95EBD0` |
| Lilac neon / code keywords | `#BDAAFF` |
| Lilac glass rim | `#CBBAFF` |
| Mint glass rim | `#A6F6D5` |
| Coral glass rim | `#F5AE96` |
| Hero heading emphasis | `#9BD8B7` |
| Primary button / Play text | `#C5E6D4` |
| Primary button icon | `#A4D9BC` |
| Navigation inactive title | `#ABB2C4` |
| Navigation inactive description | `#7F899F` |
| Navigation active title | `#D4E8DB` |
| Navigation active description | `#96AFA6` |

Use sage to communicate the current lesson and primary actions. Lilac belongs to the indigo environment, code accents, and the first concept card. Coral is a supporting accent for motion/practice and the third card. Do not assign unrelated status meanings to these colors without defining those states.

### Code syntax

| Token class | Neo | Old |
| --- | --- | --- |
| Plain code | `#DCE5DC` | `#DCE5DC` |
| `.kw` | `#BDAAFF` | `#B6B3E7` |
| `.str` | `#95EBD0` | `#A1C7AC` |
| `.fn` | `#D8BE8B` | `#D8BE8B` |
| `.num` | `#E2A68D` | `#E2A68D` |
| `.self` | `#A8BDCA` | `#A8BDCA` |
| `.line-no` | `#798C8F` | `#798C8F` |

Both themes use dark code panels. Do not convert Old's editor to a light code block as an incidental theme cleanup.

### Metadata caveat

`themes.neo.surface` in `app.js` and its CSS-variable download currently say `#262D40`; the actual runtime CSS says **`#202637`**. The in-app type-scale display is also a summary, not the full responsive scale. This documentation records both rather than silently changing the approved UI. Use the runtime CSS for reproduction; reconcile metadata explicitly in a future maintenance change.

The exported `--sage`, `--coral`, font, and type-size variables are convenient reference values. Most existing component declarations still contain literals, so changing only the downloaded tokens will not restyle the application.

## 4. Typography and iconography

- UI and body: `"Segoe UI", -apple-system, BlinkMacSystemFont, Arial, sans-serif`.
- Wordmark, small editorial tagline, and style-guide display title: `Georgia, serif`.
- Code: `"SFMono-Regular", Consolas, "Liberation Mono", monospace`.
- No web fonts are downloaded. Font rendering depends on the host OS.
- Body baseline: 15 px, line-height 1.6. Many components intentionally use smaller explicit sizes.
- Headings: line-height 1.2 generally; hero line-height 1.08. Hero emphasis is colored, not italic.
- Small uppercase eyebrows use expanded tracking, usually 1–1.7 px.
- Icons are inline 24-unit SVGs with 1.6-unit strokes, rounded caps and joins. Decorative icons are `aria-hidden`; the control supplies the accessible name.

### Effective size examples

These describe the current cascade, not a new type scale. Widths are CSS viewport pixels.

| Component | Neo | Old |
| --- | --- | --- |
| Hero, width > 1150 | 65 px | 48 px; 55 px at width ≥ 1500 |
| Hero, 901–1150 | 57 px | 42 px |
| Hero, 651–900 | 50 px | 46 px |
| Hero, width ≤ 650 | 49 px | 41 px |
| Section heading, desktop | 26 px / weight 450 | 22 px / weight 550; 26 px at ≥ 1500 |
| Section heading, ≤ 650 | 23 px | 21 px |
| Concept title, desktop | 18 px / weight 450 | 14 px / weight 550; 17 px at ≥ 1500 |
| Code, > 1150 | 12 px | 10 px; 12 px at ≥ 1500 |
| Code, 901–1150 | 12 px | 9 px |
| Code, 651–900 | 10 px | 12 px |
| Code, ≤ 650 | 10 px | 10 px |
| Navigation title, > 900 | 11 px | 11 px, reducing to 10 px at ≤ 1150 |
| Neo navigation description | 8 px above 900; hidden below | Old uses its own sidebar rules |

The non-monotonic Old code sizes and small caption sizes are current prototype behavior, not a claim of accessibility certification. Treat a readability revision as a deliberate design change.

## 5. Layout and spacing

All measurements use `box-sizing: border-box`.

```text
design-toolbar (theme selector + style guide)
site-shell
  site-header (wordmark + Learn / Lab / Practice)
  window-caption                 [Old only]
  lesson-layout
    sidebar                      [Neo horizontal / Old vertical]
    lesson-main
      breadcrumbs
      lesson-hero                [text + geometric logo]
      work-section
        lab-grid                 [code panel + preview]
        terminal
        tip
      concepts / concept-grid
      practice
      lesson-footer
style-dialog + toast             [outside the lesson layout]
```

### Main geometry

| Property | Neo | Old |
| --- | --- | --- |
| Shell maximum width | 1270 px | 1430 px |
| Shell side padding, wide | 44 px | 37 px |
| Shell side padding, 901–1150 | 44 px | 22 px |
| Shell side padding, 651–900 | 28 px | 22 px |
| Shell side padding, ≤ 650 | 20 px | 12 px |
| Site header, desktop | 108 px | 103 px |
| Site header, ≤ 650 | 87 px | 88 px |
| Lesson canvas | Block layout, no frame/padding | Grid: 205 px sidebar + flexible content |
| Old sidebar at ≤ 1150 / ≤ 900 | — | 180 px / 160 px |
| Outer window border | None | 7 px, top 4 px; mobile 5 px, top 3 px |
| Window caption | Hidden | 38 px; mobile 34 px |
| Hero padding, desktop | 35 px 0 45 px | Normally 38 px 36 px 36 px; generic responsive rules also apply |
| Neo hero gap | 80 px; 0 at ≤ 900 | — |
| Lab columns, > 900 | 1.2fr / 1fr | 1.35fr / 1fr; 1.25fr / 1fr at ≤ 1150 |
| Lab columns, 651–900 | 1fr / 1fr | One column |
| Lab columns, ≤ 650 | One column | One column |
| Lab gap | 26 px; 22 px at ≤ 900; 23 px at ≤ 650 | 16 px; 12 px at ≤ 1150; 16 px at ≤ 650 |
| Concept grid | Three columns, 23 px gap; one column / 17 px gap at ≤ 650 | Three columns / 16 px gap, 10 px gap at ≤ 900; one column at ≤ 650 |

The spacing vocabulary clusters around 4–8 px for details, 10–18 px for controls, 22–28 px for panels, and 30–45 px between sections. These are observed patterns, not a fully implemented spacing-token system. Preserve the actual selectors when matching existing components.

### Responsive behavior

- At 1150 px: content becomes more compact; some terminal and breadcrumb metadata are hidden.
- At 900 px: header tagline disappears. Old hides the decorative hero artwork; Neo retains a scaled version. Neo navigation descriptions disappear.
- At 650 px: primary header links disappear and the Lesson menu button appears. Lesson navigation is collapsed by default; `.mobile-open` reveals a two-column menu. Both themes stack code and preview, and stack the concept cards. Hero artwork is hidden.
- At 650 px: style-dialog swatches change from six to three columns; the toolbar's style-guide text is hidden while its accessible icon button remains.
- At 380 px: the toolbar's secondary “Style guide” label is hidden to keep the theme switch usable.
- The top toolbar is 51 px high and sticky at `top: 0`, `z-index: 20`. Scroll padding is 78 px normally and 76 px at ≤ 650 after the final override.

## 6. Material recipes and depth

### Neo glass panels

The material has three parts: an opaque or translucent dark surface, colored diffuse light, and a selective rim. The surface must remain readable. Panel radii are 12 px; the usual border is 1 px.

Code panels use an opaque `#151B28` interior. Their rim transitions from pale lilac at the top-left, through nearly transparent dark segments, to mint at the lower-right. Preview panels reverse the accent order. A very faint 3 px outer ring and broad low-opacity shadows create depth.

Concept cards share the same recipe with per-card light values:

| Card | `--card-light` RGB | `--card-base` | Rim |
| --- | --- | --- | --- |
| Scene | `174, 140, 250` | `#252C40` | `#CBBAFF` |
| Objects | `121, 231, 180` | `#26363B` | `#A6F6D5` |
| Motion | `238, 151, 124` | `#322E3E` | `#F5AE96` |

The main card glow is a radial gradient at `5% -12%`, alpha `.28`, fading to transparent at 64%. The rim uses a `border-box` gradient while the interior layers use `padding-box`. Retain the layer order.

### Smooth edge highlights

The small highlight above a card or panel is **not a solid line starting at full brightness**. Its alpha ramps from transparent at 0%, through a faint stop at 7%, to its peak at 22%, then falls through 50% to transparent at 100%.

Use a `drop-shadow` on the alpha-shaped highlight. A rectangular `box-shadow` can reveal the bounding box even where the line is transparent. This was the cause of a specifically rejected abrupt edge.

For true translucent surfaces, an alternative already used by the active navigation item and logo is a separate masked rim: two mask layers with `mask-composite: exclude` and the WebKit `xor` equivalent. This avoids painting a border gradient across the entire translucent interior. Decorative pseudo-elements must have `pointer-events: none`.

### Neo navigation

The final sidebar is a quiet 12 px-radius indigo glass strip with an 18 px backdrop blur. Its inactive items have no independent panel fill. The active item gets sage light, an 8 px radius, a thin graduated rim, and a small fading bottom accent. Circular numbers are 27 px on wide screens, 23 px at ≤ 900, and 22 px at ≤ 650.

Item minimum heights are 60 / 50 / 48 px across those same ranges. The navigation container uses 6 px padding and 5 px gaps. The earlier reflective metal frame and right-directed silver highlight are historical experiments, not the current design.

### Neo actions

Primary actions and Play share a dark gradient from `#2B4142` to `#25323E`, an upper-left sage radial light, and a muted sage/lilac rim. Text is `#C5E6D4`; hover lightens it to `#D8F3E3`. The hover base changes to `#304B47` → `#293847`. Never restore a solid pastel green fill in Neo.

Play is 30 × 30 px with an 8 px radius. The primary action uses the same radius. Its generic padding is 11 × 17 px, with compact-width adjustments. Hover, pressed, and keyboard-focus states are distinct; focus uses a 2 px sage outline with a 3 px offset.

### Old classic Aero

Old intentionally retains reflection changes around 49–50% in buttons and tabs. Those transitions are part of its approved nostalgic style, even though Neo avoids them. Its main action gradient is:

```css
linear-gradient(#f5f0ff, #dad9f2 49%, #bdbfe0 50%, #e4dff4)
```

The outer frame uses `#ECEAFA70`, a thin `#8589B599` outline, a white inset highlight, and a lavender drop shadow. Panels receive a 3 px pale outer ring rather than Neo's selective neon. Preserve the light reading surface and dark code editor.

## 7. Logo and decorative geometry

There are two marks: the small flat header wordmark icon and the large hero composition. Do not apply the hero's effects to the entire wordmark indiscriminately.

The hero has a circle, a square rotated 10°, a triangle rotated −9°, and a faint 26 px grid. No coordinate text, caption, or dashed ellipse should be restored.

Neo desktop sizes: circle 144 × 144 px; square 133 × 133 px; triangle SVG 110 × 100 px. The artwork container is 32% of the hero and 290 px high before responsive transforms. Old uses its original shared geometry, with classic split reflections.

Neo circle and square use radial fill gradients and a masked 1 px graduated outline. The triangle uses inline SVG gradients, not a clipped bitmap. Its path is `M55 2 L108 98 L2 98 Z`, stroke width 1.2, with round joins. Old hides that SVG path and uses the classic CSS clipped triangle background.

Current soft glow values are recorded exactly in [material-recipes.css](material-recipes.css). The circle uses 14 / 34 / 64 px outer blur radii with mint alpha `1A / 15 / 0C`; the square uses the same values in lilac. Their internal blur is 28 px. The triangle uses 9 / 24 / 42 px drop shadows with coral alpha `33 / 1F / 0D`. These are the reduced-glow values. Blur the light, not the shape or text.

## 8. Motion, focus, and scope

- Most interaction transitions are 0.2 seconds; the toast transitions opacity and vertical position.
- A primary action can lift 1 px on hover and return on press. Avoid ambient pulsing, automatic shimmer sweeps, or motion that competes with the lesson.
- The reduced-motion media rule removes CSS transitions/animations and smooth scrolling. It does **not** currently disable the user-initiated JavaScript demo.
- General focus uses a 3 px coral outline / 5 px offset. Neo lesson links use mint 2 px / 2 px offset; Neo action controls use sage 2 px / 3 px offset.
- Focus visibility and readable text take priority over a decorative glow. Existing tiny captions and contrast have not received a comprehensive accessibility audit.
- Changes to Neo must remain scoped. Old is a separate approved style, not a disabled or obsolete theme.

## 9. Maintain the reference

When a design decision changes, update the corresponding values here, [design-tokens.json](design-tokens.json), the recipe snapshot if relevant, and the in-app theme description/export when appropriate. Check both themes in a browser. Do not treat an old commit, extracted proposal, or summarized token table as stronger evidence than the current approved implementation.
