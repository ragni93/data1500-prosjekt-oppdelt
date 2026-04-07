### Oppgave 8: Feilhåndtering og Gjenoppbygging (K10.3, K10.4, K10.5, K10.6)

<span style="color:blue">**Hvordan besvare:  Teoretisk del, besvares i RAPPORT.md. Kode leveres i besvarelse/oppgave1-9/test-scripts.**</span>

**Mål:** Forstå hvordan et DBHS bruker transaksjonsloggen til å gjenoppbygge en konsistent tilstand etter feil.

**Læringsutbytte:** Studenten kan forklare forskjellen mellom UNDO og REDO, og forstår rollen til sjekkpunkter og sikkerhetskopiering.

#### 8a: Teoretisk Oppgave

Gitt følgende forenklete transaksjonslogg for et regnskapssystem:

| Log-sekvensnr. | Hendelse | Transaksjon | Detalj |
|---|---|---|---|
| 1 | `BEGIN` | T1 | — |
| 2 | `WRITE` | T1 | Sett inn i `Transaksjoner` (guid = 'A1') |
| 3 | `BEGIN` | T2 | — |
| 4 | `WRITE` | T2 | Sett inn i `Transaksjoner` (guid = 'B1') |
| 5 | `CHECKPOINT` | — | — |
| 6 | `WRITE` | T1 | Sett inn i `Posteringer` (tx_guid = 'A1', konto = 'Kasse') |
| 7 | `COMMIT` | T1 | — |
| 8 | `WRITE` | T2 | Sett inn i `Posteringer` (tx_guid = 'B1', konto = 'Bank') |
| 9 | `CRASH` | — | Systemfeil |

Besvar følgende spørsmål i rapporten:
1.  Hvilke transaksjoner skal **REDO**-es etter krasjet, og hvilke skal **UNDO**-es? Begrunn svaret.
2.  Forklar rollen til **sjekkpunktet** (linje 5) i gjenoppbyggingsprosessen. Hva ville ha vært annerledes uten det?
3.  Forklar forskjellen mellom en **instansfeil** (f.eks. strømbrudd) og en **mediefeil** (f.eks. diskkrasj), og hvilken rolle **sikkerhetskopiering** spiller for sistnevnte.

#### 8b: Praktisk Oppgave

Bruk databasesystemets innebygde funksjoner for å demonstrere at en `ROLLBACK` faktisk angrer alle endringer:

```sql
BEGIN;

INSERT INTO "Transaksjoner" (guid, ...) VALUES (...);
INSERT INTO "Posteringer" (guid, transaksjon_guid, ...) VALUES (...);

-- Verifiser at dataene er synlige innenfor transaksjonen
SELECT * FROM "Transaksjoner" WHERE guid = ...; -- Skal returnere 1 rad

ROLLBACK;

-- Verifiser at dataene er borte etter ROLLBACK
SELECT * FROM "Transaksjoner" WHERE guid = ...; -- Skal returnere 0 rader
```

---
