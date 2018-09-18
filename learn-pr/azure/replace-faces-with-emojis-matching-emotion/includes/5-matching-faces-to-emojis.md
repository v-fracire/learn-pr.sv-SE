<span data-ttu-id="eb73c-101">I det senaste kapitlet lärde vi oss att filen `shared/mojis.ts` har en lista med emojis och deras känslomässiga uttryck.</span><span class="sxs-lookup"><span data-stu-id="eb73c-101">In the last chapter we learnt that `shared/mojis.ts` file has a list of emojis and their emotional points.</span></span>

<span data-ttu-id="eb73c-102">I det här kapitlet går vi igenom resten av koden som vi kommer använda för att mappa ett ansikte till en emoji.</span><span class="sxs-lookup"><span data-stu-id="eb73c-102">In this chapter we will learn about the rest of the code that we will use to map a face to an emoji.</span></span>

<span data-ttu-id="eb73c-103">Du får lära dig att:</span><span class="sxs-lookup"><span data-stu-id="eb73c-103">You will learn how to:</span></span>

1. <span data-ttu-id="eb73c-104">Skapa ett skript där du skickar en URL till en ansiktsbild som indata.</span><span class="sxs-lookup"><span data-stu-id="eb73c-104">Create a script where you pass in a URL of an image of a face.</span></span>
2. <span data-ttu-id="eb73c-105">Utvärdera det känslomässiga uttrycket hos ansikten i bilden.</span><span class="sxs-lookup"><span data-stu-id="eb73c-105">Calculate the emotional point of any faces in the image.</span></span>
3. <span data-ttu-id="eb73c-106">Mappa ansiktena i bilden till den emoji som ligger närmast</span><span class="sxs-lookup"><span data-stu-id="eb73c-106">Map the faces in the image to the closest emojis</span></span>

<span data-ttu-id="eb73c-107">Så småningom kommer vi att implementera den här funktionen som ett Slack-kommando, men nu ska vi börja med ett enkelt node-skript som du kan köra på kommandoraden, till exempel så här:</span><span class="sxs-lookup"><span data-stu-id="eb73c-107">Eventually we will be implementing this functionally as a Slack command, but for now we are going to start with a simple node script that you can run on the comand line, like so:</span></span>

```bash
node bin/mojify.js <url-of-image-with-face>
```

## <a name="debugging-typescript"></a><span data-ttu-id="eb73c-108">Felsöka TypeScript</span><span class="sxs-lookup"><span data-stu-id="eb73c-108">Debugging TypeScript</span></span>

<span data-ttu-id="eb73c-109">Vi skriver i TypeScript men kör JavaScript.</span><span class="sxs-lookup"><span data-stu-id="eb73c-109">We are writing in TypeScript but executing JavaScript.</span></span> <span data-ttu-id="eb73c-110">Det här gör det svårt med felsökningen eftersom vi behöver ange brytpunkter och felsöka i de transpilerade JavaScript-filerna som kan vara svåra att läsa.</span><span class="sxs-lookup"><span data-stu-id="eb73c-110">This makes debugging hard as we would have to set breakpoints and debug in the transpiled JavaScript files which can be hard to read.</span></span>

<span data-ttu-id="eb73c-111">Vad vi helst vill är att skriva _och_ felsöka i TypeScript.</span><span class="sxs-lookup"><span data-stu-id="eb73c-111">What we ideally want is to write _and_ debug in TypeScript.</span></span>

<span data-ttu-id="eb73c-112">Den goda nyheten är att vi kan göra det i vs-kod med en gnutta konfigurering.</span><span class="sxs-lookup"><span data-stu-id="eb73c-112">The good news is that it's possible with vs code with just a little bit of configuration.</span></span>

<span data-ttu-id="eb73c-113">Öppna filen `launch.json` med kommandopaletten `Ctrl+P > Debug: Open launch.json`</span><span class="sxs-lookup"><span data-stu-id="eb73c-113">Open up the `launch.json` file by using the command paletee `Ctrl+P > Debug: Open launch.json`</span></span>

> <span data-ttu-id="eb73c-114">TODO: Bild</span><span class="sxs-lookup"><span data-stu-id="eb73c-114">TODO: Image</span></span>

<span data-ttu-id="eb73c-115">Lägg till ett konfigurationsalternativ så här:</span><span class="sxs-lookup"><span data-stu-id="eb73c-115">Add in a configuration option like so:</span></span>

```json
{
  "type": "node",
  "request": "launch",
  "name": "Mojify",
  "env": {
    "DEBUG": "*"
  },
  "args": [
    "https://pbs.twmedia-drafts.com/profile_images/833970306339446784/83MO53R9_400x400.jpg"
  ],
  "sourceMaps": true,
  "stopOnEntry": false,
  "console": "integratedTerminal",
  "cwd": "${workspaceRoot}",
  "program": "${workspaceRoot}/bin/mojify.js",
  "outFiles": ["${workspaceRoot}/bin/mojify.js"]
}
```

