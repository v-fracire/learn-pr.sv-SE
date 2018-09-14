Åtkomst till och bearbetning av data är viktiga uppgifter i många programvarulösningar. Överväg att vissa av dessa scenarier:

* Du har blivit ombedd att implementera ett sätt att flytta inkommande data från blob storage till Azure Cosmos DB.
* Vill du skicka inkommande meddelanden till en kö för bearbetning av en annan komponent i ditt företag.
* Din tjänst måste hämta från spelarna resultat från en kö och uppdatera en online poängtavlan.

Alla dessa exempel är om hur du flyttar data. Skiljer sig från scenariot till scenariot data källor och mål, men mönstret är liknande. Du kan ansluta till en datakälla du läser och skriver data. Azure Functions kan du integrera med data och tjänster med bindningar. 

## <a name="what-is-a-binding"></a>Vad är en bindning?

I funktioner utdatabindningar en deklarativ metod för att ansluta till data från i din kod. De gör det lättare att integrera med dataströmmar konsekvent i en funktion. En utlösare definierar hur en funktion anropas. Du kan bara ha en utlösare, men du kan ha flera bindningar i en funktion. Detta är kraftfulla eftersom du kan ansluta till dina datakällor utan att behöva hårt code värdena, som exempelvis den *anslutningssträngen*.

### <a name="two-kinds-of-bindings"></a>Två typer av bindningar

Det finns två typer av bindningar som du kan använda för dina funktioner:

1. **Indatabindning** en indatabindning är en anslutning till en **källa**. Vår funktionen läser data från den här källan.

1. **Utdatabindning** en utdatabindning är en anslutning till en **mål**. Vår funktionen skriver data till denna destination.

### <a name="types-of-supported-bindings"></a>Typer av stöds bindningar

Den *typ* definierar där vi läser eller skickar data för bindningen. Det finns en bindning ska svara på webbförfrågningar och ett stort urval av bindningar för att interagera med lagring.

Här är en lista över vanliga Bindningar:
- Blob
- Kö
- CosmosDB
- Event Hubs
- Externa filer
- Externa tabeller
- HTTP

### <a name="binding-properties"></a>Egenskaper för bindning

Det finns fyra egenskaper som krävs i alla bindningar. Du kan behöva ange ytterligare egenskaper som är baserat på vilken typ av bindning och/eller lagring som du använder.

- **Namnet** den `name` definierar egenskapen funktionsparametern genom vilka du komma åt data. Till exempel i en kö-indatabindning är detta namnet på funktionsparametern som tar emot kö meddelandeinnehåll. 

- **Typ** den `type` egensapen identifierar typen av bindning, d.v.s., vilken typ av data eller tjänsten som vi vill interagera med.

- **Riktning** den `direction` egensapen identifierar vilken riktning data flödar ie. är det en inkommande eller utgående bindning?

- **Anslutningen** den `connection` egenskapen är namnet på en appinställning som innehåller anslutningssträngen inte anslutningssträngen själva. Bindningar använda anslutningssträngar lagras i appinställningar för att genomdriva det bästa sättet att function.json innehåller inte service hemligheter.

## <a name="create-a-binding"></a>Skapa en bindning

Bindningar har definierats i JSON. En bindning har konfigurerats i din funktion konfigurationsfil, vilken har namnet `function.json` och finns i samma mapp som funktionskoden.

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

Att skapa bindningen vi:

1. Skapa en bindning i vår `function.json` fil.

1. Ange värdet för den `name` variabeln. I det här exemplet ska variabeln innehålla blob-data.

1. Skapar lagringsutrymmet `type`, i föregående exempel använder Blob storage.

1. Ange den `path`, som anger vilken blobbehållare och objektnamnet som går i den. Den `path` egenskapen krävs för Blobar.

1. Ange den `connection` sträng inställningsnamn som definieras i programmets inställningsfil. Den används som en sökning för att hitta anslutningssträngen för att ansluta till din lagring.

1. Definiera den `direction` som `in`, läses data från Blob.

Bindningar används för att ansluta till data i din funktion. I det här exemplet vi har tittat på används vi en indatabindning för att ansluta användare avbildningar som ska bearbetas av vår funktion som miniatyrbilder.