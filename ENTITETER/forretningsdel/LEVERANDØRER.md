#### Leverandører

Register over virksomhetens leverandører.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `bok_guid` | CHAR(32) | Fremmednøkkel til `Bøker`. |
| `leverandornummer` | TEXT | Internt, unikt leverandørnummer. |
| `navn` | TEXT | Leverandørens navn. |
| `organisasjonsnr` | TEXT | Leverandørens organisasjonsnummer. |
| `adresse` | TEXT | Postadresse. |
| `epost` | TEXT | E-postadresse for kontakt. |
| `valuta_guid` | CHAR(32) | Fremmednøkkel til `Valutaer`: foretrukket betalingsvaluta. |
| `betalingsbetingelse_guid` | CHAR(32) | Fremmednøkkel til `Betalingsbetingelser`. |
| `aktiv` | INTEGER | Boolsk flagg (1/0). |

---