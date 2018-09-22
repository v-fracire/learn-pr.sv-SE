<span data-ttu-id="4c8dd-101">Nu är din webbplats igång på Azure.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-101">You now have your site up and running on Azure.</span></span> <span data-ttu-id="4c8dd-102">Men hur kan du se till att din webbplats alltid är igång?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-102">But how can you help ensure your site is running 24/7?</span></span>

<span data-ttu-id="4c8dd-103">Vad händer till exempel när du behöver utföra veckounderhåll?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-103">For instance, what happens when you need to do weekly maintenance?</span></span> <span data-ttu-id="4c8dd-104">Tjänsten kommer fortfarande att vara otillgänglig under underhållsperioden.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-104">Your service will still be unavailable during your maintenance window.</span></span> <span data-ttu-id="4c8dd-105">Och eftersom webbplatsen når användare över hela världen finns det ingen bra tidpunkt att ta ned systemen för underhåll.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-105">And because your site reaches users all over the world, there's no good time to take down your systems for maintenance.</span></span> <span data-ttu-id="4c8dd-106">Du kan också stöta på prestandaproblem om för många användare ansluter samtidigt.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-106">You may also run into performance issues if too many users connect at the same time.</span></span>

## <a name="what-are-availability-and-high-availability"></a><span data-ttu-id="4c8dd-107">Vad är tillgänglighet och hög tillgänglighet?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-107">What are availability and high availability?</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="4c8dd-108">![Ett höghastighetståg som representerar hög tillgänglighet](../media/3-availability.png)</span><span class="sxs-lookup"><span data-stu-id="4c8dd-108">![A speed train representing high availability](../media/3-availability.png)</span></span>
  :::column-end:::
    <span data-ttu-id="4c8dd-109">:::column span="3"::: _Tillgänglighet_ innebär hur länge tjänsten är igång utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-109">:::column span="3"::: _Availability_ refers to how long your service is up and running without interruption.</span></span> <span data-ttu-id="4c8dd-110">_Hög tillgänglighet_ eller _högt tillgänglig_ avser en tjänst som är igång under lång tid.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-110">_High availability_, or _highly available_, refers to a service that's up and running for a long period of time.</span></span>

<span data-ttu-id="4c8dd-111">Du vet hur frustrerande det är när du inte kommer åt den information du behöver.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-111">You know how frustrating it is when you can't access the information you need.</span></span> <span data-ttu-id="4c8dd-112">Tänk på en webbplats för sociala medier eller nyheter som du besöker dagligen.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-112">Think of a social media or news site that you visit daily.</span></span> <span data-ttu-id="4c8dd-113">Kommer du alltid åt webbplatsen eller visas ofta felmeddelanden, till exempel ”503 – tjänsten är inte tillgänglig”?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-113">Can you always access the site, or do you often see error messages like "503 Service Unavailable"?</span></span>
  :::column-end:::
 :::row-end:::

<span data-ttu-id="4c8dd-114">Du kanske har hört uttryck som ”five nines”-tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-114">You may have heard terms like "five nines availability."</span></span> <span data-ttu-id="4c8dd-115">”Five nines” (fem nior) innebär att tjänsten garanterat är igång 99,999 procent av tiden.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-115">Five nines availability means that the service is guaranteed to be running 99.999 percent of the time.</span></span> <span data-ttu-id="4c8dd-116">Det är svårt att uppnå 100 procent tillgänglighet, men många team strävar efter minst fem nior.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-116">Although it's difficult to achieve 100 percent availability, many teams strive for at least five nines.</span></span>

## <a name="what-is-resiliency"></a><span data-ttu-id="4c8dd-117">Vad är återhämtning?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-117">What is resiliency?</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="4c8dd-118">![Ett hälsodiagram som representerar återhämtning](../media/3-resiliency.png)</span><span class="sxs-lookup"><span data-stu-id="4c8dd-118">![A health chart representing resiliency](../media/3-resiliency.png)</span></span>
  :::column-end:::
    <span data-ttu-id="4c8dd-119">:::column span="3"::: _Återhämtning_ avser ett systems förmåga att fortsätta att fungera under avvikande förhållanden.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-119">:::column span="3"::: _Resiliency_ refers to a system's ability to stay operational during abnormal conditions.</span></span>

