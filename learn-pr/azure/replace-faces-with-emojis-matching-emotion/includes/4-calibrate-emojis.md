Vi kan inte skicka en avbildning av emoji till brunnen eftersom den inte användas för att visa dess känslo eftersom Ansikts-API. Så för varje emoji jag behövde en mänsklig proxy mig.

Jag började med bilder av själv _korrekt_ frihandsbilden varje emoji och används den _känslomässig punkt_ för avbildningen som proxy för emoji. För att göra det intressanta jag också valde personer från mitt team och kopplade emojis samt, t.ex:

![Team Moji](/media-drafts/team.jpg)

Du kan se en lista över proxy-avbildningar för varje emoji i den `bin/proxy-images` mappen i exempelkoden som är associerade med den här självstudien.

## <a name="goal"></a>Mål

I det här kapitlet genererar du nödvändiga autentiseringsnycklar så att du kan använda Ansikts-API i Azure och sedan använda Ansikts-API för att kalibrera alla emojis med hjälp av proxyn avbildningar av mig.

## <a name="learning-objectives"></a>Utbildningsmål

- Generera API-nycklar för användning med Cognitive Services.
- Hur du kör avbildningar via Ansikts-API och extrahera känslo-information.

## <a name="generate-an-azure-face-api-key"></a>Skapa en Azure Ansikts-API-nyckel

Om du vill använda Ansikts-API för Azure behöver vi en autentiseringsnyckel.

Det snabbaste sättet att hämta en nyckel är att gå över till https://azure.microsoft.com/try/cognitive-services/?api=face-api och registrering av Ansikts-API-utvärderingsversionen.

När du registrera dig får du några delarna av information som du behöver för att lagra för senare.

1. Hämta den _endpoint_. Det bör se ut ungefär som https://westcentralus.api.cognitive.microsoft.com/face/v1.0

2. Den visar två API-nycklar, store en av dem för användning senare ingen (det spelar roll vilken)

## <a name="setup-the-environment-variables"></a>Konfigurera miljövariabler

Skriptet kalibrering behöver veta ditt Ansikts-API-URL och nyckel för att göra rätt-anrop, i stället för att hårdkoda dessa i skriptet som vi ska använda miljövariabler, kör följande kommandon i terminalen du förväntar dig att köra programmet:

```bash
export FACE_API_URL=<the-face-api-url>
export FACE_API_KEY=<your-face-api-key>
```

Vi använder också ett paket som heter `dotenv` i vår Node-programmet. Det här paketet ska vi använda lagra miljövariablerna lokalt i en fil med namnet `.env`. Den `dotenv` paketet laddas alla variabler i den här filen och visas som miljövariabler i ditt program.

> **OBS!**
>
> Inte checka in `.env` filerna till källkontroll.

Azure Functions har ett annat sätt för hantering av miljövariabler via sina `local.settings.json` filen, mer om det senare.

## <a name="create-some-proxy-images-for-emojis"></a>Skapa en proxy-avbildningar för emojis

Jag har angett alla avbildningar för proxy själv, men kan skapa din egen!

För varje emoji i den `bin/proxy-images` mappen, ta en bild av själv frihandsbilden som emoji och ersätta den med din avbildning.

## <a name="try-it-out"></a>Prova

Nu kommer roligt del! Vi kommer att köra var och en av avbildningarna i den `bin/proxy-images` via Ansikts-API för att beräkna en känslomässig tidpunkt för den emoji i _känslomässig utrymme_, kör:

```bash
node bin/calibrate.js
```

Kommandots utdata bör se ut ungefär som detta:

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

Den första skriver ut den emoji's är den bearbetning och sedan slutligen skriva ut till konsolen en matris som definierar den `EmotivePoint` av alla emoji. Det här är samma format som matrisen i `shared/mojis.ts`.

Om du har ändrat vissa via proxy bilder kan sedan kopiera utdata från det här skriptet till relevanta avsnitt av `mojis.ts`
