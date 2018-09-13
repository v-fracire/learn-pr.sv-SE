<span data-ttu-id="c5205-101">Nu när vi har ett konto kan vi logga in på **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="c5205-101">Now that we have an account, we can sign into the **Azure portal**.</span></span> <span data-ttu-id="c5205-102">Portalen är en webbaserad administrationsplats som låter dig interagera med alla dina prenumerationer och resurser som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="c5205-102">The portal is a web-based administration site that lets you interact with all of your subscriptions and resources you have created.</span></span> <span data-ttu-id="c5205-103">Nästan allt du gör med Azure kan göras via det här webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c5205-103">Almost everything you do with Azure can be done through this web interface.</span></span>

## <a name="azure-portal-layout"></a><span data-ttu-id="c5205-104">Layout för Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c5205-104">Azure portal layout</span></span>

<span data-ttu-id="c5205-105">Azure-portalen är det primära grafiska användargränssnittet (GUI) som du använder för att styra Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c5205-105">The Azure portal is the primary graphical user interface (GUI) for controlling Microsoft Azure.</span></span> <span data-ttu-id="c5205-106">Du kan utföra de flesta viktiga hanteringsåtgärder på portalen och det är vanligtvis det bästa gränssnittet för att enskilda åtgärder eller där du vill se konfigurationsalternativen i detalj.</span><span class="sxs-lookup"><span data-stu-id="c5205-106">You can carry out the majority of management actions in the portal, and it is typically the best interface for carrying out single tasks or where you want to look at the configuration options in detail.</span></span>

![Azure-portalen](../media-draft/5-portal.png)

:::row:::

    :::column:::
    ![The resources and favorites](../media-draft/5-favorites.png)
    :::column-end:::

    :::column span="3":::
    **Resource Panel**
    
    In the left-hand sidebar of the portal is the resource pane, which lists the main resource types. Note that Azure has more resource types than just those shown. The resources listed are part of your _favorites_. 

    You can customize this with the specific resource types you tend to create or administer most often. 

    You can also collapse this pane; with the **<<** caret. This will minimize it to just icons which can be convenient if you are working with limited screen real-estate.
    :::column-end:::

:::row-end:::

<span data-ttu-id="c5205-108">Resten av portalvyn är för de specifika element som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="c5205-108">The remainder of the portal view is for the specific elements you are working with.</span></span> <span data-ttu-id="c5205-109">Standardsidan (startsidan) är _instrumentpanelen_.</span><span class="sxs-lookup"><span data-stu-id="c5205-109">The default (main) page is the _dashboard_.</span></span> <span data-ttu-id="c5205-110">Vi tar upp det lite senare, men det representerar en anpassningsbar överblick över dina resurser.</span><span class="sxs-lookup"><span data-stu-id="c5205-110">We'll cover this a bit later, but this represents a customizable birds-eye-view of your resources.</span></span> <span data-ttu-id="c5205-111">Du kan använda den för att hoppa till specifika resurser som du vill hantera eller söka efter resurser med inlägget **Alla resurser** i resurspanelen.</span><span class="sxs-lookup"><span data-stu-id="c5205-111">You can use it to jump into specific resources you want to manage, or search for resources with the **All resources** entry in the resource panel.</span></span> <span data-ttu-id="c5205-112">När du hanterar en resurs som en virtuell dator eller en webbapp, kommer du att arbeta med ett _blad_ som visar specifik information om resursen.</span><span class="sxs-lookup"><span data-stu-id="c5205-112">When you are managing a resource, such as a virtual machine or a web app, you will work with a _blade_ that presents specific information about the resource.</span></span>

## <a name="what-is-a-blade"></a><span data-ttu-id="c5205-113">Vad är ett blad?</span><span class="sxs-lookup"><span data-stu-id="c5205-113">What is a blade?</span></span>

<span data-ttu-id="c5205-114">Azure-portalen använder en bladmodell för navigering.</span><span class="sxs-lookup"><span data-stu-id="c5205-114">The Azure portal uses a blades model for navigation.</span></span> <span data-ttu-id="c5205-115">Ett _blad_ är en öppningsbar panel som innehåller gränssnitt för en enda nivå i en navigeringssekvens.</span><span class="sxs-lookup"><span data-stu-id="c5205-115">A _blade_ is a slide-out panel containing the UI for a single level in a navigation sequence.</span></span> <span data-ttu-id="c5205-116">Till exempel skulle vart och ett av elementen i den här sekvensen representeras av ett blad: **Virtuella datorer** > **Compute** > **Ubuntu Server**.</span><span class="sxs-lookup"><span data-stu-id="c5205-116">For example, each of these elements in this sequence would be represented by a blade: **Virtual machines** > **Compute** > **Ubuntu Server**.</span></span>

