# Report QA — Hotel Belfiore

## 1. Grid system

Ambito: `src/assets/style.css` (nessuna modifica a template, JSON o JS). Build rigenerata con `build.ps1`. Riferimento misurato anche in diretta su atlantis.com/dubai/atlantis-the-royal (tab del browser integrato) a 1430, 1910 e 375 px di larghezza utile.

Nota sulle misure: la larghezza utile (`clientWidth`) è quella della finestra meno la scrollbar classica di Windows (15 px). A finestra 1440 la larghezza utile è 1425, quindi i valori sotto sono a 1425 (colonna 290,25 px, contenuto 1281 px). Atlantis, misurato con la stessa scrollbar, dà colonna 291,5 px a 1430. `document.documentElement.scrollWidth === clientWidth` in tutte le prove (`innerWidth` include la scrollbar, quindi il confronto corretto è con `clientWidth`).

### Token definiti (`:root`, commentati nel file)

| Token | Desktop | ≤1100px | ≤700px | Uso |
|---|---|---|---|---|
| `--u` | 8px | 8px | 8px | unità di spaziatura: tutti i padding di sezione, i gutter e i gap di griglia sono multipli di 8 |
| `--m` | 72px | 40px | 24px | margine laterale "a filo": header, menu, immagine camere, bordo destro del carosello, testo hero/story, sezione finale |
| `--max` | 1296px | – | – | larghezza massima del contenuto (= 1440 − 2×72) |
| `--mc` | `max(--m, (100vw − --max)/2)` | = `--m` | = `--m` | margine laterale del contenuto (proof, carosello, camere, FAQ, footer). Fino a 1440px coincide con `--m`; oltre, il contenuto resta centrato |
| `--g` | 40px | 40px | 24px | gutter tra colonne (e gap del carosello) |
| `--card` | 352px | 352px | 280px | card del carosello |
| `--sec` | 72px | 56px | 48px | padding verticale di sezione (carosello, camere in basso, FAQ) |
| `--sec-s` | 56px | 48px | 48px | fasce sottili (proof, spazio attorno all'immagine camere) |
| `--row` | 56px | 56px | 56px | distanza verticale tra righe di card impilate |
| `--hh` | 96px | 96px | 72px | altezza header (invariato) |

Griglia: 4 colonne fluide `repeat(4, minmax(0,1fr))` con gap `--g` dentro `--mc`. A ≤1100px diventano 2 colonne (proof, camere, colonne del footer), a ≤700px camere 1 colonna, proof e footer restano a 2.

### Misure prima / dopo (x = bordo sinistro, in px, larghezza utile indicata)

**Desktop, finestra 1440 (larghezza utile 1425). Colonne griglia: 72 / 402,3 / 732,5 / 1062,8, larghezza 290,3**

| Elemento | Prima x | Prima larg. | Dopo x | Dopo larg. | Colonna |
|---|---|---|---|---|---|
| `.proof div` 1-4 | 72 / 402,3 / 732,5 / 1062,8 | 290,3 | invariato | 290,3 | 1 / 2 / 3 / 4 |
| `.spot__l` | 72 | 292 | 72 | 290,3 | 1 (larghezza prima fissa, +1,7 px) |
| primo `.scard` | 404 | 352 | 402,3 | 352 | 2 (prima sfasato di 1,7 px) |
| `.grid4 article` 1-4 | 72 / 402,3 / 732,5 / 1062,8 | 290,3 | invariato | 290,3 | 1 / 2 / 3 / 4 |
| `.faq__l` | 72 | 292 | 72 | 290,3 | 1 |
| `.faq__r` | 404 (destra 1264) | 860 | 402,3 (destra 1353) | 950,8 | prima nessuna linea di griglia; ora colonne 2-4 |
| `.ft__cols > div` 1-4 | 462,1 / 692,8 / 923,5 / 1154,3 | 198,7 | 72 / 402,3 / 732,5 / 1062,8 | 290,3 | prima FUORI griglia; ora 1 / 2 / 3 / 4 |
| `.stay__hero` | 72 (destra 1353) | 1281 | invariato | 1281 | a filo, margine 72 |
| `.fin__c` | 306,5 | 812 | invariato | 812 | centrato |
| header: menu / logo / Richiedi | 72 / centro 712,5 / destra 1353 | 44 / 107 / 168 | invariato | | margine 72 a sinistra e a destra |
| `.hero__c` (testo) | 180 | 1065 | invariato | 1065 | centrato |

**Tablet, finestra 1100 (larghezza utile 1085). Colonne griglia (2): 40 / 562,5, larghezza 482,5**

| Elemento | Prima x | Prima larg. | Dopo x | Dopo larg. | Colonna |
|---|---|---|---|---|---|
| `.proof div` | 40 / 562,5 | 482,5 | invariato | 482,5 | 1 / 2 |
| `.grid4 article` | 40 / 562,5 | 482,5 | invariato | 482,5 | 1 / 2 |
| `.ft__cols > div` | 40 / 299,3 / 558,5 / 817,8 (4 colonne) | 227,3 | 40 / 562,5 (2 colonne) | 482,5 | prima 4 colonne mentre proof e camere ne avevano 2 |
| `.spot__l` | 40 | 520 (con 40px extra a destra) | 40 | 520 max | testo sopra il carosello |
| primo `.scard` / `.spot__track` | 40 / 40 | 352 / 1045 | invariato | | track a filo fino al bordo destro |
| `.faq__l` / `.faq__r` | 40 / 40 (destra 900) | 1005 / 860 | 40 / 40 (destra 1045) | 1005 / 1005 | `faq__r` ora arriva al margine destro |
| `.hero__c` | 24 | 1037 | 40 | 1005 | prima ignorava il margine 40 |
| `.fin` padding laterale | 24 | | 40 | | prima ignorava il margine 40 |
| `.stay__hero` | 40 (destra 1045) | 1005 | invariato | 1005 | |
| header: menu / Richiedi | 40 / destra 1045 | | invariato | | |

Padding verticali (sopra) prima → dopo a 1100: proof 48 → 48, carosello 56 → 56, camere 56 → 48, camere-griglia (sotto) 72 → 56, FAQ 72 → 56, sezione finale 96 → 96.

**Mobile, finestra 375. Margini 24, gutter 24**

| Elemento | Prima x | Prima larg. | Dopo x | Dopo larg. | Colonna |
|---|---|---|---|---|---|
| `.proof div` | 24 / 197,5 | 153,5 | 24 / 199,5 | 151,5 | 1 / 2 (gutter 20 → 24) |
| `.spot__l` | 24 (destra 311, margine dx 64) | 287 | 24 (destra 351, margine dx 24) | 327 | margini simmetrici |
| `.scard` (280) | 24 / 328 / 632 | 280 | invariato | 280 | passo 304 |
| `.grid4 article` | 24 | 327 | invariato | 327 | 1 |
| `.faq__l` / `.faq__r` | 24 | 327 | invariato | 327 | 1 |
| `.ft__cols > div` | 24 / 203,5 | 147,5 | 24 / 199,5 | 151,5 | prima sfasato di 6 px rispetto a proof; ora identico |
| `.stay__hero` | 24 | 327 | invariato | 327 | |
| header: menu / logo / Richiedi | 24 / centro 187,5 / destra 351 | | invariato | | |
| `.hero__c` | 24 | 327 | invariato | 327 | |

Padding di sezione a 375 (prima → dopo, invariati): proof 48, carosello 48, camere 32, camere-griglia (sotto) 48, FAQ 48, sezione finale 72. Tutti multipli di 8.

### Verifiche automatiche dopo la correzione
Test in iframe (IT, EN, DE) a 320, 360, 375, 414, 600, 700, 701, 768, 900, 1024, 1100, 1101, 1120, 1150, 1180, 1200, 1240, 1280, 1366, 1440, 1536, 1680, 1920, 2560 px: nessun overflow orizzontale, nessun testo che sfora la propria colonna, `.proof`, `.grid4` e le colonne del footer sulle stesse x, `.spot__l` = colonna 1, primo `.scard` = colonna 2, `.faq__l` = colonna 1, `.faq__r` = colonne 2-4 con bordo destro uguale a quello della colonna 4, margini sinistro e destro identici in proof, camere, FAQ, breadcrumb e immagine camere. A 1920 il contenuto è centrato (312 px per lato con scrollbar), come Atlantis (307 px misurati a 1910).
Carosello: pulsanti avanti/indietro con passo 392 px, l'ultima card termina a 1353 px, cioè sul margine destro del contenuto.

### Correzioni (file `src/assets/style.css`)

| Regola | Valore vecchio → nuovo |
|---|---|
| `:root` | aggiunti `--u --max --mc --g --card --sec --sec-s --row` e documentazione; `--m`, `--hh` invariati |
| `.spot` | `flex; gap:40px; padding:72px 0 72px var(--m)` → `grid 4 colonne; column-gap:var(--g); padding:var(--sec) var(--mc)` |
| `.spot__l` | `flex:0 0 292px` → `grid-column:1` (colonna 1 fluida) |
| `.spot__track` | `flex:1; gap:40px; padding-right:var(--m)` → `grid-column:2 / -1; gap:var(--g); margin-right:-var(--mc); padding-right:var(--mc)` (parte dalla colonna 2 e sfora a filo a destra) |
| `.scard`, `.scard img` | `352px` → `var(--card)`; le due regole mobile `280px` eliminate (ora è il token) |
| `.stay` | `padding:56px var(--m) 56px` → `padding:var(--sec-s) var(--m)` (tablet 56 → 48; mobile 32 invariato) |
| `.grid4` | `repeat(4,1fr); gap:40px; padding:0 var(--m) 72px` → `repeat(4,minmax(0,1fr)); gap:var(--g); padding:0 var(--mc) var(--sec)` (tablet: padding basso 72 → 56, `row-gap:56px` → `var(--row)`) |
| `.proof` | `repeat(4,1fr); gap:40px; padding:56px var(--m)` → `repeat(4,minmax(0,1fr)); gap:var(--g); padding:var(--sec-s) var(--mc)` (mobile gap `32px 20px` → `32px var(--g)`) |
| `.faq` | `292px 1fr; gap:40px; padding:72px var(--m)` → `repeat(4,minmax(0,1fr)); gap:var(--g); padding:var(--sec) var(--mc)` (tablet padding 72 → 56) |
| `.faq__l` / `.faq__r` | `.faq__r{max-width:860px}` → `.faq__l{grid-column:1}` `.faq__r{grid-column:2 / -1}` (senza tetto: bordo destro sulla colonna 4) |
| `.ft` | `padding:0 var(--m)` → `padding:0 var(--mc)` |
| `.ft__main` | `grid .75fr 2fr; gap:56px` → 1 colonna, `gap:32px` (logo sopra, poi le colonne; a ≤1100 era già così) |
| `.ft__cols` | `repeat(4,1fr); gap:32px` → `repeat(4,minmax(0,1fr)); gap:32px var(--g)`; a ≤1100 ora `repeat(2,…)` (prima restava a 4 fino a 700px) |
| `.fin` | `padding:96px 24px` → `padding:calc(var(--u)*12) var(--m)` (laterale tablet 24 → 40); mobile `72px 24px` → `calc(var(--u)*9)` verticale |
| `.hero__c` | `width:min(1065px,100% - 48px)` → `min(1065px,100% - 2*var(--m))` |
| `.story__t` | `width:min(844px,100% - 48px)` → `min(844px,100% - 2*var(--m))` |
| `.spot__l` (tablet) | `width:min(520px,100% - 40px - var(--m))` → `max-width:520px` (margini simmetrici; a 375 il blocco passa da 287 a 327 px) |
| `.spot__l h3`, `.faq__l h3` | `font:400 40px/48px` → `font-size:clamp(32px,3vw,40px)`; a ≤1100 di nuovo `40px` (vedi deviazioni) |
| `.grid4 h4` | `24px` → `clamp(20px,1.95vw,24px)`; a ≤1100 di nuovo `24px` |
| `.ft__cols a/span/.lnk-b` | aggiunto `overflow-wrap:anywhere` (email sotto i 360px) |
| `.proof span` | aggiunto `overflow-wrap:break-word` (parola lunga DE sotto i 360px) |
| `@media` | i 4 blocchi (2 da 1100px, 2 da 700px) uniti in 2 blocchi in fondo al file, commentati; rimossa la regola duplicata `.menu__x{left:var(--m)}` a ≤700 (la base usa già `--m`) |

Rigenerati `site/index.html`, `site/en/index.html`, `site/de/index.html`; `site/assets/style.css` identico al sorgente.

### Deviazioni dal riferimento Atlantis (motivate)
1. **Footer con logo sopra le 4 colonne.** In Atlantis il logo sta a sinistra e le colonne di link sono compatte a destra (x=516, passo 165 a 1430 px), fuori dalla griglia a 4 colonne. La consegna chiedeva colonne footer sulle stesse x di proof e camere: con 5 blocchi (logo + 4 gruppi) l'unico modo è mettere il logo su una riga sua. Su tablet e mobile era già così.
2. **Colonne fluide invece di 292 px fissi.** Atlantis usa colonne fluide (291,5 px a 1430, 294 px oltre 1440); il sito aveva 292 px fissi solo su testo carosello e FAQ, quindi sfasati dalle colonne di proof/camere sotto ~1440 px. Ora tutto usa la stessa griglia fluida; la card resta a 352 px (in Atlantis 351,7-354).
3. **FAQ fino alla colonna 4.** Il blocco FAQ non esiste in Atlantis; la vecchia larghezza 860 px non cadeva su nessuna linea di griglia.
4. **Tetto del contenuto a 1296 px.** Misurato in Atlantis a 1910 px (contenuto 307-1603). Come lì, l'header, l'immagine "Camere" e la sezione finale restano a filo con margine 72 px, mentre proof, carosello, camere, FAQ e footer seguono il tetto. Il footer di Atlantis invece resta a filo: qui segue il tetto per tenere le colonne sulla stessa griglia. Poiché `100vw` include la scrollbar, su Windows il contenuto massimo è 1296 meno la scrollbar (1281 a 1440 px); senza scrollbar ("overlay") è 1296.
5. **Titoli h3/h4 fluidi tra 1101 e circa 1330 px.** Con le colonne fluide, a 1101 px la colonna 1 è larga ~205 px e parole come "Vorschlag" (239 px a 40 px) o "Standardzimmer" (225 px a 24 px) sforavano. Corpo ridotto solo in quella fascia (33→40 px per h3, 21,5→24 px per h4); da 1440 px identico ad Atlantis. Da rivalutare dal passaggio "gerarchia" se cambia la scala tipografica.
6. **Header mobile.** Atlantis usa margine 16 px sull'header mobile; il sito mantiene 24 px come da consegna (uguale al contenuto).
7. **Card mobile 280 px** (Atlantis 354 px) e altezze tipografiche non toccate: scelte preesistenti, fuori dal compito.

### Non verificato / limiti
- Solo browser integrato (Chromium). Nessuna prova su Safari, Firefox o dispositivi reali; `max()`, `clamp()` e `minmax(0,1fr)` sono supportati da tutti i browser attuali.
- Scrollbar "overlay" (macOS, mobile reali): il contenuto massimo sarebbe 1296 px esatti invece di 1281; non simulabile qui.
- Le foto arrivano da hotelbelfiore.it e sono in `loading="lazy"`: alcune non erano ancora caricate negli screenshot; le misure di layout non ne dipendono (le immagini hanno dimensioni da CSS).
- Screenshot del pannello a bassa risoluzione: la verifica visiva è stata fatta su porzioni di pagina (carosello, camere, FAQ, footer) a 1440, 1100 e 375; il controllo puntuale è nelle misure `getBoundingClientRect` e nei test in iframe.
- Fuori dalla griglia, non misurati: menu a schermo intero, pannello di richiesta (`.req`, padding interni 40/24 px e `gap:20px` dei campi), barra CTA mobile, punti di navigazione della sezione story (`right:28px`).
- Valori "a mezzo passo" o non multipli di 8 lasciati per non alterare il disegno (sono spaziature interne ai componenti, non di sezione): `.fin__l` margine 36, `.fin__cta` margine 28, `.faq details p` padding 26, gap 12/14/20 nei componenti (breadcrumb, story dots, form, `.hero__cta` mobile), `.ft__main` padding 56/48.
- Finestre molto basse in orizzontale (per esempio 812×375) e stampa non controllate.

## 2. Gerarchia visiva

Ambito: `src/assets/style.css`, `src/assets/main.js` (solo la funzione della barra CTA mobile) e la chiave `hero_h1` dei tre JSON (solo spazi non separabili, vedi sotto). Nessun colore di link, bordo dei campi, focus ring, messaggio d'errore o comportamento del modulo è stato toccato. I token della griglia (`--u --m --max --mc --g --card --sec --sec-s --row`) sono invariati. Build rigenerata (`site/index.html`, `site/en/index.html`, `site/de/index.html`; `site/assets/*` identici ai sorgenti).

Metodo: audit su 1440×900, 1100×800, 768×1024 e 375×812 (IT) più EN/DE a 375, 768, 1100, 1440; lettura del CSS calcolato (dimensione, peso, colore, interlinea) di ogni livello; test "strizzare gli occhi" con `filter: blur(5px)` temporaneo sul `<main>` (rimosso, nessun filtro resta nella pagina); contrasti misurati sui pixel reali (vedi sotto).

### Mappa dei livelli per sezione (prima → dopo)

| Sezione | Prima: livello 1 / 2 / 3 | Problema trovato | Dopo |
|---|---|---|---|
| Header | 1 logo 64px · 2 menu e "Richiedi" (outline 168×48) | Nessuno: il pulsante è outline, area 46% del CTA dell'hero (8064 vs 17628 px²), non prevale (confermato dal test blur) | Invariato |
| Hero | 1 H1 60px · 2 CTA piena 339×52 · 3 sottotitolo, eyebrow, link tel, micro | H1 su 3 righe con "PISTE" sola in terza riga e "Val di Sole" spezzato; contrasto del testo sulla foto molto basso (H1 fino a 1,74:1, sottotitolo 2,99:1); micro 13px peso 300. A 375px CTA da 66px (etichetta su 2 righe) = blocco color sabbia più pesante del necessario accanto a un H1 di 4 righe | Stessa gerarchia, H1 bilanciato ("SETTIMANA BIANCA IN / VAL DI SOLE, CON NAVETTA / GRATUITA PER LE PISTE"), velo che garantisce il contrasto, CTA mobile 52px su una riga |
| Prove | 1 quattro numeri 40px (= corpo H3) · 2 didascalie 14px | I numeri hanno lo stesso corpo dei titoli di sezione e sono su una riga a 4 colonne: nel blur pesano quanto un H3 e anticipano il titolo del pannello successivo. Tra 701 e 1100px sarebbero risultati più grandi degli H3 fluidi | Numeri = H3 ÷ 1,25 (32px a 1440, 22,4px su mobile); restano il livello 1 della fascia ma non gareggiano coi titoli |
| Pannelli a scorrimento | 1 H3 40px · 2 paragrafo · 3 eyebrow, pulsante outline | Ordine corretto; testo illeggibile su parte delle foto (contrasto 1,9–2,9:1 nei punti chiari) | Ordine invariato, testo leggibile (≥ 4,9:1 a 1440) |
| Carosello | 1 immagini 352×352 e H3 di colonna 1 · 2 H4 24px · 3 testo, link "Scopri" | I link "Scopri" cadevano a quote diverse (titoli di 1 o 2 righe) | Link sulla stessa linea di base (margin-top:auto) |
| Camere (foto) | 1 H3 40px su foto · 2 CTA piena 231×52 · 3 testo, link | Contrasto testo/foto 2,7–2,9:1 | Velo radiale: ≥ 6,5:1 |
| Griglia 4 card | 1 foto 290×290 · 2 H4 24px · 3 testo · 4 outline + link | I 4 pulsanti stavano a quote diverse (titolo su 1 o 2 righe): ritmo irregolare | Pulsanti allineati sulla stessa quota (identica nelle 4 card a 1440; per riga a 2 colonne, 768px) |
| FAQ | 1 H3 di colonna 1 · 2 domande 20px · 3 risposte 14px | Nessuno | Invariato |
| Sezione finale | 1 H3 40px · 2 CTA piena 339×52 · 3 elenco, contatti | Nessuno (contrasto già ≥ 6,8:1); unica azione piena, inequivocabile | Invariato (solo peso 400 sui testi ≤14px) |
| Barra CTA mobile | 1 "Richiedi" pieno · 2 "Chiama" outline | Chiara, ma restava visibile insieme al CTA pieno di camere e finale (due pulsanti pieni "Richiedi" nella stessa schermata) | Si nasconde quando un `.btn-f` della pagina è in vista sopra di lei |

### Scala tipografica (verifica dei rapporti tra livelli)

| Livello | Prima (desktop 1440 / mobile 375) | Dopo (desktop 1440 / mobile 375) |
|---|---|---|
| Eyebrow | 16px / 14px, spaziatura 4px / 3px | invariato |
| H1 hero | `clamp(34px,4.4vw,60px)` → 60px / 32px | `clamp(36px,4.6vw,60px)` → 60px / 32px (28→32px sotto 700) |
| H3 sezione | 40px fisso (anche a 768px) / 28px | 40px / 28px; tra 701 e 1100px fluido 28→40px (`clamp(28px,3.6vw,40px)`); sotto 700px 24→28px |
| H4 card | 24px / 24px | 24px / 22px; tra 701 e 1100px 22→24px; su mobile h3 ÷ 1,27 (22px a 375, 19px a 320) |
| Numeri prove | 40px / 28px | 32px / 22,4px |
| Testo | 14/24 (16/26 nel sottotitolo hero) | invariato |
| Micro (sotto la CTA) | 13px peso 300 | 13px peso 400 |

Rapporti misurati con iframe a 15 larghezze (320 → 1920) in IT e a 5 (320, 375, 768, 1100, 1440) in EN e DE: nessuno scroll orizzontale, nessun titolo, numero o pulsante che sfora la propria casella:
- H1/H3: prima **inversione** tra 700 e 900px (H1 34–40px contro H3 fisso 40px: a 768px il titolo hero era più piccolo dei titoli di sezione, rapporto 0,85). Ora ≥ 1,27 da 701px in su (1,29 a 768, 1,28 a 1100, 1,50 a 1440). Su mobile 1,14 (375px) e 1,17 (320px): vedi "Dubbi".
- H3/H4 ≥ 1,27 a ogni larghezza (prima 1,17 su mobile: 28px contro 24px).
- H3/numeri prove = 1,25 a ogni larghezza.
- Hero: eyebrow → H1 = 3,75 (2,3 su mobile); H1 → sottotitolo = 3,75 (2,0); sottotitolo → micro = 1,23 (16px → 13px), appena sotto 1,25: lasciato perché salire con il sottotitolo o scendere con il micro peggiorerebbe la leggibilità sulla foto (differenza rinforzata da peso e opacità).
- Eyebrow e sottotitolo hanno lo stesso corpo (16px) ma stile opposto (maiuscolo Marcellus spaziato contro Figtree 300 in minuscolo): livelli distinti per forma, non per dimensione.
- Prossimità hero: eyebrow → H1 16px, H1 → sottotitolo 20px, sottotitolo → CTA 32px, CTA → micro 20px: la CTA e la sua rassicurazione formano un gruppo, staccato dal testo (rispetta la gerarchia di distanze 16 < 20 < 32).
- A 375px il titolo (4 righe, 160px in IT; 200px in DE) non è schiacciato: sottotitolo 104px, CTA 52px, link 23px, micro 48px; l'insieme (eyebrow → micro) occupa 132–680 px su 812. A 36px il titolo IT andrebbe a 6 righe strette (253px): per questo su mobile il tetto resta 32px.

### Gerarchia delle azioni

| Ruolo | Componente | Dove | Dimensione |
|---|---|---|---|
| Primaria (piena, sabbia) | `btn-f` | hero, camere, sezione finale; "Richiedi" nella barra mobile | 339×52 · 231×52 · 339×52 · flex:1 |
| Secondaria (contorno) | `btn-o` | "Richiedi" header, 6× "Scopri" nei pannelli, 4× "Richiedi disponibilità" nella griglia, "Chiama" nella barra mobile | 168×48 · 110×40 · 206×40 · 48px alto |
| Terziaria (sottolineata) | `lnk` | link tel hero, "Scopri" (carosello, camere, griglia), contatti finali, footer | 20px di riga |

Verifiche:
- **Griglia a 4 card vs pulsante primario delle camere:** non competono. Il primario è pieno (sabbia), peso 500, 52px; i quattro sono contorno, peso 400, 40px (area 8240 contro 12012 px² per singolo pulsante), bordo sabbia chiaro su bianco; nel test blur risultano linee sottili sotto le foto. Restano outline come in Atlantis; non sono stati declassati a link perché in ogni card accanto c'è già il link "Scopri" (due link affiancati sarebbero indistinguibili). Intervento fatto solo sull'allineamento: i quattro pulsanti ora stanno sulla stessa quota.
- **"Richiedi" nell'header vs hero:** non prevale (contorno contro pieno, 46% dell'area, nel blur la CTA sabbia emerge prima). Nessuna modifica. Osservazione per chi rivede i colori: il bordo sabbia sull'header scuro è molto luminoso; è un tema di contrasto, non di gerarchia.
- **Sezione finale:** un solo pulsante pieno (339×52), il resto è testo e link; azione primaria inequivocabile. Nessuna modifica di stile.
- **Barra CTA mobile:** primaria chiara (piena, flex:1) contro "Chiama" (contorno, larghezza al contenuto). Nuova regola in `main.js`: la barra si nasconde anche quando un pulsante primario `.btn-f` della pagina è visibile sopra di lei, così non ci sono mai due pulsanti pieni "Richiedi" nella stessa schermata (prima succedeva nel blocco camere e nel finale). Provata a 375px in 14 posizioni di scorrimento: spenta su hero, camere (pulsante in vista) e finale; accesa su pannelli, carosello, griglia e FAQ.
- **Pulsante primario mobile:** `.btn-f` a ≤700px da `padding:18px 44px` a `18px 24px`: l'etichetta IT più lunga ("Richiedi la tua proposta di soggiorno", 249px) sta su una riga, pulsante 327×52 invece di 327×66.

