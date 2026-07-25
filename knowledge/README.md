# Libreria di squadra — contratto

Questa cartella è la memoria trasversale del team di agenti: conoscenza di
mestiere valida su **tutti i progetti**, non solo quello in corso.

## A cosa serve

Ogni ruolo (analyst, pm, ux, architect, dev, test-architect, tech-writer)
accumula lezioni lavorando sui progetti. Quelle lezioni, se generiche,
non devono ripartire da zero al progetto successivo. Questa cartella è il
posto dove restano.

## Il confine netto: sidecar di progetto vs libreria di squadra

Ogni progetto ha una **sidecar** (`_bmad/_memory/<ruolo>-sidecar/` nel
repo del progetto): fatti specifici di QUEL progetto — scelte fatte,
trappole locali, convenzioni di quel cliente. Restano lì, non arrivano
qui.

Questa **libreria** (`knowledge/<ruolo>/lezioni.md` in questo repo)
contiene solo conoscenza che si generalizza: un pattern, una trappola
tecnica ricorrente, un criterio di review, una checklist.

**Criterio di ammissione:** prima di scrivere qui, chiediti — *lo
riuseresti su un cliente diverso?* Se sì, libreria. Se no, resta nella
sidecar del progetto.

## Formato delle voci

Un fatto per voce, datato, con il perché conta:

```
- 2026-07-25 — <fatto o regola appresa>.
  Perché conta: <cosa succede se lo dimentichi / cosa ha causato>.
```

Niente prosa lunga, niente narrazione del progetto: solo la lezione,
pronta da riusare.

## Divieti assoluti

Non finisce mai qui:
- dati di clienti (nomi, numeri, contenuti di business specifici);
- segreti, credenziali, chiavi, URL privati;
- dettagli di business specifici di un progetto (quelli restano nella
  sidecar, o non si scrivono affatto).

Se hai un dubbio se una voce è troppo specifica, non scriverla: chiedi
all'umano.

## Come si scrive qui

Ogni aggiunta passa da **PR** su questo repo. Il merge resta **umano** —
nessun agente mergia su questo repo, nemmeno il proprio contributo.
Ogni ruolo scrive SOLO nel proprio `knowledge/<ruolo>/lezioni.md`: la
conoscenza la deposita chi l'ha prodotta sul campo, non chi la osserva.

## Struttura

| Percorso | Ruolo |
|---|---|
| `analyst/lezioni.md` | Analyst |
| `pm/lezioni.md` | Product Manager |
| `ux/lezioni.md` | UX |
| `architect/lezioni.md` | Architect |
| `dev/lezioni.md` | Developer |
| `test-architect/lezioni.md` | Test Architect |
| `tech-writer/lezioni.md` | Technical Writer |
