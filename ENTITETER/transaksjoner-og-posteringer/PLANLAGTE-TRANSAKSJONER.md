#### Planlagte Transaksjoner

Maler for gjentakende transaksjoner som husleie, lønn eller faste avdrag.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `bok_guid` | CHAR(32) | Fremmednøkkel til `Bøker`. |
| `navn` | TEXT | Navn på den planlagte transaksjonen. |
| `aktiv` | INTEGER | Boolsk flagg (1/0). |
| `startdato` | DATE | Dato for første forekomst. |
| `sluttdato` | DATE | Dato for siste forekomst (NULL for evigvarende). |
| `gjentakelse_type` | TEXT | `MAANED`, `UKE`, `DAG`, `AAR`. |
| `gjentakelse_mult` | INTEGER | Multiplikator (f.eks. 2 for annenhver måned). |
| `auto_opprett` | INTEGER | Boolsk flagg (1/0): om transaksjonen skal opprettes automatisk. |
| `sist_opprettet` | DATE | Dato for siste gang transaksjonen ble generert. |

**Essens:** `Planlagte Transaksjoner` automatiserer rutinemessig bokføring og reduserer manuelle feil.

---