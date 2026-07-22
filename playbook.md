# Playbook — nuovo progetto

Gli agenti, la squad, le skill e gli MCP sono infrastruttura permanente:
non si toccano. Un nuovo progetto è: un posto dove lavorare + un obiettivo.

## Passo 0 — Classe di progetto e stack (DECISIONE)
Scegli la classe: frontend web (`templates/frontend-next/`), servizio
backend (`templates/backend-fastapi/`), oppure uno stack senza template:
in quel caso scaffolding a mano + `templates/_scheletro-generico/`
(AGENTS.md con struttura universale e fatti da compilare), e se lo stack
tornerà lo promuovi a nuovo template. Se incerto sullo stack: issue al
CoS "obiettivo X: proponimi classe e stack, con opzioni e trade-off" e
scaffolding solo dopo la risposta.

## Passo 1 — Repo e scaffolding (umano)
Crea i repository, copia il template della classe, commit iniziale.
Unico task fuori mandato per gli agenti: i loro prompt presuppongono un
progetto esistente. (Variante: se la squad ha accesso a questa libreria,
la copia del template la fa il CoS in Fase 1 del kickoff.)

## Passo 2 — Contesto
Il template include già `AGENTS.md` (fatti dello stack compilati,
`[DECISIONE]` come segnaposto) e `CLAUDE.md` (`@AGENTS.md`). Verifica che
ci siano, adatta il nome del progetto.

## Passo 3 — GitHub: regole fisiche
Branch protection su `main` (require PR; required check: il job di
qualità della CI). La CI è nel template: lint + test + build su ogni PR,
E2E post-merge. Mettila da subito anche se i primi merge li approvi tu:
è il prerequisito dell'auto-merge futuro.

## Passo 4 — Multica: collegamenti
Collega i repo alla squad (la stessa: squad nuova solo per un REPARTO
nuovo, non per un progetto nuovo). Verifica che ogni agente veda il repo
di competenza.

## Passo 5 — Kickoff
Apri l'issue da `issues/kickoff.md`, assegnata ALLA SQUAD. Tuoi momenti:
rispondere all'intervista (un giro), approvare le PR di contesto,
approvare l'elenco delle issue. Tutto il resto è loro.

## Passo 6 — Regime
Parla per obiettivi, sempre alla squad, mai ai singoli (assegnare al
singolo bypassa il leader e spegne l'autonomia). Rientri su: escalation,
decisioni che il CoS ti porta, merge — finché, a fiducia guadagnata e CI
richiesta, non attivi l'auto-merge e resti su escalation e decisioni.
Periodicamente: issue `issues/report-stato.md` al CoS (il monitoraggio
non esiste se nessuno lo invoca).

## I tre verbi dell'umano
Decidere, approvare, mergiare. Se ti accorgi di scrivere issue tecniche
dettagliate o di assegnare ai singoli, non stai usando il sistema: lo
stai sostituendo.
