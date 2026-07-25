# Lezioni — Technical Writer (libreria di squadra)

Conoscenza di mestiere valida su tutti i progetti, appresa sul campo dal
ruolo Technical Writer. Vedi `knowledge/README.md` per il contratto: un
fatto per voce, datato, con il perché conta; niente dati cliente o
segreti; ogni aggiunta via PR, merge umano.

- 2026-07-25 — Il lavoro del tech writer parte tardi nel ciclo BMAD: finché
  il progetto è ancora in Fase 1 (Analysis, gate G1) non esistono artefatti
  approvati da fasi successive (PRD, architettura, codice) da documentare
  con DP/WD. La sidecar di progetto resta legittimamente vuota in questa
  fase, e non va forzata con contenuti prematuri solo per "avere qualcosa".
  Perché conta: scrivere documentazione prima che l'artefatto a monte sia
  approvato viola il principio "il codice e gli artefatti approvati sono
  la verità, mai le intenzioni" — meglio verificare lo stage del progetto
  (issue, PR mergiate, gate superati) prima di aspettarsi lavoro da fare,
  ed eventualmente segnalarlo invece di inventare materiale.

- 2026-07-25 — Per documentare "cosa fa il prodotto oggi" in un progetto
  ancora in costruzione, non fidarti dell'assenza di una feature in
  `epics.md`/PRD per descriverla come "non ancora costruita": apri il
  codice della UI/route corrispondente e verifica che dichiari
  esplicitamente il proprio stato (es. un componente placeholder con testo
  "arriva con il prossimo ciclo di lavoro"), non solo che manchi la
  logica. Perché conta: una voce di navigazione che esiste ma è vuota può
  sembrare un bug al lettore se il documento non chiarisce che è uno stato
  atteso — il segnale giusto è nel codice (la pagina *si dichiara* incompleta),
  non nel piano.

- 2026-07-25 — Una matrice di tracciabilità/chiusura QA (finding chiusi,
  AC coperti, dichiarazione di debito zero) è un'ottima mappa di ciò che è
  *verificato*, ma resta un documento di un altro ruolo: prima di citarla
  come fonte in un documento di prodotto, riverifica a campione le sue
  affermazioni sul codice stesso (es. che le route placeholder esistano
  davvero, che i cap/i flag descritti corrispondano a ciò che è in `main`).
  Perché conta: la matrice descrive cosa è stato testato in passato, non è
  la verità sul codice al momento in cui scrivi — usarla come scorciatoia
  senza un controllo diretto rischia di propagare un errore o uno stato
  superato.

- 2026-07-25 — Separare esplicitamente in due sezioni distinte "cosa il
  prodotto fa oggi" e "confini onesti / cosa manca ancora" (invece di un
  'unica narrazione mista) rende molto più facile sia scrivere sia
  revisionare senza scivolare da fatti a intenzioni: ogni riga della prima
  sezione richiede una prova nel codice, ogni riga della seconda richiede
  una prova che il prodotto stesso dichiari l'assenza (non solo che manchi).
  Perché conta: è la struttura più efficace trovata finora per evitare di
  "documentare intenzioni invece di fatti" su un prodotto che nasce una
  story alla volta — vale su qualunque cliente con consegne incrementali.

<!-- Prossime voci:
- 2026-07-25 — <fatto appreso e perché conta>.
  Perché conta: <cosa succede se lo dimentichi>.
-->
