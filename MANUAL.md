# SciCanvas — Manuale Completo

> **LIVE SCIENTIFIC GRAPHICS ENGINE**
> Copyright © 2026 Leonardo Boselli — Licenza GNU GPL v3.0

---

## Indice

1. [Presentazione](#1-presentazione)
2. [Interfaccia Generale](#2-interfaccia-generale)
3. [Modulo CODICE](#3-modulo-codice)
   - 3.1 [Editor](#31-editor)
   - 3.2 [REPL Interattivo](#32-repl-interattivo)
   - 3.3 [Libreria Esempi](#33-libreria-esempi)
   - 3.4 [Riferimento API](#34-riferimento-api)
   - 3.5 [Gestione Script](#35-gestione-script)
4. [Modulo SIM — Simulatore Fisico 2D](#4-modulo-sim--simulatore-fisico-2d)
   - 4.1 [Corpi Fisici](#41-corpi-fisici)
   - 4.2 [Vincoli e Superfici](#42-vincoli-e-superfici)
   - 4.3 [Forze Globali](#43-forze-globali)
   - 4.4 [Integratore Numerico](#44-integratore-numerico)
   - 4.5 [CustomDraw](#45-customdraw)
   - 4.6 [Esempi Precaricati](#46-esempi-precaricati)
   - 4.7 [Salvataggio e Caricamento Scene](#47-salvataggio-e-caricamento-scene)
   - 4.8 [Controlli di Simulazione](#48-controlli-di-simulazione)
5. [Modulo GEO — Geometria Euclidea Interattiva](#5-modulo-geo--geometria-euclidea-interattiva)
   - 5.1 [Strumenti di Costruzione](#51-strumenti-di-costruzione)
   - 5.2 [Vista Algebrica](#52-vista-algebrica)
   - 5.3 [Nota Costruzione](#53-nota-costruzione)
   - 5.4 [Griglia e Scie](#54-griglia-e-scie)
   - 5.5 [Pannello Proprietà](#55-pannello-proprietà)
   - 5.6 [Esempi Precaricati](#56-esempi-precaricati)
   - 5.7 [Salvataggio e Caricamento](#57-salvataggio-e-caricamento)
   - 5.8 [Navigazione del Canvas](#58-navigazione-del-canvas)
6. [Esportazione](#6-esportazione)
7. [Scorciatoie da Tastiera](#7-scorciatoie-da-tastiera)
8. [Informazioni su Licenza e Autore](#8-informazioni-su-licenza-e-autore)

---

## 1. Presentazione

**SciCanvas** è un'applicazione web monolitica (un singolo file `.html` autocontenuto) progettata per la **creazione e visualizzazione interattiva di grafica scientifica** direttamente nel browser, senza installazioni né dipendenze esterne da configurare.

L'applicazione integra tre ambienti indipendenti ma coerenti:

| Modulo | Icona | Descrizione |
|--------|-------|-------------|
| **CODICE** | `⌨` | Editor JavaScript con API grafica Canvas, REPL interattivo, CAS simbolico, libreria esempi, documentazione API integrata e gestione script. |
| **SIM** | `⛛` | Simulatore fisico 2D con corpi rigidi, vincoli cinematici, forze globali configurabili (anche dinamiche), integratore RK4, CustomDraw e libreria di scene precaricate. |
| **GEO** | `△` | Costruttore interattivo di geometria euclidea dinamica con strumenti classici, vista algebrica, scie, slider parametrici e libreria di costruzioni. |

**Librerie esterne integrate (caricate da CDN al primo avvio):**

| Libreria | Versione | Utilizzo |
|----------|----------|----------|
| MathJax | 3 (tex-svg) | Rendering di formule LaTeX in formato SVG direttamente sul canvas |
| Algebrite | 1.4.0 | Calcolo simbolico CAS nel REPL (derivate, integrali, semplificazione, ecc.) |
| lil-gui | 0.19 | Pannelli UI parametrici per il codice utente |

---

## 2. Interfaccia Generale

### Barra di intestazione (Header)

L'intestazione è sempre visibile e si adatta dinamicamente al modulo attivo, mostrando i pulsanti pertinenti a quello in uso.

**Elementi fissi (sempre visibili):**
- **Logo `Sci·Canvas`** e tagline `LIVE SCIENTIFIC GRAPHICS ENGINE`: identificano l'applicazione.
- **Pulsante `ⓘ`**: apre la finestra modale **About** con dati su autore, ruoli e licenza.
- **Controllo scala interfaccia (`−` / `+`)**: riduce o aumenta la scala dell'intera UI in passi discreti. Il valore corrente (es. `100%`) è mostrato tra i due pulsanti. L'impostazione viene salvata nel `localStorage` del browser e ripristinata automaticamente all'avvio.

**Pulsanti del modulo CODICE:**

| Pulsante | Funzione |
|----------|----------|
| `▶ ESEGUI` | Esegue il codice nell'editor (scorciatoia: `Ctrl+↵`) |
| `■ STOP` | Ferma il loop di animazione corrente |
| `⊘ CLEAR` | Cancella il contenuto del canvas |
| `↓ PNG` | Esporta il canvas come immagine PNG |
| `⬡ HTML` | Esporta come HTML standalone (senza editor) |

**Pulsanti del modulo SIM:**

| Pulsante / Controllo | Funzione |
|----------------------|----------|
| `▶ AVVIA` / `⏸ PAUSA` | Avvia o sospende la simulazione |
| `⏭` | Avanza di un singolo passo temporale `dt` |
| `⏮` | Ripristina lo stato iniziale e azzera il tempo |
| `⊘ Clear` | Rimuove tutti i corpi e vincoli dalla scena |
| Display `t = 0.000 s` | Tempo fisico simulato corrente, aggiornato in real-time |
| Campo `vel×` | Moltiplicatore velocità di simulazione (0.1× – 20×) |
| `⬡ HTML` | Esporta la simulazione come HTML standalone interattivo |

**Pulsanti del modulo GEO:**

| Pulsante | Funzione |
|----------|----------|
| `↖ Seleziona` | Attiva la modalità selezione e trascinamento punti |
| `↩ Undo` | Annulla l'ultima operazione di costruzione |
| `⊘ Clear` | Cancella tutta la costruzione |
| `↓ PNG` | Esporta la costruzione come immagine PNG |
| `⬡ HTML` | Esporta come HTML standalone interattivo e dinamico |

### Layout principale

L'interfaccia è divisa in due pannelli affiancati:
- **Pannello sinistro**: editor di codice, REPL, palette strumenti, controlli del modulo attivo.
- **Pannello destro**: canvas HTML5 per rendering grafico, fisico o geometrico.

### Barra di stato del canvas

Nella parte inferiore del canvas sono presenti indicatori contestuali aggiornati in tempo reale:
- **Coordinate del cursore** (es. `x: 2.345, y: -1.200`): posizione del mouse nelle unità di sistema del canvas.
- **FPS e frame counter**: visibili durante le animazioni, utili per valutare le prestazioni.
- **Pulsante `⌖ RESET`**: riporta la vista al punto di origine e allo zoom di default.
- **Indicatori di stato**: `◉ PRONTO`, `○ INATTIVO` con messaggi contestuali per il modulo attivo.
- **Suggerimento navigazione**: `🖱 TRASCINA: pan · ROTELLA: zoom · Ctrl+↵: esegui`.

---

## 3. Modulo CODICE

Il modulo CODICE è il cuore creativo di SciCanvas: permette di scrivere ed eseguire codice JavaScript che disegna sul canvas tramite un'API grafica di alto livello. È organizzato in cinque sotto-schede.

### 3.1 Editor

La sotto-scheda **📝 editor** è l'ambiente principale di scrittura del codice.

**Caratteristiche:**

- **Syntax highlighting in tempo reale**: parole chiave JavaScript, numeri, stringhe, commenti e operatori vengono colorati tramite un layer sovrapposto (`syntaxHl`) all'area di testo nativa. Il coloring avviene a ogni keystroke senza latenze percettibili.
- **Numerazione delle righe**: la colonna `lnums` a sinistra mostra i numeri di riga; lo scroll è sincronizzato con il testo in ogni momento.
- **Indentazione con Tab**: il tasto `Tab` inserisce spazi di indentazione standard senza perdere il focus sull'editor.
- **Drag & drop file**: è possibile trascinare un file `.js` o `.txt` direttamente sull'editor per caricarne il contenuto. Durante il trascinamento appare un overlay animato con il messaggio `⬇ Rilascia il file .js / .txt`. Al rilascio, il contenuto del file sostituisce il codice corrente.
- **Esecuzione**: il codice viene eseguito premendo `▶ ESEGUI` o con la scorciatoia `Ctrl+↵`.
- **Stop animazione**: `■ STOP` interrompe il loop `requestAnimationFrame` in corso impostando la variabile interna `animRun = false`.
- **Clear canvas**: `⊘ CLEAR` cancella il contenuto del canvas senza fermare eventuali animazioni in corso.
- **Ridisegno automatico al resize**: se non è attivo un loop di animazione, al ridimensionamento della finestra del browser viene chiamata automaticamente la funzione `redrawStatic()`, ridisegnando il grafico statico con le nuove dimensioni del canvas.

### 3.2 REPL Interattivo

La sotto-scheda **⚡ repl** offre un *Read-Eval-Print Loop* per eseguire espressioni singole in modo immediato, senza dover riscrivere o rieseguire l'intero script dell'editor.

**Due modalità operative:**

| Modalità | Pulsante | Descrizione |
|----------|----------|-------------|
| **JS** | `JS` | Valuta espressioni JavaScript nell'ambiente del canvas. Permette di testare funzioni API, fare calcoli rapidi, ispezionare variabili o invocare comandi grafici istantanei. |
| **CAS** | `CAS` | Valuta espressioni di **algebra simbolica** tramite la libreria Algebrite: derivate, integrali, semplificazione algebrica, fattorizzazione, espansione, risoluzione simbolica di equazioni, ecc. |

**Namespace persistente `_`:**

Le variabili possono essere assegnate tramite `_.nomeVariabile = valore` e rimangono disponibili tra una valutazione REPL e l'altra nella stessa sessione. La barra `replNsBar` in fondo all'area output mostra in tempo reale il contenuto corrente dell'oggetto `_`.

**Funzionalità UI del REPL:**
- **Area di output** (`replOut`): mostra i risultati con formattazione visiva distinta per valori numerici, errori, output CAS e messaggi.
- **Prompt `›`**: precede il campo di input per evidenziare la riga di inserimento.
- **Cronologia comandi**: `↑` / `↓` navigano la cronologia dei comandi della sessione corrente.
- **`⊘ Clear`**: pulisce l'area di output senza azzerare il namespace `_`.
- **`? Aiuto`**: mostra una guida sintetica ai comandi disponibili nel REPL.
- **Invio**: tasto `↵` o pulsante `↵` sulla destra del campo invia l'espressione per la valutazione.

**Esempi in modalità CAS:**

```
› derivative(sin(x), x)
  cos(x)

› simplify((x^2 - 1) / (x - 1))
  x + 1

› integral(x^2, x)
  x^3 / 3

› factor(x^3 - x^2 - x + 1)
  (x - 1)^2 * (x + 1)

› expand((a + b)^3)
  a^3 + 3*a^2*b + 3*a*b^2 + b^3
```

**Esempi in modalità JS (con API grafica):**

```js
› drawAxis()
› plotFunction(x => Math.sin(x))
› _.f = x => x*x - 2*x + 1
› plotFunction(_.f, -2, 4)
```

### 3.3 Libreria Esempi

La sotto-scheda **📚 esempi** contiene una raccolta di script dimostrativi pre-caricati, organizzati per categoria. Cliccando su un esempio, il suo codice viene caricato nell'editor ed è immediatamente eseguibile o modificabile.

Gli esempi coprono tipicamente:
- Grafici di funzioni matematiche (trigonometriche, esponenziali, logaritmiche, polinomiali).
- Curve parametriche, spirali di Archimede, curve di Lissajous, ipotroclodi.
- Animazioni in loop con `requestAnimationFrame`.
- Uso di MathJax per inserire formule LaTeX come etichette tipograficamente corrette sul canvas.
- Visualizzazioni di concetti fisici: campi vettoriali, traiettorie, distribuzione di forze.
- Grafici di dati, istogrammi, scatter plot, bar chart.
- Utilizzo di lil-gui per creare pannelli di controllo parametrici interattivi.

### 3.4 Riferimento API

La sotto-scheda **📖 api** contiene la documentazione completa dell'API JavaScript disponibile nell'editor. Tutte le funzioni sono disponibili nell'ambiente di esecuzione senza importazioni. Cliccando su una voce dell'API, la firma viene inserita automaticamente nel punto di inserimento corrente dell'editor (o nel campo del REPL se era l'ultima scheda attiva).

L'API è organizzata in **13 categorie**:

---

#### 🎨 Stato Grafico

| Funzione | Descrizione |
|----------|-------------|
| `bg(color)` | Imposta il colore di sfondo del canvas |
| `clear()` | Pulisce il canvas (trasparente) |
| `fill(color)` / `noFill()` | Imposta il colore di riempimento / disabilita riempimento |
| `stroke(color)` / `noStroke()` | Imposta il colore del contorno / disabilita contorno |
| `lineWidth(w)` | Spessore della linea di contorno |
| `alpha(a)` | Opacità globale (0–1) |
| `lineDash([n,n])` / `noDash()` | Abilita/disabilita tratteggio con pattern `[dash, gap]` |
| `push()` / `pop()` | Salva / ripristina lo stato grafico corrente |

---

#### 📐 Primitive

| Funzione | Descrizione |
|----------|-------------|
| `circle(x, y, r)` | Cerchio con centro e raggio |
| `rect(x, y, w, h, r?)` | Rettangolo; `r` opzionale = raggi angoli arrotondati |
| `ellipse(x, y, rx, ry)` | Ellisse con semiassi `rx`, `ry` |
| `line(x1, y1, x2, y2)` | Segmento tra due punti |
| `arrow(x1,y1,x2,y2,hs?,col?)` | Freccia con punta; `hs` = dimensione testa, `col` = colore |
| `polygon([[x,y]…])` | Poligono chiuso da array di vertici |
| `arc(x,y,r,a1,a2)` | Arco di cerchio da angolo `a1` a `a2` (rad) |
| `bezier(...)` | Curva di Bézier cubica o quadratica |
| `point(x,y,r?,col?)` | Punto (piccolo cerchio); `r` e `col` opzionali |

---

#### ✏️ Testo

| Funzione | Descrizione |
|----------|-------------|
| `text(str, x, y, opt?)` | Testo libero con opzioni di stile |
| `label(str,x,y,size?,col?)` | Etichetta centrata sul punto dato |
| `mathText(str,x,y,size?)` | Testo con simboli matematici (Unicode) |
| `font(size,family,style?,w?)` | Imposta font: dimensione, famiglia, stile (italic), peso |
| `textAlign(l\|c\|r)` | Allineamento orizzontale: left / center / right |
| `textBase(top\|mid\|bot)` | Baseline verticale: top / middle / bottom |

---

#### 📊 Coordinate & Assi

| Funzione | Descrizione |
|----------|-------------|
| `setCoords(xmin,xmax,ymin,ymax)` | Definisce il sistema di coordinate matematico (4 argomenti) |
| `setCoords(xmin,xmax,aspect)` | Come sopra ma con aspect ratio; `aspect=1` → unità quadrate (3 argomenti) |
| `resetCoords()` | Torna al sistema pixel nativo del canvas |
| `axes(opt)` | Disegna assi cartesiani con frecce e tick; opzioni: colore, passo, label |
| `grid(stepX,stepY,color)` | Disegna griglia di sfondo con passo e colore configurabili |

---

#### 📈 Grafici

| Funzione | Descrizione |
|----------|-------------|
| `plot(fn, opt)` | Traccia `y = f(x)`; opzioni: range, colore, spessore, n. campioni |
| `plotFill(fn,base,opt)` | Area sotto la curva (riempita fino a `base`) |
| `parametric(xfn,yfn,opt)` | Curva parametrica `(x(t), y(t))`; opzioni: range t, campioni |
| `scatter(pts, opt)` | Scatter plot da array `[[x,y],…]`; opzioni: colore, simbolo, size |
| `barChart(data, opt)` | Istogramma a barre da array di valori o `{label,value}` |
| `gaussian(mu,sigma,opt)` | Distribuzione gaussiana normalizzata `N(μ, σ)` |

---

#### 🔢 Campi & Onde

| Funzione | Descrizione |
|----------|-------------|
| `vectorField(fx,fy,opt)` | Campo vettoriale su griglia; `fx(x,y)`, `fy(x,y)` = componenti |
| `streamlines(fx,fy,opt)` | Linee di flusso del campo vettoriale |
| `contour(fn, opt)` | Heatmap e/o curve di livello di `f(x,y)` |
| `electricField(charges,opt)` | Campo elettrico generato da array di cariche `{x,y,q}` |
| `equipotential(charges,opt)` | Superfici equipotenziali del campo elettrico |
| `magneticField(wires,opt)` | Campo magnetico generato da fili `{x,y,I}` |
| `wave(opt)` | Onda trasversale animabile; opzioni: ampiezza, lunghezza d'onda, fase |

---

#### ⚙️ Oggetti Fisici

Funzioni di disegno per oggetti fisici stilizzati, utili per illustrare problemi di meccanica:

| Funzione | Descrizione |
|----------|-------------|
| `spring(x1,y1,x2,y2,opt)` | Molla stilizzata tra due punti |
| `pendulum(x0,y0,L,ang,opt)` | Pendolo: fulcro `(x0,y0)`, lunghezza `L`, angolo `ang` |
| `mass(x,y,w,h,opt)` | Blocco massa rettangolare con etichetta opzionale |
| `wall(x,y,h,opt)` | Parete (ostacolo fisico) tratteggiata |
| `charge(x,y,q,opt)` | Rappresentazione stilizzata di una carica elettrica `q` |
| `particle(x,y,vx,vy,opt)` | Particella con vettore velocità e traccia della traiettoria |

---

#### 📐 LaTeX sul Canvas

| Funzione | Descrizione |
|----------|-------------|
| `latex(formula, x, y, opt)` | Renderizza una formula LaTeX inline nella posizione `(x,y)` |
| `latexBlock(formula, x, y, opt)` | Formula display (grande, centrata) nella posizione `(x,y)` |
| `clearLatexCache()` | Svuota la cache interna delle formule renderizzate |

**Opzioni disponibili (`opt`):**

| Opzione | Descrizione |
|---------|-------------|
| `opt.color` | Colore del testo (default: fill corrente) |
| `opt.size` | Fattore di scala (1 = default, 2 = doppio) |
| `opt.align` | Ancoraggio orizzontale: `left` / `center` / `right` |
| `opt.vAlign` | Ancoraggio verticale: `top` / `middle` / `bottom` |

```js
// Esempio
latex('E = mc^2', 0, 2, { size: 1.5, align: 'center', color: '#00d4ff' })
latexBlock('\\int_0^\\infty e^{-x^2}\\,dx = \\frac{\\sqrt{\\pi}}{2}', 0, 0)
```

---

#### 📐 Formulario Fisico

Accesso a un database di formule fisiche con risoluzione automatica per la variabile incognita:

| Funzione | Descrizione |
|----------|-------------|
| `formula(key, given)` | Risolve la formula `key` dati i valori noti in `given`; restituisce un `Qty` con `.formulaSym`, `.formulaLatex`, `.cat`, `.solvedFor` |
| `formulaInfo(key)` | Metadati della formula: nome, simbolo, LaTeX, variabili, quali si possono risolvere |
| `formulaList(cat?)` | Elenco di tutte le chiavi disponibili, filtrabili per categoria |
| `formulaCategories()` | Lista delle categorie di formule disponibili |
| `formulaSolve(key, var)` | Soluzione simbolica della formula via Algebrite |

```js
// Risolve la 2ª legge di Newton per F, dati m e a
const res = formula('newton2', { m: 5, a: 9.8 })
// res.value → 49 (N)
latex(res.formulaLatex, 0, 1)  // disegna la formula usata

// Gas ideale: risolve per T dati P, V, n
formula('ideal_gas', { P: 1e5, V: 0.01, n: 0.5 })
```

**Proprietà del risultato `res`:**

| Proprietà | Descrizione |
|-----------|-------------|
| `res.formulaSym` | Espressione simbolica della formula usata |
| `res.formulaLatex` | Stringa LaTeX della formula (per `latex()`) |
| `res.cat` | Categoria fisica della formula |
| `res.solvedFor` | Nome della variabile calcolata |

---

#### ⚛️ Costanti Fisiche & Unità di Misura

Sistema di grandezze fisiche con **propagazione automatica dell'incertezza** (CODATA):

| Funzione | Descrizione |
|----------|-------------|
| `physConst('c')` | Costante fisica per chiave → `Qty` con incertezza CODATA (es. `'c'` = velocità della luce) |
| `physConst('Fe')` | Elemento chimico → oggetto con `.z` (numero atomico) e proprietà fisiche |
| `astroBody('Mars')` | Corpo celeste → oggetto con `M` (massa), `R` (raggio), `g`, `T` (periodo), ecc. |
| `val(v, unit, unc?)` | Crea una grandezza `Qty` con valore, unità e incertezza opzionale |
| `val('3.5 ± 0.01 m')` | Parsa una stringa con valore, incertezza e unità |
| `convert(v, 'from', 'to')` | Converte rapidamente un numero tra unità → restituisce numero |
| `listConst(filtro?)` | Cerca chiavi nel database delle costanti |

**Metodi dell'oggetto `Qty`:**

| Metodo | Descrizione |
|--------|-------------|
| `q.to('km')` | Converte in un'altra unità → nuovo `Qty` |
| `q.mul(r)` / `q.div(r)` | Moltiplicazione / divisione con propagazione dell'errore |
| `q.add(r)` / `q.sub(r)` | Addizione / sottrazione (stesse dimensioni) |
| `q.pow(n)` / `q.sqrt()` | Potenza e radice con propagazione dell'errore |
| `q.inSI()` | Converte nelle unità SI base |
| `q.toString()` | Stringa `valore ± errore unità` |
| `q.toStr(sf?, unit?)` | Stringa con controllo delle cifre significative |
| `q.toLaTeX(sf?, unit?)` | Stringa LaTeX per uso con `latex()` |
| `q.format(dec, unit?)` | Notazione fissa con numero fisso di decimali |
| `q.uncertainty` | Incertezza assoluta 1σ in unità SI |
| `qmul(a,b)` / `qdiv(a,b)` | Approccio funzionale alternativo |
| `qadd(a,b)` / `qsub(a,b)` | Approccio funzionale alternativo |
| `qpow(q,n)` / `qsqrt(q)` | Approccio funzionale alternativo |

```js
// Forza con propagazione errore
const m = val('10.0 ± 0.1 kg')
const a = val('9.81 ± 0.02 m/s^2')
const F = m.mul(a)
text(F.toStr(3), 0, 0)   // es: "98.1 ± 1.2 N"
latex(F.toLaTeX(3), 0, -1)

// Costante fisica
const c = physConst('c')  // velocità della luce
text(c.toString(), 0, 2)  // "299792458 ± 0 m/s"
```

---

#### 🎛️ Interfaccia Utente — lil-gui

Crea pannelli di controllo interattivi sovrapposti al canvas:

| Funzione | Descrizione |
|----------|-------------|
| `createUI(opt?)` | Crea un pannello UI; opzioni: `title`, `width`, `top`, `right`, `left` |
| `ui.slider(lbl,init,min,max,stp?)` | Slider numerico → leggi con `.value` |
| `ui.number(lbl,init,min?,max?)` | Campo numerico editabile → `.value` |
| `ui.checkbox(lbl,init)` | Checkbox booleana → `.value` (bool) |
| `ui.color(lbl,init)` | Color picker → `.value` (stringa hex `#rrggbb`) |
| `ui.text(lbl,init)` | Campo testo libero → `.value` |
| `ui.select(lbl,init,opts[])` | Menu a tendina → `.value` |
| `ui.button(lbl,fn)` | Pulsante con callback `fn()` |
| `ui.label(lbl,text)` | Etichetta di sola lettura |
| `ui.folder(name)` | Sottogruppo collassabile → stessa API di `ui` |
| `ui.separator()` | Linea divisoria visiva |
| `ui.open()` / `ui.close()` | Espande / comprime il pannello |
| `ctrl.value = v` | Aggiorna il valore di un controllo da codice |

```js
const ui = createUI({ title: 'Parametri', width: 220 })
const freq = ui.slider('Frequenza', 1, 0.1, 10, 0.1)
const col  = ui.color('Colore', '#00d4ff')

animate(t => {
  bg('#000')
  stroke(col.value)
  plot(x => Math.sin(freq.value * x))
})
```

---

#### 🧮 CAS — Algebrite (dall'editor)

Accesso al motore di calcolo simbolico direttamente dal codice dell'editor:

| Funzione | Descrizione |
|----------|-------------|
| `calc(expr)` | Esegue un'espressione CAS → `{ text, latex }` |
| `calc('d(x^2,x)')` | Derivata → `"2*x"` |
| `calc('integral(x^2,x)')` | Integrale indefinito |
| `calc('factor(x^3-1)')` | Fattorizzazione |
| `calc('expand((x+1)^4)')` | Espansione polinomiale |
| `calc('simplify(sin(x)^2+cos(x)^2)')` | Semplificazione |
| `calc('lim(sin(x)/x,x,0)')` | Limite per `x → 0` |
| `calc('roots(x^2-5*x+6,x)')` | Radici/zeri del polinomio |
| `calc('nroots(x^3-2,x)')` | Radici numeriche |
| `calc('solve(x^2=4,x)')` | Risolve un'equazione |
| `calc('subst(x=2, x^3+1)')` | Sostituisce un valore nell'espressione |
| `calc('taylor(sin(x),x,0,5)')` | Sviluppo di Taylor all'ordine 5 |
| `calc('eigenvalues([[1,2],[3,4]])')` | Autovalori di una matrice |
| `calc('transpose([[1,2],[3,4]])')` | Trasposta di una matrice |
| `calc('det([[a,b],[c,d]])')` | Determinante simbolico |
| `calc('gcd(12,8)')` | Massimo Comun Divisore |
| `calc('lcm(4,6)')` | Minimo Comune Multiplo |
| `calc('isprime(17)')` | Test di primalità |
| `calc('sum(k,k,1,10)')` | Sommatoria Σk per k=1..10 |
| `calc('printlatex(expr)')` | Converte l'espressione in stringa LaTeX |

**Proprietà del risultato `res`:**

| Proprietà | Descrizione |
|-----------|-------------|
| `res.text` | Risultato come stringa testuale |
| `res.latex` | Risultato in LaTeX (per `latex()`) |

```js
// Calcola la derivata e la disegna sul canvas
const der = calc('d(sin(x)*exp(-x/3), x)')
latex(der.latex, 0, 2, { size: 1.4, align: 'center' })

// Sviluppo di Taylor e tracciamento
const t5 = calc('taylor(sin(x),x,0,5)')
text('Taylor: ' + t5.text, 0, -2)
```

---

#### 🎬 Animazione & Input

| Funzione | Descrizione |
|----------|-------------|
| `animate(fn(t,dt,frame))` | Avvia il loop di animazione; `fn` riceve tempo `t` (s), delta `dt` (s) e numero frame |
| `stop()` | Ferma il loop di animazione |
| `onMouseMove(fn(x,y))` | Callback al movimento del mouse (coordinate del sistema corrente) |
| `onClick(fn(x,y,btn))` | Callback al click del mouse; `btn` = 0/1/2 |
| `onKeyDown(fn(key))` | Callback alla pressione di un tasto |
| `getMouse()` | Restituisce lo stato corrente del mouse `{x, y, down}` |
| `getKeys()` | Restituisce l'insieme dei tasti attualmente premuti |

```js
animate((t, dt) => {
  bg('#111')
  const m = getMouse()
  fill('#00d4ff')
  circle(m.x, m.y, 0.3)   // cerchio segue il cursore
})
```

---

#### 🧮 Matematica & Utilità

| Funzione / Costante | Descrizione |
|---------------------|-------------|
| `W()` / `H()` | Larghezza / altezza del canvas in pixel |
| `PI`, `TAU`, `E` | Costanti matematiche (π, 2π, e) |
| `sin`, `cos`, `tan`, `atan2`, … | Funzioni trigonometriche standard |
| `sqrt`, `pow`, `exp`, `log`, … | Funzioni matematiche standard |
| `random(a,b)` | Numero casuale uniforme nell'intervallo `[a,b]` |
| `map(v,a,b,c,d)` | Rimappa `v` dall'intervallo `[a,b]` a `[c,d]` |
| `lerp(a,b,t)` | Interpolazione lineare: `a + (b-a)*t` |
| `clamp(v,min,max)` | Limita `v` all'intervallo `[min,max]` |
| `getTime()` | Tempo corrente in secondi dall'avvio |

### 3.5 Gestione Script

La sotto-scheda **💾 script** è un gestore di script personali con salvataggio persistente nel `localStorage` del browser.

| Azione | Descrizione |
|--------|-------------|
| **💾 Salva** | Apre una modale in cui si assegna un nome allo script corrente (max 80 caratteri). Lo script viene salvato nel `localStorage`. Se esiste già uno script con lo stesso nome, viene sovrascritto. |
| **⬇ Download** | Scarica lo script selezionato come file `.js` / `.txt` sul computer. |
| **⬆ Upload** | Apre un selettore file per caricare uno script da file locale (`.js`, `.txt`, `.sc`). Il contenuto viene caricato nell'editor. |

Gli script salvati appaiono come voci cliccabili nella lista: cliccando su uno di essi il codice viene caricato nell'editor. Quando la lista è vuota, viene mostrato il messaggio `Nessuno script salvato. Premi 💾 Salva per iniziare.`

> **Nota**: gli script sono memorizzati nel `localStorage` del browser. Cancellare la cache ne causerebbe la perdita. Usare **Download** per mantenere copie permanenti dei lavori importanti.

---

## 4. Modulo SIM — Simulatore Fisico 2D

Il modulo **⛛ SIM** è un simulatore fisico bidimensionale completo che permette di costruire scene con corpi rigidi, vincoli meccanici e forze globali, e di osservarne l'evoluzione dinamica tramite integrazione numerica RK4.

### 4.1 Corpi Fisici

I corpi vengono aggiunti selezionando lo strumento dalla **palette Corpi** e cliccando sul canvas. Ogni corpo ha proprietà fisiche configurabili nel **Pannello Proprietà** laterale, visibile non appena il corpo è selezionato.

| Strumento | Icona | Descrizione fisica |
|-----------|-------|--------------------|
| **Selezione** | `↖` | Modalità selezione. Clicca su un corpo per selezionarlo e visualizzarne le proprietà. Trascinalo per riposizionarlo. Non aggiunge nuovi oggetti. |
| **Particella** | `●` | Massa puntiforme (momento di inerzia nullo). Ideale per proiettili, orbite gravitazionali, moto browniano, particelle in campo elettromagnetico. |
| **Blocco** | `⮬` | Corpo rigido rettangolare con massa distribuita uniformemente. Risponde a forze, coppie e collisioni con rotazione. |
| **Sfera** | `◉` | Corpo rigido circolare con inerzia di sfera piena (`I = 2/5 mr²`). Supporta collisioni elastiche e anelastiche. |
| **Cilindro** | `⬤` | Corpo circolare con inerzia di cilindro pieno (`I = 1/2 mr²`). Utile per simulare rulli, dischi rotolanti, ingranaggi. |
| **Asta** | `━` | Corpo allungato rigido con inerzia di asta (`I = 1/12 mL²`). Fondamentale per pendoli, leve, bracci articolati e meccanismi. |
| **Anello** | `○` | Corpo circolare cavo con inerzia di anello (`I = mr²`). Permette di confrontare il comportamento dinamico con sfera e cilindro a parità di massa e raggio. |

**Proprietà configurabili per ogni corpo nel pannello Properties:**
- Massa (`m`, kg)
- Posizione iniziale (`x`, `y`, in metri)
- Orientamento iniziale (angolo in radianti o gradi)
- Velocità lineare iniziale (`vx`, `vy`, m/s)
- Velocità angolare iniziale (rad/s)
- Coefficiente di restituzione (0 = perfettamente anelastico, 1 = perfettamente elastico)
- Coefficiente di attrito
- Colore e stile visivo

### 4.2 Vincoli e Superfici

I vincoli definiscono relazioni cinematiche tra corpi o tra corpo e ambiente. Si aggiungono selezionando lo strumento dalla **palette Vincoli** e cliccando su corpi o punti del canvas.

| Strumento | Icona | Descrizione |
|-----------|-------|-------------|
| **Punto fisso** | `📌` | Ancora un punto di un corpo a una posizione fissa dello spazio. Impedisce la traslazione in quel punto ma permette la rotazione attorno ad esso. Fondamentale per pendoli, ruote, strutture ancorate. |
| **Perno** | `🔘` | Collega un punto di un corpo a un punto di un altro con un perno rotante. Trasmette forze tra i due corpi ma permette la rotazione reciproca libera (cerniera cinematica). |
| **Distanza** | `↔` | Mantiene costante la distanza tra due punti (uno su ciascun corpo, o uno fisso). Si comporta come un'asta rigida inestensibile o come un cavo teso. |
| **Cerniera** | `🔗` | Variante del perno con possibilità di definire un angolo di riposo e una rigidità torsionale (molla angolare). Utile per snodi elastici e giunti con rigidezza controllata. |
| **Guida H** | `⟶` | Vincola il corpo a scorrere solo lungo una guida orizzontale (solo traslazione in X). Simula binari, pistoni orizzontali, carrelli. |
| **Guida V** | `⟵` | Come Guida H ma in direzione verticale (solo traslazione in Y). |
| **Guida Ang** | `⟳` | Guida orientata a un angolo arbitrario: il corpo scorre solo lungo la direzione specificata. |
| **Piano** | `⬜` | Aggiunge un piano di contatto orizzontale (pavimento/soffitto) con cui i corpi interagiscono fisicamente tramite collisioni e attrito. |
| **Inclinato** | `╱` | Piano inclinato di un angolo configurabile. Permette di simulare rampe, scivoli, superfici oblique. |
| **Fune** | `〜` | Vincolo di lunghezza massima (fune inestensibile): opera solo in trazione, è passivo quando allentato e si tende quando i punti raggiungerebbero il limite di distanza. |

### 4.3 Forze Globali

La sezione **Forze Globali** permette di configurare campi di forza che agiscono su **tutti i corpi** della scena simultaneamente. Ogni forza si attiva/disattiva con una checkbox. Il valore può essere una **costante numerica** o un'**espressione dinamica** che dipende dalle variabili `x`, `y` (posizione del corpo in metri) e `t` (tempo simulato in secondi).

| Campo | Simbolo | Unità | Descrizione |
|-------|---------|-------|-------------|
| **Gravità** | `g` | m/s² | Accelerazione gravitazionale verso il basso. Default: `9.81`. Può essere un'espressione come `sin(t)` per simulare gravità oscillante. |
| **Campo elettrico** | `Ex`, `Ey` | N/C | Campo elettrico nelle due componenti. Agisce sulle particelle cariche. Supporta espressioni spaziali (es. `sin(y)`, `cos(x)`). |
| **Campo magnetico** | `Bz` | T | Campo magnetico perpendicolare al piano (asse Z). Genera la forza di Lorentz sulle particelle in moto. Può dipendere da `t` (es. `2*sin(t)`). |
| **Attrito viscoso** | `b` | N·s/m | Forza proporzionale e opposta alla velocità (smorzamento lineare). Simula resistenza dell'aria o un fluido viscoso. Può dipendere da `x`, `y`, `t`. |

**Variabili disponibili nelle espressioni:**

| Variabile | Significato |
|-----------|-------------|
| `x`, `y` | Posizione attuale del corpo (m) |
| `t` | Tempo fisico simulato corrente (s) |

**Funzioni matematiche supportate nelle espressioni:** `sin`, `cos`, `tan`, `sqrt`, `exp`, `log`, `abs`, `pow`, `PI`.

**Esempi di espressioni avanzate:**
```
g  = 9.81 + 2*sin(t)       // gravità oscillante nel tempo
Ex = -0.5*x                 // forza elastica verso l'origine (campo armonico)
Bz = 1.5                    // campo magnetico costante
b  = 0.1*exp(-t)            // attrito che decade esponenzialmente
```

### 4.4 Integratore Numerico

La sezione **Integratore** controlla i parametri numerici dell'integrazione delle equazioni del moto.

| Parametro | Dettaglio |
|-----------|-----------|
| **`dt` — passo temporale** | Intervallo di integrazione in secondi. Range: `0.0001` s – `0.05` s, default `0.005` s. Valori più piccoli aumentano la precisione ma rallentano la simulazione; valori più grandi la accelerano ma possono introdurre instabilità numerica. |
| **Metodo: RK4** | L'integratore usa il metodo di **Runge-Kutta al 4° ordine**: un metodo esplicito ad alta accuratezza per sistemi di ODE (errore locale O(dt⁵)), molto più stabile del metodo di Eulero per sistemi fisici realistici. |
| **Mostra v** (checkbox) | Sovrappone al canvas la visualizzazione dei **vettori velocità** di ciascun corpo (frecce proporzionali a modulo e direzione). Utile per analizzare cinematica e conservazione del momento. |

### 4.5 CustomDraw

La sezione **CustomDraw** permette di sovrapporre alla simulazione codice JavaScript personalizzato di disegno, eseguito a ogni frame del loop.

| Controllo | Descrizione |
|-----------|-------------|
| **✏️ Modifica** | Apre un editor modale per scrivere la funzione di disegno custom. Il codice ha accesso al contesto canvas e allo stato corrente della simulazione (posizioni, velocità dei corpi, tempo `t`). |
| **✕ Rimuovi** | Elimina il codice CustomDraw corrente. |

**Utilizzo tipico:** aggiungere grafici di grandezze fisiche in tempo reale (es. energia cinetica vs. tempo), annotazioni testuali, tracce di traiettoria, visualizzazione di campi, legende e misure sovrapposte alla scena simulata.

### 4.6 Esempi Precaricati

La libreria SIM include scene di esempio organizzate per tema fisico, accessibili dal pannello laterale.

**Meccanica classica:**
- ⬤ Pendolo semplice
- ━ Pendolo con asta rigida
- ◉ Moto del proiettile
- ╱ Sfera su piano inclinato

**Oscillazioni e Caos:**
- 〜 Oscillatore massa-molla
- 🔗 Doppio pendolo caotico
- ◉ Attrito volvente
- 🔔 Pendoli accoppiati
- 🌀 Attrattore di Lorenz

**Elettromagnetismo:**
- ⚡ Particella in campo magnetico B

**Collisioni e Gas:**
- Collisioni elastiche 2D (urto tra sfere)
- Gas ideale (particelle in una scatola)

**Gravitazione e Onde:**
- Problema dei tre corpi (gravitazione)
- Corda vibrante

### 4.7 Salvataggio e Caricamento Scene

Le scene SIM possono essere salvate, scaricate e ricaricate tramite la sezione **Simulazioni Salvate** nel pannello laterale.

| Azione | Descrizione |
|--------|-------------|
| **💾 Salva scena** | Salva la scena corrente nel `localStorage` con un nome utente. Tutti i corpi, vincoli, forze e parametri vengono memorizzati. |
| **⬇ Download JSON** | Esporta la scena come file `.json` sul computer, contenente la descrizione completa di tutti gli oggetti e parametri. |
| **⬆ Carica JSON** | Importa una scena da un file `.json` precedentemente scaricato, sostituendo quella corrente. |

### 4.8 Controlli di Simulazione

I controlli si trovano nella barra di intestazione quando il modulo SIM è attivo.

| Controllo | Descrizione |
|-----------|-------------|
| `▶ AVVIA` / `⏸ PAUSA` | Avvia o sospende la simulazione fisica in tempo reale. |
| `⏭` Step | Avanza esattamente di un passo `dt`. Permette l'analisi frame-by-frame dell'evoluzione del sistema. |
| `⏮` Reset | Riporta tutti i corpi allo stato iniziale (posizioni e velocità di partenza) e azzera il tempo `t`. |
| `⊘ Clear` | Rimuove tutti i corpi e i vincoli dalla scena, resettando completamente la simulazione. |
| Display `t = 0.000 s` | Tempo fisico simulato corrente in secondi, aggiornato a ogni frame. |
| Campo `vel×` | Moltiplicatore della velocità di simulazione: da `0.1×` (rallentato 10×) a `20×` (accelerato 20×), default `1×`. |

---

## 5. Modulo GEO — Geometria Euclidea Interattiva

Il modulo **△ GEO** è un ambiente per la **costruzione e l'esplorazione di figure geometriche euclidee dinamiche**. Il principio fondamentale è il **ridisegno dinamico**: spostando un punto libero con il mouse, tutte le figure che dipendono da esso si aggiornano istantaneamente, permettendo di esplorare proprietà geometriche e verificare congetture in modo visivo e interattivo.

### 5.1 Strumenti di Costruzione

Tutti gli strumenti si trovano nella **palette Strumenti** del pannello laterale. Si seleziona uno strumento e si clicca sul canvas secondo le istruzioni contestuali.

| Strumento | Icona | Descrizione |
|-----------|-------|-------------|
| **Seleziona** | `↖` | Selezione e modifica. Clicca per selezionare un oggetto, trascina i punti liberi per muovere la costruzione dinamicamente. Visualizza le proprietà dell'oggetto selezionato. |
| **Punto** | `●` | Crea un nuovo punto libero nel canvas. Le coordinate sono editabili nel pannello Proprietà e visibili nella Vista Algebrica. |
| **Retta** | `⟵` | Crea la retta passante per due punti. Si estende indefinitamente in entrambe le direzioni. |
| **Segmento** | `━` | Crea un segmento tra due punti. La lunghezza è calcolata e mostrata nella Vista Algebrica, aggiornata al trascinamento. |
| **Cerchio (C,r)** | `○` | Crea un cerchio dato il **centro** (primo click) e un **punto sul perimetro** (secondo click). Il raggio è modificabile trascinando il punto perimetrale. |
| **Cerchio (3p)** | `◎` | Crea il **cerchio circoscritto** a tre punti: il cerchio è univocamente determinato dai tre punti selezionati in sequenza. |
| **Perpendicolare** | `⊥` | Crea la retta perpendicolare a una retta o segmento dato, passante per un punto specificato. |
| **Parallela** | `∥` | Crea la retta parallela a una retta o segmento dato, passante per un punto specificato. |
| **Punto medio** | `⊕` | Marca il punto medio di un segmento o tra due punti. Rimane agganciato dinamicamente a entrambi i punti genitori. |
| **Intersezione** | `✕` | Calcola e posiziona il punto (o i punti) di intersezione tra due oggetti (rette, segmenti, cerchi, in qualsiasi combinazione). |
| **Poligono** | `⬡` | Crea un poligono chiuso specificando i vertici in sequenza. Calcola area e perimetro nella Vista Algebrica. |
| **Slider** | `⇔` | Aggiunge uno **slider parametrico** interattivo sul canvas: un parametro numerico variabile in un intervallo definito, utilizzabile come dipendenza in altre costruzioni per animare figure o esplorare famiglie di curve parametricamente. |
| **Bisettrice** | `∠` | Costruisce la bisettrice dell'angolo definito da tre punti (vertice al centro) o di due rette secanti. |
| **Punto su oggetto** | `⊂` | Crea un punto vincolato a scorrere su un oggetto esistente (retta, segmento, cerchio): può essere trascinato liberamente ma rimane sempre sull'oggetto padre. |
| **Angolo** | `∠` | Misura e visualizza l'angolo formato da tre punti (il secondo come vertice), in gradi o radianti, con aggiornamento dinamico. |
| **Distanza** | `↔` | Misura e visualizza la distanza tra due punti o la lunghezza di un oggetto. Il valore si aggiorna in tempo reale al trascinamento. |

**Pulsante `+ AGGIUNGI`**: permette di aggiungere oggetti mediante input diretto di espressioni o coordinate numeriche, per una costruzione analitica precisa senza click sul canvas.

**Pannello `Suggerimenti ▶`**: mostra istruzioni contestuali per lo strumento attivo, guidando l'utente passo passo nella costruzione corrente (es. "Clicca per definire il centro del cerchio").

### 5.2 Vista Algebrica

La **Vista Algebrica** (`geo-alg-list`) è un pannello testuale che mostra la rappresentazione algebrica di tutti gli oggetti presenti nella costruzione, aggiornata in tempo reale:
- **Punti**: coordinate `(x, y)`.
- **Segmenti**: lunghezza.
- **Rette**: equazione.
- **Cerchi**: centro e raggio.
- **Angoli**: valore in gradi o radianti.
- **Distanze**: valore numerico.
- **Slider**: nome del parametro e valore corrente.

La Vista Algebrica supporta anche l'**input diretto di espressioni**: è possibile digitare coordinate o definizioni analitiche per creare oggetti senza usare gli strumenti grafici.

Quando non sono presenti oggetti, viene mostrato: `Nessun oggetto. Usa gli strumenti o digita un'espressione.`

La barra in basso alla Vista Algebrica comprende tre controlli:
- **`⊘ Clear`**: cancella tutta la costruzione.
- **`⊞ Griglia`**: attiva/disattiva la griglia di riferimento.
- **`⌁ Scie`**: attiva/disattiva le scie dei punti dipendenti.

### 5.3 Nota Costruzione

Il campo **✎ Nota costruzione** è una `textarea` in cui è possibile inserire un commento o una spiegazione testuale relativa alla costruzione. Il testo viene **visualizzato direttamente sul canvas** come annotazione, ed è incluso nell'esportazione HTML standalone. Utile per documenti didattici, presentazioni o condivisione della costruzione con annotazioni esplicative.

### 5.4 Griglia e Scie

| Funzione | Icona | Descrizione |
|----------|-------|-------------|
| **Griglia** | `⊞` | Attiva/disattiva una griglia di riferimento sul canvas. Facilita la lettura delle coordinate e la costruzione di figure con misure precise. |
| **Scie** | `⌁` | Attiva/disattiva la modalità **traccia**: quando attiva, i punti dipendenti lasciano una scia grafica al passaggio durante il trascinamento di un punto libero (o durante lo spostamento di uno slider). Permette di visualizzare il **luogo geometrico** descritto da un punto al variare della costruzione — strumento potente per l'esplorazione geometrica. |

### 5.5 Pannello Proprietà

Il pannello **Proprietà** (`geo-props`) appare quando un oggetto è selezionato. Permette di visualizzare e modificare:
- **Nome/etichetta** dell'oggetto (rinominabile).
- **Colore e stile** di visualizzazione: spessore linea, tratteggio, riempimento.
- **Valori numerici**: coordinate per i punti, raggio per i cerchi, valore e intervallo per gli slider.
- **Visibilità**: mostrare/nascondere l'etichetta o l'oggetto stesso.
- **Vincoli**: informazioni sulle dipendenze (da quali oggetti dipende, quali oggetti dipendono da esso).

### 5.6 Esempi Precaricati

La libreria GEO include costruzioni classiche, organizzate per argomento. Cliccando su un esempio, la costruzione viene caricata sostituendo quella corrente.

**Triangoli e Poligoni:**
- △ Baricentro del triangolo (intersezione delle mediane)
- ⊙ Cerchio circoscritto a un triangolo (intersezione degli assi dei lati)
- △ Cerchio inscritto in un triangolo (intersezione delle bisettrici degli angoli)
- ⊿ Dimostrazione visiva del Teorema di Pitagora

**Rette e Angoli:**
- ∥ Parallele e trasversale (angoli alterni, corrispondenti, coniugati)
- ∠ Costruzione della bisettrice dell'angolo
- ⊢ Teorema di Talete (segmenti proporzionali su rette parallele)

**Cerchi:**
- ○ Retta tangente da un punto esterno a un cerchio
- ⊕ Asse radicale di due cerchi
- ⊙ Cerchio dei nove punti di un triangolo

**Costruzioni Classiche:**
- ∞ Retta di Eulero di un triangolo (ortocentro, baricentro, circocentro allineati)
- φ Sezione aurea (costruzione geometrica della proporzione φ = (1+√5)/2)
- ⇔ Slider e dipendenze (dimostrazione del parametro dinamico e del luogo geometrico)

### 5.7 Salvataggio e Caricamento

Le costruzioni GEO si salvano e si condividono dalla sezione **Costruzioni Salvate** nel pannello laterale.

| Azione | Descrizione |
|--------|-------------|
| **💾 Salva** | Salva la costruzione nel `localStorage` del browser con un nome utente. |
| **⬇ Download** | Esporta la costruzione come file scaricabile sul computer (formato JSON). |
| **⬆ Carica** | Importa una costruzione da file precedentemente scaricato. |

### 5.8 Navigazione del Canvas

| Azione | Gesto | Effetto |
|--------|-------|---------|
| **Pan** | Trascinamento su area vuota | Sposta la vista del canvas |
| **Zoom** | Rotella del mouse | Ingrandisce o rimpicciolisce intorno al cursore |
| **Reset vista** | Pulsante `⌖ RESET` nella barra di stato | Riporta a origine e zoom di default |

La barra di stato mostra in tempo reale le **coordinate del cursore** (es. `x: 3.250, y: -1.500`) nelle unità di sistema del canvas, utile per costruzioni che richiedono precisione numerica.

---

## 6. Esportazione

SciCanvas offre molteplici formati di esportazione per ogni modulo, pensati per condivisione, incorporamento e archiviazione.

### Modulo CODICE

| Formato | Pulsante | Descrizione |
|---------|----------|-------------|
| **PNG** | `↓ PNG` | Esporta il contenuto del canvas come immagine PNG ad alta fedeltà. La risoluzione in pixel corrisponde alla dimensione effettiva del canvas nell'interfaccia. |
| **HTML standalone** | `⬡ HTML` | Genera un file HTML autonomo che include il codice sorgente dell'utente e lo esegue automaticamente al caricamento nel browser, **senza** mostrare l'interfaccia dell'editor. Ideale per la condivisione di visualizzazioni, per incorporarle in pagine web o per presentazioni. |

### Modulo SIM

| Formato | Pulsante | Descrizione |
|---------|----------|-------------|
| **HTML standalone interattivo** | `⬡ HTML` | Esporta l'intera simulazione (scena con corpi, vincoli, forze e parametri) come file HTML autonomo **completamente interattivo**. Il destinatario può aprire il file nel proprio browser e avviare, mettere in pausa, resettare e interagire con la simulazione senza bisogno di SciCanvas o di alcuna installazione. |

### Modulo GEO

| Formato | Pulsante | Descrizione |
|---------|----------|-------------|
| **PNG** | `↓ PNG` | Esporta la costruzione geometrica corrente come immagine PNG statica. |
| **HTML standalone interattivo** | `⬡ HTML` | Esporta la costruzione geometrica come file HTML autonomo **dinamico**: i punti liberi rimangono trascinabili e le figure dipendenti si aggiornano in tempo reale, preservando l'interattività della geometria dinamica originale. |

---

## 7. Scorciatoie da Tastiera

| Scorciatoia | Contesto | Azione |
|-------------|----------|--------|
| `Ctrl + ↵` | Modulo CODICE (ovunque) | Esegue il codice nell'editor |
| `Tab` | Editor (area di testo) | Inserisce l'indentazione |
| `↑` / `↓` | REPL (campo di input) | Naviga la cronologia dei comandi |
| `↵` | REPL (campo di input) | Invia l'espressione per la valutazione |

---

## 8. Informazioni su Licenza e Autore

```
Copyright © 2026 Leonardo Boselli

Prompt Designer : Leonardo Boselli
Programmer      : Claude Sonnet 4.6 <https://claude.ai/>

Licenza: GNU General Public License v3.0
         https://www.gnu.org/licenses/gpl-3.0.html
```

SciCanvas è **software libero**: è possibile ridistribuirlo e/o modificarlo secondo i termini della **GNU General Public License** nella versione 3 pubblicata dalla Free Software Foundation.

Il programma viene distribuito nella speranza che sia utile, ma **senza alcuna garanzia**; senza nemmeno la garanzia implicita di commerciabilità o idoneità a uno scopo particolare. Consultare la GNU GPL v3.0 per maggiori dettagli.

---

*Documentazione generata per SciCanvas — repository: [https://github.com/YouDevIt/SciCanvas](https://github.com/YouDevIt/SciCanvas)*
