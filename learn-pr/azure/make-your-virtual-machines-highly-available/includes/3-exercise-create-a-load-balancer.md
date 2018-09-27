<span data-ttu-id="431a1-101">Anta att du arbetar för Woodgrove Bank och att du håller på att starta onlinetjänster för bankärenden.</span><span class="sxs-lookup"><span data-stu-id="431a1-101">Recall that you work for Woodgrove Bank, and that you are about to launch online banking services.</span></span> <span data-ttu-id="431a1-102">Konkurrensen är mycket kraftig i den här sektorn, så du måste garantera minst 99,99 % tjänsttillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="431a1-102">This sector is highly competitive, so you need to guarantee of a minimum of 99.99% service availability.</span></span> <span data-ttu-id="431a1-103">Du har fastställt att en Azure Load Balancer med en pool med tre virtuella datorer uppfyller det här målet.</span><span class="sxs-lookup"><span data-stu-id="431a1-103">You have determined that Azure Load Balancer with a pool of three virtual machines will meet this goal.</span></span>

<span data-ttu-id="431a1-104">I den här övningen skapar du en lastbalanserare och ett virtuellt nätverk hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="431a1-104">In this exercise, you will create a load balancer and a virtual network using the Azure portal.</span></span> <span data-ttu-id="431a1-105">Eftersom vi bara behöver en av dessa är det enkelt att skapa den i portalen.</span><span class="sxs-lookup"><span data-stu-id="431a1-105">Since we only need one of these, the portal is an easy way to create it.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-public-load-balancer"></a><span data-ttu-id="431a1-106">Skapa en offentlig lastbalanserare</span><span class="sxs-lookup"><span data-stu-id="431a1-106">Create a public load balancer</span></span>

1. <span data-ttu-id="431a1-107">Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="431a1-107">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="431a1-108">I sidofältet klickar du på **Skapa en resurs**.</span><span class="sxs-lookup"><span data-stu-id="431a1-108">In the sidebar, click **Create a resource**.</span></span>

1. <span data-ttu-id="431a1-109">Välj avsnittet **Nätverk** och klicka sedan på **Lastbalanserare**.</span><span class="sxs-lookup"><span data-stu-id="431a1-109">Select the **Networking** section, and then click **Load Balancer**.</span></span> <span data-ttu-id="431a1-110">Du kan använda sökrutan om du inte ser det alternativet.</span><span class="sxs-lookup"><span data-stu-id="431a1-110">If you don't see that choice, you can use the search box.</span></span>

    ![Skärmbild som visar Microsoft Azure Marketplace med avsnittet Nätverk valt och Lastbalanserare markerat](../media/3-azure-marketplace.png)

