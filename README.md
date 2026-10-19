# Database Comuni Italiani (v2026.10.005)

**Rilascio:** 19 Ottobre 2026
**Manutentore:** [Evolvify](https://evolvify.app)
**Contatto:** https://databasecomuniitaliani.it/contatti

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
- `comuni.json` → un documento per comune, con CAP e comuni confinanti (array), popolazione, dati geografici,
  ambientali e per l'energia annidati, e i suoi codici storici dal 1991 (array).
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

- **Confini GeoJSON** (formato `.geojson`, cartella `geojson/`)
- `comuni.geojson`, `province.geojson`, `regioni.geojson` → i confini amministrativi ISTAT al 1° gennaio, nella
  versione generalizzata pensata per le mappe, in coordinate geografiche WGS84 (EPSG:4326). Ogni elemento porta il
  codice ISTAT (`istat`) per il collegamento con le tabelle; la data dei confini è nel campo `data_riferimento` del
  file. Un comune nato da una fusione dopo quella data compare ancora diviso nei comuni d'origine.

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
- **indirizzo_sede** — Indirizzo della sede del comune (via e numero civico), come il comune lo dichiara a IndicePA.
- **cap_sede** — CAP della sede. Quello di IndicePA quando è ancora valido per il comune; altrimenti l'unico CAP del
  comune, o vuoto nelle città con più CAP, dove non si può stabilire la zona del municipio.
- **wikipedia** — URL della voce Wikipedia del comune.
- **sito_web** — URL del sito istituzionale del comune, come il comune lo dichiara a IndicePA; da Wikidata
  dove IndicePA non lo riporta.
- **belfiore** — Codice Belfiore del comune.
- **santo_patrono** — Santo patrono del comune, estratto dall'infobox Wikipedia.
- **festa_patronale** — Data della festa patronale, estratta dall'infobox Wikipedia.
- **nome_abitanti** — Nome degli abitanti (es. livornesi), estratto dall'infobox Wikipedia.
- **nome_altra_lingua** — Nome ufficiale nella seconda lingua, dove ce n'è una (es. Bozen per Bolzano), fonte ISTAT.
- **nuts1**, **nuts2**, **nuts3** — Codici della classificazione europea NUTS 2024 (Eurostat) del comune: ripartizione
  geografica, regione (in Trentino-Alto Adige la provincia autonoma) e provincia. Fonte ISTAT.
- **sll_codice**, **sll_nome** — Sistema locale del lavoro 2021 del comune (ISTAT): l'area in cui la maggior parte
  dei residenti lavora, definita dagli spostamenti casa-lavoro.
- **sll_specializzazione** — Specializzazione produttiva del sistema locale del lavoro (es. Sistemi locali
  turistici), nella classificazione ISTAT.
- **fua_codice**, **fua_nome** — Area urbana funzionale 2021 (FUA) a cui appartiene il comune, fonte ISTAT. Vuoti per
  i comuni fuori da ogni area.
- **fua_tipo** — Ruolo del comune nell'area: «Città» (il centro) o «Zona di pendolarismo».
- **regione_capoluogo** — Indica se è capoluogo di regione.
- **provincia_capoluogo** — Indica se è capoluogo di provincia.
- **istat** — Codice ISTAT corrente del comune.
- **istat_1995_2005** — Codice ISTAT valido nel periodo 1995–2005.
- **istat_2006_2009** — Codice ISTAT valido nel periodo 2006–2009.
- **istat_2010_2016** — Codice ISTAT valido nel periodo 2010–2016.
- **istat_2017_2025** — Codice ISTAT valido nel periodo 2017–2025.
- **codice_fiscale** — Codice fiscale del comune, fonte IndicePA.
- **codice_ipa** — Codice del comune nell'Indice dei domicili digitali della PA (IndicePA).

#### `data_comuni_cap`
Associa i comuni ai rispettivi codici di avviamento postale.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **cap** — Codice di avviamento postale. Un comune ha più righe quando è una città zonata, quando una
  sua frazione ha un CAP proprio o quando è nato da una fusione e ha conservato i CAP dei comuni d'origine
  (questi ultimi da Wikidata, tenuti solo se la località è fra quelle ISTAT del comune e il CAP è della
  sua provincia). A ogni rilascio si applicano le variazioni che Poste Italiane pubblica tre volte l'anno
  su poste.it/cap: il CAP nuovo entra subito, quello sostituito resta accanto al nuovo per i dodici mesi in cui
  Poste lo accetta ancora e poi passa in `data_comuni_cap_vecchi`.

#### `data_comuni_cap_vecchi`
I CAP che un comune non usa più dopo una variazione pubblicata da Poste Italiane (dal 2022). Per i dodici mesi in
cui Poste accetta ancora il CAP vecchio, il comune li ha entrambi in `data_comuni_cap`; poi il vecchio passa qui.
Chi trova in un indirizzo un CAP che `data_comuni_cap` non ha più lo cerca in questa tabella: il comune è lo stesso.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **cap** — Il CAP non più valido.
- **pubblicazione** — Pubblicazione di Poste che ha fatto la variazione: anno e numero (2026-2 è la seconda del 2026).

#### `data_comuni_confinanti`
Elenca i comuni che condividono un confine, fonte Wikidata. Ogni coppia compare in entrambe le direzioni (A → B e
B → A), così i confinanti di un comune si leggono con un solo filtro su `comune_istat`.
Nel pacchetto dimostrativo compaiono solo le coppie interne alla provincia.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **confinante_istat** — Comune confinante.

#### `data_comuni_fatturazione`
Gli uffici di ciascun comune che ricevono fatture elettroniche, fonte IndicePA. Un comune ha una riga per ufficio:
la fattura va indirizzata al codice univoco dell'ufficio che ha ordinato la spesa.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **codice_univoco** — Codice univoco dell'ufficio (6 caratteri) da indicare nella fattura elettronica.
- **ufficio** — Nome dell'ufficio, come il comune lo dichiara a IndicePA.
- **codice_fiscale** — Codice fiscale da indicare nella fattura all'ufficio; può essere diverso da quello del
  comune.
- **data_avvio** — Data da cui l'ufficio riceve fatture elettroniche.

#### `data_comuni_uffici`
Gli uffici di ciascun comune con i loro contatti istituzionali, fonte IndicePA.
Un comune ha una riga per ufficio. Dei responsabili degli uffici non c'è nulla: nomi, email e telefoni di persone
sono dati personali. Per la stessa ragione un'email ordinaria resta solo quando è dell'ufficio (contiene il nome
di un servizio o del comune) e una PEC è tolta quando ha la forma di un nome di persona: per questo alcuni uffici
hanno meno contatti che in IndicePA.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **codice_ufficio** — Codice univoco dell'ufficio in IndicePA (6 caratteri); per gli uffici che ricevono fatture
  elettroniche è il codice univoco di `data_comuni_fatturazione`.
- **codice_ufficio_padre** — Codice dell'ufficio da cui questo dipende, quando IndicePA lo indica.
- **ufficio** — Nome dell'ufficio, come il comune lo dichiara.
- **area** — Area ricavata dal nome dell'ufficio (Ufficio tecnico, Polizia locale, Tributi, Servizi demografici…).
  È indicativa: un ufficio che riunisce più servizi prende la prima area riconosciuta, e il campo resta vuoto quando
  il nome non basta a stabilirla.
- **indirizzo**, **cap** — Indirizzo dell'ufficio.
- **telefono**, **fax** — Numeri dell'ufficio.
- **email** — Email ordinaria dell'ufficio.
- **pec** — Posta elettronica certificata dell'ufficio.
- **sito** — Pagina web dell'ufficio, dove indicata.

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
- **zona_climatica** — Zona climatica di classificazione, dall'Allegato A del DPR 412/93 nel testo vigente su Normattiva. Un comune nato dopo il 1993, che la tabella non elenca, prende i valori del comune da cui viene: da una fusione, quello dei comuni d'origine che ospita il municipio (il capoluogo indicato da ISTAT e la casa comunale alla stessa altitudine); staccato da altri comuni, il più vicino di questi. Quando la sede non si può stabilire prende la zona dei comuni d'origine, se era la stessa per tutti; i pochi restanti la prendono da tabelle tecniche di settore.
- **zona_sismica** — Zona sismica (classificazione della Protezione Civile). Un comune nato da una fusione dopo l'ultimo aggiornamento della classificazione prende la zona dei comuni d'origine, se era la stessa per tutti; altrimenti resta vuoto.
- **zona_altimetrica** — Zona altimetrica.
- **gradi_giorno** — Valore dei gradi-giorno, con le stesse regole della zona. Vuoto per i comuni nati da una fusione di cui non si può stabilire la sede: li stabilisce il sindaco (art. 2, comma 3 del DPR 412/93).
- **comune_litoraneo** — Indica se il territorio comunale si affaccia sul mare.
- **comune_isolano** — Indica se il comune sta su un’isola.
- **zone_costiere** — Indica se il comune ricade nella zona costiera (classificazione ISTAT).
- **zone_montane** — Indica se il comune ricade in zona montana, secondo la classificazione ISTAT usata per la PAC.
- **zone_vincoli_naturali** — Indica se ricade in una zona con vincoli naturali significativi (stessa classificazione).
- **zone_vincoli_specifici** — Indica se ricade in una zona con vincoli specifici (stessa classificazione).
- **comune_montano** — Indica se il comune è nell'elenco dei comuni montani della legge 131/2025 (allegato del DPCM
  11 maggio 2026, n. 121, in vigore dal 22 luglio 2026). È una classificazione diversa da `zone_montane`: la legge
  stessa esclude che valga per la PAC e per l'esenzione IMU dei terreni agricoli. Un comune nato dopo l'elenco da
  comuni in parte montani e in parte no resta vuoto: la legge gli mantiene la classificazione solo se rispetta i
  criteri.
