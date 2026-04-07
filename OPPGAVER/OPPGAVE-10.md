**Arkitektur for oppgaver 10 og 11**



Studentene skal implementere en frittstående tjeneste (f.eks. et Python-skript) som:

1.  Kommuniserer med det valgte eksterne API-et.
2.  Bruker en NoSQL-database for mellomlagring eller staging av rådata.
3.  Oppdaterer den sentrale SQL-databasen med prosesserte data, **alltid innenfor en SQL-transaksjon** (jf. Oppgaver 7-9).

Det siste punktet er en bevisst kobling til transaksjonsoppgavene: selv den eksterne tjenesten må sikre at oppdateringer av `Verdipapirer` og `Kurser` er atomiske.

---


### Oppgave 10: Sanntids Valutakurs-Cache med Redis

<span style="color:blue">**Hvordan besvare: Rapport-del svares i RAPPORT.md. Kode leveres i besvarelse/oppgave10. **</span>


**Mål:** Implementere en effektiv cache-mekanisme for å redusere antall kall mot et eksternt API.

**Teknologi:** **Redis** (Key-Value Store)

Tjenesten skal implementere følgende cache-logikk:

1.  Konstruer en unik nøkkel for valutaparet, f.eks. `price:USD:NOK`.
2.  Sjekk om nøkkelen finnes i Redis (`GET price:USD:NOK`).
    -   **Cache Hit:** Returner den lagrede verdien umiddelbart uten å kalle API-et.
    -   **Cache Miss:** Kall det eksterne API-et (**Norges Bank API**), lagre resultatet i Redis med en TTL på 1 time (`SET price:USD:NOK 9.65 EX 3600`), og oppdater `prices`-tabellen i SQL-databasen **innenfor en transaksjon**.

**TTL** - `time to live` eller `hop limit` er en mekanisme som begrenser *livstid* til data i en datamaskin eller i et nettverk.

**Krav til implementasjon:**
- Skriv et skript som simulerer 10 påfølgende kall for samme valutapar og logger om hvert kall resulterte i Cache Hit eller Cache Miss.
- Diskuter i rapporten: Hva er konsekvensen for datakonsistens dersom Redis-cachen inneholder en foreldet kurs og en bruker registrerer en transaksjon basert på den? Hvordan kan dette håndteres?
