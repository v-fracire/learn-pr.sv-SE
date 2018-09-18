Du har ett lokalt datacenter som du planerar att behålla, men du vill använda Azure för att avlasta trafiktoppar med hjälp av virtuella datorer i Azure. Du behöver veta om du kan behålla ditt befintliga IP-adresseringsschema och dina befintliga nätverksenheter, samtidigt som all dataöverföring är säker.

## <a name="what-is-azure-virtual-networking"></a>Vad är virtuella Azure-nätverk?

Med **virtuella Azure-nätverk** kan Azure-resurser som virtuella datorer, webbappar och databaser kommunicera med varandra, användare på Internet och lokala klientdatorer. Du kan tänka på ett Azure-nätverk som en uppsättning resurser som länkar till andra Azure-resurser.

Virtuella Azure-nätverk tillhandahåller viktiga nätverksfunktioner:

- Isolering och segmentering
- Internetkommunikation
- Kommunicera mellan Azure-resurser
- Kommunikation med lokala resurser
- Dirigering av nätverkstrafik
- Filtrering av nätverkstrafik
- Anslutning till virtuella nätverk

### <a name="isolation-and-segmentation"></a>Isolering och segmentering

Med Azure kan du skapa flera isolerade virtuella nätverk. När du konfigurerar ett virtuellt nätverk definierar du ett privat IP-adressutrymme (Internet Protocol) med hjälp av antingen offentliga eller privata IP-adressintervall. Sedan kan du segmentera IP-adressutrymmet i undernät och allokera en del av det definierade adressutrymmet till varje namngivet undernät.

Som namnmatchning kan du använda namnmatchningstjänsten som är inbyggd i Azure eller konfigurera det virtuella nätverket till att använda antingen en intern eller extern DNS-server (Domain Name System).

### <a name="internet-communications"></a>Internetkommunikation

En virtuell dator i Azure kan ansluta till Internet som standard. Du måste dock ansluta till och styra den virtuella datorn, antingen med Azure CLI, RDP (Remote Desktop Protocol) eller SSH (Secure Shell). Du kan aktivera inkommande kommunikation genom att definiera en offentlig IP-adress eller en offentlig lastbalanserare.

### <a name="communicate-between-azure-resources"></a>Kommunicera mellan Azure-resurser

Du vill aktivera Azure-resurser att kommunicera säkert med varandra. Du kan göra det på två sätt:

- **Virtuella nätverk**
    
    Virtuella nätverk kan, förutom att ansluta till virtuella datorer, också ansluta till andra Azure-resurser, som App Service-miljöer, Azure Kubernetes Service och VM-skalningsuppsättningar för Azure.

- **Tjänstslutpunkter**
     
     Du kan använda tjänstslutpunkter för att ansluta till andra typer av Azure-resurser, som Azure SQL-databaser och lagringskonton. Med den här metoden kan du länka flera Azure-resurser till virtuella nätverk och därmed förbättra säkerheten och ge optimal routning mellan resurser.

### <a name="communicate-with-on-premises-resources"></a>Kommunikation med lokala resurser

Med virtuella Azure-nätverk kan du länka resurser i din lokala miljö och i Azure-prenumerationen, vilket skapar ett nätverk som omfattar både dina lokala och molnbaserade miljöer. Det finns tre metoder för att uppnå den här anslutningen:

- **Punkt-till-plats-VPN**

   Den här metoden fungerar som en VPN-anslutning som en dator utanför din organisation arbetar mot i företagets nätverk, förutom att den fungerar i motsatt riktning. I det här fallet initierar klientdatorn en krypterad VPN-anslutning till Azure och ansluter datorn till det virtuella Azure-nätverket.

- **Plats-till-plats-VPN** En plats-till-plats-VPN länkar din lokala VPN-enhet eller gateway till Azure VPN-gatewayen i ett virtuellt nätverk. Enheter i Azure kan i praktiken visas som om de fanns i det lokala nätverket. Anslutningen är krypterad och fungerar över Internet.