- **area_interna** — Indica se il comune è un'area interna nella classificazione ISTAT «Aree interne 2020».
- **area_interna_classe** — Classe del comune nella stessa classificazione: Polo, Polo intercomunale, Cintura
  (i centri), Intermedio, Periferico, Ultraperiferico (le aree interne).
- **ecoregione_divisione**, **ecoregione_provincia**, **ecoregione_sezione**, **ecoregione_sottosezione** — Ecoregione
  del comune nei suoi quattro livelli, fonte ISTAT.

#### `data_comuni_rischio`
Rischio idrogeologico di ciascun comune, dagli indicatori ISPRA della piattaforma IdroGEO.
Una riga per comune; ogni colonna nomina prima la classe di pericolosità, poi la misura (es. `alluvione_p2_popolazione`
sono i residenti nelle aree allagabili dello scenario di media probabilità).

**Classi:**
- **frana_p4**, **frana_p3**, **frana_p2**, **frana_p1** — Aree a pericolosità da frana molto elevata, elevata, media e
  moderata dei Piani di Assetto Idrogeologico (mosaicatura ISPRA 2024). Le classi non si sovrappongono: la pericolosità
  elevata e molto elevata è la somma di P3 e P4, per ogni misura.
- **frana_attenzione** — Aree di attenzione, dove è segnalato un pericolo di frana non ancora classificato.
- **alluvione_p3**, **alluvione_p2**, **alluvione_p1** — Aree allagabili negli scenari di alta, media e bassa probabilità
  (D.Lgs. 49/2010, mosaicatura ISPRA 2020). Gli scenari sono uno dentro l'altro: l'area P1 comprende la P2, che comprende
  la P3, quindi non si sommano.

