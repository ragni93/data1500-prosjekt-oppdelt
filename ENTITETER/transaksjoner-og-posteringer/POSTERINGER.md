#### Posteringer

Hjertet i dobbelt bokholderi. Hver postering representerer én linje på én konto i en transaksjon.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `transaksjon_guid` | CHAR(32) | Fremmednøkkel til `Transaksjoner`. Ikke NULL.|
| `konto_guid` | CHAR(32) | Fremmednøkkel til `Kontoer`. Ikke NULL.|
| `tekst` | TEXT | Notat spesifikt for denne posteringslinjen. |
| `handling` | TEXT | Handlingstype, f.eks. `Kjøp`, `Salg`, `Lønn`. |
| `avstemmingsstatus` | TEXT | `n` (ikke avstemt), `c` (klarert), `y` (avstemt mot bank). Ikke NULL. Standardverdien skal være 'n'. Sjekk at verdien er en av 'n', 'c', 'y'.|
| `avstemmingsdato` | DATE | Dato for bankavstemming. |
| `belop_teller` | BIGINT | Beløp i transaksjonens valuta (teller). Positivt = debet, negativt = kredit. Ikke NULL.|
| `belop_nevner` | BIGINT | Nevner for beløpet. Ikke NULL. Standardverdi 100. Sjekk at verdien er større enn 0. |
| `antall_teller` | BIGINT | Antall enheter (for aksjer/varer). Ikke NULL. Standardverdien skal være 0.|
| `antall_nevner` | BIGINT | Nevner for antall. Ikke NULL. Standardverdien skal være 1. Sjekk at verdrien er større enn 0.|
| `lot_guid` | CHAR(32) | Fremmednøkkel til `Lot` (for verdipapirer). |

**Essens:** `Posteringer` implementerer debet/kredit-prinsippet. Summen av `belop_teller` for alle posteringer i en transaksjon må være null.

---