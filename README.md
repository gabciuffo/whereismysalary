<h1 align="center">Where is my <em>Salary</em></h1>

<p align="center">
  <i>Mese per mese, contato a mano.</i>
</p>

<p align="center">
  Una piccola app per tenere traccia dello stipendio italiano: salario, welfare, fringe, buoni pasto, bonus e rimborsi.<br/>
  Niente account, niente cloud (opzionale), niente dipendenze. Un solo file <code>.html</code> che funziona offline e gira anche dal telefono.
</p>

---

## Cosa fa

Sei mai uscito di casa il 27 del mese chiedendoti se ti sono arrivati davvero **tutti** i pezzi della busta paga? Questa app risponde a quella domanda, calcolando ciò che ti spettava e ciò che ti è effettivamente arrivato.

- **Quanto ti devono ancora** — un'unica cifra in cima alla pagina, sempre aggiornata sui mesi già passati.
- **Sei voci di paga** — stipendio, welfare, fringe benefit, buoni pasto, bonus, rimborso spese.
- **Stato per mese** — ✓ ricevuto · ! mancante · — non spettato.
- **Buoni pasto in automatico** — calcola i giorni lavorativi italiani escludendo weekend e festività nazionali (Pasquetta inclusa, calcolata correttamente per ogni anno). Sovrascrivibile a mano per ferie, malattia, smart working diverso.
- **Fringe nel mese giusto** — di default Dicembre, configurabile.
- **Bonus & rimborsi a importo libero** — inseriti mese per mese.
- **Multi-profilo** — Tu, il/la collega, chiunque altro. Ogni profilo ha le sue cifre e il suo storico, con uno switch in alto.
- **Backup CSV** — esporta o importa per spostare i dati tra dispositivi (formato `;`-separato leggibile in Excel/Numbers).
- **Sync cloud opzionale** — un gist GitHub privato come deposito comune tra Mac, iPhone, ufficio. Niente nuovi servizi, niente account aggiuntivi.

---

## Setup iniziale (30 secondi)

1. Apri `index.html` con doppio click — si apre nel browser.
2. Clicca **"+ Crea profilo"**, scrivi il tuo nome, lo stipendio mensile e gli altri valori (welfare 400, fringe 1000, ticket 10€/giorno sono già pre-impostati).
3. Per ogni mese passato, apri la card e imposta lo stato di ogni voce.
4. Per aggiungere un secondo profilo (collega, partner, ecc.): tasto **+** in alto a destra → nuovo profilo.

---

## Come avviarla / accedervi dal telefono

Hai tre opzioni, dalla più semplice alla più "pro".

### 1. File locale via iCloud / Google Drive (zero lavoro)

1. Salva `index.html` su iCloud Drive (o Google Drive, OneDrive…).
2. Sul telefono apri il file dall'app File — si apre nel browser.
3. Aggiungilo alla schermata Home: in Safari → **Condividi → Aggiungi a Home**. Diventerà un'icona come una vera app.

> ⚠️ I dati vivono nel browser di ogni dispositivo separatamente. Per averli identici su Mac e iPhone, usa la **Sync cloud** (sotto) oppure esporta il CSV da uno e importalo sull'altro.

### 2. Netlify Drop (5 minuti, zero account)

