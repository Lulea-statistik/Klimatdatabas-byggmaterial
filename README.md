# Klimatdatabas – byggmaterial

Interaktiv svensk klimatkalkyl för nyproduktion, med Boverkets klimatdatabas som datakälla.

## Starta
Öppna [webbappen](https://lulea-statistik.github.io/Klimatdatabas-byggmaterial/) när GitHub Pages är aktiverat (Source: Deploy from a branch, branch `main`, folder `/docs`).

1. Ladda ner [Boverkets klimatdatabas 02.07.000](https://www.boverket.se/contentassets/4668ed4cc3da447385788ed30bff7d49/boverkets-klimatdatabas-version-02.07.000-sv-se.xlsx).
2. Välj Excel-filen i webbappen. Välj relevant blad och rubrikrad.
3. Koppla **materialnamn**, **A1–A3 klimatfaktor** och **faktorns enhet** (i kolumn eller som manuellt fast värde).
4. Välj material och fyll i mängden i *faktorns nämnarenhet*, t.ex. kg om enheten är kg CO₂e/kg.
5. Jämför materialens bidrag, totalsumma, kg CO₂e per m² BTA; exportera som CSV.

**Viktigt:** Typiska och konservativa faktorer har olika syften. Använd samma metod för hela jämförelsen. Applikationen räknar endast A1–A3 i detta första steg, alltså inte A4 eller A5. Det går inte att jämföra material som konstruktivt likvärdiga enbart per kg. CSV-scenarier sparas lokalt av användaren; data skickas inte till en server.

**Datastatus:** GitHub-repot innehåller inga omräknade produktvärden eftersom Boverkets Excelarbetsbok inte har importerats server-side. Webbappen läser den faktiska Excel-filen i webbläsaren (SheetJS via CDN), så inga materialvärden är påhittade. Vid senare vidareutveckling kan versionshanterade data-JSON genereras från Boverkets API.

**Källa:** Boverket, [Öppna data – Klimatdatabas](https://www.boverket.se/sv/om-boverket/oppna-data/boverkets-klimatdatabas/), version 02.07.000, publicerad 21 januari 2026.


## Automatisk datainläsning (2026-10-22)
- Boverket tillhandahåller även ett öppet [Klimatdatabas-API](https://api-portal.boverket.se/reference#api=klimatdatabas) i JSON och XML.
- Webbappen läser nu det versionsmärkta `docs/data/materials.json` när denna fil finns.
- GitHub Actions `update-materials.yml` hämtar i första steget Boverkets officiella XLSX-distribution **en gång per dag**, kontrollerar kolumnschema och publicerar A1–A3 typiskt/konservativt, A4, A5, enhet, kategori och resurs-ID.
- En **direkt API-klient är ännu inte inkopplad**. URL:en för exakt resurs-anrop, autentisering och svarsschema måste verifieras mot OpenAPI innan vi ersätter importkällan. Undvik gissade API-anrop.
- Innan första lyckade GitHub Actions-körningen finns ännu inte `docs/data/materials.json`; webbappen erbjuder då manuell Excel-import som reserv.
