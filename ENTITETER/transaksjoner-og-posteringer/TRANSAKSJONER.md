#### Transaksjoner

Representerer én finansiell hendelse (et bilag). Den er en overskrift som samler alle tilhørende posteringer.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator. Primærnøkkel. |
| `bok_guid` | CHAR(32) | Fremmednøkkel til `Bøker`. Ikke NULL. |
| `valuta_guid` | CHAR(32) | Fremmednøkkel til `Valutaer`: transaksjonens hovedvaluta. Ikke NULL.|
| `bilagsnummer` | TEXT | Bilagsnummer fra eksternt dokument (faktura, kvittering). Ikke NULL.|
| `bilagsdato` | DATE | Datoen på det eksterne dokumentet. |
| `posteringsdato` | TIMESTAMP | Datoen transaksjonen skal regnskapsføres på. Ikke NULL. Standardverdien skal være tidspunktet for innsetting/oppdatering av raden.|
| `registreringsdato` | TIMESTAMP | Tidspunktet transaksjonen ble registrert i systemet. Ikke NULL. Standardverdien skal være tidspunktet for innsetting/oppdatering av raden.|
| `beskrivelse` | TEXT | Fritekstbeskrivelse av hendelsen. Ikke NULL. |
| `kilde` | TEXT | Hvordan transaksjonen ble opprettet: `manuell`, `import`, `planlagt`. Standardverdien skal være 'manuell'. Sjekk at verdien er en av 'manuell', 'import', 'planlagt'.|
| `periode_guid` | CHAR(32) | Fremmednøkkel til `Regnskapsperioder`. Ikke NULL.|

**Essens:** `Transaksjoner` er hendelsesloggen. Skillet mellom `bilagsdato` og `posteringsdato` er et sentralt krav i bokføringsloven.

---