1. <span data-ttu-id="431a1-112">I bladet **Skapa lastbalanserare** anger du eller väljer följande information:</span><span class="sxs-lookup"><span data-stu-id="431a1-112">In the **Create load balancer** blade, enter or select the following information:</span></span>
    - <span data-ttu-id="431a1-113">**Namn:** _woodgrove-LB_</span><span class="sxs-lookup"><span data-stu-id="431a1-113">**Name:** _woodgrove-LB_</span></span>
    - <span data-ttu-id="431a1-114">**Typ:** _Offentlig_</span><span class="sxs-lookup"><span data-stu-id="431a1-114">**Type:** _Public_</span></span>
    - <span data-ttu-id="431a1-115">**SKU:** _Grundläggande_</span><span class="sxs-lookup"><span data-stu-id="431a1-115">**SKU:** _Basic_</span></span>
    - <span data-ttu-id="431a1-116">**Offentlig IP-adress:** Välj **Skapa ny**.</span><span class="sxs-lookup"><span data-stu-id="431a1-116">**Public IP address:** Select **Create new**.</span></span> <span data-ttu-id="431a1-117">I textrutan skriver du _woodgrove-LB-ip_.</span><span class="sxs-lookup"><span data-stu-id="431a1-117">In the text box, type _woodgrove-LB-ip_.</span></span> <span data-ttu-id="431a1-118">Lämna tilldelningen som _Dynamisk_.</span><span class="sxs-lookup"><span data-stu-id="431a1-118">Leave the Assignment as _Dynamic_.</span></span>
    - <span data-ttu-id="431a1-119">**Prenumeration:** bör redan ha _Concierge-prenumeration_ valt.</span><span class="sxs-lookup"><span data-stu-id="431a1-119">**Subscription:** It should already have _Concierge Subscription_ selected.</span></span>
    - <span data-ttu-id="431a1-120">**Resursgrupp:** välj **Använd befintlig** och välj _<rgn>[Resursgruppnamn för sandbox-miljön]</rgn>_.</span><span class="sxs-lookup"><span data-stu-id="431a1-120">**Resource group:** Select **Use existing** and choose _<rgn>[sandbox resource group name]</rgn>_.</span></span>
    - <span data-ttu-id="431a1-121">**Plats:** välj en region nära dig från följande lista.</span><span class="sxs-lookup"><span data-stu-id="431a1-121">**Location:** Select a region near you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    ![Skärmbild som visar skärmen för att skapa ny lastbalanserare](../media/3-create-load-balancer.png)

1. <span data-ttu-id="431a1-123">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="431a1-123">Click **Create**.</span></span>

<span data-ttu-id="431a1-124">Medan lastbalanserarresursen skapas och distribueras ska vi skapa det Azure Virtual Network vi ska använda för serversidans undernät.</span><span class="sxs-lookup"><span data-stu-id="431a1-124">While the load balancer resource is being created and deployed, let's create the Azure Virtual Network we'll use for the backend subnet.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="431a1-125">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="431a1-125">Create a virtual network</span></span>

1. <span data-ttu-id="431a1-126">I den vänstra menyn klickar du på **Skapa en resurs**.</span><span class="sxs-lookup"><span data-stu-id="431a1-126">In the left menu, click **Create a resource**.</span></span> <span data-ttu-id="431a1-127">I bladet **Ny** klickar du på **Nätverk** och därefter på **Virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="431a1-127">In the **New** blade, click **Networking**, and then click **Virtual network**.</span></span>

    ![Skärmbild som visar Microsoft Azure Marketplace med avsnittet Nätverk valt och Lastbalanserare markerat](../media/3-azure-marketplace-2.png)

1. <span data-ttu-id="431a1-129">På bladet **Skapa virtuellt nätverk** anger eller väljer du följande information:</span><span class="sxs-lookup"><span data-stu-id="431a1-129">In the **Create virtual network** blade, enter or select the following information:</span></span>
    - <span data-ttu-id="431a1-130">**Namn:** _woodgrove-VNET_</span><span class="sxs-lookup"><span data-stu-id="431a1-130">**Name:** _woodgrove-VNET_</span></span>
    - <span data-ttu-id="431a1-131">**Adressutrymme:** _172.20.0.0/16_</span><span class="sxs-lookup"><span data-stu-id="431a1-131">**Address space:** _172.20.0.0/16_</span></span>
    - <span data-ttu-id="431a1-132">**Prenumeration:** bör redan ha _Concierge-prenumeration_ valt.</span><span class="sxs-lookup"><span data-stu-id="431a1-132">**Subscription:** It should already have _Concierge Subscription_ selected.</span></span>
    - <span data-ttu-id="431a1-133">**Resursgrupp:** välj den befintliga resursgruppen _<rgn>[Resursgruppsnamn]</rgn>_ i listan.</span><span class="sxs-lookup"><span data-stu-id="431a1-133">**Resource group:** Select the existing _<rgn>[Resource Group Name]</rgn>_ resource group from the list.</span></span>
    - <span data-ttu-id="431a1-134">**Undernät:** _backendSubnet_</span><span class="sxs-lookup"><span data-stu-id="431a1-134">**Subnet name:** _backendSubnet_</span></span>
    - <span data-ttu-id="431a1-135">**Adressintervall för undernät:** _172.20.0.0/24_</span><span class="sxs-lookup"><span data-stu-id="431a1-135">**Subnet address range:** _172.20.0.0/24_</span></span>
    - <span data-ttu-id="431a1-136">**DDoS-skydd:** _Grundläggande_</span><span class="sxs-lookup"><span data-stu-id="431a1-136">**DDoS protection:** _Basic_</span></span>
    - <span data-ttu-id="431a1-137">**Tjänstens slutpunkter:** _Inaktiverat_</span><span class="sxs-lookup"><span data-stu-id="431a1-137">**Service endpoints:** _Disabled_</span></span>

    ![Skärmbild som visar skärmen för att skapa ny lastbalanserare](../media/3-create-vnet.png)

