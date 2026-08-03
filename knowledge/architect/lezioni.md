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

- 2026-08-03 - Se un sistema di agenti ha un banco di prova, il banco deve eseguire ESATTAMENTE gli artefatti di produzione: step agente definiti come dati (prompt + contratto input + contratto output + validatore) eseguiti da un runner unico, e caso di test che replica per intero la struttura dell'unita' di produzione, con un solo entrypoint di pipeline e il divieto esplicito di rami di codice "solo test". Corollario: l'esito verde/rosso del banco deve uscire solo da controlli deterministici; un LLM-giudice nell'esito eredita i bias di chi ha scritto i casi, e cio' che non e' verificabile deterministicamente va dichiarato "non provato", mai contato verde.
  Perche' conta: se banco e produzione divergono anche solo nel formato dei casi o nella copia dei prompt, la fiducia costruita sul banco si trasferisce a un sistema mai testato - e' il modo piu' silenzioso in cui una suite smette di misurare, e nessun singolo commit sembra il colpevole.