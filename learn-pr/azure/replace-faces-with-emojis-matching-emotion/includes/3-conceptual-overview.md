Innan vi delve i implementeringarna vi först besvara några frågor med högre nivå.

## <a name="how-to-calculate-the-emotion-of-a-face"></a>Hur du beräknar känslan hos ett ansikte?

Beräkning av känslo är en av de mest tillgängliga delarna av programmet. Vi använder den [FaceAPI](https://azure.microsoft.com/services/cognitive-services/face/)ingår i erbjudandet Azure Cognitive Services.

FaceAPI tar som indata för en bild och returnerar information om bilden, inklusive om den identifieras alla ansikten platserna för ansikten i bild och om så krävs det beräknar och returnerar känslor i ansikten, t.ex:

```json
{
  "anger": 0.572,
  "contempt": 0.025,
  "disgust": 0.242,
  "fear": 0.001,
  "happiness": 0.014,
  "neutral": 0.111,
  "sadness": 0.033,
  "surprise": 0.003
}
```

Ta till exempel den här bilden:

![Exempel ansikte](/media-drafts/example-face.jpg)

Du kan göra en POST-begäran till en API-slutpunkt så här för att bearbeta den här bilden:

https://xxxxxxxxx.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion

Vi ger avbildningen i brödtexten t.ex:

```json
{
  "url": "<path-to-image>"
}
```

> **Obs!**
>
> API: et som standard inte returnerar känslan, måste du uttryckligen ange fråga param `returnFaceAttributes=emotion`

Användning av en hemlig nyckel autentiserar API: et Vi måste skicka den här nyckeln med rubriken.

```
Ocp-Apim-Subscription-Key: <your-subscription-key>
```

API: et med det ovanstående fråga parametrar kommer att returnera en JSON som detta:

```json
[
  {
    "faceRectangle": {
      "top": 207,
      "left": 198,
      "width": 229,
      "height": 229
    },
    "faceAttributes": {
      "emotion": {
        "anger": 0.001,
        "contempt": 0.014,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.306,
        "neutral": 0.675,
        "sadness": 0.003,
        "surprise": 0.001
      }
    }
  }
]
```

Den returnerar en matris med resultat, en per ansikte som identifierats i avbildningen. För varje ansikte returnerar storlek/plats av ansikte som `faceRectangle` och känslor visas som ett tal från 0 till 1 som `faceAttributes`.

> **Tips**
>
> Du kan börja spela med Cognitive Services även _utan_ med ett Azure-konto, gå till [den här sidan](https://azure.microsoft.com/try/cognitive-services/?api=face-api&WT.mc_id=mojifier-sandbox-ashussai) och ange din e-postadress för att få utvärderingsperioden.

## <a name="how-to-map-an-emotion-to-an-emoji"></a>Hur du mappar en känsla till en emoji?

Anta att det finns bara två känslor, rädsla och lycka, med värden mellan 0 och 1. Sedan varje ansikte kunde ritas i en 2D _känslomässig utrymme_ baserat på känslan hos användaren, t.ex:

![Euclidean avståndet 1](/media-drafts/graph-1.jpg)

Tänk dig sedan att vi också tyckte känslomässig utgångspunkten för varje emoji och ritas på 2D känslomässig utrymme samt. Om vi beräknar avståndet mellan ansiktet och alla andra emojis här 2D känslomässig vi kan ta reda på den närmaste emoji att din känslo t.ex:

![Euclidean avståndet 2](/media-drafts/graph-2.png)

Den här beräkningen kallas den `euclidian distance`, och det här är vad vi använt men inte i 2D känslomässig utrymme i en känslomässig utrymme 8 D med (ilska, förakt, avsky, rädsla, lycka, neutral, sorg, överraskning).

> **Tips**
>
> Att göra som enklare vi använde npm-paket kallas euclidean avstånd, <https://www.npmjs.com/package/euclidean-distance>.

## <a name="shared-code"></a>Delad kod

Starter exempelprojektet levereras med en delad mapp med kod som hanterar redan mycket användningsfallen ovan.

### <a name="emotivepoint"></a>EmotivePoint

Om du titta på den `EmotivePoint` klassen i `shared/emmotive-point.ts` ser du några saker.

Konstruktorn tar som indata för ett objekt som innehåller emotive information och som lokala Medlemsvariabler, t.ex:

```typescript
 constructor({
    anger,
    contempt,
    disgust,
    fear,
    happiness,
    neutral,
    sadness,
    surprise
  }) {
    this.anger = anger;
    this.contempt = contempt;
    this.disgust = disgust;
    this.fear = fear;
    this.happiness = happiness;
    this.neutral = neutral;
    this.sadness = sadness;
    this.surprise = surprise;
  }
```

Det har också en funktion som kallas avståndet som vi kan använda för att beräkna euclidian avståndet mellan två emotive punkter, t.ex:

```typescript
  distance(other) {
    let myPoint = this.toArray();
    let otherPoint = other.toArray();
    return distance(myPoint, otherPoint);
  }
```

Vi därför kan du skapa två emotive punkter och beräkna hur nära de är t.ex:

```typescript
let a = new EmotivePoint({
  /* ... */
});
let b = new EmotivePoint({
  /* ... */
});
let distance = a.distance(b);
```

### <a name="face"></a>Ansikte

En annan hjälparklass är den `Face` klassen; det kombinerar ett par olika egenskaper, inklusive den `EmotivePoint` av ett ansikte och rektangeln definiera ansiktet i bilden; vi använder den `Rect` klass för som.

Om du titta på den `Face` klass-konstruktorn i `shared/face.ts` visas den här raden med kod:

```typescript
this.moji = this.chooseMoji(this.emotivePoint);
```

`emotivePoint` är den emotive ansiktet själva.
`chooseMoji` Returnerar en lämplig emoji baserat på emotivePoint av de står inför.

```typescript
  chooseMoji(point) {
    let closestMoji = null;
    let closestDistance = Number.MAX_VALUE;
    for (let moji of MOJIS) {
      let emoPoint = moji.emotiveValues;
      let distance = emoPoint.distance(point);
      if (distance < closestDistance) {
        closestMoji = moji;
        closestDistance = distance;
      }
    }
    return closestMoji;
  }
```

`MOJIS` är listan emotive återställningspunkter för alla emojis kommer vi att diskutera hur du skapar dessa i nästa föreläsning.

Den `chooseMoji` funktionen beräknar avståndet mellan den här ansikte och alla emojis, returnerar det närmaste.

# <a name="summary"></a>Sammanfattning

För varje emoji vi beräkna en punkt i _känslomässig_ utrymme, det här kallas _kalibrering_ och vi igenom detta mer i nästa kapitel.

Använda Ansikts-API i Azure, vi får en lista över ansikten i bilder med känslomässig punkt i varje ansikte. Sedan använder euclidian avståndet algoritmen för att hitta den närmaste emoji att varje ansikte.
