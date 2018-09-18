<span data-ttu-id="7913f-101">Härnäst ska vi titta på hur du skapar och ändrar instrumentpaneler med hjälp av Azure Portal och genom att redigera den underliggande JSON-filen direkt.</span><span class="sxs-lookup"><span data-stu-id="7913f-101">Next, let's look at how to create and modify dashboards using the Azure Portal, and by editing the underlying JSON file directly.</span></span>

## <a name="what-is-a-dashboard"></a><span data-ttu-id="7913f-102">Vad är en instrumentpanel?</span><span class="sxs-lookup"><span data-stu-id="7913f-102">What is a dashboard?</span></span>

<span data-ttu-id="7913f-103">En _instrumentpanel_ består av en anpassningsbar panelsamling i användargränssnittet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7913f-103">A _dashboard_ is a customizable collection of UI tiles displayed in the Azure portal.</span></span> <span data-ttu-id="7913f-104">Du kan lägga till, ta bort och placera panelerna för att skapa den vy du vill ha och sedan spara vyn som en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="7913f-104">You add, remove, and position tiles to create the exact view you want, and then save that view as a dashboard.</span></span> <span data-ttu-id="7913f-105">Det går att ha flera instrumentpaneler och du kan växla mellan dem efter behov.</span><span class="sxs-lookup"><span data-stu-id="7913f-105">Multiple dashboards are supported, and you can switch between them as needed.</span></span> <span data-ttu-id="7913f-106">Du kan även dela instrumentpaneler med andra teammedlemmar.</span><span class="sxs-lookup"><span data-stu-id="7913f-106">You can even share your dashboards with other team members.</span></span>

<span data-ttu-id="7913f-107">Med hjälp av instrumentpaneler får du stor flexibilitet när du hanterar Azure.</span><span class="sxs-lookup"><span data-stu-id="7913f-107">Dashboards give you considerable flexibility regarding how you manage Azure.</span></span> <span data-ttu-id="7913f-108">Du kan till exempel skapa instrumentpaneler för specifika roller i organisationen och sedan använda rollbaserad åtkomstkontroll (RBAC) för att kontrollera vem som kan komma åt instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7913f-108">For example, you can create dashboards for specific roles within the organization, and then use role-based access control (RBAC) to control who can access that dashboard.</span></span> <span data-ttu-id="7913f-109">Din databasadministratör kan således ha en instrumentpanel som innehåller vyer för SQL-databastjänsten, medan Azure Active Directory-administratören har vyer för användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7913f-109">Hence, your database administrator would have a dashboard that contains views of the SQL database service, whereas your Azure Active Directory administrator would have views of the users and groups within Azure AD.</span></span> <span data-ttu-id="7913f-110">Du kan även anpassa portalen mellan dina produktions- och utvecklingsmiljöer från portalen – skapa en specifik instrumentpanel för varje miljö som du hanterar.</span><span class="sxs-lookup"><span data-stu-id="7913f-110">You can even customize the portal between your production and development environments within the portal - creating a specific dashboard for each environment you are managing.</span></span>

