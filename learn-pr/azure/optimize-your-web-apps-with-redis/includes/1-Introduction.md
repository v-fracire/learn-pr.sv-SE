Du arbetar på ett företag som samlar in sportstatistik och som tillhandahåller ett API som returnerar frågeresultat. Det hjälper fans att följa och analysera matcher och resultat, både live och bakåt i tiden. Användarna kan också begära lagstatistik genom att söka på naturligt språk, till exempel ”Hur många gånger har John Smith gjort en home run mot en vänsterhänt pitcher?”.

När efterfrågan är extra hög, till exempel under slutspel, ökar svarstiden för din tjänst eftersom din serverdelstjänst inte har tillräcklig kapacitet för att möta efterfrågan. Du vill förbättra prestanda för dina användare och minska arbetsbelastningen på dina backend- och lagringstjänster. Dina mätvärden visar att mellan 50 % och 80 % av de data som returneras är för skrivskyddade eller nyligen begärda värden. Genom att implementera ett cacheminne för data som används ofta kan du förbättra prestanda och minska svarstiden.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Lära dig vad en Redis-cache är och hur du kan använda den för dina affärsbehov
- Skapa en design och planera att använda en Redis-cache
- Etablera en Redis-cache i Azure
- Ansluta ett program till cachen

## <a name="prerequisites"></a>Krav

- Erfarenhet av apputveckling
- Erfarenhet av att använda data i appar
