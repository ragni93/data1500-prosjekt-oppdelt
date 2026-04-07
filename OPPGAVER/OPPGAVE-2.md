
### Oppgave 2: Populering med testdata
<span style="color:blue">**Hvordan besvare: Kode leveres i besvarelse/oppgave1-9/test-scripts.**</span>

Skriv et SQL-skript (`oppgave2.sql` legges i mappen `besvarelse/test-scripts`) som implementerer alle åtte brukerscenarioene fra kapittel 4. Dataene skal inkludere:

- Valutaer: NOK, USD, SEK.
- Valutakurser: USD/NOK og SEK/NOK for relevante datoer.
- Alle åtte kontoklasser med korrekte `type` og `normal_saldo`.
- Kontoer for alle kontonumre som er brukt i scenarioene.
- Én regnskapsperiode per måned (12 regnskapsperioder for 2026).
- Alle åtte transaksjonene med tilhørende posteringer.
- MVA-koder for utgående (25%) og inngående (25%) MVA, samt tilhørende MVA-linjer.

**Verifisering:** Etter populering skal følgende spørring returnere null rader (alle transaksjoner balanserer):

```sql
SELECT t.guid, t.beskrivelse, SUM(p.belop_teller::numeric / p.belop_nevner) AS saldo
FROM "Transaksjoner" t
JOIN "Posteringer" p ON p.transaksjon_guid = t.guid
GROUP BY t.guid, t.beskrivelse
HAVING ABS(SUM(p.belop_teller::numeric / p.belop_nevner)) > 0.001;
```

#### Spesifikke krav til Oppgave 2 (dette skal gjennomgås på forelesninger)

**`DO $$...$$`-blokk for Transaksjoner og Posteringer**

Alle transaksjoner og posteringer skal opprettes i én anonym PL/pgSQL-blokk (`DO $$...$$`). Dette gjør det mulig å bruke lokale variabler (`tx_guid`, `bok_guid` osv.) som holder GUIDs mellom INSERT-setninger. Alternativet er å bruke delspørringer for dette men det kan gjore koden svært vanskelig å lese.

`plpgsql`-utvidelsen er vanligvis installert i PostgreSQL som er i Docker imagen. PL/pgSQL er et prosedyrespråk for PostgreSQL-databasesystemet. Se https://www.postgresql.org/docs/current/plpgsql-overview.html for flere detaljer.

**Brøkrepresentasjon i praksis**

Alle beløp lagres med `belop_nevner = 100` (standard), slik at beløp i øre lagres i `belop_teller`. Eksempel: 200 000 kr lagres som `belop_teller = 20000000` (to hundre millioner øre). Dette er konsistent med GnuCash-modellen og brukes i regnskapessystemer for å unngå desimalfeil.

| Beløp i kr | `belop_teller` | `belop_nevner` | Beregning |
|---|---|---|---|
| 200 000,00 | 20 000 000 | 100 | 20 000 000 / 100 = 200 000 |
| 4 375,00 | 437 500 | 100 | 437 500 / 100 = 4 375 |
| 875,00 | 87 500 | 100 | 87 500 / 100 = 875 |
| 62 500,00 | 6 250 000 | 100 | 6 250 000 / 100 = 62 500 |

**Fortegnskonvensjon (positiv = debet, negativ = kredit)**

GnuCash-modellen bruker en enkel fortegnskonvensjon: **positivt beløp = debet**, **negativt beløp = kredit**. Dobbelt bokholderi-prinsippet håndheves av at summen av alle `belop_teller`-verdier for én transaksjon alltid skal være **0**. Dette er den eneste integritetsregelen som ikke kan håndheves med en enkel `CHECK`-betingelse — den krever en aggregatspørring over `Posteringer` gruppert på `transaksjon_guid`.

**Hierarkisk kontoplan med placeholder-kontoer**

Kontoplanen bygges i tre nivåer:
1. **Rotkonto** — én enkelt placeholder som er overordnet alt.
2. **Klassekontoer** — åtte placeholder-kontoer, én per NS 4102-klasse.
3. **Driftskontoer** — de faktiske kontoene med 4-sifrede kontonumre.

Placeholder-kontoer (`er_placeholder = TRUE`) kan ikke posteres på direkte — de er kun beholdere i hierarkiet. Dette muliggjør rekursive spørringer med `WITH RECURSIVE` i Oppgave 3.

**Scenario 7 — MVA-avregning uten `2740`-postering**

I scenario 7 er `2740 Oppgjørskonto MVA` utelatt fra posteringene. Scenarioteksten viser kontoen, men i praksis er det vanlig å postere direkte fra `2700`/`2710` til `1920` uten å gå via en oppgjørskonto. Begge tilnærminger er korrekte — oppgjørskontoen er nyttig for revisjonsspor, men ikke strengt nødvendig. Man kan velge hvilken variant man bruker, så lenge transaksjonen balanserer.

---
