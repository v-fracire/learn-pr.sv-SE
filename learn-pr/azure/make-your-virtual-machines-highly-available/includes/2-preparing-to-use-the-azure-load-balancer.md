Anta att ditt företag vill se om Azure Load Balancer stöder ditt ERP-program (Enterprise Resource Planning). Programmet har ett webbgränssnitt för användare och körs på flera servrar. Varje server har en lokal kopia av ERP-databasen som är synkroniserad över alla servrar.

Här tittar vi på hur en lastbalanserare kan hjälpa dig att ge hög tillgänglighet för tjänster. Du identifierar skillnaden mellan alternativen Basic- och Standard-lastbalanserare och ser hur du skapar en lastbalanserare för virtuella Azure-datorer.

## <a name="what-is-load-balancing"></a>Vad är belastningsutjämning?

_Belastningsutjämning_ beskriver olika metoder för att distribuera arbetsbelastningar på flera enheter, till exempel beräkning, lagring och nätverk. Målet med belastningsutjämning är att optimera användningen av flera resurser, att använda dessa resurser på det effektivaste sättet när en infrastruktur skalas ut samt att säkerställa att tjänster bevaras om vissa komponenter är inte tillgängliga.

Här går vi igenom stödet för belastningsutjämning för virtuella datorer (VM) i Azure.

### <a name="what-is-high-availability"></a>Vad är hög tillgänglighet?

Hög tillgänglighet (HA) mäter möjligheten för program eller tjänster att förbli tillgängliga även om det uppstår fel i någon systemkomponent. I det ideala fallet sker ingen märkbar tjänstförlust.

Belastningsutjämning är av stor vikt för leverans av hög tillgänglighet eftersom det gör att flera virtuella datorer fungerar som en pool med servrar. Poolen kan fortsätta behandla begäranden även om några av de virtuella datorerna kraschar eller tas offline för underhåll.

## <a name="what-is-the-azure-load-balancer"></a>Vad är Azure Load Balancer?

**Azure Load Balancer** är en Azure-tjänst som distribuerar inkommande begäranden mellan flera virtuella datorer i en pool. Den distribuerar inkommande nätverkstrafik mellan en uppsättning felfria virtuella datorer och undviker alla virtuella datorer som inte kan svara.

Azure Load Balancer fungerar på lager 4 (TCP, UDP) i OSI:s 7-lagermodell. Den kan konfigureras att stödja TCP- och UDP-tillämpningsscenarier där trafiken är inkommande till virtuella Azure-datorer samt utgående scenarier där andra Azure-tjänster skickar TCP- och UDP-trafik via virtuella Azure-datorer till externa slutpunkter.

## <a name="public-vs-internal-load-balancers"></a>Offentliga kontra interna lastbalanserare

En Azure Load Balancer kan vara antingen _offentlig_ eller _intern_ beroende på källan för inkommande begäranden.

En **offentlig lastbalanserare** hanterar klientbegäranden som härrör utanför Azure-infrastrukturen. Lastbalanserarens offentliga IP-adress konfigureras automatiskt som lastbalanserarens serverdel när du skapar den offentliga IP-adressen och lastbalanserarens resurs. Följande bild visar en offentlig lastbalanserare.

![En bild som visar en offentlig lastbalanserare som distribuerar klientbegäranden från Internet till tre virtuella datorer i ett virtuellt nätverk.](../media-draft/2-public-load-balancer.png)

En **intern lastbalanserare** bearbetar begäranden som härrör inifrån ett virtuellt nätverk (eller via ett VPN). Den distribuerar begäranden till resurser inom det virtuella nätverket. Lastbalanseraren, klientdels-IP-adresser och virtuella nätverk är inte direkt åtkomliga från Internet. Följande bild visar en arkitektur som innehåller både en offentlig och en intern lastbalanserare. Den offentliga lastbalanseraren hanterar externa begäranden medan den interna lastbalanseraren vidarebefordrar begärandena till de interna virtuella datorerna och databaserna för bearbetning.

![En bild som visar en offentlig lastbalanserare som vidarebefordrar klientbegäranden till en intern lastbalanserare. Den interna lastbalanseraren distribuerar sedan begäranden till ett undernät på webbnivå eller ett undernät på databasnivån baserat på typen av begäran. Både undernätet på webbnivå och undernätet på databasnivå har flera servrar för att hantera begäranden.](../media-draft/2-internal-load-balancer.png)

## <a name="how-does-the-azure-load-balancer-work"></a>Hur fungerar Azure Load Balancer?

Azure Load Balancer använder information som konfigureras i **regler** och **hälsoavsökningar** för att avgöra hur ny inkommande trafik som tas emot i lastbalanserarens **klientdel** ska distribueras till VM-instanser i en **serverdelspool**.