<span data-ttu-id="c5205-117">Varje blad innehåller viss information och de konfigurerbara alternativ.</span><span class="sxs-lookup"><span data-stu-id="c5205-117">Each blade contains some information and configurable options.</span></span> <span data-ttu-id="c5205-118">Vissa av dessa alternativ genererar ett annat blad som visas till höger om eventuella befintliga blad.</span><span class="sxs-lookup"><span data-stu-id="c5205-118">Some of these options generate another blade, which reveals itself to the right of any existing blade.</span></span> <span data-ttu-id="c5205-119">På det nya bladet visas eventuella ytterligare konfigurerbara alternativ som kan starta ett nytt blad och så vidare.</span><span class="sxs-lookup"><span data-stu-id="c5205-119">On the new blade, any further configurable options will spawn another blade, and so on.</span></span> <span data-ttu-id="c5205-120">Efter ett tag kan du ha flera blad öppna samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c5205-120">Pretty soon, you can end up with several blades open at the same time.</span></span> <span data-ttu-id="c5205-121">Du kan maximera bladen så att de fyller hela skärmen.</span><span class="sxs-lookup"><span data-stu-id="c5205-121">You can maximize blades as well so that they fill the entire screen.</span></span>

<span data-ttu-id="c5205-122">Eftersom nya blad alltid läggs till höger om sin ägare, kan du använda rullningslisten längst ned i fönstret för att gå bakåt och se hur du kom hit i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c5205-122">Since new blades are always added to the right of the owner, you can use the scrollbar at the bottom of the window to go backwards to see how you got to this spot in the configuration.</span></span> <span data-ttu-id="c5205-123">Alternativt kan du stänga bladen individuellt genom att klicka på `X`-knappen i det övre hörnet på bladet.</span><span class="sxs-lookup"><span data-stu-id="c5205-123">Alternatively, you can close blades individually by clicking the `X` button in the top corner of the blade.</span></span> <span data-ttu-id="c5205-124">Om du har ändringar som inte sparats, kommer Azure informera dig att ändringarna går förlorade om du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="c5205-124">If you have unsaved changes, Azure will prompt you to let you know that the changes will be lost if you continue.</span></span>

## <a name="configuring-settings-in-the-azure-portal"></a><span data-ttu-id="c5205-125">Konfigurera inställningarna i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c5205-125">Configuring settings in the Azure portal</span></span>

<span data-ttu-id="c5205-126">Azure-portalen visar flera konfigurationsalternativ, för det mesta i statusfältet längst upp till höger på skärmen.</span><span class="sxs-lookup"><span data-stu-id="c5205-126">The Azure portal displays several configuration options, mostly in the status bar at the top-right of the screen.</span></span>

### <a name="notifications"></a><span data-ttu-id="c5205-127">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="c5205-127">Notifications</span></span>

<span data-ttu-id="c5205-128">När du klickar på klockikonen visas fönstret **Meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="c5205-128">Clicking the bell icon displays the **Notifications** pane.</span></span> <span data-ttu-id="c5205-129">Det här fönstret visar de senaste åtgärderna som utförts samt deras status.</span><span class="sxs-lookup"><span data-stu-id="c5205-129">This pane lists the last actions that have been carried out, along with their status.</span></span>

![Meddelandebladet](../media-draft/5-notifications-blade.png)

### <a name="cloud-shell"></a><span data-ttu-id="c5205-131">Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="c5205-131">Cloud Shell</span></span>

<span data-ttu-id="c5205-132">Om du klickar på ikonen för **Cloud Shell** (>_) skapar du en ny Azure Cloud Shell-session.</span><span class="sxs-lookup"><span data-stu-id="c5205-132">If you click the **Cloud Shell** icon (>_), you will create a new Azure Cloud Shell session.</span></span> <span data-ttu-id="c5205-133">Azure Cloud Shell är ett interaktivt, webbläsartillgängligt skal för att hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="c5205-133">Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources.</span></span> <span data-ttu-id="c5205-134">Det ger dig flexibilitet att välja den skalupplevelse som bäst passar ditt sätt att arbeta.</span><span class="sxs-lookup"><span data-stu-id="c5205-134">It provides the flexibility of choosing the shell experience that best suits the way you work.</span></span> <span data-ttu-id="c5205-135">Linux-användare kan välja en Bash-upplevelse och Windows-användare kan välja PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5205-135">Linux users can opt for a Bash experience, while Windows users can opt for PowerShell.</span></span> <span data-ttu-id="c5205-136">Den här webbläsarbaserade terminalen låter dig styra och administrera alla dina Azure-resurser i den aktuella prenumerationen via ett kommandoradsgränssnitt som är inbyggt i portalen.</span><span class="sxs-lookup"><span data-stu-id="c5205-136">This browser-based terminal lets you control and administer all of your Azure resources in the current subscription through a command-line interface built right into the portal.</span></span>

