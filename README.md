# 📍 Sul Posto

App web per chi lavora **sul posto, dal cliente**: registra le ore di lavoro, i viaggi di andata e ritorno, e calcola al volo il **costo della manodopera** (imponibile, IVA e totale).

Arrivi e premi **Arrivo**, finisci e premi **Finito** — l'app fa i conti da sola.

## 🔗 Demo dal vivo
👉 **https://maximilianred.github.io/sul-posto/**

## ✨ Funzionalità
- ▶️ Cronometro **Arrivo / Finito** con un tocco
- 🚗🏠 **Viaggi di andata e ritorno** calcolati sul **percorso stradale reale** (OpenStreetMap/OSRM), con stima automatica se sei offline
- 🔎 Sede impostabile **cercando l'indirizzo** o col GPS
- 💶 Calcolo automatico di **imponibile, IVA e totale** (tariffe e IVA impostabili)
- ⏲️ **Arrotondamento** del tempo a piacere (es. a 15 minuti)
- 📋 **Storico** con le voci separate — andata, lavoro, ritorno — ognuna con tempo e costo
- 📤 **Esportazione in CSV** per Excel / contabilità
- 💾 **Backup e ripristino** dei dati con un file
- 📱 Funziona **offline**, senza account; si aggiunge alla **schermata Home** come un'app

## 🛠️ Tecnologie
- **HTML, CSS e JavaScript** puro (vanilla), in **un solo file** `index.html`
- `localStorage` del browser per salvare i dati sul dispositivo
- Geolocalizzazione del browser + **OSRM** (percorsi stradali) e **Nominatim** (ricerca indirizzi) di OpenStreetMap
- Nessuna libreria esterna, nessun processo di build

## 🚀 Come si usa
1. Apri la [demo dal vivo](https://maximilianred.github.io/sul-posto/)
2. (Da telefono) menu del browser → **"Aggiungi a schermata Home"**
3. In ⚙ imposta tariffa e **posizione della sede**, poi: **Arrivo** all'inizio, **Finito** alla fine

## 👤 Autore
**Maximiliano Marchesi** · *MXM*

## 🤝 Trasparenza
Ho realizzato questo progetto **facendomi aiutare dall'intelligenza artificiale** (Claude). Scelgo di dichiararlo apertamente, con parole mie: per me la trasparenza conta, e sto imparando a programmare costruendo progetti veri e capendo cosa c'è dietro.

---
*Progetto personale — primo passo del mio percorso come sviluppatore.*
