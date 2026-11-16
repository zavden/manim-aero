# Manim · Neo

A Manim tutorial website with two themes: **Neo**, a dark Aero design with soft glow and subtle neon accents, and **Old**, the light Prisma design with lavender glass and classic Aero reflections.

The first visit uses Neo regardless of the system theme. The selector remembers the choice in the browser. Switching themes preserves the lesson, completion status, and animation state. Previous dark-theme preferences remain compatible.

## Documentation

Another agent can continue without access to the original conversation. Start with [AGENTS.md](AGENTS.md) and follow the documentation map:

| Reference | Contents |
| --- | --- |
| [Continuity and decisions](docs/HANDOFF.md) | Approved direction, rejected variants, checkpoints, and the pending notebook task |
| [Design system](docs/DESIGN_SYSTEM.md) | Neo/Old palettes, typography, layout, responsive rules, effects, and design constraints |
| [Components](docs/COMPONENTS.md) | DOM/classes, states, events, accessibility hooks, and extension limits |
| [Development](docs/DEVELOPMENT.md) | Runtime flow, persistence, validation, deployment context, and notebook integration planning |
| [Reference tokens](docs/design-tokens.json) | Machine-readable values, source hashes, and known metadata differences |
| [Material recipes](docs/material-recipes.css) | Exact CSS material excerpts for both themes; documentation only |

The source CSS remains authoritative. The in-app style guide and its downloads are a summary, not a complete theme engine.

## Open

Open `dist/index.html` directly in a browser. No installation or build step is required. `dist/orbita.html` preserves the original link and redirects to the main page. Legacy Spanish section links still work.

To serve the site over local HTTP:

```sh
python3 -m http.server 5173 --directory dist
```

## Files

- `dist/index.html`: page entry point.
- `dist/assets/app.js`: lesson content, style guide, downloads, and interactive controls.
- `dist/assets/styles.css`: shared styles, Neo theme, and responsive layout.
- `dist/assets/old.css`: the original Prisma styles restored for the Old theme.
- `.openai/hosting.json`: Sites identity; its presence does not mean the site is published.

The animation preview is a browser simulation. Download `first_scene.py` to run the Python example separately with Manim Community:

```sh
manim -pql first_scene.py FirstScene
```

## Validation

```sh
node --check dist/assets/app.js
```

Check visual changes, navigation, theme switching, the style guide, and playback controls in the browser.

## Next stage

The next planned step is converting Jupyter notebooks into tutorial pages using Neo and Old. See [the continuity note](docs/HANDOFF.md) for the approved design, project structure, checkpoints, and pending decisions. The first notebook has not been supplied yet.
