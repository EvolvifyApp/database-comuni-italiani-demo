# Database Comuni Italiani (v2026.10.001)

**Rilascio:** 09 Ottobre 2026
**Manutentore:** [Evolvify](https://evolvify.app)
**Contatto:** info@databasecomuniitaliani.it

---

## Questo è il pacchetto dimostrativo

Contiene **la sola provincia di Livorno (LI)**:
19 comuni, la loro provincia e la loro regione, con **tutti i campi**
del database completo — zone climatiche e gradi giorno, zone sismiche e altimetriche, frazioni, serie solari,
CAP, PEC e codici storici. Nessun campo è stato tolto o annacquato: cambia il territorio, non la profondità.

Il pacchetto dimostrativo è distribuito in formato Excel, CSV e JSON: i dump SQL e il database SQLite
fanno parte dei soli pacchetti completi.

Il database completo copre allo stesso modo tutti i comuni italiani.
Piani e licenze: https://databasecomuniitaliani.it/prezzi

---

## Contenuti del pacchetto
Il pacchetto fornisce una versione normalizzata e aggiornata dei dati territoriali italiani, utile per applicazioni
gestionali, statistiche e di ricerca.
Sono incluse le seguenti risorse:

- **File Excel** (formato `.xlsx`)
- `comuni.xlsx` → contiene la lista completa dei comuni italiani, con metadati geografici e amministrativi.
- `province.xlsx` → contiene l’elenco delle province con le relative relazioni verso regioni e codici ufficiali.
- `regioni.xlsx` → contiene l’elenco delle regioni con denominazioni e codici normalizzati.
- `nazioni.xlsx` → contiene l’elenco dei paesi e territori esteri con codici ISTAT, AT (per codice fiscale), ISO 3166 e M49.

Ogni file Excel ha intestazioni chiare, encoding UTF-8 e un unico foglio per tabella, corrispondente esattamente alla
struttura definita nello schema del database completo.

- **File JSON** (formato `.json`, cartella `json/`)
- `comuni.json` → un documento per comune, con CAP e comuni confinanti (array), popolazione, dati geografici e
  ambientali annidati.
- `province.json` → un documento per provincia, stessa struttura annidata di `comuni.json`.
- `regioni.json` → un documento per regione, stessa struttura annidata.
- `comuni_soppressi.json` → elenco dei comuni soppressi o accorpati.
- `comuni_solare_{anno}.json` → dati mensili di irraggiamento solare per anno.
- `nazioni.json` → elenco dei paesi e territori esteri con tutti i codici identificativi.

A differenza dei file Excel, nei file JSON i valori numerici sono codificati come numeri nativi (non testo), e le
chiavi annidate mancanti per una specifica entità sono presenti con valore `null` invece di essere omesse.

- **File CSV** (formato `.csv`, cartella `csv/`)
- `comuni.csv`, `comuni_cap.csv`, `comuni_confinanti.csv`, `comuni_popolazione.csv`, `comuni_geo.csv`,
  `comuni_ambiente.csv` → un file per ciascuna tabella dei comuni.
- `comuni_soppressi.csv` → elenco dei comuni soppressi o accorpati.
- `comuni_solare_{anno}.csv` → dati mensili di irraggiamento solare per anno.
- `province.csv`, `province_popolazione.csv`, `province_geo.csv`, `province_ambiente.csv` → un file per ciascuna
  tabella delle province.
- `regioni.csv`, `regioni_popolazione.csv`, `regioni_geo.csv`, `regioni_ambiente.csv` → un file per ciascuna tabella
  delle regioni.
- `nazioni.csv` → elenco dei paesi e territori esteri in formato tabellare flat.

A differenza dei file JSON, i file CSV non sono denormalizzati: ogni file corrisponde a una singola tabella e
riflette esattamente lo schema del database completo, incluso il legame 1\:N tra comune e CAP (`comuni_cap.csv`, senza duplicazioni né
valori concatenati).

- **Metadati**
- [`manifest.json`](./manifest.json) → descrizione tecnica e hash SHA256 dei file inclusi.
- [`LICENSE.md`](./LICENSE.md) → termini d’uso.
- [`CHANGELOG.md`](./CHANGELOG.md) → elenco delle modifiche tra versioni.

---

## Requisiti
- Encoding: **UTF-8**
- Locale: **it_IT**
- Timezone: **Europe/Rome**

---

## Data Dictionary

### Tabelle Comuni

#### `data_comuni`
Contiene le informazioni amministrative e identificative di ciascun comune.
**Campi:**
- **id** — Identificativo univoco del comune.
- **id_regione** — Identificativo della regione di appartenenza.
- **id_provincia** — Identificativo della provincia di appartenenza.
- **nome** — Denominazione ufficiale del comune.
- **email** — Email istituzionale del comune.
- **pec** — PEC del comune.
- **telefono** — Numero di telefono.
- **prefisso** — Prefisso telefonico del comune.
- **fax** — Numero di fax.
- **wikipedia** — URL della voce Wikipedia del comune.
- **sito_web** — URL del sito istituzionale del comune.
- **belfiore** — Codice Belfiore del comune.
- **santo_patrono** — Santo patrono del comune, estratto dall'infobox Wikipedia.
- **festa_patronale** — Data della festa patronale, estratta dall'infobox Wikipedia.
- **nome_abitanti** — Nome degli abitanti (es. livornesi), estratto dall'infobox Wikipedia.
- **regione_capoluogo** — Indica se è capoluogo di regione.
- **provincia_capoluogo** — Indica se è capoluogo di provincia.
- **istat** — Codice ISTAT corrente del comune.
- **istat_1995_2005** — Codice ISTAT valido nel periodo 1995–2005.
- **istat_2006_2009** — Codice ISTAT valido nel periodo 2006–2009.
- **istat_2010_2016** — Codice ISTAT valido nel periodo 2010–2016.
- **istat_2017_2025** — Codice ISTAT valido nel periodo 2017–2025.

#### `data_comuni_cap`
Associa i comuni ai rispettivi codici di avviamento postale.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **cap** — Codice di avviamento postale.

#### `data_comuni_confinanti`
Elenca i comuni che condividono un confine, fonte Wikidata. Ogni coppia compare in entrambe le direzioni (A → B e
B → A), così i confinanti di un comune si leggono con un solo filtro su `comune_istat`.
Nel pacchetto dimostrativo compaiono solo le coppie interne alla provincia.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **confinante_istat** — Comune confinante.

#### `data_comuni_popolazione`
Dati demografici di base relativi ai comuni.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **popolazione_maschile** — Numero di residenti maschi.
- **popolazione_femminile** — Numero di residenti femmine.
- **popolazione_totale** — Totale della popolazione residente. Per un comune nato da una fusione dopo la data della rilevazione ISTAT, la popolazione è la somma dei comuni d'origine alla stessa data.
- **densita_abitativa** — Abitanti per chilometro quadrato.
- **grado_urbanizzazione** — Grado di urbanizzazione del comune.

#### `data_comuni_ambiente`
Informazioni ambientali e territoriali dei comuni.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **zona_climatica** — Zona climatica di classificazione, dall'Allegato A del DPR 412/93 nel testo vigente su Normattiva. I comuni nati dopo il 1993, che la tabella non elenca, la prendono da tabelle tecniche di settore. Un comune nato da una fusione che nessuna fonte elenca prende la zona dei comuni d'origine, se era la stessa per tutti; altrimenti resta vuoto.
- **zona_sismica** — Zona sismica (classificazione della Protezione Civile). Un comune nato da una fusione dopo l'ultimo aggiornamento della classificazione prende la zona dei comuni d'origine, se era la stessa per tutti; altrimenti resta vuoto.
- **zona_altimetrica** — Zona altimetrica.
- **gradi_giorno** — Valore dei gradi-giorno. Vuoto per i comuni nati da una fusione dopo l'ultimo aggiornamento della tabella: li stabilisce il sindaco (art. 2, comma 3 del DPR 412/93).
- **comune_litoraneo** — Indica se il comune è litoraneo.
- **comune_isolano** — Indica se il comune è su un’isola.
- **zone_costiere** — Indica se il comune è in zona costiera.
- **zone_montane** — Indica se il comune è in zona montana.
- **zone_vincoli_naturali** — Indica se ricade in aree con vincoli naturali.
- **zone_vincoli_specifici** — Indica se ricade in aree con vincoli specifici.

#### `data_comuni_geo`
Dati geografici e fisici dei comuni.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **latitudine** — Latitudine geografica.
- **longitudine** — Longitudine geografica.
- **altitudine_slm** — Altitudine sul livello del mare.
- **superficie_kmq** — Superficie territoriale in km².

#### `data_comuni_solare`
Valori mensili di irraggiamento e parametri solari dei comuni.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **anno** — Anno di rilevazione.
- **mese** — Mese di rilevazione.
- **irradiazione_orizzontale** — Irraggiamento globale su piano orizzontale (kWh/m²/mese).
- **irradiazione_piano_ottimale** — Irraggiamento su piano a inclinazione ottimale (kWh/m²/mese).
- **irradiazione_piano_inclinato** — Irraggiamento su piano a inclinazione fissa (kWh/m²/mese).
- **irradiazione_diretta_normale** — Irraggiamento diretto su piano normale al sole (kWh/m²/mese).
- **rapporto_diffusa_globale** — Rapporto tra irraggiamento diffuso e globale.
- **temperatura_media** — Temperatura media nelle 24 ore (°C).
- **inclinazione_ottimale** — Angolo di inclinazione ottimale.
- **azimuth** — Orientamento rispetto al nord.

#### `data_comuni_soppressi`
Traccia i comuni soppressi o accorpati con informazioni sul nuovo ente.
**Campi:**
- **codice_comune_vecchio** — Codice del comune soppresso.
- **nome_vecchio** — Nome del comune soppresso.
- **sigla_provincia_vecchio** — Sigla della provincia del comune soppresso.
- **codice_provincia_vecchio** — Codice della provincia del comune soppresso.
- **anno_evento** — Anno in cui è avvenuta la soppressione o fusione.
- **data_inizio_amministrativa** — Data di decorrenza amministrativa dell’evento.
- **tipo_evento** — Tipologia dell’evento (fusione, aggregazione, soppressione).
- **codice_comune_associato** — Codice del comune successore/associato.
- **comune_associato** — Nome del comune successore/associato.
- **sigla_provincia_associato** — Sigla della provincia del comune successore.
- **codice_provincia_associato** — Codice della provincia del comune successore.

#### `data_comuni_frazioni`
Frazioni e località abitate, fonte ISTAT Basi Territoriali 2021.
**Campi:**
- **istat** — Codice ISTAT della località, univoco: frazioni dello stesso comune possono avere lo stesso nome.
- **comune_istat** — Comune di riferimento.
- **nome** — Nome della frazione o località.
- **tipo** — Tipologia: `centro_abitato`, `nucleo_abitato`, `localita_produttiva` o `case_sparse`.
- **popolazione** — Popolazione residente nella località (censimento 2021).
- **altitudine** — Altitudine sul livello del mare.
- **latitudine** — Latitudine geografica (WGS84).
- **longitudine** — Longitudine geografica (WGS84).

---

### Tabelle Province

#### `data_province`
Contiene le informazioni amministrative e identificative delle province.
**Campi:**
- **id** — Identificativo univoco della provincia.
- **id_regione** — Regione di appartenenza.
- **nome** — Denominazione ufficiale della provincia.
- **sigla** — Sigla della provincia.
- **istat** — Codice ISTAT della provincia.
- **regione_capoluogo** — Indica se ospita il capoluogo regionale.
- **numero_comuni** — Numero complessivo di comuni appartenenti.

#### `data_province_popolazione`
Dati demografici sintetici delle province.
**Campi:**
- **id_provincia** — Provincia di riferimento.
- **popolazione_maschile** — Residenti maschi.
- **popolazione_femminile** — Residenti femmine.
- **popolazione_totale** — Totale della popolazione residente.
- **densita_abitativa** — Abitanti per chilometro quadrato.

#### `data_province_ambiente`
Dati ambientali aggregati delle province, calcolati dai comuni.
**Campi:**
- **provincia_istat** — Provincia di riferimento.
- **zona_climatica_min** — Zona climatica minima tra i comuni (DPR 412/93).
- **zona_climatica_media** — Zona climatica media tra i comuni.
- **zona_climatica_max** — Zona climatica massima tra i comuni.
- **gradi_giorno_min** — Gradi-giorno minimi tra i comuni.
- **gradi_giorno_media** — Gradi-giorno medi tra i comuni.
- **gradi_giorno_max** — Gradi-giorno massimi tra i comuni.

#### `data_province_geo`
Dati geografici delle province.
**Campi:**
- **provincia_istat** — Provincia di riferimento.
- **superficie_kmq** — Superficie territoriale in km².
- **altitudine_min** — Altitudine minima s.l.m. tra i comuni.
- **altitudine_media** — Altitudine media s.l.m. tra i comuni.
- **altitudine_max** — Altitudine massima s.l.m. tra i comuni.

---

### Tabelle Regioni

#### `data_regioni`
Contiene le informazioni amministrative e identificative delle regioni.
**Campi:**
- **id** — Identificativo univoco della regione.
- **zona** — Macro-zona di appartenenza.
- **nome** — Denominazione ufficiale della regione.
- **istat** — Codice ISTAT della regione.
- **numero_province** — Numero di province appartenenti.
- **numero_comuni** — Numero di comuni appartenenti.

#### `data_regioni_popolazione`
Dati demografici sintetici delle regioni.
**Campi:**
- **id_regione** — Regione di riferimento.
- **popolazione_maschile** — Residenti maschi.
- **popolazione_femminile** — Residenti femmine.
- **popolazione_totale** — Totale della popolazione residente.
- **densita_abitativa** — Abitanti per chilometro quadrato.

#### `data_regioni_ambiente`
Dati ambientali aggregati delle regioni, calcolati dai comuni.
**Campi:**
- **regione_istat** — Regione di riferimento.
- **zona_climatica_min** — Zona climatica minima tra i comuni (DPR 412/93).
- **zona_climatica_media** — Zona climatica media tra i comuni.
- **zona_climatica_max** — Zona climatica massima tra i comuni.
- **gradi_giorno_min** — Gradi-giorno minimi tra i comuni.
- **gradi_giorno_media** — Gradi-giorno medi tra i comuni.
- **gradi_giorno_max** — Gradi-giorno massimi tra i comuni.

#### `data_regioni_geo`
Dati geografici delle regioni.
**Campi:**
- **regione_istat** — Regione di riferimento.
- **superficie_kmq** — Superficie territoriale in km².
- **altitudine_min** — Altitudine minima s.l.m. tra i comuni.
- **altitudine_media** — Altitudine media s.l.m. tra i comuni.
- **altitudine_max** — Altitudine massima s.l.m. tra i comuni.

---

## Changelog
Consulta il file completo: [`CHANGELOG.md`](./CHANGELOG.md)

---

## Licenza
Questo pacchetto è distribuito con licenza **proprietaria** per uso personale.
È vietato l’uso commerciale, la redistribuzione, la modifica e la creazione di opere derivate.
L’uso è consentito esclusivamente per **consultazioni interne** e **analisi statistiche interne**.

➡️ Testo completo: [`LICENSE.md`](./LICENSE.md)

---

© 2026 Evolvify, Modena, Italy.
