När du skapar en app som behöver lagra data, är det viktigt att tänka på hur appen ska organisera data i lagringskonton, containrar och blobar.

## <a name="storage-accounts"></a>Lagringskonton

Ett enda lagringskonto är tillräckligt flexibelt för att du ska kunna organisera dina blobar hur du vill, men du kan använda fler lagringskonton om du vill kunna separera kostnader och styra åtkomsten till data.

## <a name="containers-and-blobs"></a>Containrar och blobar

Typen av program och data som lagras ska avgöra vilken strategi du väljer för att namnge och organisera containrar och blobar.

Appar som använder blobar som en del av ett lagringsschema med en databas är vanligtvis inte särskilt beroende av organisation, namngivning eller metadata för att kunna visa något om sina data. Sådana appar använder ofta identifierare som ex.vis GUID som blobnamn och refererar till dessa identifierare i databasposter. Appen kommer att använda databasen för att fastställa var blobarna lagras och vilken typ av data de innehåller.

Andra appar kan använda Azures bloblagring mer som ett personligt filsystem, där container- och blobnamn används för att visa innebörd och struktur. Blobnamnen i dessa typer av appar ser ofta ut som traditionella filnamn med filnamnstillägg som t.ex. `.jpg` för att ange vilken typ av data de innehåller. De använder virtuella kataloger (se nedan) till att organisera blobarna och använder ofta metadatataggar för att lagra information om blobar och containrar.

Det finns några viktiga saker att tänka på när du bestämmer hur du ska organisera och lagra blobar och containrar.

### <a name="naming-limitations"></a>Namnbegränsningar

Container- och blobnamn måste följa en uppsättning regler, inklusive längdbegränsningar och teckenbegränsningar. Se avsnittet Ytterligare läsning i slutet av den här modulen för mer detaljerad information om regler för namngivning.

### <a name="public-access-and-containers-as-security-boundaries"></a>Offentlig åtkomst och containrar som säkerhetsgränser

Som standard kräver alla blobar autentisering för åtkomst. Enskilda containrar kan dock konfigureras för att tillåta offentlig hämtning av sina blobar utan autentisering. Den här funktionen stöder olika typer av användning, till exempel värdbaserade statiska webbplatsresurser och delning av filer. Detta beror på att det fungerar på samma sätt att hämta blobinnehåll som att läsa andra typer av data på nätet: Du pekar bara på en webbläsare eller något annat som kan göra en GET-begäran i blob-URL:en.

Att aktivera offentlig åtkomst är viktigt för skalbarheten, eftersom data som hämtas direkt från bloblagringen inte genererar någon trafik i din app på serversidan. Även om du inte omedelbart har någon nytta av offentlig åtkomst, eller om du vill använda en databas för att styra dataåtkomsten via ditt program, kan du planera att använda separata containrar för data som du vill ska vara offentligt tillgängliga.

> [!CAUTION]
> Blobarna i en container som är konfigurerad för offentlig åtkomst kan laddas ned utan någon typ av autentisering eller granskning av den som känner till lagrings-URL:erna. Placera aldrig blobdata i en offentlig container som du inte planerar att dela offentligt.

Förutom offentlig åtkomst har Azure en signatur för delad åtkomst som gör detaljerade behörigheter enklare att styra i containrar. Med exakt åtkomstkontroll kan scenarier som ytterligare förbättrar skalbarheten användas, så att tänka på containrar som säkerhetsgränser i allmänhet är en bra riktlinje.

### <a name="blob-name-prefixes-virtual-directories"></a>Prefix för blobnamn (virtuella kataloger)

Tekniskt sett är containrar ”platta” och saknar stöd för all slags kapsling eller hierarki. Men om du ger dina blobar hierarkiska namn som ser ut som sökvägar (till exempel `finance/budgets/2017/q1.xls`), kan API:ns liståtgärd filtrera resultaten för specifika prefix. Det innebär att du kan navigera i listan som om det fanns ett hierarkiskt system med filer och mappar.

Den här funktionen kallas ofta *virtuella kataloger* eftersom vissa verktyg och klientbibliotek använder den för att visualisera och navigera i bloblagring som om det var ett filsystem. Varje mappnavigering utlöser ett separat anrop för att göra en lista med blobarna i den mappen.

En vanlig metod för att ordna och navigera i komplexa blobdata är att använda namn som liknar filnamn för blobar.

### <a name="blob-types"></a>Blobtyper

Det finns tre olika typer av blobar som du kan lagra data i:

- **Blockblobar** består av block i olika storlekar som kan laddas upp och ned separat och parallellt. Att skriva till en blockblob innebär att överföra data till block och skicka dem till bloben &mdash; klientbiblioteken hanterar detta åt dig.
- **Bilageblobar** är specialiserade blockblobar som endast har stöd för att lägga till nya data (inte uppdatera eller radera befintliga data), men de är mycket effektiva. Att lägga till bloblagring är perfekt vid scenarier som att lagra loggar eller strömma data. 
- **Sidblobar** är utformade för scenarier som innefattar läsning och skrivning med direktåtkomst. Sidblobar används för att lagra VHD-filer som används av Azures virtuella datorer, men de är också bra för alla scenarier som inbegriper direktåtkomst.

Blockblobar är det bästa valet för de flesta scenarier som inte anropar specifikt för bilage- eller sidblobar.