### Valori CSS cambiati (`src/assets/style.css`)

| Regola | Vecchio → nuovo |
|---|---|
| `:root` | aggiunti `--fs-h1:clamp(36px,4.6vw,60px)` · `--fs-h3:clamp(28px,3.6vw,40px)` · `--fs-h4:clamp(22px,2.2vw,24px)` · `--fs-proof:calc(var(--fs-h3) / 1.25)` · `--scrim:rgba(0,0,0,.5) 0%,rgba(0,0,0,.3) 55%,rgba(0,0,0,0) 100%`; a ≤700px `--fs-h3:clamp(24px,7.5vw,28px)` e `--fs-h4:calc(var(--fs-h3) / 1.27)` |
| `h1,h3,h4,h5` | + `text-wrap:balance` |
| `.hero h1` | `clamp(34px,4.4vw,60px)` → `var(--fs-h1)`; ≤700px `32px/40px` → `clamp(28px,8.6vw,32px)/1.25` |
| `.hero__shade` | `linear(.45, .2@35%, .3@65%, .6)` → `radial-gradient(ellipse 65% 55%, var(--scrim))` + `linear(.45, .32@35%, .4@65%, .6)`; ≤1100px ellisse 95%×55%; ≤700px 120%×68% |
| `.hero__micro` | peso 300 → 400 |
| `.story__shade` | solo lineare → `radial-gradient(ellipse 55% 42%, var(--scrim))` + lineare invariato; ≤1100px 90%×42%; ≤700px 120%×48% |
| `.stay__shade` | solo lineare → `radial-gradient(ellipse 55% 45%, var(--scrim))` + lineare invariato; ≤1100px 90%×45%; ≤700px 120%×52% |
| `.story__t p`, `.stay__c p`, `.fin__c>p`, `.fin__l li`, `.fin>.fin__c>.fin__d` | peso 300 → 400 (`.fin__shade` invariato: già ≥ 6,8:1) |
| `.proof b` | `40px/48px` → `var(--fs-proof)/1.25` (32px/40px a 1440; ≤700px `28px/34px` → 22,4px/28px, regola mobile eliminata) |
| Tablet ≤1100px | `.spot__l h3,.faq__l h3{font-size:40px}` → `.story__t h3,.spot__l h3,.stay__c h3,.faq__l h3,.fin__c h3{font-size:var(--fs-h3);line-height:1.2}`; `.grid4 h4{24px}` → `.scard h4,.grid4 h4{font-size:var(--fs-h4)}` |
| Mobile ≤700px | `.btn-f{padding:18px 24px}` (nuova); h3 `28px/32px` → `var(--fs-h3)/1.15`; `.scard h4,.grid4 h4` `24px/28px` → `var(--fs-h4)` (= h3 ÷ 1,27) con interlinea 1,2 |
| `.scard` / `.scard .lnk` | `.scard` + `display:flex;flex-direction:column`; `.scard .lnk{margin-top:auto;align-self:flex-start}` |
| `.grid4 article` / `.act` | `.grid4 article{display:flex;flex-direction:column}`; `.act` + `margin-top:auto` |
| `src/assets/main.js` `stickyBar()` | + condizione "nessun `main .btn-f` in vista sopra la barra" |
| `src/lang/{it,en,de}.json` `hero_h1` | "Val di Sole" → "Val&nbsp;di&nbsp;Sole" (spazi non separabili, il testo non cambia) |

