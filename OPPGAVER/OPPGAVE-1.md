
### Oppgave 1: Implementasjon av datamodellen, mermaid-diagrammet og normalform

<span style="color:blue">**Hvordan besvare: Kode leveres i besvarelse/oppgave1-9/test-scripts. Mermaid koden skrives i besvarelse/RAPPORT.md**</span>


**Del A** Skriv et SQL-skript (`oppgave1.sql` legges i mappen `besvarelse/test-scripts`) som oppretter følgende tabeller med korrekte datatyper, primærnøkler og fremmednøkler i PostgreSQL:

- `Bøker`, `Valutaer`, `Valutakurser`
- `Kontoklasser`, `Kontoer`
- `Regnskapsperioder`
- `Transaksjoner`, `Posteringer`
- `MVA_koder`, `MVA_linjer`

**Del B** Lag et diagram i mermaid.live. Skriv mermaid-koden i filen RAPPORT.md.

**Del C** Forklar i rapporten hvofror denne modellen er på 3NF (NF - normalform).

#### Krav til skjemaet

- `Kontoer.overordnet_guid` skal være en selvhenvisende fremmednøkkel med `ON DELETE RESTRICT`.
- `Kontoer.kontoklasse` skal være en fremmednøkkel til `Kontoklasser.klasse_nr`.
- `Posteringer.transaksjon_guid` skal ha `ON DELETE CASCADE`.
- `Posteringer.konto_guid` skal ha `ON DELETE RESTRICT`.
- `CHECK`-betingelse på `Regnskapsperioder.status`: kun `'AAPEN'`, `'LUKKET'` eller `'LAAST'`.
- `CHECK`-betingelse på `Posteringer.avstemmingsstatus`: kun `'n'`, `'c'` eller `'y'`.
- `CHECK`-betingelse på `Kontoer.kontonummer`: mellom 1000 og 8999.
- Alle `belop_nevner`- og `antall_nevner`-felt skal ha `CHECK (... > 0)` for å forhindre divisjon med null.

**Ytelsesindekser**

- Indekser opprettes på kolonner som hyppig brukes i WHERE-betingelser og JOIN-operasjoner. PostgreSQL bruker B-tre-indekser som standard. Primærnøkler (guid) indekseres automatisk.
- Følgende 9 ytelsesindekser skal defineres (bruk de navn som er foreslått):

```sql 
   Tabell      |            Indeksnavn             
---------------+----------------------------------
 Kontoer       | idx_kontoer_bok_guid
 Kontoer       | idx_kontoer_kontonummer
 Kontoer       | idx_kontoer_overordnet_guid
 MVA-linjer    | idx_mva_linjer_transaksjon_guid
 Posteringer   | idx_posteringer_konto_guid
 Posteringer   | idx_posteringer_transaksjon_guid
 Transaksjoner | idx_transaksjoner_bok_guid
 Transaksjoner | idx_transaksjoner_periode_guid
 Transaksjoner | idx_transaksjoner_posteringsdato
``` 

**Kommentarer i databaseskjema**

- Bruk `COMMENT ON TABLE ... IS` for å legge inn kommentarer på en tabell (entitet) og `COMMENT ON COLUMN ... IS` for å legge inn kommentarer på en kolonne (attributt).
- Se eksempel for opprettelse av tabellen `Kontoer` (se kravspesifikasjon for alle tabellene i modellen i kapittel 5):

```sql 
CREATE TABLE "Kontoer" (
    guid            CHAR(32)    PRIMARY KEY,
    bok_guid        CHAR(32)    NOT NULL
                                REFERENCES "Bøker"(guid) ON DELETE RESTRICT,
    overordnet_guid CHAR(32)    REFERENCES "Kontoer"(guid) ON DELETE RESTRICT,
    valuta_guid     CHAR(32)    NOT NULL
                                REFERENCES "Valutaer"(guid) ON DELETE RESTRICT,
    kontonummer     INTEGER     UNIQUE
                                CHECK (kontonummer BETWEEN 1000 AND 9999),
    kontoklasse     INTEGER     NOT NULL
                                REFERENCES "Kontoklasser"(klasse_nr) ON DELETE RESTRICT,
    gnucash_type    TEXT,
    navn            TEXT        NOT NULL,
    beskrivelse     TEXT,
    er_placeholder  BOOLEAN     NOT NULL DEFAULT FALSE,
    er_skjult       BOOLEAN     NOT NULL DEFAULT FALSE,
    mva_pliktig     BOOLEAN     NOT NULL DEFAULT FALSE,
    mva_kode_guid   CHAR(32)    -- FK legges til etter opprettelse av MVA-koder
);

COMMENT ON TABLE  "Kontoer"                 IS 'Hierarkisk kontoplan. Kombinasjon av kontonummer og kontoklasse sikrer NS 4102-samsvar.';
COMMENT ON COLUMN "Kontoer".overordnet_guid IS 'Selvhenvisende FK. NULL = rotkonto. Bygger trestrukturen.';
COMMENT ON COLUMN "Kontoer".kontonummer     IS '4-sifret NS 4102-kontonummer (1000-9999). NULL for placeholder-kontoer.';
COMMENT ON COLUMN "Kontoer".er_placeholder  IS 'TRUE: kontoen er kun en beholder for underkontoer, kan ikke posteres på.';
COMMENT ON COLUMN "Kontoer".mva_pliktig     IS 'TRUE: transaksjoner på denne kontoen er normalt MVA-pliktige.';
``` 

