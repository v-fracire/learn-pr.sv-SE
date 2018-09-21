Nu när alla krav är uppfyllda kan du skriva kod som skapar en ny lagringskö och lägger till ett meddelande. Normalt skulle vi placera den här koden i apparna på klientsidan där data genereras.

## <a name="add-the-client-library-for-azure-storage"></a>Lägg till klientbiblioteket för Azure Storage

Vi börjar med att lägga till **Azure Storage-klientbiblioteket för .NET** i appen. Du kan installera det med NuGet (en .NET-pakethanterare). 

1. Installera paketet `WindowsAzure.Storage` i projektet med kommandot `dotnet add package`. Du kan göra detta i terminalfönstret _nedan_, Cloud Shell eller i samma mapp som projektet om du arbetar med din lokala dator i en terminal/konsolfönstret.

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a>Lägg till en metod för att skicka en nyhetsavisering

Nu ska vi skapa en ny metod för att skicka en nyhet till en kö.

1. Öppna filen `Program.cs` i ditt kodredigeringsprogram.

1. Lägg till följande namnområden överst i filen. Vi kommer att använda typer från båda när vi ansluter till Azure Storage och sedan arbetar med köer.

    - `System.Threading.Tasks`
    - `Microsoft.WindowsAzure.Storage`
    - `Microsoft.WindowsAzure.Storage.Queue`

1. Skapa en statisk metod i klassen `Program` med namnet `SendArticleAsync` som tar emot ett argument av typen `string` och returnerar ett `Task`-objekt. Vi använder den här metoden till att skicka en nyhetsartikel till vår tjänst. Ge indataparametern namnet `newsMessage` enligt nedan.

    ```csharp
    ...
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue; 
    
    class Program
    {
        ...
    
        static async Task SendArticleAsync(string newsMessage)
        {
        }
    }
    ```
    
1. Använd den statiska metoden `CloudStorageAccount.Parse` i den nya metoden till att parsa anslutningssträngen (kom ihåg att vi lade den i en konstantsträng). Den här metoden returnerar ett `CloudStorageAccount`-objekt som måste lagras i en variabel.

1. Anropa metoden `CreateCloudQueueClient()` i lagringskontoobjektet för att få ett klientobjekt och lagra det i en variabel.

1. Anropa sedan metoden `GetQueueReference` i klientobjektet och skicka strängen ”newsqueue” som könamn. Då returneras ett `CloudQueue`-objekt som vi kan använda för att arbeta med kön. Kön behöver inte finnas ännu.

1. Anropa `CreateIfNotExistsAsync()` på `CloudQueue`-objektet för att se till att kön är redo att användas. Då skapas kön om det behövs.
    - Eftersom det här är en asynkron metod använder du C#-nyckelordet `await` så att vi använder den korrekt. Det innebär också att du måste utöka _metoden_ med nyckelordet `async`. 
    - `CreateIfNotExistsAsync` returnerar ett `bool`-värde som är `true` om kön har skapats och `false` om den redan fanns. Skicka ett meddelande till konsolen om vi skapade kön.
    - Här är ett exempel om du behöver hjälp.

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        // Not Shown here - code from prior steps
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. Skapa en instans av ett `CloudQueueMessage`-objekt. 
    - Det tar ett `string`-värde som parameter, skicka metodparametern (`newsMessage`). Det här blir _brödtexten_ i meddelandet. Det finns också en statisk metod som kan skapa binära meddelanden.

1. Anropa `AddMessageAsync` på `CloudQueue`-objektet för att lägga till meddelandet i kön. Det här är också en asynkron metod, så du måste använda nyckelordet `await`.

1. Spara filen och kompilera den genom att skriva `dotnet build` i kommandofönstret. Om du åtgärda eventuella fel kan du använda följande kod till att kontrollera ditt arbete.

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference("newsqueue");
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a>Lägg till kod för att skicka ett meddelande

Nu ska vi ändra metoden `Main` så att alla mottagna parametrar skickas till vår nya metod, så att vi kan testa den nya sändmetoden.

1. Leta upp metoden `Main`.

1. Kontrollera den överförda `args`-parametern och se om några data skickades till kommandoraden. Använd i så fall `String.Join` och skapa en enda sträng av alla ord, med ett blanksteg som avgränsare.

1. Skicka strängen till den nya `SendArticleAsync`-metoden. 

