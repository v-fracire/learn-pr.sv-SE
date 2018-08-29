<span data-ttu-id="ffa75-101">Azure-produkter har en djup hierarki.</span><span class="sxs-lookup"><span data-stu-id="ffa75-101">Azure products have a deep hierarchy.</span></span> <span data-ttu-id="ffa75-102">Anta till exempel att du behöver skapa en virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="ffa75-102">For example, suppose you needed to create a Linux Virtual Machine.</span></span> <span data-ttu-id="ffa75-103">Du kan gå igenom dessa nivåer: **Start** **>** **Virtuella datorer** **>** **Compute** **>** **Ubuntu Server**.</span><span class="sxs-lookup"><span data-stu-id="ffa75-103">You might navigate through these levels: **Home** **>** **Virtual machines** **>** **Compute** **>** **Ubuntu Server**.</span></span> <span data-ttu-id="ffa75-104">Azure-portalen optimerar användargränssnittet för att göra den här typen av navigeringssekvens intuitiv.</span><span class="sxs-lookup"><span data-stu-id="ffa75-104">The Azure portal optimizes its UI to make this type of navigation sequence intuitive.</span></span> <span data-ttu-id="ffa75-105">Här kommer du undersöka de viktiga UI-elementen som gör detta möjligt.</span><span class="sxs-lookup"><span data-stu-id="ffa75-105">Here, you will survey the key UI elements that make this possible.</span></span> <span data-ttu-id="ffa75-106">Du navigerar genom menyer och undermenyer och använder blad för att hitta och konfigurera tjänster.</span><span class="sxs-lookup"><span data-stu-id="ffa75-106">You will navigate through menus and sub-menus and use blades to find and configure services.</span></span>

## <a name="azure-portal-layout"></a><span data-ttu-id="ffa75-107">Layout för Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ffa75-107">Azure Portal Layout</span></span>

<span data-ttu-id="ffa75-108">Azure-portalen är det viktigaste grafiska användargränssnittet (GUI) som du använder för att styra Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ffa75-108">The Azure portal is the main graphical user interface (GUI) for controlling Microsoft Azure.</span></span> <span data-ttu-id="ffa75-109">Du kan utföra de flesta viktiga hanteringsåtgärder på portalen och det är vanligtvis det bästa gränssnittet för att utföra enskilda åtgärder eller för att se konfigurationsalternativ i detalj.</span><span class="sxs-lookup"><span data-stu-id="ffa75-109">You can carry out the large majority of management actions on the portal and the portal is typically the best interface for carrying out single tasks or where you want to look at the configuration options in detail.</span></span>

<span data-ttu-id="ffa75-110">Fönstret på vänster sida i portalen är resursfönstret som visar de viktigaste resurstyperna.</span><span class="sxs-lookup"><span data-stu-id="ffa75-110">On the left-hand pane of the portal is the resource pane, which lists the main resource types.</span></span> <span data-ttu-id="ffa75-111">Observera att Azure har många fler resurstyper än de som visas.</span><span class="sxs-lookup"><span data-stu-id="ffa75-111">Note that Azure has far many more resource types than just those shown.</span></span>

## <a name="using-blades-in-azure-portal"></a><span data-ttu-id="ffa75-112">Använda blad på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ffa75-112">Using Blades in Azure Portal</span></span>

<span data-ttu-id="ffa75-113">Azure-portalen använder modellen med blad för navigering.</span><span class="sxs-lookup"><span data-stu-id="ffa75-113">The Azure Portal uses a blades model for navigation.</span></span> <span data-ttu-id="ffa75-114">Ett _blad_ är en öppningsbar panel som innehåller gränssnitt för en enda nivå i en navigeringssekvens.</span><span class="sxs-lookup"><span data-stu-id="ffa75-114">A _blade_ is a slide-out panel containing the UI for a single level in a navigation sequence.</span></span> <span data-ttu-id="ffa75-115">Var och en av de här elementen i den här sekvensen representeras till exempel av ett blad: **Virtuella datorer** **>** **Compute** **>** **Ubuntu Server**.</span><span class="sxs-lookup"><span data-stu-id="ffa75-115">For example, each of these elements in this sequence would be represented by a blade: **Virtual machines** **>** **Compute** **>** **Ubuntu Server**.</span></span>

