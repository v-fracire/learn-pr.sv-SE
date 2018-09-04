Containrar är ett modernt sätt för att leverera program och beräkna processer. När du använder containrar, program och beroenden är de paketerade i något som kallas för en *containeravbildning*. Containeravbildningar är mycket portabla tack vare sitt register för containeravbildningar. Du kan skapa en containeravbildning i ditt utvecklingssystem och sedan köra en instans av avbildningen i ett Azure-datacenter. Du kan vara säker på att det fungerar utan ytterligare ändringar.

## <a name="container-efficiencies"></a>Effektivitet för containrar

Containrar och containeravbildningar är framtagna för att de effektivt kunna använda värdresurser, till exempel diskutrymme, minne och processor. Detta innebär att det går snabbt att starta containrar. I vissa fall startas en ny instans av en container nästan omedelbart. Detta innebär inte bara snabb etablering av program, utan även en ny modell för bearbetning på begäran och skalning av åtgärder.

Tänk dig det här scenariot: Du kör en satsvis bearbetning som ibland har ökad efterfrågan. Med hjälp av containrar kan du skapa ett system som reagerar på ökad efterfrågan genom att snabbt etablera nya containerinstanser som möter detta. Det är kraftfullt och inte lika enkelt att uppnå med traditionella virtuella datorer.

Förutom en snabb start kan du med containrar uppnå ”hyperdensitet”. Det innebär att du kan köra flera program och processer med färre virtuella eller fysiska resurser.

## <a name="use-cases"></a>Användningsfall

Även om containrar är en utmärkt plattform för att köra traditionella arbetsbelastningar som exempelvis webbservrar, kan de också användas till anpassningsbar satsvis bearbetning, program som skapats med en modern och distribuerad arkitektur och allt det som krävs vid skalning på begäran.
