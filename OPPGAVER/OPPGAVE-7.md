**Oppgaver 7 - 11** belyser to viktige aspekter som spiller en avgjørende rolle i implementasjon av databasemodeller:

- For det første introduseres **dybde i SQL-database** gjennom obligatoriske oppgaver som dekker avansert transaksjonsbehandling, feilhåndtering og samtidighetskontroll. Disse temaene er avgjørende for å bygge systemer som er pålitelige nok til å håndtere finansielle data — der en feil kan bety at penger "forsvinner" fra systemet.

- For det andre introduseres **bredde i arkitektur** gjennom en utvidelse som krever at studentene bygger en ekstern mikrotjeneste. Denne tjenesten integrerer en NoSQL-database med den eksisterende SQL-databasen og henter sanntidsdata fra åpne finansielle API-er. Formålet er å gi studentene praktisk erfaring med **polyglot persistens**, der ulike databaseteknologier brukes til det de er best egnet for.

**Bakgrunn: Transaksjoner i Dobbelt Bokholderi**

I dobbelt bokholderi er en regnskapstransaksjon (f.eks. "betal leverandør 5000 kr") alltid en sammensatt operasjon: én rad settes inn i `Transaksjoner`-tabellen, og minst to rader settes inn i `Posteringer`-tabellen — én for debet og én for kredit. Summen av alle `belop_teller`-verdier i `Posteringer` for én transaksjon skal alltid være null.

Denne sammensatte operasjonen er et klassisk eksempel på en situasjon der **atomisitet** er absolutt nødvendig. Hvis systemet krasjer etter at `Transaksjoner`-raden er satt inn, men før `Posteringer`-radene er skrevet, vil databasen inneholde en "halvferdig" transaksjon som bryter med det grunnleggende prinsippet i dobbelt bokholderi. Det er nettopp for å forhindre slike situasjoner at DBHS-er implementerer transaksjonsbehandling med ACID-egenskapene.

> **ACID** er et akronym for **Atomicity** (atomisitet), **Consistency** (konsistens), **Isolation** (isolasjon) og **Durability** (varighet). Disse fire egenskapene garanterer at databasetransaksjoner behandles pålitelig.

**OBS!** Disse temaene skal gjennomgås på forelesninger.


### Oppgave 7: Atomisk Regnskapspostering (K10.1, K10.2)

<span style="color:blue">**Hvordan besvare: Kode leveres i besvarelse/oppgave1-9/test-scripts. OG/eller (?) RAPPORT.md**</span>

**Mål:** Implementere og demonstrere at en regnskapspostering i dobbelt bokholderi er en udelelig (atomisk) operasjon.

**Læringsutbytte:** Studenten forstår og kan anvende `DO`-blokk, `BEGIN`, `COMMIT` og `ROLLBACK` for å sikre dataintegritet.

#### Implementasjonskrav

Løsningen skal bruke en anonym `DO`-blokk i PostgreSQL, som er en enkel måte å kjøre prosedyrisk kode på uten å måtte definere en permanent funksjon. Hele blokken kjøres innenfor en `BEGIN`/`COMMIT`-transaksjon.

Det er to måter å `DO`-blokk direkte i psql (eller utføre som skript):

1. Når du kjører `DO` direkte i psql uten en forutgående `BEGIN`, oppretter PostgreSQL automatisk en ny transaksjon for hele `DO`-setningen. I dette tilfellet er det tillatt å bruke transaksjonskommandoer som `COMMIT` og `ROLLBACK` inni `DO`-blokken, fordi blokken "eier" sin egen transaksjonskontekst.

```sql
DO $$
BEGIN
    INSERT INTO "Transaksjoner" (...) VALUES (...);
    COMMIT;   -- ✓ Tillatt her
    INSERT INTO "Posteringer" (...) VALUES (...);
    COMMIT;   -- ✓ Tillatt her
END;
$$;
```

2. Når du kjører `DO` etter en `BEGIN`-setning, er transaksjonen allerede startet av den ytre BEGIN. `DO`-blokken arver denne transaksjonen og kan ikke bruke `COMMIT` eller `ROLLBACK` inni seg — fordi det ville forstyrre den ytre transaksjonens kontroll.

```sql 
BEGIN;
    DO $$
    BEGIN
        INSERT INTO "Transaksjoner" (guid, bok_guid, valuta_guid, bilagsnummer, bilagsdato,
         posteringsdato, beskrivelse, periode_guid) VALUES (...);
        COMMIT; -- Ikke tillatt her
    END;
    $$;
COMMIT;
```

Det anbefales å bruke `DO`-blokk på måten (2.).


#### Demonstrasjonskrav

Studenten skal demonstrere to scenarier:

