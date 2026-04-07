**Primærnøkler og UUID**

UUID - Universaly Unique IDentifier (GUID - Globally Unique IDentifier)

Alle primærnøklene skal være UUID (Universally Unique identifier (eller GUID som man ofte finner i applikasjoner for Microsoft), https://en.wikipedia.org/wiki/Universally_unique_identifier), og dermed *surrogatnøkler*.

Datatype som skal brukes for UUID er `CHAR(32)` i stedet for en auto-inkrementerende `SERIAL`. Dette er et bevisst valg arvet fra GnuCash-modellen med tre fordeler: