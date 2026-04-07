#### Lot

Brukes til å gruppere kjøps- og salgstransaksjoner for verdipapirer for å beregne realisert gevinst/tap.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `konto_guid` | CHAR(32) | Fremmednøkkel til `Kontoer` (en verdipapirkonto). |
| `beskrivelse` | TEXT | Valgfri beskrivelse av lottet. |
| `er_lukket` | INTEGER | Boolsk flagg (1/0): om alle enhetene i lottet er solgt. |

**Essens:** `Lot` er en mekanisme for å spore kostpris og gevinst for investeringer i henhold til skatteregler (FIFO-prinsippet).

---