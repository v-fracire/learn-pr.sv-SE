<span data-ttu-id="ca263-101">Nu är du redo att konfigurera dina utgivar- och konsumentappar för din händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="ca263-101">You're now ready to configure your publisher and consumer applications for your event hub.</span></span>

<span data-ttu-id="ca263-102">I den här övningen ska du konfigurera dessa appar för att skicka eller ta emot meddelanden via din händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="ca263-102">In this exercise, you'll configure these applications to send or receive messages through your event hub.</span></span> <span data-ttu-id="ca263-103">Dessa appar lagras på en GitHub-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="ca263-103">These applications are stored in a GitHub repository.</span></span>

<span data-ttu-id="ca263-104">Du konfigurerar två separata appar, en fungerar som avsändare (**SimpleSend**), den andra som mottagare (**EventProcessorSample**) av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ca263-104">You'll configure two separate applications; one acts as the message sender (**SimpleSend**), the other as the message receiver (**EventProcessorSample**).</span></span> <span data-ttu-id="ca263-105">Det här är Java-appar, vilket gör att du kan göra allt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ca263-105">These are Java applications, which enable you to do everything within the browser.</span></span> <span data-ttu-id="ca263-106">Men samma konfiguration krävs för alla plattformar, till exempel .NET.</span><span class="sxs-lookup"><span data-stu-id="ca263-106">However, the same configuration is needed for any platform, such as .NET.</span></span>

## <a name="create-a-general-purpose-standard-storage-account"></a><span data-ttu-id="ca263-107">Skapa ett allmänt standardlagringskonto</span><span class="sxs-lookup"><span data-stu-id="ca263-107">Create a general-purpose, standard storage account</span></span>

<span data-ttu-id="ca263-108">Java-mottagarappen, som du konfigurerar i den här enheten, lagrar meddelanden i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ca263-108">The Java receiver application, that you'll configure in this unit, stores messages in Azure Blob Storage.</span></span> <span data-ttu-id="ca263-109">Blob Storage kräver ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ca263-109">Blob Storage requires a storage account.</span></span>

1. <span data-ttu-id="ca263-110">Skapa ett lagringskonto (generell användning V2) i en resursgrupp med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca263-110">Create a storage account (general-purpose V2) in the resource group using the following command:</span></span>

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name>  --location <location> --sku Standard_RAGRS --encryption blob
    ```

    |<span data-ttu-id="ca263-111">Parameter</span><span class="sxs-lookup"><span data-stu-id="ca263-111">Parameter</span></span>      |<span data-ttu-id="ca263-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ca263-112">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="ca263-113">--name (krävs)</span><span class="sxs-lookup"><span data-stu-id="ca263-113">--name (required)</span></span>  |<span data-ttu-id="ca263-114">Ange ett namn för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="ca263-114">Enter a name for your storage account.</span></span>|
    |<span data-ttu-id="ca263-115">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="ca263-115">--resource-group (required)</span></span>  |<span data-ttu-id="ca263-116">Ange resursgruppen som du skapade i föregående enhet.</span><span class="sxs-lookup"><span data-stu-id="ca263-116">Enter the resource group you created in the previous unit.</span></span>|
    |<span data-ttu-id="ca263-117">--location (valfritt)</span><span class="sxs-lookup"><span data-stu-id="ca263-117">--location (optional)</span></span>    |<span data-ttu-id="ca263-118">Ange den plats som du använde för att skapa resursgruppen i den föregående enheten.</span><span class="sxs-lookup"><span data-stu-id="ca263-118">Enter the location you used to create your resource group in the previous unit.</span></span>|

1. <span data-ttu-id="ca263-119">Lista över alla åtkomstnycklar som är associerade med ditt lagringskonto med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca263-119">List all the access keys associated with your storage account using the following command:</span></span>

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```

    |<span data-ttu-id="ca263-120">Parameter</span><span class="sxs-lookup"><span data-stu-id="ca263-120">Parameter</span></span>      |<span data-ttu-id="ca263-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ca263-121">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="ca263-122">--account-name (krävs)</span><span class="sxs-lookup"><span data-stu-id="ca263-122">--account-name (required)</span></span>  |<span data-ttu-id="ca263-123">Ange namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="ca263-123">Enter the name for your storage account.</span></span>|
    |<span data-ttu-id="ca263-124">--resource-group (krävs)</span><span class="sxs-lookup"><span data-stu-id="ca263-124">--resource-group (required)</span></span>  |<span data-ttu-id="ca263-125">Ange resursgruppen som du skapade i föregående enhet.</span><span class="sxs-lookup"><span data-stu-id="ca263-125">Enter the resource group you created in the previous unit.</span></span>|

     <span data-ttu-id="ca263-126">Åtkomstnycklar som är associerade med lagringskontot listas.</span><span class="sxs-lookup"><span data-stu-id="ca263-126">Access keys associated with your storage account are listed.</span></span> <span data-ttu-id="ca263-127">Kopiera och spara värdet för **key** för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="ca263-127">Copy and save the value of **key** for future use.</span></span> <span data-ttu-id="ca263-128">Du behöver den här nyckeln för att få åtkomst till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ca263-128">You'll need this key to access your storage account.</span></span>

