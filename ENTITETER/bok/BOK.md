#### Bøker

En bok er den øverste beholderen for alle andre entiteter i systemet. Den representerer ett fullstendig regnskapssystem for én virksomhet.

| Attributt | Type | Forklaring |
|---|---|---|
| `guid` | CHAR(32) | Unik identifikator (UUID) for boken. Primærnøkkel. |
| `navn` | TEXT | Navnet på regnskapsboken, f.eks. "DATA1500 Konsult AS Regnskap". Ikke NULL. |
| `organisasjonsnr` | TEXT | Virksomhetens 9-sifrede organisasjonsnummer. |
| `adresse` | TEXT | Virksomhetens forretningsadresse. |
| `rot_konto_guid` | CHAR(32) | Peker til rotkontoen i kontohierarkiet. Fremmednøkkel til `Kontoer`. Legges inn etter opprettelse av Kontoer|
| `regnskapsaar` | DATE | Startdatoen for gjeldende regnskapsår. |

**Essens:** `Bøker` er ankerpunktet for hele regnskapet. Uten en bok finnes det ingen kontoer, transaksjoner eller andre data. Tabellen inneholder metadata om virksomheten som fører regnskapet.

---