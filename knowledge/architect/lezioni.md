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
