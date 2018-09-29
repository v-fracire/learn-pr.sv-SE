<span data-ttu-id="873b3-101">Anta att du använder en lokal PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="873b3-101">Let's assume that you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="873b3-102">Du hanterar alla säkerhetsaspekter och har låst all åtkomst till dina servrar med brandväggsregler på standardservernivå i PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="873b3-102">You're managing all security aspects and you've locked down all access to your servers using the standard PostgreSQL server-level firewall rules.</span></span> <span data-ttu-id="873b3-103">Nu har du goda kunskaper om hur du konfigurerar brandväggsregler på samma servernivå i Azure.</span><span class="sxs-lookup"><span data-stu-id="873b3-103">You now have a good understanding of how to configure the same server-level firewall rules in Azure.</span></span>

<span data-ttu-id="873b3-104">Vi ska ansluta till en av de Azure Database for PostgreSQL-servrar som du skapade.</span><span class="sxs-lookup"><span data-stu-id="873b3-104">Let's connect to one of the Azure Database for PostgreSQL servers that you created.</span></span>

## <a name="allow-azure-service-access"></a><span data-ttu-id="873b3-105">Tillåta åtkomst till Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="873b3-105">Allow Azure service access</span></span>

<span data-ttu-id="873b3-106">Innan vi börjar måste du tillåta åtkomst till Azure-tjänster, för att kunna använda PowerShell och `psql` för att kunna ansluta till din server.</span><span class="sxs-lookup"><span data-stu-id="873b3-106">Before we begin, you'll have to allow access to Azure services if you want to use PowerShell and `psql` to connect to your server.</span></span> <span data-ttu-id="873b3-107">Kom ihåg att du kan tillåta åtkomst på två sätt.</span><span class="sxs-lookup"><span data-stu-id="873b3-107">Recall that you can allow access in two ways.</span></span>

<span data-ttu-id="873b3-108">Ditt första alternativ är att aktivera **Allow access to Azure services** (Tillåt åtkomst till Azure-tjänster).</span><span class="sxs-lookup"><span data-stu-id="873b3-108">Your first option is to enable **Allow access to Azure services**.</span></span> <span data-ttu-id="873b3-109">Om du tillåter åtkomst skapas en brandväggsregel även om regeln inte finns med på listan över anpassade regler som du skapar.</span><span class="sxs-lookup"><span data-stu-id="873b3-109">Allowing access will create a firewall rule even though the rule isn't entered in the list of custom rules you create.</span></span>

<span data-ttu-id="873b3-110">Ditt andra alternativ är att skapa en brandväggsregel som tillåter åtkomst till alla IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="873b3-110">Your second option is to create a firewall rule that allows access to all IP addresses.</span></span> <span data-ttu-id="873b3-111">Regeln innehåller IP-adressen till klienten för det PowerShell som du använder till att köra `psql` från.</span><span class="sxs-lookup"><span data-stu-id="873b3-111">The rule will include the IP address for the client running PowerShell that you'll use to execute `psql` from.</span></span>

<span data-ttu-id="873b3-112">Du måste också inaktivera alternativet **Framtvinga SSL-anslutning**.</span><span class="sxs-lookup"><span data-stu-id="873b3-112">You also need to disable the **Enforce SSL connection** option.</span></span>

### <a name="create-a-firewall-rule"></a><span data-ttu-id="873b3-113">Skapa en brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="873b3-113">Create a firewall rule</span></span>

1. <span data-ttu-id="873b3-114">Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du aktiverade sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="873b3-114">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="873b3-115">Gå till serverresursen du vill skapa en brandväggsregel för.</span><span class="sxs-lookup"><span data-stu-id="873b3-115">Navigate to the server resource for which you would like to create a firewall rule.</span></span>

1. <span data-ttu-id="873b3-116">Välj alternativet **Anslutningssäkerhet** för att öppna bladet Anslutningssäkerhet till höger.</span><span class="sxs-lookup"><span data-stu-id="873b3-116">Select the **Connection Security** option to open the connection security blade to the right.</span></span>

    ![Skärmbild av Azure Portal som visar avsnittet Anslutningssäkerhet på resursbladet för PostgreSQL-databasen](../media/7-db-security-settings.png)

<span data-ttu-id="873b3-118">Kom ihåg att du vill tillåta åtkomst till PowerShell-klienter som kör `psql`.</span><span class="sxs-lookup"><span data-stu-id="873b3-118">Recall that you want to allow access to PowerShell clients running `psql`.</span></span>

<span data-ttu-id="873b3-119">Du kan välja att antingen:</span><span class="sxs-lookup"><span data-stu-id="873b3-119">You can choose to either:</span></span>

- <span data-ttu-id="873b3-120">Ställ in **Allow access to Azure services** (Tillåt åtkomst till Azure-tjänster) på **PÅ**</span><span class="sxs-lookup"><span data-stu-id="873b3-120">Set **Allow access to Azure services** to **ON**</span></span>
- <span data-ttu-id="873b3-121">Ställ in **Framtvinga SSL-anslutning** på **INAKTIVERAD**</span><span class="sxs-lookup"><span data-stu-id="873b3-121">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="873b3-122">Klicka på knappen **Spara** för att spara ändringarna</span><span class="sxs-lookup"><span data-stu-id="873b3-122">Click the **Save** button to save your changes</span></span>

