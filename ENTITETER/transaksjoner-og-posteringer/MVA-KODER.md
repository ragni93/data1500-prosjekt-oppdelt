#### MVA-koder

Definerer de ulike MVA-kodene og -satsene som brukes i Norge.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `kode` | TEXT | Standard MVA-kode, f.eks. `1`, `11`, `31`. Ikke NULL og unik.|
| `navn` | TEXT | Beskrivelse, f.eks. `Utgående MVA, høy sats (25%)`. Ikke NULL.|
| `type` | TEXT | `UTGAAENDE`, `INNGAAENDE`, `INGEN`. Ikke NULL. Sjekk for at verdien er en av 'UTGAAENDE', 'INNGAAENDE', 'INGEN'.|
| `sats_teller` | BIGINT | Sats i prosent (teller). Ikke NULL.|
| `sats_nevner` | BIGINT | Nevner (alltid 100 for prosent). Standardverdien skal være 100. Sjekk at verdien er større enn 0.|
| `mva_konto_guid` | CHAR(32) | Fremmednøkkel til `Kontoer`: kontoen MVA-beløpet skal posteres på. Ikke NULL.|
| `aktiv` | INTEGER | Boolsk flagg (1/0): om koden er i aktiv bruk. Ikke NULL. Standardverdien skal være 'TRUE'.|

**Essens:** `MVA-koder` sentraliserer MVA-logikken og kobler satser til spesifikke MVA-kontoer i kontoplanen.

For å definerer fremmednøkkelen i `Kontoer` mot `MVA-koder`:

```sql
ALTER TABLE "Kontoer"
ADD CONSTRAINT fk_mva_kode
FOREIGN KEY (mva_kode_guid)
REFERENCES "MVA-koder"(guid)
ON DELETE RESTRICT;
``` 

`ON DELETE RESTRICT` tolkes som at raden i `MVA-koder` kan ikke slettes, hvis `Kontoer.mva_kode_guid` peker på (har lik verdi i) `MVA-koder.guid`. For å unngå at det pekes på fra kontoer til MVA koder på rad i `MVA-linjer` som ikke lenger eksisterer (er slettet).

---
