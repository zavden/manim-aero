# Working on Manim Neo / Old

This directory is the active Git repository. The sibling `../dist/` is the original proposal archive, not the website to edit.

## Start here

1. Read [README.md](README.md) for entry points and commands.
2. Read [docs/HANDOFF.md](docs/HANDOFF.md) for approved decisions and the current stopping point.
3. Read [docs/DESIGN_SYSTEM.md](docs/DESIGN_SYSTEM.md) before visual changes and [docs/COMPONENTS.md](docs/COMPONENTS.md) before changing markup or interactions.
4. Read [docs/DEVELOPMENT.md](docs/DEVELOPMENT.md) before implementing notebook conversion.

## Project conventions

- Keep website copy, code examples, accessibility labels, and repository documentation in English. The user currently speaks Spanish in conversation.
- Neo is the default dark theme; Old is the classic light Aero theme. Preserve both, their distinct desktop layouts, and persisted theme selection.
- Edit the source in `dist/` directly: this is a buildless site, despite the directory name. There is no package manager, generated application bundle, or notebook converter yet.
- Scope theme-specific styling to `[data-theme=neo]` or `[data-theme=old]`. Read the final applicable declarations: `styles.css` has older overrides and responsive rules with differing specificity.
- The CSS and running application are authoritative. Documentation snapshots and the in-app exported variables are references, not a complete runtime token engine.
- Prefer established components and materials over introducing another visual system. The historical revisions listed in HANDOFF include rejected experiments.
- Follow the user's latest task scope and checkpoint requests. Do not create new approval gates from this file.

## Verification and handoff

- Use `node --check dist/assets/app.js` for JavaScript changes and `git diff --check` before committing.
- For visual changes, inspect both themes at relevant widths when browser tooling is available; report any unverified coverage accurately.
- When changing approved colors, materials, or component contracts, update the corresponding documentation and reference snapshots.
- The next planned task is converting a user-supplied Jupyter notebook. No notebook has arrived yet; inspect the actual file before choosing a converter architecture or execution policy.
