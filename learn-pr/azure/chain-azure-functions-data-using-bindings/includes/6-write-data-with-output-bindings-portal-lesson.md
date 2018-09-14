På liknande sätt som att ange bindningar, det finns flera typer av utdatabindningar.

Det finns flera typer av utdatabindningar, men inte alla typer stöder både indata och utdata. Du använder dem när du vill skicka eller lagra data. Här kan ska vi titta på de typer som stöder utdatabindningar och när de ska användas.

## <a name="output-binding-types"></a>Utdatatyper för bindning

- **BLOB-lagring** du kan använda blob-utdatabindning för att skriva BLOB-objekt.

- **Cosmos DB** bindning kan du skriva ett nytt dokument till en Azure Cosmos DB-databas med hjälp av SQL-API för Azure Cosmos DB-utdata.

- **Händelsehubbar** använda Event Hubs-utdatabindning till skriva händelser till en händelseström. Du måste ha skicka behörighet till en event hub att skriva händelser till den.

- **HTTP** Använd HTTP-utdatabindning svarar på http-begäran avsändaren. Den här bindningen kräver en HTTP-utlösare och kan du anpassa svaret som är associerade med utlösarens begäran.

- **Microsoft Graph** utdatabindningar för Microsoft Graph kan du skriva till filer i OneDrive, ändra Excel-data och skicka e-post via Outlook.

- **Mobile Apps** The Mobile Apps utdata bindning skrivningar till en ny post i en tabell med Mobile Apps.

- **Meddelandehubbar** du kan skicka push-meddelanden med Notification Hubs utdatabindningar.

- **Queue Storage** använda den Azure Queue storage-utdatabindning för att skriva meddelanden till en kö.

- **Skicka rutnät** skicka e-postmeddelanden med SendGrid-bindningar.

- **Service Bus** Använd Azure Service Bus-utdatabindning att skicka meddelanden från kön eller ämnet.

- **Tabellagring** används en Azure Table storage-utdatabindning att skriva till en tabell i ett Azure Storage-konto.

- **Twilio** skicka SMS med Twilio.

- **Webhooks** Använd HTTP-utdatabindning svarar på http-begäran avsändaren. Den här bindningen kräver en HTTP-utlösare och kan du anpassa svaret som är associerade med utlösarens begäran.

## <a name="how-to-create-an-output-binding"></a>Så här skapar du en utdatabindning?
För att definiera en bindning indata, måste du definiera den `direction` som `out`.
Parametrarna för varje typ av bindning kan skilja sig, de som är väl dokumenterat i [Microsofts dokumentation](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings)

## <a name="combining-input-and-output-bindings"></a>Kombinera indata och utdata bindningar 
Det går att använda flera bindningar till en enda funktion. På så sätt kan du definiera både inkommande och utgående bindningar.

Och indata och utdata kan även vara samma bindningstypen...