Här går vi igenom hur du skriver kod för att få åtkomst till en kö. I exemplet med fotodelning skulle detta användas av klientdelsappar för att lägga till foton till kön och av medelnivån att hämta meddelanden för bearbetning.

## <a name="message-flow"></a>Meddelandeflöde

Det typiska flödet för ett meddelande genom kön visas nedan. Avsändaren skapar kön och lägger till ett meddelande. Mottagaren hämtar ett meddelande, bearbetar det och sedan tar bort meddelandet från kön.

![Meddelandeflöde](../media-draft/6-message-flow.png)

Observera att _get_ (hämta) och _delete_ (ta bort) är separata åtgärder. Den här strukturen hanterar potentiella fel i mottagaren och implementerar ett begrepp som kallas _leverans minst en gång_. När mottagaren får ett meddelande finns meddelandet kvar i kön men är osynligt i 30 sekunder. Om mottagaren kraschar eller utsätts för ett strömavbrott under bearbetningen tar den aldrig bort meddelandet från kön. Efter 30 sekunder visas meddelandet i kön igen, och då kan en annan instans av mottagaren bearbeta det så att det slutförs.

## <a name="the-azure-storage-client-library-for-net"></a>Azure Storage-klientbiblioteket för .NET

Azure Storage-klientbiblioteket för .NET tillhandahåller typer för att representera vart och ett av de objekt som du behöver för att interagera med:

- `CloudStorageAccount` representerar ditt Azure Storage-konto
- `CloudQueueClient` representerar Azure Queue Storage-tjänsten
- `CloudQueue` representerar en av dina köinstanser
- `CloudQueueMessage` representerar ett meddelande.

Du använder dessa klasser för att få programmatisk åtkomst till din kö. Biblioteket har både synkrona och asynkrona metoder. Du använder de asynkrona versionerna för att undvika blockering i klientapparna.

> [!NOTE]
> Azure Storage-klientbiblioteket för .NET finns i **WindowsAzure.Storage** NuGet-paketet.

## <a name="how-to-connect"></a>Så ansluter du

Om du vill ansluta till en kö måste du först skapa en `CloudStorageAccount`, därefter en `CloudQueueClient` och slutligen en `CloudQueue`-instans. Koden visas nedan.

```C#
CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);

CloudQueueClient client = account.CreateCloudQueueClient();

CloudQueue queue = client.GetQueueReference("myqueue");
```

## <a name="how-to-create-a-queue"></a>Så skapar du en resurs

Du kommer att använda ett vanligt mönster för skapandet av kön: avsändarprogrammet blir ansvarigt för att skapa kön. Det här gör att programmet blir mer självständigt och mindre beroende av administrativ konfiguration. Den typiska koden visas nedan.

```C#
CloudQueue queue;
//...

await queue.CreateIfNotExistsAsync();
```

## <a name="how-to-send-a-message"></a>Så skickar du ett meddelande

För att skicka ett meddelande instansierar du ett `CloudQueueMessage`objekt. Klassen har några överlagrade konstruktorer som läser in dina data i meddelandet. Vi använder den konstruktor som tar en sträng. Efter att meddelandet har skapats kan du använda ett `CloudQueue`-objekt för att skicka det (se nedan).

```C#
var message = new CloudQueueMessage("your message here");

CloudQueue queue;
//...

await queue.AddMessageAsync(message);
```

## <a name="how-to-receive-and-delete-a-message"></a>Så tar du emot och tar bort ett meddelande

I mottagaren hämtar nästa meddelande, bearbetar det och tar sedan bort det efter att bearbetningen är klar (se nedan).

```C#
CloudQueue queue;
//...

CloudQueueMessage message = await queue.GetMessageAsync();

if (message != null)
{
    // Process the message
    //...

    await queue.DeleteMessageAsync(message);
}
```

## <a name="summary"></a>Sammanfattning

Azure Storage-klientbiblioteket för .NET ger ett enkelt sätt att få åtkomst till köer programmatiskt utan att behöva hantera råa REST-anrop. Om du förstår leverans minst en gång och asynkrona anrop blir det enklare att använda biblioteket på ett effektivt sätt.