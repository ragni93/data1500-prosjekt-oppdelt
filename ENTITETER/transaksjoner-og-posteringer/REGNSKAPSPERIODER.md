#### Regnskapsperioder

Definerer regnskapsperioder (typisk måneder) og deres status.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `bok_guid` | CHAR(32) | Fremmednøkkel til `Bøker`. Ikke NULL.|
| `navn` | TEXT | Navn på perioden, f.eks. `Januar 2026`. Ikke NULL.|
| `fra_dato` | DATE | Periodens startdato. Ikke NULL.|
| `til_dato` | DATE | Periodens sluttdato. Ikke NULL.|
| `status` | TEXT | `AAPEN`, `LUKKET`, `LAAST`. Styrer hvor posteringer kan gjøres. Ikke NULL. Standardverdiene skal være 'AAPEN'. Sjekk at verdien er en av `AAPEN`, `LUKKET`, `LAAST`.|

**Essens:** `Regnskapsperioder` er avgjørende for periodisering og korrekt rapportering (f.eks. MVA-terminer).

---
