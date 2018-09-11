<span data-ttu-id="2a4b3-101">Din vårdorganisationen lagrar personlig och potentiellt känslig kundinformation.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-101">Your healthcare organization stores personal and potentially sensitive client data.</span></span> <span data-ttu-id="2a4b3-102">En säkerhetsincident kan avslöja känsliga data, vilket kan orsaka personliga svårigheter eller ekonomisk skada.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-102">A security incident could expose this sensitive data, which could cause personal embarrassment or financial harm.</span></span> <span data-ttu-id="2a4b3-103">Hur säkerställer du dataintegriteten och ser till att dina system är säkra?</span><span class="sxs-lookup"><span data-stu-id="2a4b3-103">How do you ensure the integrity of their data and ensure your systems are secure?</span></span> 

<span data-ttu-id="2a4b3-104">Här talar vi om hur du hanterar säkerheten för en arkitektur.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-104">Here, we'll talk about how to approach the security of an architecture.</span></span>

## <a name="what-should-i-protect"></a><span data-ttu-id="2a4b3-105">Vad ska jag skydda?</span><span class="sxs-lookup"><span data-stu-id="2a4b3-105">What should I protect?</span></span>

<span data-ttu-id="2a4b3-106">Informationen som organisationen lagrar är hjärtat i skyddbara tillgångar.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-106">The data your organization stores is at the heart of your securable assets.</span></span> <span data-ttu-id="2a4b3-107">Dessa data kan vara känslig information om kunder, ekonomisk information om din organisation eller kritiska affärsdata som stödjer organisationen.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-107">This data could be sensitive data about customers, financial information about your organization, or critical line-of-business data supporting your organization.</span></span> <span data-ttu-id="2a4b3-108">Tillsammans med data är det även avgörande att skydda infrastrukturen som data finns på och de identiteter som används för att få åtkomst till dem.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-108">Along with data, securing the infrastructure it exists on, and the identities we use to access it, are also critically important.</span></span>

<span data-ttu-id="2a4b3-109">Dina data kan omfattas av ytterligare juridiska och bestämmelsemässiga krav beroende på var du finns, vilken typ av data du lagrar eller vilken bransch som ditt program verkar i.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-109">Your data may be subject to additional legal and regulatory requirements depending on where you are located, the type of data you are storing, or the industry that your application operates in.</span></span> <span data-ttu-id="2a4b3-110">Exempel: Inom sjukvården i USA finns det en lag som kallas HIPAA (Health Insurance Portability and Accountability Act).</span><span class="sxs-lookup"><span data-stu-id="2a4b3-110">For instance, in the healthcare industry in the US, there is a law called the Health Insurance Portability and Accountability Act (HIPAA).</span></span> <span data-ttu-id="2a4b3-111">Organisationer som lagrar data som omfattas av den här lagen måste se till att vissa skydd är på plats.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-111">Organizations that store data that is in scope for this law are required to ensure certain safeguards are in place.</span></span> <span data-ttu-id="2a4b3-112">I Europa anger den allmänna dataskyddsförordningen, GDPR, reglerna för hur personuppgifter skyddas och definierar personers rättigheter som rör lagrade data.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-112">In Europe, the General Data Protection Regulation (GDPR) lays out the rules of how personal data is protected, and defines individuals' rights related to stored data.</span></span> <span data-ttu-id="2a4b3-113">Vissa länder kräver att vissa typer av data inte lämnar landets gränser.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-113">Some countries require that certain types of data do not leave their borders.</span></span>

<span data-ttu-id="2a4b3-114">När ett intrång sker kan det leda till betydande effekter för ekonomin och ryktet för både organisationer och kunder.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-114">When a security breach occurs, there can be substantial impacts to the finances and reputation of both organizations and customers.</span></span> <span data-ttu-id="2a4b3-115">Det bryter ned den förtroende som kunderna har för din organisation och kan påverka tillståndet på lång sikt.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-115">This breaks down the trust customers are willing to instill in your organization, and can impact its long-term health.</span></span>

## <a name="a-multilayered-approach"></a><span data-ttu-id="2a4b3-116">En metod med flera lager</span><span class="sxs-lookup"><span data-stu-id="2a4b3-116">A multilayered approach</span></span>

