# Lezioni — Developer (libreria di squadra)

Conoscenza di mestiere valida su tutti i progetti, appresa sul campo dal
ruolo Developer. Vedi `knowledge/README.md` per il contratto: un fatto per
voce, datato, con il perché conta; niente dati cliente o segreti; ogni
aggiunta via PR, merge umano.

<!-- Esempio:
- 2026-07-25 — <fatto appreso e perché conta>.
  Perché conta: <cosa succede se lo dimentichi>.
-->

- 2026-07-25 — Una convenzione trasversale (scoping multi-tenant, autenticazione
  obbligatoria, confini fra moduli) va imposta da un **test che cammina il
  codice**, non solo scritta nella documentazione: percorrere schema e route a
  runtime e fallire quando qualcosa non la rispetta. Le eccezioni legittime
  stanno in un'**allowlist esplicita**, affiancata da un test che fa **decadere
  l'esenzione** se l'oggetto esentato cambia natura (es. una tabella di
  riferimento che acquisisce un legame col tenant).
  Perché conta: la documentazione non ferma il codice, il test sì — e vale per
  ogni story futura scritta da chi non c'era. L'allowlist sorvegliata è la
  differenza fra "esento e mi fido" ed "esento e controllo": senza il test di
  decadenza, la prima esenzione diventa il buco da cui passano le successive.

- 2026-07-25 — Ogni tabella di configurazione **a validità temporale** (aliquote,
  prezzi, parametri normativi, flag datati) ha due invarianti da chiudere subito:
  l'inserimento **chiude i periodi aperti con decorrenza minore O UGUALE** al
  nuovo, e la risoluzione ordina con un **tiebreaker deterministico** (es. data di
  creazione decrescente) oltre alla decorrenza.
  Perché conta: il caso realistico non è la nuova versione a data diversa, è la
  **ri-emissione con la stessa decorrenza** per correggere un valore sbagliato.
  Senza queste due cose restano due periodi aperti identici e quale valore si
  applica dipende dall'ordine di ritorno del database: un difetto silenzioso che
  si manifesta solo quando qualcuno inizia davvero a usare quei parametri.

- 2026-07-25 — Quando un valore mostrato è **derivato** da un altro dato (un
  conteggio, un totale, uno stato calcolato) e ha una propria query lato client,
  ogni mutazione sulla sorgente deve invalidare **anche** la cache del derivato —
  meglio se in un unico punto condiviso, non ripetuto per ogni mutazione.
  Perché conta: i test di componente con le dipendenze mockate **non possono**
  vedere questo difetto, perché la cache non esiste nel loro mondo. Lo trova solo
  un end-to-end che esercita l'app reale: è la giustificazione concreta del costo
  di tenere un e2e full-stack in CI, non un lusso.

- 2026-07-25 — Un freno agli accessi ripetuti (rate limit sul login) va applicato
  **prima** di verificare le credenziali e **anche per utenze inesistenti**, con
  la stessa risposta. Serve inoltre più di un asse: per account (utenza presa di
  mira) **e** per origine (tentativi sparsi su molte utenze, che il solo limite
  per account non vede mai perché ognuna resta sotto soglia). Sempre a finestra
  temporale, mai un blocco permanente.
  Perché conta: frenare solo gli account esistenti reintroduce dalla porta di
  servizio l'**enumerazione delle utenze** che di solito si è già chiusa altrove
  (es. rendendo indistinguibile la risposta a credenziali errate) — la
  contromisura di sicurezza ne annulla un'altra. E le tracce del freno non vanno
  legate all'utente: si scrivono prima di sapere se esiste.

- 2026-07-25 — Un'attività periodica (purge, retention, promemoria) si fa con la
  **coda di job durevole**, non con uno scheduler in memoria: l'handler si
  **riprogramma** alla fine di ogni esecuzione e un **bootstrap idempotente**
  all'avvio rimette in coda il ciclo se manca.
  Perché conta: senza riprogrammazione il ciclo muore al primo riavvio; senza
  bootstrap idempotente ogni riavvio ne accoda un altro e i cicli si moltiplicano.
  Corollario di dipendenze: il kernel che esegue i job non deve conoscere i
  moduli applicativi — serve un entrypoint di livello applicativo che importi i
  moduli (registrandone gli handler) e poi ceda al ciclo generico.
