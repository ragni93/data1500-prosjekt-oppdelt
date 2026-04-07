

### Oppgave 11: Staging av Finansielle Dokumenter med MongoDB

<span style="color:blue">**Hvordan besvare: Rapport-del svares i RAPPORT.md. Kode leveres i besvarelse/oppgave11. **</span>



**Mål:** Bruke en dokumentdatabase som et mellomlager for komplekse, semi-strukturerte API-responser.

**Teknologi:** **MongoDB** (Dokumentdatabase)

Tjenesten skal implementere følgende ETL-logikk (Extract, Transform, Load):

1.  **Extract:** Hent detaljert kurshistorikk for et verdipapir fra **Alpha Vantage**.
2.  **Load (rådata):** Lagre hele JSON-responsen som ett dokument i MongoDB-collectionen `raw_financial_data`. Hvert dokument skal inneholde feltene `ticker_symbol`, `fetch_timestamp` og `api_source` i tillegg til de rå API-dataene.
3.  **Transform & Load (SQL):** Trekk ut de relevante feltene (siste kurs og dato) og oppdater `Kurser`-tabellen i SQL-databasen **innenfor en transaksjon**.

**Krav til implementasjon:**
- Hent og lagre data for minst 3 ulike verdipapirer i MongoDB.
- Vis hvordan SQL-databasen oppdateres korrekt.
- Diskuter i rapporten: Hva er fordelen med å beholde rådataene i MongoDB selv etter at SQL-databasen er oppdatert? (Hint: tenk på feilsøking, historikk og muligheten for å re-prosessere data.)


---