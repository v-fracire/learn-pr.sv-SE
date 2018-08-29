När du skapar en app som behöver lagra data, är det viktigt att tänka på hur appen ska organisera och lagra data i lagringskonton, behållare och blobbar.

## <a name="storage-accounts"></a>Lagringskonton

Ett enda lagringskonto är tillräckligt flexibelt för att du ska kunna organisera dina blobbar hur du vill, men du kan använda fler lagringskonton om du vill kunna separera kostnader och styra åtkomsten till data.

## <a name="containers-and-blobs"></a>Behållare och blobbar

Typen av program och data som lagras ska avgöra vilken strategi du väljer för att namnge och organisera behållare och blobbar.

Appar som använder blobbar som en del av ett lagringsschema med en databas är vanligtvis inte särskilt beroende av organisation, namngivning eller metadata för att kunna visa något om sina data. Sådana appar använder ofta identifierare som ex.vis GUID som blobbnamn och refererar till dessa identifierare i databasposter. Appen kommer att använda databasen för att fastställa var blobbarna lagras och vilken typ av data de innehåller.

Andra appar kan använda Azures blobblagring mer som ett personligt filsystem, där behållare och blobbnamn används för att visa innebörd och struktur. Blobbnamnen i dessa typer av appar ser ofta ut som traditionella filnamn med filnamnstillägg som t.ex. `.jpg` för att ange vilken typ av data de innehåller. De använder virtuella kataloger (se nedan) till att organisera blobbarna och använder ofta metadatataggar för att lagra information om blobbar och behållare.

Det finns några viktiga saker att tänka på när du bestämmer hur du ska organisera och lagra blobbar och behållare.

### <a name="naming-limitations"></a>Namnbegränsningar

Behållare och blobbnamn måste följa en uppsättning regler, inklusive längdbegränsningar och teckenbegränsningar. Se avsnittet Ytterligare resurser i slutet av den här modulen för mer detaljerad information om regler för namngivning.

### <a name="public-access-and-containers-as-security-boundaries"></a>Offentlig åtkomst och behållare som säkerhetsgränser

Som standard kräver alla blobbar autentisering för åtkomst. Enskilda behållare kan dock konfigureras för att tillåta offentlig hämtning av sina blobbar utan autentisering. Den här funktionen stöder olika typer av användning, till exempel värdbaserade statiska webbplatsresurser och delning av filer. Detta beror på det fungerar på samma sätt att hämta blobbinnehåll som att läsa andra typer av data på nätet: Du pekar bara på en webbläsare eller något annat som kan göra en GET-begäran i blob-URL:en.

**TODO-bild av offentlig behållare som visar URL:en för direktåtkomst**

Att aktivera offentlig åtkomst är viktigt för skalbarheten, eftersom data som hämtas direkt från blobblagringen inte genererar någon trafik i din app på serversidan. Även om du inte omedelbart har någon nytta av offentlig åtkomst, eller om du vill använda en databas för att styra dataåtkomsten via ditt program, kan du planera att använda separata behållare för data som du vill ska vara offentligt tillgängliga.

> [!CAUTION]
> Blobbarna i en behållare som är konfigurerad för offentlig åtkomst kan laddas ned utan någon typ av autentisering eller granskning av den som känner till lagrings-URL:erna. Placera aldrig blobbdata i en offentlig behållare som du inte planerar att dela offentligt.

Förutom offentlig åtkomst har Azure en signatur för delad åtkomst som gör detaljerade behörigheter enklare att styra i behållare. Med exakt åtkomstkontroll kan scenarier som ytterligare förbättrar skalbarheten användas, så att tänka på behållare som säkerhetsgränser i allmänhet är en bra riktlinje.

### <a name="blob-name-prefixes-virtual-directories"></a>Prefix för blobbnamn (virtuella kataloger)

Tekniskt sett är behållare ”platta” och saknar stöd för all slags kapsling eller hierarki. Men om du ger dina blobbar hierarkiska namn som ser ut som sökvägar (till exempel `finance/budgets/2017/q1.xls`), kan API:ns liståtgärd filtrera resultaten för specifika prefix. Det innebär att du kan navigera i listan som om det fanns ett hierarkiskt system med filer och mappar.

Den här funktionen kallas ofta *virtuella kataloger* eftersom vissa verktyg och klientbibliotek använder den för att visualisera och navigera i blobblagring som om det var ett filsystem. Varje mappnavigering utlöser ett separat anrop för att göra en lista med blobbarna i den mappen.

**TODO-bild av Lagringsutforskaren och/eller portal med mappar**

En vanlig metod för att ordna och navigera i komplexa blobbdata är att använda namn som liknar filnamn för blobbar.

### <a name="blob-types"></a>Blobbtyper

Det finns tre olika typer av blobbar som du kan lagra data i:

- **Blockblobbar** består av block i olika storlekar som kan laddas upp och ned separat och parallellt. Att skriva till en blockblobb innebär att överföra data till block och skicka dem till blobben &mdash; klientbiblioteken hanterar detta åt dig.
- **Bilageblobbar** är specialiserade blockblobbar som endast har stöd för att lägga till nya data (inte uppdatera eller radera befintliga data), men de är mycket effektiva. Att lägga till blobblagring är perfekt vid scenarier som att lagra loggar eller strömma data.
- **Sidblobbar** är utformade för scenarier som innefattar läsning och skrivning med direktåtkomst. Sidblobbar används för att lagra VHD-filer som används av Azures virtuella datorer, men de är också bra för alla scenarier som inbegriper direktåtkomst.

Blockblobbar är det bästa valet för de flesta scenarier som inte anropar specifikt för bilage- eller sidblobbar.