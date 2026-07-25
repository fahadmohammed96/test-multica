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
