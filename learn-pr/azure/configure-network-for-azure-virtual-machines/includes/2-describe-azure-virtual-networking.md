<span data-ttu-id="d93c8-101">Du har ett lokalt datacenter som du planerar att behålla, men du vill använda Azure för att avlasta trafiktoppar med hjälp av virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="d93c8-101">You have an on-premises datacenter that you plan to keep, but you want to use Azure to offload peak traffic using virtual machines (VMs) hosted in Azure.</span></span> <span data-ttu-id="d93c8-102">Du behöver veta om du kan behålla ditt befintliga IP-adresseringsschema och dina befintliga nätverksenheter, samtidigt som all dataöverföring är säker.</span><span class="sxs-lookup"><span data-stu-id="d93c8-102">You need to know if you can keep your existing IP addressing scheme and network appliances, while ensuring that any data transfer is secure.</span></span>

## <a name="what-is-azure-virtual-networking"></a><span data-ttu-id="d93c8-103">Vad är virtuella Azure-nätverk?</span><span class="sxs-lookup"><span data-stu-id="d93c8-103">What is Azure virtual networking?</span></span>

<span data-ttu-id="d93c8-104">Med **virtuella Azure-nätverk** kan Azure-resurser som virtuella datorer, webbappar och databaser kommunicera med varandra, användare på Internet och lokala klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="d93c8-104">**Azure virtual networks** enable Azure resources, such as virtual machines, web apps, and databases, to communicate with: each other, users on the internet, and on-premises client computers.</span></span> <span data-ttu-id="d93c8-105">Du kan tänka på ett Azure-nätverk som en uppsättning resurser som länkar till andra Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="d93c8-105">You can think of an Azure network as a set of resources that links other Azure resources.</span></span>

<span data-ttu-id="d93c8-106">Virtuella Azure-nätverk tillhandahåller viktiga nätverksfunktioner:</span><span class="sxs-lookup"><span data-stu-id="d93c8-106">Azure virtual networks provide key networking capabilities:</span></span>

- <span data-ttu-id="d93c8-107">Isolering och segmentering</span><span class="sxs-lookup"><span data-stu-id="d93c8-107">Isolation and segmentation</span></span>
- <span data-ttu-id="d93c8-108">Internetkommunikation</span><span class="sxs-lookup"><span data-stu-id="d93c8-108">Internet communications</span></span>
- <span data-ttu-id="d93c8-109">Kommunicera mellan Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="d93c8-109">Communicate between Azure resources</span></span>
- <span data-ttu-id="d93c8-110">Kommunikation med lokala resurser</span><span class="sxs-lookup"><span data-stu-id="d93c8-110">Communicate with on-premises resources</span></span>
- <span data-ttu-id="d93c8-111">Dirigering av nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="d93c8-111">Route network traffic</span></span>
- <span data-ttu-id="d93c8-112">Filtrering av nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="d93c8-112">Filter network traffic</span></span>
- <span data-ttu-id="d93c8-113">Anslutning till virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="d93c8-113">Connect virtual networks</span></span>

### <a name="isolation-and-segmentation"></a><span data-ttu-id="d93c8-114">Isolering och segmentering</span><span class="sxs-lookup"><span data-stu-id="d93c8-114">Isolation and segmentation</span></span>

<span data-ttu-id="d93c8-115">Med Azure kan du skapa flera isolerade virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d93c8-115">Azure allows you to create multiple isolated virtual networks.</span></span> <span data-ttu-id="d93c8-116">När du konfigurerar ett virtuellt nätverk definierar du ett privat IP-adressutrymme (Internet Protocol) med hjälp av antingen offentliga eller privata IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="d93c8-116">When you set up a virtual network, you define a private Internet Protocol (IP) address space, using either public or private IP address ranges.</span></span> <span data-ttu-id="d93c8-117">Sedan kan du segmentera IP-adressutrymmet i undernät och allokera en del av det definierade adressutrymmet till varje namngivet undernät.</span><span class="sxs-lookup"><span data-stu-id="d93c8-117">You can then segment that IP address space into subnets, and allocate part of the defined address space to each named subnet.</span></span>