### <a name="frontend"></a>Klientdel

Lastbalanserarens klientdel är en IP-konfiguration som innehåller en eller flera offentliga IP-adresser som ger åtkomst till lastbalanseraren och dess program via Internet.

### <a name="backend-address-pool"></a>Serverdelsadresspool

Virtuella datorer ansluter till en lastbalanserare med ett virtuellt nätverkskort (vNIC). Serverdelsadresspoolen innehåller IP-adresserna för de virtuella nätverkskort som är anslutna till lastbalanseraren. Om du placerar alla dina virtuella datorer i en tillgänglighetsuppsättning kan du använda detta för att enkelt lägga till de virtuella datorerna i en serverdelspool när du konfigurerar lastbalanseraren.

### <a name="health-probe"></a>Hälsoavsökning

Lastbalanserare använder _hälsoavsökningar_ för att avgöra vilka virtuella datorer som kan bearbeta begäranden. Lastbalanseraren distribuerar endast trafik till virtuella datorer som är tillgängliga och i drift. 

En hälsoavsökning övervakar specifika portar på varje virtuell dator. Du kan definiera vilken typ av svar som motsvarar ”hälsa”. Du kan till exempel kräva ett `HTTP 200 Available`-svar från ett webbprogram. Som standard markeras en virtuell dator som ”ej tillgänglig” efter två fel i rad med intervall om 15 sekunder.

### <a name="load-balancer-rules"></a>Regler för lastbalanserare

_Regler_ för lastbalanserare definierar hur trafik ska distribueras till de virtuella serverdelsdatorerna. Målet är att distribuera begäranden på ett jämnt sätt över de felfria virtuella datorerna i serverdelspoolen.

Azure Load Balancer använder en hash-baserad algoritm för att skriva om huvudena i inkommande paket. Som standard skapar lastbalanseraren en hash från:

- källans IP-adress
- källporten
- mål-IP-adressen
- målporten
- IP-protokollnumret

Den här mekanismen säkerställer att alla paket i ett paketklientflöde skickas till samma VM-serverdelsinstans. Ett nytt flöde från en klient använder en annan slumpmässigt allokerad källport. Därför ändras hashen, och lastbalanseraren kan skicka detta flöde till en annan serverdelsslutpunkt.

## <a name="basic-vs-standard-load-balancer-skus"></a>Basic- kontra Standard-lastbalanser-SKU:er

Det finns två typer av Azure-lastbalanserare: **Basic** och **Standard**. De skiljer sig åt vad gäller skala, funktioner och priser. Exempel:

- Standard stöder HTTPS, men det gör inte Basic
- Poolstorleken kan vara mycket högre i Standard
- Basic är kostnadsfri medan Standard debiteras baserat på regler och dataflöde.

Standard är en överordnad uppsättning av Basic, så alla scenarier som är lämpliga för Basic bör även fungera med Standard. Basic-SKU:n är generellt avsedd för prototyper och testning medan Standard rekommenderas för produktion.

## <a name="start-the-deployment-of-a-basic-public-load-balancer"></a>Starta distributionen av en offentlig Basic-lastbalanserare

För att skapa ett belastningsutjämnat VM-system behöver du skapa själva lastbalanseraren, skapa ett virtuellt nätverk som ska innehålla dina virtuella datorer och sedan lägga till de virtuella datorerna till det virtuella nätverket.

För att skapa lastbalanseraren med hjälp av Azure-portalen definierar du:

- Lastbalanserarens namn
- Typ: offentlig eller intern
- SKU: Basic eller Standard
- Offentlig IP-adress: dynamisk eller statisk
- Resursgrupp och plats

De virtuella serverdelsdatorerna kommer alla att vara anslutna till samma virtuella nätverk (vNET), så nästa steg är att konfigurera den här resursen:

- Namn på vNET
- Det adressutrymme som ska användas, till exempel 172.20.0.0/16
- Resursgrupp
- Namn för det undernät som ska användas
- Adressutrymmet för undernätet (måste vara inom huvudutrymmet), till exempel 172.20.0.0/24

Sedan behöver du skapa och distribuera dina virtuella serverdelsdatorer och konfigurera dem att till använda ditt virtuella nätverk. Du bör även placera dina virtuella datorer i samma tillgänglighetsuppsättning. Tillgänglighetsuppsättningar definierar nivån av feltolerans för en grupp med virtuella datorer, men för belastningsutjämning hjälper de dig även att tilldela dina virtuella datorer till serverdelspooler.

Du har nu sett hur du använder en Azure Load Balancer som en del av en lösning för hög tillgänglighet. Härnäst använder du dessa steg för att distribuera din egen lastbalanserare.