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

- 2026-07-25 — Il criterio per **avviare** la fase successiva e quello per
  **chiudere** la precedente non sono lo stesso criterio, e confonderli costa in
  entrambe le direzioni. Per far partire un lavoro che si appoggia a un documento
  basta che il documento sia **consegnato** — leggibile, anche solo su un branch
  in PR; per dichiarare **chiusa** la fase serve che sia **integrato** nel ramo
  principale. Quando sblocchi sul contenuto, l'handoff deve dire **dove** si
  legge (ref, percorso, sezione): è obbligatorio, non una cortesia.
  Perché conta: legare l'avvio al merge serializza l'intera squadra dietro una
  persona; legare la chiusura al contenuto consegnato mette agli atti fasi che il
  repository smentisce. E senza il puntatore esatto il fallimento non è un
  errore, è un **silenzio**: chi consuma l'artefatto lo cerca dove l'artefatto
  vive normalmente — il ramo principale — vi trova la versione precedente
  credendola l'ultima, e nessun controllo scatta. Un gate umano è un collo di
  bottiglia inevitabile solo dove serve davvero: sull'integrazione, non sulla
  lettura.

- 2026-07-25 — Prima di **marcare done**, verifica la proprietà osservabile
  dell'artefatto e non il conteggio che la riassume: il commit è antenato del
  ramo di destinazione? il file esiste lì? In particolare **"zero PR aperte" non
  significa "tutte mergiate"** — una PR chiusa senza merge sparisce dall'elenco
  delle aperte esattamente come una mergiata, e il conteggio resta identico.
  Vale anche quando la conferma arriva da chi ha l'autorità di darla: l'errore
  tipico è in buona fede, dentro un riassunto di stato, non nella catena di
  comando.
  Perché conta: chiudere è l'unica azione che mette un fatto **agli atti**, e se
  il fatto non è vero il progetto acquisisce una falsa chiusura proprio nel punto
  in cui nessuno guarderà più — mentre il lavoro consegnato ma non mergiato resta
  a marcire su un branch, indistinguibile da lavoro completato. La verifica costa
  dieci secondi e va fatta **prima** di marcare qualcosa come chiuso, non dopo:
  è l'unico momento in cui è ancora gratis.