<span data-ttu-id="ffa75-116">Varje blad i användargränssnittet innehåller vanligtvis ett antal konfigurerbara alternativ.</span><span class="sxs-lookup"><span data-stu-id="ffa75-116">Each blade within the UI typically contains a number of configurable options.</span></span> <span data-ttu-id="ffa75-117">Vissa av dessa alternativ genererar ett annat blad, som visas till höger om eventuella befintliga blad.</span><span class="sxs-lookup"><span data-stu-id="ffa75-117">Some of these options generate another blade, which reveals itself to the right of any existing blade.</span></span> <span data-ttu-id="ffa75-118">På det nya bladet visas eventuella ytterligare konfigurerbara alternativ som kan starta ett nytt blad och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ffa75-118">On the new blade, any further configurable options will spawn another blade, and so on.</span></span> <span data-ttu-id="ffa75-119">Efter ett tag kan du ha flera blad öppna samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ffa75-119">Pretty soon, you can end up with several blades open at the same time.</span></span> <span data-ttu-id="ffa75-120">Du kan maximera blad så att de fyller hela skärmen.</span><span class="sxs-lookup"><span data-stu-id="ffa75-120">You can maximize blades as well so that they fill the entire screen.</span></span>

<span data-ttu-id="ffa75-121">Om du försöker stänga ett blad utan att spara de konfigurationsändringar du gjort kommer du att få en fråga om du verkligen vill fortsätta.</span><span class="sxs-lookup"><span data-stu-id="ffa75-121">If you try to close a blade without saving any configuration changes that you have made, then you will receive a prompt.</span></span>

## <a name="configuring-settings-in-azure-portal"></a><span data-ttu-id="ffa75-122">Konfigurera inställningar i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ffa75-122">Configuring Settings in Azure Portal</span></span>

<span data-ttu-id="ffa75-123">Azure-portalen visar flera konfigurationsalternativ, för det mesta i statusfältet längst upp till höger på skärmen.</span><span class="sxs-lookup"><span data-stu-id="ffa75-123">The Azure portal displays several configuration options, mostly in the status bar to the top-right of the screen.</span></span>

### <a name="notifications"></a><span data-ttu-id="ffa75-124">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="ffa75-124">Notifications</span></span>

<span data-ttu-id="ffa75-125">När du klickar på klockikonen visas meddelandefönstret.</span><span class="sxs-lookup"><span data-stu-id="ffa75-125">Clicking the bell icon displays the Notifications pane.</span></span> <span data-ttu-id="ffa75-126">Det här fönstret visar de senaste åtgärder som utförts samt deras status.</span><span class="sxs-lookup"><span data-stu-id="ffa75-126">This pane lists the last actions that have been carried out, along with their status.</span></span>

![Meddelandebladet](../images/2-notifications-blade.PNG)

### <a name="cloud-shell"></a><span data-ttu-id="ffa75-128">Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="ffa75-128">Cloud Shell</span></span>

<span data-ttu-id="ffa75-129">Om du klickar på ikonen för Cloud Shell (>_) skapar du en ny Cloud Shell-session.</span><span class="sxs-lookup"><span data-stu-id="ffa75-129">If you click the Cloud Shell icon (>_), you will create a new cloud shell session.</span></span> <span data-ttu-id="ffa75-130">Du uppmanas att använda Linux Bash eller PowerShell på Linux i den aktuella sessionen.</span><span class="sxs-lookup"><span data-stu-id="ffa75-130">You are prompted to use either Linux Bash or PowerShell on Linux in that session.</span></span>

![Cloud Shell](../images/2-choose-shell.PNG)

### <a name="settings"></a><span data-ttu-id="ffa75-132">Inställningar</span><span class="sxs-lookup"><span data-stu-id="ffa75-132">Settings</span></span>

<span data-ttu-id="ffa75-133">Klicka på ”kugghjulsikonen” för att ändra inställningarna för Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ffa75-133">Clicking on the "gear" icon to change Azure Portal settings.</span></span> <span data-ttu-id="ffa75-134">Några exempel på inställningar är:</span><span class="sxs-lookup"><span data-stu-id="ffa75-134">These settings include:</span></span>

