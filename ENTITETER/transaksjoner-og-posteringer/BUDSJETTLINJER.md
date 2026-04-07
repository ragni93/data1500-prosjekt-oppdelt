#### Budsjettlinjer

Inneholder de faktiske budsjetterte beløpene per konto per periode.

| Attributt | Type | Forklaring |
|---|---|---|
| `id` | INTEGER | Auto-inkrementerende primærnøkkel. |
| `budsjett_guid` | CHAR(32) | Fremmednøkkel til `Budsjetter`. |
| `konto_guid` | CHAR(32) | Fremmednøkkel til `Kontoer`. |
| `periode_nr` | INTEGER | Periodenummer (f.eks. 1 for januar, 12 for desember). |
| `belop_teller` | BIGINT | Budsjettert beløp (teller). |
| `belop_nevner` | BIGINT | Nevner for beløpet. |

**Essens:** `Budsjetter` og `Budsjettlinjer` muliggjør avviksanalyse mellom planlagte og faktiske resultater.

---