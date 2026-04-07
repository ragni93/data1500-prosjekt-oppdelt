#### Valutakurser

Lagrer historiske vekslingskurser mellom to valutaer på et gitt tidspunkt.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `fra_valuta_guid` | CHAR(32) | Fremmednøkkel til `Valutaer`: valutaen som prises. Ikke NULL.|
| `til_valuta_guid` | CHAR(32) | Fremmednøkkel til `Valutaer`: valutaen prisen er uttrykt i (basisvaluta, typisk NOK). Ikke NULL.|
| `dato` | TIMESTAMP | Tidspunktet kursen gjelder for. Ikke NULL. Standardverdien er tidspunktet når raden settes inn / oppdateres. |
| `kilde` | TEXT | Kilden til kursen, f.eks. `norges-bank`, `manuell`. |
| `type` | TEXT | Type kurs: `last` (siste), `bid` (kjøp), `ask` (salg), `nav` (for fond). Sjekk at verdiene er en av disse, - 'last', 'bid', 'ask', 'nav'.|
| `kurs_teller` | BIGINT | Teller for kursverdien (brøkrepresentasjon). Ikke NULL.|
| `kurs_nevner` | BIGINT | Nevner for kursverdien. Kurs = `kurs_teller / kurs_nevner`. Ikke NULL. Standardverdien er 100. Sjekke at verdien er større enn 0, for å unngå divisjon med 0.|

I tillegg legge inn en betingelse for at `fra_valuta_guid` er ikke lik `til_valuta_guid`.

**Essens:** `Valutakurser` muliggjør korrekt bokføring og rapportering av transaksjoner i utenlandsk valuta.

---