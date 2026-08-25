# Sul Posto — istruzioni per chi ci lavora (anche per l'IA)

App web per registrare gli interventi dal cliente: **Arrivo** / **Finito**, calcolo di
manodopera, viaggi e diritto di chiamata. Autore: **Maximiliano Marchesi — MXM**.

- Online (versione usata sul telefono): https://maximilianred.github.io/sul-posto/
- Copia secondaria: https://sulposto.netlify.app/

## Come è fatta

Un **unico file `index.html`**: HTML, CSS e JavaScript puro, nessuna libreria, nessun
processo di build. Si apre com'è, funziona offline.

I dati stanno nel `localStorage` del browser, con queste chiavi:

| chiave | contenuto |
|---|---|
| `segnaore_jobs` | tutti i lavori registrati |
| `segnaore_cfg` | impostazioni (tariffe, sede, chiave TomTom) |
| `segnaore_active` | il lavoro in corso, se c'è |

⚠️ **Non rinominare queste chiavi**: il telefono ha già dei dati salvati con questi nomi.
Stesso motivo per cui **non si cambia l'indirizzo del sito** senza prima fare un backup
dall'app (⚙ → 💾 Scarica backup) e ripristinarlo sul nuovo indirizzo: il `localStorage`
è legato all'indirizzo, cambiarlo fa sparire lo storico.

Impostazioni predefinite (`DEFAULTS`): 60 €/h, IVA 22%, arrotondamento 15 min,
tariffa viaggio 60 €/h, velocità media 50 km/h, diritto di chiamata 20 € netti.
`cfg` ha una chiave `v`: **non alzarla senza motivo**, azzera le impostazioni salvate
(sede compresa).

## Le regole d'oro (decise dall'utente, non cambiarle)

1. **Un solo arrotondamento, alla fine.** I minuti dei viaggi restano **grezzi**; lo
   scaglione di 15 minuti si applica **una volta sola** sul tempo totale
   (viaggi + lavoro). Mai arrotondare le singole voci: *"senno faccio 3 arrotondamenti"*.
2. **Nessuna voce «Arrotondamento» nel riepilogo**, e nessun riempimento spalmato dentro
   andata/ritorno/lavoro. Ogni riga mostra **solo i suoi valori reali**; la differenza si
   legge nella riga del tempo fatturato. Che le righe non sommino esattamente
   all'imponibile **è voluto**. (Provato in v1.20 e v1.21: bocciato entrambe le volte.)
3. **Ogni pubblicazione alza il numero di versione** in fondo alla pagina
   (`Sul Posto · v1.N · GG/MM/AAAA · MXM`). Serve all'utente per capire se il telefono
   si è aggiornato. N = numero di commit di quella versione.
4. **La firma è MXM**: va tenuta nel piè di pagina e nel README.
5. **Niente riga `Co-Authored-By: Claude` nei commit.** L'uso dell'IA è dichiarato, ma
   **in prima persona dall'autore**, nella sezione «Trasparenza» del README: deve essere
   chiaro che è una scelta consapevole di chi firma il progetto, non un timbro automatico.
6. **Nessuna chiave privata dentro il codice.** La chiave TomTom si incolla nelle
   ⚙ Impostazioni e resta nel dispositivo dell'utente. Nel repo pubblico c'è solo la
   casella vuota.

## Come si calcola il conto

```
minuti viaggi   = minAndata + minRitorno          (grezzi, come registrati)
minuti fatturati = arrotonda(lavoro + viaggi)      (un solo arrotondamento, a 15 min)
costo lavoro    = (minuti fatturati − minuti viaggi) / 60 × tariffa   (mai negativo)
imponibile      = diritto di chiamata + costo lavoro + costo viaggi
totale          = imponibile + IVA
```

Dopo ogni modifica manuale si richiama `ricalcolaJob(j)`, che rifà lo stesso conto.
Caso legittimo: **arrivo e fine alla stessa ora** (cliente assente: solo chiamata e
viaggi, zero lavoro). Si blocca il salvataggio **solo** se la fine precede l'arrivo.

## I viaggi

Il GPS viene letto all'Arrivo. Il percorso si calcola in quest'ordine:

1. **TomTom** col traffico reale — solo se l'utente ha messo la sua chiave in ⚙
2. **OpenStreetMap / OSRM** — percorso stradale vero, ma senza traffico (8 secondi di attesa)
3. **Stima** — distanza in linea d'aria × 1,3 alla velocità media

Sulla scheda si legge quale è stato usato: `🗺 percorso reale`, `≈ stima`, oppure
`✍ corretto a mano` se l'utente ha sistemato i minuti in ✏ Modifica (le mappe gratuite
sbagliano: una volta hanno detto 47 minuti per un tragitto cronometrato in 27).

In ✏ Modifica si possono correggere a mano **minuti di andata, di ritorno e km**, e anche
cambiare **l'indirizzo di partenza di quel singolo lavoro** senza toccare la sede
predefinita degli altri.

## Provare le modifiche

```bash
python -m http.server 8321 --directory sul-posto
```
Poi apri http://localhost:8321. Per provare i viaggi senza muoversi si possono
simulare GPS e risposte di internet dalla console del browser.

## Pubblicare

`git push` → **GitHub Pages** aggiorna il sito in circa 30 secondi (nessun limite).
La copia Netlify si aggiorna sola, ma il piano gratuito concede **20 pubblicazioni al
mese** (si rinnovano il 21): oltre quel numero resta ferma finché non riparte il mese,
quindi meglio **raggruppare le modifiche** invece di pubblicare a ogni virgola.

## Cose discusse ma non ancora fatte

Pausa pranzo da scalare, costo dei materiali, elenco clienti abituali, note per
lavorazione, totale mensile, sincronizzazione tra telefono e PC (servirebbe un server).
