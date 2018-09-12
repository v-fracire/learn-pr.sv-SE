<span data-ttu-id="ea05c-101">I den här enheten kommer du att installera Eclipse på din lokala dator och därefter installera Azure Toolkit så att du kan börja utveckla Java-program med Azure-integrering.</span><span class="sxs-lookup"><span data-stu-id="ea05c-101">In this unit, you will install Eclipse on your local machine and then install the Azure Toolkit, preparing you for developing Java applications with Azure integration.</span></span> <span data-ttu-id="ea05c-102">Installationen går snabbt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="ea05c-102">The installation is quick and simple.</span></span> <span data-ttu-id="ea05c-103">I slutet av den här övningen har du allt som krävs för att skapa ditt första Java-program och dra nytta av funktionerna och tjänsterna i Azure.</span><span class="sxs-lookup"><span data-stu-id="ea05c-103">At the end of the exercise, you will have everything set up that you need to start your first Java application, taking advantage of the features and services of Azure.</span></span>

## <a name="install-eclipse-ide"></a><span data-ttu-id="ea05c-104">Installera Eclipse IDE</span><span class="sxs-lookup"><span data-stu-id="ea05c-104">Install Eclipse IDE</span></span>

1. <span data-ttu-id="ea05c-105">Hämta Eclipse-versionen för ditt operativsystem från http://www.eclipse.org/downloads/packages/installer.</span><span class="sxs-lookup"><span data-stu-id="ea05c-105">Download the Eclipse version that suits your operating system from http://www.eclipse.org/downloads/packages/installer.</span></span>

1. <span data-ttu-id="ea05c-106">Starta installationsprogrammet för Eclipse när det har hämtats.</span><span class="sxs-lookup"><span data-stu-id="ea05c-106">Start the Eclipse installer once downloaded.</span></span>

    1. <span data-ttu-id="ea05c-107">I Windows dubbelklickar du på den nedladdade filen.</span><span class="sxs-lookup"><span data-stu-id="ea05c-107">On Windows, double-click the downloaded file.</span></span>

    1. <span data-ttu-id="ea05c-108">I macOS och Linux packar du upp installationsprogrammet från den nedladdade filen.</span><span class="sxs-lookup"><span data-stu-id="ea05c-108">On macOS and Linux, unzip the installer from the downloaded file.</span></span> <span data-ttu-id="ea05c-109">Starta sedan installationsprogrammet när det har packats upp.</span><span class="sxs-lookup"><span data-stu-id="ea05c-109">Then start the installer once unzipped.</span></span>

        > [!NOTE]
        > <span data-ttu-id="ea05c-110">Du kan uppmanas att installera Java Development Kit om det saknas.</span><span class="sxs-lookup"><span data-stu-id="ea05c-110">The installer may prompt you to install the Java Development Kit, if it is missing.</span></span>

1. <span data-ttu-id="ea05c-111">Välj de paket som ska installeras.</span><span class="sxs-lookup"><span data-stu-id="ea05c-111">Select the packages to install.</span></span> <span data-ttu-id="ea05c-112">Om du är Java-utvecklare väljer du alternativet Java eller Java EE Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="ea05c-112">For Java developers, choose either the Java or Java EE Eclipse IDE option.</span></span>

1. <span data-ttu-id="ea05c-113">Välj installationsmålet på din dator.</span><span class="sxs-lookup"><span data-stu-id="ea05c-113">Select the installation destination on your machine.</span></span>

1. <span data-ttu-id="ea05c-114">Starta Eclipse för att kontrollera att det har installerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="ea05c-114">Launch Eclipse to validate that it installed correctly.</span></span>

## <a name="install-azure-toolkit-for-eclipse"></a><span data-ttu-id="ea05c-115">Installera Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="ea05c-115">Install Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="ea05c-116">Azure Toolkit installeras på samma sätt i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="ea05c-116">Installing the Azure Toolkit is the same across Windows, macOS, and Linux.</span></span>