### Contrasto reale del testo sulle foto

Metodo: le 8 foto usate (hero, 6 pannelli, camere; la sezione finale usa la foto dell'hero) sono state scaricate in una cartella temporanea fuori dal progetto, ricomposte su canvas come le disegna il browser (`object-fit: cover`, `object-position`, gradiente CSS sopra, dimensioni reali dell'elemento) e per ogni riga di testo (rettangoli reali dal DOM, non il blocco) sono stati campionati tutti i pixel sotto la riga. Per ogni pixel: contrasto tra il colore del testo (con la sua trasparenza) e lo sfondo. Si riporta **p1** = valore che il 99% dei pixel supera (cioè i punti più chiari) e **p5**. Soglie: 4,5:1 per testo sotto 24px, 3:1 per testo da 24px in su. Per l'hero sono inclusi i due estremi dell'animazione Ken Burns (zoom 1,1 e 1). Le righe della sezione finale non sono cambiate.

Misura a 1440×900 e 375×812 (peggior riga di ogni sezione, contrasto p1):

| Sezione | 1440 prima | 1440 dopo | 375 prima | 375 dopo |
|---|---|---|---|---|
| Hero, testo piccolo | 2,15 (eyebrow); sottotitolo 2,99; p5 sottotitolo 3,74 | 5,79 | 2,27 (eyebrow); sottotitolo 2,98 | 5,73 |
| Hero, H1 | 1,74 (p5 3,24) | 5,43 | 2,06 | 5,49 |
| Pannelli, testo piccolo | 1,88 (eyebrow pannello 1); paragrafi 2,12–2,87 (pannelli 1, 4, 5, 6) | 4,88 | 2,10 (paragrafo pannello 4) | 5,66 |
| Pannelli, H3 | 2,17 (pannello 4); 2,24–2,33 (1, 5, 6) | 5,51 | 2,01 (pannello 1) | 5,28 |
| Camere, testo piccolo | 2,66 (eyebrow), 2,86 (paragrafo) | 6,52 | 2,80 (paragrafo) | 8,03 |
| Camere, H3 | 2,77 | 7,55 | 2,71 | 7,27 |
| Sezione finale | ≥ 6,81 | ≥ 6,81 (invariato) | ≥ 6,99 | ≥ 6,99 (invariato) |

Peggior riga (p1) con il CSS finale in altre condizioni: 1920×1080 5,11 · 1366×768 4,75 · 1100×800 5,01 · 768×1024 4,95 · 414×896 5,63 · 360×740 5,26; EN 1440 4,88 e 375 5,51; DE 1440 4,88 e 375 5,48. Minimo assoluto tra tutte le prove finali: 4,75:1 per il testo piccolo (soglia 4,5, pannello 1 a 1366×768) e 4,77:1 per il testo grande (soglia 3, EN a 375px). Il margine sul testo piccolo è quindi ridotto (0,25–0,4 punti) nei pochi punti dove la foto ha zone molto chiare sotto l'eyebrow del pannello 1.

Intervento: velo radiale scuro al centro (0,5 → 0,3 a metà raggio → 0) sopra il gradiente lineare esistente, così la foto resta piena ai bordi. Più peso 400 (invece di 300) per i testi ≤14px sulle foto. Ho provato per ogni sezione una serie di intensità (da 0,3 a 0,7 al centro) e scelto la più leggera che tiene ≥ 4,5:1 con margine; nell'hero ho anche alzato lo stop lineare al 35% (0,2 → 0,32) e al 65% (0,3 → 0,4) perché eyebrow e micro cadono dove il velo era più leggero.

