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
