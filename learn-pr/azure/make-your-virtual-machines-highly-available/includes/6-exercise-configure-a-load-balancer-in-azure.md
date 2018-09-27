<span data-ttu-id="75fd3-101">Du har redan skapat de resurser som är viktigast för din onlinebankarkitektur.</span><span class="sxs-lookup"><span data-stu-id="75fd3-101">You have created the key resources for your online bank architecture.</span></span> <span data-ttu-id="75fd3-102">I den här övningen ska du slutföra installationen genom att konfigurera regler för lastbalanseraren.</span><span class="sxs-lookup"><span data-stu-id="75fd3-102">In this exercise, you will complete the setup by configuring the load-balancing rules.</span></span>

<span data-ttu-id="75fd3-103">När installationen är klar ska du testa belastningsutjämnaren genom att ta bort en virtuell dator från poolen och verifiera att klientbegäranden inte längre skickas till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="75fd3-103">Once setup is complete, you will test the load balancer by removing a VM from the pool and verifying that client requests are no longer sent to this VM.</span></span>

<span data-ttu-id="75fd3-104">Vi börjar genom att definiera vår serverdelspool i lastbalanseraren.</span><span class="sxs-lookup"><span data-stu-id="75fd3-104">We'll start by defining our backend pool in the load balancer.</span></span> <span data-ttu-id="75fd3-105">Detta avgör var inkommande begäranden dirigeras.</span><span class="sxs-lookup"><span data-stu-id="75fd3-105">This determines where inbound requests are routed.</span></span>

## <a name="create-a-backend-address-pool"></a><span data-ttu-id="75fd3-106">Skapa en serverdelsadresspool</span><span class="sxs-lookup"><span data-stu-id="75fd3-106">Create a backend address pool</span></span>

1. <span data-ttu-id="75fd3-107">I menyn till vänster i[Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true), välj **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-107">In the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true), in the left menu, Select **All resources**.</span></span> <span data-ttu-id="75fd3-108">Välj sedan lastbalanseraren i resurslistan (**woodgrove-LB**).</span><span class="sxs-lookup"><span data-stu-id="75fd3-108">Then, in the resource list, select your load balancer (**woodgrove-LB**).</span></span>

1. <span data-ttu-id="75fd3-109">Välj **Inställningar** > **serverdelspooler**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-109">Select **Settings** > **Backend pools**.</span></span> <span data-ttu-id="75fd3-110">Observera att vi inte har några definierade ännu.</span><span class="sxs-lookup"><span data-stu-id="75fd3-110">Notice we don't have any defined yet.</span></span>

1. <span data-ttu-id="75fd3-111">Lägg till en ny serverdelspool genom att klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-111">Click **Add** to add a new backend pool.</span></span>

    ![Skärmbilden visar hur man lägger till en serverdelspool.](../media/6-backend-pools.png)

1. <span data-ttu-id="75fd3-113">Lägg till följande information på **Lägg till serverdelspool**-bladet:</span><span class="sxs-lookup"><span data-stu-id="75fd3-113">On the **Add backend pool** blade, add the following information:</span></span>
    - <span data-ttu-id="75fd3-114">Ge den namnet _woodgrove BEP_.</span><span class="sxs-lookup"><span data-stu-id="75fd3-114">Name it _woodgrove-BEP_.</span></span>
    - <span data-ttu-id="75fd3-115">Associerade poolen till en **Tillgänglighetsuppsättning**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-115">Associated the pool to an **Availability set**.</span></span>
    - <span data-ttu-id="75fd3-116">Välj _woodgrove-AS_ för tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="75fd3-116">For the availability set, select _woodgrove-AS_.</span></span>

1. <span data-ttu-id="75fd3-117">Klicka på **Lägg till en IP-konfiguration för målnätverk**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-117">Click **Add a target network IP configuration**.</span></span>

