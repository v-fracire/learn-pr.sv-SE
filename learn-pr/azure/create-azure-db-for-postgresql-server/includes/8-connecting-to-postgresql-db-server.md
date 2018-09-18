<span data-ttu-id="a317f-101">Anta att du använder en lokal PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a317f-101">Lets' assume you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="a317f-102">Du hanterar alla säkerhetsaspekter och låser all åtkomst till dina servrar med brandväggsregler på PostgreSQL-standardservernivå.</span><span class="sxs-lookup"><span data-stu-id="a317f-102">You're managing all security aspects and locked down all access to your servers using the standard PostgreSQL server level firewall rules.</span></span> <span data-ttu-id="a317f-103">Nu har du goda kunskaper om hur du konfigurerar brandväggsregler på samma servernivå i Azure.</span><span class="sxs-lookup"><span data-stu-id="a317f-103">You now have a good understanding of how to configure the same server level firewall rules in Azure.</span></span>

<span data-ttu-id="a317f-104">Nu har du chansen att ansluta till någon av de tidigare Azure-databaserna för PostgreSQL-servrar du har skapat med `psql`.</span><span class="sxs-lookup"><span data-stu-id="a317f-104">You now have the chance to connect to one of the previous Azure Databases for PostgreSQL servers you created using `psql`.</span></span>

## <a name="allow-azure-service-access"></a><span data-ttu-id="a317f-105">Tillåta åtkomst till Azure-tjänst</span><span class="sxs-lookup"><span data-stu-id="a317f-105">Allow Azure service access</span></span>

<span data-ttu-id="a317f-106">Innan vi börjar.</span><span class="sxs-lookup"><span data-stu-id="a317f-106">Before we begin.</span></span> <span data-ttu-id="a317f-107">Du måste tillåta åtkomst till Azure-tjänster om du vill använda PowerShell och `psql` för att ansluta till din server.</span><span class="sxs-lookup"><span data-stu-id="a317f-107">You'll have to allow access to Azure services if you want to use PowerShell and `psql` to connect to your server.</span></span> <span data-ttu-id="a317f-108">Kom ihåg att du kan tillåta åtkomst på två sätt.</span><span class="sxs-lookup"><span data-stu-id="a317f-108">Recall, that you can allow access in two ways.</span></span>

<span data-ttu-id="a317f-109">Ditt första alternativ är att aktivera **Allow access to Azure services** (Tillåt åtkomst till Azure-tjänster).</span><span class="sxs-lookup"><span data-stu-id="a317f-109">Your first option is to enable **Allow access to Azure services**.</span></span> <span data-ttu-id="a317f-110">Om du tillåter åtkomst skapas en brandväggsregel även om regeln inte finns med på listan över anpassade regler som du skapar.</span><span class="sxs-lookup"><span data-stu-id="a317f-110">Allowing access will create a firewall rule even though the rule isn't entered in the list of custom rules you create.</span></span>

<span data-ttu-id="a317f-111">Ditt andra alternativ är att skapa en brandväggsregel som tillåter åtkomst till alla IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="a317f-111">Your second option is to create a firewall rule that allows access to all IP addresses.</span></span> <span data-ttu-id="a317f-112">Regeln innehåller IP-adressen för den klient som kör PowerShell som du använder för att köra `psql` från.</span><span class="sxs-lookup"><span data-stu-id="a317f-112">The rule will include the IP address for the client running PowerShell that you'll use to execute `psql` from.</span></span>

<span data-ttu-id="a317f-113">Du måste också inaktivera **Framtvinga SSL-anslutning**.</span><span class="sxs-lookup"><span data-stu-id="a317f-113">You also need to disable the **Enforce SSL connection**.</span></span>

<span data-ttu-id="a317f-114">Nu sätter vi igång.</span><span class="sxs-lookup"><span data-stu-id="a317f-114">Let's begin.</span></span>

