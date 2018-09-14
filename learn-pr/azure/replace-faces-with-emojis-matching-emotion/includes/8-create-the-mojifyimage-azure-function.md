Målet med den här föreläsning är att skapa en Azure-funktion HTTP-slutpunkt som mojify en överförda avbildning och returnerar sedan mojified avbildningen så att den visas på skärmen.

I slutet lär du dig:

- Så här konverterar du en `JavaScript` Azure Function-projekt till `TypeScript`.
- Hur du returnerar `raw` bild data från en slutpunkt för funktionen.

## <a name="convert-to-typescript"></a>Konvertera till TypeScript

Vi skriver kod i `TypeScript` men funktionsappen skapas en `JavaScript` filen, måste du konvertera som över till `TypeScript`.

1. Byt namn på den `index.js` filen till `index.ts`

> **OBS!**
>
> Om du kör `npm run build` och de relaterade `tsc -w` kommando kommer `index.ts` akt direkt till `index.js` och du bör även ha en `index.map` filen som skapades.
>
> Om den `index.js` och `index.map` filer skapas inte automatiskt sedan dubbelkolla den `npm run build` körs och utdata inte visar eventuella fel.

2. Klistra in den här koden i `index.ts`

```typescript
export async function index(context, req) {
  context.log("ReplyWithMojifiedImage HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

3. Kör funktionsappen

Kontrollera att funktionen kör app med någon av metoderna som beskrevs tidigare, antingen genom att använda den `func host start` kommandot eller köra programmet via Felsök-menyn.

Besök http://localhost:7171/api/MojifyImage i en webbläsare.

Om allt fungerar som den ska så detta bör skriva ut `Hello!` till webbläsaren.

Nu har konverterat funktionen utlösaren ska TypeScript istället för JavaScript 👏.

## <a name="environment-variables-in-azure-functions"></a>Miljövariabler i Azure Functions

Om du vill skydda dig, vi inte hårdkoda privata data i vår kod. i stället har vi använt miljövariabler.

Mer specifikt vi har använt miljövariabler för att lagra Ansikts-API-URL och nyckel, t.ex:

```bash
export FACE_API_URL=[face-api-url]
export FACE_API_KEY=[face-api-key]
```

När du skapar ett Azure Functions-projekt kan även skapas en fil med namnet `local.settings.json`. Den här filen finns också i den `.gitignore` filen så _isnot kontrolleras i källkontrollen_.

Det är en plats att lagra känslig eller bara lokala konfiguration som du inte vill ska publiceras.

Som standard den ser ut så här:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "node"
  }
}
```

Allt som läggs till i `Values` objekt görs tillgänglig för NodeJS koden som en miljövariabel.

Så om du har lagt till i ansikts-API-variablerna som så:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "node",
    "FACE_API_URL": "[face-api-url]",
    "FACE_API_KEY": "[face-api-key]"
  }
}
```

Sedan `FACE_API_URL` och `FACE_API_KEY` variabler blir tillgänglig för noden som miljövariabler som detta:

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="copy-existing-code-from-the-mojifyts-script"></a>Kopiera befintliga kod från den `mojify.ts` skript

Arbetet som vi har hållit i föregående Föreläsningar skapar våra `bin\mojify.ts` kod inte har gått, de flesta av koden kan nu kopieras till den här funktionen.

Kopiera allt **utom** den `main` fungerar över till den `index.ts` filen.

Det spelar ingen roll om men jag vill ha alla import och beroende funktioner överst i filen och `index` funktionen längst ned.

> **VIKTIGT**
>
> Skriv inte över den `index` funktion, det måste finnas för Azure-funktionen ska fungera.

Vi påverkas inte den `getFaces` funktionen; det helt och hållet förblir detsamma.

Men den `createMojifiedImage` funktion gör behovet av en ändring i den ursprungliga versionen av den här funktionen avbildningen sparas till disk med en rad så här i slutet av koden:

```typescript
compositeImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

I den nya Azure Function-versionen av koden som vi inte vill att spara filen på disk. Vi vill _returnera_ avbildningen till användaren och visa dem i webbläsarfönstret i stället.

Om du vill att vi behöver något som kallas en `buffer`, kan vi be `Jimp` för att ge oss bufferten för vår avbildning, men vi måste du omsluta koden i en `Promise` eftersom den `Jimp.getBuffer` funktion är asynkron.

Så ersätter ovanstående `compositeImage.write` raden med det här:

