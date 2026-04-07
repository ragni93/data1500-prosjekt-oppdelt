#### MVA-linjer

Lagrer beregnet MVA-grunnlag og -beløp for hver transaksjon.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `transaksjon_guid` | CHAR(32) | Fremmednøkkel til `Transaksjoner`. Ikke NULL.|
| `mva_kode_guid` | CHAR(32) | Fremmednøkkel til `MVA-koder`. Ikke NULL.|
| `grunnlag_teller` | BIGINT | MVA-grunnlaget (beløpet MVA beregnes av). Ikke NULL.|
| `grunnlag_nevner` | BIGINT | Nevner for grunnlaget. Ikke NULL. Standardverdien er 100. Sjekk at verdien er større enn 0.|
| `mva_belop_teller` | BIGINT | Det beregnede MVA-beløpet. Ikke NULL.|
| `mva_belop_nevner` | BIGINT | Nevner for MVA-beløpet. Ikke NULL. Standardverdien er 100. Sjekk at verdien er større enn 0.|

**Essens:** `MVA-linjer` gir et detaljert revisjonsspor for all MVA-beregning og er grunnlaget for MVA-oppgaven.

---