**Scenario A — Vellykket postering:** En gyldig transaksjon der debet og kredit balanserer. Vis at begge tabellene (`Transaksjoner` og `Posteringer`) er korrekt oppdatert etter `COMMIT`.

Følgende er gitt:
- Transaksjon: Kjøp av kontorrekvisita 2 000 kr + 25% MVA = 2 500 kr
- Debet: 6560 Rekvisita           2 000 kr  (+200000 øre)
- Debet: 2710 Inngående MVA         500 kr  ( +50000 øre)
- Kredit: 2400 Leverandørgjeld    2 500 kr  (-250000 øre)
- Balanse: 200000 + 50000 - 250000 = 0   (skal sjekkes etter at transaksjonen er gjennomført)

**Scenario B — Mislykket postering:** Forsøk å postere til en konto-GUID som ikke finnes i `Kontoer`-tabellen. Vis at `ROLLBACK` utføres og at databasen er uendret — verken `Transaksjoner`-raden eller noen `Posteringer`-rader er satt inn.

Bruk av `exception`-struktur i PL/pgSQL:
```sql 
BEGIN

DO $$
DECLARE
    -- Bevisst ugyldig: 32 tegn, men finnes ikke i Kontoer-tabellen
	v_ugyldig_guid CHAR(32) := 'FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF';
	-- de andre guid som man trenger for INSERT
	-- v_book_guid CHAR(32)
	-- ...
BEGIN
-- Bruker select for å finne de nødvendige guid-er	
-- Bruk STRICT for å unngå feil hvis `select` returnerer ingen rader
-- SELECT guid INTO STRICT v_bok_guid FROM "Bøker" LIMIT 1; 
-- ...	
-- INSERT INTO "Transaksjoner" (guid, bok_guid, valuta_guid, bilagsnummer, bilagsdato,
--         posteringsdato, beskrivelse, periode_guid) VALUES (...);
-- INSERT INTO "Posteringer"
--        (guid, transaksjon_guid, konto_guid, tekst,
--         belop_teller, belop_nevner, antall_teller, antall_nevner)
--    VALUES (...);

-- En anbefalt måte å takle feil på i PostgreSQL
EXCEPTION
    -- Når man bruker STRICT i SELECT INTO som brukes videre i INSERT er det viktig
    -- å fange tilfeller hvor SELECT returnerer ingen rader
	WHEN NO_DATA_FOUND THEN
        RAISE NOTICE 'FEIL: Et nødvendig oppslag returnerte ingen rad (konto, valuta eller periode mangler).';
        RAISE;  -- Re-kast → ytre transaksjon markeres ABORTED
    -- Prøver å fange en spesifikk unntak/feil
    WHEN foreign_key_violation THEN
        RAISE NOTICE 'FEIL FANGET: Fremmednøkkelbrudd — konto_guid "%" finnes ikke i Kontoer.', v_ugyldig_guid;
        RAISE NOTICE 'ROLLBACK vil utføres automatisk for hele transaksjonen.';
        RAISE;  -- Re-kast unntaket for å trigge ROLLBACK
    -- Alle andre feil blir også behandlet og feilmelding fra postgreSQL vist
    -- SQLSTATE inneholer PostgreSQL sin feiltypekode, f.eks. 23503 for FK-brudd
    -- https://www.postgresql.org/docs/current/errcodes-appendix.html
    -- SQLERRM inneholder feilmeldingsteksten 
    WHEN OTHERS THEN
        RAISE NOTICE 'UVENTET FEIL: SQLSTATE=%, MELDING=%', SQLSTATE, SQLERRM;
        RAISE; -- Re-kast unntaket for å trigge ROLLBACK
END $$

ROLLBACK;

\echo '--- Verifisering etter ROLLBACK ---'
-- Bekreft at den feilede transaksjonen IKKE finnes
SELECT
    CASE WHEN COUNT(*) = 0
         THEN 'OK: B-2026-FEIL finnes IKKE i databasen (ROLLBACK virket)'
         ELSE 'FEIL: B-2026-FEIL ble lagret til tross for ROLLBACK!'
    END AS rollback_status
FROM "Transaksjoner"
WHERE bilagsnummer = 'B-2026-FEIL';
``` 

- For feilkoder og unntaksnavn (som `foreign_key_violation`) i PostgreSQL se https://www.postgresql.org/docs/current/errcodes-appendix.html




#### Rapportkrav

Diskuter i rapporten:
- Hva ville ha skjedd uten transaksjonsbehandling dersom systemet krasjet mellom innsetting av `Transaksjoner`-raden og `Posteringer`-radene?
- Forklar hvilken av ACID-egenskapene som er mest kritisk for dobbelt bokholderi, og begrunn svaret.

---