1. <span data-ttu-id="75fd3-118">I listan **Virtuell måldator**, välj _woodgrove-VM1_.</span><span class="sxs-lookup"><span data-stu-id="75fd3-118">In the **Target virtual machine** list, select _woodgrove-VM1_.</span></span>

1. <span data-ttu-id="75fd3-119">I listan **Nätverkets IP-konfiguration** väljer du den virtuella datorns IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="75fd3-119">In the **Network IP configuration** list for the VM, select the VM's IP address pool.</span></span>

1. <span data-ttu-id="75fd3-120">Upprepa dessa steg för att lägga till _woodgrove-VM2_ och _woodgrove-VM3_ i serverdelspoolen.</span><span class="sxs-lookup"><span data-stu-id="75fd3-120">Repeat those steps to add _woodgrove-VM2_ and _woodgrove-VM3_ to the backend pool.</span></span>

    ![Skärmbilden visar bladet Lägg till serverdelspool med alla tre virtuella datorer som valts.](../media/6-add-backend-pool.png)

1. <span data-ttu-id="75fd3-122">Klicka på **OK** för att stänga bladet.</span><span class="sxs-lookup"><span data-stu-id="75fd3-122">Click **OK** to close the blade.</span></span>

<span data-ttu-id="75fd3-123">Vänta tills belastningsutjämningskonfigurationen har uppdaterats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="75fd3-123">Wait until the load balancer configuration has updated before proceeding with the exercise.</span></span>

## <a name="create-a-health-probe-for-the-load-balancer"></a><span data-ttu-id="75fd3-124">Skapa en hälsoavsökning för lastbalanseraren</span><span class="sxs-lookup"><span data-stu-id="75fd3-124">Create a health probe for the load balancer</span></span>

<span data-ttu-id="75fd3-125">Lägg sedan till en hälsoavsökning för HTTP via port 80.</span><span class="sxs-lookup"><span data-stu-id="75fd3-125">Next, add a health probe for HTTP over port 80.</span></span>

1. <span data-ttu-id="75fd3-126">På bladet **woodgrove-LB** under **Inställningar** klickar du på **Hälsoavsökningar**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-126">On the **woodgrove-LB** blade, under **Settings**, click **Health probes**.</span></span> <span data-ttu-id="75fd3-127">Observera att den är tom i nuläget.</span><span class="sxs-lookup"><span data-stu-id="75fd3-127">Notice it's currently empty.</span></span>

1. <span data-ttu-id="75fd3-128">Lägg till en ny hälsoavsökning genom att klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-128">Click **Add** to add a new health probe.</span></span>

1. <span data-ttu-id="75fd3-129">Lägg till följande konfiguration på bladet **Lägg till hälsoavsökning**:</span><span class="sxs-lookup"><span data-stu-id="75fd3-129">On the **Add health probe** blade, set the following configuration:</span></span>
    - <span data-ttu-id="75fd3-130">**Namn:** _woodgrove-HP_</span><span class="sxs-lookup"><span data-stu-id="75fd3-130">**Name:** _woodgrove-HP_</span></span>
    - <span data-ttu-id="75fd3-131">**Protokoll:** _HTTP_</span><span class="sxs-lookup"><span data-stu-id="75fd3-131">**Protocol:** _HTTP_</span></span>
    - <span data-ttu-id="75fd3-132">**Port:** _80_</span><span class="sxs-lookup"><span data-stu-id="75fd3-132">**Port:** _80_</span></span>
    - <span data-ttu-id="75fd3-133">**Sök:** _/_</span><span class="sxs-lookup"><span data-stu-id="75fd3-133">**Path:** _/_</span></span>
    - <span data-ttu-id="75fd3-134">**Intervall:** _15_</span><span class="sxs-lookup"><span data-stu-id="75fd3-134">**Interval:** _15_</span></span>
    - <span data-ttu-id="75fd3-135">**Defekt tröskelvärde:** _2_</span><span class="sxs-lookup"><span data-stu-id="75fd3-135">**Unhealthy threshold:** _2_</span></span>