1. När den returnerar ett värde skriver ut meddelandet som skickades med `Console.WriteLine`.

1. Spara filen och kompilera programmet (`dotnet build`). Du kan använda koden nedan för att kontrollera ditt arbete.

    > [!NOTE]
    > Eftersom vår metod tekniskt sett är asynkron skulle vi normalt vilja använda nyckelordet `await`, men den här funktionen är inte tillgänglig för metoden `Main` såvida du inte använder C# 7.1 eller senare. Det är bara att anropa `Wait()` i metoden så blockeras väntetiden för att metoden ska återvända. Vi kan fixa detta på ett ögonblick.

    ```csharp
    static void Main(string[] args)
    {
        if (args.Length > 0)
        {
            string value = String.Join(" ", args);
            SendArticleAsync(value).Wait();
            Console.WriteLine($"Sent: {value}");
        }
    }
    ```

## <a name="execute-the-application"></a>Köra programmet

Programmet kan nu skicka meddelanden. Du kan testa programmet genom att köra det från kommandoraden med kommandot `dotnet run`. Eventuella ytterligare strängar skickas som parametrar till programmet. 

> [!WARNING]
> Se till att du har sparat alla filer i onlineredigeraren innan du kompilerar och kör programmet.

Du kan till exempel skriva så här:

```azurecli
dotnet run Send this message
```

Nu bör strängen `"Send this message"` läggas till i kön.

## <a name="check-your-results"></a>Kontrollera dina resultat

Du kan kontrollera köer i Azure Portal med hjälp av **Lagringsutforskaren**. Om du öppnar den här produkten kan du se data i alla lagringskonton du äger.

Du kan också använda Azure CLI eller PowerShell. Testa det här kommandot i gränssnittet och ersätt värdet `<connection-string>` med den faktiska anslutningssträngen:

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

Du bör då se informationen i ditt meddelande, ungefär så här:

```json
[
  {
    "content": "U2VuZCB0aGlzIG1lc3NhZ2U=",
    "dequeueCount": 0,
    "expirationTime": "2018-09-05T02:43:40+00:00",
    "id": "b47cbe9f-a246-4a81-86ae-fa02ea8d98bc",
    "insertionTime": "2018-09-24T02:43:40+00:00",
    "popReceipt": null,
    "timeNextVisible": null
  }
]
```

Du kan prova flera andra kommandon i verktygen, skriv både `az storage queue --help` och `az storage message --help` för att utforska dem.

> [!TIP]
> Placera anslutningssträngen i miljövariabeln `AZURE_STORAGE_CONNECTION_STRING` så att du inte behöver skriva parametern `--connection-string` varje gång!

## <a name="optional---use-the-async-versions-of-the-methods"></a>Valfritt – Använda asynkrona versioner av metoderna

Vi använde `Wait()` för sändmetoden ovan men detta är inte ett effektivt bruk av våra processorresurser. I stället ska vi använda C# `async` och metoderna `await`. Men vi kommer att behöva använda _minst_ C# 7.1 för att dessa nyckelord ska kunna tillämpas i vår **Main**-metod.

### <a name="switch-to-c-71"></a>Växla till C# 7.1

C#-`async` och `await`-nyckelord var inte giltiga nyckelord i **Main**-metoderna före C# 7.1. Vi kan enkelt växla till den kompilatorn via en flagga i **.csproj**-filen.

1. Öppna filen **QueueApp.csproj** i redigeraren.

1. Lägg till `<LangVersion>7.1</LangVersion>` i den första `PropertyGroup` i versionsfilen. Den bör se ut enligt nedan när du är klar.
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a>Använda async-nyckelordet

Tillämpa sedan `async`-nyckelordet på **Main**-metoden. Vi måste göra tre saker.

1. Lägga till `async`-nyckelordet i **Main**-metodens signatur.
1. Ändra returtypen från `void` till `Task`.
1. Ta bort anropet till `Wait()` för anropet till `SendArticleAsync` och ersätt det med `await`.

Kör appen igen – det bör fortfarande fungera precis som det innan, men nu kan koden frigöra tråden tillbaka till arbetstråden i .NET medan vi väntar på att meddelandet ska placeras i kö.

Nu när vi har skickat ett meddelande är det sista steget att lägga till stöd för att _ta emot_ meddelanden.