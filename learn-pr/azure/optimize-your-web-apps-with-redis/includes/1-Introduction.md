Du arbetar på ett företag som spårar Sportstatistik och tillhandahåller ett API som frågeresultat. Det hjälper fans att följa och analysera matcher och resultat, både live och bakåt i tiden. Användare kan även begära teamstatistiken med hjälp av en sökning med naturligt språk, till exempel ”hur många gånger har John Smith når en home-körning mot en vänster kanna”?

Under tider med hög belastning långsammare till exempel under slutspel, svarstid för din tjänst eftersom backend-tjänst inte har kapacitet för att möta efterfrågan. Du vill förbättra prestanda för dina användare och minska arbetsbelastningen på dina serverdels- och datalagringstjänster. Dina mätvärden visar att mellan 50 % och 80 % av de data som returneras är för skrivskyddade eller nyligen begärda värden. Genom att implementera ett cacheminne för data som används ofta kan du förbättra prestanda och minska svarstiden.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Beskriv vilka Redis cache är och hur du kan använda det för dina affärsbehov
- Skapa en design och planera att använda en Redis-cache
- Etablera en Redis-cache i Azure
- Ansluta ett program till cachen

## <a name="prerequisites"></a>Förutsättningar

- Erfarenhet av apputveckling
- Erfarenhet av att använda data i appar
