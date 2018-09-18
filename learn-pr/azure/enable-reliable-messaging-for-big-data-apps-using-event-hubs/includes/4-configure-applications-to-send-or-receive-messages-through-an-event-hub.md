<span data-ttu-id="e2842-101">När du har skapat och konfigurerat din händelsehubb måste du konfigurera appar att skicka och ta emot händelsedataströmmar.</span><span class="sxs-lookup"><span data-stu-id="e2842-101">After you have created and configured your event hub, you'll need to configure applications to send and receive event data streams.</span></span>

<span data-ttu-id="e2842-102">Till exempel använder en lösning för bearbetning av betalningar någon form av avsändarapp för att samla in data för kundens kreditkort och en mottagarapp för att verifiera att det registrerade kreditkortet är giltigt.</span><span class="sxs-lookup"><span data-stu-id="e2842-102">For example, a payment processing solution will use some form of sender application to collect customer's credit card data and a receiver application to verify that the credit card is valid.</span></span>

<span data-ttu-id="e2842-103">Det finns skillnader i hur en Java-app konfigureras, jämfört med en .NET-app, men det finns allmänna principer för att aktivera appar för att ansluta till en händelsehubb, och för att skicka eller ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e2842-103">Although there are differences in how a Java application is configured, compared to a .NET application, there are general principles for enabling applications to connect to an event hub, and to successfully send or receive messages.</span></span> <span data-ttu-id="e2842-104">Så även om processen för att redigera Java-textfiler för konfiguration skiljer sig från att förbereda en .NET-app med Visual Studio så är principerna är desamma.</span><span class="sxs-lookup"><span data-stu-id="e2842-104">So, although the process of editing Java configuration text files is different to preparing a .NET application using Visual Studio, the principles are the same.</span></span>

## <a name="what-are-the-minimum-event-hub-application-requirements"></a><span data-ttu-id="e2842-105">Vad är minimikraven för händelsehubbappar?</span><span class="sxs-lookup"><span data-stu-id="e2842-105">What are the minimum Event Hub application requirements?</span></span>

<span data-ttu-id="e2842-106">Om du vill konfigurera en app att skicka meddelanden till en händelsehubb måste du ange följande information, så att appen kan skapa autentiseringsuppgifter för anslutning:</span><span class="sxs-lookup"><span data-stu-id="e2842-106">To configure an application to send messages to an event hub, you must provide the following information, so that the application can create connection credentials:</span></span>

- <span data-ttu-id="e2842-107">Namnrymdsnamn på händelsehubb</span><span class="sxs-lookup"><span data-stu-id="e2842-107">Event hub namespace name</span></span>
- <span data-ttu-id="e2842-108">Namn på händelsehubb</span><span class="sxs-lookup"><span data-stu-id="e2842-108">Event hub name</span></span>
- <span data-ttu-id="e2842-109">Namn på princip för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="e2842-109">Shared access policy name</span></span>
- <span data-ttu-id="e2842-110">Primär delad åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="e2842-110">Primary shared access key</span></span>

<span data-ttu-id="e2842-111">Om du vill konfigurera en app att ta emot meddelanden från en händelsehubb anger du följande information, så att appen kan skapa autentiseringsuppgifter för anslutning:</span><span class="sxs-lookup"><span data-stu-id="e2842-111">To configure an application to receive messages from an event hub, provide the following information, so that the application can create connection credentials:</span></span>

- <span data-ttu-id="e2842-112">Namnrymdsnamn på händelsehubb</span><span class="sxs-lookup"><span data-stu-id="e2842-112">Event hub namespace name</span></span>
- <span data-ttu-id="e2842-113">Namn på händelsehubb</span><span class="sxs-lookup"><span data-stu-id="e2842-113">Event hub name</span></span>
- <span data-ttu-id="e2842-114">Namn på princip för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="e2842-114">Shared access policy name</span></span>
- <span data-ttu-id="e2842-115">Primär delad åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="e2842-115">Primary shared access key</span></span>
- <span data-ttu-id="e2842-116">Namn på lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e2842-116">Storage account name</span></span>
- <span data-ttu-id="e2842-117">Anslutningssträng för lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e2842-117">Storage account connection string</span></span>
- <span data-ttu-id="e2842-118">Namn på lagringskontocontainer</span><span class="sxs-lookup"><span data-stu-id="e2842-118">Storage account container name</span></span>