![Cloud Shell](../media-draft/5-choose-shell.png)

### <a name="settings"></a><span data-ttu-id="c5205-138">Inställningar</span><span class="sxs-lookup"><span data-stu-id="c5205-138">Settings</span></span>

<span data-ttu-id="c5205-139">Klicka på **kugghjulsikonen** för att ändra inställningarna för Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c5205-139">Click the **gear** icon to change the Azure portal settings.</span></span> <span data-ttu-id="c5205-140">Några exempel på inställningar är:</span><span class="sxs-lookup"><span data-stu-id="c5205-140">These settings include:</span></span>

- <span data-ttu-id="c5205-141">Utloggningstid</span><span class="sxs-lookup"><span data-stu-id="c5205-141">Logout time</span></span>
- <span data-ttu-id="c5205-142">Färgschema</span><span class="sxs-lookup"><span data-stu-id="c5205-142">Color scheme</span></span>
- <span data-ttu-id="c5205-143">Högkontrastteman</span><span class="sxs-lookup"><span data-stu-id="c5205-143">High contrast themes</span></span>
- <span data-ttu-id="c5205-144">Toast-meddelanden (till en mobil enhet)</span><span class="sxs-lookup"><span data-stu-id="c5205-144">Toast notifications (to a mobile device)</span></span>
- <span data-ttu-id="c5205-145">Dubbelklicka för att ändra temat</span><span class="sxs-lookup"><span data-stu-id="c5205-145">Double-click to change the theme</span></span>
- <span data-ttu-id="c5205-146">Språk</span><span class="sxs-lookup"><span data-stu-id="c5205-146">Language</span></span>
- <span data-ttu-id="c5205-147">Regionalt format</span><span class="sxs-lookup"><span data-stu-id="c5205-147">Regional format</span></span>

![Inställningar för portalen](../media-draft/5-settings-blade.png)

<span data-ttu-id="c5205-149">När du har ändrat inställningarna klickar du på **Tillämpa** för att godkänna ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c5205-149">When you have changed settings, click **Apply** to accept your changes.</span></span>

### <a name="feedback-blade"></a><span data-ttu-id="c5205-150">Feedbackbladet</span><span class="sxs-lookup"><span data-stu-id="c5205-150">Feedback blade</span></span>

<span data-ttu-id="c5205-151">Ikonen med **uttryckssymbolen** öppnar bladet **Skicka feedback**.</span><span class="sxs-lookup"><span data-stu-id="c5205-151">The **smiley face** icon opens the **Send us feedback** blade.</span></span> <span data-ttu-id="c5205-152">Här kan du skicka feedback till Microsoft om Azure.</span><span class="sxs-lookup"><span data-stu-id="c5205-152">Here you can send feedback to Microsoft about Azure.</span></span> <span data-ttu-id="c5205-153">Observera att du kan ange om Microsoft kan svara på din feedback via e-post.</span><span class="sxs-lookup"><span data-stu-id="c5205-153">Note that you can specify whether Microsoft can respond to your feedback by email.</span></span>

![Feedback](../media-draft/5-feedback-blade.png)

### <a name="help-blade"></a><span data-ttu-id="c5205-155">Hjälpbladet</span><span class="sxs-lookup"><span data-stu-id="c5205-155">Help blade</span></span>

<span data-ttu-id="c5205-156">Klicka på **frågetecknet** för att visa bladet **Hjälp**.</span><span class="sxs-lookup"><span data-stu-id="c5205-156">Click the **question mark** icon to show the **Help** blade.</span></span> <span data-ttu-id="c5205-157">Här kan du välja mellan flera alternativ, inklusive:</span><span class="sxs-lookup"><span data-stu-id="c5205-157">Here you choose from several options, including:</span></span>

- <span data-ttu-id="c5205-158">Nyheter</span><span class="sxs-lookup"><span data-stu-id="c5205-158">What's new</span></span>
- <span data-ttu-id="c5205-159">Azure-översikt</span><span class="sxs-lookup"><span data-stu-id="c5205-159">Azure roadmap</span></span>
- <span data-ttu-id="c5205-160">Starta guidad visning</span><span class="sxs-lookup"><span data-stu-id="c5205-160">Launch guided tour</span></span>
- <span data-ttu-id="c5205-161">Kortkommandon</span><span class="sxs-lookup"><span data-stu-id="c5205-161">Keyboard shortcuts</span></span>
- <span data-ttu-id="c5205-162">Visa diagnostik</span><span class="sxs-lookup"><span data-stu-id="c5205-162">Show diagnostics</span></span>
- <span data-ttu-id="c5205-163">Sekretess + villkor</span><span class="sxs-lookup"><span data-stu-id="c5205-163">Privacy + terms</span></span>

