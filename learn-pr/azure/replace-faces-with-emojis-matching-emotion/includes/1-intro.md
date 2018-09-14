TheMojifier är en Slack _snedstreck_ kommando som ersätter folk ansikten i bilder med emojis som matchar deras känslor, t.ex:

![Exempelbild](/media-drafts/example-mojify-image.png)

Det har utformats för att arbeta från Slack som ett anpassat kommando. Du kan kalla kommandot vad du tycker, men för den här modulen vi kallar den `mojify`.

För att köra kommandot, skriver `/mojify <image to mojify>`:

![Exempelbild](/media-drafts/9.slack-type-mojify.png)

Mojifier kommer sedan att:

  1.  Beräkna känslan hos alla personer i bild
  2.  Matcha känslor till emojis
  3.  Ersätter ansikten i bild med emojis
  4.  Publicera den uppdaterade avbildningen till Slack för svar

Mojifier skrivs med TypeScript och flera Azure-tekniker, inklusive [Azure Functions](https://azure.microsoft.com/services/functions/) och [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/). Du använder dessa för att göra en egen version av _TheMojifier_. 

> [!NOTE] 
> All kod för Mojifier finns på [GitHub](https://github.com/microsoftdocs/mslearn-the-mojifier).

## <a name="tools-youll-use"></a>Verktyg du behöver

### <a name="azure-cognitive-services"></a>Azure Cognitive Services

Azure Cognitive Services är en uppsättning avancerade API: er som du kan använda för att snabbt lägga till avancerade artificiell intelligens (AI) funktioner i en app. Om du vet hur du gör en HTTP-förfrågan sedan ska du kunna använda Cognitive Services.

### <a name="azure-functions"></a>Azure Functions

Azure Functions kan du vara värd för kodfragment som kan svara på händelser eller HTTP-begäranden. Azure hanterar automatiskt skalning problem och du betalar bara för det du använder. Som med allt på Microsoft Learn går vi igenom alla kostnader för dig i learning-miljö.
