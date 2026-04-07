#### Betalingsbetingelser

Definerer betalingsbetingelser som kan gjenbrukes på tvers av kunder, leverandører og fakturaer.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `navn` | TEXT | Navn, f.eks. `30 dager netto`. |
| `type` | TEXT | `DAGER` (antall dager) eller `PROXIMO` (fast dag i måneden). |
| `forfallsdager` | INTEGER | Antall dager til forfall. |
| `kontantrabatt_dager` | INTEGER | Antall dager for å oppnå kontantrabatt. |
| `rabatt_teller` | BIGINT | Kontantrabatt i prosent (teller). |
| `rabatt_nevner` | BIGINT | Nevner for rabatt. |