<span data-ttu-id="2a4b3-117">En metod med flera lager för att säkra miljön ökar dess säkerhetsposition.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-117">A multilayered approach to securing your environment will increase the security posture of your environment.</span></span> <span data-ttu-id="2a4b3-118">Vi kan bryta ned lagren enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2a4b3-118">We can break down the layers as follows:</span></span>

* <span data-ttu-id="2a4b3-119">Data</span><span class="sxs-lookup"><span data-stu-id="2a4b3-119">Data</span></span>
* <span data-ttu-id="2a4b3-120">Program</span><span class="sxs-lookup"><span data-stu-id="2a4b3-120">Applications</span></span>
* <span data-ttu-id="2a4b3-121">VM/beräkning</span><span class="sxs-lookup"><span data-stu-id="2a4b3-121">VM/compute</span></span>
* <span data-ttu-id="2a4b3-122">Nätverk</span><span class="sxs-lookup"><span data-stu-id="2a4b3-122">Networking</span></span>
* <span data-ttu-id="2a4b3-123">Perimeternätverk</span><span class="sxs-lookup"><span data-stu-id="2a4b3-123">Perimeter</span></span>
* <span data-ttu-id="2a4b3-124">Principer och åtkomst</span><span class="sxs-lookup"><span data-stu-id="2a4b3-124">Policies & access</span></span>
* <span data-ttu-id="2a4b3-125">Fysisk säkerhet</span><span class="sxs-lookup"><span data-stu-id="2a4b3-125">Physical security</span></span>

<span data-ttu-id="2a4b3-126">Varje lager fokuserar på ett område där attacker kan ske och skapar ett djupgående skydd, om ett lager misslyckas eller kringgås av angriparen.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-126">Each layer focuses on a different area where attacks can happen and creates a depth of protection, should one layer fail or be bypassed by an attacker.</span></span> <span data-ttu-id="2a4b3-127">Om vi bara skulle fokusera på ett lager skulle angriparna ha ohämmad åtkomst till din miljö om de tar sig igenom det lagret.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-127">If we were to just focus on one layer, an attacker would have unfettered access to your environment should they get through this layer.</span></span> <span data-ttu-id="2a4b3-128">Att hantera säkerheten i flera lager ökar arbetet som en angripare måste utföra för att få åtkomst till dina system och data.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-128">Addressing security in layers increases the work an attacker must do to gain access to your systems and data.</span></span> <span data-ttu-id="2a4b3-129">Varje lager skulle ha olika säkerhetskontroller, tekniker och funktioner som gäller.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-129">Each layer will have different security controls, technologies, and capabilities that will apply.</span></span> <span data-ttu-id="2a4b3-130">Vid identifiering av vilka skydd som ska tillämpas tas det ofta hänsyn till kostnaden, som måste balanseras mot affärskraven och den övergripande risken för företaget.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-130">When identifying the protections to put in place, cost will often be of concern, and will need to be balanced with business requirements and overall risk to the business.</span></span>

![Säkerhetslager](../media-draft/security-layers.png)

## <a name="types-of-attacks-and-best-practices"></a><span data-ttu-id="2a4b3-132">Typer av attacker och bästa praxis</span><span class="sxs-lookup"><span data-stu-id="2a4b3-132">Types of attacks and best practices</span></span>

<span data-ttu-id="2a4b3-133">På varje lager finns det några vanliga attacker som du vill skydda mot.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-133">At each layer, there are some common attacks that you will want to protect against.</span></span> <span data-ttu-id="2a4b3-134">Dessa är inte heltäckande men kan ge dig en uppfattning om hur varje lager kan angripas och vilka typer av skydd du kan behöva titta på.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-134">These are not all-inclusive, but can give you an idea of how each layer can be attacked and what types of protections you may need to look at.</span></span>

