
#### Budsjetter

Toppnivå-entitet for budsjettering.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `bok_guid` | CHAR(32) | Fremmednøkkel til `Bøker`. |
| `navn` | TEXT | Navn på budsjettet, f.eks. `Årsbudsjett 2026`. |
| `beskrivelse` | TEXT | Valgfri, utdypende beskrivelse. |
| `antall_perioder` | INTEGER | Antall perioder i budsjettet (f.eks. 12 for et årsbudsjett). |

---