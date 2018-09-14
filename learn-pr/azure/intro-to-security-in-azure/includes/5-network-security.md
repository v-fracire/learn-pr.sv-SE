Att skydda ditt nätverk från attacker och obehörig åtkomst är en viktig del av alla arkitekturer. Här tittar vi på hur nätverkssäkerhet ser ut, hur du integrerar en metod med flera lager i arkitekturen samt hur Azure kan hjälpa dig att få nätverkssäkerhet för din miljö.

## <a name="a-layered-approach-to-network-security"></a>En metod för nätverkssäkerhet med flera lager

En gemensam tråd i den här modulen har varit att använda en säkerhetsmetod med flera lager, och den metoden fungerar på samma sätt vid nätverkslagret. Det räcker inte att bara fokusera på säkra perimeternätverket eller på nätverkssäkerheten mellan tjänster i ett nätverk. En metod med flera lager ger flera nivåer av skydd, så om en angripare tar sig igenom ett lager finns det ytterligare skydd på plats för att begränsa angreppet.

Vi går igenom hur Azure kan ge verktyg för en metod med flera lager för att säkra ditt nätverksfotavtryck.

### <a name="internet-protection"></a>Internetskydd

Om vi startar i utkanten av nätverket fokuserar vi på att begränsa och eliminera attacker från Internet. Ett bra första ställe att börja är att utvärdera de resurser som är mot internet och tillåter bara inkommande och utgående kommunikation vid behov. Identifiera alla resurser som tillåter inkommande nätverkstrafik oavsett typ och se till att de är nödvändiga och begränsade till endast de portar/protokoll som krävs. Azure Security Center är ett bra ställe att leta efter den här informationen eftersom den identifierar mot internet-resurser som inte har nätverkssäkerhetsgrupper som är kopplade till dem, samt resurser som inte kontrolleras bakom en brandvägg.

För att ge inkommande skydd på perimeternätverket måste ha du ett par alternativ:

* Azure Application Gateway är en belastningsutjämnare som innehåller en brandvägg för webbaserade program som ger skydd mot vanliga, kända säkerhetsrisker.

* Virtuella nätverksinstallationer (Nva) kan användas för HTTP-tjänster eller avancerade konfigurationer. Nva: erna liknar maskinvaruinstallationer för brandväggen.


Alla resurser som är exponerade mot internet är risk för angrepp en denial of service-attack. Dessa typer av attacker försöker överbelasta en nätverksresurs genom att skicka så många begäranden att resursen blir långsam eller slutar svarar. För att lösa dessa attacker, ger Azure DDoS protection grundläggande skydd över alla Azure-tjänster och förbättrat skydd för ytterligare anpassning för dina resurser. Azure DDoS protection blockerar attack trafik och vidarebefordrar återstående trafiken till sin destination. Inom några minuter efter att ett angrepp har identifierats meddelas du via Azure Monitor-mått.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![DDoS](../media-COPIED-FROM-DESIGNFORSECURITY/ddos.png)

### <a name="virtual-network-security"></a>Säkerhet för virtuella nätverk

I ett virtuellt nätverk (VNet) är det viktigt att begränsa kommunikationen mellan resurser till det som krävs.

För kommunikation mellan virtuella datorer är nätverkssäkerhetsgrupper viktig att begränsa risken för onödig information. De ger en lista över tillåtna och nekade kommunikation till och från nätverksgränssnitt och undernät och är helt anpassningsbar.

Du kan helt ta bort offentliga Internetåtkomsten till dina tjänster genom att begränsa åtkomsten till Tjänsteslutpunkter. Med tjänstslutpunkter kan Azure-tjänståtkomst begränsas till det virtuella nätverket.

### <a name="network-integration"></a>Nätverksintegrering

Det är vanligt att ha befintliga nätverksinfrastruktur som behöver integreras att tillhandahålla kommunikation från lokala nätverk eller att tillhandahålla förbättrad kommunikation mellan tjänster i Azure. Det finns några viktiga sätt att hantera den här integreringen och förbättra säkerheten för nätverket.

Virtuella privata nätverksanslutningar (VPN) är ett vanligt sätt att upprätta säker kommunikationskanaler mellan nätverk. Anslutningen mellan Azure-nätverk och en lokal VPN-enhet är ett bra sätt att tillhandahålla säker kommunikation mellan ditt nätverk och ditt virtuella nätverk på Azure.

Du kan använda Azure ExpressRoute för att tillhandahålla en dedikerad privat anslutning mellan ditt nätverk och Azure. Med ExpressRoute kan du utöka ditt lokala nätverk till Microsoft-molnet över en privat anslutning som tillhandahålls av en anslutningsprovider. Med ExpressRoute kan du upprätta anslutningar till Microsofts molntjänster, till exempel Microsoft Azure, Office 365 och Dynamics 365. Detta förbättrar säkerheten för din lokala kommunikation genom att skicka den här trafiken över den privata anslutningen i stället för via Internet. Du behöver inte att få tillgång till dessa tjänster för dina slutanvändare via internet och youcan skicka den här trafiken via enheterna för att ytterligare trafikkontroll.

## <a name="summary"></a>Sammanfattning

En metod med flera lager för nätverkssäkerhet bidrar till att minska risken för exponering via nätverksbaserade attacker. Azure tillhandahåller flera tjänster och funktioner för att säkra din internet-ansluten resurs, interna resurser och kommunikationen mellan lokala nätverk. De här funktionerna gör det möjligt att skapa säkra lösningar på Azure.