<span data-ttu-id="9ecf0-101">När beräkningsmiljöer flyttas från kundstyrda datacenter till molndatacenter skiftar även ansvaret för säkerheten.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-101">As computing environments move from customer-controlled data centers to cloud data centers, the responsibility of security also shifts.</span></span> <span data-ttu-id="9ecf0-102">Säkerhet är nu en åtgärd som delas av både molnproviders och kunder.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-102">Security is now a concern shared both by cloud providers and customers.</span></span> <span data-ttu-id="9ecf0-103">För varje program och lösning är det viktigt att du förstår vad som är ditt eget ansvar och vad som är Azures ansvar.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-103">For every application and solution, it's important to understand what's your responsibility and what's Azure's responsibility.</span></span>

#### <a name="understand-security-threats"></a><span data-ttu-id="9ecf0-104">Förstå säkerhetshot</span><span class="sxs-lookup"><span data-stu-id="9ecf0-104">Understand security threats</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWkotg]

#### <a name="azure-security-you-versus-the-cloud"></a><span data-ttu-id="9ecf0-105">Azure-säkerhet: du mot molnet</span><span class="sxs-lookup"><span data-stu-id="9ecf0-105">Azure security: you versus the cloud</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvj]

## <a name="share-security-responsibility-with-azure"></a><span data-ttu-id="9ecf0-106">Dela säkerhetsansvaret med Azure</span><span class="sxs-lookup"><span data-stu-id="9ecf0-106">Share security responsibility with Azure</span></span>

<span data-ttu-id="9ecf0-107">Den första övergången du gör är från lokala datacenter till IaaS (infrastruktur som en tjänst).</span><span class="sxs-lookup"><span data-stu-id="9ecf0-107">The first shift you’ll make is from on-premises data centers to infrastructure as a service (IaaS).</span></span> <span data-ttu-id="9ecf0-108">Med IaaS använder du dig av tjänster på lägsta möjliga nivå, där Azure skapar virtuella datorer och virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-108">With IaaS, you are leveraging the lowest-level service and asking Azure to create virtual machines (VMs) and virtual networks.</span></span> <span data-ttu-id="9ecf0-109">På den här nivån är det fortfarande ditt eget ansvar att korrigera och skydda dina operativsystem och program, samt konfigurera nätverket så det är säkert.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-109">At this level, it's still your responsibility to patch and secure your operating systems and software, as well as configure your network to be secure.</span></span> <span data-ttu-id="9ecf0-110">Hos Contoso Shipping, drar du nytta av IaaS när du går över till virtuella Azure-datorer istället för dina lokala fysiska servrar.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-110">At Contoso Shipping, you are taking advantage of IaaS when you start using Azure VMs instead of your on-premises physical servers.</span></span> <span data-ttu-id="9ecf0-111">Förutom driftfördelarna så slipper du även bekymra sig över att skydda nätverkets fysiska delar.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-111">In addition to the operational advantages, you receive the security advantage of having outsourced concern over protecting the physical parts of the network.</span></span>

<span data-ttu-id="9ecf0-112">När du flyttar till PaaS (plattform som en tjänst) slipper du en mängd olika säkerhetsbekymmer.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-112">Moving to platform as a service (PaaS) outsources a lot of security concerns.</span></span> <span data-ttu-id="9ecf0-113">På den här nivån tar Azure hand om operativsystemet och de mest grundläggande programmen, som databashanteringssystem.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-113">At this level, Azure is taking care of the operating system and of most foundational software like database management systems.</span></span> <span data-ttu-id="9ecf0-114">Allt uppdateras med de senaste korrigeringarna och du kan integrera åtkomstkontroll med Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-114">Everything is updated with the latest security patches and can be integrated with Azure Active Directory for access controls.</span></span> <span data-ttu-id="9ecf0-115">PaaS har dessutom en mängd driftfördelar.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-115">PaaS also comes with a lot of operational advantages.</span></span> <span data-ttu-id="9ecf0-116">Istället för att skapa hela infrastrukturen och undernäten i din miljö manuellt kan du ”peka och klicka” i Azure-portalen, eller köra automatiserade skript som aktiverar och kopplar ned avancerade säkerhetssystem och skalar om dem efter behov.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-116">Rather than building whole infrastructures and subnets for your environments by hand, you can "point and click" within the Azure portal or run automated scripts to bring complex, secured systems up and down, and scale them as needed.</span></span> <span data-ttu-id="9ecf0-117">Contoso Shipping använder Azure Event Hubs för att föra in telemetridata från drönare och lastbilar &mdash; samt en webbapp med en Azure Cosmos DB-serverdel med deras mobilappar &mdash; som alla är exempel på PaaS.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-117">Contoso Shipping uses Azure Event Hubs for ingesting telemetry data from drones and trucks &mdash; as well as a web app with an Azure Cosmos DB back end with their mobile apps &mdash; which are all examples of PaaS.</span></span>

