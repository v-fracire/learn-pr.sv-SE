En Azure-funktionsapp fungerar inte förrän något beordrar den att köras. Vi kan till exempel skapa en Azure-funktion som skickar ett textmeddelande med en påminnelse till våra kunder innan en avtalad tid. Men om vi inte berättar för funktionen när den ska köras så kommer kunderna aldrig att få meddelandet. 

Här kommer du att titta närmare på utlösare på en hög nivå och utforska de vanligaste typerna av utlösare.

## <a name="what-is-a-trigger"></a>Vad är en utlösare?

En utlösare är ett objekt som definierar hur en Azure-funktion anropas. Om du till exempel vill att en funktion ska köras var tionde minut kan du använda en timerutlösare.

Varje funktion måste ha exakt en utlösare kopplad till den. Om du vill köra en del av en logik som körs under flera olika villkor så måste du skapa flera funktioner som delar samma underliggande funktionskod.

I den här modulen kommer vi att fokusera på **timer-**, **HTTP-** och **blob-** utlösartyper.

## <a name="types-of-triggers"></a>Utlösartyper

Azure Functions stöder en mängd olika utlösartyper. Här är några av de vanligaste:

| Typ | Syfte |
| --- | --- |
| **Timer** | Kör en funktion med bestämda intervall. |
| **HTTP** | Kör en funktion när en HTTP-begäran tas emot. |
| **Blob** | Kör en funktion när en fil laddas upp eller uppdateras i Azure Blob Storage. |
| **Kö** | Kör en funktion när ett meddelande läggs till i en Azure Storage-kö. |
| **Cosmos DB** | Kör en funktion när ett dokument ändras i en samling. |
| **Händelsehubb** | Kör en funktion när en händelsehubb tar emot en ny händelse. |

## <a name="what-is-a-binding"></a>Vad är en bindning?

En bindning är en anslutning till data i din funktion. Bindningar är valfria och finns som indata- och utdatabindningar. En indatabindning är data som funktionen tar emot. En utdatabindning är data som funktionen skickar.

Till skillnad från utlösare kan en funktion ha flera indata- och utdatabindningar.

I nästa övning ska vi köra en funktion enligt ett schema med hjälp av en timerutlösare.