### Deviazioni dal riferimento Atlantis (motivate)
1. **Velo radiale sotto i testi su foto** (Atlantis usa un velo uniforme leggero): con le foto di questo hotel (cielo blu ora, luci, neve) un velo uniforme sufficiente alla lettura scurirebbe tutta la foto; quello radiale la lascia piena ai bordi.
2. **Peso 400 al posto di 300 per testi ≤14px sulle foto** (paragrafi dei pannelli e delle camere, elenco e contatti finali, micro dell'hero): il tratto sottile perde contrasto effettivo sulle foto anche con velo. Il sottotitolo hero (16px) resta 300.
3. **Numeri della fascia prove a H3 ÷ 1,25** (32px a 1440): in Atlantis non esiste questa fascia (è un'aggiunta di conversione), quindi non ha un riferimento da rispettare; il corpo uguale agli H3 la faceva concorrere coi titoli.
4. **Scala H1/H3 fluida** (H1 `clamp(36px,4.6vw,60px)`, H3 28→40px sotto 1100px): a 1440 e oltre è identica al riferimento (60px / 40px). Sotto 1100px sostituisce la deviazione n. 5 del §1 ("h3 di nuovo 40px a ≤1100"): ora l'H3 scende con la larghezza per non superare l'H1 (tra 700 e 900px l'H1 era più piccolo degli H3). Le regole per i titoli di colonna 1 tra 1101 e 1330px del §1 sono invariate.
5. **`text-wrap: balance` sui titoli** e spazi non separabili in "Val di Sole" nell'H1: evita la parola sola in ultima riga e non spezza il nome del luogo. Nei browser che non supportano `balance` (prima di Chrome 114, Firefox 121, Safari 17.5) il titolo va a capo come prima.
6. **CTA mobile da 52px** invece di 66px (padding laterale 24 invece di 44): etichetta IT su una riga.
7. **Barra CTA mobile che si nasconde con un pulsante pieno in vista**: comportamento non presente in Atlantis (dove non c'è la barra), aggiunto per avere una sola azione primaria per schermata.
8. **Card allineate con `margin-top:auto`** (link del carosello, pulsanti della griglia): Atlantis lascia le azioni a quote diverse quando i titoli hanno lunghezze diverse.

### Non verificato / dubbi
- Solo Chromium (browser integrato). `text-wrap: balance` non testato in Safari/Firefox; il resto (`clamp`, `radial-gradient(ellipse …)`, custom property con lista di stop) è supportato da tutti i browser attuali.
- I contrasti dipendono dalle foto attuali di hotelbelfiore.it: se vengono sostituite vanno rimisurati. La misura ricompone l'immagine con canvas e non è un rendering pixel-per-pixel del browser (anti-aliasing del testo, scala 1,07→1 dei pannelli e stati intermedi dell'animazione non simulati; usato lo stato finale, più lo zoom 1,1 dell'hero). Non simulati nemmeno rapporti d'aspetto estremi (schermi molto larghi e bassi, telefono in orizzontale 812×375).
- **Rapporto H1/H3 su mobile 1,14 (375px) e 1,17 (320px)**, sotto l'1,25 richiesto: salire a 34–36px porta il titolo IT a 5–6 righe strette e spinge giù CTA e prove. Il primato dell'H1 su mobile poggia su posizione, massa (4 righe, 160px) e contrasto; se si vuole 1,25 l'unica strada è ridurre gli H3 mobile a ~26px (non fatto per non alterare tutti i titoli).
- Rapporto sottotitolo/micro nell'hero 1,23 (< 1,25), motivo sopra.
- Eyebrow di carosello, FAQ e camere usano lo stesso colore dei titoli (`--ink`): la gerarchia regge per dimensione e forma (14–16px spaziato contro 24–40px), ma un eyebrow più chiaro la rafforzerebbe. È un tema di colore/contrasto e lo lascio a chi rivede i colori.
- Non toccati e non rivisti: menu a schermo intero, pannello di richiesta (`.req`), footer, e tutti i contrasti su sfondo bianco (link sabbia scuro, bordi dei campi).
- Per il campionamento dei pixel ho scaricato in una cartella temporanea di sessione (fuori dal progetto, ~15,9 MB) le 8 foto che la pagina già collega e ho usato un mini-server locale sulla porta 5174 con CORS aperto; il server è spento, la porta libera, nessun file del progetto coinvolto.

## 3. Accessibilità visiva e daltonismo

Ambito: `src/assets/style.css`; `src/template.html` (5 `<span class="fe">` accanto ai campi del modulo); `src/assets/main.js` (helper `mark()` + 3 righe nel submit, solo attributi ARIA e classe `.bad` sulla casella privacy, nessuna modifica alla logica di validazione); `src/lang/{it,en,de}.json` (2 chiavi nuove in tutte e tre le lingue: `form_err_field`, `form_err_priv`). Nessun file scaricato su disco. Build rigenerata (`site/index.html`, `site/en/`, `site/de/`; `site/assets/*` identici ai sorgenti). I token griglia (`--u --m --g --card…`), la scala `--fs-*` e il velo `--scrim` non sono toccati. Vincolo di lavoro: **aderenza ad Atlantis**, quindi solo correzioni a soglia WCAG con lo scarto di luminosità più piccolo, stessa tinta, nessun colore nuovo.

Metodo: rapporto di contrasto WCAG (luminanza relativa) calcolato in JS nel browser integrato, (a) sulle coppie di valori dei token e (b) sull'audit automatico del DOM (colore calcolato del testo e sfondo composto dagli antenati con trasparenza, dimensione e peso per scegliere la soglia 4,5 o 3) su IT, EN e DE, con menu a schermo intero e pannello di richiesta aperti e messaggi di errore forzati. Dopo le correzioni: **0 coppie testo/sfondo sotto soglia** (30 coppie uniche per lingua, fuori dalle foto) nelle tre lingue. Minimo assoluto 4,61:1 (`#757575` su bianco, testo 12–15px), invariato.

### 3.1 Colore scelto per il testo sabbia su fondo chiaro
- `--sand-d` **#a98a66** (HSL 32,2° / 28,0% / 53,1%) dà 3,22:1 su bianco e 3,09:1 su #fafafa: passa per gli elementi non testuali (3:1) ma non per il testo (4,5:1).
- Nuovo token `--sand-t` **#8b6e4e** (HSL 31,5° / 28,1% / 42,5%): stessa tinta e saturazione, solo la luminosità scende dal 53% al 42,5%. È il colore più vicino a `--sand-d` (passo di 0,0005 in luminosità HSL) che raggiunge **4,73:1 su #fff e 4,53:1 su #fafafa** (sfondo del pulsante "Richiedi" nell'header chiaro).
- `--sand-d` resta com'è e viene usato solo per bordi e indicatori (3:1); `--sand` #d2b99b resta com'è sui fondi scuri (9,72:1 su #181411, 9,84:1 nel menu).
- Nuovi token di supporto: `--line` #949494 (linee dei componenti UI su bianco, 3,03:1), `--err` #b3402c (il rosso già presente, ora nominato), `--ring` (anello di focus, vedi 3.2).

### 3.2 Tabella contrasti prima → dopo (WCAG 1.4.3 testo, 1.4.11 componenti)

Testo (soglia 4,5:1; 3:1 per corpo ≥24px o ≥18,66px grassetto):

| Elemento | Colori | Prima | Dopo | Esito |
|---|---|---|---|---|
| Link `.lnk` "Scopri" (14px), carosello e griglia | `#a98a66` → `#8b6e4e` su #fff | 3,22 | **4,73** | corretto |
| Pulsante contorno `.btn-o--dk` (etichetta 14px), griglia camere | `#a98a66` → `#8b6e4e` su #fff | 3,22 | **4,73** | corretto |
| "Richiedi" nell'header chiaro (etichetta) | `#a98a66` → `#8b6e4e` su #fafafa | 3,09 | **4,53** | corretto |
| Link "privacy" nel modulo `.check a` (13px) | `#a98a66` → `#8b6e4e` su #fff | 3,22 | **4,73** | corretto |
| Hover pulsante "Invia" `.btn-s:hover` | bianco su `#a98a66` → su `#8b6e4e` | 3,22 | **4,73** | corretto |
| Hover link footer `.ft__cols a`, `.soc a` | `#a98a66` → `#8b6e4e` su #fff | 3,22 | **4,73** | corretto |
| Testo corrente `--text` | `#424242` su #fff | 10,05 | 10,05 | ok, invariato |
| Titoli / eyebrow scuri (`.eyebrow--dk`, `.scard .eyebrow` 14px, `h4`, `summary`, breadcrumb) | `#212121` su #fff | 16,10 | 16,10 | ok |
| Titoli di colonna 1 (`.spot__l h3`, `.faq__l h3`) | `#000` su #fff | 21,00 | 21,00 | ok |
| Secondario: `.ft__bar` 12px, etichette `.req label` 12px, `.req__micro` 12px, testo casella privacy 13px, placeholder del messaggio 15px (`rgb(117,117,117)` letto da `::placeholder`) | `#757575` su #fff | 4,61 | 4,61 | ok, margine sottile (0,11) |
| Errore: `.req__err` 13px e nuovi `.fe` 12px | `#b3402c` su #fff | 5,70 | 5,70 | ok |
| Pulsante pieno `.btn-f` | `#1b1712` su `#d2b99b`; hover su `#fefaf5` | 9,47; 17,16 | invariato | ok |
| Pulsante contorno su header scuro `.btn-o`; hover | `#d2b99b` su #181411; `#212121` su sand | 9,72; 8,55 | invariato | ok |
| Menu a schermo intero: voci, piede, lingua | bianco su #151311 (18,53); `#d0d0cf` su #151311 (12,01); hover sabbia (9,84); lingua attiva `#212121` su #fff (16,10) | – | invariato | ok |
| Barra CTA mobile: etichette, pieno | `#fefaf5` su #22211f (15,78, caso peggiore con contenuto bianco sotto la barra al 94%); sabbia (8,71) | – | invariato | ok |
| Link `.fin__d a` (13px) sabbia sul velo della foto finale | derivato dalla misura del §2 (testo bianco 80% ≥ 6,81:1 → sabbia stimato ≥ 5,3:1) | non misurato | non misurato | vedi 3.9 |
| Testo bianco su foto (hero, pannelli, camere, finale) | misurato nel §2 (minimo 4,75:1 piccolo, 4,77:1 grande) | – | invariato (colori e veli non toccati) | ok |

Componenti UI e grafica (soglia 3:1 contro i colori adiacenti):

| Elemento | Colori | Prima | Dopo | Esito |
|---|---|---|---|---|
| Linea dei campi del modulo | `#c9c9c9` → `#949494` su #fff | 1,66 | **3,03** | corretto |
| Bordo pulsante contorno `.btn-o--dk` (griglia camere) | `#d2b99b` → `#a98a66` su #fff | 1,88 | **3,22** | corretto |
| Bordo "Richiedi" nell'header chiaro | `#d2b99b` → `#a98a66` su #fafafa (bianco) | 1,80 (1,88) | **3,09** (3,22) | corretto |
| Anello di focus, fondi chiari | 1px `#d2b99b` → 2px `#8b6e4e` su #fff | 1,88 | **4,73** | corretto |
| Anello di focus, fondi scuri | 2px `#d2b99b` su #181411 / menu | 9,72 / 9,84 | 9,72 / 9,84 | ok (su foto: vedi 3.9) |
| Traccia del carosello `.bar` | `#d9d9d9` → `#949494` su #fff | 1,41 | **3,03** | corretto |
| Indicatore del carosello `.bar i` (2px) | `#a98a66` → `#8b6e4e` su #fff | 3,22 | **4,73** | ok (contro la nuova traccia 1,56, ma il tratto da 2px sporge sul bianco sopra e sotto la traccia da 1px) |
| Stato errore: linea del campo | `#b3402c` su #fff | 5,70 | 5,70 (invariato) + 2px, icona, testo | vedi 3.4 |
| Frecce `.arr`: glifo | `#212121` su #fff | 16,10 | 16,10 | ok |
| Frecce `.arr`: contorno del cerchio | `#d9d9d9` su #fff | 1,41 | invariato | non richiesto: il glifo identifica già il pulsante (1.4.11: il contorno non è necessario), rimane come sfumatura estetica |
| Chevron FAQ, linee dell'hamburger | `#212121` su #fff / bianco su header scuro | 16,10 / ≥ 9 | invariato | ok |
| Contorno lingua `.lang` nel menu | bianco 35% su #0e0c0a | 3,16 | 3,16 | ok |
| Casella privacy (bordo nativo Chrome con `accent-color`) | `#767676` su #fff | 4,54 | invariato | ok (dipende dal browser) |
| Punti dei pannelli `.story__dots` | bianco / bianco 40% su foto | non misurabile | invariato | `aria-hidden`, ridondante col testo del pannello; stato attivo dato anche dall'altezza (44 contro 22px); ispezione visiva su pannelli 1, 3, 5, 6: visibili |
| Separatore breadcrumb `.crumb i`, filetti `#ececec` / `#e3e3e3` delle sezioni e delle FAQ | `#c9c9c9` / chiari su #fff | 1,66 / 1,18–1,28 | invariato | decorativi: la separazione è data anche dallo spazio (gap 14px) e dal testo; non componenti UI |
| Contorno delle card | – | – | – | non esiste (le card sono foto senza bordo) |
| `:disabled` | – | – | – | non esiste nessuno stato disabilitato nel sito |

Caso limite dichiarato: con l'header chiaro (bianco 88%) una foto scura che passa sotto (immagini del carosello, foto camere) abbassa lo sfondo fino a circa #e0e0e0; in quel caso transitorio il testo `#8b6e4e` scende a 4,15:1 su #f0f0f0 e l'anello a 3,58:1. Nei tre stati stabili (header sopra le sezioni bianche) valgono i valori della tabella.

### 3.3 Focus visibile (WCAG 2.4.7, 1.4.11, 2.4.11)
- **Regola globale**: `:focus-visible{outline:2px solid var(--ring);outline-offset:4px}` (prima 1px sabbia, 1,88:1 su bianco). `--ring` è per tema: `#8b6e4e` (sabbia scuro, 4,73:1 su bianco) di default; `#d2b99b` (sabbia) su `.hd.is-dark`, `.menu`, `.hero`, `.story__stick`, `.stay__hero`, `.fin`, `.sticky`. Copre tutti i pulsanti, i link, `summary` delle FAQ, frecce e track del carosello, voci del menu a schermo intero, pannello di richiesta e link "Vai al contenuto".
- **Campi del modulo**: la regola `outline:0` è stata tolta (togliendo l'unico indicatore, cioè la linea di 1px che cambiava colore). Ora il campo a fuoco ha l'anello globale 2px e la linea passa a 2px nel colore dell'anello (padding ridotto di 1px: nessuno spostamento). Casella privacy: anello globale.
- **Carosello**: il track (`overflow-x:auto`) tagliava l'anello dei link "Scopri" in basso e a sinistra. Ho aggiunto 6px di padding compensati da margini negativi (`padding-block:6px; margin-block:-6px; padding-left:6px; margin-left:-6px; scroll-padding-left:6px`): posizioni e griglia invariate (prima card a 402,3 px a 1440, 317,5 a 1101, 40 a 701, 24 a 700 e 320: identico al §1, IT/EN/DE, nessun overflow orizzontale a 320, 375, 700, 701, 1100, 1101 e 1440).
- **FAQ**: sul `summary` l'anello è interno (`outline-offset:-2px`) per non invadere la risposta aperta.
- **WCAG 2.4.11 Focus non oscurato**: aggiunto `scroll-padding-top: calc(var(--hh) + var(--u))` (104px; 80px sotto i 700) e `scroll-padding-bottom` 80px sotto i 700px (barra CTA fissa). Prima un elemento raggiunto con Maiusc+Tab o un link a `#faq` finiva sotto l'header fisso (l'eyebrow della FAQ, a 72px, restava coperto dall'header di 96px); ora `#faq` si ferma a 104px dall'alto e l'eyebrow è visibile a 183px.
- Verificato in browser: anello visibile su skip link, header scuro, CTA hero, link telefono, pulsante del pannello 1, link e frecce del carosello, track, pulsante e link della griglia, `summary` aperta, CTA della chiusura, pulsante di chiusura del pannello, campo nome, casella privacy, voce EN nel menu.

### 3.4 Informazione affidata al solo colore (WCAG 1.4.1)
| Caso | Stato prima | Correzione |
|---|---|---|
| Errore dei campi del modulo | solo bordo rosso `#b3402c` 1px | **Corretto.** Linea 2px (contro 1px del campo normale); testo `.fe` sotto il campo con icona "!" in cerchio (CSS, esclusa dai lettori di schermo con `content:"!" / ""`); `aria-invalid="true"`; `aria-describedby="fe-<campo>"` presente solo se il campo è in errore; il `.fe` è `aria-hidden` per non entrare nel nome accessibile dell'etichetta ma resta letto come descrizione. Casella privacy: contorno rosso 2px quando non a fuoco + testo `.fe` + `.bad` e `aria-invalid` (prima non aveva alcun marcatore, solo il messaggio generico). Messaggio generico `#formErr` con la stessa icona. Testi nuovi in IT/EN/DE |
| Lingua attiva nel menu | inversione (sfondo bianco / testo scuro contro trasparente / bianco) + `aria-current` | già a posto (differenza di luminosità, non di tinta). In contrasto forzato: `Highlight` / `HighlightText` |
| Link in blocchi di testo | sottolineati (`.lnk`, `.lnk--w`, `.check a`, `.fin__d a`) | già a posto. Link di navigazione (menu, footer, breadcrumb, social) non sottolineati: sono elenchi, non testo corrente (eccezione 1.4.1) |
| Indicatore del pannello attivo | altezza 44 contro 22px | già a posto |
| Posizione nel carosello | spessore 2px contro 1px + posizione + frecce | già a posto; contrasto portato a 3:1 (vedi tabella) |
| FAQ aperta / chiusa | chevron che ruota + `<details open>` | già a posto (forma, non colore) |
| Campi obbligatori | asterisco nel testo dell'etichetta | già a posto |
| Gerarchia azioni (pieno contro contorno) | forma diversa (riempimento / bordo), etichette diverse | già a posto; visibile anche in acromatopsia |
| Hover dei pulsanti | inversione del riempimento (cambio di luminosità) | non è informazione, solo feedback; il focus ha un indicatore proprio |
| Hover di link/voci di menu | solo cambio colore (`.lnk` → ink, voci menu → sabbia) | non è informazione essenziale; i link restano sottolineati |
| Tag / eyebrow | solo testo | nessun significato affidato al colore |

### 3.5 Simulazione del daltonismo in browser
Filtri SVG `feColorMatrix` (matrici di Machado et al. 2009, gravità 1,0, `linearRGB`) applicati come `filter:url(#…)` ai figli diretti di `<body>` (non a `<html>`: un filtro sul root sposterebbe header e barra CTA fissi). Sezioni riprese a 640×415 (layout mobile): header scuro con hero e pulsante, header chiaro con carosello (barra e frecce), card camera con pulsante contorno e link, FAQ aperta, barra CTA mobile, pannello con errori forzati. Filtri e SVG **rimossi** (ricaricata la pagina); nessuna traccia nei sorgenti (ricerca `__sim`, `feColorMatrix`, `filter:url` nei file del progetto: nessun risultato).

| Simulazione | Esito |
|---|---|
| Protanopia (riprese: hero con header scuro, header chiaro + carosello, card camera, FAQ aperta, barra mobile, pannello con errori) | Nulla scompare. CTA hero piena e leggibile, "Richiedi" nell'header scuro e chiaro con bordo e testo distinti, barra del carosello e frecce visibili, pulsante contorno e link "Scopri" distinguibili (contorno / sottolineatura), chevron FAQ, "Chiama" (contorno) contro "Richiedi" (pieno). Errori del modulo: linea spessa, icona e testo leggibili |
| Deuteranopia (hero, carosello, card camera, pannello con errori) | Nulla scompare. Errori del modulo leggibili grazie a testo + icona (il rosso vira verso l'oliva, ma la linea da 2px e il testo restano) |
| Tritanopia (hero, carosello, pannello con errori in fondo: casella privacy e messaggio generico) | Nulla scompare. Il pieno sabbia vira verso il rosa ma resta ≥ 9:1 con l'etichetta scura; bordo dell'header chiaro visibile; casella in errore con contorno, icona e testo |
| Acromatopsia (hero, carosello, card camera, pannello con errori) | Nulla scompare. Il pulsante pieno diventa grigio pieno contro contorno vuoto (gerarchia conservata), l'indicatore del carosello è una linea più scura e più spessa, l'errore è riconoscibile per spessore, icona e testo |

### 3.6 Preferenze di sistema
- **`prefers-reduced-motion`**: era già coperto per slider (`behavior:auto` in `main.js`), Ken Burns, pannelli, reveal, `scroll-behavior` e pannello di richiesta. Verificato copiando le regole del blocco in un `<style>` incondizionato e leggendo `transition` e `animation` calcolate di tutti gli elementi e pseudo-elementi: restavano con movimento il menu a schermo intero (dissolvenza + scorrimento delle voci), la barra CTA mobile (scorrimento), la freccia delle FAQ (rotazione), l'indicatore del carosello, le linee dell'hamburger e i punti dei pannelli. Aggiunto un secondo blocco in fondo al file (deve stare dopo le regole di base) con `transition:none` su `.menu,.menu__nav a,.sticky,.faq summary::after,.hd__menu span,.bar i,.story__dots li`. Restano solo transizioni di colore/riempimento (`background`, `color`, `border-color`), che non sono movimento.
- **`forced-colors: active`** (blocco in fondo al file): bordi e outline restano (il focus è un outline, il colore lo sceglie il sistema); gli elementi disegnati solo con `background` riprendono colori di sistema (linee dell'hamburger e delle X in `ButtonText`; traccia del carosello, punti dei pannelli, separatore breadcrumb in `GrayText`; indicatore del carosello e punto attivo in `Highlight`); lingua attiva `Highlight`/`HighlightText`; pulsante pieno con bordo 2px per non perdere la gerarchia; testi centrati su foto (`.hero__c, .story__t, .stay__c, .fin__c`) con sfondo `Canvas` e padding 24px, altrimenti il colore forzato del testo poteva coincidere con la foto. **Non provato in un browser con contrasto forzato** (il browser integrato non lo emula): regole controllate solo per sintassi e per posizione nel file.
- **`prefers-contrast: more`** (7 righe, facoltativo): `--line` #6b6b6b (5,33:1), `--sand-t` e `--sand-d` #7a5f40 (5,93:1), testo secondario e `--text` in ink, contorno delle frecce, anello bianco sui fondi scuri, header scuro all'85% di opacità, testi bianchi al posto dell'80% sulle foto. L'aspetto predefinito non cambia. Provato copiando il blocco in un `<style>` incondizionato: valori calcolati corretti.

### 3.7 ELENCO DEGLI SCOSTAMENTI CROMATICI DA ATLANTIS (tutti gli scostamenti di colore/forma introdotti da questo passaggio)
Riferimento misurato in diretta su atlantis.com/dubai/atlantis-the-royal (stili calcolati, non ho provato il focus da tastiera là): i link "Explore" su bianco sono `#424242`, l'indicatore dei carousel è `rgb(194,158,123)` (#c29e7b, 3px) su traccia `#eee` (1,16:1), il pulsante "Book" a contorno ha bordo e testo `#d2b99b` su fondo `rgba(255,255,255,.05)`. Atlantis quindi non rispetta le stesse soglie; il sito parte già da una scelta propria (sabbia scuro `#a98a66` per link e pulsanti sui fondi chiari, traccia `#d9d9d9`) e questo passaggio la sposta il minimo necessario.

| # | Elemento | Prima | Dopo | Soglia raggiunta |
|---|---|---|---|---|
| 1 | Testo `.lnk` (link "Scopri", 14px) | `#a98a66` | `#8b6e4e` | 4,5:1 (4,73) |
| 2 | Testo `.btn-o--dk` | `#a98a66` | `#8b6e4e` | 4,5:1 (4,73) |
| 3 | Bordo `.btn-o--dk` (griglia camere) | `#d2b99b` | `#a98a66` | 3:1 (3,22) |
| 4 | Testo "Richiedi" header chiaro | `#a98a66` | `#8b6e4e` | 4,5:1 (4,53) |
| 5 | Bordo "Richiedi" header chiaro | `#d2b99b` | `#a98a66` | 3:1 (3,09) |
| 6 | Link "privacy" nel modulo | `#a98a66` | `#8b6e4e` | 4,5:1 (4,73) |
| 7 | Sfondo hover pulsante "Invia" (testo bianco) | `#a98a66` | `#8b6e4e` | 4,5:1 (4,73) |
| 8 | Hover link footer e social | `#a98a66` | `#8b6e4e` | 4,5:1 (4,73) |
| 9 | Traccia del carosello (1px) | `#d9d9d9` | `#949494` | 3:1 (3,03) |
| 10 | Indicatore del carosello (2px) | `#a98a66` | `#8b6e4e` | 3:1 (4,73) |
| 11 | Linee dei campi del modulo (1px) | `#c9c9c9` | `#949494` | 3:1 (3,03) |
| 12 | Linea del campo a fuoco | 1px `#a98a66` | 2px `#8b6e4e` + anello | 3:1 (4,73) |
| 13 | Anello di focus su fondi chiari | 1px `#d2b99b` | 2px `#8b6e4e` | 3:1 (4,73) |
| 14 | Anello di focus su fondi scuri | 1px `#d2b99b` | 2px `#d2b99b` (stesso colore) | – (solo spessore) |
| 15 | Errore del campo | linea 1px `#b3402c` | linea 2px `#b3402c` + icona + testo | 1.4.1 (non solo colore) |

Scostamenti non cromatici: anello di focus da 1px a 2px; campo a fuoco con anello (prima nessun anello); comparsa dei testi d'errore per campo; 6px di padding compensato nel track del carosello (nessuno spostamento visibile); `scroll-padding` (i salti a `#faq`, `#camere`, `#offerte` ora lasciano libero l'header: la sezione si ferma 104px sotto il bordo alto). Nessun colore nuovo fuori dalla tinta sabbia già presente, nessun elemento decorativo aggiunto.

Alternative valutate e scartate per restare vicini al riferimento: (a) scurire tutto `--sand-d` a `#8b6e4e` (avrebbe scurito anche bordi e indicatori che già passano); (b) indicatore del carosello nero come in Atlantis (cambia la tinta); (c) alone scuro sui punti dei pannelli (elemento decorativo nuovo).

### 3.8 Nota per il passaggio successivo (euristiche di Nielsen, modulo)
Marcatori lasciati sul modulo: `<span class="fe" id="fe-name|email|checkin|checkout|privacy" aria-hidden="true">` dentro ogni etichetta; visibili via CSS solo quando il campo ha `.bad` **o** `aria-invalid="true"`; `mark(el, invalid, n)` in `main.js` imposta `aria-invalid` e aggiunge/toglie `aria-describedby="fe-<n>"` (solo se in errore, altrimenti il testo verrebbe sempre letto). Se la validazione viene riscritta (es. controllo in tempo reale) basta chiamare `mark()` o replicare i tre attributi; il CSS non cambia. `f.privacy` ora riceve anche `.bad` (l'`querySelector(".bad")` che porta il focus sul primo errore continua a funzionare). Stato `:disabled`: nessuno esiste; se ne viene aggiunto uno, `--line` e `#757575` sono i colori con soglia già verificata.

### 3.9 Non verificato / dubbi
- **Contrasto sulle foto**: le foto sono cross-origin (nessuna intestazione CORS: `fetch` fallisce) e non ho scaricato nulla, quindi non ho potuto rileggere i pixel. Non ri-misurati: testo bianco sui veli (il §2 resta valido, colori e veli non toccati), anello di focus sabbia su foto (stima ≥ 3,7:1 dove il testo bianco fa ≥ 4,75:1), link sabbia `.fin__d a` sul velo finale (stima ≥ 5,3:1), hover `.lnk--w:hover` sabbia sul velo dell'hero (stima 3,1–4:1 nel punto più chiaro: è un cambio di colore dal bianco al sabbia solo in hover e con sottolineatura fissa; se la si vuole a prova di soglia bisogna tenere il bianco e cambiare solo il bordo), punti dei pannelli, bordo `#fefaf5` dei pulsanti su foto.
- **Header scuro sopra le foto**: il valore 9,7:1 è calcolato su #181411. In realtà l'header è `rgba(24,20,17,.32)` con blur sopra la foto: per il pannello 5 (cielo azzurro, il più chiaro visto) la stima è ~5,3:1 per la sabbia; se le foto cambiano e un cielo quasi bianco arriva sotto l'header, il testo sabbia dell'header scende sotto 4,5:1. Rimedio pronto (non applicato perché cambia l'aspetto): alzare l'opacità del velo dell'header da .32 a circa .5.
- **Lettori di schermo reali e dispositivi reali**: non provati. La combinazione `aria-hidden` + `aria-describedby` sul testo d'errore segue le regole ARIA (un elemento nascosto referenziato è usato come descrizione, resta fuori dal nome) ma va controllata con NVDA/JAWS/VoiceOver.
- **Contrasto forzato Windows**: regole scritte ma non provate in un browser che lo emuli; `prefers-reduced-motion` verificato staticamente (regole copiate in un `<style>` incondizionato), non con l'impostazione di sistema.
- Solo Chromium (browser integrato). Controlli nativi (calendario, tendina `select`, bordo della casella non spuntata) seguono il browser.
- Screenshot a bassa risoluzione (il pannello di anteprima è circa 640×415): i controlli visivi sono su porzioni di pagina a 640 e 760 di larghezza (layout mobile e tablet) e a 375; a 1440 e 1100 ho verificato le misure (nessun overflow, colonne del §1 invariate) ma non con screenshot di dettaglio. Simulazioni del daltonismo fatte solo su layout 640×415.
- **Fuori dal compito, da segnalare**: nel menu a schermo intero a viewport molto bassi (circa ≤ 450px di altezza, visto a 640×415) l'ultima voce ("Domande frequenti") si sovrappone al piede con lingue e contatti: è un problema di reflow (1.4.10) da rivedere nel passaggio di layout, non di colore.
- Rapporto 4,61:1 del grigio secondario `#757575` (12px) con margine di 0,11 sopra la soglia: se le etichette diventano più chiare va rimisurato.

## 4. Euristiche di Nielsen

Ambito: `src/template.html`, `src/assets/style.css`, `src/assets/main.js`, `src/lang/{it,en,de}.json` (+18 chiavi in tutte e tre le lingue, −1 chiave `form_err_field` non più usata: 212 chiavi ciascuna, insiemi identici; 3 ritocchi di testo, vedi 4.5). Nessun file scaricato su disco, nessun nuovo colore, font o componente. Build rigenerata (`site/index.html`, `site/en/`, `site/de/`; `site/assets/*` identici ai sorgenti). Priorità: aderenza ad Atlantis, quindi correzioni invisibili (attributi, testo per screen reader, stati) e, dove visibili, minime e nel linguaggio già presente (vedi 4.3).

Metodo: browser integrato (Chromium) con **eventi di tastiera reali** (Tab, Maiusc+Tab, Invio, Spazio, Esc, frecce; anche digitazione nei campi e nei segmenti della data). IT a 1440×900 (percorsi completi, carosello, ordine di Tab), 1100×800 (overflow, frecce), 375×812 (pannello, errori, stato), 640×415 e 812×375 (menu); EN (menu → voce → chiusura, audit dei link); DE (messaggi, tracciamento, errori di validazione). Audit DOM sulla pagina IT: nessun id duplicato, nessun `aria-labelledby/describedby/controls` orfano, nessun link o pulsante senza nome. Il collegamento `mailto:` è stato **intercettato** con l'evento `navigate` della Navigation API (`preventDefault`): l'URL è stato letto e verificato, ma nessun programma di posta reale è stato aperto. Viewport ripristinato a "desktop" a fine prove.

### 4.0 Bug bloccante trovato (severità 4, preesistente)
In `main.js` la funzione di misurazione `track()` veniva **sovrascritta** da `var track = $("#track")` del carosello (stesso nome nello stesso scope). Conseguenze nel codice ricevuto: (a) ogni clic su un elemento `[data-cta]` lanciava `TypeError: track is not a function`; (b) **ogni invio valido del modulo lanciava lo stesso errore prima di `window.location.href = "mailto:…"`: il programma di posta non si apriva mai** e l'utente non vedeva nulla; (c) nessun evento `cta_click` / `generate_lead` arrivava al `dataLayer`. Non emergeva perché gli altri gestori di clic (apertura del pannello, menu) sono listener separati. Corretto rinominando l'elemento del carosello in `rail`. Verificato: `cta_click` e `generate_lead` nel `dataLayer`, mailto generato (oggetto, campi, `%0D%0A`).

### 4.1 Valutazione per euristica

Severità: 0 nessun problema · 1 cosmetico · 2 minore · 3 maggiore · 4 catastrofico. "Prima" = codice ricevuto.

| # | Euristica | Problema trovato | Sev. prima | Correzione applicata / motivo di non applicazione | Sev. dopo |
|---|---|---|---|---|---|
| 1 | Stato del sistema | Invio valido senza alcun effetto (bug 4.0) e, anche corretto, nessuna conferma: il modulo apre solo il programma di posta e chi non ne ha uno non sa se è successo qualcosa | 4 | Stato "Richiesta preparata" nello stesso pannello, sotto il pulsante: paragrafo `role="status"` sempre nel DOM (vuoto) con testo inserito dopo 80 ms, così viene annunciato; riga "Non si è aperto nulla? Apri di nuovo la richiesta" con vero link `mailto:` aggiornato ai dati correnti; contatti sempre visibili (4.6). Il testo dice la verità: la richiesta arriva solo dopo l'invio dal programma di posta. Nessuna promessa di tempi | 0 |
| 1 | Stato del sistema | Errori del modulo che restavano dopo la correzione (si ripulivano solo al successivo invio) | 2 | Un campo segnalato si ripulisce appena è valido (mentre si scrive) e il messaggio si rivaluta a fine modifica; il riepilogo `role="alert"` si nasconde quando non resta nessun errore. Qualsiasi modifica dopo l'invio toglie lo stato "preparata" (non descriverebbe più il modulo) | 0 |
| 1 | Stato del sistema | Fine corsa del carosello non segnalata (frecce sempre uguali); posizione dei pannelli solo visiva | 2 | Frecce `aria-disabled` a inizio/fine (glifo attenuato, vedi 4.3); `aria-controls="track"`; il track è `role="region"` con nome dal titolo della sezione; pannello attivo con `aria-current="true"`; `aria-haspopup="dialog"` sui pulsanti che aprono il pannello. `aria-expanded` sull'hamburger e `aria-current` sulla lingua c'erano già; aggiunto `aria-current="page"` all'ultimo elemento del breadcrumb | 0 |
| 2 | Sistema e mondo reale | EN: "Check availability" promette una verifica in tempo reale, ma il pulsante apre una richiesta (IT: "Richiedi disponibilità", DE: "Verfügbarkeit anfragen") | 2 | `book_avail` EN → "Request availability" (stesso significato di IT e DE) | 0 |
| 2 | Sistema e mondo reale | 34 dei 37 link esterni (tutti quelli verso hotelbelfiore.it, tranne il link privacy del modulo; Instagram e Facebook già in nuova scheda) si aprivano nella stessa scheda senza avviso; i 17 "Scopri" / 4 "Richiedi disponibilità" non dicevano a cosa si riferivano | 2 | Tutti i 37 link esterni: `target="_blank" rel="noopener"` + `<span class="sr">(si apre in una nuova scheda)</span>` (nuova chiave `newtab`; verificato su EN: 37/37 con target, rel e testo nascosto, 0 link interni con target; il template è lo stesso per IT e DE). I "Scopri" e i 4 pulsanti della griglia hanno anche il titolo della card come testo nascosto ("Scopri: Skibus gratuito (si apre…)"). Link ancora interni (`#camere`, `#offerte`, `#faq`, `#top`, `#main`) restano nella stessa scheda. `tel:`, `mailto:` e cambio lingua invariati. Aspetto identico (110×40, 206×40, 231×52, 43,5×23 misurati come nel §2). Il tracciamento ignora il testo nascosto. Atlantis apre in nuova scheda i suoi link verso altri siti | 0 |
| 2 | Sistema e mondo reale | Stessa etichetta con destinazioni diverse: menu "Camere" / "Offerte & Special weeks" scorrono nella pagina, le voci omonime del footer escono dal sito | 1 | Non applicata (cambiare la dicitura altera i testi della reference). Il testo nascosto "(si apre in una nuova scheda)" le distingue per gli screen reader | 1 |
| 2 | Sistema e mondo reale | Campo data nella lingua del browser, non della pagina (controllo nativo) | 1 | Non applicata (servirebbe un calendario personalizzato = nuovo componente) | 1 |
| 3 | Controllo e libertà | **Il focus non entrava nel menu** (`mX.focus()` cadeva mentre `visibility` era ancora in transizione: restava sull'hamburger) e Tab scorreva su logo e "Richiedi" nascosti sotto l'overlay: nessuna trappola nel menu | 3 | Focus su "Chiudi" all'apertura (ritenta finché l'elemento è visibile), Tab e Maiusc+Tab restano dentro le 14 voci (giro completo in entrambe le direzioni), resto della pagina `inert`, Esc e clic sullo sfondo vuoto chiudono, il focus torna all'hamburger. Con una voce interna (`#faq`, `#camere`) il menu si chiude e il focus resta sulla destinazione (Tab successivo → prima domanda); con un link in nuova scheda il menu resta aperto | 0 |
| 3 | Controllo e libertà | Per costruzione del vecchio controllo (`e.target === dlg`) un clic sull'area bianca del pannello sotto il modulo (viewport alti: modulo 937px, pannello 100dvh, es. 1080px) chiudeva il pannello, come il rilascio fuori dopo una selezione di testo (non riprodotto in un viewport alto: a 900px il modulo riempie il pannello) | 2 | Il clic sullo sfondo chiude solo se pressione e rilascio sono fuori dal rettangolo del pannello | 0 |
| 3 | Controllo e libertà | Verificato senza modifiche: Esc chiude pannello e menu; il focus torna al pulsante che ha aperto il pannello (`Richiedi` dell'header); dati e stato conservati chiudendo e riaprendo; nessun avanzamento automatico (nessun `setInterval`; carosello e pannelli si muovono solo con lo scorrimento o i comandi dell'utente) | 0 | – | 0 |
| 3 | Controllo e libertà | Animazioni all'avvio senza pausa: zoom dell'hero (28 s) e filo "scorri" | 1 | Non applicata (un controllo di pausa è un nuovo elemento). Entrambe si fermano con `prefers-reduced-motion` | 1 |
| 4 | Coerenza e standard | DE: "Belfiore, **dein** Platz des Herzens" nel pannello 1, tutto il resto in "Sie" | 2 | `pn1_h` DE → "Belfiore, Ihr Platz des Herzens" | 0 |
| 4 | Coerenza e standard | DE: footer "Privacy Policy" per la stessa pagina che nel modulo è "Datenschutzerklärung" | 1 | `ft_privacy` DE → "Datenschutzerklärung" ("Cookie Policy" invariato) | 0 |
| 4 | Coerenza e standard | "Scopri" è pulsante a contorno nei pannelli e link sottolineato in carosello, camere, griglia; "Richiedi" (header) contro "Richiesta" (footer) in IT | 1 | Non applicate: schema identico ad Atlantis (Explore come pulsante nei pannelli e link nelle card). Gerarchia azioni invariata (§2). Un solo verbo per l'azione principale in tutte e tre le lingue dopo la correzione EN | 1 |
| 5 | Prevenzione errori | Partenza precedente all'arrivo accettata all'invio (solo `min` sull'input, non controllato); e la partenza veniva **sovrascritta in silenzio** con la data di arrivo cambiando l'arrivo | 3 | Validazione all'invio (messaggio dedicato); la partenza non viene più modificata: se l'arrivo cambia e la partenza diventa precedente, il campo si segnala subito. `min` della partenza = arrivo | 0 |
| 5 | Prevenzione errori | Adulti / bambini fuori intervallo (99, −1) accettati; date passate o con anni digitati per errore accettate; `min` "oggi" calcolato in UTC (sbagliato per fuso orario) | 2 | Adulti 1–10 e bambini 0–8 validati (limiti già presenti nei `min`/`max`); arrivo non nel passato; date entro 3 anni da oggi (`max`; parametro tecnico mio: da confermare con l'hotel); "oggi" in data locale. `inputmode` tel / email / numeric, `autocomplete="off"` su date e numeri, `autocapitalize`/`spellcheck` off sull'email | 0 |
| 5 | Prevenzione errori | Doppio invio riapriva il programma di posta più volte; messaggio lungo troncato in silenzio dai client (limite ~2000 caratteri dell'URL `mailto:` su Windows); righe con `\n` | 2 | Stessa richiesta entro 8 s: non riapre e non ripete `generate_lead`, mostra "Richiesta già preparata". Messaggio `maxlength="800"` (nessun contatore visibile). A capo `%0D%0A`. Bambini vuoti → "0" nella mail | 1 |
| 5 | Prevenzione errori | `required` con `novalidate` | 0 | Verificato: il browser non blocca, la validazione è tutta custom e i messaggi sono leggibili | 0 |
| 6 | Riconoscere anziché ricordare | Etichette sempre visibili (nessun segnaposto al posto dell'etichetta; solo l'area Messaggio ha un esempio), tutte le voci del menu visibili, ordine dei campi logico (nome, telefono, email, date, ospiti, camera, messaggio, privacy) | 0 | – | 0 |
| 6 | Riconoscere anziché ricordare | Asterisco "*" non spiegato; adulti e privacy obbligatori senza asterisco; riepilogo dei dati prima/dopo l'invio | 1 | Riepilogo: il modulo resta compilato e visibile sotto lo stato "preparata" (i dati non si perdono), quindi nessun blocco nuovo. Legenda "* obbligatorio": non applicata (testo nuovo nel pannello) | 1 |
| 7 | Flessibilità ed efficienza | "Vai al contenuto" spostava lo scroll ma `<main>` non era focalizzabile | 2 | `<main id="main" tabindex="-1">` + `#main:focus{outline:0}` (una cornice attorno a tutta la pagina sarebbe peggio). Verificato: Tab → "Vai al contenuto" → Invio → focus su `main` → Tab → CTA dell'hero (salta l'header) | 0 |
| 7 | Flessibilità ed efficienza | **Pannelli 2–6 a scorrimento invisibili agli screen reader** (`visibility:hidden`: nell'albero esisteva solo il primo) | 3 | Pannelli non attivi trasparenti e non cliccabili invece di `visibility:hidden` (look identico); i loro link hanno `tabindex="-1"` gestito dal JS (Tab arriva solo al link del pannello attivo); se il pannello che perde il focus cambia, il focus viene tolto (prima restava su un elemento nascosto). Verificato: tutti e sei i titoli nell'albero, Tab dopo lo scorrimento porta al link del pannello corrente | 0 |
| 7 | Flessibilità ed efficienza | Frecce del carosello: più pressioni ravvicinate si perdevano (8 pressioni = 1 card, `scrollBy` da posizione intermedia) | 2 | Il punto di arrivo parte dal traguardo precedente: 8 pressioni arrivano a fine corsa (1361 px). Frecce sinistra/destra sul track focalizzato già presenti | 0 |
| 7 | Flessibilità ed efficienza | Accesso rapido a telefono / email | 1 | Già in hero, barra mobile, sezione finale, footer e menu; ora anche nel pannello del modulo | 0 |
| 7 | Flessibilità ed efficienza | Da tastiera non si raggiunge con Tab il link dei pannelli 2–6 senza scorrere (Tab salta i 5 pannelli dopo il primo) | 1 | Non applicata: è il comportamento della sezione a scorrimento come in Atlantis; le stesse pagine sono nel menu e nel footer | 1 |
| 8 | Design estetico e minimalista | Testi lunghi nelle card (carosello e griglia: fino a ~30 parole ciascuna), `sp_p` identico a `req_lead` ("Per informazioni, richieste e proposte…") generico, due azioni per card nella griglia, chiave `hero_p2` non usata | 1 | Solo annotazione (regola: nessun elemento della reference va rimosso) | 1 |
| 9 | Diagnosi e correzione errori | Un solo messaggio generico ("Compila o correggi questo campo") per tutti i casi | 2 | Messaggio specifico per caso in IT/EN/DE: nome, email vuota / formato, arrivo mancante / già passato / da controllare, partenza mancante / **precedente all'arrivo** / da controllare, adulti, bambini, privacy. Il messaggio corretto va al posto giusto (`aria-describedby`, `aria-invalid` invariati dal §3). Focus sul primo campo in errore (verificato: nome, poi email, poi partenza). Un solo `role="alert"` (riepilogo), visibile a ogni invio con errori ma annunciato solo alla comparsa: il dettaglio è letto dal campo a fuoco | 0 |
| 10 | Aiuto e documentazione | Nessun contatto nel pannello del modulo; cosa succede dopo l'invio detto solo dal microtesto | 3 | Riga "Preferisci parlare con noi? Chiama o scrivi: telefono · email" (testo già esistente `fin_d`) sotto il microtesto; pulsante "Invia" collegato al microtesto con `aria-describedby`; il dialog ha `aria-describedby` sul testo introduttivo; stato "preparata" con riserva `mailto`. FAQ raggiungibile dal menu ("Domande frequenti") ed esiste la domanda "Come richiedo una proposta di soggiorno?" | 0 |
| – | Fuori dai 10 punti | Menu a schermo intero a ≤~450px di altezza: ultima voce sovrapposta al piede; X di chiusura coperta dalle voci | 2 | Sotto 600px di altezza `.menu__body` diventa una colonna scorrevole sotto la X, con il piede di seguito. Sopra 600px `.menu__body` è `display:contents`: geometria identica (1440×900: voci 238–662, piede 822–860, come prima). Provato a 640×415 e 812×375: nessuna sovrapposizione, ultima voce raggiungibile da tastiera | 0 |

### 4.2 Esito dei test da tastiera

**Percorso 1 — "apri pannello → compila → errore → correggi → invia" (IT, 1440×900). Tasti e digitazione reali; per abbreviare, in alcuni passaggi il focus è stato portato via script su date, casella privacy e pulsante "Invia" prima di premere Spazio / Invio / digitare.**
1. Focus su "Richiedi" (header) → Invio: il pannello si apre e il focus va su "Chiudi" (`reqX`). Tab → Nome. ✔
2. Invio sul pulsante "Invia la richiesta" con tutto vuoto: focus sul primo campo in errore (Nome); compaiono 5 messaggi specifici (nome, email, arrivo, partenza, privacy) e il riepilogo; 5 campi con `aria-invalid="true"` e `aria-describedby`. ✔
3. Digitato "Mario Rossi": errore del nome sparito subito (`aria-invalid="false"`, `aria-describedby` rimosso), il riepilogo resta perché altri campi sono ancora in errore. ✔
4. Email digitata con Tab; date digitate con tasti veri (segmenti mese/giorno/anno); partenza (10/12) precedente all'arrivo (20/12); Spazio sulla casella privacy. Invio su "Invia": nessun `mailto:`, focus sul campo Partenza, messaggio "La data di partenza è precedente all'arrivo". ✔
5. Digitata partenza corretta (27/12): errore e riepilogo spariti da soli. ✔
6. Invio: `mailto:info@hotelbelfiore.it?subject=…&body=Nome…%0D%0A…` generato (intercettato), `generate_lead` nel `dataLayer`, stato "Richiesta preparata." con la riga "Non si è aperto nulla? Apri di nuovo la richiesta" (il link ha lo stesso URL); il focus resta sul pulsante. ✔
7. Secondo Invio subito dopo (<8 s): nessun secondo `mailto`, messaggio "Richiesta già preparata". Modificando un campo lo stato sparisce. ✔
8. Esc: il pannello si chiude e il focus torna a "Richiedi" nell'header; riaperto, nome, email, date, messaggio e privacy sono ancora al loro posto. ✔ (clic sullo sfondo: chiude; clic sull'area bianca del pannello o rilascio fuori dopo una selezione: non chiude — simulato con eventi puntatore sintetici.)

**Percorso 2 — "menu → voce → chiudi".**
IT: hamburger (Invio) → focus su "Chiudi"; Tab ×14 percorre le voci e ricomincia da "Chiudi"; Maiusc+Tab da "Chiudi" va su "info@hotelbelfiore.it". Su "Domande frequenti" Invio: menu chiuso, `#faq` a 104 px dal bordo (nessuna copertura da parte dell'header), Tab → prima domanda. Esc dal menu: focus sull'hamburger, `aria-expanded="false"`, pagina non più `inert`. Link in nuova scheda: menu resta aperto; clic sullo sfondo vuoto: chiude e il focus torna all'hamburger. EN: hamburger → "Our rooms" → Invio → `#camere` a 104 px, menu chiuso. ✔

**Altro:** Tab dalla cima (IT): salto → menu → logo → Richiedi → CTA hero → telefono → "Scopri" del pannello attivo → frecce del carosello → track → 6 "Scopri" → CTA camere → 4+4 azioni griglia → 6 domande → CTA finale → tel → email → footer. Nessun elemento invisibile a fuoco (le opacità < 1 registrate sono il reveal in corso durante Tab molto rapidi). Carosello: 8× Freccia destra → fine corsa, freccia "Successivo" attenuata e `aria-disabled="true"`, focus rimasto sul track. Nessuna trappola: in nessun punto Tab o Maiusc+Tab restano bloccati.

### 4.3 SCOSTAMENTI VISIVI DA ATLANTIS introdotti in questo passaggio
Nessun colore, font, dimensione o componente nuovo; tutti i valori sono token già presenti. Elenco completo:

| # | Dove | Valore | Motivo |
|---|---|---|---|
| 1 | Frecce del carosello a inizio/fine corsa (visibile già al caricamento: la freccia "Precedente" è attenuata) | glifo a `opacity .35`, contorno `#d9d9d9` → `#ececec`; nessun effetto hover | Atlantis nasconde la freccia (`disabled`, opacity 0). Qui si attenua il solo glifo: con `aria-disabled` il focus da tastiera non si perde e l'anello di focus resta a piena opacità |
| 2 | Pannello di richiesta, dopo un invio valido | blocco sotto il pulsante: titolo peso 500 e testo 13px/20px `#212121` centrati (stessa scala del messaggio d'errore, ma in inchiostro); riga 12px/16px `#757575` con link `--sand-t` sottolineato | Necessario: il modulo non ha server e senza stato non c'è conferma. Compare solo dopo l'invio |
| 3 | Pannello di richiesta, sempre | riga di contatti sotto il microtesto: 12px `#757575`, link `--sand-t` sottolineati (come `.check a`, 4,73:1); distanza 8px dal microtesto. Il messaggio d'errore generale passa da sotto il microtesto a subito sotto il pulsante (margine 12px) | Contatti per chi non ha un programma di posta; errore vicino all'azione |
| 4 | Pannello, solo in errore | messaggi `.fe` anche su adulti e bambini (colonne strette: 2–3 righe a 12px) | Prima quei campi non avevano messaggio |
| 5 | Menu a schermo intero, solo a ≤600px di altezza | colonna scorrevole con barra sottile (`scrollbar-width:thin`, bianco 40%), piede di seguito alle voci | Reflow a viewport bassi. A >600px: nessuna differenza |
| 6 | Testi | EN "Request availability" (era "Check availability"); DE "Ihr Platz des Herzens", "Datenschutzerklärung"; nuovi messaggi d'errore e stato | Vedi 4.1. Non cambiano i fatti |
| 7 | Comportamento | menu aperto dopo un link in nuova scheda; `<main>` senza cornice di focus; pannelli non attivi trasparenti invece di `visibility:hidden` | Invisibili all'occhio |

Misure di controllo dopo le modifiche (IT 1440): "Scopri" dei pannelli 109,5×40, link 43,5×23, pulsanti griglia 205,8×40, CTA camere 231,1×52, "Richiedi" header 167,8×48; nessun overflow orizzontale a 1440, 1100 (1085/1085), 375 (375/375) e nel pannello.

### 4.4 Raccomandazioni non applicate (cambierebbero il design della reference o servono decisioni dell'hotel)
- **Invio reale (endpoint o servizio di moduli)**: eliminerebbe la dipendenza dal programma di posta (molti utenti desktop usano solo webmail) e darebbe una conferma vera. Nel codice c'è già il punto di sostituzione (commento sopra il submit). In alternativa, un pulsante "Copia la richiesta" per il webmail: nuovo elemento.
- **Pausa per le animazioni automatiche** (zoom dell'hero da 28 s, filo "scorri"): WCAG 2.2.2 la chiede oltre i 5 s; oggi si fermano solo con `prefers-reduced-motion`.
- **Etichette omonime con destinazioni diverse** (menu / footer) e **"Scopri" pulsante vs link**: sono lo schema della reference.
- **Legenda "* campo obbligatorio"** e asterisco su adulti / privacy: testo nuovo nel pannello.
- **Blocco dello scorrimento dietro il pannello** (il menu lo fa): farebbe sparire la barra di scorrimento dietro il velo (scatto di 15 px).
- **Calendario nella lingua della pagina**: richiede un componente personalizzato.
- **Privacy e cookie** puntano alle pagine italiane di hotelbelfiore.it anche da EN/DE; lo stesso per tutti i link "Scopri" verso pagine che esistono anche in lingua (sono già nelle chiavi `u_*` per lingua, ma privacy/cookie no).
- **Densità dei testi** (punto 8) e chiave `hero_p2` inutilizzata: solo annotazioni.
- **Perdita dei dati cambiando lingua** nel menu (la pagina si ricarica): servirebbe una bozza in `sessionStorage`.
- Massimo **3 anni** per le date e **800 caratteri** per il messaggio sono parametri tecnici scelti da me: da confermare con l'hotel.

### 4.5 Modifiche ai testi (nessun fatto cambiato)
EN `book_avail` "Check availability" → "Request availability"; DE `pn1_h` "…dein Platz des Herzens" → "…Ihr Platz des Herzens"; DE `ft_privacy` "Privacy Policy" → "Datenschutzerklärung". Nuove chiavi (IT/EN/DE): `newtab`, `e_name`, `e_email0`, `e_email1`, `e_in0`, `e_in1`, `e_in2`, `e_out0`, `e_out1`, `e_out2`, `e_adults`, `e_kids`, `form_ok_t`, `form_ok_m`, `form_ok2_t`, `form_ok2_m`, `form_ok_alt`, `form_ok_retry`. Rimossa: `form_err_field` (sostituita dai messaggi per caso; il §3 la citava).

### 4.6 Nota per chi tocca di nuovo il modulo
Le stringhe dei messaggi arrivano al JS da `window.BF.err` e `window.BF.ok`, riempiti nel template con i segnaposto `{{e_*}}` e `{{form_ok*}}` (nessuna modifica a `build.ps1`). Ogni campo passa da `check(nome)` (imposta `.bad`, testo `.fe`, `aria-invalid`/`aria-describedby` con `mark()`); lo stato "preparata" usa `okShow/okClear` e i tre nodi `#reqOk`, `#reqOkMsg` (`role="status"`), `#reqOkAlt`. Se si passa a un invio con `fetch()`, basta chiamare `okShow` con altri testi e togliere `window.location.href`.

### 4.7 Cosa NON ho potuto verificare
- **Screen reader reali** (NVDA, JAWS, VoiceOver, TalkBack): non provati. Da controllare in particolare l'annuncio del paragrafo `role="status"` (testo inserito 80 ms dopo), la combinazione `aria-hidden` + `aria-describedby` sui messaggi `.fe` (già segnalata nel §3), i nomi dei link ("Scopri: titolo (si apre in una nuova scheda)"), l'uso di `aria-current` su un `div` (pannello attivo) e i pannelli non attivi ora presenti nell'albero.
- **Dispositivi reali e altri browser**: solo Chromium. `inert`, `<dialog>`, `text-wrap`, campi data e tastiere `inputmode` non provati su Safari / iOS / Firefox / Android; in browser senza `inert` resta comunque il blocco di Tab nel menu.
- **Programma di posta reale**: il `mailto:` è stato intercettato; non ho potuto verificare come reagiscono client e sistemi senza un gestore registrato (Chrome / Windows possono non mostrare nulla: per questo esistono lo stato, il link di riserva e i contatti), né il troncamento dei messaggi lunghi (limite di circa 2000 caratteri dell'URL su Windows, da documentazione).
- **Contrasto forzato Windows e `prefers-contrast`**: nessuna nuova regola per i blocchi aggiunti oltre l'inclusione di `.req__oka` nell'elenco del testo secondario.
- **Screenshot**: il pannello di anteprima ha una risoluzione bassa (circa 800 px); le verifiche visive sono misure del DOM (rettangoli, opacità, valori calcolati) più screenshot d'insieme a 1440, 1100, 812×375, 640×415 e 375.
- **Senza JavaScript**: i link dei pannelli 2–6 hanno `tabindex="-1"` già nell'HTML (come prima, quando erano `visibility:hidden`, non erano raggiungibili con Tab); il resto della pagina non dipende da queste modifiche.
