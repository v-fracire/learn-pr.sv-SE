<span data-ttu-id="5a220-101">Nu är du redo att konfigurera dina utgivar- och konsumentappar för din händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="5a220-101">You're now ready to configure your publisher and consumer applications for your Event Hub.</span></span>

<span data-ttu-id="5a220-102">I den här enheten ska du konfigurera dessa appar för att skicka eller ta emot meddelanden via din händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="5a220-102">In this unit, you'll configure these applications to send or receive messages through your Event Hub.</span></span> <span data-ttu-id="5a220-103">Dessa appar lagras på en GitHub-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="5a220-103">These applications are stored in a GitHub repository.</span></span>

<span data-ttu-id="5a220-104">Du konfigurerar två separata appar, en fungerar som avsändare (**SimpleSend**), den andra som mottagare (**EventProcessorSample**) av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5a220-104">You'll configure two separate applications; one acts as the message sender (**SimpleSend**), the other as the message receiver (**EventProcessorSample**).</span></span> <span data-ttu-id="5a220-105">Det här är Java-appar, vilket gör att du kan göra allt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="5a220-105">These are Java applications, which enable you to do everything within the browser.</span></span> <span data-ttu-id="5a220-106">Men samma konfiguration krävs för alla plattformar, till exempel .NET.</span><span class="sxs-lookup"><span data-stu-id="5a220-106">However, the same configuration is needed for any platform, such as .NET.</span></span>

## <a name="create-a-general-purpose-standard-storage-account"></a><span data-ttu-id="5a220-107">Skapa ett allmänt standardlagringskonto</span><span class="sxs-lookup"><span data-stu-id="5a220-107">Create a general-purpose, standard storage account</span></span>

<span data-ttu-id="5a220-108">Java-mottagarappen, som du konfigurerar i den här enheten, lagrar meddelanden i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="5a220-108">The Java receiver application, that you'll configure in this unit, stores messages in Azure Blob Storage.</span></span> <span data-ttu-id="5a220-109">Blob Storage kräver ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5a220-109">Blob Storage requires a storage account.</span></span>

1. <span data-ttu-id="5a220-110">Skapa ett lagringskonto (generell användning V2) med kommandot `storage account create`.</span><span class="sxs-lookup"><span data-stu-id="5a220-110">Create a storage account (general-purpose V2) using the `storage account create` command.</span></span> <span data-ttu-id="5a220-111">Kom ihåg att vi ställer in en standardresursgrupp och -plats, så även om de här parametrarna normalt _krävs_, kan vi lämna dem.</span><span class="sxs-lookup"><span data-stu-id="5a220-111">Remember we set a default resource group and location, so even though those parameters are normally _required_, we can leave them off.</span></span>

    |<span data-ttu-id="5a220-112">Parameter</span><span class="sxs-lookup"><span data-stu-id="5a220-112">Parameter</span></span>      |<span data-ttu-id="5a220-113">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5a220-113">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="5a220-114">--name (krävs)</span><span class="sxs-lookup"><span data-stu-id="5a220-114">--name (required)</span></span>  | <span data-ttu-id="5a220-115">Ett namn på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="5a220-115">A name for your storage account.</span></span> |
    |<span data-ttu-id="5a220-116">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="5a220-116">--resource-group (required)</span></span>  |<span data-ttu-id="5a220-117">Resursgruppens ägare.</span><span class="sxs-lookup"><span data-stu-id="5a220-117">The resource group owner.</span></span> <span data-ttu-id="5a220-118">Vi använder den förinställda Sandbox-resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="5a220-118">We'll use the pre-created Sandbox resource group.</span></span>|
    |<span data-ttu-id="5a220-119">--location (valfritt)</span><span class="sxs-lookup"><span data-stu-id="5a220-119">--location (optional)</span></span>    |<span data-ttu-id="5a220-120">En valfri plats om du vill ha lagringskonto på en specifik plats jämfört med platsen för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="5a220-120">An optional location if you want the storage account in a specific place vs. the resource group location.</span></span>|

    <span data-ttu-id="5a220-121">Ange lagringskontonamn till en variabel.</span><span class="sxs-lookup"><span data-stu-id="5a220-121">Set the storage account name into a variable.</span></span> <span data-ttu-id="5a220-122">Det måste bestå av gemena bokstäver, siffror, med bindestreck tillåtna.</span><span class="sxs-lookup"><span data-stu-id="5a220-122">It must be composed of all lower-case letters, numbers, with hyphen separators allowed.</span></span> <span data-ttu-id="5a220-123">Det måste också vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="5a220-123">It also must be unique within Azure.</span></span>

    ```azurecli
    STORAGE_NAME=[name]
    ```

    <span data-ttu-id="5a220-124">Kör sedan detta kommando för att skapa lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="5a220-124">Then use this command to create the storage account.</span></span>

    ```azurecli
    az storage account create --name $STORAGE_NAME --sku Standard_RAGRS --encryption blob
    ```

    > [!TIP]
    > <span data-ttu-id="5a220-125">Om det inte går att skapa för lagringskonto, ändra miljövariabeln och försök igen.</span><span class="sxs-lookup"><span data-stu-id="5a220-125">If the storage account creation fails, change your environment variable and try again.</span></span>

