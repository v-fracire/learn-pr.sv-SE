<span data-ttu-id="2003e-101">Vid det här läget i programflödet vet vi nu:</span><span class="sxs-lookup"><span data-stu-id="2003e-101">At this point in the flow of the application we know:</span></span>

1.  <span data-ttu-id="2003e-102">Listan med ansikten i bilden (om det finns några).</span><span class="sxs-lookup"><span data-stu-id="2003e-102">The list of faces in the image (if any).</span></span>
2.  <span data-ttu-id="2003e-103">Vilken emoji vi ska använda för varje ansikte.</span><span class="sxs-lookup"><span data-stu-id="2003e-103">The emoji to use for each face.</span></span>
3.  <span data-ttu-id="2003e-104">Den omgivande rektangeln för varje ansikte i bilden.</span><span class="sxs-lookup"><span data-stu-id="2003e-104">The bounding rectangle of each face in the image.</span></span>

<span data-ttu-id="2003e-105">Så för varje ansikte som identifierats i bilden måste vi lägga till en emoji över ansiktet, och ändra storlek på den så att den passar.</span><span class="sxs-lookup"><span data-stu-id="2003e-105">So for each face discovered in the image, we need to layer an emoji over the face, resizing the emoji to fit the face.</span></span>

<span data-ttu-id="2003e-106">För att implementera den här funktionen använder vi bildmanipuleringsbiblioteket [Jimp](https://www.npmjs.com/package/jimp) (öppen källkod).</span><span class="sxs-lookup"><span data-stu-id="2003e-106">To implement this functionality, we will use the open source image manipulation library [Jimp](https://www.npmjs.com/package/jimp).</span></span>

<span data-ttu-id="2003e-107">Målet med den här lektionen är att du ska lära dig hur du använder biblioteket `Jimp` till att manipulera bilder, och i synnerhet hur du lägger en emoji över ett ansikte och sedan sparar bilden på disk igen.</span><span class="sxs-lookup"><span data-stu-id="2003e-107">The goal of this lecture is to learn how to use the `Jimp` library to manipulate images and specifically to learn how to layer an emoji over a face and save that image back out to disk.</span></span>

<span data-ttu-id="2003e-108">Vi kommer att utöka skriptet `bin/mojify.ts` som vi påbörjade i föregående lektion genom att fylla ut funktionen `createMojifiedImage`.</span><span class="sxs-lookup"><span data-stu-id="2003e-108">We are going to expand on the `bin/mojify.ts` script we started in the previous lecture by fleshing out the `createMojifiedImage` function.</span></span>

> <span data-ttu-id="2003e-109">Obs! Vi kommer återanvända all kod från det här skriptet när vi skapar Slack-kommandot och Azure-funktionen i senare föreläsningar.</span><span class="sxs-lookup"><span data-stu-id="2003e-109">NOTE We will be re-using all the code from this script when we create the Slack command and Azure Function in later lectures.</span></span> <span data-ttu-id="2003e-110">Så du gör inte det här i onödan!</span><span class="sxs-lookup"><span data-stu-id="2003e-110">It's not wasted effort!</span></span>

## <a name="steps"></a><span data-ttu-id="2003e-111">Steg</span><span class="sxs-lookup"><span data-stu-id="2003e-111">Steps</span></span>

### <a name="required-imports"></a><span data-ttu-id="2003e-112">Nödvändiga importer</span><span class="sxs-lookup"><span data-stu-id="2003e-112">Required Imports</span></span>

<span data-ttu-id="2003e-113">För att kunna experimentera med Jimp och redigera filer i filsystemet måste vi importera några paket längst upp</span><span class="sxs-lookup"><span data-stu-id="2003e-113">To play around with Jimp and manipulate files on our filesystem we need to import a few packages at the top</span></span>

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> <span data-ttu-id="2003e-114">Obs! `path` behövs eftersom vi vill läsa in filer från disken</span><span class="sxs-lookup"><span data-stu-id="2003e-114">NOTE `path` is needed because we want to load files from disk</span></span>

### <a name="basic-use-case"></a><span data-ttu-id="2003e-115">Enkelt användningsfall</span><span class="sxs-lookup"><span data-stu-id="2003e-115">Basic Use Case</span></span>

<span data-ttu-id="2003e-116">Vi tar bort mycket av komplexiteten och frågar oss vad vi behöver göra för att:</span><span class="sxs-lookup"><span data-stu-id="2003e-116">Let's strip away a lot of the complexity and ask what it would take to:</span></span>

1. <span data-ttu-id="2003e-117">Läsa in en bild</span><span class="sxs-lookup"><span data-stu-id="2003e-117">Load up an image</span></span>
2. <span data-ttu-id="2003e-118">Placera emojin 😕 i det övre högra hörnet (storleksändrad till 50 x 50 bildpunkter)</span><span class="sxs-lookup"><span data-stu-id="2003e-118">Place the 😕 emoji in the top right corner (resized to 50x50px)</span></span>
3. <span data-ttu-id="2003e-119">Spara bilden</span><span class="sxs-lookup"><span data-stu-id="2003e-119">Save the image</span></span>

<span data-ttu-id="2003e-120">Vi kan implementera alla funktioner ovan med bara 6 kodrader, så här:</span><span class="sxs-lookup"><span data-stu-id="2003e-120">We can implement all the functionality above in 6 lines of code, like so:</span></span>

```typescript
async function createMojifiedImage(imageUrl) {
  let sourceImage = await Jimp.read(imageUrl);

  let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
  let emojiImage = await Jimp.read(mojiPath);

  emojiImage.resize(50, 50);

  sourceImage = sourceImage.composite(emojiImage, 0, 0);
  sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
}
```

<span data-ttu-id="2003e-121">Vi tar det steg för steg.</span><span class="sxs-lookup"><span data-stu-id="2003e-121">We'll break it down step by step.</span></span>

<span data-ttu-id="2003e-122">För att läsa in en bild med `Jimp` använder vi funktionen `Jimp.read`, så här:</span><span class="sxs-lookup"><span data-stu-id="2003e-122">To load an image using `Jimp` we use the `Jimp.read` function, like so:</span></span>

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

<span data-ttu-id="2003e-123">Vi har en katalog med png-filer för varje emoji i `shared/emojis`.</span><span class="sxs-lookup"><span data-stu-id="2003e-123">We have a directory of png files for each emoji in `shared/emojis`.</span></span> <span data-ttu-id="2003e-124">Varje emoji-png heter har namnet <emoji>.png, så `😕.png` är en fil som innehåller en png av emojin 😕.</span><span class="sxs-lookup"><span data-stu-id="2003e-124">Each emoji png is named as <emoji>.png, so `😕.png` is a file that contains a png of the 😕 emoji.</span></span>

<span data-ttu-id="2003e-125">Vi läser in `😕.png` så här:</span><span class="sxs-lookup"><span data-stu-id="2003e-125">We load up `😕.png` like so:</span></span>

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

<span data-ttu-id="2003e-126">Sedan måste vi ändra storlek på emojiImage till 50 x 50 bildpunkter, och det kan vi göra så här med funktionen resize:</span><span class="sxs-lookup"><span data-stu-id="2003e-126">Next up we need to resize the emojiImage to 50 pixels width x 50 pixels height, we can do that by using the resize function like so:</span></span>

```typescript
emojiImage.resize(50, 50);
```

<span data-ttu-id="2003e-127">Vår `emojiImage` har nu storleksändrats så att den passar i ett utrymme som är 50 x 50 bildpunkter stort.</span><span class="sxs-lookup"><span data-stu-id="2003e-127">The `emojiImage` has now been resized to fit in a 50x50 px space.</span></span>

<span data-ttu-id="2003e-128">Nu måste vi _placera ut_ emojiImage över sourceImage i det övre vänstra hörnet, så här:</span><span class="sxs-lookup"><span data-stu-id="2003e-128">We now need to _place_ the emojiImage over the sourceImage in the top left corner, like so:</span></span>

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

<span data-ttu-id="2003e-129">Vi använder funktionen `composite` som placerar `emojiImage` ovanpå `sourceImage`, 0 bildpunkter ned och 0 bildpunkter till höger.</span><span class="sxs-lookup"><span data-stu-id="2003e-129">We use the `composite` function, which places `emojiImage` ontop of `sourceImage` 0 pixels from the top and 0 pixels down.</span></span> <span data-ttu-id="2003e-130">Funktionen `composite` påverkar inte `sourceImage` i sig utan returnerar i stället en kopia av `sourceImage` med `emojiImage` placerad ovanpå.</span><span class="sxs-lookup"><span data-stu-id="2003e-130">The `composite` fucntion doesn't alter `sourceImage` in place, instead it returns a copy of `sourceImage` with the `emojiImage` placed on top.</span></span>

<span data-ttu-id="2003e-131">Slutligen sparar vi utdatabilden på disk så här:</span><span class="sxs-lookup"><span data-stu-id="2003e-131">Finally we save the output image to disk like so:</span></span>

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

### <a name="full-use-case"></a><span data-ttu-id="2003e-132">Fullständigt användningsfall</span><span class="sxs-lookup"><span data-stu-id="2003e-132">Full Use Case</span></span>

<span data-ttu-id="2003e-133">Nu har du förhoppningsvis god förståelse för hur `Jimp` fungerar och hur vi kan använda det till att slå ihop bilder.</span><span class="sxs-lookup"><span data-stu-id="2003e-133">Hopefully by now you have a good understanding of how `Jimp` works and how we can use it to composite images.</span></span> <span data-ttu-id="2003e-134">När vi nu går igenom hela koden för funktionen `createMojifiedImage` förstår du den säkert bättre.</span><span class="sxs-lookup"><span data-stu-id="2003e-134">So now when we go through the full code for the `createMojifiedImage` function it should make a lot more sense.</span></span>

<span data-ttu-id="2003e-135">Kopiera och klistra in koden nedan i funktionen `createMojifiedImage` i `bin/mojify.ts`.</span><span class="sxs-lookup"><span data-stu-id="2003e-135">Copy and paste the bellow code into your `createMojifiedImage` function in `bin/mojify.ts`.</span></span>

```typescript
async function createMojifiedImage(imageUrl) {
  // Create a composite image, we will "append" to this composite an emoji image for each face found
  let compositeImage = sourceImage;

  for (let face of faces) {
    const mojiIcon = face.mojiIcon;
    const faceHeight = face.faceRectangle.height;
    const faceWidth = face.faceRectangle.width;
    const faceTop = face.faceRectangle.top;
    const faceLeft = face.faceRectangle.left;

    // Load the emoji from disk
    let mojiPath = path.resolve(
      __dirname,
      "../shared/emojis/" + mojiIcon + ".png"
    );
    let emojiImage = await Jimp.read(mojiPath);

    // Resize the emoji to fit the face
    emojiImage.resize(faceWidth, faceHeight);

    // Compose the emoji on the image
    compositeImage = compositeImage.composite(emojiImage, faceLeft, faceTop);
  }
  compositeImage.write(path.join(__dirname, "..", "mojified.jpg"));
}
```

<span data-ttu-id="2003e-136">Koden ovan är mycket lik basfallet vi precis har gått igenom, men i stället för att hårdkoda en emoji och dess position avgör vi vilken emoji som ska användas och var den ska placeras baserat på matrisen med ansikten vi får som indata.</span><span class="sxs-lookup"><span data-stu-id="2003e-136">The above code is very similar to the base case we just went through, rather than hardcoding an emoji and poisition however we are deciding which emoji to composite and where to place it based on the array of faces passed in.</span></span>

<span data-ttu-id="2003e-137">Matrisen med ansikten kommer från funktionen `getFaces` som vi byggde ut i den förra lektionen. Allt hänger ihop uppe i main-funktionen, så här:</span><span class="sxs-lookup"><span data-stu-id="2003e-137">The array of faces comes from the `getFaces` function we fleshed out in the last lecture, it's all connected up together in the main function, like so:</span></span>

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

<span data-ttu-id="2003e-138">Vi anropar `getFaces` med den `imageUrl` vi fått så att vi genererar en matris med `Face`-instanser.</span><span class="sxs-lookup"><span data-stu-id="2003e-138">We call `getFaces` with the passed in `imageUrl` to get the array of `Face` instances.</span></span>
<span data-ttu-id="2003e-139">Vi skickar den här matrisen till funktionen `createMojifiedImage` tillsammans med den ursprungliga bilden. Den här funktionen sätter ihop emojis med folks ansikten och sparar den resulterande filen som `mojified.jpg` i rotmappen för projektet.</span><span class="sxs-lookup"><span data-stu-id="2003e-139">We pass this array to the `createMojifiedImage` function along with the original image, this function composites emojis on peoples faces and saves the resulting file to the project root folder as `mojified.jpg`</span></span>

### <a name="try-it-out"></a><span data-ttu-id="2003e-140">Prova</span><span class="sxs-lookup"><span data-stu-id="2003e-140">Try it out</span></span>

<span data-ttu-id="2003e-141">Prova den här koden själv, så här:</span><span class="sxs-lookup"><span data-stu-id="2003e-141">Try this code out yourself, like so:</span></span>

```bash
node bin/mojify.js <url>
```

<span data-ttu-id="2003e-142">Om allt fungerar ska du ha en mojifierad version av ursprungsfilen med namnet `mojified.jpg` i projektroten.</span><span class="sxs-lookup"><span data-stu-id="2003e-142">If this worked then a mojified version of the source fil should be stored in the project root called `mojified.jpg`.</span></span>

<span data-ttu-id="2003e-143">Prova med några olika bilder!</span><span class="sxs-lookup"><span data-stu-id="2003e-143">Try it out with different images!</span></span>