1. Vai su [app.netlify.com/drop](https://app.netlify.com/drop).
2. Trascina `index.html` nella zona bianca.
3. Netlify ti dà un URL del tipo `https://nome-casuale.netlify.app/`.
4. Apri l'URL dal telefono → "Aggiungi a Home". Fatto.

Per cambiare il sottodominio o aggiornare il file in futuro, registrati gratis su Netlify (accesso con Google/GitHub).

### 3. GitHub Pages (1 minuto se hai già una repo)

1. Crea una repo **pubblica** (Pages free non funziona con repo private).
2. Carica `index.html` (è già il nome giusto per Pages).
3. **Settings** → **Pages** → branch: `main`, folder: `/ (root)` → Save.
4. Dopo qualche minuto: `https://<username>.github.io/<repo>/`.

---

## Privacy & sicurezza

- **Tutti i dati restano nel browser** (`localStorage`). Nessun server, nessun cloud (a meno che tu non attivi la sync), nessuna telemetria.
- Se pubblichi l'app online (Netlify o Pages), l'URL è tecnicamente pubblico ma **i tuoi dati no**: ognuno che apre l'URL parte da zero perché lo storage è per-browser. È la stessa logica per cui un blocco note in tasca è privato anche se la fabbrica del blocco note ha mille copie identiche.
- **Sync cloud (opzionale)**: il Personal Access Token GitHub viene salvato in chiaro nel `localStorage` del dispositivo. È una scelta consapevole — l'app è single-file senza backend e non può fare di meglio. Se il browser viene compromesso (estensione malevola, sessione condivisa), **revoca il token su GitHub** (Settings → Developer settings → Personal access tokens) e la sync è disattivata su tutti i dispositivi. Lo scope del token è limitato a `gist` — non può fare nient'altro sul tuo account.

---

## Come si calcola "quanto ti devono"

Per ogni voce e ogni mese:

| Stato | Conteggio |
|---|---|
| ✓ ricevuto | conta come incassato, e come atteso |
| ! mancante | conta come atteso; conta come "da ricevere" solo se il mese è già passato o è quello corrente |
| — non spettato | non conta |

L'importo atteso è ricavato dalle impostazioni del profilo (stipendio, welfare, fringe, ticket × giorni lavorativi) o dall'importo che inserisci direttamente (bonus, rimborsi).

I giorni lavorativi sono calcolati con weekend italiani esclusi e queste **festività nazionali**: Capodanno, Epifania, Pasquetta, Liberazione, Festa del Lavoro, Festa della Repubblica, Ferragosto, Ognissanti, Immacolata, Natale, Santo Stefano. La data di Pasqua si ricava con l'algoritmo di Gauss/Meeus.

> ⚠️ Le festività dei santi patroni locali (Sant'Ambrogio a Milano, San Petronio a Bologna, ecc.) **non** sono incluse: l'app conta solo le festività nazionali. Se ne hai bisogno, sovrascrivi il numero di giorni a mano per quel mese.

---

## Tech stack

> Un solo file, niente build, niente CDN obbligatorie.

- HTML + CSS + JS vanilla (~1100 righe di JS, in un unico `.html`)
- `localStorage` per la persistenza locale
- GitHub Gist API per la sync cloud opzionale (gist privato, scope `gist`)
- Google Fonts opzionali (Source Serif 4, Inter, Caveat) — caricate via CDN, ma con fallback a system serif/sans, quindi l'app è leggibile anche senza connessione
- Calcolo Pasqua e festività italiane fatto in-app (vedi `easterSunday`, `italianHolidays`)

Non ci sono framework, bundler, package manager. Il file si modifica con un editor di testo qualunque.

---

## Come è organizzato il codice

Il file è un unico `index.html`. Le sezioni principali, in ordine:

| Riga (circa) | Sezione | Note |
|---|---|---|
| 1–17 | `<head>` e meta tag | Favicon inline SVG, theme color, Apple touch icon, Google Fonts |
| 18–1030 | `<style>` | Variabili CSS in `:root`, design tokens, layout, sheet, confirm/prompt modal |
| 1030–1035 | `<body>` markup statico | Topbar, hero placeholder, sheet "new profile", sheet "settings", sheet "sync", overlay confirm + prompt |
| 1035–~2100 | `<script>` | Tutta la logica |

Dentro lo `<script>`, in ordine:

| Blocco | Cosa fa |
|---|---|
| Costanti (`VERSION`, `STORAGE_KEY`, `SYNC_KEY`, `MONTHS_IT`, `COMPONENTS`) | Definizioni base. `COMPONENTS` è l'elenco delle sei voci di paga con flag (`expectedDefault`, `hasAmount`, `hasDays`) — modifica qui per aggiungere/rimuovere voci. |
| `easterSunday`, `italianHolidays`, `workingDaysInMonth` | Calendario italiano, calcolo giorni lavorativi. |
| `loadStore`, `saveStore`, `defaultSettings`, `defaultMonth`, `ensureProfile`, `ensureYear`, `activeProfile` | Persistenza e shape del dato in `localStorage`. Lo store ha forma `{ profiles: { id: { name, settings, years: { 2025: { 1: {...}, 2: {...} } } } }, activeProfile, currentYear }`. |
| `expectedAmountFor`, `computeYearTotals`, `monthStatus` | Logica di calcolo "atteso vs ricevuto". |
| `render`, `renderTopbar`, `renderMain`, `renderMonthCard` | Rendering. **L'app rifà l'intero `#main` con `innerHTML` ad ogni interazione** — semplice, funziona bene su questa scala, ma occhio se aggiungi input con focus persistente. |
| `openSheet`, `closeSheets`, `openNewProfile` + handler `$("#btn-...")` | UI dei sheet (modali a fondo schermo) e bottoni. |
| `buildCsv`, `parseCsv`, `parseCsvLine`, `fmtNum`, `parseNum`, `csvEscape`, `applyImportedData`, `$("#btn-export")`, `$("#btn-import-file")` | Backup CSV (export/import). Formato sezionato (`# SETTINGS` + `# MESI`), separatore `;`, decimale `,`, UTF-8 con BOM. Stati mappati da inglese a italiano via `STATE_TO_IT` / `STATE_FROM_IT`. |
| `toast`, `escapeHtml` | Utility. **`escapeHtml` va usata su qualunque stringa utente interpolata in template string + `innerHTML`** — convenzione importante per evitare self-XSS. |
| `showConfirm` / `showAlert` | Modali custom che rimpiazzano `confirm()` / `alert()` nativi (alcuni browser/estensioni li mangiano, sui telefoni bloccano l'UI). Promise-based. `showAlert` è un wrapper di `showConfirm` con `opts.alert = true` (nasconde il bottone Annulla). |
| Cloud sync (`syncState`, `ghFetch`, `cloudConfigure`, `cloudPull`, `cloudPush`, `schedulePush`, `cloudDisconnect`, `refreshSyncSection`) | Sync via gist GitHub. `schedulePush` è debounced (~1.8s) per non sparare una PATCH per ogni click. `cloudPull` confronta `_lastModified` locale vs remoto per decidere chi vince. |
| `bootstrap()` | Setup all'avvio: imposta version tag, apre il mese corrente, avvia eventuale pull cloud, apre form nuovo profilo se vuoto. |

### Storage keys

| Chiave | Contenuto |
|---|---|
| `stipendio.v1` | Lo store principale (profili, mesi, settings). Versionata nel nome per facilitare migrazioni future. |
| `stipendio.sync.v1` | Stato della sync cloud (PAT, gistId, user, lastSyncAt, lastError). |

### Convenzioni difensive

- **Sempre `escapeHtml(s)`** per nomi profilo, user GitHub, messaggi d'errore interpolati in HTML.
- **Sempre `Math.min(max, Math.max(0, Number(v) || 0))`** per input numerici provenienti dall'utente (vedi `clamp` nelle settings).
- **`saveStore()` aggiorna `_lastModified`** e fa partire la sync debounced — non bypassare scrivendo direttamente su `localStorage` se non in casi tipo `cloudPull` (dove serve evitare il ciclo).
- **Mai chiamare `alert/confirm` nativi** — usa i wrapper `showAlert/showConfirm`.

---

## Personalizzazione

Tutto è configurabile dall'icona ⚙️ in alto. Per ogni profilo puoi impostare:

- Stipendio mensile (max 100.000 €)
- Welfare mensile (default 400 €, max 100.000)
- Fringe benefit annuale (default 1000 €, max 100.000)
- Mese in cui arriva il fringe (default Dicembre)
- Ticket per giorno lavorativo (default 10 €, max 1.000)
- Bonus standard (per quando segni un bonus come atteso senza specificarne l'importo)

Se i valori cambiano (es. aumento di stipendio), modificarli ricalcola automaticamente anche gli importi attesi nei mesi già marcati.

---

## Backup & portabilità

Dalle impostazioni → **Backup CSV** puoi:

- **Scarica file** — esporta un CSV con tutti i profili e tutti gli anni. Convenzione italiana: separatore `;`, decimale `,`, UTF-8 con BOM (Excel/Numbers italiani lo aprono in colonne pulite).
- **Carica file** — importa un CSV precedentemente esportato. Sovrascrive interamente i dati attuali (chiede conferma). Validazione strict: qualsiasi riga malformata → l'intero import viene rifiutato, niente stati a metà.

Formato del file (due sezioni separate da `# SETTINGS` e `# MESI`):

```
# SETTINGS
profilo;stipendio;welfare;fringe;mese_fringe;ticket_giorno;bonus_default
Massimo;2800,00;400,00;1000,00;12;10,00;0,00

# MESI
profilo;anno;mese;stipendio_stato;stipendio_importo;welfare;ticket_stato;ticket_giorni;fringe;bonus_stato;bonus_importo;rimborso_stato;rimborso_importo
Massimo;2026;1;ricevuto;3000,00;ricevuto;ricevuto;22;non_atteso;non_atteso;0,00;non_atteso;0,00
```

Stati ammessi: `ricevuto`, `mancante`, `non_atteso`. Mesi con tutti i valori a default vengono omessi dall'export per ridurre rumore.

La colonna `stipendio_importo` è un **override per-mese** opzionale: se vuota, l'app usa `settings.salary` del profilo; se valorizzata, vince sull'importo di default (utile per mesi con cifra diversa dal solito).

Conservare un export ogni tanto è una buona idea: serve sia come backup, sia per portare i dati su un altro telefono o computer.

Se usi la **sync cloud**, il backup CSV ti serve solo come misura di sicurezza extra (la sync cloud continua a usare JSON nel gist, indipendente dal CSV).

---

## Roadmap

Idee in attesa di valutazione, non promesse:

- Notifica a fine mese per "voci ancora mancanti"
- Esportazione PDF del riepilogo annuale
- Gestione TFR e tredicesima/quattordicesima
- Modalità "previsione" per simulare aumenti o cambi contratto
- Festività comunali opzionali per il calcolo buoni pasto

---

## Filosofia di design

L'aspetto dell'app segue una piccola filosofia visiva chiamata **Quiet Ledger** (file `quiet-ledger-philosophy.md` nella repo): carta calda come superficie primaria, italico serif come voce, un singolo accento sienna usato come sigillo di ceralacca. Riferimento sotterraneo a Luca Pacioli — *Summa de Arithmetica*, Venezia 1494, il trattato che cinque secoli fa codificò la partita doppia, antenata silenziosa di ogni libro mastro personale.

---

## Licenza

MIT. Fai quello che ti pare, ma una citazione fa sempre piacere.

---

<p align="center">
  <i>contato a mano, mese per mese</i>
</p>
