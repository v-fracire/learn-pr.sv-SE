<span data-ttu-id="e8546-101">Nu har du arbetat med Azure IoT Central-programmet och anslutit kaffebryggaren till Azure IoT Central.</span><span class="sxs-lookup"><span data-stu-id="e8546-101">You’ve now worked with the Azure IoT Central application and connected the coffee machine to Azure IoT Central.</span></span> <span data-ttu-id="e8546-102">Du är på väg att börja övervaka och hantera din fjärranslutna kaffebryggare.</span><span class="sxs-lookup"><span data-stu-id="e8546-102">You are well on your way to begin to monitor and manage your remote coffee machine.</span></span> <span data-ttu-id="e8546-103">I den här verifierar du din konfiguration och anslutning genom att använda mallen Ansluten kaffebryggare som du definierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e8546-103">In this unit, you take a moment to validate your setup and connection by using the Connected Coffee Maker template that you defined earlier.</span></span> <span data-ttu-id="e8546-104">Du uppdaterar den optimala temperaturen i Inställningar, kör kommandon för att kontrollera tillståndet för datorn och visar din anslutna kaffebryggare i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e8546-104">You update the optimal temperature in settings, run commands to check for the state of your machine, and view your connected coffee machine in the dashboard.</span></span> 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a><span data-ttu-id="e8546-105">Uppdatera inställningarna för att synkronisera programmet med kaffebryggaren</span><span class="sxs-lookup"><span data-stu-id="e8546-105">Update settings to sync your application with the coffee machine</span></span>

<span data-ttu-id="e8546-106">På sidan **Inställningar** skickar du konfigurationsdata till kaffebryggaren från ditt program.</span><span class="sxs-lookup"><span data-stu-id="e8546-106">On the **Settings** page, you send configuration data to the coffee machine from your application.</span></span> 

<span data-ttu-id="e8546-107">I det här scenariot ändrar du den optimala temperaturen och väljer **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="e8546-107">In this scenario, change the optimal temperature and choose **Update**.</span></span> <span data-ttu-id="e8546-108">När inställningen ändras markeras inställningen som väntande i användargränssnittet tills kaffebryggaren bekräftar att den har besvarat den ändrade inställningen.</span><span class="sxs-lookup"><span data-stu-id="e8546-108">When the setting is changed, the setting is marked as pending in the UI until the coffee machine acknowledges that it has responded to the setting change.</span></span> 

> [!NOTE]
> <span data-ttu-id="e8546-109">Lyckade uppdateringar i inställningen indikerar dataflöde och verifierar anslutningen.</span><span class="sxs-lookup"><span data-stu-id="e8546-109">Successful updates in the setting indicate data flow and validate your  connection.</span></span> <span data-ttu-id="e8546-110">Telemetrimätningarna telemetri besvarar uppdateringen av optimal temperatur.</span><span class="sxs-lookup"><span data-stu-id="e8546-110">The telemetry measurements will respond to the update in optimal temperature.</span></span> <span data-ttu-id="e8546-111">Du kan se ändringen på sidan **Mått**.</span><span class="sxs-lookup"><span data-stu-id="e8546-111">You can observe the change on the **Measurements** page.</span></span> 

## <a name="run-commands-on-the-coffee-machine"></a><span data-ttu-id="e8546-112">Kör kommandon på kaffebryggaren</span><span class="sxs-lookup"><span data-stu-id="e8546-112">Run commands on the coffee machine</span></span> 
<span data-ttu-id="e8546-113">Navigera till sidan **Kommandon** för följande övning.</span><span class="sxs-lookup"><span data-stu-id="e8546-113">Navigate to the **Commands** page for the following exercise.</span></span> <span data-ttu-id="e8546-114">För att verifiera kommandokonfigurationen kör du kommandon via en fjärranslutning på kaffebryggaren från IoT Central.</span><span class="sxs-lookup"><span data-stu-id="e8546-114">To validate the commands setup, you remotely run commands on the coffee machine from IoT Central.</span></span> <span data-ttu-id="e8546-115">Om kommandona lyckas skickas bekräftelsemeddelanden från kaffebryggaren.</span><span class="sxs-lookup"><span data-stu-id="e8546-115">If the commands are successful, confirmation messages are sent from the coffee machine.</span></span>

