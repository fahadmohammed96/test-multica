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

- 2026-07-27 — Quando un percorso **periodico** e un percorso **su richiesta**
  condividono lo stesso predicato di selezione, chiediti: *questo percorso ha una
  seconda occasione?* Il periodico ripassa e compensa, quindi può permettersi un
  filtro del tipo «c'è qualcosa da fare?»; l'esecuzione su richiesta avviene una
  volta sola e quel filtro la rende **fragile allo stato del momento**. Il rimedio
  non è cambiare il predicato condiviso — romperebbe l'altro percorso — ma dare al
  percorso senza compensazione un **passo in più**, dichiarato e testato.
  Perché conta: nel caso reale la cancellazione di dati personali su richiesta non
  toccava nessuna riga quando il campo era già vuoto, quindi non scriveva
  l'evidenza; e siccome era l'evidenza ad armare il divieto di riscrittura, il dato
  **rientrava** alla sincronizzazione successiva da una sorgente esterna. Tutti i
  test verdi, l'adempimento dichiarato eseguito, e la sua durabilità dipendente da
  come stava per caso quel campo in quell'istante.

- 2026-07-27 — Un test che costruisce **esattamente** lo scenario difettoso ma
  asserisce troppo poco è peggio di un test assente: occupa il posto di quello che
  avrebbe morso, e la sua presenza nella lista fa credere che il caso sia coperto.
  Corollario di metodo: per una funzionalità **nuova** il rosso «prima
  dell'implementazione» è poco informativo (mancano import, colonne, tabelle —
  esplode tutto). La copertura vera si misura per **mutazione**: si rompe un
  invariante alla volta sul codice finito e si verifica *quale* test cade, e che sia
  quello giusto.
  Perché conta: è l'unico modo di distinguere una suite che *descrive* il codice da
  una che lo *vincola*. Vale come autocontrollo prima di consegnare, non solo come
  tecnica di chi rivede.

- 2026-07-30 — Sostituire una `DELETE` con una **transizione tracciata su riga
  singola** può spostare in avanti la perdita di evidenza invece di chiuderla.
  Chiediti: *questo ciclo può ripetersi?* Se sì, la riapertura riscrive le date
  del giro precedente e l'evidenza dei giri passati sparisce comunque — solo più
  tardi e più silenziosamente. La forma che chiude è il **registro**: una riga per
  giro, con l'unicità spostata dall'entità alla riga **valida** (indice unico
  **parziale** sul predicato «non revocata»), così l'invariante «una sola valida»
  resta senza vietare la storia.
  Perché conta: il caso reale era la conferma di lettura di un'informativa fiscale
  cancellata quando l'utente rientrava sotto soglia. La riga portava la data in cui
  l'utente era stato informato, cioè la prova dell'adempimento. Rimpiazzare la
  `DELETE` con un campo «revocata il» sulla stessa riga sembra risolvere e passa
  ogni test scritto su un solo giro: cade al secondo, e cade in silenzio.

- 2026-07-30 — Una guardia strutturale che vieta le cancellazioni **per tabella** è
  cieca a `session.delete(oggetto)`: una cancellazione di ISTANZA non dice
  staticamente su quale tabella agisca. Serve la regola più stretta sul **modulo
  proprietario** («qui non si cancella nulla, mai»). Corollario operativo:
  aggiungere una tabella all'elenco protetto **senza** aggiungere il suo modulo a
  quella seconda regola lascia la guardia verde proprio sulla forma del difetto che
  la decisione voleva vietare.
  Perché conta: la protezione sembra estesa, l'elenco cita la tabella nuova, tutti
  i test sono verdi — e il difetto originale, reintrodotto identico, passa. Si
  verifica in un modo solo: reintrodurre la cancellazione e pretendere il rosso.

- 2026-07-25 — Un'attività periodica (purge, retention, promemoria) si fa con la
  **coda di job durevole**, non con uno scheduler in memoria: l'handler si
  **riprogramma** alla fine di ogni esecuzione e un **bootstrap idempotente**
  all'avvio rimette in coda il ciclo se manca.
  Perché conta: senza riprogrammazione il ciclo muore al primo riavvio; senza
  bootstrap idempotente ogni riavvio ne accoda un altro e i cicli si moltiplicano.
  Corollario di dipendenze: il kernel che esegue i job non deve conoscere i
  moduli applicativi — serve un entrypoint di livello applicativo che importi i
  moduli (registrandone gli handler) e poi ceda al ciclo generico.