- **Azure ExpressRoute**

    För miljöer där du behöver större bandbredd och ännu högre säkerhetsnivåer är Azure ExpressRoute den bästa metoden. Azure ExpressRoute ger en dedikerad privat anslutning till Azure som inte överförs via Internet.

### <a name="route-network-traffic"></a>Dirigering av nätverkstrafik

Azure dirigerar som standard trafik mellan undernät i alla anslutna virtuella nätverk, lokala nätverk och internet. Du kan dock styra dirigeringen och åsidosätta inställningarna enligt följande:

- **Routningstabeller**

    Med en routningstabell kan du definiera regler för hur trafiken ska dirigeras. Du kan skapa anpassade dirigeringstabeller som styr hur paket dirigeras mellan undernät.

- **Border Gateway Protocol**

    Border Gateway Protocol (BGP) används med Azure VPN-gatewayer eller ExpressRoute för att sprida lokala BGP-vägar till virtuella Azure-nätverk.

### <a name="filter-network-traffic"></a>Filtrering av nätverkstrafik

Med virtuella Azure-nätverk kan du filtrera trafik mellan undernät med hjälp av följande metoder:

- **Nätverkssäkerhetsgrupper**

    En nätverkssäkerhetsgrupp är en Azure-resurs som kan innehålla flera inkommande och utgående säkerhetsregler. Du kan definiera dessa regler för att tillåta eller blockera trafik, baserat på faktorer som IP-adress för källa och destination, port och protokoll.

- **Virtuella nätverksinstallationer**
    
    En virtuell nätverksinstallation är en specialiserad virtuell dator som kan jämföras med en förstärkt nätverksenhet. En virtuell nätverksinstallation utför en viss nätverksfunktion, som att köra en brandvägg eller utföra WAN-optimering.

## <a name="connect-virtual-networks"></a>Anslutning till virtuella nätverk

Du kan länka samman virtuella nätverk med _peering_ för virtuella nätverk. Peering aktiverar resurser i varje virtuellt nätverk så att de kan kommunicera med varandra. Dessa virtuella nätverk kan finnas i olika regioner, så du kan skapa ett globalt, sammankopplat nätverk via Azure.

## <a name="azure-virtual-network-settings"></a>Inställningar för virtuella Azure-nätverk

Du kan skapa och konfigurera virtuella Azure-nätverk från Azure Portal, Azure PowerShell på den lokala datorn eller med hjälp av Azure Cloud Shell.

### <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

När du skapar ett virtuellt Azure-nätverk kan du konfigurera ett antal grundläggande inställningar. Du har också möjlighet att konfigurera avancerade inställningar, som flera undernät, DDoS-skydd (distribuerad överbelastningsattack) och tjänstslutpunkter.

![Skärmbild av Azure Portal med ett exempel på bladfälten i Skapa virtuellt nätverk.](../media/2-create-virtual-network.PNG)

Följande inställningar behöver konfigureras för ett grundläggande virtuellt nätverk:

- **Nätverksnamn**

    Det här namnet måste vara unikt i prenumerationen, men det behöver inte vara unikt i ett globalt perspektiv. Kontrollera att namnet är beskrivande samt lätt att komma ihåg och skilja från andra virtuella nätverk.

- **Adressutrymme**
    
    När du konfigurerar ett virtuellt nätverk definierar du det interna adressutrymmet i formatet CIDR (Classless Interdomain Routing). Det här adressutrymmet måste vara unikt inom din prenumeration. Det betyder att du inte kan definiera ett adressutrymme, exempelvis 10.0.0.0/24, för ett virtuellt nätverk och sedan 10.1.0.0/8 för ett annat, eftersom nätverket 10.1.0.0/8 överlappar 10.0.0.0/24. Du kan dock ha 10.0.0.0/16 och 10.1.0.0/16 osv. 
    > [!NOTE] 
    > Du kan lägga till ytterligare adressutrymmen när du har skapat det virtuella nätverket.

- **Prenumeration**

    Gäller endast om du har flera prenumerationer att välja mellan.

