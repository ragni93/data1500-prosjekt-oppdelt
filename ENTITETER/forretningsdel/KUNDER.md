#### Kunder

Register over virksomhetens kunder.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `bok_guid` | CHAR(32) | Fremmednøkkel til `Bøker`. |
| `kundenummer` | TEXT | Internt, unikt kundenummer. |
| `navn` | TEXT | Kundens navn. |
| `organisasjonsnr` | TEXT | Kundens organisasjonsnummer (hvis bedrift). |
| `adresse` | TEXT | Fakturaadresse. |
| `epost` | TEXT | E-postadresse for fakturering. |
| `valuta_guid` | CHAR(32) | Fremmednøkkel til `Valutaer`: foretrukket fakturavaluta. |
| `betalingsbetingelse_guid` | CHAR(32) | Fremmednøkkel til `Betalingsbetingelser`. |
| `mva_kode_guid` | CHAR(32) | Fremmednøkkel til `MVA-koder`: standard MVA-behandling for kunden. |
| `aktiv` | INTEGER | Boolsk flagg (1/0). |

---