```typescript
return new Promise((resolve, reject) => {
  compositeImage.getBuffer(Jimp.MIME_JPEG, (error, buffer) => {
    // get a buffer of the composed image
    if (error) {
      let message = "There was an error adding the emoji to the image";
      context.log.error(error);
      reject(message);
    } else {
      // put the image into the context body
      resolve(buffer);
    }
  });
});
```

Det här returnerar bufferten för avbildningen, binära rådata.

Du kanske ser nu att TypeScript klagar inte känner den `context` variabeln.

Du löser problemet Lägg till `context` som det första argumentet för den `createMojifiedImage` funktion, t.ex:

```typescript
async function createMojifiedImage(context, imageUrl, faces) {
  ...
}
```

## <a name="connect-it-all-in-the-index-function"></a>Anslut den alla de `index` funktion

När någon besöker den här slutpunkten vill vi:

1. Hämta den `imageUrl` de begär
2. Mojify den `imageUrl` och få den råa bufferten.
3. Svara på HTTP-begäran så att en bild visas i webbläsaren.

Så vi börjar lägger till mer detaljerad ut funktionen index.

Den `context.res` egenskapen beskriver svaret för den här funktionen vad vi anger här är vad returneras till webbläsaren.

Nu ska vi konfigurera `context.res` t.ex:

```typescript
context.res = {
  status: 200,
  headers: {
    "Content-Type": "image/jpeg"
  },
  isRaw: true
};
```

Lägg till ovanstående längst upp i din `index` funktion, detta ställer in svaret att returnera statuskod 200, innehåll returtypen vara jpeg och anger den `isRaw` flaggan som SANT för att så att vi kan returnera binära _bild_ data.

Nu ska vi vill extrahera den `imageUrl` från frågan params så att vi vet vilken avbildning som vi ska vara mojifying. Vi vill också returnera några bra felmeddelande visas om användaren inte anger den.

```typescript
const { imageUrl } = req.query;
if (!imageUrl) {
  let message = `imageUrl is required`;
  context.log.error(message);
  return context.done(message, { status: 400, body: message });
} else {
  context.log(`Called with imageUrl: "${imageUrl}"`);
}
```

Slutligen vill vi anropa den `getFaces` och `createMojifiedImage` funktioner som vi har lagt till överst i vår fil.

```typescript
// Get the response from the faceAPI
try {
  let faces = await getFaces(imageUrl);
  let buffer = await createMojifiedImage(context, imageUrl, faces);
  context.res.body = buffer;
  context.log(`Posted reply with mojified image`);
  context.done();
} catch (err) {
  let message =
    "There was an error processing this image: " + JSON.stringify(err);
  context.log.error(message);
  context.done(null, {
    status: 400,
    body: message
  });
}
```

Viktiga två raderna ovan är:

```typescript
context.res.body = buffer;
context.done();
```

Detta anger den binära rådata som texten till vår svarsobjekt och vi anropa sedan `context.done()` så att funktionsappen vet vi är klar och den kan utföra uppgiften att returnera svaret.

Fullständig lista för funktionen index.ts är som det:

```typescript
export async function index(context, req) {
  context.log("ReplyWithMojifiedImage HTTP trigger");

  context.res = {
    status: 200,
    headers: {
      "Content-Type": "image/jpeg"
    },
    isRaw: true
  };

  const { imageUrl } = req.query;
  if (!imageUrl) {
    let message = `imageUrl is required`;
    context.log.error(message);
    return context.done(message, { status: 400, body: message });
  } else {
    context.log(`Called with imageUrl: "${imageUrl}"`);
  }

  // Get the response from the faceAPI
  try {
    let faces = await getFaces(imageUrl);
    let buffer = await createMojifiedImage(context, imageUrl, faces);
    context.res.body = buffer;
    context.log(`Posted reply with mojified image`);
    context.done();
  } catch (err) {
    let message =
      "There was an error processing this image: " + JSON.stringify(err);
    context.log.error(message);
    context.done(null, {
      status: 400,
      body: message
    });
  }
}
```

## <a name="try-it-out"></a>Prova

Se till att funktionsappen körs genom att starta den debug-menyn eller köra den från terminalen t.ex:

```bash
func host start
```

Om funktionsappen startade korrekt ska kommer utdatafönstret visa ungefär så här:

```
Http Functions:
        MojifyImage: http://localhost:7071/api/MojifyImage
```

Om funktionsappen startade korrekt besöker Webbadressen komma ihåg att skicka in URL: en till en bild via den `imageUrl` fråga param, t.ex:

http://localhost:7171/api/MojifyImage?imageUrl=[url-till-en-image]

Om allt fungerar och sedan bör den returnera en mojified version för avbildningen som detta:

![Mojified bild](/media-drafts/7.mojified-image.png)
