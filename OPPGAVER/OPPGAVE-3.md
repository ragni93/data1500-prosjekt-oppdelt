
### Oppgave 3: Grunnleggende SQL spørringer mot dobbelt bokholderi
<span style="color:blue">**Hvordan besvare: Kode leveres i besvarelse/oppgave1-9/test-scripts.**</span>

**Læringsmål:**

- Forstå grunnleggene SQL spørringer

#### Del A — Grunnleggende `SELECT` og `JOIN`

**A.1: Vis hele kontoplanen** Skriv en spørring som henter kontonummer, navn og kontoklasse for alle kontoer som har et kontonummer. Sorter resultatet etter kontonummer.

**A.2: Vis alle kontoklasser** Skriv en spørring som henter kode, navn og type (`BALANSE`/`RESULTAT`) for alle de åtte kontoklassene. Sorter etter klassenummer.

**A.3: Koble kontoer med klasser** Skriv en spørring som viser kontonummer, kontonavn og navnet på kontoklassen kontoen tilhører. Bruk en `JOIN`.

#### Del B: Aggregering med `GROUP BY`

**B.1: Antall posteringer per transaksjon** Skriv en spørring som viser bilagsnummer, beskrivelse og bilagsdato for hver transaksjon, sammen med antall posteringer den inneholder. Sorter etter dato.

**B.2: Saldo per konto** Skriv en spørring som beregner og viser den totale saldoen i kroner for hver konto som har posteringer. Vis kontonummer, navn og saldo. Sorter etter kontonummer.

#### Del C: Filtrering med `WHERE`, `HAVING` og `CASE`

**C.1: Finn MVA-pliktige eller placeholder-kontoer** Skriv en spørring som finner alle kontoer som enten er MVA-pliktige (`mva_pliktig = TRUE`) ELLER er en placeholder-konto (`er_placeholder = TRUE`).

**C.2: Vis lønnstransaksjonen med debet/kredit** Skriv en spørring som henter alle posteringer relatert til lønn (der transaksjonensbeskrivelse inneholder 'lønn'). Vis transaksjonsbeskrivelse, dato, kontonummer, kontonavn, beløp og en egen kolonne som sier 'Debet' eller 'Kredit' basert på fortegnet til beløpet.

**C.3: Antall kontoer per klasse** Skriv en spørring som viser antall reelle driftskontoer i hver av de åtte kontoklassene. Vis også hvor mange av disse som er markert som MVA-pliktige. Bruk `LEFT JOIN` for å inkludere klasser uten kontoer.

**C.4: Saldo for alle eiendelskontoer** Skriv en spørring som viser saldoen for alle reelle eiendelskontoer (klasse 1), inkludert de som har null i saldo. Bruk `LEFT JOIN`.

**C.5: Finn ubalanserte transaksjoner** Skriv en spørring som verifiserer dobbelt bokholderi-prinsippet. Spørringen skal finne alle transaksjoner der summen av `belop_teller` for alle tilhørende posteringer *ikke* er 0. Spørringen skal returnere et tomt resultat hvis databasen er i balanse.

#### Del D: Mer avanserte spørringer

**D.1: Vis alle MVA-beregninger** Skriv en spørring som henter alle MVA-linjer og kobler dem med MVA-koden og transaksjonen de tilhører. Vis MVA-kode, grunnlag, MVA-beløp og transaksjonsbeskrivelse.

**D.2: Vis alle valutakurser** Skriv en spørring som viser alle registrerte valutakurser. Vis 'fra'-valuta, 'til'-valuta, kurs og dato. Sorter etter nyeste kurs først.

**D.3: Antall transaksjoner per periode** Skriv en spørring som viser antall transaksjoner i hver regnskapsperiode som har transaksjoner. Vis periodenavn, datoer, status og antall transaksjoner.

**D.4: Total saldo per kontoklasse** Skriv en spørring som beregner den totale saldoen for hver kontoklasse. Vis klassenummer, klassenavn, type og totalsaldo.

**D.5: Detaljert analyse av resultatkontoer** Skriv en spørring som viser en detaljert analyse for alle resultatkontoer (klasse 3-8). Vis kontonummer, navn, antall posteringer, netto saldo, total debet, total kredit og gjennomsnittlig transaksjonsbeløp (absoluttverdi).

---