Hvordan ser det ut i en kjørende instans av PostgreSQL? Legg merke til bruken av `\d+` kommandoen i psql-shell for å kunne vise kommentarer (`Description`) lagt til med `COMMENT ON COLUMN`. `ON DELETE RESTRICT` forhindrer utilsiktet sletting av data i andre tabeller som er i aktiv bruk.

```sql 
regnskap=# \d+ "Kontoer"
                                                                               Table "public.Kontoer"
     Column      |     Type      | Collation | Nullable | Default | Storage  | Compression | Stats target |                               Description                                
-----------------+---------------+-----------+----------+---------+----------+-------------+--------------+--------------------------------------------------------------------------
 guid            | character(32) |           | not null |         | extended |             |              | 
 bok_guid        | character(32) |           | not null |         | extended |             |              | 
 overordnet_guid | character(32) |           |          |         | extended |             |              | Selvhenvisende FK. NULL = rotkonto. Bygger trestrukturen.
 valuta_guid     | character(32) |           | not null |         | extended |             |              | 
 kontonummer     | integer       |           |          |         | plain    |             |              | 4-sifret NS 4102-kontonummer (1000-9999). NULL for placeholder-kontoer.
 kontoklasse     | integer       |           | not null |         | plain    |             |              | 
 gnucash_type    | text          |           |          |         | extended |             |              | 
 navn            | text          |           | not null |         | extended |             |              | 
 beskrivelse     | text          |           |          |         | extended |             |              | 
 er_placeholder  | boolean       |           | not null | false   | plain    |             |              | TRUE: kontoen er kun en beholder for underkontoer, kan ikke posteres på.
 er_skjult       | boolean       |           | not null | false   | plain    |             |              | 
 mva_pliktig     | boolean       |           | not null | false   | plain    |             |              | TRUE: transaksjoner på denne kontoen er normalt MVA-pliktige.
 mva_kode_guid   | character(32) |           |          |         | extended |             |              | 
Indexes:
    "Kontoer_pkey" PRIMARY KEY, btree (guid)
    "Kontoer_kontonummer_key" UNIQUE CONSTRAINT, btree (kontonummer)
    "idx_kontoer_bok_guid" btree (bok_guid)
    "idx_kontoer_kontonummer" btree (kontonummer)
    "idx_kontoer_overordnet_guid" btree (overordnet_guid)
Check constraints:
    "Kontoer_kontonummer_check" CHECK (kontonummer >= 1000 AND kontonummer <= 9999)
Foreign-key constraints:
    "Kontoer_bok_guid_fkey" FOREIGN KEY (bok_guid) REFERENCES "Bøker"(guid) ON DELETE RESTRICT
    "Kontoer_kontoklasse_fkey" FOREIGN KEY (kontoklasse) REFERENCES "Kontoklasser"(klasse_nr) ON DELETE RESTRICT
    "Kontoer_overordnet_guid_fkey" FOREIGN KEY (overordnet_guid) REFERENCES "Kontoer"(guid) ON DELETE RESTRICT
    "Kontoer_valuta_guid_fkey" FOREIGN KEY (valuta_guid) REFERENCES "Valutaer"(guid) ON DELETE RESTRICT
    "fk_mva_kode" FOREIGN KEY (mva_kode_guid) REFERENCES "MVA-koder"(guid) ON DELETE RESTRICT
Referenced by:
    TABLE ""Kontoer"" CONSTRAINT "Kontoer_overordnet_guid_fkey" FOREIGN KEY (overordnet_guid) REFERENCES "Kontoer"(guid) ON DELETE RESTRICT
    TABLE ""MVA-koder"" CONSTRAINT "MVA-koder_mva_konto_guid_fkey" FOREIGN KEY (mva_konto_guid) REFERENCES "Kontoer"(guid) ON DELETE RESTRICT
    TABLE ""Posteringer"" CONSTRAINT "Posteringer_konto_guid_fkey" FOREIGN KEY (konto_guid) REFERENCES "Kontoer"(guid) ON DELETE RESTRICT
    TABLE ""Bøker"" CONSTRAINT "fk_rot_konto" FOREIGN KEY (rot_konto_guid) REFERENCES "Kontoer"(guid) ON DELETE RESTRICT
Access method: heap
``` 
---