TheMojifier är ett Slack- och _snedstrecks_kommando som ersätter folks ansikten i bilder med en emoji som matchar deras känsla så här:

![Exempelbild](/media-drafts/example-mojify-image.png)

Det har utformats för att fungera från Slack som ett anpassat kommando. Du kan namnge kommandot som du vill. För det här dokumentet har jag gett det namnet `mojify`.

Kör kommandot genom att skriva `/mojify <image to mojify>` så här:

![Exempelbild](/media-drafts/9.slack-type-mojify.png)

Sedan gör mojifier följande:

1.  Beräknar känslorna hos alla personer i bilden.
2.  Matchar känslouttryck till emojier.
3.  Ersätter ansiktena med emojier.
4.  Publicerar bilden tillbaka till Twitter som ett svar.

Det skrivs med TypeScript och flera Azure-tekniker, inklusive [Azure Functions](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai) och [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

I den här självstudien förklarar jag hur TheMojifier skapades och visar hur du skapar ditt eget Slack-kommando med hjälp av Azure-tekniker.

> ATT GÖRA: var ska det här finnas nu?
> All kod för Mojifier finns på [GitHub](https://github.com/jawache/mojifier)

# <a name="requirements"></a>Krav

För att kunna skapa mojifier behöver vi använda flera Azure-tjänster.

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

Azure Cognitive Services är en uppsättning högnivå-API:er som du kan använda för att lägga till avancerade AI-funktioner i ditt program snabbt. Om du kan göra en HTTP-begäran kan du använda Cognitive Services.

[Mer information](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

## <a name="azure-functions"></a>Azure Functions

Logic Apps är kraftfulla, men ibland behöver du skriva affärslogik med hjälp av den fullständiga syntaxen i ett programmeringsspråk. Azure Functions är en teknik som gör att du kan agera värd för kodavsnitt som kan svara på händelser eller HTTP-begäranden. Azure hanterar alla skalningsproblem åt dig, och du betalar bara för det du använder.

[Mer information](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai)