* <span data-ttu-id="ffa75-135">Utloggningstid</span><span class="sxs-lookup"><span data-stu-id="ffa75-135">Logout time</span></span>
* <span data-ttu-id="ffa75-136">Färgschema</span><span class="sxs-lookup"><span data-stu-id="ffa75-136">Color scheme</span></span>
* <span data-ttu-id="ffa75-137">Högkontrastteman</span><span class="sxs-lookup"><span data-stu-id="ffa75-137">High contrast themes</span></span>
* <span data-ttu-id="ffa75-138">Popup-meddelanden (till en mobil enhet)</span><span class="sxs-lookup"><span data-stu-id="ffa75-138">Toast notifications (to a mobile device)</span></span>
* <span data-ttu-id="ffa75-139">Dubbelklicka för att ändra tema</span><span class="sxs-lookup"><span data-stu-id="ffa75-139">Double-click to change theme</span></span>
* <span data-ttu-id="ffa75-140">Språk</span><span class="sxs-lookup"><span data-stu-id="ffa75-140">Language</span></span>
* <span data-ttu-id="ffa75-141">Regionalt format</span><span class="sxs-lookup"><span data-stu-id="ffa75-141">Regional format</span></span>

![Inställningar för portalen](../images/2-settings-blade.PNG)

<span data-ttu-id="ffa75-143">När du har ändrat inställningarna klickar du på **Tillämpa** för att godkänna ändringarna.</span><span class="sxs-lookup"><span data-stu-id="ffa75-143">When you have changed settings, click **Apply** to accept your changes.</span></span>

### <a name="feedback-blade"></a><span data-ttu-id="ffa75-144">Feedbackblad</span><span class="sxs-lookup"><span data-stu-id="ffa75-144">Feedback Blade</span></span>

<span data-ttu-id="ffa75-145">Ikonen med uttryckssymbolen öppnar bladet **Skicka feedback**.</span><span class="sxs-lookup"><span data-stu-id="ffa75-145">The smiley face icon opens the **Send us feedback** blade.</span></span> <span data-ttu-id="ffa75-146">Här kan du skicka feedback till Microsoft om Azure.</span><span class="sxs-lookup"><span data-stu-id="ffa75-146">Here you can send feedback to Microsoft about Azure.</span></span> <span data-ttu-id="ffa75-147">Observera att du kan ange om Microsoft kan svara på din feedback via e-post.</span><span class="sxs-lookup"><span data-stu-id="ffa75-147">Note that you can specify whether Microsoft can respond to your feedback by email.</span></span>

![Feedback](../images/2-feedback-blade.PNG)

### <a name="help-blade"></a><span data-ttu-id="ffa75-149">Hjälpbladet</span><span class="sxs-lookup"><span data-stu-id="ffa75-149">Help Blade</span></span>

<span data-ttu-id="ffa75-150">Klicka på frågetecknet för att visa bladet **Hjälp**.</span><span class="sxs-lookup"><span data-stu-id="ffa75-150">Click the question mark to show the **Help** blade.</span></span> <span data-ttu-id="ffa75-151">Här kan du välja bland ett antal ämnen, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="ffa75-151">Here you choose from a number of topics, including:</span></span>

* <span data-ttu-id="ffa75-152">Nyheter</span><span class="sxs-lookup"><span data-stu-id="ffa75-152">What's new</span></span>
* <span data-ttu-id="ffa75-153">Azure-översikt</span><span class="sxs-lookup"><span data-stu-id="ffa75-153">Azure roadmap</span></span>
* <span data-ttu-id="ffa75-154">Starta guidad visning</span><span class="sxs-lookup"><span data-stu-id="ffa75-154">Launch guided tour</span></span>
* <span data-ttu-id="ffa75-155">Kortkommandon</span><span class="sxs-lookup"><span data-stu-id="ffa75-155">Keyboard shortcuts</span></span>
* <span data-ttu-id="ffa75-156">Visa diagnostik</span><span class="sxs-lookup"><span data-stu-id="ffa75-156">Show diagnostics</span></span>
* <span data-ttu-id="ffa75-157">Sekretess + villkor</span><span class="sxs-lookup"><span data-stu-id="ffa75-157">Privacy + terms</span></span>

### <a name="directory-and-subscription"></a><span data-ttu-id="ffa75-158">Katalog och prenumeration</span><span class="sxs-lookup"><span data-stu-id="ffa75-158">Directory and Subscription</span></span>

<span data-ttu-id="ffa75-159">Klicka på ikonen boka och filtrera för att visa bladet **Katalog + prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="ffa75-159">Click the Book and Filter icon to show the **Directory + subscription** blade.</span></span>

