**Eksempel: Forhold mellom entiteter**

Her er noen punkter som kan hjelpe å komme i gang med mermaid diagrammet for regnskapsmodellen.

- En bok (`Bøker`) **inneholder** ingen eller mange transaksjoner (`Transaksjoner`); omvendt, - en transaksjon hører til i nøyaktig en bok. En bok **inneholder** også ingen eller mange kontoer og **definerer** ingen eller mange regnskapsperioder.
- En bok **har** ingen eller mange kunder, fakturaer, leverandører, budsjetter, planlagte transaksjoner.
- En transaksjon har ingen eller mange mva-linjer.
- Resten kan kartlegges basert på beskrivelsene av enitetene i dette kapittelet (kapittel 5).
- Eksempel på mermaid kode for forholdene som `Bøker`, `Kontoklasser` og `Kontoer` er involvert i.
```bash
# Eksempel på mermaid kode
BØKER ||--o{ KONTOER                 : "inneholder"
BØKER ||--o{ TRANSAKSJONER           : "inneholder"
BØKER ||--o{ BUDSJETTER              : "har"
BØKER ||--o{ REGNSKAPSPERIODER       : "definerer"
BØKER ||--o{ PLANLAGTE_TRANSAKSJONER : "har"
BØKER ||--o{ KUNDER                  : "har"
BØKER ||--o{ LEVERANDORER            : "har"
BØKER ||--o{ FAKTURAER               : "inneholder"

KONTOKLASSER ||--o{ KONTO     : "klassifiserer"
KONTOER      ||--o{ KONTO     : "er overordnet" # for å implementere hierarki av kontoer
KONTOER      }o--|| VALUTAER  : "denominert i"
KONTOER      }o--o| MVA_KODER : "bruker"
POSTERINGER  }o--|| KONTOER   : "berører"
LOT          }o--|| KONTOER   : "tilhører"
```
