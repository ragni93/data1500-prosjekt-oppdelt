

### Oppgave 6: Databaseadministrasjon og Tilgangskontroll

<span style="color:blue">**Hvordan besvare: Kode leveres i besvarelse/oppgave1-9/test-scripts. OG/eller (?) RAPPORT.md**</span>

**Læringsmål:**

-   Forstå rollen som databaseadministrator (DBA).
-   Opprette brukere og roller i PostgreSQL.
-   Tildele rettigheter med `GRANT` og `REVOKE`.
-   Forstå og anvende prinsippet om minste rettighet (Principle of Least Privilege).
-   Implementere finkornet tilgangskontroll med Row Level Security (RLS).

**Scenario:**

DATA1500 Konsult AS har vokst og trenger en mer robust tilgangskontroll til regnskapsdatabasen. Du har fått i oppgave, i rollen som DBA, å implementere en sikkerhetsmodell som skiller mellom ulike ansvarsområder.

#### Del A: Opprett Roller og Brukere

Skriv et SQL-skript som oppretter følgende fire roller og fire brukere:

| Type | Navn | Beskrivelse |
|---|---|---|
| **Rolle** | `regnskap_admin` | Skal ha fulle rettigheter til å endre og administrere alle tabeller. |
| **Rolle** | `revisor` | Skal kun ha lesetilgang (`SELECT`) til alle tabeller. |
| **Rolle** | `regnskapsforer` | Skal kunne lese alle tabeller, men kun sette inn og oppdatere transaksjonsdata (`Transaksjoner`, `Posteringer`, `MVA-linjer`). Skal **ikke** kunne slette. |
| **Rolle** | `les_tilgang` | Svært begrenset rolle som kun kan lese oppslagstabeller (`Kontoer`, `Kontoklasser`, `Valutaer`). |
| **Bruker** | `dba_ola` | En DBA som skal tildeles `regnskap_admin`-rollen. |
| **Bruker** | `revisor_kari` | En intern revisor som skal tildeles `revisor`-rollen. |
| **Bruker** | `bokforer_per` | En regnskapsmedarbeider som skal tildeles `regnskapsforer`-rollen. |
| **Bruker** | `ekstern_revisor` | En ekstern konsulent som kun skal ha den mest begrensede tilgangen (`les_tilgang`). |

#### Del B: Tildel Rettigheter (`GRANT`)

Bruk `GRANT`-kommandoer for å tildele de nødvendige rettighetene til hver av de fire rollene, og deretter tildel rollene til de respektive brukerne.

#### Del C: Verifisering og Testing

1.  Skriv en spørring mot `information_schema.role_table_grants` for å verifisere at rettighetene er tildelt korrekt.
2.  Bruk `SET ROLE`-kommandoen for å teste tilgangskontrollen. Vis med konkrete SQL-kall at:
    -   `bokforer_per` **ikke** kan slette fra `Transaksjoner`.
    -   `ekstern_revisor` **ikke** kan lese fra `Transaksjoner`.
    -   `ekstern_revisor` **kan** lese fra `Kontoer`.

#### Del D: Row Level Security (RLS)

Som en ekstra sikkerhetsmekanisme skal du implementere en policy som begrenser hvilke rader en regnskapsfører kan se.

1.  Aktiver RLS på `Transaksjoner`-tabellen.
2.  Opprett en policy kalt `policy_aar_filter` som sikrer at brukere med `regnskapsforer`-rollen kun kan se transaksjoner der `bilagsdato` er i inneværende år.
3.  Opprett en policy kalt `policy_revisor_alt` som sikrer at `revisor`-rollen fortsatt kan se alle transaksjoner (ellers vil de også bli begrenset av standard `DENY`-policyen).

#### Del E: Diskusjon

Forklar i rapporten:

-   Hva er forskjellen på en `ROLE` og en `USER` i PostgreSQL?
-   Hvorfor er det god praksis å tildele rettigheter til roller i stedet for direkte til brukere?
-   Hvilken fordel gir Row Level Security sammenlignet med å kun bruke `GRANT`? Gi et konkret eksempel fra regnskapsmodellen.

---

**Oppgaver 7 - 11** belyser to viktige aspekter som spiller en avgjørende rolle i implementasjon av databasemodeller:

- For det første introduseres **dybde i SQL-database** gjennom obligatoriske oppgaver som dekker avansert transaksjonsbehandling, feilhåndtering og samtidighetskontroll. Disse temaene er avgjørende for å bygge systemer som er pålitelige nok til å håndtere finansielle data — der en feil kan bety at penger "forsvinner" fra systemet.

- For det andre introduseres **bredde i arkitektur** gjennom en utvidelse som krever at studentene bygger en ekstern mikrotjeneste. Denne tjenesten integrerer en NoSQL-database med den eksisterende SQL-databasen og henter sanntidsdata fra åpne finansielle API-er. Formålet er å gi studentene praktisk erfaring med **polyglot persistens**, der ulike databaseteknologier brukes til det de er best egnet for.

**Bakgrunn: Transaksjoner i Dobbelt Bokholderi**

I dobbelt bokholderi er en regnskapstransaksjon (f.eks. "betal leverandør 5000 kr") alltid en sammensatt operasjon: én rad settes inn i `Transaksjoner`-tabellen, og minst to rader settes inn i `Posteringer`-tabellen — én for debet og én for kredit. Summen av alle `belop_teller`-verdier i `Posteringer` for én transaksjon skal alltid være null.

Denne sammensatte operasjonen er et klassisk eksempel på en situasjon der **atomisitet** er absolutt nødvendig. Hvis systemet krasjer etter at `Transaksjoner`-raden er satt inn, men før `Posteringer`-radene er skrevet, vil databasen inneholde en "halvferdig" transaksjon som bryter med det grunnleggende prinsippet i dobbelt bokholderi. Det er nettopp for å forhindre slike situasjoner at DBHS-er implementerer transaksjonsbehandling med ACID-egenskapene.

> **ACID** er et akronym for **Atomicity** (atomisitet), **Consistency** (konsistens), **Isolation** (isolasjon) og **Durability** (varighet). Disse fire egenskapene garanterer at databasetransaksjoner behandles pålitelig.

**OBS!** Disse temaene skal gjennomgås på forelesninger.

---
