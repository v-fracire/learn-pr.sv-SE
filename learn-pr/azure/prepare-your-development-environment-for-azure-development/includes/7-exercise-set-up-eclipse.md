<span data-ttu-id="c37bd-101">I den här övningen ska du installera Eclipse på din lokala dator och sedan installera Azure Toolkit så att du kan börja utveckla Java-program med Azure-integrering.</span><span class="sxs-lookup"><span data-stu-id="c37bd-101">In this exercise you will install Eclipse on your local machine and then install the Azure Toolkit, preparing you for developing Java applications with Azure integration.</span></span> <span data-ttu-id="c37bd-102">Installationen går snabbt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="c37bd-102">The installation is quick and simple.</span></span> <span data-ttu-id="c37bd-103">I slutet av den här övningen har du allt som krävs för att skapa ditt första Java-program och dra nytta av funktionerna och tjänsterna i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c37bd-103">At the end of the exercise you will have everything set up needed to start your first Java application taking advantage of Microsoft Azure's features and services.</span></span>

## <a name="install-eclipse-ide"></a><span data-ttu-id="c37bd-104">Installera Eclipse IDE</span><span class="sxs-lookup"><span data-stu-id="c37bd-104">Install Eclipse IDE</span></span>

1. <span data-ttu-id="c37bd-105">Ladda ned Eclipse-versionen för ditt operativsystem från http://www.eclipse.org/downloads/packages/installer.</span><span class="sxs-lookup"><span data-stu-id="c37bd-105">Download the Eclipse version that suits your operating system from http://www.eclipse.org/downloads/packages/installer.</span></span>

    ![Gå till nedladdningsplatsen för installationsprogrammet på eclipse.org och välj installationsprogrammet för ditt operativsystem](../media-draft/7-go-to-eclipse-org.png)

1. <span data-ttu-id="c37bd-107">Starta installationsprogrammet för Eclipse när det har laddats ned.</span><span class="sxs-lookup"><span data-stu-id="c37bd-107">Start the Eclipse installer once downloaded.</span></span>
    1. <span data-ttu-id="c37bd-108">I Windows dubbelklickar du på den nedladdade filen.</span><span class="sxs-lookup"><span data-stu-id="c37bd-108">On Windows, double-click the downloaded file.</span></span>
    1. <span data-ttu-id="c37bd-109">I macOS och Linux packar du upp installationsprogrammet från den nedladdade filen.</span><span class="sxs-lookup"><span data-stu-id="c37bd-109">On macOS and Linux, unzip the installer from the downloaded file.</span></span> <span data-ttu-id="c37bd-110">Starta sedan installationsprogrammet när det har packats upp.</span><span class="sxs-lookup"><span data-stu-id="c37bd-110">Then start the installer once unzipped.</span></span>

        > [!NOTE]
        > <span data-ttu-id="c37bd-111">Du kan uppmanas att installera Java Development Kit om det saknas.</span><span class="sxs-lookup"><span data-stu-id="c37bd-111">The installer may prompt you to install the Java Development Kit, if it is missing.</span></span>

3. <span data-ttu-id="c37bd-112">Välj de paket som ska installeras.</span><span class="sxs-lookup"><span data-stu-id="c37bd-112">Select the packages to install.</span></span> <span data-ttu-id="c37bd-113">Om du är Java-utvecklare väljer du alternativet Java eller Java EE Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="c37bd-113">For Java developers, choose either the Java or Java EE Eclipse IDE option.</span></span>
4. <span data-ttu-id="c37bd-114">Välj installationsmålet på din dator.</span><span class="sxs-lookup"><span data-stu-id="c37bd-114">Select the installation destination on your machine.</span></span>
5. <span data-ttu-id="c37bd-115">Starta Eclipse för att bekräfta att det har installerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="c37bd-115">Launch Eclipse to validate it installed correctly.</span></span>

    ![Klicka på knappen Starta för att starta Eclipse efter installationen](../media-draft/7-launch-eclipse-from-installer.png)

