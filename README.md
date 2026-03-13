# Sci·Canvas — Grafica Scientifica Interattiva

<div align="center">

![SciCanvas Logo](https://img.shields.io/badge/Sci%C2%B7Canvas-LIVE%20SCIENTIFIC%20GRAPHICS%20ENGINE-00f0ff?style=for-the-badge&labelColor=0b0e14)
![License](https://img.shields.io/badge/Licenza-GNU%20GPL%203.0-00ffaa?style=flat-square)
![Language](https://img.shields.io/badge/Linguaggio-HTML%20%2F%20JavaScript-ff6600?style=flat-square)
![Version](https://img.shields.io/badge/Anno-2026-cc44ff?style=flat-square)

**Ambiente di sviluppo scientifico interattivo single-file — nessuna installazione, tutto nel browser.**

### 🚀 [APRI SCICANVAS](https://htmlpreview.github.io/?https://raw.githubusercontent.com/YouDevIt/SciCanvas/master/SciCanvas.html) &nbsp;|&nbsp; 📖 [LEGGI IL MANUALE](https://htmlpreview.github.io/?https://raw.githubusercontent.com/YouDevIt/SciCanvas/master/SciCanvas-manual.html)

</div>

---

## Cos'è SciCanvas

**SciCanvas** è un ambiente integrato per la grafica e la simulazione scientifica che gira interamente nel browser, distribuito come un singolo file HTML auto-contenuto. Non richiede installazione, server o dipendenze esterne: basta aprire il file.

È pensato per studenti, docenti, ricercatori e appassionati che vogliono visualizzare fenomeni fisici, esplorare la geometria euclidea, fare calcoli simbolici e produrre notebook scientifici interattivi — il tutto in un'unica interfaccia dark, fluida e reattiva.

---

## Caratteristiche principali

### ⌨️ Tab CODICE — Editor con API scientifica
- **Editor** con syntax highlighting, numerazione righe e drag & drop di file `.js`/`.txt`
- **REPL** interattivo per eseguire espressioni al volo
- **API grafica** ricca: oltre 70 funzioni per disegno, animazione, grafici, campi fisici, LaTeX
- **Animazione** in tempo reale con `animate(t, dt)` e loop a 60 fps
- **Grafici**: `plot()`, `parametric()`, `scatter()`, `barChart()`, `gaussian()`, `contour()`
- **Campi fisici**: `vectorField()`, `electricField()`, `magneticField()`, `streamlines()`, `equipotential()`
- **Oggetti fisici** preconfezionati: `pendulum()`, `spring()`, `mass()`, `wave()`, `particle()`
- **LaTeX sul canvas** con MathJax: `latex()`, `latexBlock()`
- **CAS simbolico** via Algebrite: `calc()` per derivate, integrali, limiti, fattorizzazione…
- **Costanti fisiche CODATA** con incertezze e propagazione errori: `physConst()`, `val()`, `convert()`
- **Formulario fisico**: oltre 100 formule risolvibili per qualsiasi variabile con `formula()`
- **UI interattiva** sul canvas con slider, checkbox, color picker via `createUI()`
- **15 esempi** completi + 27 snippet API pronti all'uso
- **Gestione script**: salvataggio locale, import/export `.js`
- **Export**: PNG, SVG, HTML standalone

### ⛛ Tab SIM — Simulatore fisico 2D
- Motore fisico rigido 2D con integrazione **RK4**
- **7 tipi di corpo**: Particella, Blocco, Sfera, Cilindro, Asta, Anello + Punto fisso
- **13 vincoli**: Perno, Cerniera, Distanza, Guide (H/V/angolare), Piano, Piano inclinato, Fune, Parete, Box, Collisioni elastiche
- **Forze globali** configurabili: Gravità (anche variabile nel tempo), Campo elettrico E, Campo magnetico B, Attrito viscoso
- **Forze per corpo**: Molle (con smorzamento), Forze costanti, Gravità N-corpo
- **Custom Draw**: editor JS per disegnare overlay personalizzati sulla simulazione
- **Custom Step**: funzione JS richiamata ad ogni passo per fisica custom avanzata
- Salvataggio/caricamento scenari in JSON

### △ Tab GEO — Geometria euclidea interattiva
- Costruzione dinamica e vincolata in stile GeoGebra
- **22 strumenti**: Punto, Retta, Segmento, Cerchio (2 metodi), Conica 5P, Ellisse F₁F₂P, Iperbole, Fuochi/Centro, Perpendicolare, Parallela, Punto medio, Intersezione, Poligono, Slider, Bisettrice, Punto su oggetto, Angolo, Distanza, Asse radicale, Testo
- **Input testuale** per definire oggetti con espressioni (es. `A=(2,3)`, `c=circle(O,a)`, `a=0..5`)
- **Vista algebrica** in tempo reale con valori numerici degli oggetti
- **Note di costruzione** visibili sul canvas
- Esportazione costruzioni in JSON

### 📓 Tab NOTE — Notebook scientifico
- Notebook a **celle miste**: Markdown+MathJax, Codice JS, Simulazione SIM embedded, Costruzione GEO embedded, Formula CAS
- Supporto completo **LaTeX** inline (`$...$`) e display (`$$...$$`)
- Celle riordinabili, duplicabili, eliminabili
- Salvataggio/caricamento multipli notebook in localStorage
- **Export HTML standalone** del notebook intero

---

## File inclusi in questo repository

| File | Descrizione |
|------|-------------|
| `SciCanvas.html` | Applicazione completa (single-file, auto-contenuta) |
| `SciCanvas-manual.html` | Manuale d'uso dettagliato con riferimento completo alle API |
| `README.md` | Questo file |

---

## Utilizzo immediato

Aprire direttamente nel browser senza installare nulla:

**🔗 [https://htmlpreview.github.io/?https://raw.githubusercontent.com/YouDevIt/SciCanvas/master/SciCanvas.html](https://htmlpreview.github.io/?https://raw.githubusercontent.com/YouDevIt/SciCanvas/master/SciCanvas.html)**

Oppure scaricare `SciCanvas.html` e aprirlo localmente con qualsiasi browser moderno (Chrome, Firefox, Edge, Safari).

**📖 Manuale:**
**[https://htmlpreview.github.io/?https://raw.githubusercontent.com/YouDevIt/SciCanvas/master/SciCanvas-manual.html](https://htmlpreview.github.io/?https://raw.githubusercontent.com/YouDevIt/SciCanvas/master/SciCanvas-manual.html)**

---

## Autore e licenza

**Copyright © 2026 Leonardo Boselli**

- Prompt Designer: **Leonardo Boselli**
- Programmer: **Claude Sonnet 4.6** — [https://claude.ai/](https://claude.ai/)

Distribuito con licenza **[GNU GPL 3.0](https://www.gnu.org/licenses/gpl-3.0.html)** — software libero, modificabile e ridistribuibile a condizione di mantenere la stessa licenza.