1. <span data-ttu-id="75fd3-136">Spara ändringarna genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-136">Click **OK** to save the changes.</span></span>

<span data-ttu-id="75fd3-137">Vänta tills belastningsutjämningskonfigurationen har uppdaterats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="75fd3-137">Wait until the load balancer configuration has updated before proceeding with the exercise.</span></span>

## <a name="create-a-load-balancer-rule"></a><span data-ttu-id="75fd3-138">Skapa en lastbalanseringsregel</span><span class="sxs-lookup"><span data-stu-id="75fd3-138">Create a load balancer rule</span></span>

<span data-ttu-id="75fd3-139">Skapa slutligen en regel för lastbalanseraren för HTTP via port 80 som associerar serverdelspoolen med hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="75fd3-139">Finally, create a load-balancing rule for the HTTP over port 80, that associates the backend pool with the health probe.</span></span>

1. <span data-ttu-id="75fd3-140">På bladet **woodgrove-LB** under **Inställningar** klickar du på **Regler för lastbalanserare**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-140">On the **woodgrove-LB** blade, under **Settings**, click **Load balancing rules**.</span></span>

1. <span data-ttu-id="75fd3-141">Klicka på **Lägg till** för att lägga till en ny regel.</span><span class="sxs-lookup"><span data-stu-id="75fd3-141">Click **Add** to add a new rule.</span></span>

1. <span data-ttu-id="75fd3-142">På bladet **Lägg till regel för lastbalanserare** anger du **Namnet** _woodgrove-HTTP-LBRule_.</span><span class="sxs-lookup"><span data-stu-id="75fd3-142">On the **Add load balancing rule** blade, set the the **Name** to _woodgrove-HTTP-LBRule_.</span></span>

1. <span data-ttu-id="75fd3-143">Kontrollera att följande information har angetts automatiskt:</span><span class="sxs-lookup"><span data-stu-id="75fd3-143">Verify that the following information has been automatically entered:</span></span>
    - <span data-ttu-id="75fd3-144">IP-version: **IPv4**</span><span class="sxs-lookup"><span data-stu-id="75fd3-144">IP Version: **IPv4**</span></span>
    - <span data-ttu-id="75fd3-145">Klientdelens IP-adress: **LoadBalancerFrontEnd**</span><span class="sxs-lookup"><span data-stu-id="75fd3-145">Frontend IP address: **LoadBalancerFrontEnd**</span></span>
    - <span data-ttu-id="75fd3-146">Protokoll: **TCP**</span><span class="sxs-lookup"><span data-stu-id="75fd3-146">Protocol: **TCP**</span></span>
    - <span data-ttu-id="75fd3-147">Port: **80**</span><span class="sxs-lookup"><span data-stu-id="75fd3-147">Port: **80**</span></span>
    - <span data-ttu-id="75fd3-148">Serverdelsport: **80**</span><span class="sxs-lookup"><span data-stu-id="75fd3-148">Backend port: **80**</span></span>
    - <span data-ttu-id="75fd3-149">Serverdelspool: **woodgrove-BEP**</span><span class="sxs-lookup"><span data-stu-id="75fd3-149">Backend pool: **woodgrove-BEP**</span></span>
    - <span data-ttu-id="75fd3-150">Hälsoavsökning: **woodgrove-HP**</span><span class="sxs-lookup"><span data-stu-id="75fd3-150">Health probe: **woodgrove-HP**</span></span>
    - <span data-ttu-id="75fd3-151">Sessionspermanens: **Ingen**</span><span class="sxs-lookup"><span data-stu-id="75fd3-151">Session persistence: **None**</span></span>
    - <span data-ttu-id="75fd3-152">Tidsgräns för inaktivitet: **4 minuter**</span><span class="sxs-lookup"><span data-stu-id="75fd3-152">Idle timeout: **4 minutes**</span></span>
    - <span data-ttu-id="75fd3-153">Flytande IP: **Inaktiverat**</span><span class="sxs-lookup"><span data-stu-id="75fd3-153">Floating IP: **Disabled**</span></span>

