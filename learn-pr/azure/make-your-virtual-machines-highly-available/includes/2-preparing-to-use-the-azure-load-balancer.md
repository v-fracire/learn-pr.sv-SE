Anta att ditt företag vill se om Azure Load Balancer stöder Enterprise Resource Planning ERP-programmet. Programmet har ett webbgränssnitt för användare och körs på flera servrar. Varje server har en lokal kopia av ERP-databasen, som är synkroniserad över alla servrar.

Här ska vi titta på hur en belastningsutjämnare kan hjälpa dig att ge hög tillgänglighet för tjänster. Du ska identifiera skillnad mellan alternativen basic och standard load balancer och se hur du skapar en belastningsutjämnare för Azure Virtual Machines.

## <a name="what-is-load-balancing"></a>Vad är belastningsutjämning?

_Belastningsutjämning_ beskriver olika metoder för att distribuera arbetsbelastningar på flera enheter, till exempel beräkning, lagring och nätverk enheter. Målet med Utjämning av nätverksbelastning är att optimera användningen av flera resurser i effektivast möjliga användning av dessa resurser när en infrastruktur skalar du ut och för att säkerställa att tjänster behålls om vissa komponenter är inte tillgängliga.

Här ska vi titta på Azure har stöd för virtuella datorer (VM) för belastningsutjämning.

### <a name="what-is-high-availability"></a>Vad är hög tillgänglighet?

Hög tillgänglighet (HA) mäter möjligheten för ett program eller en tjänst förblir tillgängliga trots fel i alla systemkomponenter. Vi rekommenderar kommer det inte vara någon märkbar förlust av tjänsten.

Belastningsutjämning är grundläggande för leverans av hög tillgänglighet eftersom det gör att flera virtuella datorer som fungerar som en pool med servrar. Poolen kan fortsätta att tjänsten begäranden även om vissa virtuella datorer krasch eller tas offline för underhåll.

## <a name="what-is-azure-load-balancer"></a>Vad är Azure Load Balancer?

**Azure Load Balancer** är en Azure-tjänst som distribuerar inkommande begäranden mellan flera virtuella datorer i en pool. Den distribuerar inkommande nätverkstrafik mellan en uppsättning felfria virtuella datorer och på så sätt undviker alla virtuella datorer som inte kan svara.

 Azure Load Balancer fungerar på Layer-4 (TCP, UDP) i OSI-lager 7-modellen. Den kan konfigureras att support TCP och UDP Programscenarier där trafiken är inkommande till virtuella Azure-datorer, samt utgående scenarier där andra Azure-tjänster skickar TCP och UDP-trafik via virtuella Azure-datorer till externa slutpunkter.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yBWo]

## <a name="public-vs-internal-load-balancers"></a>Offentliga och interna belastningsutjämnare

En Azure Load Balancer kan vara antingen _offentliga_ eller _interna_ beroende på källan för inkommande begäranden.

En **offentlig belastningsutjämnare** hanterar klientförfrågningar från utanför Azure-infrastrukturen. Den offentliga IP-adressen för belastningsutjämnaren konfigureras automatiskt som belastningsutjämnarens klientdel när du skapar den offentliga IP-Adressen och belastningsutjämningsresursen. Följande bild visar en offentlig belastningsutjämnare.

![En bild som visar en offentlig belastningsutjämnare distribuerar klientbegäranden från internet till tre virtuella datorer i ett virtuellt nätverk.](../media/2-public-load-balancer.png)

En **intern belastningsutjämnare** bearbetar begäranden från inom ett virtuellt nätverk (eller via en VPN-anslutning). Den fördelar begäranden till resurser i det virtuella nätverket. Belastningsutjämnaren, frontend IP-adresser och virtuella nätverk är inte direkt åtkomliga från internet. Följande bild visar en arkitektur som innehåller både en offentliga och interna belastningsutjämnare. Offentlig load balancer hanterar förfrågningar för externa medan den interna belastningsutjämnaren vidarebefordrar begäranden till den interna virtuella datorer och databaser för bearbetning.

![En bild som visar en offentlig belastningsutjämnare vidarebefordra klientbegäranden till en intern belastningsutjämnare. Den interna belastningsutjämnaren distribuerar sedan begäranden till ett undernät på webbnivå eller undernätet för databasnivån baserat på vilken typ av begäran. Både undernät på webbnivå och undernätet för databasnivån har flera servrar för att hantera begäranden.](../media/2-internal-load-balancer.png)

## <a name="how-does-azure-load-balancer-work"></a>Hur fungerar Azure Load Balancer?

Azure Load Balancer använder information som konfigurerats i **regler** och **hälsoavsökningar** att avgöra hur nya inkommande trafik som tas emot för en belastningsutjämnare **klientdelen** är distribueras till VM-instanser i en **backend-poolen**.