1. <span data-ttu-id="431a1-139">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="431a1-139">Click **Create**.</span></span>

<span data-ttu-id="431a1-140">Medan det virtuella nätverket distribueras kan vi skapa ytterligare en sak: en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="431a1-140">While the virtual network is being deployed, let's create one more thing: a network security group.</span></span>

## <a name="create-and-configure-a-network-security-group"></a><span data-ttu-id="431a1-141">Skapa och konfigurera en nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="431a1-141">Create and configure a network security group</span></span>

1. <span data-ttu-id="431a1-142">Klicka på **Skapa en resurs**</span><span class="sxs-lookup"><span data-stu-id="431a1-142">Click **Create a resource**</span></span>

1. <span data-ttu-id="431a1-143">Välj gruppen **Nätverk** och klicka på objektet **Nätverkssäkerhetsgrupp**.</span><span class="sxs-lookup"><span data-stu-id="431a1-143">Select the **Networking** group, and Click on the **Network security group** item.</span></span>

    ![Skärmbild som visar Microsoft Azure Marketplace med avsnittet Nätverk valt och Nätverkssäkerhetsgrupp markerat](../media/3-azure-marketplace-3.png)


1. <span data-ttu-id="431a1-145">Ge den namnet **woodgrove-NSG** och placera den i Azure-sandbox-resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="431a1-145">Give it the name **woodgrove-NSG** and put it into the Azure sandbox resource group.</span></span>

1. <span data-ttu-id="431a1-146">Kontrollera att den finns på samma plats som Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="431a1-146">Make sure it's in the same location as the Azure Load Balancer.</span></span>

    ![Skärmbild som visar skärmen för att skapa nätverkssäkerhetsgrupp](../media/3-create-nsg.png)

1. <span data-ttu-id="431a1-148">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="431a1-148">Click **Create**.</span></span>

<span data-ttu-id="431a1-149">Vänta tills lastbalanseraren, det virtuella nätverket och NSG har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="431a1-149">Wait until the load balancer, virtual network, and NSG are deployed.</span></span> <span data-ttu-id="431a1-150">Sedan kan vi konfigurera nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="431a1-150">Then we can configure the network security.</span></span>

## <a name="configure-the-network-security-group"></a><span data-ttu-id="431a1-151">Konfigurera nätverkssäkerhetsgruppen</span><span class="sxs-lookup"><span data-stu-id="431a1-151">Configure the network security group</span></span>

1. <span data-ttu-id="431a1-152">Välj den nya nätverkssäkerhetsgruppen – antingen i distributionsmeddelandet eller via sökfältet högst upp i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="431a1-152">Select your new network security group - either from the deployment notification, or through the search bar at the top of the Azure portal.</span></span>

    ![Skärmbild som visar slutförd distribution på meddelandepanelen](../media/3-deployment-success.png)