1. <span data-ttu-id="5a220-126">Rada upp alla åtkomstnycklar som är associerade med ditt lagringskonto med kommandot `account keys list`.</span><span class="sxs-lookup"><span data-stu-id="5a220-126">List all the access keys associated with your storage account using the `account keys list` command.</span></span> <span data-ttu-id="5a220-127">Det tar ditt kontonamn och resursgruppen (som standard).</span><span class="sxs-lookup"><span data-stu-id="5a220-127">It takes your account name and the resource group (which is defaulted).</span></span>

    ```azurecli
    az storage account keys list --account-name $STORAGE_NAME
    ```

     <span data-ttu-id="5a220-128">Åtkomstnycklar som är associerade med lagringskontot listas.</span><span class="sxs-lookup"><span data-stu-id="5a220-128">Access keys associated with your storage account are listed.</span></span> <span data-ttu-id="5a220-129">Kopiera och spara värdet för **key** för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="5a220-129">Copy and save the value of **key** for future use.</span></span> <span data-ttu-id="5a220-130">Du behöver den här nyckeln för att få åtkomst till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5a220-130">You'll need this key to access your storage account.</span></span>

1. <span data-ttu-id="5a220-131">Visa anslutningssträngen för ditt lagringskonto med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5a220-131">View the connections string for your storage account using the following command:</span></span>

    ```azurecli
    az storage account show-connection-string -n $STORAGE_NAME
    ```

    <span data-ttu-id="5a220-132">Det här kommandot returnerar anslutningsinformation för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="5a220-132">This command returns the connection details for the storage account.</span></span> <span data-ttu-id="5a220-133">Kopiera och spara _värdet_ för **connectionString**.</span><span class="sxs-lookup"><span data-stu-id="5a220-133">Copy and save the _value_ of **connectionString**.</span></span> <span data-ttu-id="5a220-134">Det bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="5a220-134">It should look something like:</span></span>

    ```output
    "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=storage_account_name;AccountKey=VZjXuMeuDqjCkT60xX6L5fmtXixYuY2wiPmsrXwYHIhwo736kSAUAj08XBockRZh7CZwYxuYBPe31hi8XfHlWw=="
    ```

