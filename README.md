# yovanyjls.github.io

Portafolio profesional de Yovany Jesús López Serrano. Página estática
bilingüe (ES/EN) con modo claro/oscuro, generada a partir de datos reales
de `cv_generator/` (mismo proyecto que genera el CV en Word/PDF y el
catálogo de cursos en `yovanyjls.github.io/cursos`).

## Archivos

- `index.html` — estructura de la página.
- `style.css` — estilos (paleta clara/oscura, tipografía).
- `app.js` — TODOS los datos (perfil, experiencia, cursos destacados,
  íconos de tecnologías) van embebidos aquí como constantes JS, más la
  lógica de idioma/tema/render. No se usa `fetch` a JSON externos.
- `cv-es.pdf` / `cv-en.pdf` — el CV descargable (formato Harvard), uno por
  idioma. Se sirven con nombre simple para poder reemplazarlos fácilmente.

## Cómo regenerar este sitio cuando cambien los datos

Este sitio se arma con un script en el proyecto del CV, no se edita
`app.js` a mano:

```
cd cv_generator/_build
python armar_sitio_principal.py
```

Esto lee:
- `cv_generator/_build/_sitio_principal.json` (perfil + 15 empleos,
  regenerado con `python exportar_sitio_principal.py` si cambia algún
  `empleos/*.yaml` o `datos_personales.yaml`).
- `cv_generator/_build/_cursos_harvard.json` (los 6 cursos destacados,
  deben coincidir con `CURSOS_HARVARD_URLS` en `core/render_docx.py`).
- `cv_generator/_build/iconos_config.py` (qué íconos van en la sección
  Tecnologías, primaria vs. secundaria, y el flag `visible` por si quieres
  ocultar alguno puntual sin quitarlo del archivo).

El script escribe el HTML completo en una ruta de scratchpad (para
previsualizar como Artifact); para publicar aquí, se extraen el
`<style>`/`<script>` a `style.css`/`app.js` y se copian junto con los PDF
del CV Harvard más recientes (`cv_generator/salida/CV_..._harvard_.../`).

## Cache del navegador al actualizar style.css o app.js

`index.html` referencia esos archivos como `style.css?v=2` / `app.js?v=2`.
Cada vez que edites cualquiera de los dos, **sube en 1 ese número** en
`index.html` (ej. `?v=3`) — si no, algunos navegadores (sobre todo
móviles) pueden seguir usando la versión vieja en caché aunque el HTML sí
se actualice.

## Actualizar los PDF del CV

Cuando generes una versión nueva del CV en formato Harvard (ES y EN) con
`cli.py`, copia los dos PDF aquí sobrescribiendo `cv-es.pdf` y `cv-en.pdf`
(mismo nombre siempre, así no hay que tocar `app.js`).

## Ver el sitio en tu computadora antes de publicar cambios

```
python -m http.server 8000
```

y abre `http://localhost:8000` (abrir `index.html` con doble clic también
funciona aquí, ya que los datos van embebidos en `app.js` y no se usa
`fetch`).
