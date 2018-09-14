Vi har skapat den `MojifyImage` Azure-funktion som returnerar en avbildning av en mojified; vi behöver en andra slutpunkt vilka slack anropar varje gång någon kör den `\mojify` kommando.

Den här andra slutpunkten måste returnera URL-Adressen till den `MojifyImage` Azure-funktion.

När du kör ett slack-kommando som `\mojify [some-image-url]` blir en POST-begäran till slutpunkten som du har konfigurerat och skicka in `[some-image-url]` som en `text` fråga param inbäddad i meddelandets brödtext.

Målet med den här föreläsning är att skapa den här funktionen som samordnar och svarar på slack kommandon. Vi kallar den här andra Azure-funktion `RespondToSlackCommand`.

I slutet av den här föreläsning lär du dig:

- Så här skapar du en HTTP-slutpunkt som svarar på Slack kommandon du att förstå formatet Slack förväntar sig för denna begäran och hur du svarar med en avbildning.

## <a name="create-the-function-trigger-and-convert-to-typescript"></a>Skapa utlösare för funktionen och konvertera till TypeScript

Vi måste du skapa en annan http-utlöst Azure Function. Dessa instruktioner är samma som föregående lektionen; den enda skillnaden är att vi anropar den här funktionen `RespondToSlackCommand` i stället för `MojifyImage`.

1. Typ <kbd>Ctrl</kbd>+<kbd>P</kbd> öppna kommandopaletten, och sedan söka efter och välj `Azure Functions: Create Function...`

   ![Skapa ny funktion](/media-drafts/7.create-function.png)

2. Välj den mapp där du skapade function-projekt tidigare (det första alternativet är den aktuella mappen)

   ![Välj mapp](/media-drafts/7.select-current-project.png)

3. Vi skapar en _HTTP-utlösare_, så markerar du det alternativet.

   ![Välj mapp](/media-drafts/7.select-trigger.png)

4. Skriv i `RespondToSlackCommand` som namnet på den funktion som du vill skapa

   ![Välj namn](/media-drafts/7.choose-function-name.png)

5. Välj anonym autentisering på nätverksnivå som autentisering

   ![Välj namn](/media-drafts/7.choose-auth-level.png)

Om den har slutförts sedan en mapp med namnet `RespondToSlackCommand` ska finnas i rotkatalogen.

Samma som senaste bedriver undervisning vi nu konvertera det från `JavaScript` till `TypeScript`.

6. Skapa en fil med namnet `index.ts` i den `RespondToSlackCommand` mapp.

   Se till att den `TypeScript` skapandeprocessen fortfarande körs och att den automatiskt kompileras den i `index.js` och `index.map` filer.

7. Klistra in den här koden i `index.ts`

```typescript
export async function index(context, req) {
  context.log("RespondToSlackCommand HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

## <a name="try-it-out"></a>Prova

Se till att allt fungerar genom att besöka http://localhost:7171/api/RespondToSlackCommand i en webbläsare, bör den ut våra `Hello!`.

## <a name="flesh-out-the-index-function"></a>Bygga ut den `index` funktion

Detta `index` funktionen är mycket snabbare att skriva än funktionen tidigare.

Låt oss först konfigurera våra `context.res` objekt

```typescript
context.res = {
  headers: {
    "Content-Type": "application/json"
  },
  body: null
};
```

Enklare än vad som tidigare kan vi inte anger den `isRaw` egenskapen eftersom detta är som standard till `false`, men vi anger innehållstypen vara `application/json`.

Som nämnts tidigare Slack skickar bild-URL vi vill bearbeta som en frågesträng med nyckeln för `text`, men av säkerhetsskäl bäddas in detta i brödtexten i begäran, så vi behöver arbeta lite svårare för att få den information vi behöver , inte för svårt att!

Gör våra liv enklare ska vi importera noden `querystring` paketet, om det kommer som en del av NodeJS så du behöver installera något extra.

```typescript
import * as querystring from "querystring";
```

I vår `index` funktion som vi kan konvertera den `req.body` placeras i ett objekt som vi extraherar den `text` egenskap, t.ex:

```typescript
const { text } = querystring.parse(req.body);
```

Om användaren har angett kommandot korrekt så att texten ska innehålla URL: en för en avbildning, kan vi lägga till i några grundläggande verifiering t.ex:

```typescript
let message = "Your mojified image my liege...";
if (!text) {
  message = "You must provide an image to mojify";
}
```

Kom ihåg att kommandot slack anropar https://somedomain.com/api/RespondToSlackCommand, och vi behöver att svara med MojifyImage URL: en som kommer de flesta antagligen vara på samma domän, https://somedomain.com/api/MojifyImage

I stället för hardcoding domänen i filen, i stället använder vi samma domän som begäran domänen för te `MojifyImage` url, t.ex:

```typescript
const mojifyUrl = req.url.substr(0, req.url.lastIndexOf("/")) + "/MojifyImage";
```

Slutligen ska vi ange brödtexten i svaret, slack kommandot förväntar sig ett visst format i svaret, t.ex:

```typescript
context.res.body = {
  response_type: "in_channel",
  text: message,
  attachments: [
    {
      image_url: `${mojifyUrl}?imageUrl=${text}`
    }
  ]
};

context.done();
```

Det viktiga är att notera ovan den `image_url` i den `attachments` egendom; vi väljer avkastningen på `mojifyUrl` skicka i URL du angav i kommandot som den `imageUrl` fråga param.

## <a name="try-it-out"></a>Prova

Se till att allt fungerar genom att besöka http://localhost:7171/api/RespondToSlackCommand i en webbläsare, bör det nu skriva ut några `json` som den nedan:

```json
{
  "response_type": "in_channel",
  "text": "You must provide an image to mojify",
  "attachments": [
    {
      "image_url": "http://localhost:7071/api/MojifyImage?imageUrl=undefined"
    }
  ]
}
```
