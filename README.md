# Manim · Órbita

Propuesta de estilo para una web de tutoriales de Manim. Paleta índigo, salvia y coral, superficies Aero y acentos de neón menta y lila.

## Abrir

Abre `dist/index.html` directamente en el navegador. No requiere instalación ni compilación. `dist/orbita.html` conserva el enlace anterior y redirige a la página principal.

Para servirla por HTTP localmente:

```sh
python3 -m http.server 5173 --directory dist
```

## Archivos

- `dist/index.html`: entrada de la página.
- `dist/assets/app.js`: contenido de la lección, ficha de estilos, descargas y controles interactivos.
- `dist/assets/styles.css`: diseño y adaptación a pantallas pequeñas.
- `.openai/hosting.json`: identidad de Sites; no implica que el sitio esté publicado.

La vista previa de la animación es una simulación en el navegador. El ejemplo Python se puede descargar y ejecutar por separado con Manim Community.

## Comprobación

```sh
node --check dist/assets/app.js
```

Revisar en el navegador los cambios visuales, la navegación, la ficha de estilos y los controles de reproducción.
