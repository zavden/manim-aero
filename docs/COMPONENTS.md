# Components and interaction contracts

Use this alongside [DESIGN_SYSTEM.md](DESIGN_SYSTEM.md). The current renderer lives in [app.js](../dist/assets/app.js); the class names below are its actual integration points. These are implemented components, not a published component library API.

## 1. Theme and page shell

| Element / hook | Purpose | Contract |
| --- | --- | --- |
| `body[data-theme="neo"]` | Default dark appearance | Matches the scoped CSS and `themes.neo` metadata. |
| `body[data-theme="old"]` | Classic light appearance | Loads the same content with Old material and layout. |
| `.design-toolbar` | Sticky theme/style controls | Outside `.site-shell`; 51 px high. |
| `[data-theme-option]` | Theme buttons | Values `neo` and `old`; mutually exclusive `aria-pressed`. |
| `[data-current-theme]` | Current theme name | Updated by `applyTheme()`. |
| `.site-header` | Brand, top links, mobile menu | Top links navigate to the existing lesson anchors. |
| `.window-caption` | Old's decorative title bar | Hidden in Neo. Its window glyphs are decoration, not functional OS controls. |
| `.lesson-layout` | Theme-dependent content structure | Neo open/block; Old framed/grid. |
| `.lesson-main` | Main reading area | Keep `min-width: 0` so long code does not widen the page. |

The current fixed header labels and breadcrumbs describe one demonstration lesson. They are not a multi-page router or a data-driven course hierarchy.

## 2. Lesson navigation

The class remains `.sidebar` for historical reasons, even though it is horizontal in desktop Neo.

```html
<aside class="sidebar" id="course-menu">
  <nav aria-label="Lesson contents">
    <a class="lesson-link active" href="#introduction" aria-current="location">
      <span class="lesson-no">01</span>
      <span>Your first scene<small>One circle, endless possibilities</small></span>
    </a>
    <!-- More section links with the same structure. -->
  </nav>
</aside>
```

- `.lesson-no` is plain small numbering in Old and a circular badge in Neo.
- The last text span contains the title and optional description. Preserve this structure; selectors use `span:last-child`.
- `.active` controls appearance; `aria-current="location"` communicates the selected section.
- Selection follows clicks and URL hash changes, not passive scroll position.
- The `.menu-button` targets `course-menu` using `aria-controls` and toggles `aria-expanded` plus `.sidebar.mobile-open`. Selecting a lesson link closes the mobile menu.
- Old also displays `.sidebar-title`, `.course-note`, and `.reference-link` on desktop. Neo hides these. Do not remove their DOM because only one theme currently shows them.

## 3. Hero and logo

`hero()` returns `.lesson-hero#introduction`, with `.hero-text` and decorative `.hero-art` children. The action is a real anchor to `#code`.

- `.hero-text h1 em` receives the accent color. It is not italic.
- `.hero-actions` groups `.primary-btn` with `.read-time` metadata and wraps at compact widths.
- `.hero-art` is `aria-hidden="true"`; the headline and description convey the useful information.
- `.hero-circle` and `.hero-square` are CSS shapes. Their Neo outlines use masked `::after` elements.
- `.hero-triangle` is an SVG using `logo-coral-fill` and `logo-coral-edge` definitions. If multiple heroes are ever rendered on one page, those IDs must be made unique.
- Old hides the SVG path and uses the triangle's classic CSS background/clip path.
- The small `.brand-mark` is a separate CSS composition. `assets/manim-reference.png` is the original palette reference, not the rendered hero artwork.

The removed coordinate label, caption, and dashed ellipse are deliberately absent.

## 4. Code panels

```html
<section class="code-panel" aria-labelledby="example-code-title">
  <div class="panel-bar">
    <span id="example-code-title">first_scene.py</span>
    <button class="copy-btn" data-copy="source" aria-label="Copy Python code">Copy</button>
  </div>
  <pre tabindex="0" aria-label="Python code for the first scene"><code><!-- Escaped, highlighted lines --></code></pre>
  <div class="code-status">
    <span>Python · Manim CE</span>
    <button data-download>Download .py</button>
  </div>
</section>
```

Current behavior:

- `source` is one embedded Python string. The visible panel, copy action, and `.py` download share it.
- Each rendered line uses `.code-line`, a hidden-from-accessibility `.line-no`, and a span preserving whitespace.
- `escapeHtml()` runs before highlighting. Preserve literal `<`, `>`, and `&` correctly in future code inputs.
- `highlight()` is a small regex highlighter for this demo, not a full Python parser or multi-language engine. Its recognized Manim classes are explicitly listed.
- Syntax classes: `.kw`, `.fn`, `.str`, `.num`, `.self`.
- `pre` scrolls horizontally; code is not arbitrarily wrapped or truncated to fit a card.
- `[data-copy="source"]` copies the source string, not the generated line-number markup.
- `[data-copy="command"]` copies `manim -pql first_scene.py FirstScene`.
- `[data-download]` exports a Blob as `first_scene.py`.

