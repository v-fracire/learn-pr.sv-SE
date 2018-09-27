<span data-ttu-id="5716e-101">I den här övningen skapar vi en Azure-funktionsapp som anropas var 20:e sekund med hjälp av en timerutlösare.</span><span class="sxs-lookup"><span data-stu-id="5716e-101">In this unit, we create an Azure function app that's invoked every 20 seconds using a timer trigger.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="5716e-102">Skapa en Azure-funktionsapp</span><span class="sxs-lookup"><span data-stu-id="5716e-102">Create an Azure function app</span></span>

<span data-ttu-id="5716e-103">Vi börjar med att skapa en Azure-funktionsapp på portalen.</span><span class="sxs-lookup"><span data-stu-id="5716e-103">Let’s start by creating an Azure Function app in the portal.</span></span>

1. <span data-ttu-id="5716e-104">Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="5716e-104">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="5716e-105">Välj **Skapa en resurs** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="5716e-105">In the left navigation, select **Create a resource**.</span></span>

1. <span data-ttu-id="5716e-106">Välj **Beräkna**.</span><span class="sxs-lookup"><span data-stu-id="5716e-106">Select **Compute**.</span></span>

1. <span data-ttu-id="5716e-107">Leta upp och välj **Funktionsapp**.</span><span class="sxs-lookup"><span data-stu-id="5716e-107">Locate and select **Function App**.</span></span> <span data-ttu-id="5716e-108">Du kan även använda sökfältet för att hitta mallen.</span><span class="sxs-lookup"><span data-stu-id="5716e-108">You can also optionally use the search bar to locate the template.</span></span>

    ![Skärmbild av Azure-portalen som visar bladet Skapa en resurs med Funktionsapp markerat.](../media/4-click-function-app.png)

1. <span data-ttu-id="5716e-110">Ange ett globalt unikt **appnamn**.</span><span class="sxs-lookup"><span data-stu-id="5716e-110">Enter a globally unique **App name**.</span></span>

1. <span data-ttu-id="5716e-111">Välj en **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="5716e-111">Select a **Subscription**.</span></span>

1. <span data-ttu-id="5716e-112">Välj den befintliga **resursgruppen** <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>.</span><span class="sxs-lookup"><span data-stu-id="5716e-112">Select the existing **Resource group** <rgn>[sandbox resource group name]</rgn>.</span></span>

1. <span data-ttu-id="5716e-113">Välj **Windows** som **operativsystem**.</span><span class="sxs-lookup"><span data-stu-id="5716e-113">Choose **Windows** as your **OS**.</span></span>

1. <span data-ttu-id="5716e-114">Välj **Förbrukningsplan** som **värdplan**.</span><span class="sxs-lookup"><span data-stu-id="5716e-114">Choose **Consumption Plan** for your **Hosting Plan**.</span></span> <span data-ttu-id="5716e-115">Du debiteras för varje körning av funktionen.</span><span class="sxs-lookup"><span data-stu-id="5716e-115">You're charged for each execution of your function.</span></span> <span data-ttu-id="5716e-116">Resurser tilldelas automatiskt baserat på programmets arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="5716e-116">Resources are automatically allocated based on your application workload.</span></span>

1. <span data-ttu-id="5716e-117">Välj en **Plats** i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="5716e-117">Select a **Location** from the available list below.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="5716e-118">Skapa ett nytt **lagringskonto**. Du kan ändra namnet om du vill – standardnamnet är en variant av appnamnet.</span><span class="sxs-lookup"><span data-stu-id="5716e-118">Create a new **Storage** account, you can change the name if you like - it will default to a variation of the App name.</span></span>

1. <span data-ttu-id="5716e-119">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5716e-119">Select **Create**.</span></span> <span data-ttu-id="5716e-120">När funktionsappen har distribuerats går du till **Alla resurser** i portalen.</span><span class="sxs-lookup"><span data-stu-id="5716e-120">Once the function app is deployed, go to **All resources** in the portal.</span></span> <span data-ttu-id="5716e-121">Appen visas i listan med typen **Apptjänst** och har det namn du gav den.</span><span class="sxs-lookup"><span data-stu-id="5716e-121">The function app will be listed with type **App Service** and has the name you gave it.</span></span>

## <a name="create-a-timer-trigger"></a><span data-ttu-id="5716e-122">Skapa en timerutlösare</span><span class="sxs-lookup"><span data-stu-id="5716e-122">Create a timer trigger</span></span>

