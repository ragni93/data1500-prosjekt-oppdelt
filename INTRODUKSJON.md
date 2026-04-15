
## 3. Bakgrunn: Dobbelt Bokholderi

### 3.1 Prinsippet

Dobbelt bokholderi bygger på en enkel, men kraftfull idé: enhver transaksjon har to sider. Når en bedrift selger en tjeneste, oppstår det både en inntekt og en fordring. Når den betaler lønn, reduseres bankbeholdningen og en lønnskostnad oppstår. Regnskapsligningen som alltid må holde, er:

> **Eiendeler = Gjeld + Egenkapital**

Utvidet til å inkludere resultatkontoer:

> **Eiendeler − Gjeld = Egenkapital + (Inntekter − Kostnader)**

### 3.2 De fem grunnleggende kontotypene

Alle kontoer i et dobbelt bokholderisystem tilhører én av fem grunnleggende typer. Tabellen nedenfor oppsummerer disse:

| Kontotype | Norsk betegnelse | Beskrivelse | Saldo øker ved | Saldo reduseres ved |
|---|---|---|---|---|
| **Assets** | Eiendeler | Det bedriften eier (penger, utstyr, kundefordringer) | Debet | Kredit |
| **Liabilities** | Gjeld | Det bedriften skylder (lån, leverandørgjeld) | Kredit | Debet |
| **Equity** | Egenkapital | Eiernes andel av bedriften | Kredit | Debet |
| **Income** | Inntekter | Inntekter fra salg av varer og tjenester | Kredit | Debet |
| **Expenses** | Kostnader | Utgifter til drift av virksomheten | Debet | Kredit |

Det er også vanlig å bruke flere typer kontoer, som man kan klassifisere som deltyper av de fem grunnleggende typene, f.eks.  `CASH`, `BANK`, `STOCK`, `CREDIT`, `ACCOUNTS_RECEIVABLE`, `ACCOUNTS_PAYABLE`, `EQUITY`, osv.

### 3.3 T-konto og splits

Den tradisjonelle måten å visualisere dobbelt bokholderi på er gjennom **T-kontoer**, der venstre side er debet og høyre side er kredit. I GnuCash sin datamodell er dette implementert gjennom tabellen `splits`: hver rad i `splits` representerer én side av en transaksjon og er knyttet til én bestemt konto. En transaksjon med to splits tilsvarer en klassisk to-sidig postering; en transaksjon med tre eller flere splits er en sammensatt postering. Det norske navnet for `splits` er `posteringer`.

---



**Sentrale begrep**

En `balansekonto` er en konto som viser bedriftens finansielle stilling på et gitt tidspunkt. Saldoen på disse kontoene overføres fra ett regnskapsår til det neste - de "nullstilles" ikke ved årsslutt. Balansekontoer deles inn i to hovedgrupper, - `Eiendeler` (aktiva, dvs. bankinnskudd, varelager, maskiner, kundefordringer) og `Gjeld og egenkapital` (passiva). Kontoklasser 1-2 i NS 4102.

En `resultatkonto` registrerer bedriftens inntekter og kostnader i løpet av en periode (vanligvis et regnskapsår). Ved periodeslutt nullstilles disse kontoene, og nettoresultatet (overskudd eller underskudd) overføres til egenkapitalen i balansen. Resultatkontoer deles inn i `Inntekter` (salgsinntekter, renteinntekter, gevinster) og `Kostnader` (lønn, husleie, varekostnad, avskrivninger). Kontoklasser 3 (salgsinntekter), 4 (varekostnad), 5-7 (driftskostnader) og 8 (finans og skatt).

---
