<span data-ttu-id="165d5-101">Nu när du känner till fördelarna med Azures datalagring och vilka funktioner som är tillgängliga ska vi titta närmare på hur det skiljer sig från lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="165d5-101">Now that you know about the benefits and features of Azure data storage, let's see how it differs from on-premises storage.</span></span>

<span data-ttu-id="165d5-102">Termen ”lokal” syftar på lagring och underhåll av data på lokala maskiner och servrar.</span><span class="sxs-lookup"><span data-stu-id="165d5-102">The term "on-premises" refers to the storage and maintenance of data on local hardware and servers.</span></span> <span data-ttu-id="165d5-103">Det finns flera faktorer att tänka på när du jämför lokal datalagring med Azures datalagring.</span><span class="sxs-lookup"><span data-stu-id="165d5-103">There are several factors to consider when comparing on-premises to Azure data storage.</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="165d5-104">![Pappersfaktura och ett moln som representerar kostnadseffektivitet](../media/4-cost-effectiveness.png)</span><span class="sxs-lookup"><span data-stu-id="165d5-104">![Paper bill and a cloud representing cost effectiveness](../media/4-cost-effectiveness.png)</span></span>
  :::column-end:::
    <span data-ttu-id="165d5-105">:::column span="3"::: **Kostnadseffektivitet**</span><span class="sxs-lookup"><span data-stu-id="165d5-105">:::column span="3"::: **Cost effectiveness**</span></span>

<span data-ttu-id="165d5-106">En lokal lagringslösning kräver dedikerad maskinvara som måste köpas, installeras, konfigureras och underhållas.</span><span class="sxs-lookup"><span data-stu-id="165d5-106">An on-premises storage solution requires dedicated hardware that needs to be purchased, installed, configured, and maintained.</span></span> <span data-ttu-id="165d5-107">Detta kan innebära en betydande investeringskostnad (eller kapitalkostnad).</span><span class="sxs-lookup"><span data-stu-id="165d5-107">This can be a significant up-front expense (or capital cost).</span></span> <span data-ttu-id="165d5-108">Ändringar av kraven kan kräva investeringar i ny maskinvara.</span><span class="sxs-lookup"><span data-stu-id="165d5-108">Change in requirements can require investment in new hardware.</span></span> <span data-ttu-id="165d5-109">Maskinvaran måste kunna hantera toppar i efterfrågan, vilket innebär att den kan vara inaktiv eller outnyttjad under perioder med låg belastning.</span><span class="sxs-lookup"><span data-stu-id="165d5-109">Your hardware needs to be capable of handling peak demand which means it may sit idle or be under-utilized in off-peak times.</span></span>

<span data-ttu-id="165d5-110">Azures datalagring tillhandahåller en prismodell där du betalar per användning, vilket ofta är tilltalande för företag eftersom det rör sig om löpande kostnader snarare än initiala investeringskostnader.</span><span class="sxs-lookup"><span data-stu-id="165d5-110">Azure data storage provides a pay-as-you-go pricing model which is often appealing to businesses as an operating expense instead of an upfront capital cost.</span></span> <span data-ttu-id="165d5-111">Det är också skalbart, vilket innebär att du kan skalanpassa lösningen i takt med efterfrågan.</span><span class="sxs-lookup"><span data-stu-id="165d5-111">It's also scalable, allowing you to scale up or scale out as demand dictates and scale back when demand is low.</span></span> <span data-ttu-id="165d5-112">Du debiteras endast för de datatjänster som du behöver.</span><span class="sxs-lookup"><span data-stu-id="165d5-112">You are charged for data services only as you need them.</span></span>

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="165d5-113">![Certifikat som representerar tillförlitlighet](../media/4-reliability.png)</span><span class="sxs-lookup"><span data-stu-id="165d5-113">![A certificate representing reliability](../media/4-reliability.png)</span></span>
  :::column-end:::
    <span data-ttu-id="165d5-114">:::column span="3"::: **Tillförlitlighet**</span><span class="sxs-lookup"><span data-stu-id="165d5-114">:::column span="3"::: **Reliability**</span></span>

<span data-ttu-id="165d5-115">Lokal lagring kräver säkerhetskopiering av data, belastningsutjämning och strategier för haveriberedskap.</span><span class="sxs-lookup"><span data-stu-id="165d5-115">On-premises storage requires data backup, load balancing, and disaster recovery strategies.</span></span> <span data-ttu-id="165d5-116">Detta kan vara utmanande och dyrt, och ofta krävs dedikerade servrar som kräver en betydande investering i både maskinvara och IT-resurser.</span><span class="sxs-lookup"><span data-stu-id="165d5-116">These can be challenging and expensive as they often each need dedicated servers requiring a significant investment in both hardware and IT resources.</span></span>

