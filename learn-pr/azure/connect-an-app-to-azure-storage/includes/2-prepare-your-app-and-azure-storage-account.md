Din app för delning av foton behöver kunna lagra olika typer av data. Du måste kunna lagra binära data som foton och textdata som användarnamn eller e-postadresser. Du måste också se till att du har gjort rätt designöverväganden när du bestämmer dig för en viss typ av lagring.

Här kommer du att få titta närmare på några designöverväganden som kan vara bra att ha i åtanke innan du väljer en lagringsstrategi. Du vill säkerligen ha något som är lätt att använda, kompatibelt med ditt språk, tillförlitligt och dessutom snabbt.

## <a name="design-considerations"></a>Designöverväganden

Azure Storage tillgängliggör funktioner via Internet med hjälp av en HTTP-slutpunkt. Vad händer om som det är problem med Internet-anslutningen? Behöver jag koda för att kunna skapa en begäran, kommunicera med slutpunkten eller parsa svar?

## <a name="connectivity-and-networking-issues"></a>Problem med anslutningar och nätverk

I den bästa av världar är nätverket tillgängligt 100% av tiden och det är alltid tillförlitligt. Detta är dock ingen realistisk förväntan. Eftersom lagringen är en viktig del av nästan alla program, och eftersom Azure Storage-API:erna görs tillgängliga över nätverket, måste du ta höjd för att nätverksavbrott faktiskt kan inträffa. Nedan visas några saker att tänka på när du ska avgöra om Azure Storage passar för dina behov:

* Förväntar sig appen oavbruten webbanslutning?

Om du skapar en webbapp är det lätt att arbeta utifrån förväntningen att det alltid finns en webbanslutning. Många appar går inte ens att öppna utan webbanslutning. Om vi utgår från att det alltid finns en webbanslutning blir det enklare att skriva appar, eftersom du inte behöver lägga tid på att skriva kod och bygga komplexa strukturer för någon offlineanvändning. Appar utan webbdel, sådana som till exempel installeras på en dator eller mobil enhet, ställer andra krav när du designar. Om du bygger en app som utgår från ständig webbanslutning måste kunna tackla anslutningsfel då och då, till exempel om nätverket ligger nere.

* Har appen stöd för ett begränsat antal funktioner om webbanslutningen bryts?

Säg att appen klarar av frånkopplingar eller att fungera i offlineläge. Kan den köra ett begränsat antal funktioner utan att krascha, generera felrapporter eller bli fullständigt oanvändbar? Det här är viktigt att tänka på, eftersom det hanteringen av frånkopplade lösningar kräver en betydande arbetsinsats och ökad komplexitet. Kanske kan du lagra vissa nödvändiga data lokalt på enheten för att möjliggöra offlineanvändning. Måste vissa delar av appen inaktiveras tills anslutningen har återställts? Dessa överväganden är specifika för varje app och det är viktigt att tänka på det redan på ett tidigt stadium när du designar och implementerar. Sådana scenarion kräver en hel del arbete.

## <a name="communicating-with-azure-storage"></a>Kommunicera med Azure Storage

När du skriver en app ska normalt sett inte behöva skriva egen kod för element på låg nivå, till exempel formatering av data som skickas till tjänster för bearbetning. Oftast finns det redan klientbibliotek som klarar att kommunicera med tjänster som Azure Storage. Tyvärr finns det språk som saknar klientbibliotek.

* Finns det ett klientbibliotek för din teknikstack?

Om inte, vill du då skapa egna slutpunkter för tjänstens åtkomstmetoder? Detta kräver i regel en hel del arbete.

Om det finns bibliotek måste du kontrollera att det finns ett tillgängligt för din teknikstack. Om du använder populära teknikstackar som till exempel .Net och Java finns det vanligtvis färdiga klientbibliotek, men om du använder mindre vanliga teknikstackar som Haskel eller Scala kan detta saknas. Mängden extra arbete som krävs när du inte kan använda ett färdigt klientbibliotek är ofta betydande. Tänk på det när du planerar för design och implementering av ditt program.

Inom ramen för den här modulen och för att förenkla det hela, gör vi antagandet att vi skapar ett .NET Core-konsolprogram som alltid ska ha Internetanslutning och saknar ett begränsat antal funktioner vid frånkoppling.

## <a name="summary"></a>Sammanfattning

Azure Storage är en flexibel och kraftfull lagringslösning vars funktioner görs tillgängliga via en rad olika HTTP-API-slutpunkter. Det är viktigt ta höjd för problem med nätverksanslutningen och tillgången på klientbibliotek vid design och implementering av appen när du ska avgöra om Azure Storage är det rätta valet för ditt program.


