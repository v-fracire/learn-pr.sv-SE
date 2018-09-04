Du har valt att använda en kö för att hantera toppar i efterfrågan på ditt program med bidrag av nyhetsartiklar.

I den här enheten skriver du C#-kod som skapar en ny kö i ditt Azure-lagringskonto och lägger till ett meddelande i kön. Den här enheten är en fortsättning på den föregående övningen.

## <a name="create-a-queue"></a>Skapa en kö

Nu när alla krav är uppfyllda kan du skriva kod som skapar en ny lagringskö och lägger till ett meddelande.

1. I **Visual Studio Code** öppnar du filen **sendarticle/Program.cs**.

1. Leta upp metoden `SendAnArticleAsync()` och ta bort platshållarinstruktionen `throw`. Allt arbete utförs i den här metoden.

1. Använd den statiska metoden `CloudStorageAccount.Parse` så får anslutningssträngen en lagringskontoinstans.

1. Anropa metoden `CreateCloudQueueClient` på lagringskontot för att hämta ett klientobjekt.

1. Anropa metoden `GetQueueReference` på klientobjektet och skicka **newsqueue** för könamnet. Då får du ett `CloudQueue`-objekt (det är OK om kön inte finns ännu; det är nästa steg).

1. Anropa `CreateIfNotExistsAsync` på `CloudQueue`-objektet för att se till att kön är redo att användas.

1. Använd `new` för att skapa en instans av `CloudQueueMessage`. Skicka en meddelandesträng till konstruktorn.

1. Anropa `AddMessageAsync` på `CloudQueue`-objektet för att lägga till meddelandet i kön.

## <a name="execute-the-application"></a>Köra programmet

Programmet kan nu skicka meddelanden. Testa det genom att följa dessa steg:

1. I **Visual Studio Code** går du till menyn **Visa** och klickar på **Felsöka**.

1. Längst upp på fönsterrutan **FELSÖKA** går du till listrutan **FELSÖKA** och väljer **.NET Core Launch Send Article** (.NET Core, starta skickande av artikel).

1. På menyn **Felsöka** klickar du på **Starta felsökning**. **Visual Studio Code** bygger och kör programmet.

1. Växla till Azure-portalen.

1. I det vänstra navigeringsfönstret klickar du på **Alla resurser**.

1. I listan över resurser klickar du på det lagringskonto som du skapade tidigare.

1. Klicka på **Storage Explorer**.

1. Expandera listan över **KÖER** och klicka sedan på **newsqueue**. Portalen visar det meddelande som lades till av ditt program.

## <a name="summary"></a>Sammanfattning

Den kod som skapar en meddelandekö och lägger till meddelanden i den är nu klar. Därefter behöver du skriva kod som hämtar meddelanden från kön.