<span data-ttu-id="eb73c-116">Nu ser du alternativet `Mojify` i felsökningsmenyn som kör mojify-skriptet och skickar en URL som argument.</span><span class="sxs-lookup"><span data-stu-id="eb73c-116">Now in the debug menu you will see an option called `Mojify` this will run the mojify script passing in a URL as the argument.</span></span> <span data-ttu-id="eb73c-117">Du kan ange brytpunkter i TypeScript-filen och inspektera variabler direkt därifrån.</span><span class="sxs-lookup"><span data-stu-id="eb73c-117">You will be able to set breakpoints in the TypeScript file and inspect variables directly from there.</span></span>

## <a name="open-up-binmojists"></a><span data-ttu-id="eb73c-118">Öppna `bin/mojis.ts`</span><span class="sxs-lookup"><span data-stu-id="eb73c-118">Open up `bin/mojis.ts`</span></span>

<span data-ttu-id="eb73c-119">Filen är redan är ifylld med alla nödvändiga importer:</span><span class="sxs-lookup"><span data-stu-id="eb73c-119">The file is already scaffolded with all the required imports, like so:</span></span>

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

<span data-ttu-id="eb73c-120">`dotenv` är en hjälpkomponent som läser in innehållet i en .env-fil till projektroten som miljövariabler, vilket är användbart vid utveckling.</span><span class="sxs-lookup"><span data-stu-id="eb73c-120">`dotenv` is a helper package which loads up the contents of a .env file in the root of your project as environment variables, useful for development.</span></span>

<span data-ttu-id="eb73c-121">`node-fetch` används till att skicka http-förfrågningar till API:t för ansiktsigenkänning i Azure.</span><span class="sxs-lookup"><span data-stu-id="eb73c-121">`node-fetch` we use to make http requests to the Azure Face API.</span></span>

<span data-ttu-id="eb73c-122">`EmotivePoint` är en hjälpklass som beskriver en punkt i _det känslomässiga utrymmet_ – vi kommer att diskutera detta mer detaljerat nedan.</span><span class="sxs-lookup"><span data-stu-id="eb73c-122">`EmotivePoint` is a helper class that describes a point in _emotional space_ - we will be discussing this in more details below.</span></span>

<span data-ttu-id="eb73c-123">`Rect` är en hjälpklass som beskriver en rektangulär form, vi använder den till att beskriva ett ansiktes position i en bild.</span><span class="sxs-lookup"><span data-stu-id="eb73c-123">`Rect` is a helper class to describe a rectangle shape, we use this to describe the position of a face in an image.</span></span>

<span data-ttu-id="eb73c-124">`Face` är en hjälpklass där rektangel- och känsloinformationen om ett ansikte i en bild kombineras.</span><span class="sxs-lookup"><span data-stu-id="eb73c-124">`Face` is a helper utility class which combines the rectangle and emotive point informatoin about a face in an image.</span></span>

<span data-ttu-id="eb73c-125">Du bör se några funktionsskelett i mitten av filen:</span><span class="sxs-lookup"><span data-stu-id="eb73c-125">In the middle of the file you should see some stub functions, like so:</span></span>

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

<span data-ttu-id="eb73c-126">Det är dessa funktioner du kommer att bygga ut i den här lektionen och nästa</span><span class="sxs-lookup"><span data-stu-id="eb73c-126">These are the functions you will be fleshing out in this lecture and the next</span></span>

<span data-ttu-id="eb73c-127">Mot slutet av filen bör du se följande kod:</span><span class="sxs-lookup"><span data-stu-id="eb73c-127">At the end of the file you should see this code</span></span>

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

## <a name="add-the-environment-variables"></a><span data-ttu-id="eb73c-128">Lägg till miljövariablerna</span><span class="sxs-lookup"><span data-stu-id="eb73c-128">Add the environment variables</span></span>

<span data-ttu-id="eb73c-129">Vi ska anropa API:t för ansiktsigenkänning, så vi måste använda de hemliga nycklar och webbadresser vi genererade tidigare. Lägg till dem överst i filen under importerna:</span><span class="sxs-lookup"><span data-stu-id="eb73c-129">We are going to call the Face API so we need to use those secret keys and urls we generated before, add this to the top of the file under the imports:</span></span>

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="call-the-face-api-with-the-provided-image-and-get-a-response"></a><span data-ttu-id="eb73c-130">Anropa API:t för ansiktsigenkänning med den tillhandahållna bilden och få svar</span><span class="sxs-lookup"><span data-stu-id="eb73c-130">Call the Face API with the provided image and get a response</span></span>

<span data-ttu-id="eb73c-131">För att göra förfrågningar till API:t för ansiktsigenkänning lägger vi till följande kod längst upp i funktionen `getFaces`:</span><span class="sxs-lookup"><span data-stu-id="eb73c-131">To make a reques to the Face API we add this code to the top of the `getFaces` function</span></span>

```typescript
let response = await fetch(API_URL, {
  headers: {
    "Ocp-Apim-Subscription-Key": API_KEY,
    "Content-Type": "application/json"
  },
  method: "POST",
  body: JSON.stringify({ url: imageUrl })
});
let resp = await response.json();
console.log(resp);
```