<span data-ttu-id="9ecf0-118">Med SaaS (programvara som en tjänst) outsourcar du nästan allt.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-118">With software as a service (SaaS), you outsource almost everything.</span></span> <span data-ttu-id="9ecf0-119">SaaS är programvara som körs med en Internetinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-119">SaaS is software that runs with an internet infrastructure.</span></span> <span data-ttu-id="9ecf0-120">Koden styrs av leverantören men konfigureras för användning av kunden.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-120">The code is controlled by the vendor but configured to be used by the customer.</span></span> <span data-ttu-id="9ecf0-121">Precis som många andra företag så använder Contoso Shipping Office 365 och det är ett utmärkt exempel på SaaS!</span><span class="sxs-lookup"><span data-stu-id="9ecf0-121">Like so many companies, Contoso Shipping uses Office 365, which is a great example of SaaS!</span></span>

![shared_responsibility.png](../media/shared_responsibilities.png)

## <a name="a-layered-approach-to-security"></a><span data-ttu-id="9ecf0-123">Säkerhet på flera nivåer</span><span class="sxs-lookup"><span data-stu-id="9ecf0-123">A layered approach to security</span></span>

<span data-ttu-id="9ecf0-124">*Skydd på djupet* är en strategi som använder en serie mekanismer till att fördröja en attack som syftar till att få obehörig åtkomst till information.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-124">*Defense in depth* is a strategy that employs a series of mechanisms to slow the advance of an attack aimed at acquiring unauthorized access to information.</span></span> <span data-ttu-id="9ecf0-125">Varje nivå ger skydd, så om intrång sker på en nivå finns det ytterligare nivåer som förhindrar exponering.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-125">Each layer provides protection so that if one layer is breached, a subsequent layer is already in place to prevent further exposure.</span></span> <span data-ttu-id="9ecf0-126">Microsoft använder säkerhet på flera nivåer både i fysiska datacenter och mellan olika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-126">Microsoft applies a layered approach to security, both in physical data centers and across Azure services.</span></span> <span data-ttu-id="9ecf0-127">Målet med ett djupgående skydd är att skydda och förhindra att information blir stulen av personer som inte har behörighet till den.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-127">The objective of defense in depth is to protect and prevent information from being stolen by individuals who are not authorized to access it.</span></span>

<span data-ttu-id="9ecf0-128">Djupgående skydd kan visualiseras som en uppsättning koncentriska ringar, där de data som ska skyddas ligger i mitten.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-128">Defense in depth can be visualized as a set of concentric rings, with the data to be secured at the center.</span></span> <span data-ttu-id="9ecf0-129">Varje ring är en extra säkerhetsnivå kring datan.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-129">Each ring adds an additional layer of security around the data.</span></span> <span data-ttu-id="9ecf0-130">Den här metoden innebär att man inte behöver förlita sig på en enda skyddsnivå. Den fördröjer angrepp och aviserar med telemetri som kan åtgärdas, antingen automatiskt eller manuellt.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-130">This approach removes reliance on any single layer of protection and acts to slow down an attack and provide alert telemetry that can be acted upon, either automatically or manually.</span></span> <span data-ttu-id="9ecf0-131">Låt oss ta en titt på var och en av dessa nivåer.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-131">Let's take a look at each of the layers.</span></span>

