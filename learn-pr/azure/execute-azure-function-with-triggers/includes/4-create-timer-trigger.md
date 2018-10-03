<span data-ttu-id="79462-101">I den här övningen skapar vi en Azure-funktionsapp som anropas var 20:e sekund med hjälp av en timerutlösare.</span><span class="sxs-lookup"><span data-stu-id="79462-101">In this unit, we create an Azure function app that's invoked every 20 seconds using a timer trigger.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="79462-102">Skapa en Azure-funktionsapp</span><span class="sxs-lookup"><span data-stu-id="79462-102">Create an Azure function app</span></span>

<span data-ttu-id="79462-103">Vi börjar med att skapa en Azure-funktionsapp på portalen.</span><span class="sxs-lookup"><span data-stu-id="79462-103">Let’s start by creating an Azure Function app in the portal.</span></span>

1. <span data-ttu-id="79462-104">Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="79462-104">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="79462-105">Välj **Skapa en resurs** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="79462-105">In the left navigation, select **Create a resource**.</span></span>

1. <span data-ttu-id="79462-106">Välj **Beräkna**.</span><span class="sxs-lookup"><span data-stu-id="79462-106">Select **Compute**.</span></span>

1. <span data-ttu-id="79462-107">Leta upp och välj **Funktionsapp**.</span><span class="sxs-lookup"><span data-stu-id="79462-107">Locate and select **Function App**.</span></span> <span data-ttu-id="79462-108">Du kan även använda sökfältet för att hitta mallen.</span><span class="sxs-lookup"><span data-stu-id="79462-108">You can also optionally use the search bar to locate the template.</span></span>

    ![Skärmbild av Azure-portalen som visar bladet Skapa en resurs med Funktionsapp markerat.](../media/4-click-function-app.png)

1. <span data-ttu-id="79462-110">Ange ett globalt unikt **appnamn**.</span><span class="sxs-lookup"><span data-stu-id="79462-110">Enter a globally unique **App name**.</span></span>

1. <span data-ttu-id="79462-111">Välj en **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="79462-111">Select a **Subscription**.</span></span>

1. <span data-ttu-id="79462-112">Välj den befintliga **resursgruppen** <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>.</span><span class="sxs-lookup"><span data-stu-id="79462-112">Select the existing **Resource group** <rgn>[sandbox resource group name]</rgn>.</span></span>

1. <span data-ttu-id="79462-113">Välj **Windows** som **operativsystem**.</span><span class="sxs-lookup"><span data-stu-id="79462-113">Choose **Windows** as your **OS**.</span></span>

1. <span data-ttu-id="79462-114">Välj **Förbrukningsplan** som **värdplan**.</span><span class="sxs-lookup"><span data-stu-id="79462-114">Choose **Consumption Plan** for your **Hosting Plan**.</span></span> <span data-ttu-id="79462-115">Du debiteras för varje körning av funktionen.</span><span class="sxs-lookup"><span data-stu-id="79462-115">You're charged for each execution of your function.</span></span> <span data-ttu-id="79462-116">Resurser tilldelas automatiskt baserat på programmets arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="79462-116">Resources are automatically allocated based on your application workload.</span></span>

1. <span data-ttu-id="79462-117">Välj en **plats** i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="79462-117">Select a **Location** from the available list below.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="79462-118">För **Körningsstack** lämnar du som standard *.NET*, som är det språk som vi implementerar funktionsexemplen i den här övningen i.</span><span class="sxs-lookup"><span data-stu-id="79462-118">For **Runtime Stack**, leave as default *.NET*, which is the language in which we implement the function examples in this exercise.</span></span>

1. <span data-ttu-id="79462-119">Skapa ett nytt **lagringskonto**. Du kan ändra namnet om du vill – standardnamnet är en variant av appnamnet.</span><span class="sxs-lookup"><span data-stu-id="79462-119">Create a new **Storage** account, you can change the name if you like - it will default to a variation of the App name.</span></span>

1. <span data-ttu-id="79462-120">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="79462-120">Select **Create**.</span></span> <span data-ttu-id="79462-121">När funktionsappen har distribuerats går du till **Alla resurser** i portalen.</span><span class="sxs-lookup"><span data-stu-id="79462-121">Once the function app is deployed, go to **All resources** in the portal.</span></span> <span data-ttu-id="79462-122">Appen visas i listan med typen **Apptjänst** och har det namn du gav den.</span><span class="sxs-lookup"><span data-stu-id="79462-122">The function app will be listed with type **App Service** and has the name you gave it.</span></span>
 
<!-- Start temporary fix for issue #2498. -->
> [!IMPORTANT]
> <span data-ttu-id="79462-123">Övningarna i den här modulen fungerar för närvarande med Azure Functions V1.</span><span class="sxs-lookup"><span data-stu-id="79462-123">The exercises in this module currently work with Azure Functions V1.</span></span> <span data-ttu-id="79462-124">Följ dessa steg noggrant för att bekräfta att appen använder V1-körningsversionen.</span><span class="sxs-lookup"><span data-stu-id="79462-124">Please follow these steps carefully to make sure your function app uses the V1 runtime version.</span></span> 

1. <span data-ttu-id="79462-125">När funktionsappen har skapats väljer du **Alla resurser** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="79462-125">After the function app is created, select **All resources** from the left navigation.</span></span>