1. <span data-ttu-id="ca263-129">Visa anslutningssträngen för ditt lagringskonto med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca263-129">View the connections string for your storage account using the following command:</span></span>

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```

    |<span data-ttu-id="ca263-130">Parameter</span><span class="sxs-lookup"><span data-stu-id="ca263-130">Parameter</span></span>      |<span data-ttu-id="ca263-131">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ca263-131">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="ca263-132">-n (krävs)</span><span class="sxs-lookup"><span data-stu-id="ca263-132">-n (required)</span></span>  |<span data-ttu-id="ca263-133">Ange namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="ca263-133">Enter the name for your storage account.</span></span>|
    |<span data-ttu-id="ca263-134">-g (krävs)</span><span class="sxs-lookup"><span data-stu-id="ca263-134">-g (required)</span></span>  |<span data-ttu-id="ca263-135">Ange namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ca263-135">Enter the name of your resource group.</span></span>|

    <span data-ttu-id="ca263-136">Det här kommandot returnerar anslutningsinformation för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="ca263-136">This command returns the connection details for the storage account.</span></span> <span data-ttu-id="ca263-137">Kopiera och spara värdet för **connectionString**.</span><span class="sxs-lookup"><span data-stu-id="ca263-137">Copy and save the value of **connectionString**.</span></span>

1. <span data-ttu-id="ca263-138">Skapa en container som kallas **messages** på lagringskontot med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="ca263-138">Create a container called **messages** in your storage account using the following command.</span></span> <span data-ttu-id="ca263-139">Använd **connectionString** du kopierade i föregående steg:</span><span class="sxs-lookup"><span data-stu-id="ca263-139">Use the **connectionString** you copied in the previous step:</span></span>

    ```azurecli
    az storage container create -n messages --connection-string "<connection string>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a><span data-ttu-id="ca263-140">Klona Event Hubs GitHub-databasen</span><span class="sxs-lookup"><span data-stu-id="ca263-140">Clone the Event Hubs GitHub repository</span></span>

<span data-ttu-id="ca263-141">Använd följande steg för att klona Event Hubs GitHub-databasen.</span><span class="sxs-lookup"><span data-stu-id="ca263-141">Use the following steps to clone the Event Hubs GitHub repository.</span></span>

1. <span data-ttu-id="ca263-142">Logga in på Azure Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="ca263-142">Sign in to Azure Cloud Shell (Bash).</span></span>

