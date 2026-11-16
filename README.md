# Manim · Neo

A Manim tutorial website with two themes: **Neo**, a dark Aero design with soft glow and subtle neon accents, and **Old**, the light Prisma design with lavender glass and classic Aero reflections.

The first visit uses Neo regardless of the system theme. The selector remembers the choice in the browser. Switching themes preserves the lesson, completion status, and animation state. Previous dark-theme preferences remain compatible.

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