1. <span data-ttu-id="431a1-154">Välj avsnittet **Inställningar** > **Ingående säkerhetsregler**.</span><span class="sxs-lookup"><span data-stu-id="431a1-154">Select the **Settings** > **Inbound security rules** section.</span></span> <span data-ttu-id="431a1-155">Observera att vi har tre fördefinierade regler som alltid används.</span><span class="sxs-lookup"><span data-stu-id="431a1-155">Notice that we have three pre-defined rules that are always applied.</span></span> <span data-ttu-id="431a1-156">Dessa regler är:</span><span class="sxs-lookup"><span data-stu-id="431a1-156">These rules are:</span></span>
    - <span data-ttu-id="431a1-157">**AllowVnetInbound** – tillåter all intern trafik som flödar i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="431a1-157">**AllowVnetInbound** - Allow all internal traffic flowing on the virtual network.</span></span> <span data-ttu-id="431a1-158">Den här regeln gör att virtuella datorer som delar nätverk kan kommunicera med varandra.</span><span class="sxs-lookup"><span data-stu-id="431a1-158">This rule allows VMs that share the network to talk to each other.</span></span>
    - <span data-ttu-id="431a1-159">**AllowAzureLoadBalancerInBound** – tillåter att lastbalanseraren ”pingar” tjänster i nätverket för att se om de är aktiva.</span><span class="sxs-lookup"><span data-stu-id="431a1-159">**AllowAzureLoadBalancerInBound** - Allow the load balancer to "ping" services on the network to see whether they are alive.</span></span>
    - <span data-ttu-id="431a1-160">**DenyAllInbound** – nekar all annan trafik.</span><span class="sxs-lookup"><span data-stu-id="431a1-160">**DenyAllInbound** - Deny all other traffic.</span></span>

    <span data-ttu-id="431a1-161">Regeln **DenyAllInbound** är särskilt viktig – den säkerställer att all ingående trafik som inte har en regel med högre prioritet blockeras.</span><span class="sxs-lookup"><span data-stu-id="431a1-161">The **DenyAllInbound** rule is particularly important - it ensures that all inbound traffic that doesn't have a higher priority rule is blocked.</span></span> <span data-ttu-id="431a1-162">Det är därför vi måste lägga till en ny regel som tillåter HTTP (80)-trafik på våra webbservrar.</span><span class="sxs-lookup"><span data-stu-id="431a1-162">That's why we will need to add a new rule to allow HTTP (80) traffic for our web servers.</span></span>

    > [!NOTE]
    > <span data-ttu-id="431a1-163">Prioritet i NSG-regler går från 0–65500 och reglerna utvärderas i ordning.</span><span class="sxs-lookup"><span data-stu-id="431a1-163">Priority in NSG rules goes from 0 - 65500 and rules are evaluated in order.</span></span> <span data-ttu-id="431a1-164">Den första matchande regeln är avgörande.</span><span class="sxs-lookup"><span data-stu-id="431a1-164">The first matching rule becomes the decision maker.</span></span> <span data-ttu-id="431a1-165">Du vill alltid placera dina regler relativt lågt – med start vid cirka 1000 så att de har företräde framför de fördefinierade.</span><span class="sxs-lookup"><span data-stu-id="431a1-165">You will always want to place your rules fairly low - starting around 1000 so they take precedence over the pre-defined ones.</span></span>

1. <span data-ttu-id="431a1-166">Klicka på **Lägg till** för att lägga till en ny regel.</span><span class="sxs-lookup"><span data-stu-id="431a1-166">Click **Add** to add a new rule.</span></span>

    ![Skärmbild som visar ingående nätverkssäkerhetsregler med knappen Lägg till markerad](../media/3-inbound-security-rules.png)

1. <span data-ttu-id="431a1-168">Klicka på knappen **Basic** längst upp för att växla till vyn ”Basic”.</span><span class="sxs-lookup"><span data-stu-id="431a1-168">Click the **Basic** button at the top to switch the the "basic" view.</span></span>