- **Resursgrupp**
    
    Precis som med alla andra Azure-resurser måste ett virtuellt nätverk finnas i en resursgrupp. Du kan välja en befintlig resursgrupp eller skapa en ny.
    
- **Plats**

    Välj den plats där du vill att det virtuella nätverket ska finnas.

- **Undernät**
    
    Inom varje enskilt virtuellt nätverk kan du skapa ett eller flera undernät som partitionerar det virtuella nätverkets adressutrymme. Routning mellan undernät förlitar sig sedan på standardtrafikvägar eller så kan du definiera anpassade vägar. Alternativt kan du definiera ett undernät som omfattar det virtuella nätverkets alla adressintervall.

    > [!NOTE]
    > Namn på undernät måste börja med en bokstav eller siffra, sluta med en bokstav, siffra eller ett understreck och får endast innehålla bokstäver, siffror, understreck, punkter eller bindestreck.

- **DDoS-skydd**

    Du kan välja DDoS-skydd på nivån Basic eller Standard. Standard-DDoS är en premiumtjänst. Mer information om Standard-DDoS.

- **Tjänstslutpunkter**
    
    Här kan du aktivera tjänstslutpunkter och sedan välja i listan vilka Azure-tjänstslutpunkter som du vill aktivera. Du kan välja Azure Cosmos DB, Azure Service Bus, Azure Key Vault och så vidare.

När du har konfigurerat de här inställningarna klickar du på knappen **Skapa**.

### <a name="define-additional-settings"></a>Definiera fler inställningar

När du har skapat ett virtuellt nätverk kan du definiera fler inställningar. Exempel på dessa är:

- **Nätverkssäkerhetsgrupp**
    
    Nätverkssäkerhetsgrupper har säkerhetsregler som gör det möjligt att filtrera vilken typ av nätverkstrafik som kan flöda in i och ut ur virtuella nätverk, undernät och nätverksgränssnitt. Du skapar nätverkssäkerhetsgruppen separat och associerar den sedan med det virtuella nätverket.

- **Routningstabell**

    Azure skapar automatiskt en routningstabell för varje undernät inom ett virtuellt Azure-nätverk och lägger till systemstandardvägar i tabellen. Du kan dock lägga till anpassade routningstabeller för att ändra trafiken mellan virtuella nätverk.

Du kan också ändra tjänstslutpunkterna.

![Skärmbild av Azure Portal med ett exempel på bladfälten för redigering av virtuella nätverksinställningar.](../media/2-virtual-network-additional-settings.PNG)

### <a name="configure-virtual-networks"></a>Konfigurera virtuella nätverk

När du har skapat ett virtuellt nätverk kan du ändra eventuella ytterligare inställningar på bladet Virtuella nätverk i Azure Portal. Du kan också använda PowerShell-kommandon eller CloudShell-kommandon för att göra ändringar.

![Skärmbild av Azure Portal med ett exempel på ett blad för konfiguration av ett virtuellt nätverk.](../media/2-configure-virtual-network.PNG)

Du kan sedan granska och ändra inställningarna på de underordnade bladen.
Några av dessa inställningar är:

- Adressutrymmen

    Du kan lägga till ytterligare adressutrymmen i den inledande definitionen

- Anslutna enheter

    Använd det virtuella nätverket för att ansluta datorer

- Undernät

    Lägg till ytterligare undernät

- Peering-sessioner

    Länka virtuella nätverk i peering-konfigurationer

Du kan också övervaka och felsöka virtuella nätverk eller skapa ett automationsskript för att generera det nuvarande virtuella nätverket.

## <a name="summary"></a>Sammanfattning

Virtuella nätverk är kraftfulla och mycket konfigurerbara mekanismer för att ansluta enheter i Azure. Du kan ansluta Azure-resurser till varandra eller till resurser som du har lokalt. Du kan isolera, filtrera och dirigera nätverkstrafiken, och med Azure kan du öka säkerheten där du anser att du behöver det.
