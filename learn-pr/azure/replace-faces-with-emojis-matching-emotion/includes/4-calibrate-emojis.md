Vi kan inte skicka en bild av emojin till ansikts-API:n för att hämta dess känsla för att, ja, för att det inte är mänskligt helt enkelt. Så för varje emoji behövde jag en mänsklig proxy, nämligen jag själv.

Jag tog bilder av mig själv där jag _noggrant_ imiterade varje emoji och använde bildens _känslomässiga uttryck_ som proxy för emojin. För skojs skull valde jag också människor i mitt team och associerade dem med emojis, så här:

![Team Moji](/media-drafts/team.jpg)

För emojin med kärleksögon (😍) valde jag en bild på min fru ❤️. Till minne av [Stephen Hawking](https://en.wikipedia.org/wiki/Stephen_Hawking) valde jag en bild av honom som ska representera 🤔.

Du kan se listan över proxybilder för varje emoji i mappen `bin/proxy-images` i exempelkoden som är associerad till den här självstudien.

I det här kapitlet kommer du att generera en nyckel så att du kan använda Azures ansikts-API och sedan använda ansikts-API:n för att kalibrera alla emojis med proxybilder av mig.

## <a name="generate-an-azure-face-api-key"></a>Generera en nyckel för ansikts-API i Azure

<!-- To make calls to the Azure Face API we will need a special authorization key.

We are going to create one using the `az` CLI. -->

För att använda ansikts-API i Azure behöver vi en särskild autentiseringsnyckel, så gå till https://azure.microsoft.com/try/cognitive-services/ och registrera dig för en kostnadsfri utvärderingsversion av ansikts-API.

![Team Moji](/media-drafts/4.calibrating-emojis.get-face-api.png)

> TODO: Hitta az-kommandon för att skapa ansikts-API och hämta nycklar

<!-- > NOTE the Azure Face API doesn't return the emotion information by default, we need to switch on this behavior by setting some query parameters, like so:
> https://westeurope.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion -->

## <a name="setup-the-environment-variables"></a>Konfigurera miljövariabler

Kalibreringsskriptet behöver veta webbadressen till din ansikts-API och nyckel för att göra rätt anrop, istället för att hårdkoda dem i skriptet vi ska använda miljövariabler i, och köra de här kommandona i terminalen du förväntar dig att köra i programmet i:

```bash
FACE_API_URL=<the-face-api-url>
FACE_API_KEY=<your-face-api-key>
```

<!-- > NOTE
> Don't forget to add the query param returnFaceAttributes=emotion to ensure the Face API returns emotion as well -->

## <a name="create-some-proxy-images-for-emojis"></a>Skapa proxybilder för emojis

Jag har lagt till alla proxybilder själv, men generera gärna dina egna!

För varje emoji i mappen `bin/proxy-images` tar du en bild av dig själv när du imiterar en emoji och ersätter bilden med din bild.

## <a name="try-it-out"></a>Prova

Nu till den roliga biten! Vi ska köra varje bild i `bin/proxy-images` via ansikts-API:t för att beräkna det känslomässiga uttrycket för den emojin _i det känslomässiga utrymmet_ och kör:

```bash
node bin/calibrate.js
```

Kommandots utdata bör se ut ungefär så här:

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

Först skrivs de emojis ut som bearbetas och slutligen skrivs en matris ut till konsolen som definierar `EmotivePoint` för alla emojis. Det här är samma format som matrisen i `shared/mojis.ts`.

Om du ändrade några av proxybilderna kopierar du sedan skriptets utdata till relevant del av `mojis.ts`