![Skydd på djupet](../media/defense_in_depth_layers_small.PNG)

:::row:::
  :::column:::
    <span data-ttu-id="9ecf0-133">![Bild som representerar data](../media/2-data.png)</span><span class="sxs-lookup"><span data-stu-id="9ecf0-133">![Image representing data](../media/2-data.png)</span></span>
  :::column-end:::
    <span data-ttu-id="9ecf0-134">:::column span="3"::: **Data**</span><span class="sxs-lookup"><span data-stu-id="9ecf0-134">:::column span="3"::: **Data**</span></span>

<span data-ttu-id="9ecf0-135">I nästan alla fall är angriparna ute efter data:</span><span class="sxs-lookup"><span data-stu-id="9ecf0-135">In almost all cases, attackers are after data:</span></span>

- <span data-ttu-id="9ecf0-136">Data som lagras i en databas</span><span class="sxs-lookup"><span data-stu-id="9ecf0-136">Data stored in a database</span></span>
- <span data-ttu-id="9ecf0-137">Data som lagras på en disk i virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="9ecf0-137">Data stored on disk inside virtual machines</span></span>
- <span data-ttu-id="9ecf0-138">Data som lagras i ett SaaS-program, till exempel Office 365</span><span class="sxs-lookup"><span data-stu-id="9ecf0-138">Data stored on a SaaS application such as Office 365</span></span>
- <span data-ttu-id="9ecf0-139">Data som lagras i molnlagring</span><span class="sxs-lookup"><span data-stu-id="9ecf0-139">Data stored in cloud storage</span></span>

<span data-ttu-id="9ecf0-140">Ansvaret för att säkerställa att datan är skyddad ligger hos de som lagrar och styr åtkomsten till den.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-140">It's the responsibility of those storing and controlling access to data to ensure that it's properly secured.</span></span> <span data-ttu-id="9ecf0-141">Det finns ofta regelverk som fastställer vilka kontroller och processer som behövs för att säkerställa sekretess, integritet och tillgänglighet för datan.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-141">Often, there are regulatory requirements that dictate the controls and processes that must be in place to ensure the confidentiality, integrity, and availability of the data.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="9ecf0-142">![Bild av en fil i nätverket](../media/2-application.png)</span><span class="sxs-lookup"><span data-stu-id="9ecf0-142">![Image of a file on the network](../media/2-application.png)</span></span>
  :::column-end:::
    <span data-ttu-id="9ecf0-143">:::column span="3"::: **Program**</span><span class="sxs-lookup"><span data-stu-id="9ecf0-143">:::column span="3"::: **Application**</span></span>

- <span data-ttu-id="9ecf0-144">Se till att programmen är säkra utan sårbarheter.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-144">Ensure applications are secure and free of vulnerabilities.</span></span>
- <span data-ttu-id="9ecf0-145">Lagra känsliga programhemligheter i ett säkert lagringsmedium.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-145">Store sensitive application secrets in a secure storage medium.</span></span>
- <span data-ttu-id="9ecf0-146">Gör säkerhet till ett designkrav för all programutveckling.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-146">Make security a design requirement for all application development.</span></span>

<span data-ttu-id="9ecf0-147">Om säkerhet introduceras i livscykeln för programutveckling kan antalet säkerhetsrisker i koden minskas.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-147">Integrating security into the application development life cycle will help reduce the number of vulnerabilities introduced in code.</span></span> <span data-ttu-id="9ecf0-148">Uppmuntra alla utvecklingsteam att säkerställa att programmen är säkra som standard och gör kraven på säkerhet icke förhandlingsbara.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-148">We encourage all development teams to ensure their applications are secure by default, and that they're making security requirements non-negotiable.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="9ecf0-149">![En terminal som representerar compute](../media/2-compute.png)</span><span class="sxs-lookup"><span data-stu-id="9ecf0-149">![A terminal representing compute](../media/2-compute.png)</span></span>
  :::column-end:::
    <span data-ttu-id="9ecf0-150">:::column span="3"::: **Beräkning**</span><span class="sxs-lookup"><span data-stu-id="9ecf0-150">:::column span="3"::: **Compute**</span></span>