1. Skapa en container som kallas **messages** på lagringskontot med följande kommando. <span data-ttu-id="5a220-136">Använd **connectionString** du kopierade i föregående steg:</span><span class="sxs-lookup"><span data-stu-id="5a220-136">Use the **connectionString** you copied in the previous step:</span></span>

    ```azurecli
    az storage container create -n messages --connection-string "<connection string here>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a><span data-ttu-id="5a220-137">Klona Event Hubs GitHub-databasen</span><span class="sxs-lookup"><span data-stu-id="5a220-137">Clone the Event Hubs GitHub repository</span></span>

<span data-ttu-id="5a220-138">Använd följande steg för att klona Event Hubs GitHub-lagringsplatsen med `git`.</span><span class="sxs-lookup"><span data-stu-id="5a220-138">Use the following steps to clone the Event Hubs GitHub repository with `git`.</span></span> <span data-ttu-id="5a220-139">Du kan köra det här direkt i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="5a220-139">You can execute this right in the Cloud Shell.</span></span>

1. <span data-ttu-id="5a220-140">Källfilerna för de program som du bygger i den här enheten finns på en [GitHub-lagringsplats](https://github.com/Azure/azure-event-hubs).</span><span class="sxs-lookup"><span data-stu-id="5a220-140">The source files for the applications that you'll build in this unit are located in a [GitHub repository](https://github.com/Azure/azure-event-hubs).</span></span> <span data-ttu-id="5a220-141">Använd följande kommandon för att se till att du är i din arbetskatalog i Cloud Shell, och klona sedan den här lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="5a220-141">Use the following commands to make sure that you are in your home directory in Cloud Shell, and then to clone this repository:</span></span>

    ```bash
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    <span data-ttu-id="5a220-142">Databasen klonas till arbetsmappen.</span><span class="sxs-lookup"><span data-stu-id="5a220-142">The repository is cloned to your home folder.</span></span>

## <a name="edit-simplesendjava"></a><span data-ttu-id="5a220-143">Redigera SimpleSend.java</span><span class="sxs-lookup"><span data-stu-id="5a220-143">Edit SimpleSend.java</span></span>

<span data-ttu-id="5a220-144">Vi ska använda den inbyggda kodredigeraren i Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="5a220-144">We're going to use the built-in Cloud Shell Code editor.</span></span> <span data-ttu-id="5a220-145">Detta baseras på Monaco-redigeraren och liknar Visual Studio Code men är helt online.</span><span class="sxs-lookup"><span data-stu-id="5a220-145">This is based on the Monaco editor and is similar to Visual Studio Code, but completely online.</span></span>

<span data-ttu-id="5a220-146">Vi använder redigeraren till att ändra SimpleSend-appen och lägg till Event Hubs-namnrymden, händelsehubbens namn, namnet på principen för delad åtkomst och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="5a220-146">We'll use the editor to modify the SimpleSend application and add your Event Hubs namespace, Event Hub name, shared access policy name, and primary key.</span></span> <span data-ttu-id="5a220-147">De huvudsakliga kommandona visas längst ned i redigerarfönstret.</span><span class="sxs-lookup"><span data-stu-id="5a220-147">The main commands are displayed at the bottom of the editor window.</span></span> 

<span data-ttu-id="5a220-148">Du måste skriva ut dina ändringar med hjälp av <kbd>Ctrl + O</kbd>, och sedan <kbd>RETUR</kbd> för att bekräfta namnet på utdatafilen och avsluta redigeraren med <kbd>Ctrl + X</kbd>.</span><span class="sxs-lookup"><span data-stu-id="5a220-148">You'll need to write out your edits using <kbd>Ctrl+O</kbd>, and then <kbd>ENTER</kbd> to confirm the output file name, and exit the editor using <kbd>Ctrl+X</kbd>.</span></span> <span data-ttu-id="5a220-149">Redigeraren har också en ”...”-meny i det övre högra hörnet för alla redigeringskommandon.</span><span class="sxs-lookup"><span data-stu-id="5a220-149">Alternatively, the editor has a "..." menu in the top/right corner for all the editing commands.</span></span>

