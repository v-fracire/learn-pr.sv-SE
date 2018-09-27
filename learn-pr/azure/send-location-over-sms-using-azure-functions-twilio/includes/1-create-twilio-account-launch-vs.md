> [!TIP]
> <span data-ttu-id="f97da-101">Användarnamnet och lösenordet som du behöver för att logga in på den virtuella datorn finns på fliken **Resurser**.</span><span class="sxs-lookup"><span data-stu-id="f97da-101">The username and password you need to sign in to the VM are located on the **Resources** tab.</span></span>

> [!NOTE]
> <span data-ttu-id="f97da-102">Om du använder en Mac kan du när du startar den virtuella datorn behöva använda antingen blixtikonen i verktygsfältet eller alternativet **Ctrl+Alt+Delete** på fliken **Resurser** bredvid anvisningarna för att låsa upp den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f97da-102">If you are using a Mac, after launching the VM you may need to use either the lightning icon on the toolbar, or the **Ctrl+Alt+Delete** option from the **Resources** tab next to the instructions to unlock the VM.</span></span>


<span data-ttu-id="f97da-103">I den här modulen skapar du en plattformsoberoende Xamarin.Forms-app med en serverlös serverdel.</span><span class="sxs-lookup"><span data-stu-id="f97da-103">In this module, you'll create a cross-platform Xamarin.Forms app with a serverless back end.</span></span> <span data-ttu-id="f97da-104">Appen hämtar användarnas position från deras enheter och skickar både den och en lista med telefonnummer till en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="f97da-104">This app will get the user's location from their device and send it with a list of phone numbers to an Azure function.</span></span> <span data-ttu-id="f97da-105">Funktionen använder sedan en bindning till en tjänst från tredje part (Twilio) och skickar positionen som ett SMS till de angivna telefonnumren.</span><span class="sxs-lookup"><span data-stu-id="f97da-105">The function will then use a binding to a third-party service (Twilio) to send your location as an SMS message to all the phone numbers provided.</span></span>

<span data-ttu-id="f97da-106">Den här processen omfattar följande steg:</span><span class="sxs-lookup"><span data-stu-id="f97da-106">This process involves the following steps:</span></span>

1. <span data-ttu-id="f97da-107">Appen samlar in platsinformationen med Xamarin.Essentials som en abstraktion över via enhetsspecifika plats-API:er.</span><span class="sxs-lookup"><span data-stu-id="f97da-107">The app captures your location using Xamarin.Essentials as an abstraction over device-specific location APIs.</span></span>

1. <span data-ttu-id="f97da-108">Platsinformationen och telefonnumren paketeras i en JSON-nyttolast och skickas till en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="f97da-108">The location and phone numbers are packaged up into a JSON payload and sent to an Azure function.</span></span>

1. <span data-ttu-id="f97da-109">Azure-funktionen avkodar JSON-nyttolasten och skapar SMS:en.</span><span class="sxs-lookup"><span data-stu-id="f97da-109">The Azure function decodes the JSON payload and creates SMS messages.</span></span>