<span data-ttu-id="4c8dd-120">Exempel på sådana förhållanden:</span><span class="sxs-lookup"><span data-stu-id="4c8dd-120">These conditions include:</span></span>

- <span data-ttu-id="4c8dd-121">Naturkatastrofer.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-121">Natural disasters.</span></span>
- <span data-ttu-id="4c8dd-122">Systemunderhåll, både planerat och oplanerat, bland annat programuppdateringar och säkerhetskorrigeringar.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-122">System maintenance, both planned and unplanned, including software updates and security patches.</span></span>
- <span data-ttu-id="4c8dd-123">Toppar i trafiken på webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-123">Spikes in traffic to your site.</span></span>
- <span data-ttu-id="4c8dd-124">Hot från parter med ont uppsåt, till exempel distribuerade överbelastningsattacker (DDoS).</span><span class="sxs-lookup"><span data-stu-id="4c8dd-124">Threats made by malicious parties, such as distributed denial of service, or DDoS, attacks.</span></span>
  :::column-end:::
:::row-end:::

<span data-ttu-id="4c8dd-125">Tänk dig att marknadsavdelningen vill ha en blixtrea för att marknadsföra en ny serie vitamintillskott.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-125">Imagine your marketing team wants to have a flash sale to promote a new line of vitamin supplements.</span></span> <span data-ttu-id="4c8dd-126">Du kan förvänta dig en hög topp i trafiken under den här tiden.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-126">You might expect a huge spike in traffic during this time.</span></span> <span data-ttu-id="4c8dd-127">Den här toppen kan överbelasta bearbetningssystemet, så att det blir långsamt eller stannar helt och dina användare blir besvikna.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-127">This spike could overwhelm your processing system, causing it to slow down or halt, disappointing your users.</span></span> <span data-ttu-id="4c8dd-128">Du kan ha upplevt den här besvikelsen själv.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-128">You may have experienced this disappointment for yourself.</span></span> <span data-ttu-id="4c8dd-129">Har du någonsin försökt komma åt en onlinerea och upplevt att webbplatsen inte svarar?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-129">Have you ever tried to access an online sale only to find the website wasn't responding?</span></span>

## <a name="what-is-a-load-balancer"></a><span data-ttu-id="4c8dd-130">Vad är en lastbalanserare?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-130">What is a load balancer?</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="4c8dd-131">![En våg som representerar lastbalansering](../media/3-lb.png)</span><span class="sxs-lookup"><span data-stu-id="4c8dd-131">![A scale representing load balancing](../media/3-lb.png)</span></span>
  :::column-end:::
    <span data-ttu-id="4c8dd-132">:::column span="3"::: En _lastbalanserare_ distribuerar trafik jämt mellan alla system i en pool.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-132">:::column span="3"::: A _load balancer_ distributes traffic evenly among each system in a pool.</span></span> <span data-ttu-id="4c8dd-133">Med hjälp av en lastbalanserare kan du uppnå både hög tillgänglighet och återhämtning.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-133">A load balancer can help you achieve both high availability and resiliency.</span></span>

<span data-ttu-id="4c8dd-134">Anta att du börjar med att lägga till ytterligare virtuella datorer, med identisk konfiguration, på varje nivå.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-134">Say you start by adding additional VMs, each configured identically, to each tier.</span></span> <span data-ttu-id="4c8dd-135">Tanken är att ha ytterligare system som är redo om ett blir otillgängligt eller hanterar för många användare samtidigt.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-135">The idea is to have additional systems ready, in case one goes down or is serving too many users at the same time.</span></span>
  :::column-end:::
:::row-end:::

<span data-ttu-id="4c8dd-136">Problemet här är att varje virtuell dator har en egen IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-136">The problem here is that each VM would have its own IP address.</span></span> <span data-ttu-id="4c8dd-137">Dessutom går det inte att distribuera trafik om ett system blir otillgängligt eller är upptaget.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-137">Plus, you don't have a way to distribute traffic in case one system goes down or is busy.</span></span> <span data-ttu-id="4c8dd-138">Hur kopplar du samman de virtuella datorerna så att de visas som ett enda system för användaren?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-138">How do you connect your VMs so that they appear to the user as one system?</span></span>