<span data-ttu-id="ffa75-160">Azure gör det möjligt för dig att ha mer än en prenumeration associerad med en katalog.</span><span class="sxs-lookup"><span data-stu-id="ffa75-160">Azure allows you to have more than one subscription associated with one directory.</span></span> <span data-ttu-id="ffa75-161">Du kan ändra mellan prenumerationer på bladet Katalog och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ffa75-161">In the Directory and Subscription blade, you can change between subscriptions.</span></span> <span data-ttu-id="ffa75-162">Här kan du kan ändra din prenumeration eller ändra till en annan katalog.</span><span class="sxs-lookup"><span data-stu-id="ffa75-162">Here, you can change your subscription, or change to another directory.</span></span>

![Katalog](../images/2-directory-blade-1.PNG)

### <a name="profile-settings"></a><span data-ttu-id="ffa75-164">Profilinställningar</span><span class="sxs-lookup"><span data-stu-id="ffa75-164">Profile Settings</span></span>

<span data-ttu-id="ffa75-165">Om du klickar på ditt namn i det övre högra hörnet kan du sedan ändra profilinställningarna.</span><span class="sxs-lookup"><span data-stu-id="ffa75-165">If you click on your name in the top right-hand corner, you can then change your profile settings.</span></span>
<span data-ttu-id="ffa75-166">Alternativen är:</span><span class="sxs-lookup"><span data-stu-id="ffa75-166">Options include:</span></span>

* <span data-ttu-id="ffa75-167">Logga ut från Azure</span><span class="sxs-lookup"><span data-stu-id="ffa75-167">Sign out of Azure</span></span>
* <span data-ttu-id="ffa75-168">Ändra lösenord</span><span class="sxs-lookup"><span data-stu-id="ffa75-168">Change password</span></span>
* <span data-ttu-id="ffa75-169">Ändra kontaktuppgifter</span><span class="sxs-lookup"><span data-stu-id="ffa75-169">Change contact information</span></span>
* <span data-ttu-id="ffa75-170">Visa behörigheter</span><span class="sxs-lookup"><span data-stu-id="ffa75-170">View permissions</span></span>
* <span data-ttu-id="ffa75-171">Skicka en idé till Azure-teamet</span><span class="sxs-lookup"><span data-stu-id="ffa75-171">Submit an idea to the Azure team</span></span>
* <span data-ttu-id="ffa75-172">Visa din faktura</span><span class="sxs-lookup"><span data-stu-id="ffa75-172">View your bill</span></span>
* <span data-ttu-id="ffa75-173">Växla katalog (visar bladet Katalog + prenumeration som i föregående avsnitt)</span><span class="sxs-lookup"><span data-stu-id="ffa75-173">Switch directory (shows the Directory + Subscription blade as in the previous section)</span></span>

![Profilinställningar](../images/2-portal-menu.png)

<span data-ttu-id="ffa75-175">Om du nu klickar på **Visa min faktura** tar Azure dig till sidan **Kostnadshantering + Fakturering – Fakturor** som hjälper dig att analysera var Azure genererar kostnader.</span><span class="sxs-lookup"><span data-stu-id="ffa75-175">If you now click **View my bill**, Azure takes you to the **Cost Management + Billing - Invoices** page, which helps you analyze where Azure is generating costs.</span></span>

![Faktureringssida](../images/2-portal-billing.PNG)

## <a name="summary"></a><span data-ttu-id="ffa75-177">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ffa75-177">Summary</span></span>

<span data-ttu-id="ffa75-178">Azure är en stor produkt och det återspeglas i portalens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ffa75-178">Azure is a large product and the portal UI reflects this.</span></span> <span data-ttu-id="ffa75-179">Det huvudsakliga sättet som portalen hjälper dig att navigera genom den här komplexiteten är med hjälp av blad som visar hierarkin.</span><span class="sxs-lookup"><span data-stu-id="ffa75-179">The primary way that the portal helps you navigate this complexity is with blades to indicate hierarchy.</span></span> <span data-ttu-id="ffa75-180">Blad gör det möjligt för dig att fokusera på en viss uppgift, samtidigt som det tydligt visar hur du gjorde för att nå den punkten.</span><span class="sxs-lookup"><span data-stu-id="ffa75-180">Blades let you focus on a specific task while clearly indicating the path you took to reach that point.</span></span>