<span data-ttu-id="165d5-117">Azures datalagring tillhandahåller säkerhetskopiering av data, belastningsutjämning, katastrofåterställning och replikering av data som tjänst för att garantera datasäkerhet och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="165d5-117">Azure data storage provides data backup, load balancing, disaster recovery, and data replication as services to ensure data safety and high availability.</span></span>

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="165d5-118">![En unikt formad byggnad som representerar olika lagringstyper](../media/4-storage-types.png)</span><span class="sxs-lookup"><span data-stu-id="165d5-118">![A uniquely shaped building representing different storage types](../media/4-storage-types.png)</span></span>
  :::column-end:::
    <span data-ttu-id="165d5-119">:::column span="3"::: **Lagringstyper**</span><span class="sxs-lookup"><span data-stu-id="165d5-119">:::column span="3"::: **Storage types**</span></span>

<span data-ttu-id="165d5-120">Ibland krävs flera olika lagringstyper för en lösning, t.ex fil- och databaslagring.</span><span class="sxs-lookup"><span data-stu-id="165d5-120">Sometimes multiple different storage types are required for a solution, such as file and database storage.</span></span> <span data-ttu-id="165d5-121">En lokal metod kräver ofta flera servrar och administrativa verktyg för respektive lagringstyp.</span><span class="sxs-lookup"><span data-stu-id="165d5-121">An on-premises approach often requires numerous servers and administrative tools for each storage type.</span></span>

<span data-ttu-id="165d5-122">Azures datalagring tillhandahåller en mängd olika lagringsalternativ, däribland distribuerad åtkomst och nivåindelad lagring.</span><span class="sxs-lookup"><span data-stu-id="165d5-122">Azure data storage provides a variety of different storage options including distributed access and tiered storage.</span></span> <span data-ttu-id="165d5-123">Detta gör det möjligt att integrera en kombination av lagringstekniker som ger det bästa lagringsalternativet för varje del av din lösning.</span><span class="sxs-lookup"><span data-stu-id="165d5-123">This makes it possible to integrate a combination of storage technologies providing the best storage choice for each part of your solution.</span></span>

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="165d5-124">![En sportbok som representerar flexibilitet](../media/4-agility.png)</span><span class="sxs-lookup"><span data-stu-id="165d5-124">![A sports playbook representing agility](../media/4-agility.png)</span></span>
  :::column-end:::
    <span data-ttu-id="165d5-125">:::column span="3"::: **Flexibilitet**</span><span class="sxs-lookup"><span data-stu-id="165d5-125">:::column span="3"::: **Agility**</span></span>

<span data-ttu-id="165d5-126">Krav och tekniker ändras.</span><span class="sxs-lookup"><span data-stu-id="165d5-126">Requirements and technologies change.</span></span> <span data-ttu-id="165d5-127">För en lokal distribution kan detta innebära att nya servrar och delar av infrastrukturen måste etableras och distribueras, vilket är både tidskrävande och dyrt.</span><span class="sxs-lookup"><span data-stu-id="165d5-127">For an on-premises deployment this may mean provisioning and deploying new servers and infrastructure pieces, which is a time consuming and expensive activity.</span></span>

<span data-ttu-id="165d5-128">Azures datalagring ger dig möjlighet att skapa nya tjänster på några minuter.</span><span class="sxs-lookup"><span data-stu-id="165d5-128">Azure data storage gives you the flexibility to create new services in minutes.</span></span> <span data-ttu-id="165d5-129">Med den här flexibiliteten kan du snabbt ändra lagringsservrar utan att behöva göra någon betydande maskinvaruinvestering.</span><span class="sxs-lookup"><span data-stu-id="165d5-129">This flexibility allows you to change storage back-ends quickly without needing a significant hardware investment.</span></span>

<span data-ttu-id="165d5-130">Följande bild visar skillnaderna mellan lokal lagring och Azures datalagring.</span><span class="sxs-lookup"><span data-stu-id="165d5-130">The following illustration shows differences between on-premise storage and Azure data storage.</span></span>

![En bild som visar en jämförelse av lokal lagring och Azures datalagring för flera vanliga affärsbehov.](../media/4-Comparison.png)

  :::column-end:::
:::row-end:::