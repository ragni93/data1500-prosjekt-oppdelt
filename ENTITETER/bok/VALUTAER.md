#### Valutaer

Inneholder alle typer omsettelige valutaer som brukes i regnskapet, primært basert på ISO 4217-standarden.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `kode` | TEXT | ISO 4217-koden, f.eks. `NOK`, `USD`, `EUR`. Ikke NULL og unik.|
| `navn` | TEXT | Fullt navn, f.eks. `Norske kroner`. Ikke NULL.|
| `desimaler` | INTEGER | Antall desimaler valutaen opererer med (f.eks. 2 for NOK). Ikke NULL. Standardverdi 100. Sjekk at verdien er over 0.|
| `hent_kurs_flag` | INTEGER | Boolsk flagg (1/0): om systemet skal forsøke å hente kurser automatisk. Ikke NULL. Standardverdi 0. Sjekk at verdien er enten 0 eller 1. |
| `kurs_kilde` | TEXT | Standard kilde for automatisk kurshenting, f.eks. `norges-bank`, `ecb`.|

**Essens:** `Valutaer` definerer de monetære enhetene som brukes. Alle kontoer og transaksjoner er knyttet til en valuta.

---