- <span data-ttu-id="9ecf0-151">Säker åtkomst till virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-151">Secure access to virtual machines.</span></span>
- <span data-ttu-id="9ecf0-152">Implementera slutpunktsskydd och se till att systemen är korrigerade och aktuella.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-152">Implement endpoint protection and keep systems patched and current.</span></span>

<span data-ttu-id="9ecf0-153">Skadlig kod, okorrigerade system och felaktigt skyddade system gör din miljö sårbar för attacker.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-153">Malware, unpatched systems, and improperly secured systems open your environment to attacks.</span></span> <span data-ttu-id="9ecf0-154">Fokus på denna nivå är att se till att dina beräkningsresurser är säkra och att du har rätt kontroller på plats för att minimera eventuella säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-154">The focus in this layer is on making sure your compute resources are secure, and that you have the proper controls in place to minimize security issues.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="9ecf0-155">![Tre anslutna system som representerar nätverk](../media/2-networking.png)</span><span class="sxs-lookup"><span data-stu-id="9ecf0-155">![Three connected systems representing networking](../media/2-networking.png)</span></span>
  :::column-end:::
    <span data-ttu-id="9ecf0-156">:::column span="3"::: **Nätverk**</span><span class="sxs-lookup"><span data-stu-id="9ecf0-156">:::column span="3"::: **Networking**</span></span>

- <span data-ttu-id="9ecf0-157">Begränsa kommunikationen mellan resurser.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-157">Limit communication between resources.</span></span>
- <span data-ttu-id="9ecf0-158">Neka som standard.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-158">Deny by default.</span></span>
- <span data-ttu-id="9ecf0-159">Begränsa inkommande Internetåtkomst och begränsa utgående när det är lämpligt.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-159">Restrict inbound internet access and limit outbound, where appropriate.</span></span>
- <span data-ttu-id="9ecf0-160">Implementera säker anslutning till lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-160">Implement secure connectivity to on-premises networks.</span></span>

<span data-ttu-id="9ecf0-161">På den här nivån ligger fokus på att begränsa nätverksanslutningen mellan alla dina resurser och endast tillåta det som krävs.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-161">At this layer, the focus is on limiting the network connectivity across all your resources to allow only what is required.</span></span> <span data-ttu-id="9ecf0-162">Genom att begränsa kommunikationen kan du minska risken för lateral förflyttning i hela nätverket.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-162">By limiting this communication, you reduce the risk of lateral movement throughout your network.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="9ecf0-163">![En fysisk barriär som representerar perimeternätverket](../media/2-perimeter.png)</span><span class="sxs-lookup"><span data-stu-id="9ecf0-163">![A physical barrier representing the network perimeter](../media/2-perimeter.png)</span></span>
  :::column-end:::
    <span data-ttu-id="9ecf0-164">:::column span="3"::: **Perimeter**</span><span class="sxs-lookup"><span data-stu-id="9ecf0-164">:::column span="3"::: **Perimeter**</span></span>

- <span data-ttu-id="9ecf0-165">Använd ett skydd mot distribuerade överbelastningsattacker till att filtrera storskaliga attacker, innan de orsakar en överbelastningsattack hos slutanvändarna.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-165">Use distributed denial of service (DDoS) protection to filter large-scale attacks before they can cause a denial of service for end users.</span></span>
- <span data-ttu-id="9ecf0-166">Använd perimeterbrandväggar till att identifiera och varna vid skadliga attacker mot ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-166">Use perimeter firewalls to identify and alert on malicious attacks against your network.</span></span>