**Misure** (per ognuna il valore e, nella colonna che finisce in `_perc`, la quota sul totale del comune in percentuale):
- **area_kmq** — Superficie, in km².
- **popolazione** — Residenti: censimento ISTAT 2021 per le frane, 2011 per le alluvioni.
- **famiglie** — Famiglie, dagli stessi censimenti.
- **edifici** — Edifici: Database di Sintesi Nazionale IGMI per le frane, censimento ISTAT 2011 per le alluvioni.
- **imprese** — Unità locali di imprese: registro ASIA ISTAT 2022 per le frane, censimento dell'industria e dei servizi
  2011 per le alluvioni.
- **beni_culturali** — Beni culturali vincolati (Vincoli in Rete).

ISPRA calcola gli indicatori sui confini ISTAT del 2024: per un comune nato da una fusione dopo quella data i valori dei
comuni d'origine sono sommati e le quote ricalcolate. Una cella vuota è un dato che ISPRA non ha: lo scenario P3 delle
alluvioni manca dove l'autorità di bacino non lo ha mappato. Sono indicatori a scala comunale, per confronti e analisi
del territorio: non sostituiscono la cartografia delle Autorità di Bacino, che ha valore legale, né una valutazione
all'indirizzo. La zona sismica è nella tabella `data_comuni_ambiente`.

