Du har ett lokalt datacenter som du planerar att fortsätta, men du vill använda Azure för att avlasta trafiktoppar med hjälp av virtuella datorer (VM) i Azure. Du vill behålla din befintliga IP-adresser schema och nätverket enheter, samtidigt som man säkerställer att eventuell dataöverföring är säker.

## <a name="what-is-azure-virtual-networking"></a>Vad är Azure virtuellt nätverk?

**Azure-nätverk** aktivera Azure-resurser, till exempel virtuella datorer, webbappar och databaser, att kommunicera med: varje andra, användare på Internet och lokala klientdatorer. Du kan tänka på ett Azure-nätverk som en uppsättning resurser som länkar till andra Azure-resurser.

Azure virtuella nätverk tillhandahåller viktiga nätverksfunktioner:

- Isolering och segmentering
- Internetkommunikation
- Kommunicera mellan Azure-resurser
- Kommunicera med lokala resurser
- Dirigering av nätverkstrafik
- Filtrering av nätverkstrafik
- Anslutning av virtuella nätverk

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEve]

### <a name="isolation-and-segmentation"></a>Isolering och segmentering

Med Azure kan du skapa flera isolerade virtuella nätverk. När du har konfigurerat ett virtuellt nätverk kan definiera du en privat IP (Internet Protocol)-adressutrymme med hjälp av antingen offentliga eller privata IP-adressintervall. Sedan kan du segmentera IP-adressutrymmet i undernät och allokera en del av det definierade adressutrymmet till varje namngivet undernät.

Du kan använda den namnmatchningstjänst som är inbyggda i Azure för namnmatchning, eller du kan konfigurera det virtuella nätverket för att använda en intern eller en extern Domain Name System (DNS)-server.

### <a name="internet-communications"></a>Internetkommunikation

En virtuell dator i Azure kan ansluta till Internet som standard. Dock måste du ansluta till och styra den virtuella datorn med Azure CLI, Remote Desktop Protocol (RDP) eller Secure Shell (SSH). Du kan aktivera inkommande kommunikation genom att definiera en offentlig IP-adress eller en offentlig belastningsutjämnare.

### <a name="communicate-between-azure-resources"></a>Kommunicera mellan Azure-resurser

Du vill aktivera Azure-resurser att kommunicera säkert med varandra. Du kan göra det på två sätt:

- **Virtuella nätverk**
    
    Virtuella nätverk kan ansluta inte bara virtuella datorer, men andra Azure-resurser, till exempel App Service Environment, Azure Kubernetes Service och Azure VM-skalningsuppsättningar.

- **Serviceslutpunkter**
     
     Du kan använda tjänstslutpunkter för att ansluta till andra typer av Azure-resurser, till exempel Azure SQL-databaser och lagringskonton. Med den här metoden kan du länka flera Azure-resurser till virtuella nätverk och därmed förbättra säkerheten och ge optimal routning mellan resurser.

### <a name="communicate-with-on-premises-resources"></a>Kommunicera med lokala resurser

Azure-nätverk kan du länka resurser tillsammans i din lokala miljö och i din Azure-prenumeration som i praktiken skapar ett nätverk som omfattar både dina lokala och molnbaserade miljöer. Det finns tre mekanismer för att uppnå den här anslutningen:

- **Punkt-till-plats VPN-nätverk**

   Den här metoden är som en anslutning för virtuella privata nätverk (VPN) som en dator utanför din organisation gör tillbaka till företagets nätverk, förutom att det fungerar i motsatt riktning. I det här fallet initierar klientdatorn en krypterad VPN-anslutning till Azure och ansluter datorn till det virtuella Azure-nätverket.

- **Plats-till-plats virtuella privata nätverk** en plats-till-plats-VPN länkar din lokala VPN-enhet eller en gateway till Azure VPN-gateway i ett virtuellt nätverk. Enheter i Azure kan därmed visas som om de finns i det lokala nätverket. Anslutningen är krypterad och fungerar över Internet.

- **Azure ExpressRoute**

    För miljöer där du behöver större bandbredd och ännu högre säkerhetsnivåer, är det bästa sättet för Azure ExpressRoute. Azure ExpressRoute ger dedikerad privat anslutning till Azure som inte följer med dig via Internet.

