I det senaste kapitlet lärde vi oss att filen `shared/mojis.ts` har en lista med emojis och deras känslomässiga uttryck.

I det här kapitlet går vi igenom resten av koden som vi kommer använda för att mappa ett ansikte till en emoji.

Du får lära dig att:

1. Skapa ett skript där du skickar en URL till en ansiktsbild som indata.
2. Utvärdera det känslomässiga uttrycket hos ansikten i bilden.
3. Mappa ansiktena i bilden till den emoji som ligger närmast

Så småningom kommer vi att implementera den här funktionen som ett Slack-kommando, men nu ska vi börja med ett enkelt node-skript som du kan köra på kommandoraden, till exempel så här:

```bash
node bin/mojify.js <url-of-image-with-face>
```

## <a name="debugging-typescript"></a>Felsöka TypeScript

Vi skriver i TypeScript men kör JavaScript. Det här gör det svårt med felsökningen eftersom vi behöver ange brytpunkter och felsöka i de transpilerade JavaScript-filerna som kan vara svåra att läsa.

Vad vi helst vill är att skriva _och_ felsöka i TypeScript.

Den goda nyheten är att vi kan göra det i vs-kod med en gnutta konfigurering.

Öppna filen `launch.json` med kommandopaletten `Ctrl+P > Debug: Open launch.json`

> TODO: Bild

Lägg till ett konfigurationsalternativ så här:

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

Nu ser du alternativet `Mojify` i felsökningsmenyn som kör mojify-skriptet och skickar en URL som argument. Du kan ange brytpunkter i TypeScript-filen och inspektera variabler direkt därifrån.

## <a name="open-up-binmojists"></a>Öppna `bin/mojis.ts`

Filen är redan är ifylld med alla nödvändiga importer:

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

`dotenv` är en hjälpkomponent som läser in innehållet i en .env-fil till projektroten som miljövariabler, vilket är användbart vid utveckling.

`node-fetch` används till att skicka http-förfrågningar till API:t för ansiktsigenkänning i Azure.

`EmotivePoint` är en hjälpklass som beskriver en punkt i _det känslomässiga utrymmet_ – vi kommer att diskutera detta mer detaljerat nedan.

`Rect` är en hjälpklass som beskriver en rektangulär form, vi använder den till att beskriva ett ansiktes position i en bild.

`Face` är en hjälpklass där rektangel- och känsloinformationen om ett ansikte i en bild kombineras.

Du bör se några funktionsskelett i mitten av filen:

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

Det är dessa funktioner du kommer att bygga ut i den här lektionen och nästa

Mot slutet av filen bör du se följande kod:

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

## <a name="add-the-environment-variables"></a>Lägg till miljövariablerna

Vi ska anropa API:t för ansiktsigenkänning, så vi måste använda de hemliga nycklar och webbadresser vi genererade tidigare. Lägg till dem överst i filen under importerna:

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="call-the-face-api-with-the-provided-image-and-get-a-response"></a>Anropa API:t för ansiktsigenkänning med den tillhandahållna bilden och få svar

För att göra förfrågningar till API:t för ansiktsigenkänning lägger vi till följande kod längst upp i funktionen `getFaces`:

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

I koden ovan används kommandot `fetch` till att skicka en `POST`-förfrågan till API:t för ansiktsigenkänning.

Vi skickar `API_KEY` i rubriken så att API:t för ansiktsigenkänning vet att förfrågningen kommer från oss, annars avvisas den.

Vi skickar den `imageUrl` vi vill att API:t för ansiktsigenkänning ska analysera i brödtexten.

Vi får sedan svaret från förfrågningen och skriver ut det.

Om du nu kör skriptet med

```bash
node bin/mojify.js <path-to-image>
```

Så ska json-svaret som returnerades från bildanalysen skrivas ut.

## <a name="parse-the-responce"></a>Parsa svaret

I emojiberäkningen måste vi konvertera de ansikten som returnerades i API-svaret till en instans av klassen `Face`.

Vi lägger till den här koden direkt efter koden för API-anropet i funktionen `getFaces`:

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

- Vi itererar över alla ansikten som returneras i svaret.
- Vi genererar instanser av `EmotivePoint`, `Rect` och `Face` från json-svaret.
- När vi skapar `Face`-instansen matchas ansiktet mot en emoji.
- För att se vilken emoji som matchades skriver vi ut en `mojiicon`.

## <a name="try-it-out"></a>Prova

Om du kör skriptet nu bör det göra följande:

- Skicka den angivna bilden till API:t för ansiktsigenkänning och avgöra de känslouttryck som visas.
- Matcha känslouttryck mot en emojis.
- Skriva ut emojisarna i terminalen.

Så här:

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