#### `data_comuni_amministrazione`
L'amministrazione di ciascun comune, dall'elenco degli amministratori locali del Ministero dell'Interno.
Una riga per comune presente nell'elenco. Delle persone non c'è nulla: nomi, date e luoghi di nascita, liste, titoli di
studio e professioni sono dati personali e vengono scartati al download: non entrano nel pacchetto, nel database,
nell'archivio delle sorgenti né nei log. Restano solo il comune, la carica e le sue date.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **data_elezione** — Data dell'ultima elezione del sindaco o, dove non c'è il sindaco, del consiglio comunale. Vuota
  per i comuni commissariati e dove l'elenco non ha né il sindaco né il consiglio.
- **data_insediamento** — Giorno in cui il sindaco in carica ha preso servizio. Vuoto dove il comune è commissariato o
  non ha un sindaco.
- **anno_scadenza_mandato** — Anno in cui il mandato scade naturalmente: l'anno dell'elezione più cinque. È un anno,
  non una data, e non è l'anno del voto: questo cade di norma nella finestra di primavera, dal 15 aprile al 15 giugno,
  di quell'anno o del successivo (legge 182/1991, art. 2), e le regioni a statuto speciale hanno regole proprie. Lo
  scioglimento anticipato del consiglio o le dimissioni del sindaco la anticipano, e l'anno può essere già passato se
  l'elenco del Ministero non è aggiornato. Vuoto dove il comune è commissariato.
- **commissariato** — Indica se il comune è retto da un commissario o da una commissione straordinaria. L'elenco può
  riportare insieme un sindaco e un commissario, quando uno dei due è l'amministrazione precedente: conta chi è entrato
  in carica per ultimo.
- **tipo_commissariamento** — Commissario straordinario, commissario prefettizio, commissione straordinaria o
  commissario regionale, come l'elenco chiama l'ufficio dell'ultimo commissario. Una commissione è un solo
  commissariamento, anche se ha più membri.
