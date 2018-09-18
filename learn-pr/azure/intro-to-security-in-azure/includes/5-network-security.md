Att skydda ditt nätverk från attacker och obehörig åtkomst är en viktig del i alla arkitekturer. Här tittar vi på hur nätverkssäkerheten ser ut, hur du integrerar en metod med flera lager i arkitekturen, samt hur Azure kan hjälpa dig med nätverkssäkerheten för din miljö.

## <a name="a-layered-approach-to-network-security"></a>En metod för nätverkssäkerhet med flera lager

En gemensam tråd i den här modulen har varit att använda en säkerhetsmetod med flera lager, och den metoden fungerar på samma sätt vid nätverkslagret. Det räcker inte att bara fokusera på säkra perimeternätverket eller på nätverkssäkerheten mellan tjänster i ett nätverk. En metod med flera lager ger flera nivåer av skydd, så om en angripare tar sig igenom ett lager finns det ytterligare skydd på plats för att begränsa angreppet.

Vi går igenom hur Azure kan ge verktyg för en metod med flera lager för att säkra ditt nätverksfotavtryck.

### <a name="internet-protection"></a>Internetskydd

Om vi startar i utkanten av nätverket fokuserar vi på att begränsa och eliminera attacker från Internet. Ett bra område att börja med är att utvärdera de resurser som är riktade mot Internet och endast tillåta inkommande och utgående kommunikation där det finns behov. Identifiera alla resurser som tillåter inkommande nätverkstrafik oavsett typ och se till att de är nödvändiga och begränsade till endast de portar/protokoll som krävs. Azure Security Center är ett bra ställe att leta efter den här informationen eftersom det identifierar Internetanslutna resurser som inte har nätverkssäkerhetsgrupper (NSG) associerade, samt resurser som inte skyddas bakom en brandvägg.

Det finns flera sätt att ge inkommande skydd i perimetern:

* Application Gateway är en lastbalanserare med en brandvägg för webbaserade program som ger skydd mot kända säkerhetsrisker.

* Virtuella nätverksinstallationer (NVA:er) kan användas för tjänster utan http eller avancerade konfigurationer. NVA:erna liknar maskinvaruinstallationer för brandväggen.


För resurser som är exponerade mot Internet finns det risk för överbelastningsattacker. Dessa typer av attacker försöker överbelasta en nätverksresurs genom att skicka så många begäranden att resursen blir långsam eller slutar svara. För att förhindra dessa attacker ger Azure DDoS ett grundläggande skydd för alla Azure-tjänster och förbättrat skydd vid ytterligare anpassning för dina resurser. DDoS-skyddet blockerar attackerande trafik och vidarebefordrar återstående trafik till dess avsedda destination. Inom några minuter efter att ett angrepp har identifierats meddelas du via Azure Monitor.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![DDoS](../media-COPIED-FROM-DESIGNFORSECURITY/ddos.png)

### <a name="virtual-network-security"></a>Säkerhet för virtuella nätverk

I ett virtuellt nätverk (VNet) är det viktigt att begränsa kommunikationen mellan resurser till det som krävs.

Vid kommunikation mellan virtuella datorer är nätverkssäkerhetsgrupper (NSG) en viktig del i att begränsa onödig kommunikation. De visar en lista med tillåten respektive nekad kommunikation till och från nätverksgränssnitt och undernät, samt är helt anpassningsbara.

Du kan ta bort offentlig Internetåtkomst till dina tjänster helt genom att begränsa åtkomsten till tjänstens slutpunkter. Med tjänstslutpunkter kan Azures tjänståtkomst begränsas till ditt virtuella nätverk.

### <a name="network-integration"></a>Nätverksintegrering

Det är vanligt att ha en befintlig nätverksinfrastruktur som behöver integreras för att tillhandahålla kommunikation från lokala nätverk eller för att tillhandahålla förbättrad kommunikation mellan tjänster i Azure. Det finns några viktiga sätt att hantera den här integreringen på och förbättra säkerheten för nätverket.

Anslutningar till ett virtuellt privat nätverk (VPN) är ett vanligt sätt att upprätta säkra kommunikationskanaler mellan nätverk på. En anslutning mellan virtuella Azure-nätverk och en lokal VPN-enhet är ett bra sätt för säker kommunikation mellan ditt nätverk och ditt virtuella nätverk på Azure.

Du kan använda ExpressRoute för att tillhandahålla en dedikerad privat anslutning mellan ditt nätverk och Azure. Med ExpressRoute kan du utöka ditt lokala nätverk till Microsoft-molnet över en privat anslutning som tillhandahålls av en anslutningsprovider. Med ExpressRoute kan du upprätta anslutningar till Microsofts molntjänster, till exempel Microsoft Azure, Office 365 och Dynamics 365. Detta förbättrar säkerheten för din lokala kommunikation genom att skicka den här trafiken över den privata anslutningen i stället för via Internet. Du behöver inte tillåta åtkomst till dessa tjänster för dina slutanvändare via Internet, och du kan skicka trafiken via enheterna för ytterligare trafikkontroll.

## <a name="summary"></a>Sammanfattning

En metod med flera lager för nätverkssäkerhet bidrar till att minska risken för exponering via nätverksbaserade attacker. Azure tillhandahåller flera tjänster och funktioner för att skydda dina Internetinriktade resurser, interna resurser och kommunikationen mellan lokala nätverk. De här funktionerna gör det möjligt att skapa säkra lösningar på Azure.