1. <span data-ttu-id="431a1-169">Ange uppgifterna för den nya regeln.</span><span class="sxs-lookup"><span data-stu-id="431a1-169">Fill in the details for the new rule.</span></span>
    - <span data-ttu-id="431a1-170">Välj _HTTP_ för **Tjänst**.</span><span class="sxs-lookup"><span data-stu-id="431a1-170">Select _HTTP_ for the **Service**.</span></span>
    - <span data-ttu-id="431a1-171">Ange **Prioritet** till _1000_.</span><span class="sxs-lookup"><span data-stu-id="431a1-171">Set the **Priority** to _1000_.</span></span>
    - <span data-ttu-id="431a1-172">Ge regeln ett namn (eller låt standardnamnet stå kvar).</span><span class="sxs-lookup"><span data-stu-id="431a1-172">Give the rule a name (or leave the default).</span></span>

    ![Skärmbild som visar dialogrutan Lägg till ingående säkerhetsregel ifylld med HTTP-information](../media/3-add-inbound-rule.png)

1. <span data-ttu-id="431a1-174">Klicka på **Lägg till** för att lägga till regeln.</span><span class="sxs-lookup"><span data-stu-id="431a1-174">Click **Add** to add the rule.</span></span>

    ![Skärmbild som visar den nyligen tillagda HTTP-regeln i listan NSG-regler](../media/3-new-added-rule.png)

## <a name="apply-the-network-security-group-to-the-vnet"></a><span data-ttu-id="431a1-176">Tillämpa nätverkssäkerhetsgruppen på VNet</span><span class="sxs-lookup"><span data-stu-id="431a1-176">Apply the network security group to the VNet</span></span>

<span data-ttu-id="431a1-177">Nu ska vi tillämpa denna NSG på det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="431a1-177">Next, let's apply this NSG to the virtual network.</span></span>

1. <span data-ttu-id="431a1-178">Leta reda på det virtuella nätverket – du kan använda sökrutan överst.</span><span class="sxs-lookup"><span data-stu-id="431a1-178">Find your virtual network - you can use the search box at the top.</span></span> <span data-ttu-id="431a1-179">Namnet är **woodgrove-VNET**.</span><span class="sxs-lookup"><span data-stu-id="431a1-179">It's named **woodgrove-VNET**.</span></span>

1. <span data-ttu-id="431a1-180">Välj **Inställningar** > **Undernät** för att komma till dina definierade undernät.</span><span class="sxs-lookup"><span data-stu-id="431a1-180">Select **Settings** > **Subnets** to get to your defined subnets.</span></span>

1. <span data-ttu-id="431a1-181">Klicka på posten **backendSubnet** för att öppna egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="431a1-181">Click the **backendSubnet** entry to open the properties.</span></span>

    ![Skärmbild som visar backendSubnet-posterna i området Undernät för undernäten](../media/3-subnets.png)

1. <span data-ttu-id="431a1-183">Klicka på posten ”Ingen” i **Nätverkssäkerhetsgrupp**</span><span class="sxs-lookup"><span data-stu-id="431a1-183">Click the "None" entry on the **Network security group**</span></span>

    ![Skärmbild som visar ett tomt Nätverkssäkerhet i backendSubnet](../media/3-add-network-security-group.png)

1. <span data-ttu-id="431a1-185">Välj **woodgrove-NSG** för att lägga till det i det här virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="431a1-185">Select the **woodgrove-NSG** to add it to this VNET.</span></span>

1. <span data-ttu-id="431a1-186">Klicka på **Spara** för att tillämpa ändringen.</span><span class="sxs-lookup"><span data-stu-id="431a1-186">Click **Save** to apply the change.</span></span>

<span data-ttu-id="431a1-187">Nu när vårt nätverk är klart kan vi skapa några virtuella datorer och placera dem i nätverket!</span><span class="sxs-lookup"><span data-stu-id="431a1-187">Now that our network is ready, let's create some virtual machines to sit on this network!</span></span>