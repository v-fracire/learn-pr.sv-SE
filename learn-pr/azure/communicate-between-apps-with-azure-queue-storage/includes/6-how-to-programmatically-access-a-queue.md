Köer innehåller meddelanden - datapaket vars form känns avsändarprogram och mottagarprogram. Avsändaren skapar kön och lägger till ett meddelande. Mottagaren hämtar ett meddelande, bearbetar det och tar sedan bort meddelandet från kön. Följande illustration visar en översikt över en vanlig sådan process.

![En bild som visar ett vanligt meddelandeflöde via Azure Queue.](../media/6-message-flow.png)

Observera att `get` och `delete` är separata åtgärder. Den här strukturen hanterar potentiella fel i mottagaren och implementerar ett begrepp som kallas _leverans minst en gång_. När mottagaren får ett meddelande finns meddelandet kvar i kön men är osynligt i 30 sekunder. Om mottagaren kraschar eller utsätts för ett strömavbrott under bearbetningen tar den aldrig bort meddelandet från kön. Efter 30 sekunder visas meddelandet i kön igen, och då kan en annan instans av mottagaren bearbeta det så att det slutförs.

## <a name="the-azure-storage-client-library-for-net"></a>Azure Storage-klientbiblioteket för .NET

**Azure Storage-klientbiblioteket för .NET** tillhandahåller typer för att representera vart och ett av de objekt som du behöver för att interagera med:

- `CloudStorageAccount` representerar ditt Azure Storage-konto.
- `CloudQueueClient` representerar Azure Queue-lagring.
- `CloudQueue` representerar en av dina köinstanser.
- `CloudQueueMessage` representerar ett meddelande.

Du använder de här klasserna för att få programmatisk åtkomst till din kö. Biblioteket har både synkrona och asynkrona metoder. Du bör använda de asynkrona versionerna för att undvika blockering i klientapparna.

> [!NOTE]
> Azure Storage-klientbiblioteket för .NET finns i **WindowsAzure.Storage** NuGet-paketet. Du kan installera det via IDE, Azure CLI eller PowerShell `Install-Package WindowsAzure.Storage`.

## <a name="how-to-connect-to-a-queue"></a>Ansluta till en kö

Om du vill ansluta till en kö måste du först skapa ett `CloudStorageAccount` med anslutningssträngen. Objektet kan sedan skapa en `CloudQueueClient`, vilket i sin tur kan öppna en `CloudQueue`-instans. Det grundläggande kodflödet visas nedan.

```csharp
CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);

CloudQueueClient client = account.CreateCloudQueueClient();

CloudQueue queue = client.GetQueueReference("myqueue");
```

Att skapa en `CloudQueue` innebär inte nödvändigtvis att den _faktiska_ lagringskön finns, men du kan använda det här objektet för att skapa, ta bort och söka efter en befintlig kö. Som nämnts ovan kan alla metoder användas med både synkrona och asynkrona versioner, men vi kommer endast att använda de `Task`-baserade asynkrona versionerna.

## <a name="how-to-create-a-queue"></a>Så skapar du en resurs

Du ska använda ett vanligt mönster för att skapa en kö: avsändarprogrammet ska alltid ansvara för att skapa kön. Detta gör programmet mer självständigt och mindre beroende av den administrativa strukturen. 

Klientbiblioteket underlättar skapandet genom att exponera en `CreateIfNotExistsAsync`-metod som skapar kön om det behövs, eller returnerar `false` om kön redan finns. 

Den typiska koden visas nedan.

```csharp
CloudQueue queue;
//...

await queue.CreateIfNotExistsAsync();
```

> [!NOTE]
> Du måste ha `Write`- eller `Create`-behörighet till lagringskontot för att kunna använda detta API. Detta gäller alltid om du använder säkerhetsmodellen med **åtkomstnyckel**, men du kan låsa behörigheterna till kontot med andra metoder som enbart tillåter läsåtgärder mot kön.

## <a name="how-to-send-a-message"></a>Så skickar du ett meddelande

För att skicka ett meddelande instansierar du ett `CloudQueueMessage`objekt. Klassen har några överlagrade konstruktorer som läser in dina data i meddelandet. Vi använder den konstruktor som tar en `string`. När meddelandet har skapats använder du ett `CloudQueue`-objekt för att skicka det.

Här är ett vanligt exempel:

```csharp
var message = new CloudQueueMessage("your message here");

CloudQueue queue;
//...

await queue.AddMessageAsync(message);
```

> [!NOTE]
> Den totala köstorleken kan vara upp till 500 TB, men enskilda meddelanden i kön kan endast vara upp till 64 kB stora (48 kB om du använder Base64-kodning). Om du behöver en större nyttolast kan du kombinera köer och blobbar, och skicka webbadressen till de faktiska data (som lagras som en blob) i meddelandet. Med den metoden kan du ha upp till 200 GB för ett enskilt objekt i kön.

## <a name="how-to-receive-and-delete-a-message"></a>Så tar du emot och tar bort ett meddelande

I mottagaren tar du emot nästa meddelande, bearbetar det och tar sedan bort det efter att bearbetningen är klar. Här är ett enkelt exempel:

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

Nu ska vi använda de nya kunskaperna på vårt program!