1. <span data-ttu-id="5a220-150">Ändra till mappen **SimpleSend**.</span><span class="sxs-lookup"><span data-stu-id="5a220-150">Change to the **SimpleSend** folder.</span></span>

    ```bash
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. <span data-ttu-id="5a220-151">Öppna kodredigeraren i den aktuella mappen.</span><span class="sxs-lookup"><span data-stu-id="5a220-151">Open the code editor in the current folder.</span></span> <span data-ttu-id="5a220-152">Detta visar en lista över filer till vänster och ett redigeringsutrymme till höger.</span><span class="sxs-lookup"><span data-stu-id="5a220-152">This will show a list of files on the left and an editor space on the right.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="5a220-153">Öppna filen **SimpleSend.java** genom att välja den från listan.</span><span class="sxs-lookup"><span data-stu-id="5a220-153">Open the **SimpleSend.java** file by selecting it from the file list.</span></span>

1. <span data-ttu-id="5a220-154">Leta upp och ersätt följande strängar i redigeraren:</span><span class="sxs-lookup"><span data-stu-id="5a220-154">In the editor, locate and replace the following strings:</span></span>

    - <span data-ttu-id="5a220-155">`"Your Event Hubs namespace name"` med namnet på händelsehubbens namnrymd.</span><span class="sxs-lookup"><span data-stu-id="5a220-155">`"Your Event Hubs namespace name"` with the name of your Event Hub namespace.</span></span>
    - <span data-ttu-id="5a220-156">`"Your Event Hub"` med namnet på händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="5a220-156">`"Your Event Hub"` with the name of your Event Hub.</span></span>
    - <span data-ttu-id="5a220-157">`"Your policy name"` med **Välj RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="5a220-157">`"Your policy name"` with **RootManageSharedAccessKey**.</span></span>
    - <span data-ttu-id="5a220-158">`"Your primary SAS key"` med värdet för nyckeln **primaryKey** för händelsehubbens namnrymd som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="5a220-158">`"Your primary SAS key"` with the value of the **primaryKey** key for your Event Hub namespace that you saved earlier.</span></span>
 
    > [!TIP]
    > <span data-ttu-id="5a220-159">Till skillnad från terminalfönstret kan redigeraren använda vanliga kortkommandon för att kopiera och klistra in för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="5a220-159">Unlike the terminal window, the editor can use typical copy/paste keyboard accelerator keys for your OS.</span></span>

    <span data-ttu-id="5a220-160">Om du har glömt vissa av dem kan du växla till terminalfönstret under redigeraren och använda kommandot `echo` för att lista ut miljövariablerna.</span><span class="sxs-lookup"><span data-stu-id="5a220-160">If you've forgotten some of them, you can switch down to the terminal window below the editor and use the `echo` command to list out one of the environment variables.</span></span> <span data-ttu-id="5a220-161">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5a220-161">For example:</span></span>

    ```bash
    echo $NS_NAME
    ```
    <span data-ttu-id="5a220-162">När du skapar en Event Hubs-namnrymd skapas en 256-bitars SAS-nyckel med namnet **RootManageSharedAccessKey** som har ett associerat par primära och sekundära nycklar som tilldelar behörigheter för att skicka, lyssna och hantera till namnrymden.</span><span class="sxs-lookup"><span data-stu-id="5a220-162">When you create an Event Hubs namespace, a 256-bit SAS key called **RootManageSharedAccessKey** is created that has an associated pair of primary and secondary keys that grant send, listen, and manage rights to the namespace.</span></span> <span data-ttu-id="5a220-163">I den föregående enheten visade du nyckeln med ett Azure CLI-kommando, och du kan även hitta den här nyckeln genom att öppna sidan **Policyer för delad åtkomst** för din Event Hubs-namnrymd i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5a220-163">In the previous unit, you displayed the key using an Azure CLI command, and you can also find this key by opening the **Shared access policies** page for your Event Hubs namespace in the Azure portal.</span></span>

1. <span data-ttu-id="5a220-164">Spara **SimpleSend.java** antingen via menyn ”...” eller snabbtangenten (<kbd>Ctrl + S</kbd> i Windows och Linux, <kbd>Cmd + S</kbd> på macOS).</span><span class="sxs-lookup"><span data-stu-id="5a220-164">Save **SimpleSend.java** either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="5a220-165">Stäng redigeraren via menyn ”...” eller kortkommandot <kbd>CTRL + Q</kbd>.</span><span class="sxs-lookup"><span data-stu-id="5a220-165">Close the editor through the "..." menu, or the accelerator key <kbd>CTRL+Q</kbd>.</span></span>

## <a name="use-maven-to-build-simplesendjava"></a><span data-ttu-id="5a220-166">Använda Maven för att bygga SimpleSend.java</span><span class="sxs-lookup"><span data-stu-id="5a220-166">Use Maven to build SimpleSend.java</span></span>

<span data-ttu-id="5a220-167">Nu ska du bygga Java-appen med kommandot **mvn**.</span><span class="sxs-lookup"><span data-stu-id="5a220-167">You'll now build the Java application using **mvn** commands.</span></span>

1. <span data-ttu-id="5a220-168">Ändra tillbaka till huvudmappen **SimpleSend**.</span><span class="sxs-lookup"><span data-stu-id="5a220-168">Change back to the main **SimpleSend** folder.</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. <span data-ttu-id="5a220-169">Skapa SimpleSend för Java-program.</span><span class="sxs-lookup"><span data-stu-id="5a220-169">Build the Java SimpleSend application.</span></span> <span data-ttu-id="5a220-170">Det här ser till att appen använder anslutningsinformationen för händelsehubben:</span><span class="sxs-lookup"><span data-stu-id="5a220-170">This ensures that your application  uses the connection details for your Event Hub:</span></span>

    ```bash
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="5a220-171">Byggprocessen kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="5a220-171">The build process may take several minutes to complete.</span></span> <span data-ttu-id="5a220-172">Kontrollera att meddelandet **[INFO] BUILD SUCCESS** visas innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="5a220-172">Ensure that you see the **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![Byggresultat för avsändarappen](../media/5-sender-build.png)

