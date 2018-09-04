Du använder en kö för att hantera toppar i efterfrågan på ditt program med bidrag av nyhetsartiklar. Du har redan skrivit den kod som skapar en kö och lägger till meddelanden i den.

Nu ska du slutföra programmet genom att skriva kod i den hämtande komponenten som läser nästa meddelande i kön, bearbetar det och sedan tar bort det från kön.

## <a name="dequeue-a-message"></a>Ta bort ett meddelande från kön

Lägga till kod som tar bort nästa meddelande från kön genom att följa dessa steg:

1. I **Visual Studio Code** öppnar du filen **receivearticle/Program.cs**.

1. Leta upp metoden `RetrieveAnArticleAsync()` och ta bort platshållarinstruktionen `throw`. Allt arbete utförs i den här metoden.

1. Använd den statiska metoden `CloudStorageAccount.Parse` så får anslutningssträngen en lagringskontoinstans.

1. Anropa metoden `CreateCloudQueueClient` på lagringskontot för att hämta ett klientobjekt.

1. Anropa metoden `GetQueueReference` på klientobjektet och skicka **newsqueue** för könamnet. Då får du ett `CloudQueue`-objekt.

1. Anropa `GetMessageAsync` på objektet `CloudQueue` för att hämta nästa `CloudQueueMessage` från kön. Det returnerade värdet blir `null` om kön är tom.

1. Använd egenskapen `AsString` på `CloudQueueMessage`-objektet för att hämta innehållet i meddelandet. Skriv ut strängen till konsolfönstret.

1. Anropa `DeleteMessageAsync` på `CloudQueue`-objektet för att ta bort meddelandet från kön.

## <a name="execute-the-application"></a>Köra programmet

Programmet kan nu skicka och hämta meddelanden. Testa det genom att följa dessa steg:

1. I **Visual Studio Code** går du till menyn **Visa** och klickar på **Felsöka**.

1. Längst upp på fönsterrutan **FELSÖKA** går du till listrutan **FELSÖKA** och väljer **.NET Core Launch Receive Article** (.NET Core, starta mottagande av artikel).

1. På menyn **Felsöka** klickar du på **Starta felsökning**. **Visual Studio Code** bygger och kör programmet.

1. Växla till Azure-portalen.

1. I det vänstra navigeringsfönstret klickar du på **Alla resurser**.

1. I listan över resurser klickar du på det lagringskonto som du skapade tidigare.

1. Klicka på **Storage Explorer**.

1. Expandera listan över **KÖER** och klicka sedan på **newsqueue**. Kön är tom eftersom ditt program har tagit bort meddelandet från kön.

## <a name="summary"></a>Sammanfattning

Din kod nu skapar en kö, lägger till meddelanden i den, läser meddelandet längst fram i kön, bearbetar det och tar slutligen bort det bearbetade meddelandet från kön. Koden för lagringskön är nu klar.