<span data-ttu-id="d93c8-118">Som namnmatchning kan du använda namnmatchningstjänsten som är inbyggd i Azure eller konfigurera det virtuella nätverket till att använda antingen en intern eller extern DNS-server (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="d93c8-118">For name resolution, you can use the name resolution service that's built in to Azure, or you can configure the virtual network to use either an internal or an external Domain Name System (DNS) server.</span></span>

### <a name="internet-communications"></a><span data-ttu-id="d93c8-119">Internetkommunikation</span><span class="sxs-lookup"><span data-stu-id="d93c8-119">Internet communications</span></span>

<span data-ttu-id="d93c8-120">En virtuell dator i Azure kan ansluta till Internet som standard.</span><span class="sxs-lookup"><span data-stu-id="d93c8-120">A VM in Azure can connect to the internet by default.</span></span> <span data-ttu-id="d93c8-121">Du måste dock ansluta till och styra den virtuella datorn, antingen med Azure CLI, RDP (Remote Desktop Protocol) eller SSH (Secure Shell).</span><span class="sxs-lookup"><span data-stu-id="d93c8-121">However, you need to connect to and control that VM, with either the Azure CLI, Remote Desktop Protocol (RDP), or Secure Shell (SSH).</span></span> <span data-ttu-id="d93c8-122">Du kan aktivera inkommande kommunikation genom att definiera en offentlig IP-adress eller en offentlig lastbalanserare.</span><span class="sxs-lookup"><span data-stu-id="d93c8-122">You can enable incoming communications by defining a public IP address or a public load balancer.</span></span>

### <a name="communicate-between-azure-resources"></a><span data-ttu-id="d93c8-123">Kommunicera mellan Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="d93c8-123">Communicate between Azure resources</span></span>

<span data-ttu-id="d93c8-124">Du vill aktivera Azure-resurser att kommunicera säkert med varandra.</span><span class="sxs-lookup"><span data-stu-id="d93c8-124">You will want to enable Azure resources to communicate securely with each other.</span></span> <span data-ttu-id="d93c8-125">Du kan göra det på två sätt:</span><span class="sxs-lookup"><span data-stu-id="d93c8-125">You can do that in one of two ways:</span></span>

- <span data-ttu-id="d93c8-126">**Virtuella nätverk**</span><span class="sxs-lookup"><span data-stu-id="d93c8-126">**Virtual networks**</span></span>
    
    <span data-ttu-id="d93c8-127">Virtuella nätverk kan, förutom att ansluta till virtuella datorer, också ansluta till andra Azure-resurser, som App Service-miljöer, Azure Kubernetes Service och VM-skalningsuppsättningar för Azure.</span><span class="sxs-lookup"><span data-stu-id="d93c8-127">Virtual networks can connect not only VMs, but other Azure resources, such as the App Service Environment, Azure Kubernetes Service, and Azure virtual machine scale sets.</span></span>

- <span data-ttu-id="d93c8-128">**Tjänstslutpunkter**</span><span class="sxs-lookup"><span data-stu-id="d93c8-128">**Service endpoints**</span></span>
     
     <span data-ttu-id="d93c8-129">Du kan använda tjänstslutpunkter för att ansluta till andra typer av Azure-resurser, som Azure SQL-databaser och lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="d93c8-129">You can use service endpoints to connect to other Azure resource types, such as Azure SQL databases and storage accounts.</span></span> <span data-ttu-id="d93c8-130">Med den här metoden kan du länka flera Azure-resurser till virtuella nätverk och därmed förbättra säkerheten och ge optimal routning mellan resurser.</span><span class="sxs-lookup"><span data-stu-id="d93c8-130">This approach enables you to link multiple Azure resources to virtual networks, thereby improving security and providing optimal routing between resources.</span></span>

### <a name="communicate-with-on-premises-resources"></a><span data-ttu-id="d93c8-131">Kommunikation med lokala resurser</span><span class="sxs-lookup"><span data-stu-id="d93c8-131">Communicate with on-premises resources</span></span>

<span data-ttu-id="d93c8-132">Med virtuella Azure-nätverk kan du länka resurser i din lokala miljö och i Azure-prenumerationen, vilket skapar ett nätverk som omfattar både dina lokala och molnbaserade miljöer.</span><span class="sxs-lookup"><span data-stu-id="d93c8-132">Azure virtual networks enable you to link resources together in your on-premises environment and within your Azure subscription, in effect creating a network that spans both your local and cloud environments.</span></span> <span data-ttu-id="d93c8-133">Det finns tre metoder för att uppnå den här anslutningen:</span><span class="sxs-lookup"><span data-stu-id="d93c8-133">There are three mechanisms for you to achieve this connectivity:</span></span>

- <span data-ttu-id="d93c8-134">**Punkt-till-plats-VPN**</span><span class="sxs-lookup"><span data-stu-id="d93c8-134">**Point-to-site VPNs**</span></span>

   <span data-ttu-id="d93c8-135">Den här metoden fungerar som en VPN-anslutning som en dator utanför din organisation arbetar mot i företagets nätverk, förutom att den fungerar i motsatt riktning.</span><span class="sxs-lookup"><span data-stu-id="d93c8-135">This approach is like a VPN connection that a computer outside your organization makes back into your corporate network, except that it's working in the opposite direction.</span></span> <span data-ttu-id="d93c8-136">I det här fallet initierar klientdatorn en krypterad VPN-anslutning till Azure och ansluter datorn till det virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="d93c8-136">In this case, the client computer initiates an encrypted VPN connection to Azure, connecting that computer to the Azure virtual network.</span></span>

- <span data-ttu-id="d93c8-137">**Plats-till-plats-VPN** En plats-till-plats-VPN länkar din lokala VPN-enhet eller gateway till Azure VPN-gatewayen i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d93c8-137">**Site-to-site VPNs** A site-to-site VPN links your on-premises VPN device or gateway to the Azure VPN gateway in a virtual network.</span></span> <span data-ttu-id="d93c8-138">Enheter i Azure kan i praktiken visas som om de fanns i det lokala nätverket.</span><span class="sxs-lookup"><span data-stu-id="d93c8-138">In effect, the devices in Azure can appear as being on the local network.</span></span> <span data-ttu-id="d93c8-139">Anslutningen är krypterad och fungerar över Internet.</span><span class="sxs-lookup"><span data-stu-id="d93c8-139">The connection is encrypted and works over the internet.</span></span>

- <span data-ttu-id="d93c8-140">**Azure ExpressRoute**</span><span class="sxs-lookup"><span data-stu-id="d93c8-140">**Azure ExpressRoute**</span></span>

    <span data-ttu-id="d93c8-141">För miljöer där du behöver större bandbredd och ännu högre säkerhetsnivåer är Azure ExpressRoute den bästa metoden.</span><span class="sxs-lookup"><span data-stu-id="d93c8-141">For environments where you need greater bandwidth and even higher levels of security, Azure ExpressRoute is the best approach.</span></span> <span data-ttu-id="d93c8-142">Azure ExpressRoute ger en dedikerad privat anslutning till Azure som inte överförs via Internet.</span><span class="sxs-lookup"><span data-stu-id="d93c8-142">Azure ExpressRoute provides dedicated private connectivity to Azure that does not travel over the internet.</span></span>

### <a name="route-network-traffic"></a><span data-ttu-id="d93c8-143">Dirigering av nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="d93c8-143">Route network traffic</span></span>

<span data-ttu-id="d93c8-144">Azure dirigerar som standard trafik mellan undernät i alla anslutna virtuella nätverk, lokala nätverk och internet.</span><span class="sxs-lookup"><span data-stu-id="d93c8-144">By default, Azure will route traffic between subnets on any connected virtual networks, on-premises networks, and the internet.</span></span> <span data-ttu-id="d93c8-145">Du kan dock styra dirigeringen och åsidosätta inställningarna enligt följande:</span><span class="sxs-lookup"><span data-stu-id="d93c8-145">However, you can control routing and override those settings as follows:</span></span>

- <span data-ttu-id="d93c8-146">**Routningstabeller**</span><span class="sxs-lookup"><span data-stu-id="d93c8-146">**Route tables**</span></span>

    <span data-ttu-id="d93c8-147">Med en routningstabell kan du definiera regler för hur trafiken ska dirigeras.</span><span class="sxs-lookup"><span data-stu-id="d93c8-147">A route table allows you to define rules as to how traffic should be directed.</span></span> <span data-ttu-id="d93c8-148">Du kan skapa anpassade dirigeringstabeller som styr hur paket dirigeras mellan undernät.</span><span class="sxs-lookup"><span data-stu-id="d93c8-148">You can create custom route tables that control how packets are routed between subnets.</span></span>

- <span data-ttu-id="d93c8-149">**Border Gateway Protocol**</span><span class="sxs-lookup"><span data-stu-id="d93c8-149">**Border Gateway Protocol**</span></span>

    <span data-ttu-id="d93c8-150">Border Gateway Protocol (BGP) används med Azure VPN-gatewayer eller ExpressRoute för att sprida lokala BGP-vägar till virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="d93c8-150">Border Gateway Protocol (BGP) works with Azure VPN gateways or ExpressRoute to propagate on-premises BGP routes to Azure virtual networks.</span></span>

### <a name="filter-network-traffic"></a><span data-ttu-id="d93c8-151">Filtrering av nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="d93c8-151">Filter network traffic</span></span>

<span data-ttu-id="d93c8-152">Med virtuella Azure-nätverk kan du filtrera trafik mellan undernät med hjälp av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="d93c8-152">Azure virtual networks enable you to filter traffic between subnets by using the following approaches:</span></span>

- <span data-ttu-id="d93c8-153">**Nätverkssäkerhetsgrupper**</span><span class="sxs-lookup"><span data-stu-id="d93c8-153">**Network security groups**</span></span>

    <span data-ttu-id="d93c8-154">En nätverkssäkerhetsgrupp är en Azure-resurs som kan innehålla flera inkommande och utgående säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="d93c8-154">A network security group is an Azure resource that can contain multiple inbound and outbound security rules.</span></span> <span data-ttu-id="d93c8-155">Du kan definiera dessa regler för att tillåta eller blockera trafik, baserat på faktorer som IP-adress för källa och destination, port och protokoll.</span><span class="sxs-lookup"><span data-stu-id="d93c8-155">You can define these rules to allow or block traffic, based on factors such as source and destination IP address, port, and protocol.</span></span>

- <span data-ttu-id="d93c8-156">**Virtuella nätverksinstallationer**</span><span class="sxs-lookup"><span data-stu-id="d93c8-156">**Network virtual appliances**</span></span>
    
    <span data-ttu-id="d93c8-157">En virtuell nätverksinstallation är en specialiserad virtuell dator som kan jämföras med en förstärkt nätverksenhet.</span><span class="sxs-lookup"><span data-stu-id="d93c8-157">A network virtual appliance is a specialized VM that can be compared to a hardened network appliance.</span></span> <span data-ttu-id="d93c8-158">En virtuell nätverksinstallation utför en viss nätverksfunktion, som att köra en brandvägg eller utföra WAN-optimering.</span><span class="sxs-lookup"><span data-stu-id="d93c8-158">A network virtual appliance carries out a particular network function, such as running a firewall or performing WAN optimization.</span></span>

## <a name="connect-virtual-networks"></a><span data-ttu-id="d93c8-159">Anslutning till virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="d93c8-159">Connect virtual networks</span></span>

<span data-ttu-id="d93c8-160">Du kan länka samman virtuella nätverk med _peering_ för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d93c8-160">You can link virtual networks together using virtual network _peering_.</span></span> <span data-ttu-id="d93c8-161">Peering aktiverar resurser i varje virtuellt nätverk så att de kan kommunicera med varandra.</span><span class="sxs-lookup"><span data-stu-id="d93c8-161">Peering enables resources in each virtual network to communicate with each other.</span></span> <span data-ttu-id="d93c8-162">Dessa virtuella nätverk kan finnas i olika regioner, så du kan skapa ett globalt, sammankopplat nätverk via Azure.</span><span class="sxs-lookup"><span data-stu-id="d93c8-162">These virtual networks can be in separate regions, allowing you to create a global interconnected network through Azure.</span></span>

## <a name="azure-virtual-network-settings"></a><span data-ttu-id="d93c8-163">Inställningar för virtuella Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="d93c8-163">Azure virtual network settings</span></span>

<span data-ttu-id="d93c8-164">Du kan skapa och konfigurera virtuella Azure-nätverk från Azure Portal, Azure PowerShell på den lokala datorn eller med hjälp av Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="d93c8-164">You can create and configure Azure virtual networks from the Azure portal, Azure PowerShell on your local computer, or Azure Cloud Shell.</span></span>

### <a name="create-a-virtual-network"></a><span data-ttu-id="d93c8-165">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="d93c8-165">Create a virtual network</span></span>

<span data-ttu-id="d93c8-166">När du skapar ett virtuellt Azure-nätverk kan du konfigurera ett antal grundläggande inställningar.</span><span class="sxs-lookup"><span data-stu-id="d93c8-166">When you create an Azure virtual network, you configure a number of basic settings.</span></span> <span data-ttu-id="d93c8-167">Du har också möjlighet att konfigurera avancerade inställningar, som flera undernät, DDoS-skydd (distribuerad överbelastningsattack) och tjänstslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="d93c8-167">You also have the option to configure advanced settings, such as multiple subnets, distributed denial of service (DDoS) protection, and service endpoints.</span></span>

![Skärmbild av Azure Portal med ett exempel på bladfälten i Skapa virtuellt nätverk.](../media/2-create-virtual-network.PNG)

<span data-ttu-id="d93c8-169">Följande inställningar behöver konfigureras för ett grundläggande virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="d93c8-169">Settings that you need to configure for a basic virtual network are:</span></span>

- <span data-ttu-id="d93c8-170">**Nätverksnamn**</span><span class="sxs-lookup"><span data-stu-id="d93c8-170">**Network name**</span></span>

    <span data-ttu-id="d93c8-171">Det här namnet måste vara unikt i prenumerationen, men det behöver inte vara unikt i ett globalt perspektiv.</span><span class="sxs-lookup"><span data-stu-id="d93c8-171">This name must be unique in your subscription but does not need to be globally unique.</span></span> <span data-ttu-id="d93c8-172">Kontrollera att namnet är beskrivande samt lätt att komma ihåg och skilja från andra virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d93c8-172">Make the name a descriptive one that is easy to remember and distinguish from other virtual networks.</span></span>

- <span data-ttu-id="d93c8-173">**Adressutrymme**</span><span class="sxs-lookup"><span data-stu-id="d93c8-173">**Address space**</span></span>
    
    <span data-ttu-id="d93c8-174">När du konfigurerar ett virtuellt nätverk definierar du det interna adressutrymmet i formatet CIDR (Classless Interdomain Routing).</span><span class="sxs-lookup"><span data-stu-id="d93c8-174">When you set up a virtual network, you define the internal address space in Classless Interdomain Routing (CIDR) format.</span></span> <span data-ttu-id="d93c8-175">Det här adressutrymmet måste vara unikt inom din prenumeration. Det betyder att du inte kan definiera ett adressutrymme, exempelvis 10.0.0.0/24, för ett virtuellt nätverk och sedan 10.1.0.0/8 för ett annat, eftersom nätverket 10.1.0.0/8 överlappar 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="d93c8-175">This address space needs to be unique within your subscription, so you cannot define an address space of, say, 10.0.0.0/24 for one virtual network and then 10.1.0.0/8 for another, as the 10.1.0.0/8 network will overlap 10.0.0.0/24.</span></span> <span data-ttu-id="d93c8-176">Du kan dock ha 10.0.0.0/16 och 10.1.0.0/16 osv.</span><span class="sxs-lookup"><span data-stu-id="d93c8-176">However, you can have 10.0.0.0/16 and 10.1.0.0/16, etc.</span></span> 
    > [!NOTE] 
    > <span data-ttu-id="d93c8-177">Du kan lägga till ytterligare adressutrymmen när du har skapat det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="d93c8-177">You can add address spaces after creating the virtual network.</span></span>

- <span data-ttu-id="d93c8-178">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="d93c8-178">**Subscription**</span></span>

    <span data-ttu-id="d93c8-179">Gäller endast om du har flera prenumerationer att välja mellan.</span><span class="sxs-lookup"><span data-stu-id="d93c8-179">Only applies if you have multiple subscriptions to choose from.</span></span>

- <span data-ttu-id="d93c8-180">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d93c8-180">**Resource group**</span></span>
    
    <span data-ttu-id="d93c8-181">Precis som med alla andra Azure-resurser måste ett virtuellt nätverk finnas i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d93c8-181">Like any other Azure resource, a virtual network needs to exist in a resource group.</span></span> <span data-ttu-id="d93c8-182">Du kan välja en befintlig resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="d93c8-182">You can either select an existing RG or create a new one.</span></span>
    
- <span data-ttu-id="d93c8-183">**Plats**</span><span class="sxs-lookup"><span data-stu-id="d93c8-183">**Location**</span></span>

    <span data-ttu-id="d93c8-184">Välj den plats där du vill att det virtuella nätverket ska finnas.</span><span class="sxs-lookup"><span data-stu-id="d93c8-184">Select the location where you want the virtual network to exist.</span></span>

- <span data-ttu-id="d93c8-185">**Undernät**</span><span class="sxs-lookup"><span data-stu-id="d93c8-185">**Subnet**</span></span>
    
    <span data-ttu-id="d93c8-186">Inom varje enskilt virtuellt nätverk kan du skapa ett eller flera undernät som partitionerar det virtuella nätverkets adressutrymme.</span><span class="sxs-lookup"><span data-stu-id="d93c8-186">Within each virtual network address range, you can create one or more subnets that partition the virtual network's address space.</span></span> <span data-ttu-id="d93c8-187">Routning mellan undernät förlitar sig sedan på standardtrafikvägar eller så kan du definiera anpassade vägar.</span><span class="sxs-lookup"><span data-stu-id="d93c8-187">Routing between subnets will then depend on the default traffic routes, or you can define custom routes.</span></span> <span data-ttu-id="d93c8-188">Alternativt kan du definiera ett undernät som omfattar det virtuella nätverkets alla adressintervall.</span><span class="sxs-lookup"><span data-stu-id="d93c8-188">Alternatively, you can define one subnet that encompasses all the virtual networks' address ranges.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d93c8-189">Namn på undernät måste börja med en bokstav eller siffra, sluta med en bokstav, siffra eller ett understreck och får endast innehålla bokstäver, siffror, understreck, punkter eller bindestreck.</span><span class="sxs-lookup"><span data-stu-id="d93c8-189">Subnet names must begin with a letter or number, end with a letter, number or underscore, and may contain only letters, numbers, underscores, periods, or hyphens.</span></span>

- <span data-ttu-id="d93c8-190">**DDoS-skydd**</span><span class="sxs-lookup"><span data-stu-id="d93c8-190">**DDoS protection**</span></span>

    <span data-ttu-id="d93c8-191">Du kan välja DDoS-skydd på nivån Basic eller Standard.</span><span class="sxs-lookup"><span data-stu-id="d93c8-191">You can select either Basic or Standard DDoS protection.</span></span> <span data-ttu-id="d93c8-192">Standard-DDoS är en premiumtjänst.</span><span class="sxs-lookup"><span data-stu-id="d93c8-192">Standard DDoS Protection is a premium service.</span></span> <span data-ttu-id="d93c8-193">Mer information om Standard-DDoS.</span><span class="sxs-lookup"><span data-stu-id="d93c8-193">For more information on Standard DDoS protection.</span></span>

- <span data-ttu-id="d93c8-194">**Tjänstslutpunkter**</span><span class="sxs-lookup"><span data-stu-id="d93c8-194">**Service Endpoints**</span></span>
    
    <span data-ttu-id="d93c8-195">Här kan du aktivera tjänstslutpunkter och sedan välja i listan vilka Azure-tjänstslutpunkter som du vill aktivera.</span><span class="sxs-lookup"><span data-stu-id="d93c8-195">Here, you enable service endpoints, and then select from the list which Azure service endpoints you want to enable.</span></span> <span data-ttu-id="d93c8-196">Du kan välja Azure Cosmos DB, Azure Service Bus, Azure Key Vault och så vidare.</span><span class="sxs-lookup"><span data-stu-id="d93c8-196">Options include Azure Cosmos DB, Azure Service Bus, Azure Key Vault, and so on.</span></span>

<span data-ttu-id="d93c8-197">När du har konfigurerat de här inställningarna klickar du på knappen **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d93c8-197">When you have configured these settings, click the **Create** button.</span></span>

### <a name="define-additional-settings"></a><span data-ttu-id="d93c8-198">Definiera fler inställningar</span><span class="sxs-lookup"><span data-stu-id="d93c8-198">Define additional settings</span></span>

<span data-ttu-id="d93c8-199">När du har skapat ett virtuellt nätverk kan du definiera fler inställningar.</span><span class="sxs-lookup"><span data-stu-id="d93c8-199">After creating a virtual network, you can then define further settings.</span></span> <span data-ttu-id="d93c8-200">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="d93c8-200">These include:</span></span>

- <span data-ttu-id="d93c8-201">**Nätverkssäkerhetsgrupp**</span><span class="sxs-lookup"><span data-stu-id="d93c8-201">**Network security group**</span></span>
    
    <span data-ttu-id="d93c8-202">Nätverkssäkerhetsgrupper har säkerhetsregler som gör det möjligt att filtrera vilken typ av nätverkstrafik som kan flöda in i och ut ur virtuella nätverk, undernät och nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="d93c8-202">Network security groups have security rules that enable you to filter the type of network traffic that can flow in and out of virtual network subnets and network interfaces.</span></span> <span data-ttu-id="d93c8-203">Du skapar nätverkssäkerhetsgruppen separat och associerar den sedan med det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="d93c8-203">You create the network security group separately, and then associate it with the virtual network.</span></span>

- <span data-ttu-id="d93c8-204">**Routningstabell**</span><span class="sxs-lookup"><span data-stu-id="d93c8-204">**Route table**</span></span>

    <span data-ttu-id="d93c8-205">Azure skapar automatiskt en routningstabell för varje undernät inom ett virtuellt Azure-nätverk och lägger till systemstandardvägar i tabellen.</span><span class="sxs-lookup"><span data-stu-id="d93c8-205">Azure automatically creates a route table for each subnet within an Azure virtual network and adds system default routes to the table.</span></span> <span data-ttu-id="d93c8-206">Du kan dock lägga till anpassade routningstabeller för att ändra trafiken mellan virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d93c8-206">However, you can add custom route tables to modify traffic between virtual networks.</span></span>

<span data-ttu-id="d93c8-207">Du kan också ändra tjänstslutpunkterna.</span><span class="sxs-lookup"><span data-stu-id="d93c8-207">You can also amend the service endpoints.</span></span>

![Skärmbild av Azure Portal med ett exempel på bladfälten för redigering av virtuella nätverksinställningar.](../media/2-virtual-network-additional-settings.PNG)

### <a name="configure-virtual-networks"></a><span data-ttu-id="d93c8-209">Konfigurera virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="d93c8-209">Configure virtual networks</span></span>

<span data-ttu-id="d93c8-210">När du har skapat ett virtuellt nätverk kan du ändra eventuella ytterligare inställningar på bladet Virtuella nätverk i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="d93c8-210">When you have created a virtual network, you can change any further settings from the Virtual Networks blade in the Azure portal.</span></span> <span data-ttu-id="d93c8-211">Du kan också använda PowerShell-kommandon eller CloudShell-kommandon för att göra ändringar.</span><span class="sxs-lookup"><span data-stu-id="d93c8-211">Alternatively, you can use PowerShell commands or commands in Cloud Shell to make changes.</span></span>

![Skärmbild av Azure Portal med ett exempel på ett blad för konfiguration av ett virtuellt nätverk.](../media/2-configure-virtual-network.PNG)

<span data-ttu-id="d93c8-213">Du kan sedan granska och ändra inställningarna på de underordnade bladen.</span><span class="sxs-lookup"><span data-stu-id="d93c8-213">You can then review and change settings in further sub-blades.</span></span>
<span data-ttu-id="d93c8-214">Några av dessa inställningar är:</span><span class="sxs-lookup"><span data-stu-id="d93c8-214">These settings include:</span></span>

- <span data-ttu-id="d93c8-215">Adressutrymmen</span><span class="sxs-lookup"><span data-stu-id="d93c8-215">Address spaces</span></span>

    <span data-ttu-id="d93c8-216">Du kan lägga till ytterligare adressutrymmen i den inledande definitionen</span><span class="sxs-lookup"><span data-stu-id="d93c8-216">You can add further address spaces to the initial definition</span></span>

- <span data-ttu-id="d93c8-217">Anslutna enheter</span><span class="sxs-lookup"><span data-stu-id="d93c8-217">Connected devices</span></span>

    <span data-ttu-id="d93c8-218">Använd det virtuella nätverket för att ansluta datorer</span><span class="sxs-lookup"><span data-stu-id="d93c8-218">Use the virtual network to connect machines</span></span>

- <span data-ttu-id="d93c8-219">Undernät</span><span class="sxs-lookup"><span data-stu-id="d93c8-219">Subnets</span></span>

    <span data-ttu-id="d93c8-220">Lägg till ytterligare undernät</span><span class="sxs-lookup"><span data-stu-id="d93c8-220">Add further subnets</span></span>

- <span data-ttu-id="d93c8-221">Peering-sessioner</span><span class="sxs-lookup"><span data-stu-id="d93c8-221">Peerings</span></span>

    <span data-ttu-id="d93c8-222">Länka virtuella nätverk i peering-konfigurationer</span><span class="sxs-lookup"><span data-stu-id="d93c8-222">Link virtual networks in peering arrangements</span></span>

<span data-ttu-id="d93c8-223">Du kan också övervaka och felsöka virtuella nätverk eller skapa ett automationsskript för att generera det nuvarande virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="d93c8-223">You can also monitor and troubleshoot virtual networks, or create an automation script to generate the current virtual network.</span></span>

## <a name="summary"></a><span data-ttu-id="d93c8-224">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d93c8-224">Summary</span></span>

<span data-ttu-id="d93c8-225">Virtuella nätverk är kraftfulla och mycket konfigurerbara mekanismer för att ansluta enheter i Azure.</span><span class="sxs-lookup"><span data-stu-id="d93c8-225">Virtual networks are powerful and highly configurable mechanisms for connecting entities in Azure.</span></span> <span data-ttu-id="d93c8-226">Du kan ansluta Azure-resurser till varandra eller till resurser som du har lokalt.</span><span class="sxs-lookup"><span data-stu-id="d93c8-226">You can connect Azure resources to one another or to resources you have on-premises.</span></span> <span data-ttu-id="d93c8-227">Du kan isolera, filtrera och dirigera nätverkstrafiken, och med Azure kan du öka säkerheten där du anser att du behöver det.</span><span class="sxs-lookup"><span data-stu-id="d93c8-227">You can isolate, filter and route your network traffic, and Azure allows you to increase security where you feel you need it.</span></span>
