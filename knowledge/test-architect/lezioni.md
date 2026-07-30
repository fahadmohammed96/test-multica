# Lezioni — Test Architect (libreria di squadra)

Conoscenza di mestiere valida su tutti i progetti, appresa sul campo dal
ruolo Test Architect. Vedi `knowledge/README.md` per il contratto: un fatto per
voce, datato, con il perché conta; niente dati cliente o segreti; ogni
aggiunta via PR, merge umano.

<!-- Esempio:
- 2026-07-25 — <fatto appreso e perché conta>.
  Perché conta: <cosa succede se lo dimentichi>.
-->

- 2026-07-25 — Con gli hook di rete mockati, un bug di **invalidazione cache è invisibile
  per costruzione**: il mock risponde sempre il valore aggiornato, quindi il test di
  componente passa anche quando in produzione la UI resta ferma sul dato vecchio. I difetti
  di coerenza fra cache correlate (valore derivato che dipende da un'altra risorsa) si
  vedono solo end-to-end contro un backend reale.
  Perché conta: se accetti la copertura di componente come sufficiente per una UI con cache
  lato client, stai misurando il mock, non il prodotto — e il difetto arriva all'utente.
  Corollario operativo: quando un valore è derivato da un'altra risorsa, l'invalidazione va
  centralizzata in un unico punto, così la prossima mutazione non può dimenticarsene.

- 2026-07-25 — Un test di concorrenza con **troppi pochi contendenti passa a vuoto** e
  produce falsa sicurezza: una race su una finestra critica stretta può non riprodursi con
  2 thread e riprodursi sistematicamente con 8 allineati da un barrier. Prima di dichiarare
  chiuso un difetto di concorrenza, va **riprodotto in rosso** con il numero di contendenti
  che serve; e il barrier va messo fra i client, mai dentro il codice sotto test — se la
  correzione è basata su lock, un barrier interno manda il test in deadlock invece che in
  rosso, mascherando l'esito.
  Perché conta: un test di concorrenza verde che non ha mai visto la race è peggio di
  nessun test, perché chiude la questione per tutti quelli che verranno dopo.

- 2026-07-25 — Quando una guardia strutturale (meta-test che impone una convenzione su tutto
  il codice) ha bisogno di un'eccezione, l'eccezione va **esplicita in allowlist e a sua
  volta sorvegliata** da un test che la fa decadere se cambia la premessa — es. dati di
  riferimento condivisi esentati dallo scoping per tenant, più un test che fallisce se una
  di quelle tabelle acquisisce un legame con il tenant.
  Perché conta: le allowlist sono il punto in cui le guardie muoiono in silenzio. Esentare e
  fidarsi trasforma un invariante in una convenzione; esentare e sorvegliare l'esenzione lo
  mantiene tale anche fra sei mesi, quando la premessa non sarà più vera e nessuno se lo
  ricorderà.

- 2026-07-25 — Chiudere una fase a **«debito zero» obbliga a separare due tabelle**: i
  *finding chiusi* (ognuno con la PR che lo chiude e il **nome** del test di regressione) e i
  *rischi tracciati* — condizioni note che non violano alcun criterio di accettazione, non
  hanno un test rosso, ma hanno un **momento preciso di rivalutazione** (al prossimo bump
  della dipendenza, alla prima messa in esercizio dietro proxy, al secondo uso di quel
  meccanismo). Un finding si dichiara chiuso solo se puoi citare il test che lo terrebbe
  chiuso; un rischio si dichiara accettato solo se puoi citare quando lo riguarderai.
  Perché conta: senza la seconda tabella i rischi noti fanno una delle due brutte fini —
  inquinano il registro del debito (e allora «debito zero» diventa impossibile da
  raggiungere e la dichiarazione perde valore), oppure spariscono, e riappaiono mesi dopo
  come debito nato per dimenticanza. E senza il test nominato accanto a ogni finding chiuso,
  «chiuso» significa solo «qualcuno ha detto che è a posto»: la dichiarazione di chiusura
  diventa un atto di fiducia invece che un documento verificabile da chi non c'era.

- 2026-07-25 — **Il conteggio grezzo di `npm audit` non è un dato di qualità**, e il suo
  `fix` non è un consiglio. Due trappole che vanno smontate ogni volta: (1) l'audit somma
  toolchain di sviluppo e runtime — un progetto con 14 advisory *high* può averne 3 che
  arrivano davvero al bundle, e la separazione si legge solo con `npm audit --omit=dev`;
  (2) `fixAvailable` indica *la versione più vecchia senza advisory*, che per un pacchetto la
  cui storia è tutta più recente dell'advisory è una versione **precedente** — visto dal vero:
  `npm audit fix --force` proponeva un downgrade major da `next` 16 a `next` 9 come
  "correzione". La valutazione utile è sempre la stessa terna: la vulnerabilità è nel
  runtime o solo negli strumenti? il codice del progetto raggiunge quella superficie? e se
  non c'è patch, qual è l'**evento** che fa rivalutare (il prossimo bump), non la data?
  Perché conta: chi mette `npm audit` come gate senza il primo punto blocca i rilasci per
  advisory che non spediscono; chi applica `--force` senza il secondo rompe il progetto in
  nome della sicurezza. Entrambi gli errori sono facili da fare in buona fede, e il secondo
  passa la CI solo perché nessuno guarda quale versione è stata installata.

- 2026-07-25 — In ogni sincronizzazione da una sorgente esterna, la regola «l'elemento non è
  più nel payload ⇒ marcalo rimosso» **va condizionata a un parse completo e validato**, mai
  applicata al contenuto scaricato così com'è. Un corpo troncato, vuoto con esito 200, o
  parziale è indistinguibile — per quella regola — da «l'elemento è stato davvero cancellato
  all'origine»: il risultato è che un errore di **trasporto** diventa una **cancellazione di
  dati** che si propaga a tutto ciò che dipende da quello stato (derivati che decadono, allarmi
  che si chiudono, contatori che scendono). Il presidio è un criterio di completezza esplicito
  — sentinella di fine documento, conteggio coerente, dimensione attesa — e un test per ciascuna
  forma di troncamento.
  Perché conta: è l'unico difetto di questa famiglia che **non produce alcun errore visibile**.
  Il run risulta riuscito, i dati spariscono in silenzio, e la scoperta arriva dall'utente. Ha
  probabilità alta perché l'implementazione ingenua lo contiene per costruzione: nessuno scrive
  «se il parse è completo» finché non ha visto una risposta tagliata a metà.

- 2026-07-25 — Un criterio di accettazione che richiede un'**uscita osservabile** (un alert, un
  contatore, uno stato esposto) **non ha copertura possibile** se la superficie di osservabilità
  è pianificata in una fase successiva. Non è un problema di priorità dei test: è un AC senza
  conseguenza verificabile, che finisce inevitabilmente coperto «per ispezione». Quando pianifichi
  le fasi, controlla per ogni AC che l'uscita su cui si fonda esista nella stessa fase; se non
  esiste, le opzioni oneste sono due — anticipare il minimo di osservabilità, oppure **de-scopare
  l'AC per iscritto**. La terza (lasciarlo lì e dichiararlo coperto) è quella che si prende da sola.
  Perché conta: rende la domanda «anticipiamo l'osservabilità?» decidibile con un argomento
  tecnico invece che con una preferenza. E toglie di mezzo il caso peggiore, in cui un requisito
  di affidabilità risulta verde in una matrice di tracciabilità senza che nulla lo verifichi.

