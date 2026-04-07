
### Oppgave 5: Ytelsesanalyse med `EXPLAIN ANALYZE` og `MATERIALIZED VIEW`

<span style="color:blue">**Hvordan besvare: Kode leveres i besvarelse/oppgave1-9/test-scripts. OG/eller (?) RAPPORT.md**</span>

#### Del A — Opprett et komplekst `VIEW`

Lag et `VIEW` kalt `v_salgsrapport` som joiner `Fakturaer`, `Fakturalinjer`, `Kunder`, `Kontoer` (for inntektskonto) og `MVA_koder` for å vise en detaljert salgsrapport. Viewet skal inneholde: kundenavn, fakturanummer, fakturadato, varebeskrivelse, antall, enhetspris (eks. MVA), MVA-sats og totalbeløp inkl. MVA.

#### Del B — Analyser med `EXPLAIN ANALYZE`

Kjør følgende kommando og lim inn det fullstendige resultatet i prosjektrapporten:

```sql
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT) SELECT * FROM v_salgsrapport;
```

Analyser resultatet og svar på følgende spørsmål i rapporten:

- Hvilken join-algoritme valgte PostgreSQL (f.eks. `Hash Join`, `Nested Loop`)?
- Hva er den totale estimerte og faktiske kjøretiden?
- Identifiser den operasjonen som bruker mest tid (høyest `actual time`).

#### Del C — Implementer `MATERIALIZED VIEW`

Opprett et `MATERIALIZED VIEW` kalt `mv_salgsrapport` med nøyaktig samme spørring:

```sql
CREATE MATERIALIZED VIEW mv_salgsrapport AS
SELECT * FROM v_salgsrapport;
```

Kjør `EXPLAIN ANALYZE SELECT * FROM mv_salgsrapport;` og sammenlign kjøretiden med Del B.

#### Del D — Diskusjon

Forklar i rapporten:

- Hvorfor er det materialiserte viewet raskere for lesing?
- Hva er den fundamentale ulempen (stikkord: datakonsistens, `REFRESH MATERIALIZED VIEW`)?
- I hvilke situasjoner er et `MATERIALIZED VIEW` uegnet, og hva kan brukes i stedet?

---