Bilddelning programmet kräver möjlighet att lagra olika typer av data. Du har binära data som foton och textdata, till exempel användarnamn eller e-postadresser.

Här kan titta du på vissa designöverväganden att ha i åtanke när du planerar en lagringsstrategi för. Du vill säkerligen ha något som är lätt att använda, kompatibelt med ditt språk, tillförlitligt och dessutom snabbt.

## <a name="design-considerations"></a>Designöverväganden

Azure Storage innehåller funktioner som använder en HTTP-slutpunkt via Internet. Vad händer om mitt program inte går att nå slutpunkten? Hur hanterar problem med nätverksanslutningen?

## <a name="connectivity-and-networking-issues"></a>Problem med anslutningar och nätverk

I den bästa av världar är nätverket tillgängligt 100% av tiden och det är alltid tillförlitligt. Men det är inte en realistisk förväntan och vi har ofta behöver bry dig om problem med nätverksanslutningen - särskilt om vi använder mobila enheter. Eftersom lagringen är en del av nästan alla program och API: erna för Azure Storage körs i nätverket, måste du ta hänsyn till effekterna av ett nätverksavbrott. Överväg att dessa två frågor.

### <a name="does-your-application-expect-that-it-can-always-maintain-web-connectivity"></a>Förväntar sig appen oavbruten webbanslutning?

Om du skapar ett webbprogram, det finns ofta ett antagande att webbanslutningen är alltid tillgänglig – när alla hur kan begäran även sätta igång om det fanns inget nätverk? Detta verkligen blir enklare att skriva program, inte behöver du fel kontroller eller ska hantera offline-scenarier. Andra typer av appar, till exempel dator eller mobila program, det går inte att göra sådana antaganden. Dessa typer av appar behöver för att kunna hantera anslutningsfel ibland.

### <a name="can-your-application-gracefully-respond-to-network-outages"></a>Kan programmet gradvis svara på avbrott i nätverket?

Om ditt program kan kopplas från regelbundet eller minst fungerar i frånkopplat läge, kommer att försämras gradvis utan kraschar eller rapporterar ett fel och som inte kan användas? Det här är viktigt att tänka på, eftersom det hanteringen av frånkopplade lösningar kräver en betydande arbetsinsats och ökad komplexitet. Kanske kan du lagra vissa nödvändiga data lokalt på enheten för att möjliggöra offlineanvändning. Vissa aspekter av programmet måste inaktiveras tills anslutningen har återställts? Dessa överväganden är specifika för programmet och det är viktigt att inkludera det i design och implementering tidigt på grund av arbetet som ingår i restaurang för det här scenariot.

## <a name="communicating-with-azure-storage"></a>Kommunicera med Azure Storage

När du skriver en app ska normalt sett inte behöva skriva egen kod för element på låg nivå, till exempel formatering av data som skickas till tjänster för bearbetning. Oftast finns det redan klientbibliotek som klarar att kommunicera med tjänster som Azure Storage. Tyvärr finns det språk som saknar klientbibliotek.

### <a name="is-there-an-available-client-library-for-your-technology-stack-of-choice"></a>Finns det ett klientbibliotek för din teknikstack?

Om inte, är du innehållet för att skapa tjänstens slutpunkt åtkomstmetoder, vilket omfattar vanligtvis mer av dig?

Om det finns bibliotek måste du kontrollera att det finns ett tillgängligt för din teknikstack. Om du använder mer modern teknik-stackar, till exempel .NET och Java, är vanligtvis klientbibliotek tillgängliga. Men om du använder mindre vanliga teknikstackar, t.ex. Haskel eller Scala, så är detta kanske inte fallet. Mängden extra arbete som krävs när du inte kan använda ett färdigt klientbibliotek är ofta betydande. Tänk på det när du planerar för design och implementering av ditt program.

Nu ska vi skapa ett program som ska använda Azure Storage.