## <a name="edit-eventprocessorsamplejava"></a><span data-ttu-id="5a220-174">Redigera EventProcessorSample.java</span><span class="sxs-lookup"><span data-stu-id="5a220-174">Edit EventProcessorSample.java</span></span>

<span data-ttu-id="5a220-175">Nu ska du konfigurera en app av typen **mottagare** (kallas även **prenumeranter** eller **konsumenter**) för att mata in data från händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="5a220-175">You'll now configure a **receiver** (also known as **subscribers** or **consumers**) application to ingest data from your Event Hub.</span></span>

<span data-ttu-id="5a220-176">För mottagarappen finns det två tillgängliga metoder, **EventHubReceiver** och **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="5a220-176">For the receiver application, two methods are available; **EventHubReceiver** and **EventProcessorHost**.</span></span> <span data-ttu-id="5a220-177">EventProcessorHost byggs ovanpå EventHubReceiver men har ett enklare programmatiskt gränssnitt än EventHubReceiver.</span><span class="sxs-lookup"><span data-stu-id="5a220-177">EventProcessorHost is built on top of EventHubReceiver, but provides simpler programmatic interface than EventHubReceiver.</span></span> <span data-ttu-id="5a220-178">EventProcessorHost kan distribuera meddelandepartitioner till flera instanser av EventProcessorHost med samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5a220-178">EventProcessorHost can automatically distribute message partitions across multiple instances of EventProcessorHost using the same storage account.</span></span>

<span data-ttu-id="5a220-179">I den här enheten använder du metoden EventProcessorHost.</span><span class="sxs-lookup"><span data-stu-id="5a220-179">In this unit, you’ll use the EventProcessorHost method.</span></span> <span data-ttu-id="5a220-180">Du redigerar appen EventProcessorSample för att lägga till Event Hubs-namnrymden, händelsehubbens namn, namn och primärnyckel för principen för delad åtkomst, lagringskontots namn, anslutningssträng och containernamn.</span><span class="sxs-lookup"><span data-stu-id="5a220-180">You'll edit the EventProcessorSample application to add your Event Hubs namespace, Event Hub name, shared access policy name and primary key, storage account name, connection string, and container name.</span></span>

