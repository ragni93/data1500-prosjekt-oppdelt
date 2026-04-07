
**Essens:** `Kunder`, `Leverandører`, `Fakturaer`, `Fakturalinjer` og `Betalingsbetingelser` utgjør et komplett fakturerings- og reskontrosystem som er tett integrert med kjerne-regnskapet. En faktura er ikke bare et dokument, men en hendelse som direkte påvirker regnskapskontoer som `Kundefordringer` og `Salgsinntekter`.



**Kjerne — bok og valuta**

| Entitet (flertall) | Rolle                                          |
| ------------------ | ---------------------------------------------- |
| `Bøker`            | Ankerpunkt for hele regnskapet.                |
| `Valutaer`         | ISO 4217-valutaer (NOK, USD, EUR).             |
| `Valutakurser`     | Historiske vekslingskurser mellom to valutaer. |

**NS 4102-kontostruktur**

| Entitet (flertall) | Rolle                                                                    |
| ------------------ | ------------------------------------------------------------------------ |
| `Kontoklasser`     | Oppslagstabell for de åtte NS 4102-klassene.                             |
| `Kontoer`          | Hierarkisk kontoplan med `kontonummer` (4-sifret) og `kontoklasse` (FK). |

**Transaksjoner og posteringer**

| Entitet (flertall)        | Rolle                                                          |
| ------------------------- | -------------------------------------------------------------- |
| `Regnskapsperioder`       | Åpne/lukkede perioder med status.                              |
| `Transaksjoner`           | Bilagshode med skille mellom `bilagsdato` og `posteringsdato`. |
| `Posteringer`             | Debet/kredit-linjer. Summen skal alltid være null.             |
| `Lot`                     | Kobler kjøp og salg av verdipapirer (FIFO).                    |
| `MVA-koder`               | Norske MVA-koder og -satser.                                   |
| `MVA-linjer`              | Beregnet grunnlag og MVA-beløp per transaksjon.                |
| `Budsjetter`              | Toppnivå for budsjettering.                                    |
| `Budsjettlinjer`          | Budsjettert beløp per konto per periode.                       |
| `Planlagte Transaksjoner` | Maler for gjentakende posteringer.                             |

**Forretningsdel**

| Entitet (flertall)     | Rolle                                                      |
| ---------------------- | ---------------------------------------------------------- |
| `Kunder`               | Kunderegisteret med org.nr. og MVA-kode.                   |
| `Leverandører`         | Leverandørregisteret.                                      |
| `Fakturaer`            | Salgs- og kjøpsfakturaer med status.                       |
| `Fakturalinjer`        | Varelinjer med separate FK til inntekts- og kostnadskonto. |
| `Betalingsbetingelser` | Gjenbrukbare betalingsvilkår.                              |

---