<span data-ttu-id="9ecf0-167">I nätverksperimetern handlar det om att skydda sig mot nätverksbaserade attacker mot dina resurser.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-167">At the network perimeter, it's about protecting from network-based attacks against your resources.</span></span> <span data-ttu-id="9ecf0-168">Att identifiera dessa attacker, eliminera deras inverkan och varna dig när de inträffar är viktiga sätt att skydda ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-168">Identifying these attacks, eliminating their impact, and alerting you when they happen are important ways to keep your network secure.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="9ecf0-169">![Ett märke som representerar ett säkert nätverk](../media/2-policies-and-access.png)</span><span class="sxs-lookup"><span data-stu-id="9ecf0-169">![A badge representing a secure access](../media/2-policies-and-access.png)</span></span>
  :::column-end:::
    <span data-ttu-id="9ecf0-170">:::column span="3"::: **Principer och åtkomst**</span><span class="sxs-lookup"><span data-stu-id="9ecf0-170">:::column span="3"::: **Policies and access**</span></span>

- <span data-ttu-id="9ecf0-171">Kontrollera åtkomst till infrastruktur och ändra kontrollen.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-171">Control access to infrastructure and change control.</span></span>
- <span data-ttu-id="9ecf0-172">Använd enkel inloggning och multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-172">Use single sign-on and multi-factor authentication.</span></span>
- <span data-ttu-id="9ecf0-173">Granska händelser och ändringar.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-173">Audit events and changes.</span></span>

<span data-ttu-id="9ecf0-174">Princip- och åtkomstnivån handlar om att säkerställa att identiteter är skyddade, samt att enbart bevilja åtkomst för det som behövs och att ändringar loggas.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-174">The policy and access layer is all about ensuring identities are secure, access granted is only what is needed, and changes are logged.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="9ecf0-175">![En övervakningskamera som representerar fysisk säkerhet](../media/2-physical-security.png)</span><span class="sxs-lookup"><span data-stu-id="9ecf0-175">![A security camera representing physical security](../media/2-physical-security.png)</span></span>
  :::column-end:::
    <span data-ttu-id="9ecf0-176">:::column span="3"::: **Fysisk säkerhet**</span><span class="sxs-lookup"><span data-stu-id="9ecf0-176">:::column span="3"::: **Physical security**</span></span>

- <span data-ttu-id="9ecf0-177">Den första försvarslinjen är att fysiskt bygga in säkerhet och styra åtkomsten till maskinvaran i datacentret.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-177">Physical building security and controlling access to computing hardware within the data center is the first line of defense.</span></span>

<span data-ttu-id="9ecf0-178">Avsikten är att ge ett fysiskt skydd mot åtkomst till tillgångarna.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-178">With physical security, the intent is to provide physical safeguards against access to assets.</span></span> <span data-ttu-id="9ecf0-179">Det här gör att andra nivåer inte kan kringgås, samt att förluster eller stöld hanteras korrekt.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-179">This ensures that other layers can't be bypassed, and loss or theft is handled appropriately.</span></span>
  :::column-end:::
:::row-end:::

## <a name="summary"></a><span data-ttu-id="9ecf0-180">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9ecf0-180">Summary</span></span>

<span data-ttu-id="9ecf0-181">Vi har sett att Azure kan hjälpa dig med många av dina säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-181">We've seen here that Azure helps a lot with your security concerns.</span></span> <span data-ttu-id="9ecf0-182">Säkerheten är dock fortfarande ett **delat ansvar**.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-182">But security is still a **shared responsibility**.</span></span> <span data-ttu-id="9ecf0-183">Hur mycket av ansvaret som ligger på oss själva beror på vilken modell vi använder i Azure.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-183">How much of that responsibility falls on us depends on which model we use with Azure.</span></span>

<span data-ttu-id="9ecf0-184">Vi använder ringarna i *skydd på djupet* som riktlinje när vi överväger vilket skydd som är lämpligt för våra data och miljöer.</span><span class="sxs-lookup"><span data-stu-id="9ecf0-184">We use the *defense in depth* rings as a guideline for considering what protections are adequate for our data and environments.</span></span>