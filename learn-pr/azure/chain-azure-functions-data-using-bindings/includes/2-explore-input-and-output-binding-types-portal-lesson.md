Åtkomst till och bearbetning av data är viktiga uppgifter i många programvarulösningar. Tänk på några av följande scenarier:

* Du har blivit ombedd att implementera ett sätt att flytta inkommande data från blogglagring till Azure Cosmos DB.
* Du vill skicka inkommande meddelanden till en kö för bearbetning av en annan komponent i företaget.
* Di tjänst måste hämta spelresultat från en kö och uppdatera en poängtavla online.

Alla dessa exempel handlar om att flytta data. Datakällan och -målen skiljer sig för olika scenarier men mönstret är liknande. Du ansluter till en datakälla och läser och skriver data. Azure Functions hjälper dig att integrera data och tjänster med bindningar. 

## <a name="what-is-a-binding"></a>Vad är en bindning?

I Azure Functions utgör bindningar ett deklarativt sätt att ansluta till data i koden. De gör det lättare att integrera dataströmmar konsekvent i en funktion. Du kan ha flera bindningar som ger åtkomst till olika dataelement. Detta är kraftfullt eftersom du kan ansluta till dina datakällor utan att behöva koda en specifik anslutningslogik (som databasanslutningar eller webb-API-gränssnitt).

### <a name="types-of-bindings"></a>Typer av bindningar

Det finns två typer av bindningar du kan använda för funktioner:

1. **Indatabindning** En indatabindning är en anslutning till en **datakälla**. Vår funktion kan läsa data från dessa indata.

1. **Utdatabindning** En utdatabindning är en anslutning till ett **datamål**. Vår funktion kan skriva data till dessa mål.

Det finns även utlösare. Utlösare är särskilda typer av indatabindningar som kör en funktion. Till exempel kan ett Azure Event Grid-meddelande konfigureras som en utlösare. När en händelse inträffar, körs funktionen.

### <a name="types-of-supported-bindings"></a>Typer av bindningar som stöds

*Typen* av bindning definierar var data läses eller vart de skickas. Det finns en bindning om att svara på webbförfrågningar och ett stort urval av bindningar om att interagera direkt med olika Azure-tjänster samt tjänster från tredje part.

En bindningstyp kan användas som indata, utdata eller båda. Till exempel kan en funktion skriva till en Azure Blob Storage-utdatabindning, men en uppdatering i Blob Storage kan utlösa en annan funktion.

Några vanliga bindningstyper visas nedan:
- Blob Storage
- Azure Service Bus-köer
- Azure Cosmos DB
- Azure Event Hubs
- Externa filer
- Externa tabeller
- HTTP-slutpunkter

Dessa typer är bara ett urval. Det finns fler och funktioner har en utöknignsmodell för att lägga till fler bindningar.

### <a name="binding-properties"></a>Bindningsegenskaper

Tre egenskaper krävs i alla bindningar. Du kanske måste ange ytterligare egenskaper utifrån den typ av bindning och lagring du använder.

1. **Namn** Definierar funktionsparametern genom vilken du får åtkomst till data. I till exempel en köindatabindning är detta namnet på funktionsparametern som tar emot kömeddelandeinnehållet. 

1. **Typ** Identifierar typen av bindning, alltså den typ av data eller tjänst som vi vill interagera med.

1. **Riktning** Identifierar vilken riktning data flödar, alltså om det är en indata- eller utdatabindning.

De flesta bindningstyper behöver dessutom en fjärde egenskap: 

4. **Anslutning** Innehåller namnet på en appinställningsnyckel som innehåller anslutningssträngen. Bindningar med anslutningssträngar som lagras i appinställningar för att hålla hemligheter utanför funktionskoden. Detta gör koden mer konfigurerbar och säker.

## <a name="create-a-binding"></a>Skapa en bindning

Bindningar definieras i JSON. En bindning konfigureras i funktionens konfigurationsfil, som har namnet *function-json* och finns i samma mapp som funktionskoden.

 Låt oss nu undersöka ett exempel på en *indatabindning*:

```json
    ...
    {
      "name": "headshotBlob",
      "type": "blob",
      "path": "thumbnail-images/{filename}",
      "connection": "HeadshotStorageConnection",
      "direction": "in"
    },
    ...
```

Vi gör följande för att skapa bindningen:

1. Skapa en bindning i vår *function.json*-fil.

1. Ange värdet för variabeln `name`. I det här exemplet ska variabeln innehålla blob-data.

1. Ange `type` för lagringen. I föregående exempel används Blob Storage.

1. Ange `path`, som anger containern och objektnamnet som placerar där. Egenskapen `path` krävs för blobar.

1. Ange namnet på `connection`-stränginställningen som definieras programmets inställningsfil. Det används som nyckel för att hitta anslutningssträngen för att ansluta till lagringskontot.

1. Definiera `direction` som `in`. Det läser data från bloben.

Bindningar används till att ansluta till data i en funktion. I det här exemplet har vi använt en indatabindning för att ansluta till användarbilder som ska bearbetas av funktionen som miniatyrbilder.
