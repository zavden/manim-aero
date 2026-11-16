# Development and continuation guide

## Start from the active project

The active Git repository is `manim-aero/`, not its workspace parent and not the extracted `../dist/` proposal directory. Open `dist/index.html`; `dist/orbita.html` is only a compatibility redirect. There is no package manifest or build process. The tracked `dist/` files are the editable source.

Read [HANDOFF.md](HANDOFF.md) for user decisions, [DESIGN_SYSTEM.md](DESIGN_SYSTEM.md) for appearance, and [COMPONENTS.md](COMPONENTS.md) for markup/behavior contracts. Root [AGENTS.md](../AGENTS.md) provides the entry point for another agent.

## Runtime flow

```text
index.html
  loads styles.css, then old.css
  sets body[data-theme] from the saved preference before app rendering
  runs deferred app.js
    lesson() renders the shared lesson and dialog
    setupInteractions() binds controls and initializes state
    applyTheme(theme, false) synchronizes metadata and theme-specific UI
```

`applyTheme()` changes the body attribute, browser color scheme, theme-color metadata, document title, selector state, and style-guide content. It does not call `lesson()` again. Preserve this separation when extracting a reusable renderer: rebuilding the whole page on theme changes would lose slider, playback, focus, and disclosure state.

### Persistence and compatibility

| Storage / URL | Current behavior |
| --- | --- |
| `manim-appearance` | `neo` or `old`; only `old` explicitly changes the bootstrap default |
| Legacy saved `orbita` | Resolves to Neo through the default path |
| Missing/unavailable localStorage | Site still renders and uses Neo; persistence may be unavailable |
| `manim-first-lesson` | String boolean for the single demo lesson |
| `#introduccion` | Mapped to `#introduction` |
| `#codigo` | Mapped to `#code` |
| `#conceptos` | Mapped to `#concepts` |
| `#practica` | Mapped to `#practice` |

Legacy fragments are normalized when possible using `history.replaceState`, scrolled into view, and reflected in navigation selection. The app is still a single-page demonstration, not a route-based course application.

## Editing appearance without regressions

1. Locate the final applicable rule, including media queries and `old.css` overrides.
2. Determine whether the change belongs to a shared component or one theme. Use explicit theme scoping for the latter.
3. Reuse existing markup and tokens/recipes where they actually exist. Many values remain literal; do not assume the in-app download is a runtime configuration engine.
4. Check the requested component in context, not only as a cropped card. Keep the hierarchy between content, action, and decoration.
5. Update the corresponding documentation if the user accepts a new design decision.

Do not refactor the entire stylesheet just to make a small edit. Consolidation is possible later, but its visual equivalence needs verification because the file contains earlier declarations intentionally overridden by later ones.

## Verification

For JavaScript changes:

```sh
node --check dist/assets/app.js
```

For pending changes before a commit:

```sh
git diff --check
git status --short
```

For a local HTTP preview, when the environment permits binding a port:

```sh
python3 -m http.server 5173 --directory dist
```

Direct local-file viewing is supported. In the previous environment, opening a local server was denied by the sandbox, while viewing the file in Chrome worked. Recheck current tool availability rather than assuming that limitation is permanent.

### Focused visual review matrix

These are recommended checks for future changes, not a record that all have already passed:

| Scenario | Inspect |
| --- | --- |
| Both themes, 1440 px | Header, hero, lesson navigation, editor/preview alignment, cards |
| Both themes, 900 px | Column changes, decorative artwork, text wrapping |
| Both themes, 650 / 390 / 320 px | Theme selector, mobile menu, stacked panels, code overflow, dialog |
| New browser preference | Neo loads without consulting OS theme |
| Select Old, then reload | Old remains selected; classic appearance remains intact |
| Change theme while demo is paused/playing | Source, controls, disclosure, completion, and animation state remain usable |
| Keyboard | Named controls, visible focus, menu state, code scroll, dialog close/focus return |
| Reduced motion | No incidental CSS transitions or smooth scrolling; demo remains explicit/user initiated |
| Long code / long heading | Content is not clipped and does not widen the whole viewport |
| Copy/download | Raw code, correct filename/class/command, and the selected theme's variables |

Only desktop Chrome visual inspection, theme switching/persistence, and focused syntax/source checks were performed during the design iterations. Python parsing verifies syntax, not a successful Manim render. No mobile device or cross-browser certification is implied.

## Known gaps relevant to the next stage

- Lesson content is hardcoded inside template strings. There is no lesson schema or notebook importer.
- Code/download/preview selectors currently assume one example per page.
- Syntax highlighting is a small demo-specific regex, not a robust language service.
- Markdown, LaTeX, notebook MIME bundles, images, attachments, tables, tracebacks, and actual videos have no general output-rendering pipeline yet.
- Theme metadata and summary exports are not perfectly identical to computed CSS. See the discrepancy note in DESIGN_SYSTEM.
- The class `.sidebar` is horizontal in Neo and vertical in Old; a component refactor must preserve that distinction.
- Mobile styling exists but has not been comprehensively visually audited.
- Several decorative captions use very small text; no WCAG compliance claim is made.

## Jupyter implementation boundary

The confirmed next objective is **Jupyter-authored content → tutorial pages in Neo / Old**. The first notebook is still pending. Do not treat this document as an agreement to execute notebooks, add a web editor, use a particular framework, or build a hosted backend.

After receiving the notebook:

1. Inspect Markdown, code, saved outputs, cell IDs/tags, attachments, and file references.
2. Identify which content belongs to the page and what visual outputs already exist.
3. Propose the smallest content model that fits that evidence and the approved components.
4. Separate source content from the current rendering functions, introducing stable per-cell IDs and independent event bindings where required.
5. Preserve source/output order, text escaping, and the distinction between a real rendered result and the current browser simulation.
6. Validate the converted example in both themes before generalizing.

Notebook execution, missing render generation, multi-lesson routing, and metadata conventions remain open decisions. Ask only for information that cannot be inferred from the supplied file or the user's instructions.

## Git, deployment, and reference files

Meaningful checkpoints are listed in HANDOFF. Follow explicit user requests about when to commit a baseline and when to leave a new visual experiment uncommitted. Do not overwrite historical checkpoints to conceal rejected variants.

The reference JSON and recipe CSS in `docs/` are not loaded by the site. Their source hashes allow another agent to notice when code has changed since the snapshot. Update them together with accepted design changes; an outdated snapshot is a prompt to inspect the source, not a reason to revert working code.

`.openai/hosting.json` retains the existing Sites project ID. The earlier publication attempt failed to resolve the source host; no deployed URL was verified. Publication and source synchronization are separate from local Git commits. If publishing is requested later, reuse the recorded Site identity and the supported workflow.