### <a name="front-end"></a>Klientdel

Klientsidan belastningsutjämnare är en IP-konfiguration, som innehåller en eller flera offentliga IP-adresser, som ger åtkomst till belastningsutjämnaren och dess program via Internet.

### <a name="back-end-address-pool"></a>Backend adresspool

Virtuella datorer ansluta till en belastningsutjämnare med hjälp av sina virtuella nätverkskort (vNIC). Backend-adresspoolen innehåller IP-adresserna för de virtuella nätverkskort som är anslutna till belastningsutjämnaren. Du kan använda detta att enkelt lägga till dina virtuella datorer till en backend-pool när du konfigurerar belastningsutjämnaren om du placerar dina virtuella datorer i en tillgänglighetsuppsättning.

### <a name="health-probe"></a>Hälsoavsökning

Läsa in belastningsutjämnare används _hälsoavsökningar_ att avgöra vilka virtuella datorer kan betjäna begäranden. Belastningsutjämnaren kommer endast att distribuera trafik till virtuella datorer som är tillgänglig och fungerar. 

En hälsoavsökning övervakar specifika portar på varje virtuell dator. Du kan definiera vilken typ av svar som motsvarar ”hälsa”. Du kan till exempel kräva ett `HTTP 200 Available` svar från ett webbprogram. Som standard markeras en virtuell dator som ”ej tillgänglig” när du har två efterföljande fel med 15-sekunder intervall.

### <a name="load-balancer-rules"></a>Regler för belastningsutjämnaren

Belastningsutjämnare _regler_ definiera hur trafiken ska distribueras till backend-virtuella datorer. Målet är ganska distribueras begäranden till felfria virtuella datorer i backend-poolen.

Azure belastningsutjämnare använder en algoritm för hash-baserade skriva om huvuden inkommande paket. Som standard skapar belastningsutjämnaren en hash från:

- Källans IP-adresser
- Källportar
- Mål-IP-adresser
- Målportar
- IP-protokollnummer

Den här mekanismen säkerställer att alla paket i en client paketflödet skickas till samma backend-VM-instans. Ett nytt flöde från en klient använder en annan slumpmässigt tilldelade källport. Detta innebär att hashen att ändras och belastningsutjämnaren kan skicka det här flödet till en annan backend-slutpunkt.

## <a name="basic-vs-standard-load-balancer-skus"></a>Grundläggande och Standard Load Balancer SKU: er

Det finns två versioner av Azure Load Balancer: **grundläggande** och **Standard**. De skiljer sig åt i skala, funktioner och priser. Exempel:

- Standard stöds HTTPS men Basic inte
- Poolstorlek kan vara mycket högre i Standard
- Basic är kostnadsfria, medan Standard debiteras baserat på regler och dataflöde.

Standard är en supermängd Basic, så alla scenarier som är lämplig för Basic bör också fungera på Standard. Grundläggande SKU: N är vanligtvis avsedd för prototyper och testa medan Standard rekommenderas för produktionsarbetsbelastningar.

## <a name="start-the-deployment-of-a-basic-public-load-balancer"></a>Starta distribution av en offentlig grundläggande belastningsutjämnare

För att skapa ett system för Utjämning av nätverksbelastning VM, behöver du skapar belastningsutjämnaren, skapa ett virtuellt nätverk för att innehålla dina virtuella datorer och sedan lägga till virtuella datorer till det virtuella nätverket.

Om du vill skapa belastningsutjämnaren med hjälp av Azure portal, definierar du följande:

- Namnet på belastningsutjämnaren
- Typ: offentlig eller intern
- SKU: Basic eller Standard
- Offentlig IP-adress: dynamisk eller statisk
- Resursgrupp och plats

Dina backend-virtuella datorer kommer alla vara ansluten till samma virtuella nätverk, så måste du konfigurera den här resursen bredvid:

- Namn på virtuellt nätverk
- -Adressutrymmet som ska användas, till exempel 172.20.0.0/16
- Resursgrupp
- Namn för undernätet för att använda
- Adressutrymme för undernätet (måste vara inom det största utrymmet), till exempel 172.20.0.0/24

Du måste skapa och distribuera dina backend-virtuella datorer och konfigurera dem att använda ditt virtuella nätverk. Du bör också placera dina virtuella datorer i samma tillgänglighetsuppsättning. Tillgänglighetsuppsättningar definiera nivå av feltolerans för en grupp med virtuella datorer, men belastningsutjämning, de också hjälper dig att tilldela dina virtuella datorer till backend-adresspooler.

Du har nu sett hur du använder Azure Load Balancer som en del av en lösning för hög tillgänglighet. Sedan använder de här stegen för att distribuera en egen belastningsutjämnare.