1. <span data-ttu-id="ea05c-117">Starta Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ea05c-117">Start Eclipse.</span></span>

1. <span data-ttu-id="ea05c-118">Gå till **Hjälp** > **Installera ny programvara...**.</span><span class="sxs-lookup"><span data-stu-id="ea05c-118">Go to **Help** > **Install New Software...**.</span></span>

    <span data-ttu-id="ea05c-119">På följande skärmbild visas menyplatsen för alternativet **Installera ny programvara...**.</span><span class="sxs-lookup"><span data-stu-id="ea05c-119">The following screenshot shows the menu location of the **Install New Software...** item.</span></span>

    ![Skärmbild av alternativet Installera ny programvara markerat på Hjälp-menyn i Eclipse.](../media/7-eclipse-install-new-software.png)

1. <span data-ttu-id="ea05c-121">Dialogrutan **Tillgänglig programvara** öppnas.</span><span class="sxs-lookup"><span data-stu-id="ea05c-121">The **Available Software** dialog will open.</span></span> <span data-ttu-id="ea05c-122">Gå till textrutan **Arbeta med:**, skriv `http://dl.microsoft.com/eclipse/` och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="ea05c-122">In the **Work with:** text box, type `http://dl.microsoft.com/eclipse/` and press Enter.</span></span>

1. <span data-ttu-id="ea05c-123">Markera alternativet **Azure Toolkit för Java** i resultatet.</span><span class="sxs-lookup"><span data-stu-id="ea05c-123">In the results, check the **Azure Toolkit for Java** option.</span></span> <span data-ttu-id="ea05c-124">Avmarkera alternativet **Kontakta alla uppdateringsplatser under installationen för att hitta nödvändig programvara** om det inte redan är avmarkerat.</span><span class="sxs-lookup"><span data-stu-id="ea05c-124">Make sure you uncheck the **Contact all update sites during install to find required software** option, if it isn't already.</span></span>

    <span data-ttu-id="ea05c-125">Följande skärmbild visar installationskonfigurationen **Tillgänglig programvara** som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="ea05c-125">The following screenshot shows the **Available Software** install configuration as described above.</span></span>

    ![Skärmbild av fönstret Tillgänglig programvara i Eclipse med rutor som visar den konfigurationen som krävs för att hitta och installera Azure Toolkit för Java.](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. <span data-ttu-id="ea05c-127">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ea05c-127">Click **Next**.</span></span>

1. <span data-ttu-id="ea05c-128">Granska och acceptera licensavtalet när du uppmanas att göra det och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="ea05c-128">Review and accept the license agreements when prompted, and click **Finish**.</span></span>

1. <span data-ttu-id="ea05c-129">Eclipse laddar ned och installerar Azure Toolkit.</span><span class="sxs-lookup"><span data-stu-id="ea05c-129">Eclipse will download and install the Azure Toolkit.</span></span>

1. <span data-ttu-id="ea05c-130">Starta om Eclipse om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ea05c-130">Restart Eclipse if required.</span></span>

1. <span data-ttu-id="ea05c-131">Verifiera installationen av Azure Toolkit genom att kontrollera att menyalternativet **Verktyg** > **Azure** visas i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ea05c-131">Validate installation of Azure Toolkit by verifying that you can find a **Tools** > **Azure** menu option in Eclipse.</span></span>

## <a name="summary"></a><span data-ttu-id="ea05c-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ea05c-132">Summary</span></span>

<span data-ttu-id="ea05c-133">I den här kursdelen har du installerat Eclipse för Java och förberett det för att använda integrering med tjänsterna och produkterna i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ea05c-133">In this unit, you installed Eclipse for Java, and prepared it to take advantage of the integration with Azure services and products.</span></span> <span data-ttu-id="ea05c-134">Installationen är snabb och enkel vilket gör Eclipse perfekt för Java-utveckling med integrering av molntjänster.</span><span class="sxs-lookup"><span data-stu-id="ea05c-134">The installation is quick and straightforward, making Eclipse ideal for the task of Java development with cloud services integration.</span></span>