**Current limitation:** copy and download handlers refer to one global example. Repeating this markup alone will not give multiple notebook cells independent copy/download behavior. The converter stage must introduce per-example data and scoped handlers first.

## 5. Preview and playback

The preview panel consists of `.panel-bar`, `.preview-stage`, `.player-controls`, and `.demo-caption`.

| Hook | Responsibility |
| --- | --- |
| `.morph-shape` | Browser-only circle-to-square illustration |
| `[data-play]` / `.play-button` | Start/pause; accessible name changes to Play/Pause animation |
| `#timeline` | Range from 0 to 1000; input pauses playback and updates position |
| `--progress` | CSS percentage used for the filled portion of the timeline |
| `#time-label` | Elapsed / total seconds |
| `[data-reset]` | Pause and return to position zero |
| `#stage-operation` | Caption naming the simulated source operation |

Playback uses `requestAnimationFrame` over 4.5 seconds. It visually approximates Create, a short wait, Transform, and a final wait. It does not run Python or reproduce Manim output pixel-for-pixel. Preserve the visible “Browser simulation · does not run Python” caption unless the implementation actually changes.

Theme switching does not rebuild these nodes, so playback and slider state survive. There is no video player backend yet. These IDs are currently singleton IDs and must be scoped or made unique before supporting multiple previews.

## 6. Terminal, tips, and concept cards

- `.terminal`: a compact command with a subdued label, literal `$` prompt, scrollable `code`, and copy action.
- `.tip`: an informational callout with a small circled `i`, a bold lead sentence, and ordinary prose. Keep it less prominent than the code/preview pair.
- `.concept-grid`: three educational articles, with `.concept-symbol`, `.concept-number`, heading, API label, and explanatory prose.
- Neo's three accent variants currently come from `nth-child(2)` and `nth-child(3)`. A general renderer should use explicit semantic variants if the order or number of cards changes.
- Cards receive a subtle hover response, but the current articles are not buttons or links. Do not add interactive semantics unless an actual action exists.

## 7. Practice and completion

- `.practice#practice` contains an exercise, native `<details>` / `<summary>` hint, and completion button.
- The hint can contain prose and an independent code fragment.
- `[data-complete]` toggles `.completed`, `aria-pressed`, and its English label.
- Completion is stored as the string `"true"` or `"false"` under `manim-first-lesson`.
- Completion belongs to the lesson, not the theme. Theme switching must not mark a lesson complete, incomplete, or reset its hint disclosure.

The storage key is intentionally described as a prototype limitation: multiple converted lessons will need separate stable IDs and per-lesson state.

## 8. Style guide, downloads, and messages

- `[data-open-guide]` calls `dialog.showModal()`; `[data-close-guide]` closes the dialog.
- The dialog records its opener and returns focus on close. Native Escape closing is available; clicking outside its bounds also closes it.
- `guideContent()` renders the current theme description, palette, type/material summary, and CSS-variable download button.
- Palette buttons use `[data-color]`; `[data-tokens]` calls `downloadThemeTokens()`.
- Their click handlers are delegated to the dialog because theme switching replaces `.dialog-content`.
- Export names are `manim-neo-tokens.css` and `manim-old-tokens.css`. These are summarized reference tokens, not a complete standalone stylesheet.
- `.toast` has `role="status"` and `aria-live="polite"`; copy/completion messages disappear after approximately 2.6 seconds.
- Clipboard support uses the modern API when available in a secure context, then a legacy selection-based fallback. Failure displays an English message rather than claiming success.

## 9. State and accessibility review

Preserve these existing affordances:

- Real links for navigation and real buttons for actions.
- Named icon controls, `aria-pressed` for theme/completion state, and `aria-expanded` for the mobile menu.
- Visible focus outlines and a keyboard-focusable code region.
- Decorative SVGs excluded from the accessibility tree.
- Dialog focus return and native disclosure behavior.
- Reduced-motion styling for CSS effects.

Do not claim an accessibility audit has passed. Small labels, the existing skip-link contrast, dialog naming, touch target sizes, and browser-specific masked borders deserve focused review if accessibility becomes the next task. Such a review should be separate from incidental documentation work.

## 10. Extending for notebooks

These are candidate mappings to evaluate after inspecting the supplied notebook; they are not implemented conventions:

| Notebook material | Existing visual building block | Work still needed |
| --- | --- | --- |
| Title / introductory Markdown | Hero and section headings | Decide title and section metadata conventions |
| Explanatory Markdown | Reading sections, tips, cards | Define supported Markdown and math rendering |
| Source code cell | Code panel | Per-cell identity, highlighter, copy/download binding |
| Text/traceback output | Output panel with code typography | Explicit output/error variants; currently absent |
| Image/plot | Preview surface | Asset extraction, sizing, captions, accessible alternatives |
| Manim video output | Preview/player surface | Real media renderer and asset handling |
| Exercise | Practice section | Only when explicit exercise metadata/content exists |

Preserve cell order and distinguish source from output. Do not turn arbitrary notebook cells into invented exercises or browser simulations. The goal is to apply the approved visual framework to the user's actual notebook, not to substitute generic lesson content.