1. <span data-ttu-id="e8546-116">Börja brygga via fjärranslutning genom att välja **Kör**.</span><span class="sxs-lookup"><span data-stu-id="e8546-116">Start brewing remotely by choosing **Run**.</span></span> 
    
    <span data-ttu-id="e8546-117">Kaffebryggaren startar om följande tre villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="e8546-117">The coffee machine will start if these three conditions are satisfied:</span></span>
    - <span data-ttu-id="e8546-118">En kopp har identifierats</span><span class="sxs-lookup"><span data-stu-id="e8546-118">Cup detected</span></span>
    - <span data-ttu-id="e8546-119">Bryggaren är inte i underhållsläge</span><span class="sxs-lookup"><span data-stu-id="e8546-119">Not in maintenance</span></span>
    - <span data-ttu-id="e8546-120">Någon bryggning pågår inte redan</span><span class="sxs-lookup"><span data-stu-id="e8546-120">Not brewing already</span></span>  

    > [!NOTE]
    > <span data-ttu-id="e8546-121">När du har startat bryggningen ändras bryggarens status till gult, vilket indikeras under **Mått** > **Status**.</span><span class="sxs-lookup"><span data-stu-id="e8546-121">When you've successfully started brewing, the state of the machine changes to yellow as indicated in **Measurements** > **State**.</span></span> 
    
    <span data-ttu-id="e8546-122">Sök efter bekräftelsemeddelanden i den simulerade kaffebryggarens konsollogg för node.js.</span><span class="sxs-lookup"><span data-stu-id="e8546-122">Look for confirmation messages in the console log on the node.js simulated coffee machine.</span></span> 

    ![Köra kommandon](../media/4-commands-brewing.png)

1. <span data-ttu-id="e8546-124">Ställ in underhållsläge genom att välja **Kör**.</span><span class="sxs-lookup"><span data-stu-id="e8546-124">Set maintenance mode by choosing **Run**.</span></span> <span data-ttu-id="e8546-125">Kaffebryggaren sätts i underhållsläge om den *inte* redan är i underhåll.</span><span class="sxs-lookup"><span data-stu-id="e8546-125">The coffee machine will set to maintenance if it's *not* already in maintenance.</span></span>
    
    <span data-ttu-id="e8546-126">Sök efter bekräftelsemeddelanden i kaffebryggarens konsollogg.</span><span class="sxs-lookup"><span data-stu-id="e8546-126">Look for confirmation messages in the console log on the coffee machine.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="e8546-127">Liksom i verkligheten när en tekniker tar en maskin offline för att utföra nödvändiga reparationer innan han sätter den online igen, så fortsätter kaffebryggaren att vara i underhållsläge tills du startar om klientkoden.</span><span class="sxs-lookup"><span data-stu-id="e8546-127">As in real life, when the technician takes the machine offline to perform necessary repairs before switching it back online, the coffee machine continues to stay in the maintenance mode until you reboot the client code.</span></span>

    ![Kör kommandon](../media/4-commands-maintenance.png)

> [!IMPORTANT]
> <span data-ttu-id="e8546-129">Det rekommenderas att du kör Node.js-program i högst ca 60 minuter för att förhindra att programmet skickar dig oönskade meddelanden/e-postmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="e8546-129">It's recommended that you run the Node.js application no more than 60 minutes or so to prevent the application from sending you unwanted notifications/emails.</span></span> <span data-ttu-id="e8546-130">Genom att stoppa programmet när du inte arbetar med modulen förhindrar du även att den dagliga meddelandekvoten tar slut.</span><span class="sxs-lookup"><span data-stu-id="e8546-130">Stopping the application when you're not working on the module also prevents you from exhausting the daily message quota.</span></span>

## <a name="view-the-coffee-machine-in-the-dashboard"></a><span data-ttu-id="e8546-131">Visa kaffebryggaren på instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="e8546-131">View the coffee machine in the dashboard</span></span>

1. <span data-ttu-id="e8546-132">Gå till fliken **Instrumentpanel**.</span><span class="sxs-lookup"><span data-stu-id="e8546-132">Navigate to the **Dashboard** tab.</span></span>