- **data_commissariamento** — Giorno in cui è entrato in carica il commissario (il primo, se sono più d'uno).
- **aggiornamento_fonte** — Giorno a cui il Ministero ha aggiornato l'elenco, uguale in tutte le righe: dice quanto sono
  recenti i dati.

I comuni di cui l'elenco usa un nome che non corrisponde a nessun comune ISTAT della provincia non hanno riga: nessun
abbinamento è tirato a indovinare.

#### `data_comuni_redditi`
I redditi dei contribuenti di ciascun comune, dalle statistiche sulle dichiarazioni IRPEF su base comunale del
Ministero dell'Economia e delle Finanze, Dipartimento delle Finanze (CC BY 3.0 IT).
Una riga per comune presente nelle statistiche. Per ogni voce della dichiarazione e per ogni classe di reddito ci sono due
colonne: i contribuenti che l'hanno (`_contribuenti`, la «frequenza» del MEF) e l'ammontare in euro (`_ammontare`).
**Campi:**
- **comune_istat** — Comune di riferimento.
- **anno_imposta** — Anno d'imposta dei redditi, quello scritto nel file del MEF: i redditi sono quelli prodotti in
  quell'anno e dichiarati l'anno dopo. Il MEF pubblica le statistiche con più di un anno di ritardo: ogni rilascio prende
  l'edizione più recente disponibile.
- **contribuenti** — Numero dei contribuenti del comune nelle statistiche.
- **reddito_complessivo**, **reddito_imponibile**, **imposta_netta** — Reddito complessivo dichiarato, reddito imponibile
  e imposta netta dovuta.
- **lavoro_dipendente**, **pensione**, **lavoro_autonomo**, **fabbricati** — Reddito da lavoro dipendente e assimilati, da
  pensione, da lavoro autonomo (il MEF conta anche i valori nulli) e da fabbricati.
- **addizionale_comunale** — Addizionale comunale all'IRPEF dovuta.
- **classe_fino_0**, **classe_0_10000**, **classe_10000_15000**, **classe_15000_26000**, **classe_26000_55000**,
  **classe_55000_75000**, **classe_75000_120000**, **classe_oltre_120000** — Le otto classi di reddito complessivo: i
  contribuenti che ci rientrano e il reddito complessivo che hanno. L'ammontare della prima classe può essere negativo.
- **reddito_complessivo_medio**, **reddito_imponibile_medio** — Reddito di un contribuente, in euro: l'ammontare diviso i
  contribuenti, arrotondato all'euro.
- **reddito_per_abitante** — Reddito complessivo diviso i residenti del comune (reddito pro capite), in euro. I
  residenti sono quelli di `data_comuni_popolazione`: la popolazione ISTAT al 1° gennaio più recente del pacchetto, che
  può cadere uno o due anni dopo l'anno d'imposta. Pesa il reddito sull'intera popolazione, anche su chi non presenta
  dichiarazione: di norma è più basso del reddito medio di un contribuente e serve a confrontare i comuni, non a
  stimare il reddito di una famiglia.

Il MEF nasconde per riservatezza i valori dove i contribuenti sono pochi: la cella resta vuota, non è uno zero, e restano
vuote le medie che ne dipendono. Il MEF conosce i comuni dell'anno d'imposta: per un comune nato da una fusione dopo quella
data i contribuenti e gli ammontari dei comuni d'origine sono sommati e le medie ricalcolate sulle somme; se il MEF nasconde
un valore in uno dei comuni d'origine, la somma resta vuota. I contribuenti il cui comune manca o è errato nelle
dichiarazioni il MEF li tiene in una riga a parte: non sono attribuiti a nessun comune e non sono contati qui.

#### `data_comuni_demografia`
L'età e le famiglie dei residenti di ciascun comune, da ISTAT (CC BY 4.0): la popolazione residente per età e il
Censimento permanente della popolazione.
Una riga per comune che ha la popolazione per età.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **data_riferimento** — Data a cui si riferisce l'età dei residenti, quella scritta nel titolo del file ISTAT: di norma il
  1° gennaio. È uguale in ogni riga.
- **eta_media** — Età media dei residenti, in anni con un decimale: la media delle età con ogni anno di età contato a
  metà (x + 0,5), quindi la classe «100 e oltre» vale 100,5. Vuota se il comune non ha residenti.
- **popolazione_0_14**, **popolazione_15_64**, **popolazione_65_oltre** — Residenti per classe di età, alla data di
  riferimento. La classe «100 e oltre» è nei 65 anni e più.
- **popolazione_80_oltre** — Residenti di 80 anni e più: sono già contati anche in `popolazione_65_oltre`.
- **indice_vecchiaia** — Residenti di 65 anni e più ogni 100 residenti da 0 a 14 anni, con un decimale. Vuoto se non ci
  sono residenti da 0 a 14 anni.
- **indice_dipendenza** — Residenti da 0 a 14 anni e da 65 anni in su ogni 100 residenti da 15 a 64 anni, con un decimale.
  Vuoto se non ci sono residenti da 15 a 64 anni.
- **anno_famiglie** — Anno a cui si riferiscono le famiglie, l'ultimo del Censimento permanente della popolazione: è lo
  stesso in ogni riga che ha le famiglie e può essere precedente alla data di riferimento dell'età.
- **famiglie**, **popolazione_in_famiglia** — Famiglie e residenti che vivono in famiglia nell'anno delle famiglie,
  arrotondati all'unità. Chi vive in convivenza (case di riposo, collegi, caserme) non è nei residenti in famiglia.
- **componenti_medi_famiglia** — Residenti in famiglia diviso famiglie, con due decimali.

Le famiglie di un comune che ISTAT non ha per l'anno più recente, o di cui dà un valore che non è un numero, restano vuote:
non è uno zero, e non si usano gli anni precedenti al suo posto. I file di ISTAT conoscono i comuni alle loro date: per un
comune nato da una fusione dopo quelle date i residenti di ogni anno di età e le famiglie dei comuni d'origine sono
sommati, e l'età media e gli indici sono ricalcolati sulle somme; se in uno dei comuni d'origine ISTAT dà un valore che
non è un numero, la somma resta vuota.

#### `data_comuni_agevolazioni`
Se ciascun comune è in tre elenchi ufficiali: i comuni ad alta tensione abitativa, il cratere del sisma 2016-2017 e la
fascia dei frontalieri con la Svizzera.
Una riga per comune, con i flag falsi dove il comune non è in nessun elenco. La tabella dice in quale elenco è il comune e da
quale atto viene, non le condizioni delle norme che richiamano gli elenchi.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **alta_tensione_abitativa** — Il comune è nell'elenco dei comuni ad alta tensione abitativa della
  delibera CIPE 13 novembre 2003, n. 87 (Allegato A, Gazzetta Ufficiale n. 40 del 18 febbraio 2004). È l'ultimo elenco
  approvato dal CIPE.
- **alta_tensione_abitativa_elenco** — Il nome con cui l'elenco chiama il comune, quando è diverso da quello di oggi: Borgo
  Virgilio è «Virgilio» nell'elenco. Vuoto negli altri casi.
- **cratere_sisma_2016** — Il comune è tra i comuni dei crateri del sisma 2016-2017 (allegati 1, 2 e 2-bis del
  decreto-legge 189/2016), nell'elenco di ISTAT del 4 maggio 2017.
- **cratere_sisma_2016_evento** — L'evento per cui è nel cratere: «24 agosto 2016» oppure «26 e 30 ottobre 2016, 18 gennaio
  2017». Vuoto se il comune non è nel cratere.
- **frontaliere_svizzera** — Il comune è nell'elenco della fascia di confine con la Svizzera, allegato alla procedura
  amichevole del 22 dicembre 2023 sull'Accordo Italia-Svizzera del 23 dicembre 2020 (si applica dal 1° gennaio 2024).

I codici degli elenchi sono portati al comune di oggi. Un comune nato da una fusione è nell'elenco quando lo era almeno uno dei
comuni da cui nasce, e per l'alta tensione abitativa il nome che l'elenco dava a quei comuni resta nel campo
`alta_tensione_abitativa_elenco`. Per questo un comune nato da una fusione dopo la data di un elenco può avere quel flag vero.

#### `data_comuni_geo`
Dati geografici e fisici dei comuni.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **latitudine** — Latitudine del centro abitato del capoluogo comunale (WGS84, gradi decimali), fonte ISTAT
  Basi territoriali 2021; per i comuni nati dopo il 2021 e per pochi centri verificati a mano, Wikidata.
- **longitudine** — Longitudine dello stesso punto (WGS84, gradi decimali).
- **altitudine_slm** — Altitudine della casa comunale sul livello del mare: quella della tabella del DPR 412/93 per i comuni che
  elenca, a cui si riferiscono i gradi giorno, quella del municipio di oggi (ISTAT) per gli altri.
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

#### `data_comuni_energia`
Dati per l'energia di ciascun comune, calcolati con le regole ufficiali.
**Campi:**
- **comune_istat** — Comune di riferimento.
- **altitudine_dpr_412** — Altitudine del comune nella tabella dell'Allegato A del DPR 412/93, quella con cui si calcola il
  coefficiente C. Un comune nato dopo il 1993 ha quella del comune d'origine da cui prende i gradi giorno; vuota quando i
  gradi giorno non vengono dalla tabella.
- **coefficiente_c** — Coefficiente C convenzionale del gas (ARERA, RTDG 2020-2027, art. 6): converte in Smc i metri cubi
  letti da un contatore in bassa pressione senza correttore, all'altitudine del comune. Calcolato da `altitudine_dpr_412`
  e da gradi giorno e zona climatica di `data_comuni_ambiente`, con gli arrotondamenti della regola. Vuoto in zona A, che
  la regola non prevede, e dove manca uno dei valori. Per edifici molto più in alto o in basso del comune, o per pressioni
  di misura più alte, il distributore applica valori propri.
- **ambito_tariffario_gas** — Ambito tariffario della distribuzione del gas (ARERA, RTDG 2020-2027, art. 43), stabilito
  per regione.
- **zona_mercato_elettrico** — Zona di offerta del mercato elettrico (Terna, Codice di Rete, allegato A.24), stabilita per
  regione: Nord, Centro Nord, Centro Sud, Sud, Calabria, Sicilia, Sardegna.
- **conto_termico_piccolo_comune** — Indica se il comune ha fino a 15.000 abitanti, la soglia sotto cui il Conto Termico
  3.0 copre il 100% delle spese per gli edifici del comune (DM 7 agosto 2025, art. 11, comma 2). Calcolato su
  `popolazione_totale` di `data_comuni_popolazione`; vuoto senza quel dato.
- **zona_neve** — Zona di carico da neve della provincia secondo le Norme tecniche per le costruzioni del 2018 (§3.4.2):
  I - Alpina, I - Mediterranea, II, III. Le province sarde istituite dopo il 2018 sono in zona III, come tutte quelle
  sarde che la norma elenca.
- **carico_neve_suolo** — Carico di riferimento della neve al suolo (qsk, kN/m²) all'altitudine della casa comunale.
- **zona_vento** — Zona del vento (Tab. 3.3.I), da 1 a 9: per regione, con le province di Trieste (8) e di Reggio Calabria
  (4), la Sardegna divisa dalla retta fra Capo Teulada e l'isola della Maddalena (5 a est, 6 a ovest, secondo il centro
  abitato del comune) e le isole minori in mare (9).
