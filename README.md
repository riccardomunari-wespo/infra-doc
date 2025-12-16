# Century Italia – Documentazione dell’Infrastruttura di Progetto

![Century Italia](https://www.century-italia.it/wp-content/themes/century/images/logo.png)

Questo documento fornisce una panoramica completa della configurazione dell’infrastruttura del progetto **Century Italia**, includendo il repository Git, la configurazione di Sanity CMS e i dettagli di deploy su Vercel.

---

## 📋 Indice

1. [Panoramica del Progetto](#panoramica-del-progetto)
2. [Repository Git](#repository-git)
3. [Configurazione Sanity CMS](#configurazione-sanity-cms)
4. [Deploy su Vercel](#deploy-su-vercel)
5. [Variabili d’Ambiente](#variabili-dambiente)
6. [Stack Tecnologico](#stack-tecnologico)
7. [Architettura del Progetto](#architettura-del-progetto)
8. [Informazioni di Accesso](#informazioni-di-accesso)

---

## 🎯 Panoramica del Progetto

**Nome Progetto**: Century Italia – Catalogo Prodotti Illuminazione
**Descrizione**: Sito web moderno e ad alte prestazioni per il catalogo prodotti di illuminazione, sviluppato con Next.js e Sanity CMS, con gestione completa dei prodotti, organizzazione per categorie e ottimizzazione SEO.

**Funzionalità principali**:

* Applicazione React con rendering server-side tramite Next.js 15+
* CMS headless con Sanity per la gestione dei contenuti
* Sistema automatizzato di importazione prodotti con cron job notturni
* Capacità di editing visuale in tempo reale
* Supporto multi-lingua
* SEO ottimizzato con eccellenti punteggi PageSpeed

---

## 🔗 Repository Git

### Informazioni sul Repository

| Proprietà                   | Valore                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------ |
| **Piattaforma**             | GitHub                                                                               |
| **URL Repository**          | [https://github.com/centuryitalia/Century](https://github.com/centuryitalia/Century) |
| **Proprietario Repository** | Century                                                                              |
| **Nome Repository**         | century-sanity-plus-next                                                             |
| **Branch di Default**       | master                                                                               |
| **Branch di Sviluppo**      | stage                                                                                |

### Strategia dei Branch

* **`master`** – Branch di produzione, deployato su Vercel (production)
* **`stage`** – Branch di sviluppo/staging per test prima della produzione
* I feature branch vengono creati a partire da `stage` e mergiati dopo revisione

### Comando di Clone

```bash
git clone https://github.com/centuryitalia/Century
cd Century
```

### Struttura del Repository

```
century/
├── frontend/              # Applicazione frontend Next.js
│   ├── app/              # Pagine App Router di Next.js
│   ├── components/       # Componenti React
│   ├── lib/              # Funzioni di utilità e client Sanity
│   ├── public/           # Asset statici
│   ├── package.json      # Dipendenze frontend
│   └── vercel.json       # Configurazione Vercel
├── studio/               # Sanity CMS Studio
│   ├── src/
│   │   ├── schemaTypes/  # Schemi dei contenuti
│   │   └── structure/    # Configurazione struttura Studio
│   ├── sanity.config.ts  # Configurazione Sanity
│   └── package.json      # Dipendenze Studio
├── package.json          # Package.json root con configurazione workspace
└── README.md             # Documentazione del progetto
```

---

## 🎨 Configurazione Sanity CMS

### Dettagli Progetto Sanity

| Proprietà                   | Valore                                                                                                   |
| --------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Project ID**              | `lnqcgpdf (da aggiornare con il tuo account)`                                                            |
| **Dataset**                 | `production`                                                                                             |
| **Titolo Progetto**         | Sanity + Next.js Starter Template                                                                        |
| **Studio URL (Locale)**     | [http://localhost:3333](http://localhost:3333)                                                           |
| **Studio URL (Produzione)** | [https://century-sanity-plus-next-studio.vercel.app](https://century-sanity-plus-next-studio.vercel.app) |

### Schemi di Contenuto

#### 1. Schema Prodotto

Gestione completa dei prodotti di illuminazione con **oltre 40 campi personalizzati**, organizzati in 5 gruppi:

* **Caratteristiche Generali**: Nome, codice articolo, codice EAN, tecnologia, famiglia, linea, categorie
* **Caratteristiche Tecniche**: Potenza, flusso luminoso, colore della luce, CRI, tensione, rating IP/IK
* **Dimensioni**: Lunghezza, larghezza, spessore, peso
* **Imballo**: Tipo di confezione, quantità per cartone, peso lordo
* **Media e Immagini**: Immagini prodotto, galleria, documenti tecnici

#### 2. Schema Categoria

Organizzazione gerarchica delle categorie con:

* Informazioni base (nome, slug, descrizione, immagine)
* Relazioni padre-figlio
* Assegnazione bidirezionale dei prodotti
* Metadati SEO (meta title, meta description)
* Ordine di visualizzazione e attivazione/disattivazione

#### 3. Schemi Aggiuntivi

* **Schema Page**: Pagine personalizzate con contenuti flessibili
* **Schema Post**: Articoli e post del blog
* **Schema Person**: Autori e membri del team
* **Schema Settings**: Impostazioni globali del sito (singleton)

### Plugin Sanity

Lo Studio utilizza i seguenti plugin:

1. **Presentation Tool** – Editing visuale con anteprima live
2. **Structure Tool** – Struttura personalizzata dello Studio
3. **Unsplash Image Asset** – Integrazione immagini stock
4. **Sanity Assist** – Assistenza AI per la creazione dei contenuti
5. **Vision Tool** – Test delle query GROQ

### File di Configurazione Sanity

Configurazioni principali:

* Project ID e dataset tramite variabili d’ambiente
* URL di preview per l’editing visuale
* Resolver dei documenti per i diversi tipi di contenuto
* Resolver di posizione per il presentation tool

---

## 🚀 Deploy su Vercel

### Informazioni di Deploy

| Proprietà               | Valore                                                                                                       |
| ----------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Piattaforma**         | Vercel                                                                                                       |
| **URL Frontend**        | [https://century-sanity-plus-next-frontend.vercel.app](https://century-sanity-plus-next-frontend.vercel.app) |
| **URL Studio**          | [https://century-sanity-plus-next-studio.vercel.app](https://century-sanity-plus-next-studio.vercel.app)     |
| **Framework**           | Next.js                                                                                                      |
| **Versione Node**       | 20.x                                                                                                         |
| **Build Command**       | `npm run build`                                                                                              |
| **Directory di Output** | `.next`                                                                                                      |

### Configurazione Vercel

```json
{
  "framework": "nextjs",
  "crons": [
    {
      "path": "/api/import/cron",
      "schedule": "0 2 * * *"
    }
  ]
}
```

### Cron Job

**Importazione automatica prodotti**:

* **Path**: `/api/import/cron`
* **Schedulazione**: `0 2 * * *` (ogni giorno alle 02:00 UTC)
* **Scopo**: Importazione notturna automatica dei dati prodotto da file Excel

### Workflow di Deploy

1. **Deploy automatici**:

   * Push su `master` → deploy in produzione
   * Push su `stage` → deploy di preview

2. **Processo di Build**:

   * Installazione dipendenze
   * Generazione tipi Sanity (`npm run typegen`)
   * Build applicazione Next.js (`next build`)
   * Deploy sulla rete edge di Vercel

3. **Preview Deployments**:

   * Ogni pull request genera una preview dedicata
   * Aggiornamenti automatici ad ogni commit

---

## 🔐 Variabili d’Ambiente

### Variabili Frontend

Necessarie per l’applicazione Next.js:

| Variabile                       | Descrizione                              | Esempio                     |
| ------------------------------- | ---------------------------------------- | --------------------------- |
| `NEXT_PUBLIC_SANITY_PROJECT_ID` | Project ID Sanity (pubblico)             | `lnqcgpdf (da aggiornare)`  |
| `NEXT_PUBLIC_SANITY_DATASET`    | Nome dataset Sanity (pubblico)           | `production`                |
| `SANITY_API_READ_TOKEN`         | Token API Sanity per letture server-side | `sk...` (segreto)           |
| `BLOB_READ_WRITE_TOKEN`         | Token Vercel Blob storage                | `vercel_blob_...` (segreto) |

### Variabili Studio

Necessarie per Sanity Studio:

| Variabile                   | Descrizione                 | Esempio                                                                                                 |
| --------------------------- | --------------------------- | ------------------------------------------------------------------------------------------------------- |
| `SANITY_STUDIO_PROJECT_ID`  | Project ID Sanity           | `lnqcgpdf (da aggiornare)`                                                                              |
| `SANITY_STUDIO_DATASET`     | Dataset Sanity              | `production`                                                                                            |
| `SANITY_STUDIO_PREVIEW_URL` | URL frontend per la preview | `http://localhost:3000` (locale)<br>`https://century-sanity-plus-next-frontend.vercel.app` (produzione) |

### Configurazione Variabili d’Ambiente

#### Sviluppo Locale

**Frontend** (`frontend/.env.local`):

```env
NEXT_PUBLIC_SANITY_PROJECT_ID=lnqcgpdf (da aggiornare)
NEXT_PUBLIC_SANITY_DATASET=production
SANITY_API_READ_TOKEN=your_token_here
BLOB_READ_WRITE_TOKEN=your_blob_token_here
```

**Studio** (`studio/.env`):

```env
SANITY_STUDIO_PROJECT_ID=lnqcgpdf (da aggiornare)
SANITY_STUDIO_DATASET=production
SANITY_STUDIO_PREVIEW_URL=http://localhost:3000
```

#### Produzione su Vercel

1. Accedi alla Dashboard Vercel → Project Settings → Environment Variables
2. Aggiungi ogni variabile con lo scope appropriato (Production / Preview / Development)
3. Esegui un redeploy per applicare le modifiche

### Ottenere un Token API Sanity

1. Visita [https://sanity.io/manage](https://sanity.io/manage)
2. Seleziona il progetto `lnqcgpdf (da aggiornare)`
3. Vai su **API** → **Tokens**
4. Clicca su **Add API token**
5. Configura:

   * **Nome**: Vercel Read Token
   * **Permessi**: Viewer (sola lettura) o Editor (lettura/scrittura)
6. Copia il token e aggiungilo alle variabili d’ambiente

---

## 🛠️ Stack Tecnologico

### Frontend

| Tecnologia | Versione | Scopo |
|------------|----------|-------|
| **Next.js** | 15.5.7 | Framework React con App Router |
| **React** | 19.1.1 | Libreria UI |
| **TypeScript** | 5.9.2 | Tipizzazione statica |
| **Tailwind CSS** | 4.1.12 | Framework CSS utility-first |
| **next-sanity** | 10.0.14 | Integrazione Sanity per Next.js |
| **@sanity/client** | 7.9.0 | Client Sanity |
| **@vercel/blob** | 0.27.0 | Storage Vercel Blob |
| **@vercel/speed-insights** | 1.2.0 | Monitoraggio delle performance |
| **xlsx** | 0.18.5 | Elaborazione file Excel |

### CMS (Sanity Studio)

| Tecnologia | Versione | Scopo |
|------------|----------|-------|
| **Sanity** | 4.5.0 | Headless CMS |
| **@sanity/vision** | Latest | Test delle query GROQ |
| **sanity-plugin-asset-source-unsplash** | Latest | Integrazione immagini stock |
| **@sanity/assist** | Latest | Assistenza AI per i contenuti |

### Strumenti di Sviluppo

- **ESLint** – Analisi statica e linting del codice
- **Prettier** – Formattazione automatica del codice
- **npm-run-all2** – Esecuzione di più script in parallelo

---

## 🏗️ Architettura del Progetto

### Struttura Monorepo

Il progetto utilizza **npm workspaces** per gestire due applicazioni separate:

```json
{
  "workspaces": [
    "studio",
    "frontend"
  ]
}

## 🧩 Architettura Frontend

frontend/
├── app/ # Next.js App Router
│ ├── api/ # Route API
│ │ ├── draft-mode/ # Modalità bozza per preview
│ │ └── import/ # Endpoint import prodotti
│ │ ├── cron/ # Gestione cron job
│ │ ├── execute/ # Esecuzione import
│ │ └── process/ # Elaborazione import
│ ├── layout.tsx # Layout principale
│ └── page.tsx # Homepage
├── components/ # Componenti React
├── lib/ # Utility
│ ├── sanity/ # Client e query Sanity
│ └── import/ # Utility sistema di import
└── public/ # Asset statici

## 🔄 Flusso dei Dati

```mermaid
graph LR
    A[File Excel] --> B[API Import]
    B --> C[Elaborazione & Validazione]
    C --> D[Sanity CMS]
    D --> E[Frontend Next.js]
    E --> F[Browser Utente]
    G[Sanity Studio] --> D
    H[Vercel Blob] --> E

## 🧱 Architettura del Sistema di Import

### Caricamento Excel
Il client carica il file Excel tramite uno strumento dedicato in **Sanity Studio**.

### Elaborazione
L’API valida i dati e mappa **113 colonne** nello schema prodotto.

### Gestione Media
Le immagini prodotto vengono caricate e salvate su **Vercel Blob Storage**.

### Aggiornamento Sanity
I prodotti vengono creati o aggiornati all’interno del CMS **Sanity**.

### Sincronizzazione Automatica
Un **cron job notturno** elabora automaticamente i nuovi dati disponibili.

### Soft Delete
I prodotti vengono marcati come inattivi in base al valore della colonna  
`"articolo sul web"` presente nel file Excel.

---

## 🔑 Informazioni di Accesso

### Accesso al Repository
- **Repository GitHub**: https://github.com/centuryitalia/Century  
- **Livello di Accesso**: Repository privato (richiede account GitHub autorizzato)

### Accesso a Sanity CMS
- **Sanity Manage**: https://sanity.io/manage  
- **Project ID**: `lnqcgpdf (will-update-with-your-account)`  
- **Studio (Produzione)**: https://century-sanity-plus-next-studio.vercel.app  
- **Accesso**: Richiede un account Sanity con permessi sul progetto

### Accesso a Vercel
- **Dashboard Vercel**: https://vercel.com/dashboard  
- **Frontend**: https://century-sanity-plus-next-frontend.vercel.app  
- **Studio**: https://century-sanity-plus-next-studio.vercel.app  
- **Accesso**: Richiede un account Vercel con accesso al team/progetto