- 2026-07-30 — Un cancello di qualità va legato all'**artefatto immutabile** (lo SHA del
  commit, la digest dell'immagine, la versione dell'oggetto), mai al contenitore mutabile
  che lo ospita (la PR, il ramo, il tag). Motivo misurato, non teorico: subito dopo un push
  già completato, l'API che descrive la PR ha continuato a riportare la head **precedente**
  per qualche secondo. Ogni controllo del tipo «l'artefatto che sto giudicando è ancora
  quello corrente?» è quindi una cortesia soggetta a una finestra di corsa, e non può essere
  la garanzia. Se invece il verdetto è **attaccato allo SHA**, la finestra non è un buco: il
  giudizio finisce sul commit vecchio e quello nuovo nasce senza, cioè non approvato — non
  esiste ereditarietà da disattivare, e nessuno deve ricordarsi di invalidare niente.
  Perché conta: è la differenza fra un'approvazione che scade da sola a ogni push e una che
  resta appesa a un contenitore il cui contenuto è cambiato. La seconda forma è quella con
  cui si mergia in buona fede una versione che nessuno ha guardato.

- 2026-07-30 — Quando un cancello scrive **più di un artefatto**, l'ordine delle scritture è
  parte del progetto di sicurezza e va asserito da un test, non lasciato all'implementazione.
  Regola generale: la scrittura che **blocca** va per prima, quella che **sblocca** per
  ultima e solo dopo che tutto il resto è riuscito. Così ogni guasto parziale — permessi,
  rete, timeout a metà — atterra nello stato chiuso. Corollario spesso dimenticato: se la
  scrittura «umana» (il commento, la review, la notifica) passa e quella «meccanica» no, va
  pubblicata una **ritrattazione esplicita**, altrimenti resta in giro una pagina che afferma
  un via libera che non esiste.
  Perché conta: senza ordine dichiarato, metà dei percorsi di errore aprono il cancello per
  inerzia, e sono esattamente i percorsi che nessuno prova a mano. Il test che li copre non è
  «il caso raro»: è l'unico posto in cui quella decisione di progetto è scritta.

- 2026-07-30 — Prima di progettare un'automazione sopra un servizio esterno, **prova le
  chiamate reali con le credenziali reali**, anche quando l'API è documentatissima. Tre
  vincoli trovati in mezz'ora su GitHub, tutti invisibili ai mock e tutti capaci di cambiare
  il disegno: (1) i permessi di un token sono più granulari di come vengono descritti — poter
  scrivere contenuti e pull request **non** implica poter scrivere gli stati di commit; (2)
  un account non può approvare le proprie pull request (422), quindi se gli agenti pubblicano
  con lo stesso account che apre le PR, la review formale non è una strada percorribile e va
  scelto un artefatto diverso; (3) le letture possono essere in ritardo sulle scritture.
  Perché conta: ognuno dei tre trasforma un requisito scritto in buona fede in un requisito
  irrealizzabile. Scoprirli dopo aver costruito significa riprogettare; scoprirli prima
  significa scegliere l'artefatto giusto al primo colpo. Il costo del banco di prova
  usa-e-getta — un ramo e una PR aperta e chiusa — è due ordini di grandezza sotto.