- **velocita_base_vento** — Velocità base di riferimento del vento (vb, m/s) all'altitudine della casa comunale (§3.3.1).

  I due valori sono un riferimento del comune: per un progetto la norma chiede l'altitudine del sito della costruzione.
  Sopra i 1.500 m chiede di valutare le condizioni locali, con valori non inferiori a quelli dei 1.500 m: per quei
  comuni i valori sono proprio quelli dei 1.500 m.

#### `data_comuni_soppressi`
Traccia i comuni soppressi o accorpati con informazioni sul nuovo ente.
**Campi:**
- **codice_comune_vecchio** — Codice del comune soppresso.
- **nome_vecchio** — Nome del comune soppresso.
- **sigla_provincia_vecchio** — Sigla della provincia del comune soppresso.
- **codice_provincia_vecchio** — Codice della provincia del comune soppresso.
- **anno_evento** — Anno in cui è avvenuta la soppressione o fusione.
- **data_inizio_amministrativa** — Data di decorrenza amministrativa dell’evento.
- **tipo_evento** — Tipo di evento: «Estinzione» quando il comune confluisce in quello associato (fusione o
  incorporazione), «Estinzione per cessione a stato estero» per i territori ceduti.
- **codice_comune_associato** — Codice del comune successore/associato.
- **comune_associato** — Nome del comune successore/associato.
- **sigla_provincia_associato** — Sigla della provincia del comune successore.
- **codice_provincia_associato** — Codice della provincia del comune successore.
- **belfiore_vecchio** — Codice catastale (Belfiore) del comune soppresso: è quello del codice fiscale di chi ci è
  nato prima della data dell'evento. Vuoto nei pochi casi in cui ISTAT non lo riporta.
