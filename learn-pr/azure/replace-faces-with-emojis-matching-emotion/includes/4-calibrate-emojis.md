<span data-ttu-id="b3dc3-101">Vi kan inte skicka en bild av emojin till ansikts-API:n för att hämta dess känsla för att, ja, för att det inte är mänskligt helt enkelt.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-101">We can’t pass an image of the emoji to the Face API to get it’s emotion because, well because it’s not human.</span></span> <span data-ttu-id="b3dc3-102">Så för varje emoji behövde jag en mänsklig proxy, nämligen jag själv.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-102">So for each emoji, I needed a human proxy, me.</span></span>

<span data-ttu-id="b3dc3-103">Jag tog bilder av mig själv där jag _noggrant_ imiterade varje emoji och använde bildens _känslomässiga uttryck_ som proxy för emojin.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-103">I took pictures of myself _accurately_ mimicking each emoji, and used the _emotional point_ for that image as the proxy for the emoji.</span></span> <span data-ttu-id="b3dc3-104">För skojs skull valde jag också människor i mitt team och associerade dem med emojis, så här:</span><span class="sxs-lookup"><span data-stu-id="b3dc3-104">To keep things interesting I also chose people from my team and associated them with emojis as well, like so:</span></span>

![Team Moji](/media-drafts/team.jpg)

