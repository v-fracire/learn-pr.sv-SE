I flödet av programmet vet vi nu:

1.  Lista över ansikten i bild (om sådan finns).
2.  Emoji att använda för varje ansikte.
3.  Den omgivande rektangeln för varje ansikte i bilden.

För varje ansikte som identifieras i avbildningen måste vi även lägga till en emoji över ansiktet, storleksändring emoji så att de passar ansiktet.

För att implementera den här funktionen, använder vi manipulering för öppen källkod bildbibliotek [Jimp](https://www.npmjs.com/package/jimp).

Målet med den här föreläsning är att lära dig hur du använder den `Jimp` bibliotek att manipulera avbildningar och specifikt för att lära dig hur olika lager med en emoji över ett ansikte och spara avbildningen tillbaka till disk.

Vi kommer att expandera den `bin/mojify.ts` skript som vi påbörjade i föregående föreläsning som lägger till mer detaljerad ut den `createMojifiedImage` funktion.

> **Obs!**
>
> Vi använder all kod från det här skriptet igen när vi skapar kommandot Slack och Azure-funktion i senare föreläsningar. Det har inte gått arbete!

## <a name="add-the-required-imports"></a>Lägg till nödvändiga importer

Om du vill experimentera med Jimp och redigera filer på vår filsystem, vi måste du importera några paket överst i filen, t.ex:

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> **Obs!**
>
> `path` är nödvändigt eftersom vi vill läsa in filer från disken

## <a name="simplified-use-case"></a>Förenklad användningsfall

Nu ska vi ta bort mycket av komplexiteten och be vad det tar för att just:

1. Ladda upp en bild
2. Plats i 😕 emoji i det övre högra hörnet (ändra storlek till 50x50px)
3. Spara bilden

Vi kan implementera alla funktioner som ovan i 6 kodrader, t.ex:

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

Vi mer steg för steg.

Att läsa in en avbildning med hjälp av `Jimp` vi använder den `Jimp.read` funktion, t.ex:

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

Vi har en katalog med png-filer för varje emoji i `shared/emojis`. Varje emoji png heter som <emoji>.png, så `😕.png` är en fil som innehåller en png av den 😕 emoji.

Vi läser in `😕.png` t.ex:

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

Därefter upp måste vi ändra storlek på den `emojiImage` till 50 bildpunkter bredd x 50 bildpunkter höjd, kan vi göra det med hjälp av funktionen för storleksändring t.ex:

```typescript
emojiImage.resize(50, 50);
```

Den `emojiImage` har nu ändrats så att de passar med 50 x 50 bildpunkter blanksteg.

Vi måste nu _placera_ den `emojiImage` över den `sourceImage` i det övre vänstra hörnet, t.ex:

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

Vi använder den `composite` som placerar `emojiImage` ovanpå `sourceImage`, de sista två argumenten definiera var den `emojiImage` är placerade, vi placerar det 0 pixlar från de översta och 0 bildpunkterna ned.

Den `composite` funktionen påverkar inte `sourceImage` på plats, i stället returnerar en kopia av `sourceImage` med den `emojiImage` placeras högst upp, det är därför vi tilldela resultatet tillbaka till sourceImage `sourceImage = ...`

Slutligen vi spara utdata-avbildningen till disk som detta:

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

## <a name="try-it-out"></a>Prova

Testa den här koden själv, t.ex:

```bash
node bin/mojify.js [url-to-image]
```

Om detta arbetat och sedan en fil sparas i projektroten kallas `mojified.jpg` söker ungefär så här:

![Enkel avbildning](/media-drafts/6.simple-mojified-image.jpg)

## <a name="full-use-case"></a>Fullständig användningsfall

Förhoppningsvis kan nu du har en god förståelse av hur `Jimp` fungerar och hur vi kan använda den för att sammansatta bilder. Nu när vi går igenom den fullständiga koden för den `createMojifiedImage` funktion, bör det vara mycket bättre.

Kopiera och klistra in den nedan kod till din `createMojifiedImage` fungera i `bin/mojify.ts`.

```typescript
async function createMojifiedImage(imageUrl) {
  let sourceImage = await Jimp.read(imageUrl);
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

Koden ovan är mycket lik förenklad fallet vi precis har gått igenom, i stället hardcoding en emoji och position, men vi bestämmer vilka emoji för att sammanfoga och var du ska placera utifrån en matris med ansikten skickas in.

Matris med ansikten kommer från den `getFaces` funktionen vi fleshed ut på sista föreläsning; dess alla sammankopplade upp i huvudfunktionen, t.ex:

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

Vi kallar `getFaces` med det skickade i `imageUrl` att hämta matris med `Face` instanser.
Vi skicka den här matrisen till den `createMojifiedImage` funktionen tillsammans med den ursprungliga bilden, den här funktionen sammansättningar emojis på folk ansikten och sparar den resulterande filen i rotmappen för projektet som `mojified.jpg`

## <a name="try-it-out"></a>Prova

Testa den här koden själv, t.ex:

```bash
node bin/mojify.js <url>
```

Om detta arbetat och sedan en mojified version av källfilen sparas i projektroten kallas `mojified.jpg`, t.ex:

![Komplex bild](/media-drafts/6.complex-mojified-image.jpg)

Prova med olika bilder!