<span data-ttu-id="e2842-119">Om du har en mottagarapp som lagrar meddelanden i Azure Blob Storage måste du också konfigurera ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e2842-119">If you have a receiver application that stores messages in Azure Blob Storage, you'll also need to first configure a storage account.</span></span>

## <a name="the-azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a><span data-ttu-id="e2842-120">Azure CLI-kommandon för att skapa ett allmänt standardlagringskonto</span><span class="sxs-lookup"><span data-stu-id="e2842-120">The Azure CLI commands for creating a general-purpose standard storage account</span></span>

1. <span data-ttu-id="e2842-121">Skapa ett V2-lagringskonto för generell användning i din resursgrupp och på samma Azure-datacenterplats som du använde när du skapade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e2842-121">Create a general-purpose V2 Storage account in your resource group, and the same Azure datacenter location that you used when creating the resource group.</span></span>

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name> --location <location> --sku Standard_RAGRS --encryption blob
    ```

1. <span data-ttu-id="e2842-122">För att komma åt den här lagringen behöver du en åtkomstnyckel för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="e2842-122">To access this storage, you'll need a storage account access key.</span></span> <span data-ttu-id="e2842-123">Visa åtkomstnycklarna som är associerade med lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="e2842-123">View the access keys that are associated with the storage account.</span></span>

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```

1. <span data-ttu-id="e2842-124">Spara värdet som är associerat med **key1**.</span><span class="sxs-lookup"><span data-stu-id="e2842-124">Save the value associated with **key1**.</span></span>

