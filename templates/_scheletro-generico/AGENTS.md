# Progetto

[DECISIONE] Cosa fa questo progetto e per chi — definire in Fase 1 del kickoff.

# Stack

[FATTO — compilare verificando nel repo dopo lo scaffolding]
- Linguaggio e versione:
- Framework:
- Gestione dipendenze (uno solo, dichiarato):
- Test:
- Lint/format:

# Comandi (verificati sul repo pulito)

[FATTO — ogni comando va ESEGUITO prima di scriverlo qui]
- Installazione:
- Avvio locale:
- Test:  — la suite deve passare prima di ogni PR
- Lint:  — il codice consegnato lo passa

# Struttura

[FATTO — descrivere la struttura reale delle cartelle e dove vanno i test]

# Convenzioni di codice

[FATTO/DECISIONE — naming, tipizzazione, docstring sulle API pubbliche]

# API (se il progetto espone API)

- Contratto: [FATTO — dove vive]
- Versionamento: [DECISIONE]
- Formato errori: [DECISIONE] — mai dettagli interni nelle risposte

# Dati e migrazioni (se il progetto ha un database)

- Strumento migrazioni: [DECISIONE]
- Ogni migrazione ha rollback funzionante; irreversibilità dichiarata ed
  eccezionale; modifiche distruttive sempre segnalate nella nota di consegna

# Test

- Strategia per database e servizi esterni nei test: [DECISIONE]
- Servizi esterni sempre mockati; test deterministici

# Log e sicurezza

- Log strutturati: [DECISIONE]. MAI dati sensibili o secret nei log.
- Secret solo da variabili d'ambiente (dichiarate in `.env.example`,
  nomi senza valori); mai nel codice o nella configurazione versionata

# Deploy

- [DECISIONE] Dove e come va online — definire in Fase 1.
- Il deploy NON è mai un task della squad: è un automatismo di
  piattaforma (es. deploy da main) o un gesto umano.

# Flusso di lavoro

- I task arrivano come issue su Multica; consegne SEMPRE via PR verso
  `main` (protetto), mai push diretto
- Ogni PR include la nota di consegna nel formato definito dalle
  istruzioni dell'agente
- La CI deve essere verde perché la PR sia approvabile
- Fuori scope per questo repo: [FATTO/DECISIONE — cosa compete ad altri
  repo o team] → segnalare sull'issue, non implementare
