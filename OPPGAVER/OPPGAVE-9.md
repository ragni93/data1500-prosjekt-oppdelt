### Oppgave 9: Samtidighetsproblemer og Låsing (K10.7, K10.8, K10.9, K10.10, K10.11)

<span style="color:blue">**Hvordan besvare: Kode leveres i besvarelse/oppgave1-9/test-scripts.**</span>


**Mål:** Forstå, demonstrere og løse "tapt oppdatering" (lost update) — et klassisk samtidighetsproblem i flerbrukerdatabaser.

**Læringsutbytte:** Studenten kan forklare *les-beregn-skriv*-syklusen, forstår behovet for låsing, og kan implementere korrekte løsninger.

#### Scenario: Dobbel Betaling

To regnskapsmedarbeidere, Ane og Bjørn, jobber samtidig mot den samme bedriftskontoen (en konto av typen `BANK` i `Kontoer`-tabellen). Hvis kontoen har en saldo på 10 000 kr (representert som summen av alle tilknyttede `Posteringer.belop_nevner`).

-   **Ane (Prosess A):** Skal registrere en utbetaling på 3 000 kr.
-   **Bjørn (Prosess B):** Skal registrere en innbetaling på 1 500 kr.

Korrekt sluttresultat: 10 000 − 3 000 + 1 500 = **8 500 kr**.

#### Demonstrer Problemet (Tapt Oppdatering)

*GnuCash/NS 4102-modellen* er immun mot tapt oppdatering slik den er designet, fordi at den aldri bruker `UPDATE SET saldo = ny_verdi`. Saldoen beregnes alltid som SUM("Posteringer".belop_teller). I Oppgave 7 skulle dere lage en løsning, som garanterte at oppdateringer av `Transaksjoner` og `Posteringer` blir alltid oppdatert eller angret innenfor en transaksjon. For å demonstrere ekte tapt oppdatering må man bruke en separat tabell med en oppdaterbar saldo-kolonne.

