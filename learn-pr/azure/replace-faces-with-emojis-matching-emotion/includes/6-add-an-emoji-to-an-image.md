Vid det här läget i programflödet vet vi nu:

1.  Listan med ansikten i bilden (om det finns några).
2.  Vilken emoji vi ska använda för varje ansikte.
3.  Den omgivande rektangeln för varje ansikte i bilden.

Så för varje ansikte som identifierats i bilden måste vi lägga till en emoji över ansiktet, och ändra storlek på den så att den passar.

För att implementera den här funktionen använder vi bildmanipuleringsbiblioteket [Jimp](https://www.npmjs.com/package/jimp) (öppen källkod).

Målet med den här lektionen är att du ska lära dig hur du använder biblioteket `Jimp` till att manipulera bilder, och i synnerhet hur du lägger en emoji över ett ansikte och sedan sparar bilden på disk igen.

Vi kommer att utöka skriptet `bin/mojify.ts` som vi påbörjade i föregående lektion genom att fylla ut funktionen `createMojifiedImage`.

> Obs! Vi kommer återanvända all kod från det här skriptet när vi skapar Slack-kommandot och Azure-funktionen i senare föreläsningar. Så du gör inte det här i onödan!

## <a name="steps"></a>Steg

### <a name="required-imports"></a>Nödvändiga importer

För att kunna experimentera med Jimp och redigera filer i filsystemet måste vi importera några paket längst upp

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> Obs! `path` behövs eftersom vi vill läsa in filer från disken

### <a name="basic-use-case"></a>Enkelt användningsfall

Vi tar bort mycket av komplexiteten och frågar oss vad vi behöver göra för att:

1. Läsa in en bild
2. Placera emojin 😕 i det övre högra hörnet (storleksändrad till 50 x 50 bildpunkter)
3. Spara bilden

Vi kan implementera alla funktioner ovan med bara 6 kodrader, så här:

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

Vi tar det steg för steg.

För att läsa in en bild med `Jimp` använder vi funktionen `Jimp.read`, så här:

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

Vi har en katalog med png-filer för varje emoji i `shared/emojis`. Varje emoji-png heter har namnet <emoji>.png, så `😕.png` är en fil som innehåller en png av emojin 😕.

Vi läser in `😕.png` så här:

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

Sedan måste vi ändra storlek på emojiImage till 50 x 50 bildpunkter, och det kan vi göra så här med funktionen resize:

```typescript
emojiImage.resize(50, 50);
```

Vår `emojiImage` har nu storleksändrats så att den passar i ett utrymme som är 50 x 50 bildpunkter stort.

Nu måste vi _placera ut_ emojiImage över sourceImage i det övre vänstra hörnet, så här:

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

Vi använder funktionen `composite` som placerar `emojiImage` ovanpå `sourceImage`, 0 bildpunkter ned och 0 bildpunkter till höger. Funktionen `composite` påverkar inte `sourceImage` i sig utan returnerar i stället en kopia av `sourceImage` med `emojiImage` placerad ovanpå.

Slutligen sparar vi utdatabilden på disk så här:

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

### <a name="full-use-case"></a>Fullständigt användningsfall

Nu har du förhoppningsvis god förståelse för hur `Jimp` fungerar och hur vi kan använda det till att slå ihop bilder. När vi nu går igenom hela koden för funktionen `createMojifiedImage` förstår du den säkert bättre.

Kopiera och klistra in koden nedan i funktionen `createMojifiedImage` i `bin/mojify.ts`.

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

Koden ovan är mycket lik basfallet vi precis har gått igenom, men i stället för att hårdkoda en emoji och dess position avgör vi vilken emoji som ska användas och var den ska placeras baserat på matrisen med ansikten vi får som indata.

Matrisen med ansikten kommer från funktionen `getFaces` som vi byggde ut i den förra lektionen. Allt hänger ihop uppe i main-funktionen, så här:

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

Vi anropar `getFaces` med den `imageUrl` vi fått så att vi genererar en matris med `Face`-instanser.
Vi skickar den här matrisen till funktionen `createMojifiedImage` tillsammans med den ursprungliga bilden. Den här funktionen sätter ihop emojis med folks ansikten och sparar den resulterande filen som `mojified.jpg` i rotmappen för projektet.

### <a name="try-it-out"></a>Prova

Prova den här koden själv, så här:

```bash
node bin/mojify.js <url>
```

Om allt fungerar ska du ha en mojifierad version av ursprungsfilen med namnet `mojified.jpg` i projektroten.

Prova med några olika bilder!