### <a name="directory-and-subscription"></a><span data-ttu-id="c5205-164">Katalog och prenumeration</span><span class="sxs-lookup"><span data-stu-id="c5205-164">Directory and subscription</span></span>

<span data-ttu-id="c5205-165">Klicka på ikonen **Boka och filtrera** för att visa bladet **Katalog + prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="c5205-165">Click the **Book and Filter** icon to show the **Directory + subscription** blade.</span></span>

<span data-ttu-id="c5205-166">Azure gör det möjligt för dig att ha mer än en prenumeration associerad med en katalog.</span><span class="sxs-lookup"><span data-stu-id="c5205-166">Azure allows you to have more than one subscription associated with one directory.</span></span> <span data-ttu-id="c5205-167">Du kan ändra mellan prenumerationer på bladet **Katalog och prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="c5205-167">On the **Directory + subscription** blade, you can change between subscriptions.</span></span> <span data-ttu-id="c5205-168">Här kan du kan ändra din prenumeration eller ändra till en annan katalog.</span><span class="sxs-lookup"><span data-stu-id="c5205-168">Here, you can change your subscription or change to another directory.</span></span>

![Katalog](../media-draft/5-directory-blade-1.png)

### <a name="profile-settings"></a><span data-ttu-id="c5205-170">Profilinställningar</span><span class="sxs-lookup"><span data-stu-id="c5205-170">Profile settings</span></span>

<span data-ttu-id="c5205-171">Om du klickar på ditt namn i det övre högra hörnet kan du sedan ändra profilinställningarna.</span><span class="sxs-lookup"><span data-stu-id="c5205-171">If you click on your name in the top right-hand corner, you can then change your profile settings.</span></span>
<span data-ttu-id="c5205-172">Alternativen är:</span><span class="sxs-lookup"><span data-stu-id="c5205-172">Options include:</span></span>

- <span data-ttu-id="c5205-173">Logga ut från Azure</span><span class="sxs-lookup"><span data-stu-id="c5205-173">Sign out of Azure</span></span>
- <span data-ttu-id="c5205-174">Ändra lösenord</span><span class="sxs-lookup"><span data-stu-id="c5205-174">Change password</span></span>
- <span data-ttu-id="c5205-175">Ändra kontaktuppgifter</span><span class="sxs-lookup"><span data-stu-id="c5205-175">Change contact information</span></span>
- <span data-ttu-id="c5205-176">Visa behörigheter</span><span class="sxs-lookup"><span data-stu-id="c5205-176">View permissions</span></span>
- <span data-ttu-id="c5205-177">Skicka en idé till Azure-teamet</span><span class="sxs-lookup"><span data-stu-id="c5205-177">Submit an idea to the Azure team</span></span>
- <span data-ttu-id="c5205-178">Visa din faktura</span><span class="sxs-lookup"><span data-stu-id="c5205-178">View your bill</span></span>
- <span data-ttu-id="c5205-179">Växla katalog (visar bladet **Katalog + prenumeration** som i föregående avsnitt)</span><span class="sxs-lookup"><span data-stu-id="c5205-179">Switch directory (shows the **Directory + subscription** blade as in the previous section)</span></span>

![Profilinställningar](../media-draft/5-portal-menu.png)

<span data-ttu-id="c5205-181">Om du nu klickar på **Visa min faktura** tar Azure dig till sidan **Kostnadshantering + Fakturering – Fakturor** som hjälper dig att analysera var Azure genererar kostnader.</span><span class="sxs-lookup"><span data-stu-id="c5205-181">If you now click **View my bill**, Azure takes you to the **Cost Management + Billing - Invoices** page, which helps you analyze where Azure is generating costs.</span></span>

![Faktureringssida](../media-draft/5-portal-billing.png)

<span data-ttu-id="c5205-183">Azure är en stor produkt och det återspeglas i portalens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c5205-183">Azure is a large product, and the Azure portal user interface (UI) reflects this.</span></span> <span data-ttu-id="c5205-184">Metoden med glidande blad låter dig navigera fram och tillbaka mellan de olika administrationsuppgifterna med enkelhet.</span><span class="sxs-lookup"><span data-stu-id="c5205-184">The sliding blade approach allows you to navigate back and forth through the various administration tasks with ease.</span></span> <span data-ttu-id="c5205-185">Nu ska vi testa lite med det här användargränssnittet så att du får öva lite.</span><span class="sxs-lookup"><span data-stu-id="c5205-185">Let's experiment a bit with this UI so you get some practice.</span></span>