<span data-ttu-id="7913f-111">Instrumentpaneler lagras som JSON-filer (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="7913f-111">Dashboards are stored as JavaScript Object Notation (JSON) files.</span></span> <span data-ttu-id="7913f-112">Det innebär att de kan laddas upp och laddas ned till andra datorer eller delas med andra i Azure-katalogen.</span><span class="sxs-lookup"><span data-stu-id="7913f-112">This means they can be uploaded and downloaded to other computers, or shared with members of the Azure directory.</span></span> <span data-ttu-id="7913f-113">Azure lagrar instrumentpaneler i resursgrupper, precis som virtuella datorer eller lagringskonton som du kan hantera i portalen.</span><span class="sxs-lookup"><span data-stu-id="7913f-113">Azure stores dashboards within resource groups, just like virtual machines or storage accounts that you can manage within the portal.</span></span>

> [!TIP]
> <span data-ttu-id="7913f-114">Eftersom instrumentpaneler är JSON-filer kan du även [anpassa dem programmatiskt](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically), vilket gör dem till detaljerade administrativa verktyg.</span><span class="sxs-lookup"><span data-stu-id="7913f-114">Because dashboards are JSON files, you can also [customize them programmatically](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically), making them compelling administrative tools.</span></span> <span data-ttu-id="7913f-115">Dessutom kan vissa typer av paneler vara frågebaserade så att de uppdateras automatiskt när källdata ändras.</span><span class="sxs-lookup"><span data-stu-id="7913f-115">Also, some tile types can be query-based, so they update automatically when the source data changes.</span></span>

## <a name="explore-the-default-dashboard"></a><span data-ttu-id="7913f-116">Titta närmare på standardinstrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="7913f-116">Explore the default dashboard</span></span>

<span data-ttu-id="7913f-117">Standardinstrumentpanelen heter ”Instrumentpanel”.</span><span class="sxs-lookup"><span data-stu-id="7913f-117">The default dashboard is named "Dashboard".</span></span> <span data-ttu-id="7913f-118">När du loggar in på portalen visas den här instrumentpanelen och dess fem webbdelar.</span><span class="sxs-lookup"><span data-stu-id="7913f-118">When you log into the portal, you are presented with this dashboard containing five web parts.</span></span>

![Standardwebbdelar](../media-draft/8-dashboard-default-webparts.png)

<span data-ttu-id="7913f-120">Standardwebbdelarna är</span><span class="sxs-lookup"><span data-stu-id="7913f-120">These default web parts are</span></span>

1. <span data-ttu-id="7913f-121">Alla resurser</span><span class="sxs-lookup"><span data-stu-id="7913f-121">All resources</span></span>

1. <span data-ttu-id="7913f-122">Komma igång med Azure</span><span class="sxs-lookup"><span data-stu-id="7913f-122">Azure Getting Started</span></span>

1. <span data-ttu-id="7913f-123">Snabbstarter och självstudier</span><span class="sxs-lookup"><span data-stu-id="7913f-123">Quickstarts + tutorials</span></span>

1. <span data-ttu-id="7913f-124">Marketplace</span><span class="sxs-lookup"><span data-stu-id="7913f-124">Marketplace</span></span>

1. <span data-ttu-id="7913f-125">Service Health</span><span class="sxs-lookup"><span data-stu-id="7913f-125">Service Health</span></span>

## <a name="creating-and-managing-dashboards"></a><span data-ttu-id="7913f-126">Skapa och hantera instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="7913f-126">Creating and managing dashboards</span></span>

<span data-ttu-id="7913f-127">Överst på instrumentpanelen finns det kontroller för att skapa, ladda upp, ladda ned, redigera och dela en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="7913f-127">Along the top of the dashboard are the controls that enable you to create, upload, download, edit, and share a dashboard.</span></span> <span data-ttu-id="7913f-128">Du kan även visa en instrumentpanel i fullskärmsläge samt klona eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="7913f-128">You can also switch a dashboard to full screen, clone it, or delete it.</span></span>

![Anpassa instrumentpanelskontroller](../media-draft/8-customise-dashboard-controls.png)

- [<span data-ttu-id="7913f-130">Välj instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-130">Select dashboard</span></span>](#select-dashboard)
- [<span data-ttu-id="7913f-131">Skapa en ny instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-131">Create a new dashboard</span></span>](#create-new)
- [<span data-ttu-id="7913f-132">Ladda upp och ladda ned</span><span class="sxs-lookup"><span data-stu-id="7913f-132">Upload and Download</span></span>](#upload-download)
- [<span data-ttu-id="7913f-133">Redigera</span><span class="sxs-lookup"><span data-stu-id="7913f-133">Edit</span></span>](#edit-dashboard)
- [<span data-ttu-id="7913f-134">Dela</span><span class="sxs-lookup"><span data-stu-id="7913f-134">Share</span></span>](#share-dashboard)
- [<span data-ttu-id="7913f-135">Helskärm</span><span class="sxs-lookup"><span data-stu-id="7913f-135">Full screen</span></span>](#full-screen)
- [<span data-ttu-id="7913f-136">Klona</span><span class="sxs-lookup"><span data-stu-id="7913f-136">Clone</span></span>](#clone-dashboard)
- [<span data-ttu-id="7913f-137">Ta bort</span><span class="sxs-lookup"><span data-stu-id="7913f-137">Delete</span></span>](#delete-dashboard)

<a name="select-dashboard"></a>

## <a name="select-dashboard"></a><span data-ttu-id="7913f-138">Välja instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-138">Select dashboard</span></span>

<span data-ttu-id="7913f-139">Till vänster finns den nedrullningsbara kontrollen **Välj instrumentpanel**.</span><span class="sxs-lookup"><span data-stu-id="7913f-139">To the far left of the toolbar is the **Select Dashboard** drop-down control.</span></span> <span data-ttu-id="7913f-140">Klicka på den här kontrollen om du vill välja bland instrumentpaneler som du redan har definierat för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="7913f-140">Clicking this control enables you to select from dashboards that you have already defined for your account.</span></span> <span data-ttu-id="7913f-141">Den här kontrollen gör det enkelt för dig att definiera flera instrumentpaneler för olika syften, och sedan växla mellan dem beroende på vad du behöver göra.</span><span class="sxs-lookup"><span data-stu-id="7913f-141">This control makes it simple for you to define multiple dashboards for different purposes and then switch from one to another and back again, depending on what you are trying to do at the time.</span></span>

<span data-ttu-id="7913f-142">Observera att alla instrumentpaneler som du skapar först är privata. Det vill säga att bara du kan se dem.</span><span class="sxs-lookup"><span data-stu-id="7913f-142">Note that any dashboards that you create will initially be private; that is, only you can see them.</span></span> <span data-ttu-id="7913f-143">Du måste dela en instrumentpanel om du vill göra den tillgänglig för hela företaget.</span><span class="sxs-lookup"><span data-stu-id="7913f-143">To make a dashboard available across your enterprise, you need to share it.</span></span> <span data-ttu-id="7913f-144">Vi ska titta på det alternativet inom kort.</span><span class="sxs-lookup"><span data-stu-id="7913f-144">We'll look at that option shortly.</span></span>

<a name="create-new"></a>

## <a name="create-a-new-dashboard"></a><span data-ttu-id="7913f-145">Skapa en ny instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-145">Create a new dashboard</span></span>

<span data-ttu-id="7913f-146">Om du vill skapa en ny instrumentpanel klickar du på **Ny instrumentpanel**.</span><span class="sxs-lookup"><span data-stu-id="7913f-146">To create a new dashboard, click **New dashboard**.</span></span> <span data-ttu-id="7913f-147">Arbetsytan för instrumentpanelen öppnas utan paneler.</span><span class="sxs-lookup"><span data-stu-id="7913f-147">The dashboard workspace appears, with no tiles present.</span></span> <span data-ttu-id="7913f-148">Du kan sedan lägga till, ta bort och justera paneler som du vill.</span><span class="sxs-lookup"><span data-stu-id="7913f-148">You can then add, remove and adjust tiles however you like.</span></span> <span data-ttu-id="7913f-149">När du är klar med att anpassa instrumentpanelen klickar du på **Anpassningen är klar** för att spara och växla till den instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7913f-149">When you are finished customizing the dashboard, click **Done customizing** to save and switch to that dashboard.</span></span>

<a name="upload-download"></a>

## <a name="upload-and-download"></a><span data-ttu-id="7913f-150">Ladda upp och ladda ned</span><span class="sxs-lookup"><span data-stu-id="7913f-150">Upload and Download</span></span>

<span data-ttu-id="7913f-151">Med knapparna **Ladda upp** och **Ladda ned** kan du ladda ned din befintliga instrumentpanel som en JSON-fil, anpassa den och sedan distribuera och ladda upp den eller låta någon annan ladda upp filen igen till Azure Portal, så att deras befintliga instrumentpanel ersätts.</span><span class="sxs-lookup"><span data-stu-id="7913f-151">The **Upload** and **Download** buttons enable you to download your current dashboard as a JSON file, customize it, and then distribute it and upload it or have someone else upload that file back to the Azure portal, thereby replacing their current dashboard.</span></span>

<span data-ttu-id="7913f-152">Om du klickar på **Ladda ned** laddas den aktuella instrumentpanelen ned till din standardmapp för nedladdningar.</span><span class="sxs-lookup"><span data-stu-id="7913f-152">If you click **Download**, the current dashboard downloads into your default Downloads folder.</span></span> <span data-ttu-id="7913f-153">När du öppnar den nedladdade filen visas JSON-koden.</span><span class="sxs-lookup"><span data-stu-id="7913f-153">Opening the downloaded file then shows the JSON code.</span></span>

![JSON-kod för instrumentpanel](../media-draft/8-dashboard-json-code.png)

<span data-ttu-id="7913f-155">Du kan sedan redigera koden manuellt (till exempel genom att ändra panelstorlekar) och sedan ladda upp till Azure genom att klicka på knappen **Ladda upp**.</span><span class="sxs-lookup"><span data-stu-id="7913f-155">You can then edit that code manually (for example, by changing tile sizes) and then upload it back to Azure by clicking the **Upload** button.</span></span>

<a name="edit-dashboard"></a>

### <a name="edit-a-dashboard"></a><span data-ttu-id="7913f-156">Redigera en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-156">Edit a dashboard</span></span>

<span data-ttu-id="7913f-157">Det går att redigera en instrumentpanel genom att ladda ned JSON-filen, ändra värdena i filen och ladda upp den till Azure igen, men den här metoden är inte intuitiv för användargränssnittets utformning.</span><span class="sxs-lookup"><span data-stu-id="7913f-157">Although you can edit a dashboard by downloading the JSON file, changing values in the file, and uploading the file back to Azure, that approach isn't intuitive for designing a user interface.</span></span> <span data-ttu-id="7913f-158">Om du vill använda det grafiska användargränssnittet för att konfigurera din aktuella instrumentpanel kan du aktivera redigeringsläget på flera sätt:</span><span class="sxs-lookup"><span data-stu-id="7913f-158">To use the GUI to configure your current dashboard you can enter edit mode in several ways:</span></span>

1. <span data-ttu-id="7913f-159">Klicka på knappen **Redigera**</span><span class="sxs-lookup"><span data-stu-id="7913f-159">Click the **Edit** button</span></span>
1. <span data-ttu-id="7913f-160">Högerklicka på instrumentpanelen och klicka på **Redigera**.</span><span class="sxs-lookup"><span data-stu-id="7913f-160">Right-click on the dashboard and click **Edit**.</span></span> 
1. <span data-ttu-id="7913f-161">Hovra över en panel på instrumentpanelen – en `...`-menyn visas i det övre högra hörnet med redigeringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="7913f-161">Hover over a tile on the dashboard - a `...` menu will appear on the top/right corner with edit options.</span></span>

<span data-ttu-id="7913f-162">Instrumentpanelen övergår till redigeringsläget.</span><span class="sxs-lookup"><span data-stu-id="7913f-162">The dashboard switches to edit mode.</span></span>

![Redigera instrumentpanel](../media-draft/8-edit-dashboard.png)

<span data-ttu-id="7913f-164">På vänster sida visas panelgalleriet med flera tillgängliga paneler.</span><span class="sxs-lookup"><span data-stu-id="7913f-164">On the left-hand side appears the Tile Gallery, with several possible tiles.</span></span> <span data-ttu-id="7913f-165">Du kan filtrera panelgalleriet via kategori och resurstyp:</span><span class="sxs-lookup"><span data-stu-id="7913f-165">You can filter the Tile Gallery by category and resource type:</span></span>

![Panelgalleri](../media-draft/8-tile-gallery.png)

<span data-ttu-id="7913f-167">Det är enkelt att lägga till paneler. Välj bara panelen i listan till vänster och dra den till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="7913f-167">Adding tiles is as easy as selecting the tile from the list on the left and then dragging it to the work area.</span></span> <span data-ttu-id="7913f-168">Sedan kan du flytta varje panel till önskad plats, ändra dess storlek eller vilka data som visas.</span><span class="sxs-lookup"><span data-stu-id="7913f-168">You can then move each tile about, resize it, or change the data that it displays.</span></span>

> [!TIP]
> <span data-ttu-id="7913f-169">En fiffig funktion som många inte känner till är att du kan välja element på underordnade blad och lägga till dem på din instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="7913f-169">One cool feature a lot of people are unaware of is that you can take elements on child blades and put them on your dashboard.</span></span> <span data-ttu-id="7913f-170">Hovra bara över objektet och leta efter `...`-panelredigeringsmenyn – den visas med alternativet ”Fäst på instrumentpanelen” – som du kan använda för att snabbt hämta en panel från en tjänst och placera den på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7913f-170">Just hover over the item and look for the `...` tile edit menu - this will have a "Pin to Dashboard" option which lets you quickly grab a tile from a service and put it onto the dashboard.</span></span>

<span data-ttu-id="7913f-171">Arbetsytan är indelad i rutor i redigeringsläget.</span><span class="sxs-lookup"><span data-stu-id="7913f-171">The work area in edit mode is divided into squares.</span></span> <span data-ttu-id="7913f-172">Varje panel måste fylla minst en ruta och panelerna fästs intill närmaste största panelavgränsare.</span><span class="sxs-lookup"><span data-stu-id="7913f-172">Each tile must occupy at least one square, and tiles will snap to the nearest largest set of tile dividers.</span></span> <span data-ttu-id="7913f-173">Överlappande paneler flyttas undan.</span><span class="sxs-lookup"><span data-stu-id="7913f-173">Any overlapping tiles are moved out of the way.</span></span> <span data-ttu-id="7913f-174">När du gör en panel mindre flyttas omgivande paneler och placeras intill den panelen.</span><span class="sxs-lookup"><span data-stu-id="7913f-174">When you make a tile smaller, the surrounding tiles will move back up against it.</span></span>

#### <a name="change-tile-sizes"></a><span data-ttu-id="7913f-175">Ändra panelstorlekar</span><span class="sxs-lookup"><span data-stu-id="7913f-175">Change tile sizes</span></span>

<span data-ttu-id="7913f-176">Vissa paneler har en fast storlek och du kan bara redigera deras storlek programmatiskt.</span><span class="sxs-lookup"><span data-stu-id="7913f-176">Some tiles have a set size, and you can edit their size only programmatically.</span></span> <span data-ttu-id="7913f-177">Du kan däremot redigera paneler med ett grått hörn längst ned till höger genom att dra hörnindikatorn.</span><span class="sxs-lookup"><span data-stu-id="7913f-177">However, you can edit tiles with a gray bottom right-hand corner by dragging the corner indicator.</span></span>

![Panel som kan storleksanpassas](../media-draft/8-resizable-tile.png)

<span data-ttu-id="7913f-179">Du kan också högerklicka på snabbmenyn och ange önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="7913f-179">Alternatively, right-click the context menu and specify the size you want.</span></span>

![Panelstorlek](../media-draft/8-tile-size.png)

<span data-ttu-id="7913f-181">Skapa en instrumentpanel genom att dra paneler från panelgalleriet till arbetsytan och sedan ordna dem.</span><span class="sxs-lookup"><span data-stu-id="7913f-181">To create your dashboard, pull tiles from the Tile Gallery onto the workspace and then rearrange them.</span></span>

#### <a name="change-tile-settings"></a><span data-ttu-id="7913f-182">Ändra panelinställningar</span><span class="sxs-lookup"><span data-stu-id="7913f-182">Change tile settings</span></span>

<span data-ttu-id="7913f-183">Vissa paneler har inställningar som kan redigeras.</span><span class="sxs-lookup"><span data-stu-id="7913f-183">Some tiles have editable settings.</span></span> <span data-ttu-id="7913f-184">När du till exempel drar klockpanelen till arbetsytan öppnas panelen **Redigera klockan**.</span><span class="sxs-lookup"><span data-stu-id="7913f-184">For example, with the clock tile, when you drag it onto the workspace, it opens the **Edit clock** tile.</span></span> <span data-ttu-id="7913f-185">Här kan du ange den tidszon som visas och välja 12- eller 24-timmarsformat.</span><span class="sxs-lookup"><span data-stu-id="7913f-185">You can then set the time zone, which it displays, and also set whether it displays in 12- or 24-hour format.</span></span>

![Redigera klockan](../media-draft/8-edit-clock.png)

<span data-ttu-id="7913f-187">Om ditt företag har verksamhet i flera länder eller på olika kontinenter kan du lägga till klockor – var och en i olika tidszoner.</span><span class="sxs-lookup"><span data-stu-id="7913f-187">For multi-national or transcontinental companies, you can add clocks, each in a different time zone.</span></span>

#### <a name="accepting-your-edits"></a><span data-ttu-id="7913f-188">Godkänna redigeringarna</span><span class="sxs-lookup"><span data-stu-id="7913f-188">Accepting your edits</span></span>

<span data-ttu-id="7913f-189">När du är klar med att ordna panelerna kan du klicka på **Anpassningen är klar** eller högerklicka och välja **Anpassningen är klar**.</span><span class="sxs-lookup"><span data-stu-id="7913f-189">When you have arranged the tiles as you want them, either click **Done customizing**, or right-click and then click **Done customizing**.</span></span>

## <a name="edit-a-dashboard-by-changing-the-json-file"></a><span data-ttu-id="7913f-190">Redigera en instrumentpanel genom att ändra JSON-filen</span><span class="sxs-lookup"><span data-stu-id="7913f-190">Edit a dashboard by changing the JSON file</span></span>

<span data-ttu-id="7913f-191">Du kan även redigera en instrumentpanel genom att ändra JSON-filen.</span><span class="sxs-lookup"><span data-stu-id="7913f-191">You can also edit a dashboard by changing the JSON file.</span></span> <span data-ttu-id="7913f-192">Med den här metoden får du fler alternativ för att ändra inställningar, men ändringarna visas inte förrän du har laddat upp filen till Azure igen.</span><span class="sxs-lookup"><span data-stu-id="7913f-192">This approach provides more options for changing settings, but you cannot see the changes until you upload the file back into Azure.</span></span>

![JSON-inställningar](../media-draft/8-json-code.png)

<span data-ttu-id="7913f-194">I exemplet ovan ändrar du storlek på panelen genom att redigera variablerna **colSpan** och **rowSpan**. Spara sedan filen och ladda upp den till Azure igen.</span><span class="sxs-lookup"><span data-stu-id="7913f-194">In the example above, to change the size of the tile, edit the **colSpan** and **rowSpan** variables, then save the file and upload it back to Azure.</span></span> <span data-ttu-id="7913f-195">Filen kan även distribueras till andra användare.</span><span class="sxs-lookup"><span data-stu-id="7913f-195">You can also distribute the file to other users.</span></span>

## <a name="reset-a-dashboard"></a><span data-ttu-id="7913f-196">Återställa en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-196">Reset a dashboard</span></span>

<span data-ttu-id="7913f-197">En instrumentpanel kan när som helst återställas till standardtillståndet.</span><span class="sxs-lookup"><span data-stu-id="7913f-197">You can reset any dashboard to the default style.</span></span> <span data-ttu-id="7913f-198">Högerklicka och välj **Återställ till standardtillstånd** i redigeringsläget.</span><span class="sxs-lookup"><span data-stu-id="7913f-198">In edit mode, right-click and select **Reset to default state**.</span></span> <span data-ttu-id="7913f-199">En dialogruta öppnas där du ombeds bekräfta att du vill återställa instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7913f-199">A dialog box will ask you to confirm that you want to reset that dashboard.</span></span>

<a name="share-dashboard"></a>

## <a name="share-or-unshare-a-dashboard"></a><span data-ttu-id="7913f-200">Dela eller sluta dela en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-200">Share or unshare a dashboard</span></span>

<span data-ttu-id="7913f-201">När du definierar en ny instrumentpanel är den privat och syns endast på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="7913f-201">When you define a new dashboard, it is private and visible only to your account.</span></span> <span data-ttu-id="7913f-202">Om du vill göra instrumentpanelen synlig för andra måste du dela den.</span><span class="sxs-lookup"><span data-stu-id="7913f-202">To make it visible to others, you need to share a dashboard.</span></span> <span data-ttu-id="7913f-203">Som för alla andra Azure-resurser behöver du ange en resursgrupp (eller använda en befintlig resursgrupp) för att lagra delade instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="7913f-203">However, as with any other Azure resource, you need to specify a resource group (or use an existing resource group) to store shared dashboards in.</span></span> <span data-ttu-id="7913f-204">Om du inte har en resursgrupp skapar Azure en resursgrupp för *instrumentpaneler* på den plats som du anger.</span><span class="sxs-lookup"><span data-stu-id="7913f-204">If you do not have an existing resource group, Azure will create a *dashboards* resource group in whichever location you specify.</span></span> <span data-ttu-id="7913f-205">Om du har resursgrupper kan du välja en resursgrupp för att lagra instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="7913f-205">If you have existing resource groups, you can specify that resource group to store the dashboards.</span></span>

![Delning och åtkomstkontroll 1](../media-draft/8-share-dashboards-default.png)

<span data-ttu-id="7913f-207">När du har delat mallen visas andra bladet för **Delning och åtkomstkontroll**.</span><span class="sxs-lookup"><span data-stu-id="7913f-207">When you have shared the template, you will see a second **Sharing + access control** blade.</span></span>

![Delning och åtkomstkontroll 2](../media-draft/8-share-dashboards-access-control.png)

<span data-ttu-id="7913f-209">Nu kan du klicka på **Hantera användare** och ange vilka användare som ska ha åtkomst till instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7913f-209">You can then click **Manage users** to specify the users who have access to that dashboard.</span></span>

### <a name="switching-to-a-shared-dashboard"></a><span data-ttu-id="7913f-210">Växla till en delad instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-210">Switching to a shared dashboard</span></span>

<span data-ttu-id="7913f-211">Om du vill växla till en delad instrumentpanel klickar du på listan över instrumentpaneler och sedan på **Bläddra bland alla instrumentpaneler**.</span><span class="sxs-lookup"><span data-stu-id="7913f-211">To switch to a shared dashboard, you click on the list of dashboards, and then click **Browse all dashboards**.</span></span>

![Bläddra bland alla instrumentpaneler](../media-draft/8-browse-dashboards.png)

<span data-ttu-id="7913f-213">Nu visas bladet **Alla instrumentpaneler**, och namnen på alla delade instrumentpaneler visas.</span><span class="sxs-lookup"><span data-stu-id="7913f-213">You will now see the **All dashboards** blade, with the names of any shared dashboards displayed.</span></span> <span data-ttu-id="7913f-214">Klicka bara på en instrumentpanel för att tillämpa den på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7913f-214">Just click on a dashboard to apply it to the Azure portal.</span></span>

![Delade instrumentpaneler](../media-draft/8-select-shared-dashboard.png)

<a name="full-screen"></a>

## <a name="display-a-dashboard-as-a-full-screen"></a><span data-ttu-id="7913f-216">Visa en instrumentpanel i fullskärmsläge</span><span class="sxs-lookup"><span data-stu-id="7913f-216">Display a dashboard as a full screen</span></span>

<span data-ttu-id="7913f-217">Om du vill visa instrumentpanelen i största möjliga storlek klickar du på **fullskärmsknappen** så visas din aktuella instrumentpanel utan några webbläsarmenyer.</span><span class="sxs-lookup"><span data-stu-id="7913f-217">If you want the largest dashboard real estate, click the **Full screen** button to display your current dashboard without any browser menus.</span></span> <span data-ttu-id="7913f-218">Om det finns paneler utanför skärmvisningens gränser visas skjutreglage till höger och längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="7913f-218">If you have any tiles outside the boundaries of your screen display, slider bars will appear at the right and bottom of your screen.</span></span>

<span data-ttu-id="7913f-219">När du är klar med att arbeta i fullskärmsläge trycker du på ESC eller klickar på **Avsluta fullskärmsläge** bredvid instrumentpanelens namn högst upp på skärmen.</span><span class="sxs-lookup"><span data-stu-id="7913f-219">When you have finished working in full-screen mode, press the ESC key or click **Exit Full Screen** next to the Dashboard name at the top of the screen.</span></span>

<a name="clone-dashboard"></a>

## <a name="clone-a-dashboard"></a><span data-ttu-id="7913f-220">Klona en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-220">Clone a dashboard</span></span>

<span data-ttu-id="7913f-221">När du klonar en instrumentpanel skapas en kopia som kallas ”Klon av \<instrumentpanelens namn>” och kopian blir den aktuella instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7913f-221">Cloning a dashboard creates an instant copy called "Clone of \<dashboard name>" and switches to that copy as the current dashboard.</span></span> <span data-ttu-id="7913f-222">Att klona är även ett enkelt sätt att skapa instrumentpaneler innan du delar dem.</span><span class="sxs-lookup"><span data-stu-id="7913f-222">Cloning is also an easy way to create dashboards before sharing them.</span></span> <span data-ttu-id="7913f-223">Om du till exempel har en instrumentpanel som är nästan klar, kan du klona den, göra de ändringar som behövs och sedan dela den.</span><span class="sxs-lookup"><span data-stu-id="7913f-223">For example, if you have a dashboard that is almost as you want it, clone it, make the changes that you need, and then share it.</span></span>

<a name="delete-dashboard"></a>

## <a name="delete-a-dashboard"></a><span data-ttu-id="7913f-224">Ta bort en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="7913f-224">Delete a dashboard</span></span>

<span data-ttu-id="7913f-225">När du tar bort en instrumentpanel tas den även bort från listan med tillgängliga instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="7913f-225">Deleting a dashboard removes it from your list of available dashboards.</span></span> <span data-ttu-id="7913f-226">Du uppmanas att bekräfta att instrumentpanelen ska tas bort. Det går inte att återställa en instrumentpanel som har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="7913f-226">You are prompted to confirm that you want to delete the dashboard, but there is no facility to recover a dashboard that has been deleted.</span></span>

<span data-ttu-id="7913f-227">Vi ska prova några av alternativen genom att skapa en ny instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="7913f-227">Let's try out some of these options by creating a new dashboard.</span></span>