1. <span data-ttu-id="e2842-125">Du behöver även anslutningsinformationen för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="e2842-125">You will also need the connection details for the storage account.</span></span> <span data-ttu-id="e2842-126">Visa anslutningssträngen för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="e2842-126">View the connection string for the storage account.</span></span>

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```

1. <span data-ttu-id="e2842-127">Spara värdet som är associerat med **connectionString**.</span><span class="sxs-lookup"><span data-stu-id="e2842-127">Save the value associated with the **connectionString**.</span></span>

1. <span data-ttu-id="e2842-128">Meddelanden lagras i en container på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e2842-128">Messages will be stored in a container within your storage account.</span></span> <span data-ttu-id="e2842-129">Skapa en container på ditt lagringskonto med hjälp av `<connection string>` med anslutningssträngen från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="e2842-129">Create a container in your storage account, using `<connection string>` with the connection string from the previous step.</span></span>

    ```azurecli
    az storage container create -n <container> --connection-string "<connection string>"
    ```

## <a name="shell-command-for-cloning-an-application-github-repository"></a><span data-ttu-id="e2842-130">Shell-kommando för att klona GitHub-lagringsplatsen för en app</span><span class="sxs-lookup"><span data-stu-id="e2842-130">Shell command for cloning an application GitHub repository</span></span>

<span data-ttu-id="e2842-131">Git är ett samarbetsverktyg som använder en kontrollmodell för distribuerad version, och har utformats för samarbete i programvaru- och dokumentationsprojekt.</span><span class="sxs-lookup"><span data-stu-id="e2842-131">Git is a collaboration tool that uses a distributed version control model, and is designed for collaborative working on software and documentation projects.</span></span> <span data-ttu-id="e2842-132">Git-klienter finns för flera plattformar, bland annat Windows, och Git-kommandoraden ingår i Azure Bash Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="e2842-132">Git clients are available for multiple platforms, including Windows, and the Git command line is included in the Azure Bash cloud shell.</span></span> <span data-ttu-id="e2842-133">GitHub är en webbaserad värdtjänst för Git-lagringsplatser.</span><span class="sxs-lookup"><span data-stu-id="e2842-133">GitHub is a web-based hosting service for Git repositories.</span></span> 

<span data-ttu-id="e2842-134">Om du har en app som är värdbaserad som ett projekt i GitHub kan du skapa en lokal kopia av projektet genom att klona dess lagringsplats med kommandot **git clone**.</span><span class="sxs-lookup"><span data-stu-id="e2842-134">If you have an application that is hosted as a project in GitHub, you can make a local copy of the project, by cloning its repository using the **git clone** command.</span></span>

1. <span data-ttu-id="e2842-135">Klona ett datalager i hemkatalogen.</span><span class="sxs-lookup"><span data-stu-id="e2842-135">Clone a repository to your home directory.</span></span>

    ```azurecli
      git clone https://github.com/<project repository>
    ```

## <a name="shell-commands-for-preparing-an-application"></a><span data-ttu-id="e2842-136">Shell-kommandon för att förbereda en app</span><span class="sxs-lookup"><span data-stu-id="e2842-136">Shell commands for preparing an application</span></span>

1. <span data-ttu-id="e2842-137">Nu kan du använda ett redigeringsprogram, till exempel **nano**, till att redigera appen och lägga till händelsehubbens namnrymd, händelsehubbens namn, namnet på principen för delad åtkomst och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="e2842-137">You can now use an editor, such as **nano** to edit the application and add your event hub namespace, event hub name, shared access policy name, and primary key.</span></span> <span data-ttu-id="e2842-138">Nano är en enkelt textredigeringsprogram som är tillgängligt för Linux och andra Unix-lika operativsystem. Det är även tillgängligt i Azure Bash Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="e2842-138">Nano is a simple text editor available for Linux and other Unix-like operating systems; it is also available in the Azure Bash cloud shell.</span></span> <span data-ttu-id="e2842-139">Du kan till exempel använda kommandot till att öppna en Java-programfil i **nano**-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="e2842-139">For example, use this command to open a Java application file in the **nano** editor.</span></span>

    ```azurecli
    nano <application>.java
    ```

1. <span data-ttu-id="e2842-140">Beroende på hur dina appar har utvecklats kan du behöva kompilera eller skapa appen när du har lagt till händelsehubbens konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="e2842-140">Depending on how your applications are developed, you may need to compile or build the application after you've added the event hub configuration information.</span></span> <span data-ttu-id="e2842-141">Till exempel måste Java-apparna som används i nästa enhet skapas med ett verktyg som Apache **Maven**.</span><span class="sxs-lookup"><span data-stu-id="e2842-141">For example, the Java applications used in the next unit need to be built using a tool such as Apache **Maven**.</span></span> <span data-ttu-id="e2842-142">Maven kompilerar .java-filer i .class-filer eller paketerar dem i .jar-filer.</span><span class="sxs-lookup"><span data-stu-id="e2842-142">Maven compiles .java files into .class files or packages them into .jar files.</span></span> <span data-ttu-id="e2842-143">Maven är tillgängligt via Bash Cloud Shell. Om du vill skapa Java-appen använder du **mvn**-kommandon.</span><span class="sxs-lookup"><span data-stu-id="e2842-143">Maven is available from the Bash cloud shell; to build the Java application, you use **mvn** commands.</span></span> <span data-ttu-id="e2842-144">Använd det här mvn-kommandot till att skapa en Java-app.</span><span class="sxs-lookup"><span data-stu-id="e2842-144">Use this mvn command to build a Java  application.</span></span>

    ```azurecli
    mvn clean package -DskipTests
    ```

## <a name="summary"></a><span data-ttu-id="e2842-145">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e2842-145">Summary</span></span>

<span data-ttu-id="e2842-146">Avsändar- och mottagarappar måste konfigureras med specifik information om händelsehubbmiljön.</span><span class="sxs-lookup"><span data-stu-id="e2842-146">Sender and receiver applications must be configured with specific information about the event hub environment.</span></span> <span data-ttu-id="e2842-147">Du skapar ett lagringskonto om mottagarappen lagrar meddelanden i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="e2842-147">You create a storage account if your receiver application stores messages in Blob Storage.</span></span> <span data-ttu-id="e2842-148">Om appen finns på GitHub måste du klona den i din lokala katalog.</span><span class="sxs-lookup"><span data-stu-id="e2842-148">If your application is hosted on GitHub, you have to clone it to your local directory.</span></span> <span data-ttu-id="e2842-149">Textredigerare, som **nano**, används till att lägga till namnrymden i appen.</span><span class="sxs-lookup"><span data-stu-id="e2842-149">Text editors, such as **nano** is used to  add your namespace to the application.</span></span>