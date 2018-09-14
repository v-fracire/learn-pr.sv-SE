Många utvecklare överväga ramverk och bibliotek som de använder för att skapa sin programvara med beslutas främst av funktioner eller smak. Men det ramverk som du väljer är ett viktigt beslut, inte bara ur en design och funktioner och från en _security_ perspektiv. Välja ett ramverk med moderna säkerhetsfunktioner och hålla den uppdaterad är en av de bästa sätten att se till att dina appar är säkra.

## <a name="choose-your-framework-carefully"></a>Välj ditt ramverk noggrant

Den viktigaste faktorn avseende säkerhet när du väljer ett ramverk stöds hur bra det är. De bästa ramverken ha angett-ordning och stöds av stora grupper som förbättra och testa ramen. Inga program är 100% felfritt eller helt säkra, men när en säkerhetsrisk identifieras vi vill vara säker på att den kommer att avslutas eller har en lösning som tillhandahålls snabbt.

Ofta ”bra stöds” är synonyma ”moderna”. Äldre ramverk tenderar att antingen ersättas eller så småningom Tona i större utsträckning. Även om du har betydande erfarenhet eller eller många appar som skrivits i en äldre framework kan du är det bättre att välja en modern bibliotek som har de funktioner du behöver. Moderna ramverk brukar bygger på erfarenheter av tidigare iterationer, vilket gör att välja dem för nya appar en form av attackytan för hot. Du har en mindre app oroa dig om en säkerhetsrisk identifieras i det äldre ramverk för äldre program som är skrivna i.

<!-- TODO: add link; Should we be pointing to other modules? -->
<!--
For more information on secure design and reducing threat surface, please see [Design For Security in Azure](../../design-for-security-in-azure/index.yml).
-->

## <a name="keep-your-framework-updated"></a>Håll ditt ramverk som uppdateras

Utvecklingsramverk för programvara, till exempel Java Spring och .NET Core viktiga uppdateringar och nya versioner regelbundet. Dessa uppdateringar innehåller nya funktioner, ta bort gamla funktioner, och ofta säkerhetskorrigeringar eller förbättringar. När vi tillåter vår ramverk som behövs för att bli inaktuell skapar ”tekniska skulder”. Den ytterligare inaktuell vi få, svårare och riskfyllda blir det att använda vår kod upp till den senaste versionen. Ungefär som dessutom inledande framework-val, med äldre versioner av framework öppnar du upp till flera säkerhetshot som fastställts i nyare versioner av framework.

Till exempel från 2016-2017 [30 sårbarheter](https://www.cvedetails.com/product/6117/Apache-Struts.html?vendor_id=45) hittades i Apache Struts-ramverket. Dessa snabbt beskrivs i Utvecklingsteamet, men vissa företag kunde inte tillämpas uppdateringarna och [betald priset i form av ett intrång data](https://www.zdnet.com/article/equifax-confirms-apache-struts-flaw-it-failed-to-patch-was-to-blame-for-data-breach/). **Se till att hålla ditt ramverk och bibliotek uppdaterade**.

### <a name="how-do-i-update-my-framework"></a>Hur uppdaterar jag mina framework?

Vissa ramverk, som Java eller .NET, kräver en installation och tenderar att släppa på en känd takt. Det är en bra idé att bevaka nya versioner och planerar att göra en gren i din kod att testa den när den släpps. Till exempel .NET Core upprätthåller en [release notes sidan](https://github.com/dotnet/core/tree/master/release-notes) som du kan kontrollera för att hitta de senaste versionerna som är tillgängliga.

Mer specialiserade bibliotek, till exempel JavaScript-ramverk eller .NET-komponenter kan uppdateras via en pakethanterare. **NPM** och **Bower** är populära alternativ för webbprojekt och stöds av de flesta IDE: er eller skapa verktyg. I .NET, använder vi **NuGet** att hantera vår komponentberoenden. Uppdaterar ramverket är branchning din kod, uppdatera komponenterna och testning liksom en bra teknik för att verifiera en ny version av ett beroende.

> [!NOTE]
> Den `dotnet` kommandoradsverktyget har en `add package` och `remove package` alternativet att lägga till eller ta bort NuGet-paket men saknar en motsvarande `update package` kommando. Men det har visat sig kan du köra `dotnet add package <package-name>` i ditt projekt och det kommer du att automatiskt _uppgradera_ paketet till den senaste versionen. Det här är ett enkelt sätt att uppdatera beroenden utan att behöva öppna IDE.

## <a name="take-advantage-of-built-in-security"></a>Dra nytta av inbyggd säkerhet

Kontrollera alltid om du vill se vilka säkerhetsfunktioner erbjudandet ramverk. **Aldrig** distribuera egna säkerhet om det finns en standard teknik eller funktion. Dessutom kan förlita dig på beprövade algoritmer och arbetsflöden eftersom de ofta har granskas av experter och är mycket critiqued och förstärka så att du kan lita på att de är tillförlitlig och säker.

.NET Core-ramverket har oräkneliga säkerhetsfunktioner, här är några kärnor från platser i dokumentationen.
* [Autentisering-Identitetshantering](https://docs.microsoft.com/aspnet/core/security/authentication/index?view=aspnetcore-2.1)
* [Auktorisering](https://docs.microsoft.com/aspnet/core/security/authorization/index?view=aspnetcore-2.1)
* [Dataskydd](https://docs.microsoft.com/aspnet/core/security/data-protection/index?view=aspnetcore-2.1)
* [Konfigureringen av säker](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/index?view=aspnetcore-2.1)
* [Security utökningsbarhet API: er](https://docs.microsoft.com/aspnet/core/security/data-protection/extensibility/index?view=aspnetcore-2.1)

Var och en av dessa funktioner av experter i sina respektive fält, och sedan battered med tester för att se till att den fungerar som avsett och endast som avsett. Andra ramverk har liknande funktioner – Kontrollera med leverantören som ger möjlighet att ta reda på vad de hade i varje kategori.

> [!WARNING]
> Skriva egna kontroller för informationssäkerhet, istället för att använda det som tillhandahålls av din framework är inte bara ha lagt tid, är det mindre säkra.


## <a name="azure-security-center"></a>Azure Security Center

När du använder Azure som värd för dina webbprogram varnar Security Center dig om ditt ramverk är inaktuell som en del av fliken rekommendationer.  Glöm inte att du ser det då och då för att se om det finns några varningar relaterade till dina appar.

![Azure Security Center rekommenderar en uppgradering av framework.](../media-draft/ASCFramework.png)


## <a name="summary"></a>Sammanfattning

Välj ett moderna ramverk för att skapa dina appar alltid använda de inbyggda säkerhetsfunktionerna och kontrollera att du hålla den uppdaterad när det är möjligt. Dessa enkla regler ser du till ditt program startar på en stabil grund.
