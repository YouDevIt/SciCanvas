# SciCanvas

[📘 MANUAL.md](MANUAL.md) • [▶️ SciCanvas.html](SciCanvas.html)

**SciCanvas** is a self-contained, single-file scientific graphics engine that runs entirely in the browser. Write a script in the built-in editor and see the result rendered live on an interactive canvas — no build step, no dependencies to install, no server required.

```
open scicanvas.html   →   start coding
```

---

## Features

- **Live canvas** with pan (drag) and zoom (scroll wheel), coordinate systems, axes, and grid
- **Plotting** — functions, parametric curves, scatter, bar charts, filled areas, Gaussian bells
- **Physics visualisation** — vector fields, streamlines, contour maps, electric/magnetic fields, equipotentials
- **Animation engine** — `animate(t => { … })` with frame counter, delta-time, and FPS display
- **Interactive input** — mouse click/move callbacks, keyboard events, `createUI()` floating control panels (powered by [lil-gui](https://lil-gui.georgealways.com/))
- **Physica** — dimensional quantities with full unit analysis and uncertainty propagation (CODATA values)
- **Formulario** — 52 physics/chemistry/maths formulas, all algebraically invertible
- **Algebrite CAS** — symbolic calculus (derivatives, integrals, factoring, roots …) via [Algebrite](http://algebrite.org/)
- **LaTeX rendering** on canvas via [MathJax 3](https://www.mathjax.org/)
- **Physical constants** — 40+ CODATA-2018 constants with uncertainties; 100+ elements of the periodic table; 20+ celestial bodies
- **Unit registry** — 319 unit aliases covering SI, imperial, astronomical, particle-physics units, and more
- **Interactive REPL** — persistent `_` namespace, command history, tab-completion, console redirection
- **Standalone HTML export** — click **⬡ HTML** to download a minimal, self-contained file that runs only your script, with no editor UI and only the libraries your script actually uses

---

## Quick Start

1. Download `scicanvas.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. Replace the default script in the editor with your own — or pick an example from the **ESEMPI** tab
4. Press **▶ ESEGUI** (or `Ctrl+Enter`) to run

---

## Interface Overview

| Area | Description |
|---|---|
| **Header** | Run / Stop / Clear / PNG export / HTML export / UI scale |
| **Canvas** (right) | Render area — drag to pan, scroll to zoom |
| **📝 CODICE** | Code editor with syntax highlighting |
| **⚡ REPL** | Interactive console with persistent `_` namespace |
| **📖 API** | Searchable API reference — click any item to insert it |
| **📋 ESEMPI** | 15 ready-to-run examples |
| **💾 SCRIPT** | Save / load / download scripts |

### UI Scale
Use the **−** / **+** buttons in the header to scale the editor interface (70 %–200 %) without touching the canvas resolution. The preference is saved in `localStorage`.

---

## Hello World

```js
bg('#050c18');
setCoords(-5, 5, -4, 4);
grid(1, 1, '#0d1525');
axes();

plot(x => Math.sin(x), { color: '#00d4ff', width: 2 });
plot(x => Math.cos(x), { color: '#00ff88', width: 2 });

fill('#00d4ff');
font(14, 'monospace');
text('sin(x) & cos(x)', -4.8, 3.5);
```

---

## Animated Example

```js
bg('#020810');
setCoords(-5, 5, -4, 4);

animate(t => {
  bg('#020810');
  fill('#00d4ff');
  circle(3 * Math.cos(t), 2 * Math.sin(t), 0.2);
});
```

---

## Standalone Export

Click **⬡ HTML** in the header to export a standalone HTML file containing only:
- the canvas
- the drawing engine
- your script
- only the external libraries (MathJax, Algebrite, lil-gui) that your script actually uses

The exported file runs as a full-screen animation or graphic — no editor, no panels.

---

## Requirements

A modern browser with ES2020+ support. No installation required.

| Browser | Minimum version |
|---|---|
| Chrome / Edge | 85+ |
| Firefox | 79+ |
| Safari | 15+ |

---

## File Structure

```
scicanvas.html   — the entire application (single file, ~2900 lines)
README.md        — this file
MANUAL.md        — complete API and feature reference
```

---

## License

SciCanvas is free software released under the **GNU General Public License v3.0**.

```
Copyright (C) 2025  SciCanvas Contributors

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <https://www.gnu.org/licenses/>.
```

See [LICENSE](LICENSE) for the full text of the GNU GPL v3.

---

## Third-party Libraries

SciCanvas loads the following libraries from CDN at runtime (only when the user's script requires them):

| Library | Version | License | Purpose |
|---|---|---|---|
| [Algebrite](http://algebrite.org/) | 1.4.0 | MIT | Symbolic computer algebra |
| [MathJax](https://www.mathjax.org/) | 3.x | Apache 2.0 | LaTeX rendering on canvas |
| [lil-gui](https://lil-gui.georgealways.com/) | 0.19 | MIT | Floating UI control panels |

These libraries are **not** bundled in `scicanvas.html` and are subject to their own licenses.