### <a name="route-network-traffic"></a>Dirigering av nätverkstrafik

Som standard dirigerar Azure trafik mellan undernät på alla anslutna virtuella nätverk, lokala nätverk och Internet. Du kan dock kontrollera routningen och åsidosätta inställningarna enligt följande:

- **Routningstabeller**

    En routningstabell kan du definiera regler för hur trafiken ska dirigeras. Du kan skapa anpassade routningstabeller som styr hur paket dirigeras mellan undernät.

- **Border Gateway Protocol**

    Border Gateway Protocol (BGP) fungerar med Azure VPN gateway eller ExpressRoute för sprida lokala BGP-vägar till Azure-nätverk.

### <a name="filter-network-traffic"></a>Filtrering av nätverkstrafik

Virtuella Azure-nätverk gör det möjligt att filtrera trafik mellan undernät med hjälp av följande metoder:

- **Nätverkssäkerhetsgrupper**

    En nätverkssäkerhetsgrupp är en Azure-resurs som kan innehålla flera inkommande och utgående säkerhetsregler. Du kan definiera dessa regler så att de tillåter eller blockerar trafik, baserat på faktorer som IP-adress för källa och destination, port och protokoll.

- **Virtuella nätverksinstallationer**
    
    En virtuell nätverksinstallation är en specialiserad virtuell dator som kan jämföras med en förstärkt nätverksenhet. En virtuell nätverksinstallation utför en viss nätverksfunktion, till exempel köra en brandvägg eller utföra WAN-optimering.

## <a name="connect-virtual-networks"></a>Anslutning av virtuella nätverk

Du kan länka samman virtuella nätverk med _peering_ för virtuella nätverk. Peering aktiverar resurser i varje virtuellt nätverk så att de kan kommunicera med varandra. Dessa virtuella nätverk kan finnas i olika regioner, vilket gör att du kan skapa ett globalt, sammankopplat nätverk via Azure.

## <a name="azure-virtual-network-settings"></a>Inställningar för Azure virtuella nätverk

Du kan skapa och konfigurera Azure-nätverk från Azure-portalen, Azure PowerShell på den lokala datorn eller Azure Cloud Shell.

### <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

När du skapar ett Azure-nätverk kan konfigurera du ett antal grundläggande inställningar. Du har möjlighet att konfigurera avancerade inställningar, till exempel flera undernät, datorn service (DDoS) skydd och Tjänsteslutpunkter.

![Skärmbild av Azure-portalen som visar ett exempel på det virtuella nätverket skapa bladfält.](../media/2-create-virtual-network.PNG)

Du kan konfigurera följande inställningar för ett grundläggande virtuellt nätverk:

- **Nätverksnamn**

    Nätverksnamn måste vara unikt i din prenumeration, men behöver inte vara globalt unikt. Kontrollera namnet på en beskrivande alternativ som är lätt att komma ihåg och identifierad från andra virtuella nätverk.

- **Adressutrymme**
    
    När du har konfigurerat ett virtuellt nätverk kan definiera du adressutrymmet för interna Classless Inter-Domain Routing CIDR-format. Det här adressutrymmet måste vara unikt inom prenumerationen och andra nätverk som du ansluter till.
    
    Vi antar, du väljer ett adressutrymme 10.0.0.0/24 för ditt första virtuella nätverk. Adresser som definieras i den här adressintervall mellan 10.0.0.1 - 10.0.0.254. Du kan skapa en andra virtuell nätverks- och välj ett adressutrymme för 10.1.0.0./8. Adressen i det här adressutrymmet mellan 10.0.0.1 - 10.255.255.254. Några av adressen överlappar varandra och kan inte användas för de två virtuella nätverken.

    Du kan dock använda 10.0.0.0/16, med adresser mellan 10.0.0.1 - 10.0.255.254 och 10.1.0.0/16 med adresser som sträcker sig från 10.1.0.1 - 10.1.255.254. Du kan tilldela dessa adressutrymmen till dina virtuella nätverk eftersom det inte finns någon överlappning adress.

    > [!NOTE] 
    > Du kan lägga till adressutrymmen när du har skapat det virtuella nätverket.

