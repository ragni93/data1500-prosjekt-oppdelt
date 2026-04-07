#### Kontoklasser

En oppslagstabell som definerer de åtte hovedklassene i Norsk Standard Kontoplan (NS 4102).

| Attributt | Type | Forklaring |
|---|---|---|
| `klasse_nr` | INTEGER | 1–8. Primærnøkkel. |
| `navn` | TEXT | Navnet på klassen, f.eks. `Eiendeler`, `Varekostnad`. Ikke NULL og unik.|
| `type` | TEXT | `BALANSE` eller `RESULTAT`. Ikke NULL. Sjekk at verdien er enten 'BALANSE' eller 'RESULTAT'. |
| `normal_saldo` | TEXT | `DEBET` eller `KREDIT`. Angir hvilken side som øker saldoen. Ikke NULL. Sjekk at verdien er enten 'DEBET' eller 'KREDIT'.|
| `beskrivelse` | TEXT | Kort forklaring av klassens formål. |

**Essens:** `Kontoklasser` formaliserer strukturen i NS 4102 og gjør det mulig å validere kontoplanen og forenkle rapportering.

---