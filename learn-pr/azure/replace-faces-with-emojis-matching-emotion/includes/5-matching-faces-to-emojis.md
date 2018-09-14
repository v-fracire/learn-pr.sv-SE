I det senaste kapitlet vi lärt som `shared/mojis.ts` filen har en lista över emojis och sitt känslomässig distributionsplatser.

I det här kapitlet gå vi igenom resten av koden som vi använder för att mappa ett ansikte till en emoji.

Lär dig att:

1. Skapa ett skript som där du skickar i en URL för en avbildning av ett ansikte.
2. Beräkna känslomässig punkt i alla ansikten i bild.
3. Mappa ansikten i bild till närmaste emojis

Så småningom vi implementera detta som samma funktioner som ett Slack kommando, men nu ska vi börja med en enkel nod-skript som du kan köra på kommandoraden, t.ex:

```bash
node bin/mojify.js <url-of-image-with-face>
```

## <a name="debugging-typescript-in-vs-code"></a>TypeScript i VS Code-felsökning

Vi skriver `TypeScript` men körs `JavaScript`; det här gör felsökning svårt när vi skulle behöva ange brytpunkter och felsöka i transpiled JavaScript-filer som kan vara svårt att läsa.

Vad vi helst vill är att skriva _och_ felsöka i TypeScript.

Den goda nyheten är att det är möjligt med vs code med bara lite av konfigurationen.

Öppna den `launch.json` filen med hjälp av kommandot palete <kbd>Ctrl</kbd>+<kbd>P</kbd> och de skriva `Debug: Open launch.json`

![Öppna Start-Json](/media-drafts/5.open-debug-launch.json.png)

Gör det öppnas den `launch.json` konfigurationsfilen. Om du vill lägga till och ta bort debug konfigurationer redigera vi den här filen.

Lägg till den nedan debug konfigurationsalternativet i inställningar-matrisen:

```json
{
  "type": "node",
  "request": "launch",
  "name": "Mojify",
  "env": {
    "DEBUG": "*"
  },
  "args": [
    "https://pbs.twimg.com/profile_images/833970306339446784/83MO53R9_400x400.jpg"
  ],
  "sourceMaps": true,
  "stopOnEntry": false,
  "console": "integratedTerminal",
  "cwd": "${workspaceRoot}",
  "program": "${workspaceRoot}/bin/mojify.js",
  "outFiles": ["${workspaceRoot}/bin/mojify.js"]
}
```

Nu i Felsök-menyn visas alternativet `Mojify` den här lösningen körs skriptet mojify skicka i en URL som argument. Du kan ange brytpunkter den `TypeScript` filen och inspektera variabler direkt därifrån.

## <a name="open-up-binmojists"></a>Öppna `bin/mojis.ts`

Filen är redan är Autogenererad med alla nödvändiga importer, t.ex:

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

`dotenv` är ett helper-paket som laddar upp innehållet i en .env-filen i roten av projektet som miljövariabler, användbara för utveckling.

`node-fetch` Vi använder för att göra HTTP-förfrågningar till Ansikts-API i Azure.

`EmotivePoint` är en hjälparklass som beskriver en punkt i _känslomässig utrymme_ -vi går igenom detta mer detaljerat nedan.

`Rect` är en hjälparklass att beskriva en rektangelfigur vi använder informationen för att beskriva positionen för ett ansikte i en bild.

`Face` är en hjälparklass verktyget som kombinerar rektangel och emotive punkt information om ett ansikte i en bild.

Mitt i filen bör du se vissa stub-funktioner, t.ex:

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

Det här är de funktioner som du kommer lägger till mer detaljerad ut i den här Föreläsningar och nästa.

Du bör se den här koden i slutet av filen:

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

## <a name="add-the-environment-variables"></a>Lägg till miljövariabler

Vi ska anropa Ansikts-API, så vi behöver att använda de hemliga nycklarna och URL: er som vi tidigare genererad, lägga till detta i överst i filen under importen:

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="call-the-face-api-with-the-provided-image-and-get-a-response"></a>Anropa Ansikts-API med den tillhandahållna avbildningsfilen och få svar

En begäran till Ansikts-API vi lägger du till den här koden längst upp i den `getFaces` funktion

```typescript
const fullUrl =
  API_URL +
  "/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion";

let response = await fetch(fullUrl, {
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

Koden ovan använder den `fetch` kommando för att skicka en `POST` begäran till Ansikts-API.

> **OBS!**
>
> Vi måste lägga till `/detect` och vissa fråga parametrar till API_URL för att få det att identifiera ansikten och också returnera känslor.

Vi skickar den `API_KEY` i rubriken, så vet att Ansikts-API begäran kommer från oss, annars avvisas begäran.

Vi skickar den `imageUrl` vi vill Ansikts-API för att analysera i brödtexten.

Vi kan sedan få svaret från API-begäran och skriva ut.

Om du kör skriptet med nu

```bash
node bin/mojify.js <path-to-image>
```

Det bör skriva ut JSON-svaret som returnerades från att skicka avbildningen till ansikts-API.

## <a name="parse-the-response"></a>Parsa svaret

För att beräkna emojis ee behöva konvertera varje ansikte som returnerades i svaret från API: et till en instans av en `Face` klass.

Vi lägger till den här koden efter kod för att anropa API: et den `getFaces` funktionen:

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

- Vi gå igenom varje ansikte som returnerades i svaret.
- Vi genererar en `EmotivePoint`, ett `Rect` och en `Face` från den returnerade JSON-filen.
- Skapa den `Face` instans matchar smiley för att en emoji
- För att se vilka emoji matchades vi skriva ut den `mojiicon`.

## <a name="try-it-out"></a>Prova

Om du kör skriptet den borde nu:

1. Skicka den tillhandahållna avbildningsfilen via Ansikts-API och beräkna känslo.
2. Matcha känslor till emojis.
3. Skriva ut emojis till terminalen.

T.ex:

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
