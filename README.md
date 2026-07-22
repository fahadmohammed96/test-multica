# Repo-base — punto di partenza di ogni progetto

Ogni nuovo progetto del team di agenti parte da qui. Contiene solo ciò
che serve ad avviare e condurre un progetto: NON i prompt degli agenti
(vivono nella configurazione di Multica).

| Percorso | Contenuto |
|---|---|
| `playbook.md` | I passi per avviare un progetto (0-6) e il regime |
| `templates/frontend-next/` | Repo-tipo frontend, verificato (Next.js + TS + Tailwind + Vitest + Playwright + CI) |
| `templates/backend-fastapi/` | Repo-tipo backend, verificato (FastAPI + pytest + ruff + CI) |
| `templates/_scheletro-generico/` | AGENTS.md universale per stack senza template |
| `issues/kickoff.md` | Template dell'issue di avvio (Fase 1 contesto, Fase 2 issue) |
| `issues/report-stato.md` | Template dell'issue ricorrente di monitoraggio |

## Il principio

L'AGENTS.md di ogni template ha due tipi di contenuto: la STRUTTURA
(le sezioni: universali, uguali per tutti i progetti) e i FATTI (veri
per lo stack di quel template). Progetto su uno stack coperto → copi il
template e i fatti sono già giusti. Stack nuovo → scheletro generico, e
il primo progetto che lo usa genera il template successivo.

Le lezioni imparate sul campo si correggono QUI, una volta, non a ogni
progetto.