1. <span data-ttu-id="5a220-181">Ändra mappen **EventProcessorSample** med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5a220-181">Change to the **EventProcessorSample** folder using the following command:</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. <span data-ttu-id="5a220-182">Öppna kodredigeraren.</span><span class="sxs-lookup"><span data-stu-id="5a220-182">Open the code editor.</span></span>

    ```bash
    code .
    ```
    
1. <span data-ttu-id="5a220-183">Välj filen **EventProcessorSample.java**.</span><span class="sxs-lookup"><span data-stu-id="5a220-183">Select the **EventProcessorSample.java** file.</span></span>

1. <span data-ttu-id="5a220-184">Leta upp och ersätt följande strängar i redigeraren:</span><span class="sxs-lookup"><span data-stu-id="5a220-184">Locate and replace the following strings in the editor:</span></span>

    - <span data-ttu-id="5a220-185">`----ServiceBusNamespaceName----` med namnet på Event Hubs-namnrymden.</span><span class="sxs-lookup"><span data-stu-id="5a220-185">`----ServiceBusNamespaceName----` with the name of your Event Hubs namespace.</span></span>
    - <span data-ttu-id="5a220-186">`----EventHubName----` med namnet på händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="5a220-186">`----EventHubName----` with the name of your Event Hub.</span></span>
    - <span data-ttu-id="5a220-187">`----SharedAccessSignatureKeyName----` med **Välj RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="5a220-187">`----SharedAccessSignatureKeyName----` with **RootManageSharedAccessKey**.</span></span>
    - <span data-ttu-id="5a220-188">`----SharedAccessSignatureKey----` med värdet för nyckeln **primaryKey** för Event Hubs-namnrymden som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="5a220-188">`----SharedAccessSignatureKey----` with the value of the **primaryKey** key for your Event Hubs namespace that you saved earlier.</span></span>
    - <span data-ttu-id="5a220-189">`----AzureStorageConnectionString----` med lagringskontots anslutningssträng som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="5a220-189">`----AzureStorageConnectionString----` with your storage account connection string that you saved earlier.</span></span>
    - <span data-ttu-id="5a220-190">`----StorageContainerName----` med **messages**.</span><span class="sxs-lookup"><span data-stu-id="5a220-190">`----StorageContainerName----` with **messages**.</span></span>
    - <span data-ttu-id="5a220-191">`----HostNamePrefix----` med namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5a220-191">`----HostNamePrefix----` with the name of your storage account.</span></span>

1. <span data-ttu-id="5a220-192">Spara **EventProcessorSample.java** antingen via menyn ”...” eller snabbtangenten (<kbd>Ctrl + S</kbd> i Windows och Linux, <kbd>Cmd + S</kbd> på macOS).</span><span class="sxs-lookup"><span data-stu-id="5a220-192">Save **EventProcessorSample.java** either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="5a220-193">Stäng redigeraren.</span><span class="sxs-lookup"><span data-stu-id="5a220-193">Close the editor.</span></span>

## <a name="use-maven-to-build-eventprocessorsamplejava"></a><span data-ttu-id="5a220-194">Använda Maven för att bygga EventProcessorSample.java</span><span class="sxs-lookup"><span data-stu-id="5a220-194">Use Maven to build EventProcessorSample.java</span></span>

