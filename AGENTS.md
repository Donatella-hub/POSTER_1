# POSTER Interattivo

## Panoramica
Poster interattivo HTML/SVG con un personaggio che "mangia" le lettere viola trascinate alla sua bocca. Il corpo si gonfia gradualmente e, dopo un numero casuale di lettere, il personaggio esplode.

## Struttura file
- `index.html` — tutto incluso: SVG del poster, CSS, JavaScript
- `POSTER.svg` — poster originale statico (sorgente)
- `AGENTS.md` — questo file

## Meccaniche principali

### Lettere trascinabili
- Sono i 15 testi viola originali dell'SVG (classe `cls-7`), ognuno wrappato in `<g class="draggable-letter">`
- Il drag usa pointer events con event delegation sull'SVG
- Coordinate convertite da schermo a SVG via `getBoundingClientRect()` + `viewBox`
- La posizione del drag è gestita con `transform="translate(dx, dy)"` sul gruppo
- Quando la lettera viene rilasciata vicino alla bocca, si anima via CSS transform (scale + translate) con transition

### Bocca del personaggio
- Posizione bersaglio: x=297, y=400 (coordinate SVG)
- Raggio di attivazione: 70px
- Quando una lettera trascinata entra nel raggio, parte l'animazione "mangiata"

### Gonfiamento del corpo
- Ad ogni lettera mangiata, `fullness += 0.05`
- Il corpo (`#corpo`) scala con `transform-origin: 296px 434px` (all'altezza del collo)
- Le gambe (`#gambe`) restano fisse, ancorate al rettangolo viola in basso
- La testa resta al suo posto (il collo è l'origine della scala)
- La pancia cresce verso il basso sovrapponendosi naturalmente alle gambe
- Transizione CSS fluida: `transition: transform 0.3s ease-out`

### Esplosione
- Il numero di lettere prima dell'esplosione è random da `[1, 3, 5, 7, 11, 13]`
- Include: flash bianco, onda d'urto, 80 particelle, 120 scintille, 15 nuvole di fumo, 20 detriti testuali, 15 frammenti irregolari
- Le parti SVG del personaggio (testa, corpo, gambe) si sbriciolano in 350 frammenti colorati che volano via con rotazione e scala decrescente
- Dopo 1.5s appare il pulsante "Ricomincia"
- Il reset ripristina: posizione lettere, fullness, overlay esplosione

## Costanti chiave
| Costante | Valore | Descrizione |
|---|---|---|
| `MOUTH_X` | 297 | Centro bocca X |
| `MOUTH_Y` | 400 | Centro bocca Y |
| `EAT_DIST` | 70 | Raggio attivazione bocca |
| `INFLATE_STEP` | 0.05 | Incremento scala corpo per lettera |
| `EXPLOSION_TRIGGERS` | [1,3,5,7,11,13] | Lettere prima dell'esplosione |

## SVG struttura
- `<g id="TESTA">` — testa del personaggio (non si gonfia)
- `<g id="gambe">` — gambe (traslano verso il basso)
- `<g id="corpo">` — corpo/torso (si scala)
- `<g class="draggable-letter">` ×15 — testi viola trascinabili
- `<rect class="cls-7" ...>` — barra viola in basso (NON è un testo, ignorata)
