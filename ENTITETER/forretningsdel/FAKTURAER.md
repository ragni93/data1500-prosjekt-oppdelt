#### Fakturaer

Representerer salgsfakturaer, inngående fakturaer og utgiftsbilag.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `bok_guid` | CHAR(32) | Fremmednøkkel til `Bøker`. |
| `fakturanummer` | TEXT | Eksternt fakturanummer (unikt per leverandør/salg). |
| `type` | TEXT | `SALG`, `KJOP`, `UTGIFT`. |
| `kunde_guid` | CHAR(32) | Fremmednøkkel til `Kunder` (for salgsfakturaer). |
| `leverandor_guid` | CHAR(32) | Fremmednøkkel til `Leverandører` (for kjøpsfakturaer). |
| `valuta_guid` | CHAR(32) | Fremmednøkkel til `Valutaer`. |
| `fakturadato` | DATE | Datoen på fakturaen. |
| `forfallsdato` | DATE | Betalingsfrist. |
| `posteringsdato` | TIMESTAMP | Dato fakturaen ble bokført. |
| `status` | TEXT | `UTKAST`, `SENDT`, `BETALT`, `KREDITERT`. |
| `betalingsbetingelse_guid` | CHAR(32) | Fremmednøkkel til `Betalingsbetingelser`. |
| `transaksjon_guid` | CHAR(32) | Fremmednøkkel til `Transaksjoner` (kobler til betalingstransaksjonen). |

---
