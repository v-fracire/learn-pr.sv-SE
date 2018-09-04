En Azure-kö är en högpresterande meddelandebuffert som ansluter producenter och konsumenter i ditt program.

Anta att ditt företag publicerar ett program för delning av foto. Användare får åtkomst till programmet antingen via en webbplats eller en mobilapp. Webbplatsen körs på webbservrar som finns hos din Internetleverantör. Affärslogiken för att lägga till, ta bort och redigera foton ligger i en webbtjänst på en mellannivå som finns i Azure. Data lagras i en Azure SQL Database.

Din mellannivå ger tillräckligt med kapacitet för att klara normal belastning. Men under den senaste månförmörkelsen märkte du att systemet blev kraftigt överbelastat när användarna försökte ladda upp sina bilder. Vissa användare klagade över att programmet slutade svara och andra säger att de förlorade sina överförda foton. Sådana här toppar i efterfrågan kommer ske varje gång ett naturfenomen inträffar.

Du behöver ett sätt att hantera dessa oväntade toppar. Du vill undvika att lägga till fler instanser av webbplatsen och webbtjänsten på mellannivå, eftersom det är dyrt och inte behövs under normala förhållanden.

Du kan lösa problemet med hjälp av Azure Queue Storage. Frontend-komponenterna, som t.ex. på webbplatsen och mobila appar kan för varje nytt foto lägga ett meddelande i en kö. Webbtjänsten på mellannivå kan hämta upp meddelandena från kön för bearbetning. Vid tidpunkter med hög efterfrågan växer kön men inga bilder går förlorade och programmet fortsätter att svara. När behovet sjunker tillbaka till normalnivåerna kan webbtjänsten komma ikapp genom att gå igenom kvarvarande uppgifter i kön.

Här visas hur du använder Azure Queue Storage för att hantera hög efterfrågan och förbättra flexibiliteten i dina distribuerade program.

## <a name="learning-objectives"></a>Utbildningsmål
I den här modulen kommer du att:

- Skapa ett Azure Storage-konto med stöd för köer.
- Skapa en kö med C# och Azure Storage-klientbiblioteket för .NET.
- Lägga till, hämta och ta bort meddelanden från en kö med C# och Azure Storage-klientbiblioteket för .NET.

## <a name="prerequisites"></a>Förutsättningar

- Vana att skriva kod i C#
- Erfarenhet av Azure och vanliga Azure-objekt inklusive resursgrupper
- Erfarenhet av installation av NuGet-paket i .NET-program
- Utvecklingsdator med .NET Core-ramverket, Visual Studio Code och kommandoradsverktyget för GIT installerat