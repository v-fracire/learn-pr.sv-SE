Många utvecklare anser att de ramverk och bibliotek som de använder för att skapa sin programvara främst avgörs av funktioner eller personligt tycke. Valet av ramverk är dock ett viktigt beslut inte bara vad gäller design och funktioner, utan även ur ett _säkerhetsperspektiv_. Att välja ett ramverk med moderna säkerhetsfunktioner och att hålla det uppdaterat är ett av de bästa sätten att se till att dina appar är säkra.

## <a name="choose-your-framework-carefully"></a>Välj ditt ramverk noggrant

Den viktigaste faktorn avseende säkerhet när du väljer ett ramverk är hur väl det underhålls. De bästa ramverken har uttryckliga säkerhetsplaner och stöds av stora communities som förbättrar och testar ramverket. Inga program är 100 % felfria eller helt säkra, men när en säkerhetsrisk identifieras vill vi vara säkra på att den åtgärdas eller att en kringåtgärd tillhandahålls snabbt.

Ofta är ”bra stöd” synonymt med ”modernt”. Äldre ramverk tenderar att antingen ersättas eller gradvis bli mindre populära. Även om du har betydande erfarenhet eller många appar som skrivits i ett äldre ramverk är det bättre att välja ett modernt bibliotek som har de funktioner du behöver. Moderna ramverk brukar bygga på erfarenheter av tidigare iterationer, vilket gör att de minskar attackytan när de används för nya appar. Du får en mindre app att oroa dig för om en säkerhetsrisk identifieras i det äldre ramverket som dina äldre program är skrivna i.

<!-- TODO: add link; Should we be pointing to other modules? -->
<!--
For more information on secure design and reducing threat surface, please see [Design For Security in Azure](../../design-for-security-in-azure/index.yml).
-->

## <a name="keep-your-framework-updated"></a>Håll ditt ramverk uppdaterat

Ramverk för programvaruutveckling, till exempel Java Spring och .NET Core, släpper uppdateringar och nya versioner regelbundet. Dessa uppdateringar innehåller nya funktioner, borttagning av gamla funktioner och ofta säkerhetskorrigeringar eller förbättringar. När vi låter våra ramverk bli inaktuella skapas ”tekniska skulder”. Ju mer inaktuella de blir, desto svårare och mer riskfyllt blir det att föra in koden till den senaste versionen. Ungefär som med det inledande valet av ramverk ökar användningen av äldre ramverksversioner sårbarheten mot säkerhetshot som har åtgärdats i nyare versioner av ramverket.

Till exempel hittades [30 sårbarheter](https://www.cvedetails.com/product/6117/Apache-Struts.html?vendor_id=45) under 2016–2017 i Apache Struts-ramverket. Dessa åtgärdades snabbt av utvecklingsteamet, men vissa företag tillämpade inte uppdateringarna och [drabbades därmed av ett dataintrång](https://www.zdnet.com/article/equifax-confirms-apache-struts-flaw-it-failed-to-patch-was-to-blame-for-data-breach/). **Se till att hålla dina ramverk och bibliotek uppdaterade**.

### <a name="how-do-i-update-my-framework"></a>Hur uppdaterar jag mitt ramverk?

Vissa ramverk, till exempel Java eller .NET, kräver en installation och tenderar att lanseras regelbundet. Det är en bra praxis att bevaka nya versioner och planera inför att skapa en egen kodgren för att prova dem när de släpps. Till exempel upprätthåller .NET Core en [sida med viktig information](https://github.com/dotnet/core/tree/master/release-notes) som du kan läsa för att hitta de senaste tillgängliga versionerna.

Mer specialiserade bibliotek såsom JavaScript-ramverk eller .NET-komponenter kan uppdateras via en pakethanterare. **NPM** och **Bower** är populära alternativ för webbprojekt och stöds av de flesta IDE:er eller byggverktyg. I .NET använder vi **NuGet** för att hantera våra komponentberoenden. Liksom att uppdatera kärnramverket och förgrena koden är uppdatering av komponenterna och testning en bra teknik för att verifiera en ny version av ett beroende.

> [!NOTE]
> Kommandoradsverktyget `dotnet` har alternativen `add package` och `remove package` för att lägga till eller ta bort NuGet-paket men saknar ett motsvarande `update package`-kommando. Dock visar det sig att du kan köra `dotnet add package <package-name>` i ditt projekt så _uppgraderar_ det automatiskt paketet till den senaste versionen. Det här är ett enkelt sätt att uppdatera beroenden utan att behöva öppna IDE.

## <a name="take-advantage-of-built-in-security"></a>Dra nytta av inbyggd säkerhet

Kontrollera alltid vilka säkerhetsfunktioner dina ramverk erbjuder. Distribuera **aldrig** din egen säkerhet om det finns en inbyggd standardteknik eller funktion. Dessutom bör du använda beprövade algoritmer och arbetsflöden eftersom de ofta har granskats av många experter som påpekat och förbättrat funktionerna så att du kan lita på att de är tillförlitliga och säkra.

.NET Core-ramverket har ett mycket stort antal säkerhetsfunktioner. Här är några väsentliga platser att börja i dokumentationen.
* [Autentisering – Identitetshantering](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/index?view=aspnetcore-2.1)
* [Auktorisering](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/index?view=aspnetcore-2.1)
* [Dataskydd](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/index?view=aspnetcore-2.1)
* [Säker konfiguration](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/configuration/index?view=aspnetcore-2.1)
* [API:er för utökningsbarhet av säkerhet](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/extensibility/index?view=aspnetcore-2.1)

Alla dessa funktioner skrevs av experter inom sina respektive områden, och utsattes sedan för ett otal tester för att se till att de fungerar som avsett och inte på något annat sätt. Andra ramverk har liknande funktioner – hör med ramverkets leverantör om vad som finns i varje kategori.

> [!WARNING]
> Att skriva ens egen säkerhetskod i stället för att använda den som tillhandahålls av ramverket är inte bara slöseri med tid, utan även mindre säkert.


## <a name="azure-security-center"></a>Azure Security Center

När du använder Azure som värd för dina webbprogram varnar Security Center dig om dina ramverk är inaktuella som en del av fliken med rekommendationer.  Glöm inte att då och då titta om det finns några varningar angående dina appar där.

![Azure Security Center rekommenderar en ramverksuppgradering.](../media-draft/ASCFramework.png)


## <a name="summary"></a>Sammanfattning

Närhelst det är möjligt bör du välja ett modernt ramverk för att bygga dina appar. Använd alltid de senaste inbyggda funktionerna och se till att du håller det uppdaterad. Dessa enkla regler hjälper till att ge programmet en fördelaktig start.