- **belfiore_associato** — Codice catastale del comune successore.
- **comune_istat_attuale** — Codice ISTAT del comune che oggi ha il territorio del comune soppresso. Il codice del
  successore è quello di allora, e da allora può essere cambiato con le province o con fusioni successive: questo
  campo segue la catena delle variazioni fino a oggi, controllando a ogni passaggio che il codice appartenga
  ancora allo stesso comune. Vuoto per i comuni ceduti a uno stato estero e nei rari casi in cui la catena non si
  può ricostruire.

#### `data_comuni_raccordo`
Tabella di raccordo dei codici ISTAT dal 1991, fonte ISTAT: ogni codice e denominazione che un comune ha avuto, con
il comune che li ha oggi. Serve a portare al comune di oggi un archivio compilato con i codici di un altro anno.
**Campi:**
- **codice_storico** — Codice ISTAT del comune com'era scritto quando era in uso (sei cifre, come testo).
- **denominazione_storica** — Denominazione del comune quando aveva quel codice.
- **comune_istat** — Codice ISTAT del comune di oggi.
- **data_variazione** — Data da cui il codice o la denominazione non sono più in uso; vuota per quelli di oggi.
- **variazione** — Cosa li ha chiusi: Cambio di provincia, Rinumerazione, Cambio di denominazione, Estinzione.
- **provvedimento** — L'atto che ha disposto la variazione, come lo riporta ISTAT.

