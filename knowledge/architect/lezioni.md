# Lezioni — Architect (libreria di squadra)

Conoscenza di mestiere valida su tutti i progetti, appresa sul campo dal
ruolo Architect. Vedi `knowledge/README.md` per il contratto: un fatto per
voce, datato, con il perché conta; niente dati cliente o segreti; ogni
aggiunta via PR, merge umano.

<!-- Esempio:
- 2026-07-25 — <fatto appreso e perché conta>.
  Perché conta: <cosa succede se lo dimentichi>.
-->

- 2026-07-25 — Ogni URL fornito dall'utente e dereferenziato dal server (feed di calendario, webhook, import "da link") è una superficie SSRF che va governata da un invariante architetturale esplicito, non lasciata al codice. Contenuto minimo dell'invariante: schemi ammessi (`http`/`https` soltanto, mai `file`/`gopher`/`ftp`); classe di indirizzi vietata valutata sull'**indirizzo risolto dal DNS** — non sulla stringa dell'URL — (loopback, reti private, link-local, metadati d'istanza), rivalidata **dopo ogni redirect**; timeout e cap di dimensione come configurazione; rifiuto senza divulgazione (errore d'uso per l'utente, mai un oracolo della rete interna). Denylist vs allowlist vs proxy di egress è una decisione con trade-off (l'allowlist rompe le integrazioni minori, il proxy costa infrastruttura): va registrata come tale nello spine, con le alternative come opzioni note.
  Perché conta: se nessun artefatto d'architettura nomina la politica di egress, ogni implementatore ne inventa una (o nessuna) e la falla emerge in review o in produzione; senza requisito registrato nell'inventario, i test finiscono ancorati a un requisito sbagliato o inesistente e la copertura sembra esserci senza esserci.
- 2026-07-30 — Una lista esaustiva in un invariante («le UNICHE cancellazioni ammesse sono…») è decidibile solo se il perimetro del sostantivo che governa è scritto accanto alla lista. «Distruzione di dato» senza dire QUALE dato — dato di dominio (entità, evidenza, dato personale) vs stato operativo (code di job, sessioni scadute, righe di trasporto consegnate) — si legge in due modi opposti sulla stessa `DELETE`, entrambi ragionevoli. Il segnale empirico dell'ambiguità è due escalation indipendenti sulla stessa domanda; il precedente silenzioso (una purge equivalente già in produzione senza che nessuno invocasse l'invariante) rivela quale lettura è quella operante. Rimedio: emendare il testo definendo il perimetro e le condizioni esplicite del fuori-perimetro — non allungare la lista caso per caso, e non lasciare che ogni occorrenza costi un'escalation.
  Perché conta: ogni sistema con un invariante «archiviare, mai distruggere» incontra prima o poi la manutenzione del proprio stato operativo (retention delle code, purge sessioni, outbox consegnata); se il perimetro non è scritto, la prima retention di infrastruttura o passa in silenzio con letture divergenti, o si ferma davanti a un invariante che non parlava di lei.
