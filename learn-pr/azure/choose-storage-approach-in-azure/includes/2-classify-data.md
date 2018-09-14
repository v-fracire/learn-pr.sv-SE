En onlinebutiker företag har olika typer av data. Varje typ av data kan ha nytta av en annan lösning. 

Programdata kan klassificeras på något av tre sätt: strukturerade, halvstrukturerade och Ostrukturerade. Här lär du dig att klassificera dina data så att du kan välja lämplig lagringslösning.

## <a name="structured-data"></a>Strukturerade data

Strukturerade data följer ett schema, så att alla dataelement har samma fält eller egenskaper. Strukturerade data kan lagras i en databastabell med rader och kolumner. Strukturerade data är beroende av nycklar som visar hur en rad i en tabell relaterar till data i en annan rad i en annan tabell. Strukturerade data kallas också relationsdata, eftersom den dataschema definierar tabell med data, fälten i tabellen och rensa relationen mellan två.

Det är enkelt att använda strukturerade data eftersom de är enkla att ange, köra frågor mot och analysera. Alla data följer samma format.

Här är några exempel på strukturerade data:

- Sensordata
- Finansiella data

## <a name="semi-structured-data"></a>Halvstrukturerade data

Halvstrukturerade data är mindre ordnad än strukturerade data och lagras inte i ett relationsformat som fälten snyggt inte ryms i tabeller, rader och kolumner. Halvstrukturerade data innehåller taggar som gör för organisationen och en hierarki av data tydligt. Halvstrukturerade data kallas även för icke-relationella data eller NoSQL-data.

Här är några exempel på halvstrukturerade data:

- JSON-filer
- XML-filer

## <a name="unstructured-data"></a>Ostrukturerade data

Organisationen av Ostrukturerade data är vanligtvis tvetydig. Ofta levereras Ostrukturerade data i filer, till exempel foton och videor. Själva videofilen kan ha en övergripande struktur och levereras med halvstrukturerade metadata, men de data som utgör själva videon är ostrukturerade. Därför klassificeras foton, videor och andra liknande filer som ostrukturerade data.

Här är några exempel på ostrukturerade data:

- Mediafiler som foton, videor och ljudfiler
- Office-filer som Word-dokument
- Textfiler
- Loggfiler

Nu när du vet skillnaderna mellan varje typ av data ska vi titta på de uppgifter som används i ett företag för onlinebutiker och klassificera dem.

## <a name="product-catalog-data"></a>Data i produktkatalogen

Produkten katalogdata för en onlinebutiker företag är ganska strukturerad sin natur, eftersom varje produkt har en produkt-SKU, en beskrivning, en kvantitet, ett pris, alternativ för storlek, färgalternativ, ett foto och eventuellt en video. Den här typen av data kan därför verka relationsbaserad eftersom alla dataelement har samma struktur. När du lägger till nya produkter eller olika typer av produkter, kanske du vill lägga till olika fält med tiden. Till exempel avser nya tennisskor som du körts Bluetooth-aktiverade relay sensordata från skoaffärsinvesteringen till en lämplighet på användarens telefon. Det här verkar vara en växande trend och du vill ger kunderna möjlighet att filtrera efter ”Bluetooth-aktiverade” skor i framtiden. Du inte vill gå tillbaka och uppdatera alla dina befintliga sko-data med en Bluetooth-aktiverade egenskap vill du bara lägga till den nya situation.

Med hjälp av egenskapen Bluetooth-aktiverade inte dina sko data längre homogen, som du har infört skillnader i schemat. Om det här är det enda undantaget som du förväntar dig att stöta på kan du gå tillbaka och normalisera befintliga data så att alla produkter med ett ”Bluetooth-aktiverade”-fält för att upprätthålla en strukturerad, relationell organisation. Men om det är bara en av många special fält som du tänker dig stöd i framtiden kan är sedan klassificeringen av data halvstrukturerade. Data är ordnad efter taggar, men varje produkt i katalogen kan innehålla unika fält.

Dataklassificering: **halvstrukturerade**

## <a name="photos-and-videos"></a>Foton och videor

Foton och videor som visas på produktsidor är ostrukturerade data. Även om mediefilen kan innehålla metadata är mediefilens innehåll ostrukturerat.

Dataklassificering: **ostrukturerade**

## <a name="business-data"></a>Affärsdata

Affärsanalytiker vill använda business intelligence till att utvärdera lagerflöden och granska säljdata. För att kunna utföra de här åtgärderna så måste data från flera olika månader aggregeras och därefter frågas. På grund av behovet att samla in liknande data måste dessa data vara strukturerade, så att en månad kan jämföras med nästa.

Dataklassificering: **strukturerade**

## <a name="summary"></a>Sammanfattning

Data kan klassificeras på något av tre sätt: strukturerade, halvstrukturerade och Ostrukturerade. Förstå skillnaderna så att du kan klassificera dina egna data hjälper dig att välja rätt lagringslösningen. 

Om du vill tar och sammanfattar, är strukturerade data ordnad data som snyggt passar in i rader och kolumner i tabeller. Halvstrukturerade data visserligen organiserade och har tydliga egenskaper och värden, men det kan finnas skillnader mellan datapunkterna. Ostrukturerade data passar inte in i tabeller, och det finns inte heller något schema.