Liksom för indatabindningar finns det flera typer av utdatabindningar. Alla stöder inte både indata och utdata. Du använder dem när du vill skicka eller ta emot data. Här tittar vi på de typer som stöder utdatabindningar och på när du använder dem.

## <a name="output-binding-types"></a>Typer av utdatabindningar

- **Blob Storage** – Du kan använda blob-utdatabindningen till att skriva blobar.

- **Cosmos DB** – Med Azure Cosmos DB-utdatabindningen kan du skriva ett nytt dokument till enAzure Cosmos DB-databas med SQL-API.

- **Event Hubs** – Använd Event Hubs-utdatabindningen till att skriva händelser till en händelseström. Du måste ha behörighet att skicka till en händelsehubb för att kunna skicka händelser till den.

- **HTTP** – Använd HTTP-utdatabindningen till att svara på avsändaren av HTTP-begäran. Den här bindningen kräver en HTTP-utlösare och du kan anpassa svaret som associeras med utlösarens begäran. Detta kan också användas för att ansluta till webhooks.

- **Microsoft Graph** – Med Microsoft Graph-utdatabindningen kan du skriva till filer på OneDrive, ändra Excel-data och skicka e-post via Outlook.

- **Mobile Apps** – Mobile Apps-utdatabindningen skriver en ny post till en Mobile Apps-tabell.

- **Notification Hubs** – Du kan skicka push-meddelanden med Notification Hubs-utdatabindningar.

- **Queue Storage** – Använd Azure Queue Storage-utdatabindningen till att skriva meddelanden till en kö.

- **Send Grid** – Skicka e-postmeddelanden med SendGrid-bindningar.

- **Service Bus** – Använd Azure Service Bus-utdatabindningen till att skicka kö- eller ämnesmeddelanden.

- **Table Storage** – Använd en Azure Table Storage-utdatabindning till att skriva till en tabell på ett Azure Storage-konto.

- **Twilio** – Skicka SMS med Twilio.

Om du vill skapa en bindning som utdata, måste du definiera `direction` som `out`. Parametrarna för varje typ av bindning kan variera.

## <a name="combining-input-and-output-bindings"></a>Kombinera indata- och utdatabindningar 

Det är möjligt att tillämpa flera bindningar på en enda funktion. Därmed kan du definiera såväl bindningar för såväl indata som utdata och de kan till och med ha samma bindningstyp.