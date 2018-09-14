För att kunna ansluta till en datakälla som vi måste konfigurera en *indatabindning*. Den här bindningen gör det möjligt att skriva minimal kod för att skapa ett meddelande. Du behöver skriva kod för uppgifter som att öppna en lagringsanslutning. Azure Functions runtime och bindningen hand tar om dessa uppgifter åt dig.

## <a name="input-binding-types"></a>Ange bindningstyper

Det finns flera typer av indata, men inte alla typer stöder både indata och utdata. Du använder dem när du vill att mata in data av den typen. Här kan ska vi titta på de typer som stöder indatabindningar och när de ska användas.

- **BLOB-lagring** blob storage-bindningar kan du läsa från en blob.

- **Cosmos DB** för Azure Cosmos DB-indatabindning använder SQL-API för att hämta en eller flera Azure Cosmos DB-dokument och skickar dem till Indataparametern för funktionen. Dokument-ID eller frågeparametrar kan fastställas baserat på utlösare som anropar funktionen.

- **Microsoft Graph** indatabindningar för Microsoft Graph kan du läsa filer från OneDrive, läsa data från Excel och få auth-token så att du kan interagera med Microsoft Graph API: er.
- **Mobile Apps** The Mobile Apps-indatabindning läser in en post från mobila tabellslutpunkt och skickar det till din funktion.

- **Tabellagring** du kan läsa data och arbeta med Azure Table storage.

## <a name="how-to-create-an-input-binding"></a>Så här skapar du en indatabindning?

För att definiera en bindning indata, måste du definiera den `direction` som `in`.
Parametrarna för varje typ av bindning kan skilja sig, de som är väl dokumenterat i [Microsofts dokumentation](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings?azure-portal=true)

## <a name="what-is-a-binding-expression"></a>Vad är ett uttryck för bindning?

Ett uttryck för bindning är specialiserade text i function.json, funktionsparametrar eller kod som utvärderas när funktionen anropas för att ge ett värde. Du kan till exempel använda en bindning-uttryck för att hämta den aktuella tiden eller hämta ett värde från appen inställningar.

### <a name="types-of-binding-expressions"></a>Typer av uttryck för bindning

- Appinställningar
- Filnamn för utlösare
- Utlösaren metadata
- JSON-nyttolaster
- Nytt GUID
- Aktuellt datum och tid
- Uttryck för bindning

De flesta uttryck identifieras genom att omsluta dem anger mellan klammerparenteser. Men app inställningen bindning uttryck identifieras på olika sätt från andra uttryck för bindning: de placeras i procenttecken snarare än av klammerparenteser. Till exempel om bindningssökväg för blob-utdata är `%Environment%/newblob.txt` och miljö appen Inställningsvärdet är utveckling, skapas en blob i behållaren utveckling.

Indatabindningar kan vi ansluta vår funktion till en datakälla. Det finns flera typer av datakällor som vi kan ansluta till och parametrarna för varje kan variera. Vi kan använda bindningen uttryck i function.json, funktionsparametrar eller kod, för att matcha värden från olika källor.