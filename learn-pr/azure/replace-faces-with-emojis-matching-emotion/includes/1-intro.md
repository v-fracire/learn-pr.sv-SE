<span data-ttu-id="6679a-101">TheMojifier är ett Slack- och _snedstrecks_kommando som ersätter folks ansikten i bilder med en emoji som matchar deras känsla så här:</span><span class="sxs-lookup"><span data-stu-id="6679a-101">TheMojifier is a Slack _slash_ command which replaces peoples faces in images with emojis matching their emotion, like so:</span></span>

![Exempelbild](/media-drafts/example-mojify-image.png)

<span data-ttu-id="6679a-103">Det har utformats för att fungera från Slack som ett anpassat kommando. Du kan namnge kommandot som du vill. För det här dokumentet har jag gett det namnet `mojify`.</span><span class="sxs-lookup"><span data-stu-id="6679a-103">It's designed to work from Slack as a custom command, you can name the command how you want, for this document I've named it `mojify`.</span></span>

<span data-ttu-id="6679a-104">Kör kommandot genom att skriva `/mojify <image to mojify>` så här:</span><span class="sxs-lookup"><span data-stu-id="6679a-104">To execute the commmand type `/mojify <image to mojify>`, like so:</span></span>

![Exempelbild](/media-drafts/9.slack-type-mojify.png)

<span data-ttu-id="6679a-106">Sedan gör mojifier följande:</span><span class="sxs-lookup"><span data-stu-id="6679a-106">The mojifier then:</span></span>

1.  <span data-ttu-id="6679a-107">Beräknar känslorna hos alla personer i bilden.</span><span class="sxs-lookup"><span data-stu-id="6679a-107">Calculates the emotion of any people in the image.</span></span>
2.  <span data-ttu-id="6679a-108">Matchar känslouttryck till emojier.</span><span class="sxs-lookup"><span data-stu-id="6679a-108">Matches emotions to emojis.</span></span>
3.  <span data-ttu-id="6679a-109">Ersätter ansiktena med emojier.</span><span class="sxs-lookup"><span data-stu-id="6679a-109">Replaces the faces with emojis.</span></span>
4.  <span data-ttu-id="6679a-110">Publicerar bilden tillbaka till Twitter som ett svar.</span><span class="sxs-lookup"><span data-stu-id="6679a-110">Posts the image back to Twitter as a reply.</span></span>

<span data-ttu-id="6679a-111">Det skrivs med TypeScript och flera Azure-tekniker, inklusive [Azure Functions](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai) och [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)</span><span class="sxs-lookup"><span data-stu-id="6679a-111">It’s written using TypeScript and several Azure technologies including [Azure Functions](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai) and [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)</span></span>

<span data-ttu-id="6679a-112">I den här självstudien förklarar jag hur TheMojifier skapades och visar hur du skapar ditt eget Slack-kommando med hjälp av Azure-tekniker.</span><span class="sxs-lookup"><span data-stu-id="6679a-112">In this tutorial I’m going to explain how TheMojifier was made and show you how to create your own Slack command using Azure technologies.</span></span>

> <span data-ttu-id="6679a-113">ATT GÖRA: var ska det här finnas nu?</span><span class="sxs-lookup"><span data-stu-id="6679a-113">TODO, where will this be now?</span></span>
> <span data-ttu-id="6679a-114">All kod för Mojifier finns på [GitHub](https://github.com/jawache/mojifier)</span><span class="sxs-lookup"><span data-stu-id="6679a-114">All the code for Mojifier is available on [GitHub](https://github.com/jawache/mojifier)</span></span>

# <a name="requirements"></a><span data-ttu-id="6679a-115">Krav</span><span class="sxs-lookup"><span data-stu-id="6679a-115">Requirements</span></span>

<span data-ttu-id="6679a-116">För att kunna skapa mojifier behöver vi använda flera Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="6679a-116">To build the mojifier, we need to use several Azure services.</span></span>

## <a name="azure-cognitive-services"></a><span data-ttu-id="6679a-117">Azure Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="6679a-117">Azure Cognitive Services</span></span>

<span data-ttu-id="6679a-118">Azure Cognitive Services är en uppsättning högnivå-API:er som du kan använda för att lägga till avancerade AI-funktioner i ditt program snabbt.</span><span class="sxs-lookup"><span data-stu-id="6679a-118">Azure Cognitive Services are a set of high-level APIs you can use to add advanced AI functionality into your application quickly.</span></span> <span data-ttu-id="6679a-119">Om du kan göra en HTTP-begäran kan du använda Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="6679a-119">If you can make an HTTP request, you can use Cognitive Services.</span></span>

[<span data-ttu-id="6679a-120">Mer information</span><span class="sxs-lookup"><span data-stu-id="6679a-120">More info</span></span>](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

## <a name="azure-functions"></a><span data-ttu-id="6679a-121">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="6679a-121">Azure Functions</span></span>

<span data-ttu-id="6679a-122">Logic Apps är kraftfulla, men ibland behöver du skriva affärslogik med hjälp av den fullständiga syntaxen i ett programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="6679a-122">As powerful as Logic Apps are sometimes you need to write business logic using the full expressiveness of a programming language.</span></span> <span data-ttu-id="6679a-123">Azure Functions är en teknik som gör att du kan agera värd för kodavsnitt som kan svara på händelser eller HTTP-begäranden. Azure hanterar alla skalningsproblem åt dig, och du betalar bara för det du använder.</span><span class="sxs-lookup"><span data-stu-id="6679a-123">Azure Functions is a technology that lets you host snippets of code that can respond to events or HTTP requests, Azure handles all of the scaling issues for you and you only pay for what you use.</span></span>

[<span data-ttu-id="6679a-124">Mer information</span><span class="sxs-lookup"><span data-stu-id="6679a-124">More info</span></span>](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai)