* <span data-ttu-id="2a4b3-135">**Datanivå**: Exponering av krypteringsnyckeln eller användning av en svag kryptering kan göra dina data sårbara för obehörig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-135">**Data layer**: Encryption key exposure or using weak encryption can leave your data vulnerable should unauthorized access occur.</span></span>
* <span data-ttu-id="2a4b3-136">**Programnivå**: Inmatning och körning av skadlig kod är kännetecknande för attacker på programnivå.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-136">**Application layer**: Malicious code injection and execution are the hallmarks of application-layer attacks.</span></span> <span data-ttu-id="2a4b3-137">Vanliga attacker är SQL-inmatning och skriptkörning över flera webbplatser (XSS).</span><span class="sxs-lookup"><span data-stu-id="2a4b3-137">Common attacks include SQL injection and cross-site scripting (XSS).</span></span>
* <span data-ttu-id="2a4b3-138">**VM/beräkning-nivå**: Skadlig kod är en vanlig metod för att angripa en miljö, vilket inbegriper körning av skadlig kod för att kompromettera ett system.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-138">**VM/compute layer**: Malware is a common method of attacking an environment, which involves executing malicious code to compromise a system.</span></span> <span data-ttu-id="2a4b3-139">När den skadliga koden finns i systemet leder vidare attacker till avslöjande av autentiseringsuppgifter och laterala rörelser i hela miljö kan förekomma.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-139">Once malware is present on a system, further attacks leading to credential exposure and lateral movement throughout the environment can occur.</span></span>
* <span data-ttu-id="2a4b3-140">**Nätverksnivå**: Onödiga öppna portar till internet är en vanlig attackmetod.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-140">**Networking layer**: Unnecessary open ports to the Internet are a common method of attack.</span></span> <span data-ttu-id="2a4b3-141">Det kan vara att lämna SSH eller RDP öppet för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-141">These could include leaving SSH or RDP open to virtual machines.</span></span> <span data-ttu-id="2a4b3-142">När dessa är öppna kan det ske råstyrkeattacker mot dina system när angripare försöker få åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-142">When open, these could allow brute-force attacks against your systems as attackers attempt to gain access.</span></span>
* <span data-ttu-id="2a4b3-143">**Perimeternivå**: DoS-attacker sker ofta på den här nivån.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-143">**Perimeter layer**: Denial-of-service (DoS) attacks are often seen at this layer.</span></span> <span data-ttu-id="2a4b3-144">Dessa attacker försöker överbelasta nätverksresurserna och tvinga dem att koppa från eller göra dem oförmögna att svara på legitima begäranden.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-144">These attacks attempt to overwhelm network resources, forcing them to go  offline or making them incapable of responding to legitimate requests.</span></span>
* <span data-ttu-id="2a4b3-145">**Princip- och åtkomstnivå**: Här sker autentiseringen för ditt program.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-145">**Policies & access layer**: This is where authentication occurs for your application.</span></span> <span data-ttu-id="2a4b3-146">Det här kan inbegripa moderna autentiseringsprotokoll som OpenID Connect, OAuth eller Kerberos-baserad autentisering som Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-146">This could include modern authentication protocols such as OpenID Connect, OAuth, or Kerberos-based authentication such as Active Directory.</span></span> <span data-ttu-id="2a4b3-147">Exponerade autentiseringsuppgifter är en risk här och det är viktigt att begränsa antalet identitetsbehörigheter för identiteter.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-147">Exposed credentials are a risk here and it's important to limit the number of identities permissions of identities.</span></span> <span data-ttu-id="2a4b3-148">Vi vill även ha övervakning på plats för att leta efter möjliga kapade konton, till exempel inloggningar från ovanliga platser.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-148">We also want to have monitoring in place to look for possible compromised accounts, such as logins coming from unusual places.</span></span>
* <span data-ttu-id="2a4b3-149">**Fysisk nivå**: Obehörig åtkomst till resurser genom metoder som ”door drafting” och stöld av säkerhetsmärken kan ske på den här nivån.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-149">**Physical layer**: Unauthorized access to facilities through methods such as door drafting and theft of security badges can be seen at this layer.</span></span>

<span data-ttu-id="2a4b3-150">Det finns inga enskilda system, kontroller eller tekniker som skyddar din arkitektur helt.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-150">There is no single security system, control, or technology that will fully protect your architecture.</span></span> <span data-ttu-id="2a4b3-151">Säkerhet handlar inte bara om teknik, utan även om personer och processer.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-151">Security is more than just technology, it's also about people and processes.</span></span> <span data-ttu-id="2a4b3-152">Att skapa en miljö som ser holistiskt på säkerhet och gör det till ett krav som standard hjälper till att skydda din organisation så mycket som möjligt.</span><span class="sxs-lookup"><span data-stu-id="2a4b3-152">Creating an environment that looks holistically at security, and making it a requirement by default will help ensure your organization is as secure as possible.</span></span>