<span data-ttu-id="4c8dd-139">Lösningen är att använda en belastningsutjämnare för att distribuera trafik.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-139">The answer is to use a load balancer to distribute traffic.</span></span> <span data-ttu-id="4c8dd-140">Belastningsutjämnaren blir användarens startpunkt.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-140">The load balancer becomes the entry point to the user.</span></span> <span data-ttu-id="4c8dd-141">Användaren vet inte (och behöver inte veta) vilket system lastbalanseraren väljer för att ta emot begäran.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-141">The user doesn't know (or need to know) which system the load balancer chooses to receive the request.</span></span>

<span data-ttu-id="4c8dd-142">Här är en bild som visar rollen för en lastbalanserare.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-142">The following illustration shows the role of a load balancer.</span></span>

![En bild som visar webbnivån i en arkitektur med tre nivåer.](../media/3-load-balancer.png)

<span data-ttu-id="4c8dd-146">Du ser att lastbalanseraren tar emot användarens begäran.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-146">You see that the load balancer receives the user's request.</span></span> <span data-ttu-id="4c8dd-147">Belastningsutjämnaren dirigerar begäran till någon av de virtuella datorerna på webbnivån.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-147">The load balancer directs the request to one of the VMs in the web tier.</span></span> <span data-ttu-id="4c8dd-148">Om en virtuell dator inte är tillgänglig eller inte svarar slutar belastningsutjämnaren att skicka trafik till den.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-148">If a VM is unavailable or stops responding, the load balancer stops sending traffic to it.</span></span> <span data-ttu-id="4c8dd-149">Belastningsutjämnaren dirigerar sedan trafik till en av de tillgängliga servrarna.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-149">The load balancer then directs traffic to one of the responsive servers.</span></span>

<span data-ttu-id="4c8dd-150">Med hjälp av belastningsutjämning kan du köra underhållsuppgifter utan att avbryta tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-150">Load balancing enables you to run maintenance tasks without interrupting service.</span></span> <span data-ttu-id="4c8dd-151">Du kan exempelvis sprida ut underhållsperioden för alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-151">For example, you can stagger the maintenance window for each VM.</span></span> <span data-ttu-id="4c8dd-152">Under underhållsperioden känner belastningsutjämnaren av att den virtuella datorn inte svarar och dirigerar trafik till andra virtuella datorer i poolen.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-152">During the maintenance window, the load balancer detects that the VM is unresponsive, and directs traffic to other VMs in the pool.</span></span>

<span data-ttu-id="4c8dd-153">För din näthandelswebbplats kan app- och datanivåerna också ha en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-153">For your e-commerce site, the app and data tiers can also have a load balancer.</span></span> <span data-ttu-id="4c8dd-154">Det beror helt på vad tjänsten kräver.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-154">It all depends on what your service requires.</span></span>

## <a name="what-is-azure-load-balancer"></a><span data-ttu-id="4c8dd-155">Vad är Azure Load Balancer?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-155">What is Azure Load Balancer?</span></span>

<span data-ttu-id="4c8dd-156">Azure Load Balancer är en tjänst för lastbalansering som Microsoft tillhandahåller vilken hjälper att ta hand om underhållet åt dig.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-156">Azure Load Balancer is a load balancer service that Microsoft provides that helps take care of the maintenance for you.</span></span>

<span data-ttu-id="4c8dd-157">När du manuellt konfigurerar normal programvara för belastningsutjämning på en virtuell dator finns det en nackdel: nu har du ytterligare ett system som du behöver underhålla.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-157">When you manually configure typical load balancer software on a virtual machine, there's a downside: you now have an additional system that you need to maintain.</span></span> <span data-ttu-id="4c8dd-158">Om lastbalanseraren blir otillgänglig eller kräver rutinunderhåll kommer du tillbaka till det ursprungliga problemet.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-158">If your load balancer goes down or needs routine maintenance, you're back to your original problem.</span></span>

