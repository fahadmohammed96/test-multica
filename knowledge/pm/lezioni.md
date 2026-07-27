# Lezioni — Product Manager (libreria di squadra)

Conoscenza di mestiere valida su tutti i progetti, appresa sul campo dal
ruolo Product Manager. Vedi `knowledge/README.md` per il contratto: un fatto per
voce, datato, con il perché conta; niente dati cliente o segreti; ogni
aggiunta via PR, merge umano.

<!-- Esempio:
- 2026-07-25 — <fatto appreso e perché conta>.
  Perché conta: <cosa succede se lo dimentichi>.
-->

- 2026-07-25 — Un'approvazione umana **tersa** ("ok", "approvato") su un gate che
  portava più decisioni aperte non è una risposta: è una delega. La mossa giusta
  non è chiedere di nuovo né scegliere in silenzio, ma **registrare per iscritto
  l'interpretazione adottata**, decisione per decisione, nell'artefatto (non solo
  in chat) e nel messaggio di consegna, invitando esplicitamente a correggere.
  Perché conta: un gate che chiude cinque bivi con una parola lascia cinque
  interpretazioni non scritte, e ognuna torna a essere ridiscussa in fase di
  implementazione — quando costa dieci volte tanto. Mettere per iscritto ciò che
  hai dedotto trasforma il silenzio in un consenso verificabile: se hai capito
  male, l'umano lo vede subito, non tre story dopo.

- 2026-07-25 — Quando una decisione di prodotto è ancora dell'umano, **non
  bloccare la pianificazione e non decidere al suo posto: rendi la Story
  parametrica**. Il valore contestato (una soglia, una finestra temporale, un
  testo normativo, un'aliquota) esce dal codice e diventa configurazione con un
  criterio di accettazione che lo dimostra — un test che cambia il parametro e
  vede cambiare l'esito.
  Perché conta: separa il "cosa deve succedere" (progettabile ora) dal "con quale
  valore" (decidibile dopo), quindi la fase successiva parte senza aspettare, e
  quando la decisione arriva — o cambia perché è cambiata la norma o il mercato —
  si applica **senza rilascio di codice**. Senza il test che esercita il cambio di
  valore, "parametrico" resta un'intenzione: le costanti rientrano dalla finestra.

- 2026-07-25 — Ogni decisione aperta va etichettata con **la fase in cui diventa
  bloccante**, non lasciata in un elenco indistinto di "punti aperti". "Serve
  prima del rilascio della compliance (Epic 3)" è un'informazione azionabile;
  "da definire" non lo è.
  Perché conta: un elenco piatto di decisioni aperte non produce pressione e
  arriva in ritardo proprio su quella che serviva per prima, fermando un intero
  Epic in partenza. L'etichetta di scadenza dice all'umano quanta strada ha
  ancora, e dice a te quando smettere di segnalare e iniziare a escalare.

- 2026-07-25 — Quando un artefatto è **prerequisito** del task che stai per
  assegnare, il suo merge va messo in coda **prima** del dispatch, non in
  parallelo. "L'ho consegnato in PR, ti dico io su quale branch guardare" regge
  un giro e non regge la crescita: chi consuma l'artefatto lo cerca dove
  l'artefatto vive normalmente — il ramo principale — e lì trova la versione
  precedente.
  Perché conta: il fallimento non è un errore, è un **silenzio**. Nessuno vede
  un messaggio "documento non aggiornato": si legge una versione vecchia
  credendola l'ultima, e il lavoro che ne discende è sbagliato a valle senza che
  nessun controllo scatti. Se il merge non può precedere il dispatch, allora il
  puntatore esatto (branch + file + sezione) è **obbligatorio nel task**, non
  una cortesia.

- 2026-07-25 — **"Zero PR aperte" non significa "tutte mergiate".** Una PR chiusa
  senza merge sparisce dall'elenco delle aperte esattamente come una mergiata,
  quindi il conteggio non distingue i due casi. Una consegna si verifica sul
  **contenuto nel ramo principale** — il commit è antenato di `main`? il file
  esiste lì? — non sullo stato di una lista.
  Perché conta: è il modo più economico per mettere agli atti una chiusura che
  il repository smentisce, e nessuno se ne accorge finché qualcuno non clona il
  progetto e non trova ciò che era stato dichiarato consegnato. Il controllo
  costa dieci secondi e va fatto **prima** di marcare qualcosa come chiuso, non
  dopo: è l'unico momento in cui è ancora gratis.
