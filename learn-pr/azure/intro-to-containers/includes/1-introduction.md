Containrar är ett modernt sätt för att leverera program och beräkna processer. När du använder containrar, program och beroenden är de paketerade i något som kallas för en *containeravbildning*. Containeravbildningar är mycket portabla när ett register för containeravbildningar används. Du kan skapa en containeravbildning i ditt utvecklingssystem och sedan köra en instans av avbildningen i ett Azure-datacenter. Då kan du vara säker på att det fungerar utan ytterligare ändringar.

## <a name="container-efficiencies"></a>Effektivitet för containrar

Containrar och containeravbildningar är framtagna för att de effektivt kunna använda värdresurser, till exempel diskutrymme, minne och processor. Detta innebär att det går snabbt att starta containrar. I vissa fall startas en ny instans av en container nästan omedelbart. Detta innebär inte bara snabb etablering av program, utan möjliggör även en ny modell för bearbetning på begäran och skalning av åtgärder.

Tänk dig det här scenariot: Du kör en satsvis bearbetning som ibland har ökad efterfrågan. Med hjälp av containrar kan du skapa ett system som reagerar på ökad efterfrågan genom att snabbt etablera nya containerinstanser som möter detta. Det är kraftfullt och inte lika enkelt att uppnå med traditionella virtuella datorer.

Förutom snabb uppstart tillåter containrarna även "hyperdensitet". Det innebär att du kan köra flera program och processer med färre virtuella eller fysiska resurser.

## <a name="use-cases"></a>Användningsfall

Även om containrar är en utmärkt plattform för att köra traditionella arbetsbelastningar som exempelvis webbservrar, kan de också användas till anpassningsbar satsvis bearbetning, program som skapats med en modern och distribuerad arkitektur och allt det som krävs vid skalning på begäran.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att:

- Förbereda en lokal miljö för containerutveckling.
- Läsa om grundläggande Docker-åtgärder.
- Köra, visa och ta bort containrar.
- Skapa anpassade containeravbildningar.
- Överföra containeravbildningar till ett offentligt containerregister och köra containrarna från avbildningarna.