<span data-ttu-id="a317f-115">Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="a317f-115">Sign in to [the Azure portal](https://portal.azure.com?azure-portal=true).</span></span> <span data-ttu-id="a317f-116">Gå till serverresursen du vill skapa en brandväggsregel för.</span><span class="sxs-lookup"><span data-stu-id="a317f-116">Navigate to the server resource for which you would like to create a firewall rule.</span></span>

<span data-ttu-id="a317f-117">Välj alternativet **Anslutningssäkerhet** för att öppna bladet Anslutningssäkerhet till höger.</span><span class="sxs-lookup"><span data-stu-id="a317f-117">Select the **Connection Security** option to open the connection security blade to the right.</span></span>

![Säkerhetsinställningar för PostgreSQL-databas](../media-draft/7-db-security-settings.png)

<span data-ttu-id="a317f-119">Kom ihåg att du inte vill tillåta åtkomst till PowerShell-klienter som kör `psql`.</span><span class="sxs-lookup"><span data-stu-id="a317f-119">Recall, you want to allow access to PowerShell clients running `psql`.</span></span>

<span data-ttu-id="a317f-120">Du kan välja att antingen:</span><span class="sxs-lookup"><span data-stu-id="a317f-120">You can choose to either:</span></span>

- <span data-ttu-id="a317f-121">Ställ in **Allow access to Azure services** (Tillåt åtkomst till Azure-tjänster) på **PÅ**</span><span class="sxs-lookup"><span data-stu-id="a317f-121">Set **Allow access to Azure services** to **ON**</span></span>
- <span data-ttu-id="a317f-122">Ställ in **Framtvinga SSL-anslutning** på **INAKTIVERAD**</span><span class="sxs-lookup"><span data-stu-id="a317f-122">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="a317f-123">Klicka på knappen **Spara** för att spara ändringarna</span><span class="sxs-lookup"><span data-stu-id="a317f-123">Click the **Save** button to save your changes</span></span>

<span data-ttu-id="a317f-124">Eller så kan du lägga till en brandväggsregel för att tillåta åtkomst till alla IP-adresser genom att lägga till en brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="a317f-124">Or, you can add a firewall rule to allow access to all IP addresses by adding a firewall rule.</span></span> <span data-ttu-id="a317f-125">Ange följande värden:</span><span class="sxs-lookup"><span data-stu-id="a317f-125">Use the following values:</span></span>

- <span data-ttu-id="a317f-126">Regelnamn: `AllowAll`</span><span class="sxs-lookup"><span data-stu-id="a317f-126">Rule Name: `AllowAll`</span></span>
- <span data-ttu-id="a317f-127">Start-IP: `0.0.0.0`</span><span class="sxs-lookup"><span data-stu-id="a317f-127">Start IP: `0.0.0.0`</span></span>
- <span data-ttu-id="a317f-128">Slut-IP: `255.255.255.255`</span><span class="sxs-lookup"><span data-stu-id="a317f-128">End IP: `255.255.255.255`</span></span>
- <span data-ttu-id="a317f-129">Ställ in **Framtvinga SSL-anslutning** på **INAKTIVERAD**</span><span class="sxs-lookup"><span data-stu-id="a317f-129">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="a317f-130">Klicka på knappen **Spara** för att spara ändringarna</span><span class="sxs-lookup"><span data-stu-id="a317f-130">Click the **Save** button to save your changes</span></span>

> [!Warning]
> <span data-ttu-id="a317f-131">Om du skapar den här brandväggsregeln får alla IP-adresser på Internet tillåtelse att försöka ansluta till din server.</span><span class="sxs-lookup"><span data-stu-id="a317f-131">Creating this firewall rule will allow any IP address on the Internet to attempt to connect to your server.</span></span> <span data-ttu-id="a317f-132">Trots att klienter inte får åtkomst till servern utan användarnamnet och lösenordet ska du aktivera den här regeln med försiktighet och kontrollera att du förstår säkerhetsriskerna.</span><span class="sxs-lookup"><span data-stu-id="a317f-132">Even though clients will not be able access the server without the username and password, enable this rule with caution and make sure you understand the security implications.</span></span> <span data-ttu-id="a317f-133">I produktionsmiljöer tillåter du endast åtkomst till specifika IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="a317f-133">In production environments, you'll only allow access to specific client IP addresses.</span></span>

<span data-ttu-id="a317f-134">För nästa steg startar du en Azure Cloud Shell-session.</span><span class="sxs-lookup"><span data-stu-id="a317f-134">For the next steps, you'll start an Azure Cloud Shell session.</span></span> <span data-ttu-id="a317f-135">I den här labbuppgiften används `bash` som kommandoradsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a317f-135">This lab uses `bash` as the command-line environment.</span></span>

- <span data-ttu-id="a317f-136">Öppna Open Shell från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a317f-136">Open the Cloud Shell from the Azure portal.</span></span> <span data-ttu-id="a317f-137">Gå till [Azure-portalen ](https://portal.azure.com?azure-portal=true) och klicka på Open Cloud Shell-knappen:</span><span class="sxs-lookup"><span data-stu-id="a317f-137">Go to [Azure portal](https://portal.azure.com?azure-portal=true) and click the Open Cloud Shell button:</span></span>

  ![Cloud Shell-knapp](../media-draft/cloud-shell-button.png)

- <span data-ttu-id="a317f-139">Om du har flera prenumerationer kontrollerar du att du använder rätt prenumeration när du blir ombedd.</span><span class="sxs-lookup"><span data-stu-id="a317f-139">In case you have several subscriptions, check to make sure you're using the correct subscription when asked.</span></span> <span data-ttu-id="a317f-140">Du kan även köra följande kommando för att ange den aktiva prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="a317f-140">You can also run the following command to set the active subscription.</span></span> <span data-ttu-id="a317f-141">Kom ihåg att ersätta nollorna med ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="a317f-141">Remember to replace the zeros with your subscription identifier.</span></span>

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

- <span data-ttu-id="a317f-142">Anslut psql till din server med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a317f-142">Connect psql to your server using the following command:</span></span>

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
  ```

   <span data-ttu-id="a317f-143">Kom ihåg att `server-name` och `admin-user` är värdena du valde för administratörskontot när du skapade servern.</span><span class="sxs-lookup"><span data-stu-id="a317f-143">Recall, `server-name`, and `admin-user` are the values you chose for the administrator account when you created the server.</span></span> <span data-ttu-id="a317f-144">`postgres` är standarddatabasen för hantering som varje PostgreSQL-server skapas med.</span><span class="sxs-lookup"><span data-stu-id="a317f-144">`postgres` is the default management database every PostgreSQL server is created with.</span></span> <span data-ttu-id="a317f-145">Du blir uppmanad att ange lösenordet som du angav när du skapade servern.</span><span class="sxs-lookup"><span data-stu-id="a317f-145">You'll be prompted for the password you provided when you created the server.</span></span>

- <span data-ttu-id="a317f-146">När du är ansluten kör du kommandot `\l` för att lista alla databaser.</span><span class="sxs-lookup"><span data-stu-id="a317f-146">Once successfully connected, execute the `\l` command to list all databases.</span></span>

   <span data-ttu-id="a317f-147">Det här kommandot leder till två eller fler standarddatabaser som returneras.</span><span class="sxs-lookup"><span data-stu-id="a317f-147">This command will result in two or more default databases returned from.</span></span>

- <span data-ttu-id="a317f-148">När du är klar med att köra psql-åtgärder på servern kör du kommandot `\q` för att avsluta `psql`.</span><span class="sxs-lookup"><span data-stu-id="a317f-148">When you're finished executing psql operations on your server, execute the command `\q` to quit `psql`.</span></span>

- <span data-ttu-id="a317f-149">För att till slut avsluta Cloud Shell kör du kommandot `exit`.</span><span class="sxs-lookup"><span data-stu-id="a317f-149">Finally, to exit Cloud Shell, execute the command `exit`.</span></span>
