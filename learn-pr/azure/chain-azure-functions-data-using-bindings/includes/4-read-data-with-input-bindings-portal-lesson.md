För att kunna ansluta till en datakälla måste du konfigurera en *indatabindning*. Den här bindningen gör det möjligt att skriva minimal kod för att skapa ett meddelande. Du behöver inte skriva kod för uppgifter som att öppna en lagringsanslutning. Azure Functions-körningen och bindningen tar hand om de här aktiviteterna åt dig.

## <a name="input-binding-types"></a>Indatabindningstyper

Det finns flera typer av indata. Alla stöder inte både indata och utdata. Du använder dem när du vill att mata in data av den här typen. Här kan ska vi titta på de typer som stöder indatabindningar och när de ska användas.

- **Blob Storage** Blob-lagringsbindningar gör att du kan läsa från en blob.

- **Azure Cosmos DB** Azure Cosmos DB-indatabindningen använder SQL API för att hämta en eller flera Azure Cosmos DB-dokument och skickar dem till indataparametern för funktionen. Dokument-ID:t och frågeparametrarna kan fastställas baserat på den utlösare som anropar funktionen.

- Med **Microsoft Graph**-indatabindningar kan du läsa filer från OneDrive, läsa data från Excel och få auth-token så att du kan interagera med Microsoft Graph API:er.

- **Mobile Apps** Indatabindningen Mobile Apps läser in en post från en mobil tabellslutpunkt och skickar det till din funktion.

- Med **Tabellagring** kan du läsa data och arbeta med Azure Table-lagring.

Om du vill skapa en bindning som indata, måste du definiera `direction` som `in`.
Parametrarna för varje typ av bindning kan variera.

## <a name="what-is-a-binding-expression"></a>Vad är ett bindningsuttryck?

Ett bindningsuttryck är specialiserad text i **function.json**, funktionsparametrar eller kod som utvärderas när funktionen anropas för att ge ett värde. Om du har en bindning för Service Bus-kö kan du använda ett bindningsuttryck för att få könamnet från appinställningarna.

### <a name="types-of-binding-expressions"></a>Typer av bindande uttryck

- Appinställningar
- Filnamn för utlösare
- Utlösarmetadata
- JSON-nyttolaster
- Ny GUID
- Aktuell dag och tid

De flesta uttryck kan identifieras genom att de omsluts av klammerparenteser. Bindningsuttrycken i appinställningarna omges av procenttecekn istället för klammerparenteser. Till exempel, om bindningssökvägen för blob-utdata är `%Environment%/newblob.txt` och appinställningsvärdet för Miljö är Utveckling, skapas en blob i containern Utveckling.

## <a name="summary"></a>Sammanfattning

Med hjälp av indatabindningar kan du koppla vår funktion till en datakälla. Det finns flera typer av datakällor som vi kan ansluta till och parametrarna för var och en kan variera. Vi kan använda bindningsuttryck i *function.json*, funktionsparametrar eller kod för att matcha värden från olika källor.