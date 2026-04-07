#### Kontoer

Implementerer den fullstendige, hierarkiske kontoplanen for virksomheten, tilpasset NS 4102.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `bok_guid` | CHAR(32) | Fremmednøkkel til `Bøker`. Ikke NULL.|
| `overordnet_guid` | CHAR(32) | Fremmednøkkel til `Kontoer` (selvhenvisning) for å bygge hierarki. |
| `valuta_guid` | CHAR(32) | Fremmednøkkel til `Valutaer`: valutaen kontoen er denominert i. Ikke NULL. |
| `kontonummer` | INTEGER | Det 4-sifrede kontonummeret fra NS 4102, f.eks. 1920. Unik. Sjekk at verdien er mellom 1000 og 9999.|
| `kontoklasse` | INTEGER | Fremmednøkkel til `Kontoklasser` (1–8). Ikke NULL.|
| `gnucash_type` | TEXT | Den underliggende GnuCash-typen, f.eks. `BANK`, `EXPENSE`. |
| `navn` | TEXT | Kontonavnet, f.eks. `Bankinnskudd`. Ikke NULL.|
| `beskrivelse` | TEXT | Valgfri, utdypende beskrivelse. |
| `er_placeholder` | INTEGER | Boolsk flagg (1/0): om kontoen kun er en beholder for underkontoer. Ikke NULL. Standardverdien er `FALSE`.|
| `er_skjult` | INTEGER | Boolsk flagg (1/0): om kontoen skal skjules i brukergrensesnittet. Ikke NULL. Standardverdien er `FALSE`.|
| `mva_pliktig` | INTEGER | Boolsk flagg (1/0): om transaksjoner på denne kontoen normalt er MVA-pliktige. Ikke NULL. Standardverdien er `FALSE`.|
| `mva_kode_guid` | CHAR(32) | Fremmednøkkel til `MVA-koder`: standard MVA-kode for denne kontoen. *FK legges til etter opprettelse av MVA-koder*. |

**Essens:** `Kontoer` er selve kontoplanen. Kombinasjonen av `kontonummer` og `kontoklasse` sikrer samsvar med norsk standard, mens `overordnet_guid` gir hierarkisk struktur for rapportering.

For å løse den sirkulære avhengigheten mellom `Bøker` og `Kontoer` bruk

```sql
ALTER TABLE "Bøker" 
ADD CONSTRAINT fk_rot_konto 
FOREIGN KEY (rot_konto_guid)
REFERENCES "Kontoer"(guid)
ON DELETE RESTRICT;
```

`ON DELETE RESTRICT` tolkes som at raden i `Kontoer` kan ikke slettes, hvis `Bøker.rot_konto_guid` peker på (har lik verdi i) `Kontoer.guid`. For å unngå at det pekes på fra bøker til kontoer på en konto som ikke lenger eksisterer (er slettet).

---