1. <span data-ttu-id="e8546-133">Välj **Redigera mall** för att redigera instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e8546-133">Select **Edit Template** to edit the dashboard.</span></span>

1. <span data-ttu-id="e8546-134">Välj **Linjediagram** på sidomenyn.</span><span class="sxs-lookup"><span data-stu-id="e8546-134">Choose **Line Chart** from the side menu.</span></span>

1. <span data-ttu-id="e8546-135">Under **Konfigurera diagram** anger du `Telemetry` i fältet **Rubrik**.</span><span class="sxs-lookup"><span data-stu-id="e8546-135">In **Configure Chart**  enter `Telemetry` in the **Title** field.</span></span> <span data-ttu-id="e8546-136">Vi ska visa telemetridata med det här diagrammet.</span><span class="sxs-lookup"><span data-stu-id="e8546-136">We'll view our telemetry data with this chart.</span></span> 

1. <span data-ttu-id="e8546-137">Välj **Senaste 30 minuterna** för **Tidsintervall**.</span><span class="sxs-lookup"><span data-stu-id="e8546-137">Select **Past 30 minutes** for **Time Range**.</span></span> 

1. <span data-ttu-id="e8546-138">I området **Mått** väljer du synlighetsikonen till höger om varje mått för att göra måttet synligt i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="e8546-138">In the **Measures** area, select the visibility icon to the right of each measurement to make that measurement visible for our chart.</span></span> 

1. <span data-ttu-id="e8546-139">Välj **Spara** för att spara konfigurationen och visa linjediagrammet.</span><span class="sxs-lookup"><span data-stu-id="e8546-139">Select **Save** to save our configuration and display the line chart.</span></span> 

    ![Visa instrumentpanelen](../media/4-dashboard-a.png)

1. <span data-ttu-id="e8546-141">Välj **Inställningar och egenskaper** i den vänstra menyn för att öppna panelen **Configure Device Details** (Konfigurera enhetsinformation).</span><span class="sxs-lookup"><span data-stu-id="e8546-141">Choose **Settings and Properties** in the left-hand menu to open the **Configure Device Details** panel.</span></span> 

1. <span data-ttu-id="e8546-142">Ange `Device properties` i fältet **Rubrik**.</span><span class="sxs-lookup"><span data-stu-id="e8546-142">Enter `Device properties` in the **Title** filed.</span></span>

1. <span data-ttu-id="e8546-143">I **Lägg till/ta bort** väljer du **Coffee Makers Max Temperature** (Kaffebryggarens högsta temperatur), **Coffee Makers Min Temperature** (Kaffebryggarens lägsta temperatur), **Device Warranty Expired** (Enhetsgaranti utgången) och sedan **OK** för att slutföra valen.</span><span class="sxs-lookup"><span data-stu-id="e8546-143">In **Add/Remove**, choose **Coffee Makers Max Temperature**, **Coffee Makers Min Temperature**, **Device Warranty Expired** and then select **OK** to complete the selection.</span></span>

1. <span data-ttu-id="e8546-144">Välj **Spara** för att skapa en ny panel för *Enhetsegenskaper* på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e8546-144">Select **Save** to create a new *Device properties* panel in our dashboard.</span></span> 

1. <span data-ttu-id="e8546-145">Välj **Inställningar och egenskaper** igen och ange `Optimal Temperature` som rubrik.</span><span class="sxs-lookup"><span data-stu-id="e8546-145">Choose **Settings and Properties** again,  and enter `Optimal Temperature` as the title.</span></span> <span data-ttu-id="e8546-146">I **Lägg till/ta bort** väljer du **Optimal temperatur**.</span><span class="sxs-lookup"><span data-stu-id="e8546-146">In **Add/Remove**, choose **Optimal  Temperature**.</span></span>

1. <span data-ttu-id="e8546-147">Välj **Spara** för att spara arbetet och visa ett nytt fönster på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e8546-147">Select **Save** to save your work and display a new pane in our dashboard.</span></span> 

1. <span data-ttu-id="e8546-148">Välj **Klar** för att avsluta läget och visa den nya instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e8546-148">Select **Done** to exit edit mode and display the new dashboard.</span></span> 