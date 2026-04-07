
### Oppgave 4: Hierarkisk Kontoplan med `WITH RECURSIVE`

<span style="color:blue">**Hvordan besvare: Kode leveres i besvarelse/oppgave1-9/test-scripts.**</span>

#### Del A — Rekursiv traversering

Skriv en rekursiv CTE-spørring som henter hele kontoplanen og presenterer den med innrykk for å vise hierarkiet. Resultatet skal vise kontonummer, kontonavn, dybdenivå og en `sti`-kolonne som viser hele veien fra rot til konto.

```sql
-- forventet output første  5 linjer 
 nivaa | kontonr  |                navn                |                              sti                               
-------+----------+------------------------------------+----------------------------------------------------------------
     0 | —        | Rotkonto                           | Rotkonto
     1 |   —      |   Annen driftskostnad              | Rotkonto > Annen driftskostnad
     2 |     6560 |     Rekvisita                      | Rotkonto > Annen driftskostnad > Rekvisita
     1 |   —      |   Egenkapital og gjeld             | Rotkonto > Egenkapital og gjeld
     2 |     2000 |     Aksjekapital                   | Rotkonto > Egenkapital og gjeld > Aksjekapital
     ...
```

#### Del B — Aggregert saldo oppover i hierarkiet

Utvid spørringen fra Del A. Bruk en CTE (Common Table Expression) til å først beregne saldoen for hver enkelt konto (basert på `Posteringer`). Deretter, i den rekursive delen, aggreger saldoene oppover slik at overordnede kontoer summerer saldoene til alle sine underkontoer. Presenter en fullstendig saldobalanse for alle balansekontoer (klasse 1 og 2), sortert etter kontonummer.

---
