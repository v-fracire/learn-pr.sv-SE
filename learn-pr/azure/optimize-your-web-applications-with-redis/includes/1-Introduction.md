Du arbetar på ett företag som samlar in sportstatistik och som tillhandahåller ett API för appar som returnerar frågeresultat. Det hjälper fans att följa och analysera matcher och resultat, både live och bakåt i tiden. Användarna kan också begära lagstatistik genom att söka på naturligt språk, till exempel ”Hur många gånger har John Smith gjort en home run mot en vänsterhänt pitcher?”.

När efterfrågan är extra hög, till exempel under slutspel, ökar svarstiden för din tjänst eftersom din backend-tjänst inte har tillräcklig kapacitet för att möta efterfrågan. Du vill förbättra prestanda för dina användare och minska arbetsbelastningen på dina backend- och lagringstjänster. Dina mätvärden visar att mellan 50 % och 80 % av de data som returneras är för skrivskyddade eller nyligen begärda värden. Genom att implementera ett cacheminne för data som används ofta kan du förbättra prestanda och minska svarstiden.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att:

- Lära dig vad en Redis-cache är och vad du kan använda den för.
- Skapa en design och planera att använda en Redis-cache.
- Etablera en Redis-cache i Azure.
- Ansluta en webbapp till cachen.

## <a name="prerequisites"></a>Nödvändiga komponenter

- Erfarenhet av apputveckling
- Erfarenhet av att använda data i appar
