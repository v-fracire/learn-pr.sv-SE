<span data-ttu-id="0722f-101">Här installerar du Eclipse och Azure Toolkit på en utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="0722f-101">Here you'll install Eclipse and the Azure Toolkit on your development machine.</span></span> <span data-ttu-id="0722f-102">I slutet av övningen har du allt du behöver för att skapa ett Java-program som är anslutet till Azure.</span><span class="sxs-lookup"><span data-stu-id="0722f-102">By the end of the exercise, you'll have everything you need to create a Java application connected to Azure.</span></span>

## <a name="install-eclipse-ide"></a><span data-ttu-id="0722f-103">Installera Eclipse IDE</span><span class="sxs-lookup"><span data-stu-id="0722f-103">Install Eclipse IDE</span></span>

1. <span data-ttu-id="0722f-104">Ladda ned lämplig [Eclipse IDE för ditt operativsystem](https://www.eclipse.org/downloads/packages/installer).</span><span class="sxs-lookup"><span data-stu-id="0722f-104">Download the appropriate [Eclipse IDE for your operating system](https://www.eclipse.org/downloads/packages/installer).</span></span>

1. <span data-ttu-id="0722f-105">Starta installationsprogrammet för Eclipse när det har hämtats.</span><span class="sxs-lookup"><span data-stu-id="0722f-105">Start the Eclipse installer once downloaded.</span></span>

    1. <span data-ttu-id="0722f-106">I Windows dubbelklickar du på den nedladdade filen.</span><span class="sxs-lookup"><span data-stu-id="0722f-106">On Windows, double-click the downloaded file.</span></span>

    1. <span data-ttu-id="0722f-107">I macOS och Linux packar du upp installationsprogrammet från den nedladdade filen och kör det.</span><span class="sxs-lookup"><span data-stu-id="0722f-107">On macOS and Linux, unzip the installer from the downloaded file and run it.</span></span>

        > [!NOTE]
        > <span data-ttu-id="0722f-108">Du kan uppmanas att installera Java Development Kit om det saknas.</span><span class="sxs-lookup"><span data-stu-id="0722f-108">The installer may prompt you to install the Java Development Kit, if it is missing.</span></span>

1. <span data-ttu-id="0722f-109">Välj de paket som ska installeras.</span><span class="sxs-lookup"><span data-stu-id="0722f-109">Select the packages to install.</span></span> <span data-ttu-id="0722f-110">Om du är Java-utvecklare väljer du alternativet Java eller Java EE Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="0722f-110">For Java developers, choose either the Java or Java EE Eclipse IDE option.</span></span>

1. <span data-ttu-id="0722f-111">Välj installationsmålet på din dator.</span><span class="sxs-lookup"><span data-stu-id="0722f-111">Select the installation destination on your machine.</span></span>

1. <span data-ttu-id="0722f-112">Starta Eclipse för att kontrollera att det har installerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="0722f-112">Launch Eclipse to validate that it installed correctly.</span></span>

## <a name="install-azure-toolkit-for-eclipse"></a><span data-ttu-id="0722f-113">Installera Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="0722f-113">Install Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="0722f-114">Azure Toolkit installeras på samma sätt i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="0722f-114">Installing the Azure Toolkit is the same across Windows, macOS, and Linux.</span></span>

1. <span data-ttu-id="0722f-115">Starta Eclipse.</span><span class="sxs-lookup"><span data-stu-id="0722f-115">Start Eclipse.</span></span>

1. <span data-ttu-id="0722f-116">Gå till **Hjälp** > **Installera ny programvara...**.</span><span class="sxs-lookup"><span data-stu-id="0722f-116">Go to **Help** > **Install New Software...**.</span></span>

    <span data-ttu-id="0722f-117">På följande skärmbild visas menyplatsen för alternativet **Installera ny programvara...**.</span><span class="sxs-lookup"><span data-stu-id="0722f-117">The following screenshot shows the menu location of the **Install New Software...** item.</span></span>

    ![Skärmbild av alternativet Installera ny programvara markerat på Hjälp-menyn i Eclipse.](../media/7-eclipse-install-new-software.png)

1. <span data-ttu-id="0722f-119">Dialogrutan **Tillgänglig programvara** öppnas.</span><span class="sxs-lookup"><span data-stu-id="0722f-119">The **Available Software** dialog will open.</span></span> <span data-ttu-id="0722f-120">Gå till textrutan **Arbeta med:**, skriv `http://dl.microsoft.com/eclipse/` och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="0722f-120">In the **Work with:** text box, type `http://dl.microsoft.com/eclipse/` and press Enter.</span></span>

1. <span data-ttu-id="0722f-121">Markera alternativet **Azure Toolkit för Java** i resultatet.</span><span class="sxs-lookup"><span data-stu-id="0722f-121">In the results, check the **Azure Toolkit for Java** option.</span></span> <span data-ttu-id="0722f-122">Avmarkera alternativet **Kontakta alla uppdateringsplatser under installationen för att hitta nödvändig programvara** om det inte redan är avmarkerat.</span><span class="sxs-lookup"><span data-stu-id="0722f-122">Make sure you uncheck the **Contact all update sites during install to find required software** option, if it isn't already.</span></span>

    <span data-ttu-id="0722f-123">Följande skärmbild visar installationskonfigurationen **Tillgänglig programvara** som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="0722f-123">The following screenshot shows the **Available Software** install configuration as described above.</span></span>

    ![Skärmbild av fönstret Tillgänglig programvara i Eclipse med rutor som visar den konfigurationen som krävs för att hitta och installera Azure Toolkit för Java.](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. <span data-ttu-id="0722f-125">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="0722f-125">Click **Next**.</span></span>

1. <span data-ttu-id="0722f-126">Granska och acceptera licensavtalet när du uppmanas att göra det och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="0722f-126">Review and accept the license agreements when prompted, and click **Finish**.</span></span>

1. <span data-ttu-id="0722f-127">Eclipse laddar ned och installerar Azure Toolkit.</span><span class="sxs-lookup"><span data-stu-id="0722f-127">Eclipse will download and install the Azure Toolkit.</span></span>

1. <span data-ttu-id="0722f-128">Starta om Eclipse om det behövs.</span><span class="sxs-lookup"><span data-stu-id="0722f-128">Restart Eclipse if required.</span></span>

1. <span data-ttu-id="0722f-129">Verifiera installationen av Azure Toolkit genom att kontrollera att menyalternativet **Verktyg** > **Azure** visas i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="0722f-129">Validate the Azure Toolkit installation by verifying that you can find a **Tools** > **Azure** menu option in Eclipse.</span></span>
