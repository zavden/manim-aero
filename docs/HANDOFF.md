# Continuity note: Neo / Old and the upcoming Jupyter workflow

## Current request and stopping point

The user approved the current website, requested commits before compacting, and then asked for complete design documentation so another agent can continue independently. The design system, component contracts, implementation guide, token reference, and material recipes are now documented. After compaction, the user will provide a Jupyter notebook. Their next objective is to use this website as a framework that converts code and content authored in Jupyter into tutorial pages using the established styles.

**No notebook has been provided yet. No notebook converter, notebook schema, or execution pipeline has been implemented or agreed upon.** Resume by inspecting the supplied notebook and its outputs before choosing the conversion approach. Do not start inventing a sample notebook or replacing the approved design while waiting.

## Documentation map

- [AGENTS.md](../AGENTS.md): agent entry point and repository conventions.
- [DESIGN_SYSTEM.md](DESIGN_SYSTEM.md): full visual specification and effective responsive values.
- [COMPONENTS.md](COMPONENTS.md): markup, event/data hooks, state, and current limitations.
- [DEVELOPMENT.md](DEVELOPMENT.md): runtime flow, verification, and notebook integration boundaries.
- [design-tokens.json](design-tokens.json): reference values with source hashes.
- [material-recipes.css](material-recipes.css): exact material declarations for Neo and Old, not loaded by the site.

These documents distinguish approved behavior from implementation gaps and future proposals. In particular, the in-app Neo surface token differs from the runtime CSS; follow the documented source-of-truth notes rather than assuming the exported token file fully reproduces the theme.

## Active project

- Repository: `/Users/alex/Manim/CURSO/guia_estilos/manim-aero`, branch `main`.
- Workspace parent: `/Users/alex/Manim/CURSO/guia_estilos`. It is not itself the Git repository.
- Entry point: `dist/index.html`. The site works directly from a local file, with no build step or dependencies.
- `dist/orbita.html` is a legacy redirect to the entry point; retain compatibility.
- The sibling directory `../dist/` contains the original four proposals extracted by the user. It is reference material, **not the active website**. `../propuestas-originales.zip` is their archive.

## Approved appearance

- **Neo** is the default dark theme, formerly called Órbita / Oscuro. The first visit uses it regardless of the OS theme.
- **Old** is the light theme restored from the original Prisma proposal. Preserve its classic Aero material: lavender glass, window frame, split reflections, glossy controls, and vertical lesson navigation.
- Neo uses an open layout without an outer window frame around `lesson-layout`. Its lesson navigation remains horizontal on desktop.
- Neo code panels and cards use smooth glow gradients, selective neon contours, and soft shadows. The short bright edge accents fade in and out; they must not begin abruptly.
- Neo navigation uses understated indigo glass, circular numbers, short descriptions, and sage light on the active item. Previous bright silver / directional metallic navigation was rejected and is preserved in history.
- Neo primary actions and Play use dark sage glass with pale text and subtle borders. The former bright pastel green buttons were rejected. Old retains its classic controls.
- The main logo contains three geometric shapes and a faint grid. Its coordinate text, caption, and dashed ellipse were removed. Neo shapes use smooth inner gradients, graduated mint/lilac/coral contours, and soft outer glow reduced to roughly half of the stronger experiment. Preserve sharp shape edges.
- The palette was made moderately more vivid, especially inside the logo. Dark background: `#191E29`; accent: `#B5AFF2`; sage: `#8BBDA3`; coral: `#DF866B`; neon mint: `#95EBD0`; neon lilac: `#BDAAFF`.
- The entire active website, accessibility labels, style guides, example code identifiers, downloads, and README are now **English**. The conversation with the user has remained Spanish.

## Implementation map

- `dist/assets/app.js` contains theme metadata, lesson markup, an embedded Python example, SVG icons/logo, code highlighting, the style guide, downloads, completion state, and a browser animation demo.
- `dist/assets/styles.css` contains shared styling and Neo rules, scoped with `[data-theme=neo]`.
- `dist/assets/old.css` contains the original Prisma styles scoped with `[data-theme=old]`, plus shared-UI compatibility and responsive rules.
- `dist/index.html` bootstraps the persisted theme before the main script runs. Theme switching changes the body attribute and style-guide content without rebuilding the lesson or resetting animation state.
- Appearance preference: `localStorage['manim-appearance']`, values `neo` / `old`. The former dark value `orbita` falls back to Neo.
- Completion preference: `localStorage['manim-first-lesson']`. This is currently a single demo lesson, not yet a general course data model.
- Current anchors: `#introduction`, `#code`, `#concepts`, `#practice`. The script maps the previous Spanish anchors to these English equivalents.
- Python download: `first_scene.py`, class `FirstScene`; command: `manim -pql first_scene.py FirstScene`.
- The animated circle-to-square preview is **a browser simulation**, not Python execution or a Manim render. The page explicitly identifies it as such.

## Next stage: notebook-to-website framework

Confirmed user intent: author code in Jupyter and convert the notebook into pages using Neo and Old.

The notebook itself should determine the first implementation. Inspect its Markdown cells, code cells, cell order and IDs, and saved outputs such as images, HTML, equations, or video references. Establish which outputs are already present and which, if any, need rendering. Then separate lesson data from the current hardcoded page so the same components and theme switch can render the supplied content.

Still undecided: notebook conventions, whether conversion should execute cells, output asset handling, navigation across multiple lessons, and the converter interface. The user has not requested a particular framework or build system. Keep the initial implementation proportional to the supplied notebook and preserve the approved visuals.

## Verification and environment

- `node --check dist/assets/app.js` and `git diff --check` pass.
- The embedded translated Python example was parsed with Python `ast.parse`; its class, filename, and terminal command agree. Manim rendering has not been run.
- Both themes, theme persistence after reload, translated content, and the style guide were visually checked in Chrome. Do not claim comprehensive mobile or cross-browser testing.
- A local HTTP server was previously blocked by sandbox socket permissions. Direct `file://` viewing in Chrome worked.
- Sites identity is retained in `.openai/hosting.json`: `appgprj_6afb5055075c8191bc890c30b9aa10a4`. Publication was blocked because `git.chatgpt-team.site` could not be resolved. No deployment URL has been verified. Reuse this identity if publication is resumed; do not register another Site.

## Useful checkpoints

- `42a2680`: initial standalone Órbita style guide.
- `a232ed2`: soft glow, selective neon, and logo before the more vivid palette.
- `1251e1f`: vivid palette and the previous dark metallic navigation.
- `6e2543e`: approved understated lesson navigation.
- `90156c8`: default dark theme plus classic light Old theme.
- `dcb537e`: dark glass primary and playback controls.
- `d47806a`: English translation and dark-theme rename to Neo.
- `082b3e1`: initial continuity note for the Jupyter stage.

The latest documentation commit contains this handoff. Start the next stage from the current branch, not an earlier visual experiment.
