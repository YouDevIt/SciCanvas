# SciCanvas — Complete Manual

> Version 2025 · Single-file scientific graphics engine for the browser  
> License: GNU GPL v3

---

## Table of Contents

1. [Coordinate System](#1-coordinate-system)
2. [Drawing State](#2-drawing-state)
3. [Primitive Shapes](#3-primitive-shapes)
4. [Text & Labels](#4-text--labels)
5. [Plotting & Charts](#5-plotting--charts)
6. [Physics Visualisation](#6-physics-visualisation)
7. [Animation & Input](#7-animation--input)
8. [User Interface (lil-gui)](#8-user-interface-lil-gui)
9. [LaTeX on Canvas](#9-latex-on-canvas)
10. [Physica — Units & Constants](#10-physica--units--constants)
11. [Formulario — Physics Formulas](#11-formulario--physics-formulas)
12. [Algebrite — Computer Algebra](#12-algebrite--computer-algebra)
13. [Math Utilities](#13-math-utilities)
14. [REPL](#14-repl)
15. [Standalone HTML Export](#15-standalone-html-export)
16. [Global Constants](#16-global-constants)
17. [Coordinate Transforms](#17-coordinate-transforms)

---

## 1. Coordinate System

By default the canvas uses **pixel coordinates** (origin top-left). Call `setCoords` to switch to a named mathematical coordinate system.

### `setCoords(xmin, xmax, ymin, ymax)`

Activates a Cartesian coordinate system. All subsequent drawing calls use mathematical coordinates.

```js
setCoords(-5, 5, -4, 4);    // x ∈ [-5, 5], y ∈ [-4, 4]
setCoords(-10, 10, -7, 7);  // wider view
```

The aspect ratio of the viewport is respected automatically — the shorter axis is expanded to keep circles round.

### `resetCoords()`

Restores pixel coordinates and resets pan/zoom to the identity transform.

### `axes(options?)`

Draws labelled x/y axes with tick marks and optional grid.

```js
axes();   // default style

axes({
  color:      '#2a3d55',   // axis line colour
  gridColor:  '#1e2535',   // grid line colour (null = no grid)
  xLabel:     'x',
  yLabel:     'y',
  labelColor: '#3a5570',
  bold:       true,
  tickSize:   0.1,         // tick length in coordinate units
  fontSize:   11,
});
```

### `grid(stepX, stepY, color, options?)`

Draws a grid independently of axes.

```js
grid(1, 1, '#0d1525');
grid(0.5, 0.5, '#1a2535', { width: 0.5, dash: [2, 4] });
```

---

## 2. Drawing State

SciCanvas maintains a global drawing state. Functions modify that state for all subsequent draw calls until changed again.

### Fill & Stroke

```js
fill('#00d4ff');          // set fill colour (any CSS colour string)
noFill();                 // disable fill
stroke('#ff4466');        // set stroke colour
noStroke();               // disable stroke
lineWidth(2);             // stroke width (coordinate units or pixels depending on context)
alpha(0.7);               // global opacity 0–1 (alias: opacity(v))
lineDash([6, 4]);         // set dash pattern
noDash();                 // clear dash pattern
```

### Push / Pop

```js
push();   // save current drawing state (ctx.save)
pop();    // restore previous state (ctx.restore)
```

### Background

```js
bg('#020810');   // fill the entire canvas with a solid colour
clear();         // clear to transparent
```

---

## 3. Primitive Shapes

All coordinates are in the current coordinate system.

### `rect(x, y, width, height, radius?)`

```js
rect(-2, -1, 4, 2);           // plain rectangle
rect(-1, -1, 2, 2, 0.15);     // rounded corners (radius in coordinate units)
```

### `circle(x, y, radius)`

```js
circle(0, 0, 1.5);
```

### `ellipse(x, y, rx, ry)`

```js
ellipse(0, 0, 2, 1);
```

### `line(x1, y1, x2, y2)`

```js
line(-3, 0, 3, 0);
```

### `arc(x, y, r, angle1, angle2, counterClockwise?)`

Angles in radians.

```js
arc(0, 0, 1, 0, PI / 2);        // quarter circle
arc(0, 0, 1, 0, TAU);           // full circle (same as circle())
```

### `bezier(x1, y1, cx1, cy1, cx2, cy2, x2, y2)`

```js
bezier(-2, 0, -1, 2, 1, 2, 2, 0);
```

### `polygon(points)`

`points` is an array of `[x, y]` pairs. The path is automatically closed.

```js
polygon([[-1, 0], [0, 2], [1, 0]]);   // triangle
```

### `point(x, y, radius?, color?)`

Draws a filled dot. Radius is in pixels.

```js
point(1.5, 2.3, 4, '#ff4466');
```

### `arrow(x1, y1, x2, y2, headSize?, color?)`

```js
arrow(-2, 0, 2, 0);
arrow(0, 0, 1.5, 1.5, 12, '#00d4ff');
```

---

## 4. Text & Labels

### `font(size, family?, style?, weight?)`

Sets the font for subsequent `text()` calls.

```js
font(16, 'Syne, sans-serif', 'normal', '700');
font(12, 'JetBrains Mono, monospace', 'italic');
```

### `textAlign(align)`

`'left'` | `'center'` | `'right'`

### `textBase(baseline)`

`'top'` | `'middle'` | `'bottom'` | `'alphabetic'`

### `text(string, x, y, options?)`

```js
fill('#00d4ff');
font(14, 'monospace');
textAlign('center');
textBase('middle');
text('Hello SciCanvas', 0, 0);

// With inline options:
text('note', 1, 2, { size: 10, color: '#888', weight: 'bold' });
```

### `label(string, x, y, size?, color?, family?)`

Convenience shorthand — always centered and middle-aligned.

```js
label('Origin', 0, 0, 12, '#aaa');
```

### `mathText(string, x, y, size?, color?)`

Renders text in italic serif (Georgia) — suitable for variable names and simple inline maths.

```js
mathText('f(x) = sin(x)', -4, 3, 14, '#00d4ff');
```

---

## 5. Plotting & Charts

### `plot(fn, options?)`

Plots a function `y = f(x)`.

```js
plot(x => Math.sin(x), { color: '#00d4ff', width: 2 });
plot(x => x * x,       { color: '#ff8844', width: 1.5, xmin: -3, xmax: 3 });
plot(x => Math.tan(x), { color: '#aa66ff', steps: 1200, dash: [4, 4] });
```

| Option | Default | Description |
|---|---|---|
| `color` | `'#00d4ff'` | Line colour |
| `width` | `2` | Line width |
| `xmin` | coordinate min | Left bound |
| `xmax` | coordinate max | Right bound |
| `steps` | `800` | Number of sample points |
| `dash` | `null` | Dash pattern array |

### `plotFill(fn, baseline, options?)`

Like `plot` but fills the area between the curve and `baseline`.

```js
plotFill(x => Math.sin(x), 0, {
  color:       'rgba(0,212,255,0.2)',
  strokeColor: '#00d4ff',
  width:       2,
});
```

### `parametric(xFn, yFn, options?)`

Plots a parametric curve `(x(t), y(t))`.

```js
parametric(t => 3 * Math.cos(t), t => 2 * Math.sin(t), {
  tmin: 0, tmax: TAU,
  color: '#aa66ff', width: 2, steps: 500,
});

// Lissajous
parametric(t => Math.sin(3*t), t => Math.sin(2*t + PI/4), {
  tmin: 0, tmax: TAU * 3,
  color: '#00ff88', steps: 1000,
});
```

### `scatter(points, options?)`

```js
const data = [[1, 2.1], [2, 3.8], [3, 2.9], [4, 5.1]];
scatter(data, { color: '#ff4466', r: 5, stroke: '#fff', labels: ['A','B','C','D'] });
```

### `gaussian(mu, sigma, options?)`

Plots a Gaussian (normal) distribution.

```js
gaussian(0, 1,   { color: '#00d4ff', width: 2, fill: true });
gaussian(2, 0.5, { color: '#ff8844', A: 1.5 });
```

### `wave(options?)`

Plots a sine or cosine wave.

```js
wave({ A: 1.5, lambda: 2, phi: PI/4, color: '#00ff88', fill: true });
wave({ type: 'cos', A: 0.8, lambda: 1 });
```

### `barChart(data, options?)`

```js
barChart([42, 67, 31, 85, 54], {
  labels: ['A', 'B', 'C', 'D', 'E'],
  colors: ['#00d4ff', '#ff8844', '#00ff88', '#aa66ff', '#ff4466'],
  x: -4, y: -3, w: 8,
});
```

### `contour(fn, options?)`

Renders a 2D scalar field as a colour-mapped image.

```js
contour((x, y) => Math.sin(x) * Math.cos(y), {
  colorMap: { low: '#000088', mid: '#008800', high: '#ff4400' },
  opacity: 0.9, nx: 120, ny: 90,
});
```

---

## 6. Physics Visualisation

### `vectorField(fxFn, fyFn, options?)`

Draws a 2D vector field as arrows.

```js
vectorField(
  (x, y) => -y,
  (x, y) =>  x,
  { color: '#00d4ff', nx: 16, ny: 12, normalize: true, scale: 0.7 }
);
```

### `streamlines(fxFn, fyFn, options?)`

Traces streamlines through a vector field.

```js
streamlines((x, y) => -y, (x, y) => x, {
  seeds: 30, steps: 400, dt: 0.02,
  color: '#4488ff', alpha: 0.6,
});
```

### `electricField(charges, options?)`

Visualises the electric field of point charges. `charges` is an array of `[x, y, q]`.

```js
electricField([
  [2, 0,  1],   // positive charge at (2, 0)
  [-2, 0, -1],  // negative charge at (-2, 0)
], { nx: 20, ny: 15 });
```

### `equipotential(charges, options?)`

Colour-maps the electrostatic potential.

```js
equipotential([[2, 0, 1], [-2, 0, -1]], {
  colorMap: { low: '#000066', mid: '#003300', high: '#660000' },
});
```

### `magneticField(wires, options?)`

Visualises the magnetic field of infinite current-carrying wires. `wires` is an array of `[x, y, I]`.

```js
magneticField([[0, 0, 1]], { color: '#aa66ff' });
```

### Physics Objects (decorative)

These render a styled symbol at a given position.

```js
spring(x1, y1, x2, y2, options?)    // coil spring between two points
pendulum(x, y, length, angle, options?)
mass(x, y, radius?, options?)        // filled square mass block
wall(x, y, width, height, options?)  // hatched wall
charge(x, y, q, options?)            // ⊕ / ⊖ charge symbol
particle(x, y, radius?, options?)    // circle with glow
```

---

## 7. Animation & Input

### `animate(callback)`

Starts the animation loop. The callback is called every frame.

```js
animate((t, dt, frame) => {
  bg('#020810');
  circle(Math.cos(t) * 3, Math.sin(t) * 2, 0.3);
});
```

| Parameter | Type | Description |
|---|---|---|
| `t` | `number` | Elapsed time in seconds |
| `dt` | `number` | Delta-time since last frame (seconds) |
| `frame` | `number` | Frame counter (integer, starts at 1) |

### `stop()`

Stops the animation loop (also available as the **■ STOP** button).

### Mouse Input

```js
onMouseMove((x, y, event) => {
  // x, y in coordinate-system units
});

onClick((x, y, button, event) => {
  // button: 0 = left, 1 = middle, 2 = right
});

const m = getMouse();   // { x, y, px, py, pressed, button }
```

### Keyboard Input

```js
onKeyDown((key, event) => { /* e.g. key === 'ArrowLeft' */ });
onKeyUp((key, event) => { });

const pressed = getKeys();   // Set<string> of currently held keys
```

### Time

```js
const t = getTime();   // elapsed seconds since last runCode()
```

---

## 8. User Interface (lil-gui)

`createUI()` creates a floating panel of controls that overlay the canvas.

**Requires** the [lil-gui](https://lil-gui.georgealways.com/) CDN library (loaded automatically when the page is open; included in standalone exports if `createUI` is used).

```js
const ui = createUI({ title: 'Controls', width: 220, top: 10, right: 10 });

const params = { freq: 1, amp: 1.5, color: '#00d4ff', show: true };

const freqCtrl = ui.slider('Frequenza', params, 'freq', 0.1, 5, 0.1);
const ampCtrl  = ui.slider('Ampiezza',  params, 'amp',  0.1, 3, 0.1);
ui.color('Colore', params, 'color');
ui.toggle('Mostra', params, 'show');
ui.button('Reset', () => { params.freq = 1; params.amp = 1.5; });

animate(t => {
  bg('#020810');
  if (params.show)
    plot(x => params.amp * Math.sin(params.freq * x), { color: params.color });
});
```

### UI Methods

| Method | Description |
|---|---|
| `ui.slider(label, obj, key, min, max, step?)` | Numeric slider |
| `ui.number(label, obj, key, min?, max?, step?)` | Number input |
| `ui.color(label, obj, key)` | Colour picker |
| `ui.toggle(label, obj, key)` | Boolean checkbox |
| `ui.select(label, obj, key, options)` | Dropdown (`options`: array or object) |
| `ui.text(label, obj, key)` | String input |
| `ui.button(label, fn)` | Button with callback |
| `ui.folder(label)` | Collapsible sub-panel (returns a nested UI wrapper) |

### Control Value Reference

```js
const ctrl = ui.slider('Speed', p, 'speed', 0, 10);
ctrl.value;          // read current value
ctrl.value = 5;      // set and update the slider
```

---

## 9. LaTeX on Canvas

LaTeX is rendered via MathJax 3 into SVG and then drawn onto the canvas. Rendering is asynchronous — the result appears on the next frame after the first render.

### `latex(expr, x, y, options?)`

Inline (display=false) or display-mode LaTeX at position `(x, y)`.

```js
latex('E = mc^2', 0, 2);
latex('\\int_0^\\infty e^{-x^2}\\,dx = \\frac{\\sqrt{\\pi}}{2}', -3, 1, {
  color: '#00d4ff',
  size:  22,
  align: 'center',   // 'left' | 'center' | 'right'
  base:  'middle',   // 'top' | 'middle' | 'bottom'
});
```

### `latexBlock(expr, x, y, options?)`

Like `latex` but with `display: true` (centred fraction/integral style).

```js
latexBlock('\\sum_{n=1}^{\\infty} \\frac{1}{n^2} = \\frac{\\pi^2}{6}', 0, 0, {
  color: '#aa66ff', size: 28,
});
```

### `clearLatexCache()`

Clears the internal SVG cache. Call this if you are dynamically changing many different LaTeX expressions.

---

## 10. Physica — Units & Constants

Physica provides a dimensional quantity system with unit analysis and uncertainty propagation.

### `val(value, unit?)`

Creates a `Qty` object. `unit` is any registered unit string.

```js
const d  = val(150, 'km');
const t  = val(1.5, 'h');
const v  = d.div(t);               // 100 km/h
console.log(v.to('m/s').toStr(3)); // "27.8 m/s"
```

### `Qty` Methods

| Method | Description |
|---|---|
| `.to(unit)` | Convert to a different unit — throws if dimensions are incompatible |
| `.toStr(sigfigs?, unit?)` | Formatted string: `"1.234 m/s"` |
| `.toLaTeX(sigfigs?)` | LaTeX string: `"1.23 \\; \\mathrm{m/s}"` |
| `.add(qty)` | Addition (same dimensions required) |
| `.sub(qty)` | Subtraction |
| `.mul(qty\|number)` | Multiplication |
| `.div(qty\|number)` | Division |
| `.pow(n)` | Integer power |
| `.sqrt()` | Square root |
| `.neg()` | Negate |
| `.abs()` | Absolute value |
| `.value` | Numeric value in SI base units |
| `.unit` | Unit string |
| `.uncertainty` | Absolute uncertainty (if set) |

### `convert(value, fromUnit, toUnit)`

Quick scalar conversion — no `Qty` object created.

```js
convert(100, 'km/h', 'm/s')   // → 27.7778
convert(1,   'atm',  'Pa')    // → 101325
```

### `physConst(key, unit?)`

Returns a `Qty` with the CODATA value and uncertainty. Optionally converts to `unit`.

```js
const c   = physConst('c');           // speed of light in m/s
const c_  = physConst('c', 'km/s');   // in km/s
const me  = physConst('me', 'MeV/c2'); // electron mass in MeV/c²

console.log(c.toStr(5));   // "2.9979 × 10⁸ m/s"
```

### Physical Constants

| Key | Symbol | Quantity | Unit |
|---|---|---|---|
| `c` | c | Speed of light | m/s |
| `h` | h | Planck constant | J·s |
| `hbar` | ℏ | Reduced Planck constant | J·s |
| `e` / `e_c` | e | Elementary charge | C |
| `k_B` | k_B | Boltzmann constant | J/K |
| `N_A` | N_A | Avogadro constant | mol⁻¹ |
| `R` | R | Molar gas constant | J/mol/K |
| `G` | G | Gravitational constant | m³/s²/kg |
| `mu0` | μ₀ | Permeability of vacuum | H/m |
| `eps0` | ε₀ | Permittivity of vacuum | F/m |
| `me` | mₑ | Electron mass | kg |
| `mp` | mₚ | Proton mass | kg |
| `mn` | mₙ | Neutron mass | kg |
| `mu_m` | u | Atomic mass unit | kg |
| `a0` | a₀ | Bohr radius | m |
| `F_c` | F | Faraday constant | C/mol |
| `sigma` | σ | Stefan–Boltzmann constant | W/m²/K⁴ |
| `wien` | b | Wien displacement constant | m·K |
| `mu_B` | μ_B | Bohr magneton | J/T |
| `g0` | g₀ | Standard gravity | m/s² |
| `H0` | H₀ | Hubble constant (Planck 2018) | km/s/Mpc |
| `T_CMB` | T_CMB | CMB temperature | K |
| `Lp` | l_P | Planck length | m |
| `tp` | t_P | Planck time | s |
| `alpha` | α | Fine-structure constant | — |

Full list (40+ constants): call `listConst()` in the REPL.

### `astroBody(name, property?)`

Returns data for solar-system bodies. `property` can be `'mass'`, `'radius'`, `'distance'`, etc.

```js
astroBody('Earth')           // full data object
astroBody('Jupiter', 'mass') // Qty for Jupiter's mass
```

Bodies: Sun, Moon, Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune, Pluto, ISS (and more).

### `chemElem(symbolOrNumber)`

Returns periodic table data for an element.

```js
chemElem('Fe')   // Iron: { name, symbol, z, mass_u, ... }
chemElem(79)     // Gold
```

### Unit Registry

SciCanvas recognises **319 unit aliases** including:

- **Length** — m, km, cm, mm, μm, nm, pm, Å, in, ft, yd, mi, nmi, ly, pc, kpc, Mpc, AU, R☉, R⊕ …
- **Mass** — kg, g, mg, t, lb, oz, slug, u, Da, M☉, M⊕ …
- **Time** — s, ms, μs, ns, min, h, d, yr, Myr, Gyr …
- **Energy** — J, kJ, MJ, eV, keV, MeV, GeV, cal, kcal, kWh, BTU …
- **Force** — N, kN, MN, lbf, dyn …
- **Pressure** — Pa, kPa, MPa, bar, atm, torr, mmHg, psi …
- **Temperature** — K, °C, °F (via `convert()`) …
- **Particle physics mass** — MeV/c², GeV/c², keV/c², eV/c² …
- **Compound** — m³/s²/kg, J·s, mol⁻¹, J/T, C/mol, F/m, H/m …

### Arithmetic API

For functional-style programming without chaining:

```js
qmul(a, b)   // a.mul(b)
qdiv(a, b)   // a.div(b)
qadd(a, b)   // a.add(b)
qsub(a, b)   // a.sub(b)
qpow(a, n)   // a.pow(n)
qsqrt(a)     // a.sqrt()
qneg(a)      // a.neg()
qabs(a)      // a.abs()
```

---

## 11. Formulario — Physics Formulas

The Formulario contains **52 physics, chemistry, and mathematics formulas** that are algebraically invertible — given any subset of variables, it solves for the missing ones.

### `formula(key, given)`

Returns a `Qty` with the solved variable attached with metadata.

```js
const res = formula('ke', { m: 0.145, v: 44.7 });
// res           — Ek as a Qty (in J)
// res.solvedFor — 'Ek'
// res.formulaSym   — 'Ek = ½mv²'
// res.formulaLatex — 'E_k = \\tfrac{1}{2} m v^2'
// res.cat          — 'Energia'

// Invert — solve for v given Ek and m:
formula('ke', { Ek: 3.2, m: 0.1 }).toStr(4)  // → "8.000 m/s"
```

### `formulaInfo(key)`

Returns full metadata for a formula.

```js
formulaInfo('ideal_gas')
// { key, name, sym, latex, vars, solveFor, cat }
```

### `formulaList(category?)`

Returns an array of keys, optionally filtered by category.

```js
formulaList()               // all 52 keys
formulaList('Termodinamica') // ['ideal_gas', 'heat', 'carnot', 'stefan', 'wien']
```

### `formulaCategories()`

Returns all category names.

### `formulaSolve(key, varName)`

Returns the symbolic solution string via Algebrite.

```js
formulaSolve('ke', 'v')   // "sqrt(2*Ek/m)"
```

### Formula Catalogue

#### Dinamica
| Key | Formula | Solves for |
|---|---|---|
| `newton2` | F = m·a | F, m, a |
| `impulse` | J = F·Δt | J, F, Δt |
| `momentum` | p = m·v | p, m, v |
| `friction` | f = μ·N | f, μ, N |
| `circ_accel` | a = v²/r | a, v, r |

#### Cinematica
| Key | Formula | Solves for |
|---|---|---|
| `moto_rett` | s = v·t | s, v, t |
| `moto_acc` | v = v₀ + a·t | v, v₀, a, t |
| `spazio_acc` | s = v₀t + ½at² | s, a, t |
| `free_fall` | h = ½g·t² | h, t |

#### Energia
| Key | Formula | Solves for |
|---|---|---|
| `ke` | Ek = ½mv² | Ek, m, v |
| `pe_grav` | Ep = mgh | Ep, m, h |
| `pe_spring` | Ee = ½kx² | Ee, k, x |
| `work` | W = F·d·cos θ | W, F, d |
| `power` | P = W/t | P, W, t |
| `mass_energy` | E = mc² | E, m |

#### Gravitazione
| Key | Formula | Solves for |
|---|---|---|
| `grav_force` | F = G·m₁m₂/r² | F, r |
| `kepler3` | T² = 4π²r³/GM | T, r, M |
| `escape_v` | v = √(2GM/r) | v, r |
| `schwarzschild` | rs = 2GM/c² | rs, M |

#### Termodinamica
| Key | Formula | Solves for |
|---|---|---|
| `ideal_gas` | PV = nRT | P, V, n, T |
| `heat` | Q = mcΔT | Q, m, ΔT |
| `carnot` | η = 1 − Tc/Th | η, Th, Tc |
| `stefan` | P = εσAT⁴ | P, T, A |
| `wien` | λmax = b/T | λ, T |

#### Onde
| Key | Formula | Solves for |
|---|---|---|
| `wave_v` | v = f·λ | v, f, λ |
| `doppler` | f' = f(v+v₀)/(v−vs) | f', f |
| `pendulum` | T = 2π√(L/g) | T, L |
| `spring_osc` | T = 2π√(m/k) | T, m, k |

#### Ottica
| Key | Formula | Solves for |
|---|---|---|
| `snell` | n₁ sin θ₁ = n₂ sin θ₂ | θ₂, n₂ |
| `thin_lens` | 1/f = 1/do + 1/di | f, do, di |

#### Elettromagnetismo
| Key | Formula | Solves for |
|---|---|---|
| `ohm` | V = I·R | V, I, R |
| `joule` | P = V·I | P, V, I |
| `capacitor` | Q = C·V | Q, C, V |
| `coulomb` | F = k·q₁q₂/r² | F, r |
| `lorentz` | F = qvB sin θ | F, B, v |
| `faraday` | ε = −N·dΦ/dt | ε, dΦ, dt |

#### Fisica Moderna
| Key | Formula | Solves for |
|---|---|---|
| `photon_e` | E = hf | E, f |
| `photon_e_lam` | E = hc/λ | E, λ |
| `debroglie` | λ = h/mv | λ, v, m |
| `lorentz_factor` | γ = 1/√(1−v²/c²) | γ, v |
| `time_dilation` | Δt' = γ·Δt | Δt', Δt |
| `hydrogen` | En = −13.6/n² eV | En |

#### Chimica
| Key | Formula | Solves for |
|---|---|---|
| `molarity` | c = n/V | c, n, V |
| `arrhenius` | k = A·e^(−Ea/RT) | k, T |
| `henderson` | pH = pKa + log([A⁻]/[HA]) | pH, ratio |
| `nernst` | E = E° − (RT/nF)·ln Q | E_cell |

#### Astronomia
| Key | Formula | Solves for |
|---|---|---|
| `hubble` | v = H₀·d | v, d |
| `luminosity` | L = 4πR²σT⁴ | L, R, T |
| `abs_magnitude` | m−M = 5 log(d/10 pc) | d, m, M |

#### Matematica / Statistica
| Key | Formula | Solves for |
|---|---|---|
| `quadratic` | x = (−b ± √Δ) / 2a | x₁, x₂ |
| `compound_interest` | A = P(1+r/n)^(nt) | A, P, t |
| `normal_pdf` | f(x) = Gaussian PDF | f |

---

## 12. Algebrite — Computer Algebra

Algebrite provides symbolic algebra inside SciCanvas. Requires a CDN connection (always available in full SciCanvas; included in standalone exports when `algebrite()` is called).

### `algebrite(expression)`

Evaluates an Algebrite expression and returns `{ text, symja, latex }`.

```js
algebrite('d(x^2 + 3*x, x)')         // → { text: "2*x+3", latex: "2 x+3", … }
algebrite('integral(x^2, x)')         // → "1/3*x^3"
algebrite('simplify(sin(x)^2+cos(x)^2)') // → "1"
algebrite('factor(x^2 - 4)')          // → "(x+2)*(x-2)"
algebrite('solve(x^2 - 4, x)')        // → "[2,-2]"
algebrite('eigenvalues([[1,2],[3,4]])')
```

### Common Operations

| Expression | Result |
|---|---|
| `d(f, x)` | Derivative of f with respect to x |
| `integral(f, x)` | Indefinite integral |
| `lim(f, x, a)` | Limit as x → a |
| `taylor(f, x, 0, 5)` | Taylor series around 0 to order 5 |
| `factor(expr)` | Factorise |
| `expand(expr)` | Expand |
| `simplify(expr)` | Simplify |
| `solve(eq, x)` | Solve equation for x |
| `roots(poly, x)` | Polynomial roots |
| `nroots(poly, x)` | Numerical roots |
| `subst(expr, x, val)` | Substitute value |
| `gcd(a, b)` | Greatest common divisor |
| `isprime(n)` | Primality test |
| `printlatex(expr)` | Get LaTeX string |
| `eigenvalues(M)` | Matrix eigenvalues |
| `transpose(M)` | Matrix transpose |
| `det(M)` | Matrix determinant |

### Rendering a CAS result as LaTeX

```js
const res = algebrite('integral(sin(x)^2, x)');
latexBlock(res.latex, 0, 1, { color: '#00ff88', size: 24 });
```

---

## 13. Math Utilities

These are available as global functions inside every script.

### `map(value, inMin, inMax, outMin, outMax)`

Linearly maps a value from one range to another.

```js
map(0.5, 0, 1, -5, 5)   // → 0
map(75,  0, 100, 0, TAU) // → 4.712
```

### `lerp(a, b, t)`

Linear interpolation. `t = 0` → `a`, `t = 1` → `b`.

```js
lerp(0, 10, 0.3)  // → 3
```

### `clamp(value, min, max)`

Clamps a value to `[min, max]`.

```js
clamp(1.5, 0, 1)  // → 1
```

### `lerpColor(colorA, colorB, t)`

Linearly interpolates between two hex colours.

```js
lerpColor('#000000', '#00d4ff', 0.5)  // → '#006880'
```

### `random(min?, max?)`

Returns a random float. With no arguments: `[0, 1)`. With arguments: `[min, max)`.

```js
random()        // [0, 1)
random(2, 5)    // [2, 5)
```

### `randomInt(min, max)`

Returns a random integer in `[min, max]` inclusive.

```js
randomInt(1, 6)   // simulates a dice roll
```

### `dist(x1, y1, x2, y2)`

Euclidean distance.

```js
dist(0, 0, 3, 4)  // → 5
```

### `norm(x, y)`

Length of a 2D vector.

### `gradient(x, y, x2, y2, ...colorStops)`

Creates a linear gradient for use with `fill()` or `stroke()`.

```js
fill(gradient(-3, 0, 3, 0, '#001a2e', '#00d4ff'));
rect(-3, -1, 6, 2);
```

### `radialGradient(cx, cy, r1, r2, ...colorStops)`

Creates a radial gradient.

---

## 14. REPL

The **⚡ REPL** tab provides an interactive console for exploring the API.

### Persistent Namespace

The `_` object persists across commands.

```
› _.r = val('3 m')
← Qty { value: 3, unit: 'm', … }

› _.A = val('PI * _.r.value^2 m^2')
› _.A.toStr(4)
← "28.27 m²"
```

### Built-in Commands

| Command | Description |
|---|---|
| `.help` | Show command reference |
| `cls()` / `.clear` | Clear output |
| `clc()` | Clear canvas |
| `dir()` / `.vars` | List all `_.*` variables |
| `reset()` | Clear namespace and canvas |
| `print(v, …)` | Output to REPL (not browser console) |

### Keyboard Shortcuts

| Key | Action |
|---|---|
| `Enter` | Execute command |
| `↑` / `↓` | Navigate command history (200 entries) |
| `Tab` | Autocomplete API function names |
| `Ctrl+L` | Clear output |
| `Ctrl+Enter` | Execute (overrides global run shortcut) |

### REPL Canvas Interaction

Every command that draws something is reflected immediately on the canvas.

```
› bg('#020810')
› setCoords(-5, 5, -4, 4)
› fill('#00d4ff'); circle(0, 0, 1.5)
← undefined
```

---

## 15. Standalone HTML Export

Click the **⬡ HTML** button in the header to generate a standalone HTML file.

### What is included

- Full-screen `<canvas>` (no editor, no panels)
- The complete drawing engine (coordinate transforms, API, all drawing primitives)
- Pan and zoom (drag / scroll-wheel)
- Your script embedded in a `<script type="text/plain">` block
- Only the CDN libraries that your script actually uses:
  - **Algebrite** — only if `algebrite()` is called
  - **MathJax** — only if `latex()` or `latexBlock()` is called
  - **lil-gui** — only if `createUI()` is called
- **Physica** (units, constants, Formulario) — only if any Physica API is called

### What is stripped

- The code editor, REPL, API reference, examples, and script manager panels
- The status bar, toolbar, and UI-scale controls
- All editor-related JavaScript (≈ 40 % of the total code)
- Unused libraries

### Error display

If the script throws, an error message appears in the bottom-left corner.

---

## 16. Global Constants

These are always available inside scripts.

| Name | Value | Description |
|---|---|---|
| `PI` | 3.14159… | π |
| `TAU` | 6.28318… | 2π |
| `E` | 2.71828… | Euler's number |
| `W` | canvas width in pixels | Read-only |
| `H` | canvas height in pixels | Read-only |
| `canvas` | `HTMLCanvasElement` | The canvas element |
| `ctx` | `CanvasRenderingContext2D` | 2D rendering context |
| `Math` | `Math` | Standard JS Math object |
| `sin`, `cos`, `tan` | — | Aliases for `Math.sin` etc. |
| `abs`, `sqrt`, `exp`, `log` | — | Aliases for Math functions |
| `floor`, `ceil`, `round` | — | Aliases for Math functions |
| `min`, `max` | — | Aliases for `Math.min`/`Math.max` |
| `pow` | — | Alias for `Math.pow` |

---

## 17. Coordinate Transforms

When `setCoords` is active, SciCanvas exposes the transform functions as part of the API. These are useful for mixing coordinate-system drawing with direct pixel-level canvas operations.

| Function | Description |
|---|---|
| `sx(x)` | World x → canvas pixel x |
| `sy(y)` | World y → canvas pixel y |
| `wx(px)` | Canvas pixel x → world x |
| `wy(py)` | Canvas pixel y → world y |
| `sw(w)` | World width → pixel width |
| `sh(h)` | World height → pixel height |

```js
setCoords(-5, 5, -4, 4);

// Draw a native canvas 2px red border at world (0,0):
ctx.strokeStyle = 'red';
ctx.lineWidth   = 2;
ctx.strokeRect(sx(-1), sy(1), sw(2), sh(2));
```

---

*This manual covers SciCanvas v2025. For the latest version see the project repository.*