- **Prenumeration**

    Gäller bara om du har flera prenumerationer kan välja bland.

- **Resursgrupp**
    
    Som alla andra Azure-resurser måste ett virtuellt nätverk finnas i en resursgrupp. Du kan välja en befintlig resursgrupp, eller så kan du skapa en ny.
    
- **Plats**

    Välj den plats där du vill att det virtuella nätverket finns.

- **Undernät**
    
    Du kan skapa en eller flera undernät som partitionerar det virtuella nätverkets adressutrymme-adressintervallet för varje virtuellt nätverk. Routning mellan undernät förlitar sig sedan på standardtrafikvägar eller så kan du definiera anpassade vägar. Du kan också definiera ett undernät som omfattar de virtuella nätverken-adressintervall.

    > [!NOTE]
    > Namn på undernät måste börja med en bokstav eller siffra, sluta med en bokstav, siffra eller ett understreck och får endast innehålla bokstäver, siffror, understreck, punkter eller bindestreck.

- **Distribuerad Överbelastningsattack (DDoS)-skydd**

    Du kan välja Basic eller Standard DDoS protection. Standard-DDoS är en premiumtjänst. Den [Azure DDoS Protection Standard](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview) ger mer information om Standard DDoS-skydd. 

- **Tjänsteslutpunkter**
    
    Här kan aktivera du Tjänsteslutpunkter och välj från listan över vilka Azure-tjänstslutpunkter som du vill aktivera. Alternativ inkluderar Azure Cosmos DB, Azure Service Bus, Azure Key Vault och så vidare.

När du har konfigurerat de här inställningarna klickar du på knappen **Skapa**.

### <a name="define-additional-settings"></a>Ange ytterligare inställningar

När du har skapat ett virtuellt nätverk kan du definiera fler inställningar. Exempel på dessa är:

- **Nätverkssäkerhetsgrupp**
    
    Nätverkssäkerhetsgrupper har säkerhetsregler som gör det möjligt att filtrera typ av nätverkstrafik som kan flödar in och ut ur virtuellt nätverk, undernät och nätverksgränssnitt. Du skapar den nya nätverkssäkerhetsgruppen separat och koppla den till det virtuella nätverket.

- **Routningstabell**

    Azure skapar automatiskt en routningstabell för varje undernät inom ett virtuellt Azure-nätverk och lägger till systemstandardvägar i tabellen. Du kan dock lägga till anpassade routningstabeller för att ändra trafik mellan virtuella nätverk.

Du kan också ändra tjänstslutpunkterna.

![Skärmbild av Azure-portalen som visar ett exempel blad för att redigera inställningar för virtuella nätverk.](../media/2-virtual-network-additional-settings.PNG)

### <a name="configure-virtual-networks"></a>Konfigurera virtuella nätverk

När du har skapat ett virtuellt nätverk kan du ändra eventuella ytterligare inställningar från bladet för virtuella nätverk i Azure Portal. Du kan också använda PowerShell-kommandon eller kommandon i Cloud Shell för att göra ändringar.

![Skärmbild av Azure-portalen som visar ett exempel blad för att konfigurera ett virtuellt nätverk.](../media/2-configure-virtual-network.PNG)

Du kan sedan granska och ändra inställningar i underordnade blad.
Några av dessa inställningar är:

- Adressutrymmen

    Du kan lägga till ytterligare adressutrymmen inledande definition

- Anslutna enheter

    Använd virtual network för att ansluta datorer

- Undernät

    Lägga till ytterligare undernät

- Peering-sessioner

    Länken virtuella nätverken i peering-åtgärder

Du kan också övervaka och felsöka virtuella nätverk eller skapa ett automationsskript för att generera det nuvarande virtuellt nätverk.

## <a name="summary"></a>Sammanfattning

Virtuella nätverk är kraftfulla och mycket konfigurerbara mekanismer för att ansluta enheter i Azure. Du kan ansluta Azure-resurser till varandra eller till resurser som du har lokalt. Du kan isolera, filtrera och dirigera trafik på nätverket och Azure kan du öka säkerhet där du anser att du behöver den.