Bruk Python programmeringsspråk for å simulerer "samtidighet", dvs. at Ane og Bjørn kobler seg til databasen med to separate Psycopg2-koblinger (hvor Psycopg2 er en mye brukt adapter for Python programmeringsspråket; https://www.psycopg.org/), som da utføres i to separate *tråder* i Python (`threading` modulen).

Ta utgangspunkt i Python programmet i filen ´startkode/oppgave9.py´ hvor funksjoner for alle operasjoner mot databasen og samtidighet som simulerer den usikre *les-beregn-skriv*-syklusen er implementert. Dette skal gjennomgås på forelesninger.

Skriv python kode som genererer følgende output:

```bash
██████████████████████████████████████████████████████████████
  DEMONSTRASJON: TAPT OPPDATERING (LOST UPDATE)
  Oppgave 9 — Samtidige transaksjoner i PostgreSQL
██████████████████████████████████████████████████████████████
  Forsinkelse LES→SKRIV: 0.3s  |  Ane: +3,000 kr  |  Bjørn: +1,500 kr

══════════════════════════════════════════════════════════════
  SCENARIO A: INSERT-basert modell (GnuCash/NS 4102)
  Begge tråder leser samme saldo, men skriver uavhengige INSERT-rader.
  Resultat: INGEN tapt oppdatering — INSERT er immunt by design.
──────────────────────────────────────────────────────────────
  Saldo FØR: 248,500 kr  |  Forventet: 253,000 kr
──────────────────────────────────────────────────────────────
  [Bjørn ] LES: saldo = 248,500 kr
  [Ane   ] LES: saldo = 248,500 kr
  [Bjørn ] COMMIT: ny saldo = 253,000 kr  (+1,500 kr)
  [Ane   ] COMMIT: ny saldo = 253,000 kr  (+3,000 kr)
──────────────────────────────────────────────────────────────
  Saldo ETTER: 253,000 kr  |  Forventet: 253,000 kr  |  ✓ Korrekt

══════════════════════════════════════════════════════════════
  SCENARIO B1: UPDATE-basert — USIKKER (anti-mønster)
  Begge tråder leser SAMME saldo, beregner ny verdi og
  overskriver hverandre med UPDATE. Den siste vinner.
──────────────────────────────────────────────────────────────
  Saldo FØR: 248,500 kr  |  Forventet: 253,000 kr
──────────────────────────────────────────────────────────────
  [Ane   ] LES: saldo = 248,500 kr
  [Bjørn ] LES: saldo = 248,500 kr
  [Ane   ] COMMIT: satte saldo = 251,500 kr  (lest 248,500 + 3,000)  |  faktisk nå: 250,000 kr
  [Bjørn ] COMMIT: satte saldo = 250,000 kr  (lest 248,500 + 1,500)  |  faktisk nå: 250,000 kr
──────────────────────────────────────────────────────────────
  Saldo ETTER: 250,000 kr  |  Forventet: 253,000 kr  |  ⚠️  TAPT OPPDATERING: 3,000 kr gikk tapt!

══════════════════════════════════════════════════════════════
  SCENARIO B2: UPDATE-basert — SIKKER (SELECT FOR UPDATE)
  Den første tråden låser raden. Den andre venter til låsen frigis.
  Ingen Barrier nødvendig — låsen er synkroniseringsmekanismen.
──────────────────────────────────────────────────────────────
  Saldo FØR: 248,500 kr  |  Forventet: 253,000 kr
──────────────────────────────────────────────────────────────
  [Bjørn ] LES+LÅS: saldo = 248,500 kr
  [Ane   ] LES+LÅS: saldo = 250,000 kr
  [Bjørn ] COMMIT+FRIGJØR LÅS: saldo = 250,000 kr  (lest 248,500 + 1,500)
  [Ane   ] COMMIT+FRIGJØR LÅS: saldo = 253,000 kr  (lest 250,000 + 3,000)
──────────────────────────────────────────────────────────────
  Saldo ETTER: 253,000 kr  |  Forventet: 253,000 kr  |  ✓ Korrekt

══════════════════════════════════════════════════════════════
  SAMMENDRAG
──────────────────────────────────────────────────────────────
  Scenario                                              Avvik
  ──────────────────────────────────────────────── ──────────
  A:  INSERT-basert (GnuCash/NS 4102) — ingen låsing     0 kr ✓
  B1: UPDATE-basert — USIKKER (les-beregn-skriv)    -3,000 kr
  B2: UPDATE-basert — SIKKER  (SELECT FOR UPDATE)      0 kr ✓
══════════════════════════════════════════════════════════════
``` 
---

**Programmeringsoppgaver: Integrasjon med Ekstern Tjeneste og NoSQL**

Det finnes flere relevante API-tjenester som man kan utforske.

| Tjeneste | Datatype | Bruksområde i GnuCash-modellen | Fordeler for prosjektet |
|---|---|---|---|
| **Norges Bank API** [^7] | Valutakurser, renter | Automatisk oppdatering av `prices`-tabellen for NOK mot andre valutaer. | Offisiell, pålitelig kilde. Enkel REST API med SDMX-JSON-format. Ingen API-nøkkel nødvendig. |
| **Alpha Vantage** [^8] | Aksjekurser, valuta, krypto | Oppdatering av `prices` for et bredt spekter av `commodities`. | Svært omfattende. Gratis API-nøkkel. |
| **ExchangeRate-API** [^9] | Valutakurser | Enklere alternativ for global valutakursdata. | Krever ingen API-nøkkel for grunnleggende bruk. |
| **CoinGecko** [^10] | Kryptovaluta-kurser | Spesialisert for kryptovalutaer som `commodities`. | Bransjestandard for kryptodata. |
| **OpenFIGI** [^11] | Finansielle identifikatorer | Mapping mellom ticker-symboler og globale FIGI-identifikatorer. | Introduserer en reell problemstilling innen finansiell datahåndtering. |

**Norges Bank API** og **Alpha Vantage** skal brukes i oppgaveløsningene.