1. <span data-ttu-id="75fd3-154">Spara regeln genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-154">Click **OK** to save the rule.</span></span>

<span data-ttu-id="75fd3-155">Vänta tills belastningsutjämningskonfigurationen har uppdaterats innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="75fd3-155">Wait until the load balancer configuration has updated before proceeding with the exercise.</span></span>

## <a name="test-the-load-balancer"></a><span data-ttu-id="75fd3-156">Testa lastbalanseraren</span><span class="sxs-lookup"><span data-stu-id="75fd3-156">Test the load balancer</span></span>

1. <span data-ttu-id="75fd3-157">Gå tillbaka till sidan **Översikt** i lastbalanseraren och klicka på länken **Offentlig IP-adress** för att komma till den tilldelade IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="75fd3-157">Switch back to the **Overview** page of the load balancer and click the **Public IP address** link on the page to get to the IP address assigned.</span></span> <span data-ttu-id="75fd3-158">Alternativt kan du använda **Alla resurser** och sedan välja **woodgrove-LB-ip** i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="75fd3-158">Alternatively, you can use **All resources** and then in the resource list, select **woodgrove-LB-ip**.</span></span>

1. <span data-ttu-id="75fd3-159">Välj **IP-adress** på bladet **Översikt** och klicka sedan på knappen **Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-159">On the **Overview** blade, select the **IP address**, and click the **Copy** button next to it.</span></span> <span data-ttu-id="75fd3-160">Det här är den _offentliga_ IP-adressen för lastbalanseraren.</span><span class="sxs-lookup"><span data-stu-id="75fd3-160">This is the _public_ IP address for the load balancer.</span></span>

    ![Skärmbild visar översiktspanelen för den offentliga IP-adressen](../media/6-public-ip.png)

1. <span data-ttu-id="75fd3-162">Öppna en ny webbläsarflik och klistra in IP-adressen i adressfältet.</span><span class="sxs-lookup"><span data-stu-id="75fd3-162">Open a new browser tab and paste the IP address into the address bar of your browser.</span></span> <span data-ttu-id="75fd3-163">Anteckna namnet på servern och håll fliken öppen.</span><span class="sxs-lookup"><span data-stu-id="75fd3-163">Note the name of the server and keep this tab open.</span></span>

1. <span data-ttu-id="75fd3-164">Klicka på **Alla resurser** i menyn till vänster i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="75fd3-164">In the Azure portal, in the left menu, click **All resources**.</span></span> <span data-ttu-id="75fd3-165">Klicka på den server som du antecknade ovan i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="75fd3-165">Then, in the resource list, click the server you noted above.</span></span>

1. <span data-ttu-id="75fd3-166">Klicka på **Stoppa** på **Översikt**-bladet och klicka sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="75fd3-166">On the **Overview** blade, click **Stop**, and then click **Yes**.</span></span>

1. <span data-ttu-id="75fd3-167">Vänta tills den virtuella datorn har stoppats och växla sedan till fliken du öppnade i steg 3.</span><span class="sxs-lookup"><span data-stu-id="75fd3-167">Wait until the VM has stopped, and then switch to the tab you viewed in step 3.</span></span> <span data-ttu-id="75fd3-168">Uppdatera sidan.</span><span class="sxs-lookup"><span data-stu-id="75fd3-168">Refresh the page.</span></span>

1. <span data-ttu-id="75fd3-169">Belastningsutjämnaren skickar nu din HTTP-begäran till en av dina andra virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="75fd3-169">The load balancer will now send your HTTP request to one of your other VMs.</span></span>

<span data-ttu-id="75fd3-170">I den här övningen har du slutfört distributionen av dina virtuella datorer för serverdelar och för lastbalanseraren.</span><span class="sxs-lookup"><span data-stu-id="75fd3-170">In this exercise, you completed the deployment of your backend VMs and the load balancer.</span></span>