<span data-ttu-id="b3dc3-106">För emojin med kärleksögon (😍) valde jag en bild på min fru ❤️.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-106">For the emoji of love eyes (😍) I chose a picture of my wife ❤️.</span></span> <span data-ttu-id="b3dc3-107">Till minne av [Stephen Hawking](https://en.wikipedia.org/wiki/Stephen_Hawking) valde jag en bild av honom som ska representera 🤔.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-107">In memory of [Stephen Hawking](https://en.wikipedia.org/wiki/Stephen_Hawking) I picked a picture of him to represent 🤔.</span></span>

<span data-ttu-id="b3dc3-108">Du kan se listan över proxybilder för varje emoji i mappen `bin/proxy-images` i exempelkoden som är associerad till den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-108">You can see the list of proxy images for each emoji in the `bin/proxy-images` folder in the sample code associated with this tutorial.</span></span>

<span data-ttu-id="b3dc3-109">I det här kapitlet kommer du att generera en nyckel så att du kan använda Azures ansikts-API och sedan använda ansikts-API:n för att kalibrera alla emojis med proxybilder av mig.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-109">In this chapter you will generate a key so you can use the Azure Face API and then use the Face API to calibrate all the emojies using proxied images of me.</span></span>

## <a name="generate-an-azure-face-api-key"></a><span data-ttu-id="b3dc3-110">Generera en nyckel för ansikts-API i Azure</span><span class="sxs-lookup"><span data-stu-id="b3dc3-110">Generate an Azure Face API Key</span></span>

<!-- To make calls to the Azure Face API we will need a special authorization key.

We are going to create one using the `az` CLI. -->

<span data-ttu-id="b3dc3-111">För att använda ansikts-API i Azure behöver vi en särskild autentiseringsnyckel, så gå till https://azure.microsoft.com/try/cognitive-services/ och registrera dig för en kostnadsfri utvärderingsversion av ansikts-API.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-111">To use the Azure Face API we need a special authentication key, head over to https://azure.microsoft.com/try/cognitive-services/ and signup to trial the Face API.</span></span>

![Team Moji](/media-drafts/4.calibrating-emojis.get-face-api.png)

> <span data-ttu-id="b3dc3-113">TODO: Hitta az-kommandon för att skapa ansikts-API och hämta nycklar</span><span class="sxs-lookup"><span data-stu-id="b3dc3-113">TODO: Find az commands to create faceAPI and grab keys</span></span>

<!-- > NOTE the Azure Face API doesn't return the emotion information by default, we need to switch on this behavior by setting some query parameters, like so:
> https://westeurope.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion -->

## <a name="setup-the-environment-variables"></a><span data-ttu-id="b3dc3-114">Konfigurera miljövariabler</span><span class="sxs-lookup"><span data-stu-id="b3dc3-114">Setup the environment variables</span></span>

<span data-ttu-id="b3dc3-115">Kalibreringsskriptet behöver veta webbadressen till din ansikts-API och nyckel för att göra rätt anrop, istället för att hårdkoda dem i skriptet vi ska använda miljövariabler i, och köra de här kommandona i terminalen du förväntar dig att köra i programmet i:</span><span class="sxs-lookup"><span data-stu-id="b3dc3-115">The calibration script needs to know your Face API URL and Key in order to make the correct calls, rather than hardcoding these in the script we are going to use environment varialbes, run these commands in the terminal you expect to run the application in:</span></span>

```bash
FACE_API_URL=<the-face-api-url>
FACE_API_KEY=<your-face-api-key>
```

<!-- > NOTE
> Don't forget to add the query param returnFaceAttributes=emotion to ensure the Face API returns emotion as well -->

## <a name="create-some-proxy-images-for-emojis"></a><span data-ttu-id="b3dc3-116">Skapa proxybilder för emojis</span><span class="sxs-lookup"><span data-stu-id="b3dc3-116">Create some proxy images for emojis</span></span>

<span data-ttu-id="b3dc3-117">Jag har lagt till alla proxybilder själv, men generera gärna dina egna!</span><span class="sxs-lookup"><span data-stu-id="b3dc3-117">I've provided all the proxy images myself, but feel free to generate your own!</span></span>

<span data-ttu-id="b3dc3-118">För varje emoji i mappen `bin/proxy-images` tar du en bild av dig själv när du imiterar en emoji och ersätter bilden med din bild.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-118">For each emoji in the `bin/proxy-images` folder, take a picture of yourself mimicking that emoji and replace the image with your image.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="b3dc3-119">Prova</span><span class="sxs-lookup"><span data-stu-id="b3dc3-119">Try it out</span></span>

<span data-ttu-id="b3dc3-120">Nu till den roliga biten!</span><span class="sxs-lookup"><span data-stu-id="b3dc3-120">Now comes the fun part!</span></span> <span data-ttu-id="b3dc3-121">Vi ska köra varje bild i `bin/proxy-images` via ansikts-API:t för att beräkna det känslomässiga uttrycket för den emojin _i det känslomässiga utrymmet_ och kör:</span><span class="sxs-lookup"><span data-stu-id="b3dc3-121">We are going to run each of the images in the `bin/proxy-images` through the Face API to calculate an emotional point for that emoji in _emotional space_, run:</span></span>

```bash
node bin/calibrate.js
```

<span data-ttu-id="b3dc3-122">Kommandots utdata bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="b3dc3-122">The output of this command should look something like so:</span></span>

```json
...
Processing 🤓
Processing 🤔
Processing 🦄
Processing 😃
Processing 😆
...
{
    emotiveValues: new EmotivePoint({
        anger: 0,
        contempt: 0.005,
        disgust: 0.001,
        fear: 0,
        happiness: 0,
        neutral: 0.922,
        sadness: 0.071,
        surprise: 0
    }),
    emojiIcon: "😴"
}
```

<span data-ttu-id="b3dc3-123">Först skrivs de emojis ut som bearbetas och slutligen skrivs en matris ut till konsolen som definierar `EmotivePoint` för alla emojis.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-123">It will first print out the emoji's it is processing and then finally print out to the console an array which defines the `EmotivePoint` of all the emoji's.</span></span> <span data-ttu-id="b3dc3-124">Det här är samma format som matrisen i `shared/mojis.ts`.</span><span class="sxs-lookup"><span data-stu-id="b3dc3-124">This is the same format as the array in `shared/mojis.ts`.</span></span>

<span data-ttu-id="b3dc3-125">Om du ändrade några av proxybilderna kopierar du sedan skriptets utdata till relevant del av `mojis.ts`</span><span class="sxs-lookup"><span data-stu-id="b3dc3-125">If you changed some of the proxied images then copy the output of this script to the relevant section of `mojis.ts`</span></span>