<span data-ttu-id="873b3-123">Eller så kan du lägga till en brandväggsregel för att tillåta åtkomst till alla IP-adresser genom att lägga till en brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="873b3-123">Or, you can add a firewall rule to allow access to all IP addresses by adding a firewall rule.</span></span> <span data-ttu-id="873b3-124">Ange följande värden:</span><span class="sxs-lookup"><span data-stu-id="873b3-124">Use the following values:</span></span>

- <span data-ttu-id="873b3-125">Regelnamn: `AllowAll`</span><span class="sxs-lookup"><span data-stu-id="873b3-125">Rule Name: `AllowAll`</span></span>
- <span data-ttu-id="873b3-126">Start-IP: `0.0.0.0`</span><span class="sxs-lookup"><span data-stu-id="873b3-126">Start IP: `0.0.0.0`</span></span>
- <span data-ttu-id="873b3-127">Slut-IP: `255.255.255.255`</span><span class="sxs-lookup"><span data-stu-id="873b3-127">End IP: `255.255.255.255`</span></span>
- <span data-ttu-id="873b3-128">Ställ in **Framtvinga SSL-anslutning** på **INAKTIVERAD**</span><span class="sxs-lookup"><span data-stu-id="873b3-128">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="873b3-129">Klicka på knappen **Spara** för att spara ändringarna</span><span class="sxs-lookup"><span data-stu-id="873b3-129">Click the **Save** button to save your changes</span></span>

> [!Warning]
> <span data-ttu-id="873b3-130">Om du skapar den här brandväggsregeln får alla IP-adresser på Internet tillåtelse att försöka ansluta till din server.</span><span class="sxs-lookup"><span data-stu-id="873b3-130">Creating this firewall rule will allow any IP address on the Internet to attempt to connect to your server.</span></span> <span data-ttu-id="873b3-131">I produktionsmiljöer vill du begränsa åtkomsten till specifika IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="873b3-131">In production environments, you'll want to restrict access to specific IP addresses.</span></span>

### <a name="connect-to-the-database-with-psql"></a><span data-ttu-id="873b3-132">Ansluta till databasen med psql</span><span class="sxs-lookup"><span data-stu-id="873b3-132">Connect to the database with psql</span></span>

1. <span data-ttu-id="873b3-133">Anslut PSQL till servern med följande kommando i Azure Cloud Shell till höger.</span><span class="sxs-lookup"><span data-stu-id="873b3-133">In the Azure Cloud Shell on the right, connect PSQL to your server using the following command.</span></span> <span data-ttu-id="873b3-134">Byt ut servernamnet och administratörsnamnet.</span><span class="sxs-lookup"><span data-stu-id="873b3-134">Make sure to replace the server name and admin name.</span></span>

    ```bash
    psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
    ```

    <span data-ttu-id="873b3-135">Använd de värden som du valde för `server-name` och `admin-user`.</span><span class="sxs-lookup"><span data-stu-id="873b3-135">Use the values you chose for the `server-name`, and `admin-user`.</span></span>

1. <span data-ttu-id="873b3-136">**postgres** är standarddatabasen för hantering som varje PostgreSQL-server skapas med.</span><span class="sxs-lookup"><span data-stu-id="873b3-136">**postgres** is the default management database every PostgreSQL server is created with.</span></span> <span data-ttu-id="873b3-137">Du blir uppmanad att ange lösenordet som du angav när du skapade servern.</span><span class="sxs-lookup"><span data-stu-id="873b3-137">You'll be prompted for the password you provided when you created the server.</span></span>

1. <span data-ttu-id="873b3-138">När du är ansluten kör du kommandot <kbd>\l</kbd> för att visa en lista med alla databaser.</span><span class="sxs-lookup"><span data-stu-id="873b3-138">Once successfully connected, execute the <kbd>\l</kbd> command to list all databases.</span></span> <span data-ttu-id="873b3-139">Det här kommandot leder till att två eller fler standarddatabaser returneras.</span><span class="sxs-lookup"><span data-stu-id="873b3-139">This command will result in two or more default databases returned.</span></span>

1. <span data-ttu-id="873b3-140">Tryck på <kbd>q</kbd> för att avsluta listan.</span><span class="sxs-lookup"><span data-stu-id="873b3-140">Hit <kbd>q</kbd> to exit the list.</span></span>

1. <span data-ttu-id="873b3-141">Du kan prova andra PSQL-kommandon.</span><span class="sxs-lookup"><span data-stu-id="873b3-141">You can try other PSQL commands.</span></span>
    - <kbd>\?</kbd> <span data-ttu-id="873b3-142">Få hjälp.</span><span class="sxs-lookup"><span data-stu-id="873b3-142">to get help.</span></span>
    - <span data-ttu-id="873b3-143"><kbd>\dt</kbd> för att göra en lista med tabellerna.</span><span class="sxs-lookup"><span data-stu-id="873b3-143"><kbd>\dt</kbd> to list the tables.</span></span>

1. <span data-ttu-id="873b3-144">När du är klar med att köra PSQL-åtgärder på servern, kör du kommandot <kbd>\q</kbd> för att avsluta PSQL.</span><span class="sxs-lookup"><span data-stu-id="873b3-144">When you're finished executing PSQL operations on your server, execute the command <kbd>\q</kbd> to quit PSQL.</span></span>
