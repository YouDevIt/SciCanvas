# Sci·Canvas — Live Scientific Graphics Engine

> **Un'applicazione web monolitica per la grafica scientifica interattiva nel browser.**

[![Licenza: GPL v3](https://img.shields.io/badge/Licenza-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Single File](https://img.shields.io/badge/Formato-Single%20HTML-green.svg)]()
[![No Install](https://img.shields.io/badge/Installazione-Zero-brightgreen.svg)]()

---

## 🚀 Avvio rapido

**Nessuna installazione richiesta.** Apri direttamente nel browser:

👉 **[Apri SciCanvas](https://htmlpreview.github.io/?https://github.com/YouDevIt/SciCanvas/blob/main/SciCanvas.html)**

Oppure scarica [`SciCanvas.html`](https://github.com/YouDevIt/SciCanvas/blob/main/SciCanvas.html) e aprilo localmente con qualsiasi browser moderno.

---

## ✨ Caratteristiche principali

SciCanvas integra **tre ambienti** indipendenti in un singolo file `.html`:

### ⌨ CODICE — Editor JavaScript con API Grafica
- Editor con **syntax highlighting** e numerazione righe
- **API grafica** di alto livello su canvas HTML5
- **REPL interattivo** in modalità JS e **CAS simbolico** (Algebrite)
- Rendering di **formule LaTeX** sul canvas tramite MathJax
- Libreria di **esempi** pre-caricati
- **Documentazione API** integrata
- **Gestione script**: salva, carica ed esporta i propri lavori
- Esportazione come **PNG** o **HTML standalone** (senza editor)

### ⛛ SIM — Simulatore Fisico 2D
- Corpi rigidi: **Particella, Blocco, Sfera, Cilindro, Asta, Anello**
- Vincoli: **Punto fisso, Perno, Distanza**
- Controlli: Avvia/Pausa, Step-by-step, Reset
- **Moltiplicatore di velocità** (0.1× – 20×)
- Display del tempo fisico in tempo reale
- Esportazione come **HTML standalone interattivo**

### △ GEO — Geometria Euclidea Interattiva
- Strumenti di costruzione euclidea classici
- **Ridisegno dinamico**: spostando un punto, le figure dipendenti si aggiornano in tempo reale
- Undo, Clear, esportazione **PNG** e **HTML standalone interattivo**

---

## 📖 Documentazione

| Risorsa | Link |
|---------|------|
| **Manuale completo (HTML)** | [SciCanvas-manual.html](https://htmlpreview.github.io/?https://github.com/YouDevIt/SciCanvas/blob/main/SciCanvas-manual.html) |
| **Manuale completo (Markdown)** | [MANUAL.md](MANUAL.md) |

---

## 🛠 Tecnologie

| Libreria | Utilizzo |
|----------|----------|
| [MathJax 3](https://www.mathjax.org/) | Rendering formule LaTeX su canvas |
| [Algebrite](https://algebrite.org/) | Calcolo simbolico CAS nel REPL |
| [lil-gui](https://lil-gui.georgealways.com/) | Pannelli UI parametrici |

---

## 📄 Licenza

```
Copyright © 2026 Leonardo Boselli

Prompt Designer : Leonardo Boselli
Programmer      : Claude Sonnet 4.6 <https://claude.ai/>
```

SciCanvas è distribuito sotto licenza **GNU General Public License v3.0**.
Consulta il file [LICENSE](https://www.gnu.org/licenses/gpl-3.0.html) o visita gnu.org per i dettagli.
