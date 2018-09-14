Tidigare såg du hur **Azure Load Balancer** hjälper dig att uppnå hög tillgänglighet och minimera driftstopp.

Även om din e-handelswebbplats är mer tillgänglig löser det problemet med svarstider skapar inte återhämtningsförmåga i olika geografiska områden.

Hur kan du göra så att din webbplats, som finns i USA, läses in snabbare för användare i Europa eller Asien?

## <a name="what-is-network-latency"></a>Vad är svarstid för nätverk?

_Svarstid_ är den tid det tar för data att överföras via nätverket. Svarstiden mäts vanligtvis i millisekunder.

Jämför svarstid med bandbredd. Bandbredd är den mängd data som får plats på anslutningen. Svarstid är den tid det tar för data att nå sitt mål.

Faktorer såsom den typ av anslutning du använder och hur programmet är utformat kan påverka svarstiden. Men den största faktorn är kanske avståndet.

Ta svarstid din e-handelswebbplats på Azure, som är i regionen USA, östra. Det skulle vanligtvis ta kortare tid att överföra data till Atlanta (ett avstånd på cirka 640 km) än att överföra data till London (ett avstånd på cirka 6 400 km).

Din e-handelswebbplats levererar standard-HTML, CSS, JavaScript och bilder. När det förekommer många filer kan svarstiden för nätverk bli betydande. Hur kan du minska svarstiden för användare som är geografiskt långt borta?

## <a name="scale-out-to-different-regions"></a>Skala ut till olika regioner

Azure tillhandahåller datacenter i regioner över hela världen.

Beakta kostnaden för att bygga ett datacenter. Kostnaderna för utrustningen är inte den enda faktorn. Du måste ange den kraft, kylning, och personal att hålla dina system som körs på varje plats. Det kan vara orimligt dyrt att replikera hela datacentret. Men om du gör det med Azure kan kosta mycket mindre eftersom Azure har redan utrustning och personal på plats.

Ett sätt att minska svarstiden är att tillhandahålla exakta kopior av din tjänst i flera regioner. HThe som följande bild visar ett exempel på global distribution.

![En bild som visar en världskarta med tre Azure-datacenter som är markerat. Alla Datacenter är märkta med ett unikt domännamn.](../media/4-global-deployment.png)

Diagrammet visar din e-handelswebbplats som körs i tre Azure-regioner: USA, östra, Europa, norra och Asien, östra. Lägg märke till DNS-namnet för var och en. Hur kan du ansluta användare till den tjänst som ligger närmast geografiskt, men under domänen contoso.com?

## <a name="use-traffic-manager-to-route-users-to-the-closest-endpoint"></a>Använda Traffic Manager för att dirigera användare till den närmaste slutpunkten

Ett svar är **Azure Traffic Manager**. Traffic Manager använder den DNS-server som är närmast användaren för att dirigera användartrafik till en globalt distribuerad slutpunkt. Följande bild visar rollen för Traffic Manager.

![En bild som visar Azure Traffic Manager routning en begäran till närmaste datacenter. ](../media/4-traffic-manager.png)

Traffic Manager ser inte den trafik som skickas mellan klienten och servern. I stället dirigeras klientens webbläsare till en önskad slutpunkt. Traffic Manager kan dirigera trafik på ett par olika sätt, till exempel till slutpunkten med kortast svarstid.

Visas inte här, kan den här konfigurationen även innehålla den lokala distributionen som körs i Kalifornien. Du kan ansluta Traffic Manager till dina egna lokala nätverk, så att du kan underhålla dina befintliga datacenterinvesteringar. Eller så kan du flytta ditt program helt och hållet till molnet. Valet är ditt.

## <a name="compare-load-balancer-to-traffic-manager"></a>Jämför belastningsutjämnare till Traffic Manager

Azure Load Balancer distribuerar trafiken inom samma region för att göra dina tjänster mer tillgängliga och motståndskraftiga. Traffic Manager fungerar på DNS-nivå och dirigerar klienten att en önskad slutpunkt. Den här slutpunkten kan vara till den region som ligger närmast dina användare.

Läsa in belastningsutjämning och Traffic Manager både hjälper dig att göra dina tjänster mer motståndskraftig, men på olika sätt. När belastningsutjämnaren identifierar en dator som inte svarar, dirigeras trafiken till andra virtuella datorer i poolen. Traffic Manager övervakar hälsan för dina slutpunkter. När Traffic Manager hittar en slutpunkt som inte svarar dirigerar den däremot trafiken till nästa närmaste slutpunkt som svarar.

## <a name="summary"></a>Sammanfattning

Geografiskt avstånd är en av de största faktorer som bidrar till svarstid. Med Traffic Manager på plats kan du hantera exakta kopior av din tjänst i flera geografiska områden. På så sätt får användare i USA, Europa och Asien en bra upplevelse med hjälp av din e-handelsplats.
