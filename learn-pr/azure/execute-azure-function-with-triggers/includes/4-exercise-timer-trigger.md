<span data-ttu-id="4c95a-101">I den här övningen ska vi skapa en Azure-funktion som anropas var 20:e sekund med hjälp av en timerutlösare.</span><span class="sxs-lookup"><span data-stu-id="4c95a-101">In this exercise, we create an Azure function that's invoked every 20 seconds using a timer trigger.</span></span>

> [!NOTE] 
> <span data-ttu-id="4c95a-102">För att kunna slutföra den här övningen måste du vara inloggad på [Azure-portalen](https://portal.azure.com/) med ett giltigt konto.</span><span class="sxs-lookup"><span data-stu-id="4c95a-102">To complete this exercise, make sure you're logged into the [Azure portal](https://portal.azure.com/) with a valid account.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="4c95a-103">Skapa en Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="4c95a-103">Create an Azure function</span></span>

<span data-ttu-id="4c95a-104">Vi ska börja med att skapa en Azure-funktion i portalen.</span><span class="sxs-lookup"><span data-stu-id="4c95a-104">Let’s start by creating an Azure function in the portal.</span></span>

1. <span data-ttu-id="4c95a-105">Välj **Skapa en resurs** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="4c95a-105">In the left navigation, select **Create a resource**.</span></span>

1. <span data-ttu-id="4c95a-106">Välj **Beräkna**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-106">Select **Compute**.</span></span>

1. <span data-ttu-id="4c95a-107">Leta upp och välj **Funktionsapp**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-107">Locate and select **Function App**.</span></span> <span data-ttu-id="4c95a-108">Du kan även använda sökfältet för att hitta mallen.</span><span class="sxs-lookup"><span data-stu-id="4c95a-108">You can also optionally use the search bar to locate the template.</span></span>

    ![Välj Funktionsapp](../media-drafts/4-click-function-app.png)

1. <span data-ttu-id="4c95a-110">Ange ett unikt **appnamn**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-110">Enter a unique **App name**.</span></span>

1. <span data-ttu-id="4c95a-111">Välj en **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-111">Select a **Subscription**.</span></span>

1. <span data-ttu-id="4c95a-112">Skapa en ny **resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-112">Create a new **Resource Group**.</span></span>

1. <span data-ttu-id="4c95a-113">Välj **Windows** som **operativsystem**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-113">Choose **Windows** as your **OS**.</span></span>

1. <span data-ttu-id="4c95a-114">Välj **Förbrukningsplan** som **värdplan**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-114">Choose **Consumption Plan** for your **Hosting Plan**.</span></span> <span data-ttu-id="4c95a-115">Du debiteras för varje körning av funktionen.</span><span class="sxs-lookup"><span data-stu-id="4c95a-115">You're charged for each execution of your function.</span></span> <span data-ttu-id="4c95a-116">Resurser tilldelas automatiskt baserat på programmets arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="4c95a-116">Resources are automatically allocated based on your application workload.</span></span>

1. <span data-ttu-id="4c95a-117">Välj en **plats**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-117">Select a **Location**.</span></span>

1. <span data-ttu-id="4c95a-118">Skapa ett nytt **lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-118">Create a new **Storage** account.</span></span> <span data-ttu-id="4c95a-119">(Det här värdet krävs, men vi kommer inte att använda det.)</span><span class="sxs-lookup"><span data-stu-id="4c95a-119">(This value is required, but we're not going to use it.)</span></span>

1. <span data-ttu-id="4c95a-120">Stäng av **Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-120">Turn off **Application Insights**.</span></span>

1. <span data-ttu-id="4c95a-121">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-121">Select **Create**.</span></span>

## <a name="create-a-timer-trigger"></a><span data-ttu-id="4c95a-122">Skapa en timerutlösare</span><span class="sxs-lookup"><span data-stu-id="4c95a-122">Create a timer trigger</span></span>

<span data-ttu-id="4c95a-123">Nu ska vi skapa en timerutlösare i vår Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="4c95a-123">Now we're going to create a timer trigger inside our Azure function.</span></span>

1. <span data-ttu-id="4c95a-124">När Azure-funktionen har skapats väljer du **Alla resurser** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="4c95a-124">After the Azure function is created, select **All resources** from the left navigation.</span></span>

1. <span data-ttu-id="4c95a-125">Leta upp och välj din Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="4c95a-125">Locate and select your Azure function.</span></span>

1. <span data-ttu-id="4c95a-126">Peka på **Funktioner** och välj plustecknet (+) på det nya bladet.</span><span class="sxs-lookup"><span data-stu-id="4c95a-126">On the new blade, point to **Functions** and select the plus (+) icon.</span></span>

    ![Peka på Funktioner och välj plustecknet](../media-drafts/4-hover-function.png)

1. <span data-ttu-id="4c95a-128">Välj **Timer**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-128">Select **Timer**.</span></span>

1. <span data-ttu-id="4c95a-129">Välj **CSharp** som språk.</span><span class="sxs-lookup"><span data-stu-id="4c95a-129">Select **CSharp** as the language.</span></span>

1. <span data-ttu-id="4c95a-130">Välj **Skapa den här funktionen**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-130">Select **Create this function**.</span></span>

## <a name="configure-the-timer-trigger"></a><span data-ttu-id="4c95a-131">Konfigurera timerutlösaren</span><span class="sxs-lookup"><span data-stu-id="4c95a-131">Configure the timer trigger</span></span>

<span data-ttu-id="4c95a-132">Vi har en Azure-funktion med logik som skriver ut ett meddelande till loggfönstret.</span><span class="sxs-lookup"><span data-stu-id="4c95a-132">We have an Azure function with logic to print a message to the log window.</span></span> <span data-ttu-id="4c95a-133">Vi ska ange timerns schema så att den körs var 20:e sekund.</span><span class="sxs-lookup"><span data-stu-id="4c95a-133">We're going to set the schedule of the timer to execute every 20 seconds.</span></span>

1. <span data-ttu-id="4c95a-134">Välj **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-134">Select **Integrate**.</span></span>

1. <span data-ttu-id="4c95a-135">Ange följande värde i rutan **Schema**:</span><span class="sxs-lookup"><span data-stu-id="4c95a-135">Enter the following value into the **Schedule** box:</span></span>

    ```
    */20 * * * * *
    ```

1. <span data-ttu-id="4c95a-136">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-136">Select **Save**.</span></span>

## <a name="start-the-timer"></a><span data-ttu-id="4c95a-137">Starta timern</span><span class="sxs-lookup"><span data-stu-id="4c95a-137">Start the timer</span></span>

<span data-ttu-id="4c95a-138">Nu när vi har konfigurerat timern är vi redo att starta den.</span><span class="sxs-lookup"><span data-stu-id="4c95a-138">Now that we've configured the timer, we're ready to start it.</span></span>

1. <span data-ttu-id="4c95a-139">Välj **TimerTriggerCSharp1**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-139">Select **TimerTriggerCSharp1**.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="4c95a-140">**TimerTriggerCSharp1** är standardnamnet.</span><span class="sxs-lookup"><span data-stu-id="4c95a-140">**TimerTriggerCSharp1** is a default name.</span></span> <span data-ttu-id="4c95a-141">Det väljs automatiskt när du skapar utlösaren.</span><span class="sxs-lookup"><span data-stu-id="4c95a-141">It's automatically selected when you create the trigger.</span></span>

1. <span data-ttu-id="4c95a-142">Välj **Kör**.</span><span class="sxs-lookup"><span data-stu-id="4c95a-142">Select **Run**.</span></span> 

<span data-ttu-id="4c95a-143">Du bör nu se ett meddelande i loggfönstret var 20:e sekund.</span><span class="sxs-lookup"><span data-stu-id="4c95a-143">At this point, you should see a message every 20 seconds in the log window.</span></span>

## <a name="clean-up"></a><span data-ttu-id="4c95a-144">Rensa</span><span class="sxs-lookup"><span data-stu-id="4c95a-144">Clean up</span></span>

<span data-ttu-id="4c95a-145">För att undvika att debiteras för den här funktionen väljer du **Pausa** ovanför loggfönstret för att stoppa timern.</span><span class="sxs-lookup"><span data-stu-id="4c95a-145">To ensure that you aren't charged for this function, above the log window, select **Pause** to stop the timer.</span></span>

![Pausa](../media-drafts/4-pause-timer.png)


