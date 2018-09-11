Nu gör vi färdigt programmet genom att skriva kod som läser nästa meddelande i kön, bearbetar det och sedan tar bort det från kön. 

Vi placerar koden i samma program och kör den när du inte skickar några parametrar, men i vårt nyhetsscenario skulle vi egentligen placera koden på våra mellannivåservrar för bearbetning av nyheterna.

## <a name="dequeue-a-message"></a>Ta bort ett meddelande från kön

Vi lägger till en ny metod som hämtar nästa meddelande från kön.

1. Växla till mappen `QueueApp` i Cloud Shell och skriv `code .` för att öppna onlineredigeraren.
 
1. Öppna källfilen `Program.cs`.

1. Skapa en statisk metod i klassen `Program` med namnet `ReceiveArticleAsync` som inte tar några parametrar och returnerar en `Task<string>`. Vi använder den här metoden till att hämta en nyhetsartikel från kön och returnera den.
    - Gå vidare och lägg till nyckelordet `async` i metoden eftersom vi ska använda en del asynkrona `Task`-baserade metoder.

    ```csharp
    static async Task<string> ReceiveArticleAsync()
    {
    }

1. All of the setup code to get a `CloudQueue` will be identical to what we did in the last exercise. Code duplication is a bad habit, even in samples so go ahead and, refactor the code that obtains the `CloudQueue` to a new method named `GetQueue` and change the `SendArticleAsync` to use your new method.
     - Make sure to leave the code that _creates_ the queue in the `SendArticleAsync` method; remember only the **publisher** should create the queue.

    ```csharp
    const string ConnectionString = ...;
    // ...

    static CloudQueue GetQueue()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        return queueClient.GetQueueReference("newsqueue");
    }
    ```
    
1. I metoden `ReceiveArticleAsync` ska du anropa den nya `GetQueue`-metoden för att hämta köreferensen och tilldela den till en variabel.

1. Anropa sedan metoden `ExistsAsync` i `CloudQueue`-objektet, då returneras information om kön har skapats. Om vi försöker hämta ett meddelande från en kö som inte finns genereras ett undantag i API:t.
    - Den här metoden är asynkron, så använde `await` för att hämta returvärdet.
    - Du bör redan ha nyckelordet `async` i metoden `ReceiveArticleAsync`, annars lägger du till det nu.


1. Lägg till ett `if`-block som använder returvärdet från `ExistsAsync`. Vi lägger till kod för att läsa in ett värde från kön till blocket. Lägg till en sista retursträngen i metoden som anger att inget värde har lästs. Metoden bör se ut ungefär så här:

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
    }

    return "<queue empty or not created>";
}
```

1. Anropa `GetMessageAsync` i `CloudQueue`-objektet för att hämta den första `CloudQueueMessage`-instansen från kön. Det returnerade värdet blir `null` om kön är tom.

1. Om värdet inte är null använder du egenskapen `AsString` i `CloudQueueMessage`-objektet till att hämta meddelandets innehåll.

1. Anropa `DeleteMessageAsync` i `CloudQueue`-objektet för att ta bort meddelandet från kön.

Implementeringen av den sista metoden bör se ut ungefär så här:

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
        CloudQueueMessage retrievedArticle = await queue.GetMessageAsync();
        if (retrievedArticle != null)
        {
            string newsMessage = retrievedArticle.AsString;
            await queue.DeleteMessageAsync(retrievedArticle);
            return newsMessage;
        }
    }

    return "<queue empty or not created>";
}
```

## <a name="call-the-receivearticleasync-method"></a>Anropa metoden ReceiveArticleAsync

Slutligen ska vi lägga till stöd för att anropa vår nya metod. Det här gör vi när vi inte skickar några parametrar till programmet.

1. Leta rätt på metoden `Main`, och i synnerhet `if`-blocket du lade till tidigare för att söka efter parametrar.

1. Lägg till ett `else`-villkor och anropa metoden `ReceiveArticleAsync`. 

1. Eftersom metoden är asynkron vill vi normalt använda `await`, men som vi förklarade tidigare så fungerar inte det i alla versioner av C#. Använd istället egenskapen `Result` till att hämta returvärdet och skriva ut det i konsolfönstret.

Koden bör se ut ungefär så här:

```csharp
if (args.Length > 0)
{
    // ...
}
else
{
    string value = ReceiveArticleAsync().Result;
    Console.WriteLine($"Received {value}");
}
```

## <a name="execute-the-application"></a>Kör programmet

Koden är nu färdig. Programmet kan skicka och hämta meddelanden. Testa det med `dotnet run` och ange parametrar för att skicka meddelanden, och kör programmet utan parametrar för att läsa enstaka meddelanden.

Om du vill testa programmet när det inte finns någon kö kan du ta bort kön (och alla data) med Azure CLI. Se till att ersätta parametern `<connection-string>` (eller ställa in miljövariabeln).

```azurecli
az storage queue delete --name newsqueue --connection-string <connection-string> 
```

Nästa gång du lägger till ett meddelande ska kön återskapas.