<span data-ttu-id="5716e-123">Nu ska vi skapa en timerutlösare i vår funktion.</span><span class="sxs-lookup"><span data-stu-id="5716e-123">Now we're going to create a timer trigger inside our function.</span></span>

1. <span data-ttu-id="5716e-124">När funktionen har skapats väljer du **Alla resurser** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="5716e-124">After the function is created, select **All resources** from the left navigation.</span></span>

1. <span data-ttu-id="5716e-125">Hitta din funktionsapp i listan och välj den.</span><span class="sxs-lookup"><span data-stu-id="5716e-125">Find your function app in the list and select it.</span></span>

1. <span data-ttu-id="5716e-126">På det nya bladet pekar du på **Funktioner** och väljer plustecknet (+).</span><span class="sxs-lookup"><span data-stu-id="5716e-126">On the new blade, point to **Functions** and select the plus (+) icon.</span></span>

    ![Skärmbild av Azure Portal som visar ett funktionsappblad med knappen Lägg till (+) för undermenyn Funktioner markerad.](../media/4-hover-function.png)

1. <span data-ttu-id="5716e-128">Välj **Timer**.</span><span class="sxs-lookup"><span data-stu-id="5716e-128">Select **Timer**.</span></span>

1. <span data-ttu-id="5716e-129">Välj **CSharp** som språk.</span><span class="sxs-lookup"><span data-stu-id="5716e-129">Select **CSharp** as the language.</span></span>

1. <span data-ttu-id="5716e-130">Välj **Skapa den här funktionen**.</span><span class="sxs-lookup"><span data-stu-id="5716e-130">Select **Create this function**.</span></span>

## <a name="configure-the-timer-trigger"></a><span data-ttu-id="5716e-131">Konfigurera timerutlösaren</span><span class="sxs-lookup"><span data-stu-id="5716e-131">Configure the timer trigger</span></span>

<span data-ttu-id="5716e-132">Vi har en Azure-funktionsapp med logik som skriver ut ett meddelande till loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="5716e-132">We have an Azure function app with logic to print a message to the log window.</span></span> <span data-ttu-id="5716e-133">Vi ska ange timerns schema så att den körs var 20:e sekund.</span><span class="sxs-lookup"><span data-stu-id="5716e-133">We're going to set the schedule of the timer to execute every 20 seconds.</span></span>

1. <span data-ttu-id="5716e-134">Välj **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="5716e-134">Select **Integrate**.</span></span>

1. <span data-ttu-id="5716e-135">Ange följande värde i rutan **Schema**:</span><span class="sxs-lookup"><span data-stu-id="5716e-135">Enter the following value into the **Schedule** box:</span></span>

    ```log
    */20 * * * * *
    ```

1. <span data-ttu-id="5716e-136">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5716e-136">Select **Save**.</span></span>

## <a name="test-the-timer"></a><span data-ttu-id="5716e-137">Testa timern</span><span class="sxs-lookup"><span data-stu-id="5716e-137">Test the timer</span></span>

<span data-ttu-id="5716e-138">Nu när vi har konfigurerat timern kommer den anropa funktionen med den intervall som vi definierade.</span><span class="sxs-lookup"><span data-stu-id="5716e-138">Now that we've configured the timer, it will invoke the function on the interval we defined.</span></span>

1. <span data-ttu-id="5716e-139">Välj **TimerTriggerCSharp1**.</span><span class="sxs-lookup"><span data-stu-id="5716e-139">Select **TimerTriggerCSharp1**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5716e-140">**TimerTriggerCSharp1** är standardnamnet.</span><span class="sxs-lookup"><span data-stu-id="5716e-140">**TimerTriggerCSharp1** is a default name.</span></span> <span data-ttu-id="5716e-141">Det väljs automatiskt när du skapar utlösaren.</span><span class="sxs-lookup"><span data-stu-id="5716e-141">It's automatically selected when you create the trigger.</span></span>

1. <span data-ttu-id="5716e-142">Öppna **Loggpanelen** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="5716e-142">Open the **Logs** panel at the bottom of the screen.</span></span>

1. <span data-ttu-id="5716e-143">Lägg märke till att nya meddelanden tas emot var 20:e sekund i loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="5716e-143">Observe new messages arrive every 20 seconds in the log window.</span></span>

1. <span data-ttu-id="5716e-144">Om du vill stoppa funktionen från att köras, välj **Hantera** och ändra sedan **Funktionstillstånd** till *Inaktiverad*.</span><span class="sxs-lookup"><span data-stu-id="5716e-144">To stop the function from running, select **Manage** and then switch **Function State** to *Disabled*.</span></span>