## <a name="install-azure-toolkit-for-eclipse"></a><span data-ttu-id="c37bd-117">Installera Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="c37bd-117">Install Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="c37bd-118">Azure Toolkit installeras på samma sätt i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="c37bd-118">Installing the Azure Toolkit is the same approach across Windows, macOS, and Linux.</span></span>

1. <span data-ttu-id="c37bd-119">Starta Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c37bd-119">Start Eclipse.</span></span>
2. <span data-ttu-id="c37bd-120">Gå till **Help** (Hjälp)  > **Install New Software** (Installera ny programvara).</span><span class="sxs-lookup"><span data-stu-id="c37bd-120">Go to **Help** > **Install New Software...**.</span></span>

    ![Gå till menyalternativet Install New Software (Installera ny programvara) i Eclipse](../media-draft/7-eclipse-install-new-software.png)

3. <span data-ttu-id="c37bd-122">Dialogrutan **Available Software** (Tillgänglig programvara) öppnas.</span><span class="sxs-lookup"><span data-stu-id="c37bd-122">The **Available Software** dialog will open.</span></span> <span data-ttu-id="c37bd-123">Skriv `http://dl.microsoft.com/eclipse/` i textrutan **Work with** (Arbeta med) och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="c37bd-123">In the **Work with:** text box, type `http://dl.microsoft.com/eclipse/` and press enter.</span></span>
4. <span data-ttu-id="c37bd-124">Markera alternativet **Azure Toolkit for Java** (Azure Toolkit för Java) i resultatet.</span><span class="sxs-lookup"><span data-stu-id="c37bd-124">In the results, check the **Azure Toolkit for Java** option.</span></span> <span data-ttu-id="c37bd-125">Avmarkera alternativet **Contact all update sites during install to find required software** (Kontakta alla uppdateringsplatser under installationen för att hitta nödvändig programvara) om det inte redan är avmarkerat.</span><span class="sxs-lookup"><span data-stu-id="c37bd-125">Make sure you uncheck the **Contact all update sites during install to find required software** option, if it isn't already.</span></span>

    ![Söka efter och installera Azure Toolkit för Java i Eclipse](../media-draft/7-eclipse-download-azure-toolkit-for-java.png)

5. <span data-ttu-id="c37bd-127">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c37bd-127">Click **Next**.</span></span>
6. <span data-ttu-id="c37bd-128">Granska och acceptera licensavtalet när du uppmanas att göra det och klicka sedan på **Finish** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="c37bd-128">Review and accept the license agreements when prompted and click **Finish**.</span></span>
7. <span data-ttu-id="c37bd-129">Eclipse laddar ned och installerar Azure Toolkit.</span><span class="sxs-lookup"><span data-stu-id="c37bd-129">Eclipse will download and install the Azure Toolkit.</span></span>
8. <span data-ttu-id="c37bd-130">Starta om Eclipse om det behövs.</span><span class="sxs-lookup"><span data-stu-id="c37bd-130">Restart Eclipse if required.</span></span>
9. <span data-ttu-id="c37bd-131">Verifiera installationen av Azure Toolkit genom att kontrollera att menyalternativet **Tools** (Verktyg)  > **Azure** visas i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c37bd-131">Validate installation of Azure Toolkit by verifying you can find a **Tools** > **Azure** menu option in Eclipse.</span></span>

## <a name="summary"></a><span data-ttu-id="c37bd-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c37bd-132">Summary</span></span>

<span data-ttu-id="c37bd-133">I den här kursdelen har du installerat Eclipse för Java och förberett det för att dra nytta av integreringen med tjänsterna och produkterna i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c37bd-133">In this unit you installed Eclipse for Java and prepared it to take advantage of the integration with Microsoft Azure services and products.</span></span> <span data-ttu-id="c37bd-134">Installationen är snabb och enkel vilket gör Eclipse perfekt för Java-utveckling med integrering av molntjänster.</span><span class="sxs-lookup"><span data-stu-id="c37bd-134">The installation is quick and straightforward making Eclipse ideal for the task of Java development with cloud services integration.</span></span>