<span data-ttu-id="4c8dd-159">Om du istället använder Azure Load Balancer så finns det ingen infrastruktur eller programvara att underhålla.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-159">If instead, however, you use Azure Load Balancer, there's no infrastructure or software for you to maintain.</span></span>

<span data-ttu-id="4c8dd-160">Följande bild visar rollen för Azure-lastbalanserare i en arkitektur med flera nivåer.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-160">The following illustration shows  the role of Azure load balancers in a multi-tier architecture.</span></span>

![En bild som visar webbnivån i en arkitektur med tre nivåer.](../media/3-azure-load-balancer.png)

## <a name="what-about-dns"></a><span data-ttu-id="4c8dd-164">Hur är det med DNS?</span><span class="sxs-lookup"><span data-stu-id="4c8dd-164">What about DNS?</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="4c8dd-165">![En nål på en karta som representerar DNS](../media/3-map-pin.png)</span><span class="sxs-lookup"><span data-stu-id="4c8dd-165">![A pin on a map representing DNS](../media/3-map-pin.png)</span></span>
  :::column-end:::
    <span data-ttu-id="4c8dd-166">:::column span="3"::: DNS, eller Domain Name System, är ett sätt att mappa användarvänliga namn till sina IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-166">:::column span="3"::: DNS, or Domain Name System, is a way to map user-friendly names to their IP addresses.</span></span> <span data-ttu-id="4c8dd-167">Du kan tänka dig DNS som Internets telefonkatalog.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-167">You can think of DNS as the phonebook of the internet.</span></span>

<span data-ttu-id="4c8dd-168">Domännamnet contoso.com kan till exempel mappas till belastningsutjämnarens IP-adress på webbnivå, 40.65.106.192.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-168">For example, your domain name, contoso.com, might map to the IP address of the load balancer at the web tier, 40.65.106.192.</span></span>

<span data-ttu-id="4c8dd-169">Du kan ta med din egen DNS-server eller använda Azure DNS, en värdtjänst för DNS-domäner som körs på Azure-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-169">You can bring your own DNS server or use Azure DNS, a hosting service for DNS domains that runs on Azure infrastructure.</span></span>
  :::column-end:::
:::row-end:::

<span data-ttu-id="4c8dd-170">Följande bild visar Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-170">The following illustration shows Azure DNS.</span></span> <span data-ttu-id="4c8dd-171">När användaren navigerar till contoso.com dirigerar Azure DNS trafik till lastbalanseraren.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-171">When the user navigates to contoso.com, Azure DNS routes traffic to the load balancer.</span></span>

![En bild som visar Azures domännamnssystem placerat framför lastbalanseraren.](../media/3-dns.png)

## <a name="summary"></a><span data-ttu-id="4c8dd-173">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="4c8dd-173">Summary</span></span>

<span data-ttu-id="4c8dd-174">Med belastningsutjämning har din näthandelswebbplats nu högre tillgänglighet och återhämtning.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-174">With load balancing in place, your e-commerce site is now more highly available and resilient.</span></span> <span data-ttu-id="4c8dd-175">När du utför underhåll eller när trafiken ökar tillfälligt kan belastningsutjämnaren distribuera trafik till ett annat tillgängligt system.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-175">When you perform maintenance or receive an uptick in traffic, your load balancer can distribute traffic to another available system.</span></span>

<span data-ttu-id="4c8dd-176">Du kan konfigurera en egen belastningsutjämnare på en virtuell dator, men Azure Load Balancer minskar behovet av underhåll eftersom det inte finns någon infrastruktur eller programvara att underhålla.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-176">Although you can configure your own load balancer on a VM, Azure Load Balancer reduces upkeep because there's no infrastructure or software to maintain.</span></span>

<span data-ttu-id="4c8dd-177">DNS mappar användarvänliga namn till deras IP-adresser, ungefär som en telefonkatalog mappar namn på personer och företag till telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-177">DNS maps user-friendly names to their IP addresses, much like how a phonebook maps names of people or businesses to phone numbers.</span></span> <span data-ttu-id="4c8dd-178">Du kan ta med en egen DNS-server eller använda Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="4c8dd-178">You can bring your own DNS server, or use Azure DNS.</span></span>