1. <span data-ttu-id="f97da-110">SMS:en skickas via [Twilio](https://www.twilio.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="f97da-110">The SMS messages are sent via [Twilio](https://www.twilio.com/?azure-portal=true).</span></span>

<span data-ttu-id="f97da-111">Följande bild visar en översikt över den här processen.</span><span class="sxs-lookup"><span data-stu-id="f97da-111">The following illustration shows an overview of this process.</span></span>

![En illustration med en övergripande arkitektur för processen för med att dela plats via SMS.](../media/1-architecture.png)

## <a name="create-a-twilio-account"></a><span data-ttu-id="f97da-113">Skapa ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="f97da-113">Create a Twilio account</span></span>

<span data-ttu-id="f97da-114">Om du vill kunna skicka SMS från en Azure-funktion måste du ha ett Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="f97da-114">To be able to send SMS messages from an Azure function, you'll need a Twilio account.</span></span> <span data-ttu-id="f97da-115">Det kostnadsfria kontot räcker för att du ska komma igång.</span><span class="sxs-lookup"><span data-stu-id="f97da-115">The free account is more than enough to get started.</span></span>

1. <span data-ttu-id="f97da-116">Gå till [twilio.com](https://www.twilio.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="f97da-116">Head to [twilio.com](https://www.twilio.com?azure-portal=true).</span></span>

1. <span data-ttu-id="f97da-117">Klicka på den röda knappen **Sign Up** (registrera dig) uppe till höger.</span><span class="sxs-lookup"><span data-stu-id="f97da-117">Click the red **Sign Up** button in the top-right corner.</span></span>

1. <span data-ttu-id="f97da-118">Fyll i dina uppgifter och klicka på **Get Started** (kom igång).</span><span class="sxs-lookup"><span data-stu-id="f97da-118">Fill in your details and click **Get Started**.</span></span>

1. <span data-ttu-id="f97da-119">Du måste verifiera ditt telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="f97da-119">You'll need to verify your phone number.</span></span> <span data-ttu-id="f97da-120">Med Twilios kostnadsfria konto kan du bara skicka meddelanden till verifierade telefonnummer, så att de inte ska kunna användas för att skicka skräppost.</span><span class="sxs-lookup"><span data-stu-id="f97da-120">Twilio free accounts let you send messages only to verified phone numbers to stop them being used for spam.</span></span> <span data-ttu-id="f97da-121">Twilio skickar en verifieringskod som du måste ange för att verifiera din telefon.</span><span class="sxs-lookup"><span data-stu-id="f97da-121">Twilio will send you a verification code that you need to enter to verify your phone.</span></span>

1. <span data-ttu-id="f97da-122">Välj fliken **Produkter**, klicka på **Programmable SMS** (Programmerbart SMS) och klicka sedan på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="f97da-122">Select the **Products** tab, and click **Programmable SMS**, then click **Continue**.</span></span>

1. <span data-ttu-id="f97da-123">Ange ett namn för ditt första projekt, till exempel ”Hej världen”, och klicka sedan på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="f97da-123">Provide a name for your first project, such as "I'm here", then click **Continue**.</span></span>

1. <span data-ttu-id="f97da-124">Hoppa över det här steget om du vill bjuda in en teammedlem.</span><span class="sxs-lookup"><span data-stu-id="f97da-124">Skip the step to invite a team mate.</span></span>

1. <span data-ttu-id="f97da-125">Expandera panelen **Projektinformation** från meddelandeinstrumentpanelen i Twilio.</span><span class="sxs-lookup"><span data-stu-id="f97da-125">From the Twilio messaging dashboard, expand the **Project Info** panel.</span></span>

1. <span data-ttu-id="f97da-126">Anteckna värdena för **KONTO-SID** och **AUTENTISERINGSTOKEN** eftersom du behöver dem senare.</span><span class="sxs-lookup"><span data-stu-id="f97da-126">Note your **ACCOUNT SID** and **AUTH TOKEN** because you will need these values later.</span></span>

    <span data-ttu-id="f97da-127">När du skapar ett Twilio-konto tilldelas du ett telefonnummer som du kan skicka meddelanden från.</span><span class="sxs-lookup"><span data-stu-id="f97da-127">When you create a Twilio account, you are assigned a phone number that you can send messages from.</span></span> <span data-ttu-id="f97da-128">Du hittar det här telefonnumret på Twilio-instrumentpanelen **Phone Numbers** (telefonnummer).</span><span class="sxs-lookup"><span data-stu-id="f97da-128">You can find this phone number on the Twilio **Phone Numbers** dashboard.</span></span>

1. <span data-ttu-id="f97da-129">Välj ellipserna längst ned i den vänstra menyn på Twilio-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="f97da-129">From the Twilio site, select the ellipses at the bottom of the left-hand menu.</span></span> <span data-ttu-id="f97da-130">Välj sedan *SUPER NETWORK -> Phone Numbers*.</span><span class="sxs-lookup"><span data-stu-id="f97da-130">Then, select *SUPER NETWORK->Phone Numbers*.</span></span> <span data-ttu-id="f97da-131">Du kan fästa den här instrumentpanelen vid den vänstra menyn med en knappnålsikon.</span><span class="sxs-lookup"><span data-stu-id="f97da-131">You can pin this dashboard to the left-hand menu using the pin icon.</span></span> <span data-ttu-id="f97da-132">Du ser ditt Twilio-nummer under *Manage Numbers -> Active Numbers* (hantera nummer -> aktiva nummer).</span><span class="sxs-lookup"><span data-stu-id="f97da-132">Your Twilio number will be under *Manage Numbers->Active Numbers*.</span></span>

    ![Hitta ditt Twilio-nummer](../media/7-twilio-find-number.png)

    > [!TIP]
    > <span data-ttu-id="f97da-134">Om du inte ännu har ett aktivt nummer väljer du **Get Started** (Kom igång) på sidan Aktiva nummer för att starta processen med att skapa ett nummer.</span><span class="sxs-lookup"><span data-stu-id="f97da-134">If you don't have an active number yet, select **Get Started** in the Active Numbers page to begin the process of creating a number.</span></span>

1. <span data-ttu-id="f97da-135">Notera ditt aktiva telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="f97da-135">Note your active phone number.</span></span> <span data-ttu-id="f97da-136">Det kommer att användas senare i modulen.</span><span class="sxs-lookup"><span data-stu-id="f97da-136">It will be used later in this module.</span></span>


> [!NOTE]
> <span data-ttu-id="f97da-137">När du registrerar dig tilldelas du ett Twilio-telefonnummer som används för att skicka SMS.</span><span class="sxs-lookup"><span data-stu-id="f97da-137">When you sign up, you will be assigned a Twilio phone number that will be used to send SMS messages.</span></span> <span data-ttu-id="f97da-138">I vissa länder kan det hända att dessa nummer inte kan skicka SMS.</span><span class="sxs-lookup"><span data-stu-id="f97da-138">In some countries, these numbers may not be able to send SMS messages.</span></span> <span data-ttu-id="f97da-139">Twilio-dokumentationen anger [vilka länder som har begränsningar](https://support.twilio.com/hc/articles/223183068-Twilio-international-phone-number-availability-and-their-capabilities?azure-portal=true) och visar olika sätt att skicka SMS med hjälp av ett [internationellt nummer eller alfanumeriskt avsändar-ID](https://support.twilio.com/hc/articles/226690868-Using-Twilio-when-SMS-numbers-are-unavailable-in-your-country?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="f97da-139">The Twilio documentation lists [which countries have restrictions](https://support.twilio.com/hc/articles/223183068-Twilio-international-phone-number-availability-and-their-capabilities?azure-portal=true), and shows ways to send SMS messages using an [international number or AlphaNumeric sender Id](https://support.twilio.com/hc/articles/226690868-Using-Twilio-when-SMS-numbers-are-unavailable-in-your-country?azure-portal=true).</span></span>

## <a name="launch-visual-studio"></a><span data-ttu-id="f97da-140">Starta Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97da-140">Launch Visual Studio</span></span>

<span data-ttu-id="f97da-141">I den här modulen kommer du att utveckla mobilappen och Azure Functions-appen i Visual Studio 2017, som är tillgängligt via en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f97da-141">For this module, you'll develop the mobile app and Azure Functions app using Visual Studio 2017, available via a virtual machine.</span></span> <span data-ttu-id="f97da-142">Även om du kan skapa Xamarin.Forms-appar för körning i iOS, Android och Universal Windows Platform (UWP) så ligger fokus i den här modulen på UWP så att appen fungerar i den virtuella labbdatorn.</span><span class="sxs-lookup"><span data-stu-id="f97da-142">Although Xamarin.Forms apps can be built to run on iOS, Android, and Universal Windows Platform (UWP), this module will just focus on UWP so that it works inside the lab virtual machine.</span></span>

<span data-ttu-id="f97da-143">Starta Visual Studio 2017 från den virtuella datorns startmeny eller från genvägen på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="f97da-143">Launch Visual Studio 2017 from the VM's Start Menu, or from the desktop shortcut.</span></span>

## <a name="summary"></a><span data-ttu-id="f97da-144">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f97da-144">Summary</span></span>

<span data-ttu-id="f97da-145">I den här utbildningsenheten har du skapat ett Twilio-konto som du ska använda till att skicka SMS, och du har startat Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f97da-145">In this unit, you created a Twilio account to use to send SMS messages and launched Visual Studio.</span></span> <span data-ttu-id="f97da-146">I nästa steg får du lära dig att skapa en Xamarin.Forms-app och lägga till NuGet-paketet Xamarin.Essentials.</span><span class="sxs-lookup"><span data-stu-id="f97da-146">Next, you learn how to create a Xamarin.Forms app and add the Xamarin.Essentials NuGet package.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f97da-147">Notera värdena för **KONTO-SID** och **AUTENTISERINGSTOKEN** och **Active Phone Number** (Aktivt telefonnummer) i Twilio som du samlade in i den här enheten, eftersom du behöver dessa värden senare.</span><span class="sxs-lookup"><span data-stu-id="f97da-147">Keep note of the Twilio  **ACCOUNT SID** and **AUTH TOKEN** and **Active Phone Number** values that you gathered in this unit, because you'll need these values later.</span></span>
