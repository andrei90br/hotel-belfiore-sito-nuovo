# Hotel Belfiore — sito nuovo

Landing page statica in tre lingue (italiano, inglese, tedesco) per l'Hotel Belfiore, Dimaro Folgarida (Val di Sole).
Testi e foto sono quelli del sito attuale (hotelbelfiore.it); il layout segue quello di riferimento di
atlantis.com/dubai/atlantis-the-royal (solo studio di struttura e misure: non è incluso alcun asset di Atlantis).

## Struttura

```
src/template.html      template unico con segnaposto {{chiave}}
src/lang/*.json        testi in it / en / de (stesse chiavi)
src/assets/            style.css, main.js
build.ps1              genera site/ (IT in radice, /en/, /de/)
serve.ps1              anteprima locale su http://localhost:5173/
QA-report.md           audit su grid, gerarchia visiva, accessibilità (daltonismo) e euristiche di Nielsen
.github/workflows/     deploy su GitHub Pages
```

## Comandi (Windows PowerShell)

```powershell
# genera il sito
powershell -ExecutionPolicy Bypass -File build.ps1

# anteprima locale
powershell -ExecutionPolicy Bypass -File serve.ps1
```

Parametri di `build.ps1`: `-Site` (URL finale per canonical e hreflang), `-Img` e `-Media` (base delle immagini),
`-NoIndex` (aggiunge `noindex`, usato dal deploy di anteprima).

## Deploy

Ogni push su `main` o `sito-nuovo` avvia il workflow che rigenera il sito e lo pubblica su GitHub Pages
(`https://<utente>.github.io/<repository>/`). L'anteprima è marcata `noindex`.

## Prima del go-live

- Le foto sono collegate direttamente da hotelbelfiore.it: vanno scaricate, ottimizzate e servite dal nuovo dominio
  (cambiare `-Img` / `-Media` in `build.ps1`).
- Il modulo di richiesta apre il programma di posta con `mailto:`. Per un invio vero serve un endpoint (es. Formspree).
- Cambiare `-Site` con il dominio definitivo e togliere `-NoIndex`.
- Confermare con l'hotel i testi con affermazioni verificabili (150+ km di piste, navetta gratuita, inclusioni del benessere).