1. <span data-ttu-id="5a220-195">Ändra huvudmappen för **EventProcessorSample** med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5a220-195">Change to the main **EventProcessorSample** folder using the following command:</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. <span data-ttu-id="5a220-196">Bygg Java SimpleSend-appen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="5a220-196">Build the Java SimpleSend application using the following command.</span></span> <span data-ttu-id="5a220-197">Det här ser till att appen använder anslutningsinformationen för händelsehubben:</span><span class="sxs-lookup"><span data-stu-id="5a220-197">This ensures that your application uses the connection details for your Event Hub:</span></span>

    ```bash
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="5a220-198">Byggprocessen kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="5a220-198">The build process may take several minutes to complete.</span></span> <span data-ttu-id="5a220-199">Kontrollera att meddelandet **[INFO] BUILD SUCCESS** visas innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="5a220-199">Ensure that you see a **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![Byggresultat för mottagarapp](../media/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a><span data-ttu-id="5a220-201">Starta avsändar- och mottagarappen</span><span class="sxs-lookup"><span data-stu-id="5a220-201">Start the sender and receiver apps</span></span>

1. <span data-ttu-id="5a220-202">Kör Java-appen via kommandoraden genom att använda **java**-kommandot och ange ett .jar-paket.</span><span class="sxs-lookup"><span data-stu-id="5a220-202">Run Java application from the command line by using the **java** command, and specifying a .jar package.</span></span> <span data-ttu-id="5a220-203">Använd följande kommandon för att starta SimpleSend-appen:</span><span class="sxs-lookup"><span data-stu-id="5a220-203">Use the following commands to start the SimpleSend application:</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="5a220-204">Tryck på <kbd>RETUR</kbd> när du ser **Överföringen är klar...**.</span><span class="sxs-lookup"><span data-stu-id="5a220-204">When you see **Send Complete...**, press <kbd>ENTER</kbd>.</span></span>

    ```output
    jar-with-dependencies.jar
    SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
    SLF4J: Defaulting to no-operation (NOP) logger implementation
    SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
    2018-09-18T19:42:15.146Z: Send Complete...
    ```

1. <span data-ttu-id="5a220-205">Starta EventProcessorSample-appen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="5a220-205">Start the EventProcessorSample application using the following command.</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="5a220-206">När meddelanden inte visas längre i konsolen, trycker du på <kbd>RETUR</kbd> eller <kbd>CTRL + C</kbd> för att avsluta programmet.</span><span class="sxs-lookup"><span data-stu-id="5a220-206">When messages stop being displayed to the console, press <kbd>ENTER</kbd> or <kbd>CTRL+C</kbd> to end the program.</span></span>

    ```output
    ...
    SAMPLE: Partition 0 checkpointing at 1064,19
    SAMPLE (3,1120,20): "Message 80"
    SAMPLE (3,1176,21): "Message 84"
    SAMPLE (3,1232,22): "Message 88"
    SAMPLE (3,1288,23): "Message 92"
    SAMPLE (3,1344,24): "Message 96"
    SAMPLE: Partition 3 checkpointing at 1344,24
    SAMPLE (2,1120,20): "Message 83"
    SAMPLE (2,1176,21): "Message 87"
    SAMPLE (2,1232,22): "Message 91"
    SAMPLE (2,1288,23): "Message 95"
    SAMPLE (2,1344,24): "Message 99"
    SAMPLE: Partition 2 checkpointing at 1344,24
    SAMPLE: Partition 1 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE (0,1120,20): "Message 81"
    SAMPLE (0,1176,21): "Message 85"
    SAMPLE: Partition 0 batch size was 10 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 got event batch
    SAMPLE (0,1232,22): "Message 89"
    SAMPLE (0,1288,23): "Message 93"
    SAMPLE (0,1344,24): "Message 97"
    SAMPLE: Partition 0 checkpointing at 1344,24
    SAMPLE: Partition 3 batch size was 8 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 2 batch size was 9 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    ```

## <a name="summary"></a><span data-ttu-id="5a220-207">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5a220-207">Summary</span></span>

<span data-ttu-id="5a220-208">Nu har du konfigurerat en avsändarapp redo att skicka meddelanden till händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="5a220-208">You've now configured a sender application ready to send messages to your Event Hub.</span></span> <span data-ttu-id="5a220-209">Du har även konfigurerat en mottagarapp redo att ta emot meddelanden från händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="5a220-209">You've also configured a receiver application ready to receive messages from your Event Hub.</span></span>