1. <span data-ttu-id="79462-126">Välj funktionsappen i listan **Funktionsappar**.</span><span class="sxs-lookup"><span data-stu-id="79462-126">Select your function app in the **Function Apps** list.</span></span>
1. <span data-ttu-id="79462-127">Välj **Plattformsfunktioner**.</span><span class="sxs-lookup"><span data-stu-id="79462-127">Select **Platform features**.</span></span>
1. <span data-ttu-id="79462-128">På skärmen **Plattformsfunktioner** väljer du **Funktionsappinställningar** under **Allmänna inställningar**.</span><span class="sxs-lookup"><span data-stu-id="79462-128">In the **Platform features** screen, select **Function app settings** under **General Settings**.</span></span>
1. <span data-ttu-id="79462-129">Välj *~1* i **Körningsversion** .</span><span class="sxs-lookup"><span data-stu-id="79462-129">Select *~1* in the **Runtime version** .</span></span>
1. <span data-ttu-id="79462-130">Stäng **Funktionsappinställningar**.</span><span class="sxs-lookup"><span data-stu-id="79462-130">Close **Function app settings**.</span></span>

<span data-ttu-id="79462-131">Vår funktionsapp har nu konfigurerats att använda Azure Functions V1-körningsversionen.</span><span class="sxs-lookup"><span data-stu-id="79462-131">Our function app is now configured to use the Azure Functions V1 runtime.</span></span> <span data-ttu-id="79462-132">Nu kan vi fortsätta att skapa vår första funktion.</span><span class="sxs-lookup"><span data-stu-id="79462-132">We can now continue to create our first function.</span></span>
<!-- End temporary fix for issue #2498. --> 

## <a name="create-a-timer-trigger"></a><span data-ttu-id="79462-133">Skapa en timerutlösare</span><span class="sxs-lookup"><span data-stu-id="79462-133">Create a timer trigger</span></span>

<span data-ttu-id="79462-134">Nu ska vi skapa en timerutlösare i vår funktion.</span><span class="sxs-lookup"><span data-stu-id="79462-134">Now we're going to create a timer trigger inside our function.</span></span>



1. <span data-ttu-id="79462-135">På det nya bladet pekar du på **Funktioner** och väljer plustecknet (+).</span><span class="sxs-lookup"><span data-stu-id="79462-135">On the new blade, point to **Functions** and select the plus (+) icon.</span></span>

    ![Skärmbild av Azure Portal som visar ett funktionsappblad med knappen Lägg till (+) för undermenyn Funktioner markerad.](../media/4-hover-function.png)

1. <span data-ttu-id="79462-137">Välj **Timer**.</span><span class="sxs-lookup"><span data-stu-id="79462-137">Select **Timer**.</span></span>

1. <span data-ttu-id="79462-138">Välj **Skapa den här funktionen**.</span><span class="sxs-lookup"><span data-stu-id="79462-138">Select **Create this function**.</span></span>

## <a name="configure-the-timer-trigger"></a><span data-ttu-id="79462-139">Konfigurera timerutlösaren</span><span class="sxs-lookup"><span data-stu-id="79462-139">Configure the timer trigger</span></span>

<span data-ttu-id="79462-140">Vi har en Azure-funktionsapp med logik som skriver ut ett meddelande till loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="79462-140">We have an Azure function app with logic to print a message to the log window.</span></span> <span data-ttu-id="79462-141">Vi ska ange timerns schema så att den körs var 20:e sekund.</span><span class="sxs-lookup"><span data-stu-id="79462-141">We're going to set the schedule of the timer to execute every 20 seconds.</span></span>

1. <span data-ttu-id="79462-142">Välj **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="79462-142">Select **Integrate**.</span></span>

1. <span data-ttu-id="79462-143">Ange följande värde i rutan **Schema**:</span><span class="sxs-lookup"><span data-stu-id="79462-143">Enter the following value into the **Schedule** box:</span></span>

    ```log
    */20 * * * * *
    ```

1. <span data-ttu-id="79462-144">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="79462-144">Select **Save**.</span></span>

## <a name="test-the-timer"></a><span data-ttu-id="79462-145">Testa timern</span><span class="sxs-lookup"><span data-stu-id="79462-145">Test the timer</span></span>

<span data-ttu-id="79462-146">Nu när vi har konfigurerat timern kommer den anropa funktionen med den intervall som vi definierade.</span><span class="sxs-lookup"><span data-stu-id="79462-146">Now that we've configured the timer, it will invoke the function on the interval we defined.</span></span>

1. <span data-ttu-id="79462-147">Välj **TimerTriggerCSharp1**.</span><span class="sxs-lookup"><span data-stu-id="79462-147">Select **TimerTriggerCSharp1**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="79462-148">**TimerTriggerCSharp1** är standardnamnet.</span><span class="sxs-lookup"><span data-stu-id="79462-148">**TimerTriggerCSharp1** is a default name.</span></span> <span data-ttu-id="79462-149">Det väljs automatiskt när du skapar utlösaren.</span><span class="sxs-lookup"><span data-stu-id="79462-149">It's automatically selected when you create the trigger.</span></span>

1. <span data-ttu-id="79462-150">Öppna **Loggpanelen** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="79462-150">Open the **Logs** panel at the bottom of the screen.</span></span>

1. <span data-ttu-id="79462-151">Lägg märke till att nya meddelanden tas emot var 20:e sekund i loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="79462-151">Observe new messages arrive every 20 seconds in the log window.</span></span>

1. <span data-ttu-id="79462-152">Om du vill stoppa funktionen från att köras, välj **Hantera** och ändra sedan **Funktionstillstånd** till *Inaktiverad*.</span><span class="sxs-lookup"><span data-stu-id="79462-152">To stop the function from running, select **Manage** and then switch **Function State** to *Disabled*.</span></span>