<span data-ttu-id="eb73c-132">I koden ovan används kommandot `fetch` till att skicka en `POST`-förfrågan till API:t för ansiktsigenkänning.</span><span class="sxs-lookup"><span data-stu-id="eb73c-132">The code above uses the `fetch` command to send a `POST` request to the Face API.</span></span>

<span data-ttu-id="eb73c-133">Vi skickar `API_KEY` i rubriken så att API:t för ansiktsigenkänning vet att förfrågningen kommer från oss, annars avvisas den.</span><span class="sxs-lookup"><span data-stu-id="eb73c-133">We pass the `API_KEY` in the header so the Face API knows the request comes from us, otherwise the request is rejected.</span></span>

<span data-ttu-id="eb73c-134">Vi skickar den `imageUrl` vi vill att API:t för ansiktsigenkänning ska analysera i brödtexten.</span><span class="sxs-lookup"><span data-stu-id="eb73c-134">We pass the `imageUrl` we want the Face API to analyse in the body.</span></span>

<span data-ttu-id="eb73c-135">Vi får sedan svaret från förfrågningen och skriver ut det.</span><span class="sxs-lookup"><span data-stu-id="eb73c-135">We then get the responce from the API request and print it out.</span></span>

<span data-ttu-id="eb73c-136">Om du nu kör skriptet med</span><span class="sxs-lookup"><span data-stu-id="eb73c-136">If you now run the script with</span></span>

```bash
node bin/mojify.js <path-to-image>
```

<span data-ttu-id="eb73c-137">Så ska json-svaret som returnerades från bildanalysen skrivas ut.</span><span class="sxs-lookup"><span data-stu-id="eb73c-137">It should print out the json responce returned from passing that image to the face API.</span></span>

## <a name="parse-the-responce"></a><span data-ttu-id="eb73c-138">Parsa svaret</span><span class="sxs-lookup"><span data-stu-id="eb73c-138">Parse the responce</span></span>

<span data-ttu-id="eb73c-139">I emojiberäkningen måste vi konvertera de ansikten som returnerades i API-svaret till en instans av klassen `Face`.</span><span class="sxs-lookup"><span data-stu-id="eb73c-139">To calculate the emojis ee need to convert each face returned in the responce from the API to an instance of a `Face` class.</span></span>

<span data-ttu-id="eb73c-140">Vi lägger till den här koden direkt efter koden för API-anropet i funktionen `getFaces`:</span><span class="sxs-lookup"><span data-stu-id="eb73c-140">We add this code just after the code to call the API in the `getFaces` fucntion:</span></span>

```typescript
let faces = [];
for (let f of resp) {
  let scores = new EmotivePoint(f.faceAttributes.emotion);
  let faceRectangle = new Rect(f.faceRectangle);
  let face = new Face(scores, faceRectangle);
  console.log(face.mojiIcon);
  faces.push(face);
}
return faces;
```

- <span data-ttu-id="eb73c-141">Vi itererar över alla ansikten som returneras i svaret.</span><span class="sxs-lookup"><span data-stu-id="eb73c-141">We loop through each face returned in the responce.</span></span>
- <span data-ttu-id="eb73c-142">Vi genererar instanser av `EmotivePoint`, `Rect` och `Face` från json-svaret.</span><span class="sxs-lookup"><span data-stu-id="eb73c-142">We generate an `EmotivePoint`, a `Rect` and a `Face` from the returned json.</span></span>
- <span data-ttu-id="eb73c-143">När vi skapar `Face`-instansen matchas ansiktet mot en emoji.</span><span class="sxs-lookup"><span data-stu-id="eb73c-143">Creating the `Face` instance matches the face to an emoji</span></span>
- <span data-ttu-id="eb73c-144">För att se vilken emoji som matchades skriver vi ut en `mojiicon`.</span><span class="sxs-lookup"><span data-stu-id="eb73c-144">To see which emoji was matched we print out the `mojiicon`.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="eb73c-145">Prova</span><span class="sxs-lookup"><span data-stu-id="eb73c-145">Try it out</span></span>

<span data-ttu-id="eb73c-146">Om du kör skriptet nu bör det göra följande:</span><span class="sxs-lookup"><span data-stu-id="eb73c-146">Now if you run the script it should:</span></span>

- <span data-ttu-id="eb73c-147">Skicka den angivna bilden till API:t för ansiktsigenkänning och avgöra de känslouttryck som visas.</span><span class="sxs-lookup"><span data-stu-id="eb73c-147">Pass the provided image through the Face API and calculate the emotion.</span></span>
- <span data-ttu-id="eb73c-148">Matcha känslouttryck mot en emojis.</span><span class="sxs-lookup"><span data-stu-id="eb73c-148">Match emotions to emojis.</span></span>
- <span data-ttu-id="eb73c-149">Skriva ut emojisarna i terminalen.</span><span class="sxs-lookup"><span data-stu-id="eb73c-149">Print the emojis to the terminal.</span></span>

<span data-ttu-id="eb73c-150">Så här:</span><span class="sxs-lookup"><span data-stu-id="eb73c-150">Like so:</span></span>

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