1. <span data-ttu-id="ca263-143">Källfilerna för appen du bygger i den här övningen finns på en [GitHub-lagringsplats](https://github.com/Azure/azure-event-hubs).</span><span class="sxs-lookup"><span data-stu-id="ca263-143">The source files for the applications that you'll build in this exercise are located in a [GitHub repository](https://github.com/Azure/azure-event-hubs).</span></span> <span data-ttu-id="ca263-144">Använd följande kommandon för att se till att du är i din arbetskatalog i Cloud Shell, och klona sedan den här lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="ca263-144">Use the following commands to make sure that you are in your home directory in Cloud Shell, and then to clone this repository:</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    <span data-ttu-id="ca263-145">Lagringsplatsen klonas till `/home/<username>/azure-event-hubs`.</span><span class="sxs-lookup"><span data-stu-id="ca263-145">The repository is cloned to `/home/<username>/azure-event-hubs`.</span></span>

## <a name="use-nano-to-edit-simplesendjava"></a><span data-ttu-id="ca263-146">Använda nano till att redigera SimpleSend.java</span><span class="sxs-lookup"><span data-stu-id="ca263-146">Use nano to edit SimpleSend.java</span></span>

<span data-ttu-id="ca263-147">Använd **nano**-redigeraren till att redigera SimpleSend-appen och lägg till Event Hubs-namnrymden, händelsehubbens namn, namnet på principen för delad åtkomst och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="ca263-147">Use the **nano** editor to edit the SimpleSend application and add your Event Hubs namespace, event hub name, shared access policy name, and primary key.</span></span> <span data-ttu-id="ca263-148">Huvudkommandon visas längst ned i redigeringsfönstret. I den här enheten måste du skriva ut dina redigeringar med CTRL+O och sedan RETUR för att bekräfta utdatafilens namn, och avsluta redigeraren med CTRL+X.</span><span class="sxs-lookup"><span data-stu-id="ca263-148">The main commands are displayed at the bottom of the editor window; in this unit, you'll need to write out your edits using CTRL +O, and then ENTER to confirm the output file name, and exit the editor using CTRL +X.</span></span>

1. <span data-ttu-id="ca263-149">Ändra till mappen **SimpleSend** med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca263-149">Change to the **SimpleSend** folder using the following command:</span></span>

    ```azurecli
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. <span data-ttu-id="ca263-150">Öppna **SimpleSend.java**-filen i **nano**-redigeraren med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca263-150">Open the **SimpleSend.java** file in the **nano** editor using the following command:</span></span>

    ```azurecli
    nano SimpleSend.java
    ```

1. <span data-ttu-id="ca263-151">Leta upp och ersätt följande strängar i nano-redigeraren:</span><span class="sxs-lookup"><span data-stu-id="ca263-151">In the nano editor, locate and replace the following strings:</span></span>

    - <span data-ttu-id="ca263-152">`"Your Event Hubs namespace name"` med namnet på händelsehubbens namnrymd.</span><span class="sxs-lookup"><span data-stu-id="ca263-152">`"Your Event Hubs namespace name"` with the name of your event hub namespace.</span></span>
    - <span data-ttu-id="ca263-153">`"Your event hub"` med namnet på händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="ca263-153">`"Your event hub"` with the name of your event hub.</span></span>
    - <span data-ttu-id="ca263-154">`"Your primary SAS key"` med värdet för nyckeln **primaryKey** för händelsehubbens namnrymd som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ca263-154">`"Your primary SAS key"` with the value of the **primaryKey** key for your event hub namespace that you saved earlier.</span></span>
    - <span data-ttu-id="ca263-155">`"Your policy name"` med **Välj RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="ca263-155">`"Your policy name"` with **RootManageSharedAccessKey**.</span></span>
 
        <span data-ttu-id="ca263-156">När du skapar en Event Hubs-namnrymd skapas en 256-bitars SAS-nyckel med namnet **RootManageSharedAccessKey** som har ett associerat par primära och sekundära nycklar som tilldelar behörigheter för att skicka, lyssna och hantera till namnrymden.</span><span class="sxs-lookup"><span data-stu-id="ca263-156">When you create an Event Hubs namespace, a 256-bit SAS key called **RootManageSharedAccessKey** is created that has an associated pair of primary and secondary keys that grant send, listen, and manage rights to the namespace.</span></span> <span data-ttu-id="ca263-157">I den föregående enheten visade du nyckeln med ett Azure CLI-kommando, och du kan även hitta den här nyckeln genom att öppna sidan **Policyer för delad åtkomst** för din Event Hubs-namnrymd i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ca263-157">In the previous unit, you displayed the key using an Azure CLI command, and you can also find this key by opening the **Shared access policies** page for your Event Hubs namespace in the Azure portal.</span></span>

    ![Konfigurationsinformation för avsändarappen](../media-draft/5-sender-configure.png)

1. <span data-ttu-id="ca263-159">Spara **SimpleSend.java** med hjälp av följande kommando och avsluta nano:</span><span class="sxs-lookup"><span data-stu-id="ca263-159">Save **SimpleSend.java** using the following command, and exit nano:</span></span>

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## <a name="use-maven-to-build-simplesendjava"></a><span data-ttu-id="ca263-160">Använda Maven för att bygga SimpleSend.java</span><span class="sxs-lookup"><span data-stu-id="ca263-160">Use Maven to build SimpleSend.java</span></span>

<span data-ttu-id="ca263-161">Nu ska du bygga Java-appen med kommandot **mvn**.</span><span class="sxs-lookup"><span data-stu-id="ca263-161">You'll now build the Java application using **mvn** commands.</span></span>

1. <span data-ttu-id="ca263-162">Ändra till huvudmappen för **SimpleSend** med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca263-162">Change to the main **SimpleSend** folder using the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. <span data-ttu-id="ca263-163">Bygg Java SimpleSend-appen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="ca263-163">Build the Java SimpleSend application using the following command.</span></span> <span data-ttu-id="ca263-164">Det här ser till att appen använder anslutningsinformationen för händelsehubben:</span><span class="sxs-lookup"><span data-stu-id="ca263-164">This ensures that your application  uses the connection details for your event hub:</span></span>

    ```azurecli
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="ca263-165">Byggprocessen kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="ca263-165">The build process may take several minutes to complete.</span></span> <span data-ttu-id="ca263-166">Kontrollera att meddelandet **[INFO] BUILD SUCCESS** visas innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="ca263-166">Ensure that you see the **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![Byggresultat för avsändarappen](../media-draft/5-sender-build.png)

## <a name="use-nano-to-edit-eventprocessorsamplejava"></a><span data-ttu-id="ca263-168">Använda nano till att redigera EventProcessorSample.java</span><span class="sxs-lookup"><span data-stu-id="ca263-168">Use nano to edit EventProcessorSample.java</span></span>

<span data-ttu-id="ca263-169">Nu ska du konfigurera en app av typen **mottagare** (kallas även **prenumeranter** eller **konsumenter**) för att mata in data från händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="ca263-169">You'll now configure a **receiver** (also known as **subscribers** or **consumers**) application to ingest data from your event hub.</span></span>

<span data-ttu-id="ca263-170">För mottagarappen finns det två tillgängliga metoder, **EventHubReceiver** och **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="ca263-170">For the receiver application, two methods are available; **EventHubReceiver** and **EventProcessorHost**.</span></span> <span data-ttu-id="ca263-171">EventProcessorHost byggs ovanpå EventHubReceiver men har ett enklare programmatiskt gränssnitt än EventHubReceiver.</span><span class="sxs-lookup"><span data-stu-id="ca263-171">EventProcessorHost is built on top of EventHubReceiver, but provides simpler programmatic interface than EventHubReceiver.</span></span> <span data-ttu-id="ca263-172">EventProcessorHost kan distribuera meddelandepartitioner till flera instanser av EventProcessorHost med samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ca263-172">EventProcessorHost can automatically distribute message partitions across multiple instances of EventProcessorHost using the same storage account.</span></span>

<span data-ttu-id="ca263-173">I den här enheten använder du metoden EventProcessorHost.</span><span class="sxs-lookup"><span data-stu-id="ca263-173">In this unit, you’ll use the EventProcessorHost method.</span></span> <span data-ttu-id="ca263-174">Du använder återigen nano och redigerar appen EventProcessorSample för att lägga till Event Hubs-namnrymden, händelsehubbens namn, namn och primärnyckel för principen för delad åtkomst, lagringskontots namn, anslutningssträng och containernamn.</span><span class="sxs-lookup"><span data-stu-id="ca263-174">You'll again use nano, and edit the EventProcessorSample application to add your Event Hubs namespace, event hub name, shared access policy name and primary key, storage account name, connection string, and container name.</span></span>

1. <span data-ttu-id="ca263-175">Ändra mappen **EventProcessorSample** med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca263-175">Change to the **EventProcessorSample** folder using the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. <span data-ttu-id="ca263-176">Öppna filen **EventProcessorSample.java** i **nano**-redigeraren med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca263-176">Open the **EventProcessorSample.java** file in the **nano** editor using the following command:</span></span>

    ```azurecli
    nano EventProcessorSample.java
    ```
1. <span data-ttu-id="ca263-177">Leta upp och ersätt följande strängar i nano-redigeraren:</span><span class="sxs-lookup"><span data-stu-id="ca263-177">Locate and replace the following strings in the nano editor:</span></span>

    - <span data-ttu-id="ca263-178">`----ServiceBusNamespaceName----` med namnet på Event Hubs-namnrymden.</span><span class="sxs-lookup"><span data-stu-id="ca263-178">`----ServiceBusNamespaceName----` with the name of your Event Hubs namespace.</span></span>
    - <span data-ttu-id="ca263-179">`----EventHubName----` med namnet på händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="ca263-179">`----EventHubName----` with the name of your event hub.</span></span>
    - <span data-ttu-id="ca263-180">`----SharedAccessSignatureKeyName----` med **Välj RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="ca263-180">`----SharedAccessSignatureKeyName----` with **RootManageSharedAccessKey**.</span></span>
    - <span data-ttu-id="ca263-181">`----SharedAccessSignatureKey----` med värdet för nyckeln **primaryKey** för Event Hubs-namnrymden som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ca263-181">`----SharedAccessSignatureKey----` with the value of the **primaryKey** key for your Event Hubs namespace that you saved earlier.</span></span>
    - <span data-ttu-id="ca263-182">`----AzureStorageConnectionString----` med lagringskontots anslutningssträng som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ca263-182">`----AzureStorageConnectionString----` with your storage account connection string that you saved earlier.</span></span>
    - <span data-ttu-id="ca263-183">`----StorageContainerName----` med **messages**.</span><span class="sxs-lookup"><span data-stu-id="ca263-183">`----StorageContainerName----` with **messages**.</span></span>
    - <span data-ttu-id="ca263-184">`----HostNamePrefix----` med namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ca263-184">`----HostNamePrefix----` with the name of your storage account.</span></span>

    ![Konfigurationsinformation för mottagarapp](../media-draft/5-receiver-configure.png)

1. <span data-ttu-id="ca263-186">Spara **EventProcessorSample.java** med följande kommando och avsluta nano:</span><span class="sxs-lookup"><span data-stu-id="ca263-186">Save **EventProcessorSample.java** using the following command and exit nano:</span></span>

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## <a name="use-maven-to-build-eventprocessorsamplejava"></a><span data-ttu-id="ca263-187">Använda Maven för att bygga EventProcessorSample.java</span><span class="sxs-lookup"><span data-stu-id="ca263-187">Use Maven to build EventProcessorSample.java</span></span>

1. <span data-ttu-id="ca263-188">Ändra huvudmappen för **EventProcessorSample** med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca263-188">Change to the main **EventProcessorSample** folder using the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. <span data-ttu-id="ca263-189">Bygg Java SimpleSend-appen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="ca263-189">Build the Java SimpleSend application using the following command.</span></span> <span data-ttu-id="ca263-190">Det här ser till att appen använder anslutningsinformationen för händelsehubben:</span><span class="sxs-lookup"><span data-stu-id="ca263-190">This ensures that your application uses the connection details for your event hub:</span></span>

    ```azurecli
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="ca263-191">Byggprocessen kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="ca263-191">The build process may take several minutes to complete.</span></span> <span data-ttu-id="ca263-192">Kontrollera att meddelandet **[INFO] BUILD SUCCESS** visas innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="ca263-192">Ensure that you see a **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![Byggresultat för mottagarapp](../media-draft/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a><span data-ttu-id="ca263-194">Starta avsändar- och mottagarappen</span><span class="sxs-lookup"><span data-stu-id="ca263-194">Start the sender and receiver apps</span></span>

1. <span data-ttu-id="ca263-195">Kör Java-appen via kommandoraden genom att använda **java**-kommandot och ange ett .jar-paket.</span><span class="sxs-lookup"><span data-stu-id="ca263-195">Run Java application from the command line by using the **java** command, and specifying a .jar package.</span></span> <span data-ttu-id="ca263-196">Använd följande kommandon för att starta SimpleSend-appen:</span><span class="sxs-lookup"><span data-stu-id="ca263-196">Use the following commands to start the SimpleSend application:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. <span data-ttu-id="ca263-197">Tryck på RETUR när du ser **Överföringen är klar...**.</span><span class="sxs-lookup"><span data-stu-id="ca263-197">When you see **Send Complete...**, press ENTER.</span></span>

    ![Körningsresultat för avsändarappen](../media-draft/5-sender-run.png)

1. <span data-ttu-id="ca263-199">Starta EventProcessorSample-appen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="ca263-199">Start the EventProcessorSample application using the following command.</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. <span data-ttu-id="ca263-200">När meddelanden slutar visas i konsolen trycker du på RETUR.</span><span class="sxs-lookup"><span data-stu-id="ca263-200">When messages stop being displayed to the console, press ENTER.</span></span>

    ![Körningsresultat för mottagarapp](../media-draft/5-receiver-run.png)

## <a name="summary"></a><span data-ttu-id="ca263-202">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ca263-202">Summary</span></span>

<span data-ttu-id="ca263-203">Nu har du konfigurerat en avsändarapp redo att skicka meddelanden till händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="ca263-203">You've now configured a sender application ready to send messages to your event hub.</span></span> <span data-ttu-id="ca263-204">Du har även konfigurerat en mottagarapp redo att ta emot meddelanden från händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="ca263-204">You've also configured a receiver application ready to receive messages from your event hub.</span></span>