Un codice usato in due periodi dallo stesso comune (i comuni sardi passati alle nuove province e tornati) compare
una volta, con l'ultima variazione.

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
- **capoluogo** — Indica la località che ISTAT considera il centro del capoluogo comunale.

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
- **tipologia** — Tipologia di ente: Provincia, Provincia autonoma, Città metropolitana, Libero consorzio di comuni
  o Unità non amministrativa (le ex province del Friuli-Venezia Giulia). Fonte ISTAT.
- **nuts3** — Codice NUTS 2024 di livello 3. Vuoto per le province sarde del 2025 che stanno a cavallo di due aree
  NUTS: il codice esatto è sul comune.
- **numero_comuni** — Numero complessivo di comuni appartenenti.
- **denominazione_ente** — Denominazione dell'ente pubblico che governa la provincia (la provincia, la città
  metropolitana, il libero consorzio comunale, l'ente di decentramento regionale in Friuli-Venezia Giulia), come
  la scrive IndicePA, con le maiuscole, gli accenti e i trattini sistemati. Vuoto per Aosta: in Valle d'Aosta IndicePA non ha un ente provinciale.
- **codice_fiscale** — Codice fiscale dell'ente, come lo dichiara a IndicePA.
- **codice_ipa** — Codice dell'ente nell'Indice dei domicili digitali della PA (IndicePA).
- **pec** — Indirizzo PEC dell'ente: il primo che IndicePA dichiara come PEC.
- **sito_web** — URL del sito istituzionale dell'ente, come lo dichiara a IndicePA.
- **indirizzo_sede** — Indirizzo della sede dell'ente (via e numero civico), come lo dichiara a IndicePA.
- **cap_sede** — CAP della sede dell'ente, come lo dichiara a IndicePA.
- **comune_sede_istat** — Codice ISTAT del comune di oggi dove ha sede l'ente.

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
- **nuts1**, **nuts2** — Codici NUTS 2024 di livello 1 e 2. Il Trentino-Alto Adige non ha un codice di livello 2: al
  suo posto la classificazione usa le due province autonome, che si trovano sul comune.
- **numero_province** — Numero di province appartenenti.
- **numero_comuni** — Numero di comuni appartenenti.
- **denominazione_ente** — Denominazione dell'ente pubblico che governa la regione, come la scrive IndicePA, con le
  maiuscole, gli accenti e i trattini sistemati.
- **codice_fiscale** — Codice fiscale dell'ente, come lo dichiara a IndicePA.
- **codice_ipa** — Codice dell'ente nell'Indice dei domicili digitali della PA (IndicePA).
- **pec** — Indirizzo PEC dell'ente: il primo che IndicePA dichiara come PEC.
- **sito_web** — URL del sito istituzionale dell'ente, come lo dichiara a IndicePA.
- **indirizzo_sede** — Indirizzo della sede dell'ente (via e numero civico), come lo dichiara a IndicePA.
- **cap_sede** — CAP della sede dell'ente, come lo dichiara a IndicePA.
- **comune_sede_istat** — Codice ISTAT del comune di oggi dove ha sede l'ente.

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
