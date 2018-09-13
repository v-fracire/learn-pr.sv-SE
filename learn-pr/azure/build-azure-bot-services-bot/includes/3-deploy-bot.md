<span data-ttu-id="bb725-101">När du skapade en Azure Web App-robot i [övning 1](#Exercise1) distribuerades även en Azure Web App som fungerar som värd.</span><span class="sxs-lookup"><span data-stu-id="bb725-101">When you created an Azure web app bot in [Exercise 1](#Exercise1), an Azure web app was deployed to host it.</span></span> <span data-ttu-id="bb725-102">Roboten kräver dock lite en del kodning och den måste även distribueras till Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="bb725-102">But the bot does require some code, and it still needs to be deployed to the Azure web app.</span></span> <span data-ttu-id="bb725-103">Som tur är kan har koden redan genererats åt dig av Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="bb725-103">Fortunately, the code was generated for you by the Azure Bot Service.</span></span> <span data-ttu-id="bb725-104">I den här övningen ska du använda Visual Studio Code för att placera koden på en lokal Git-lagringsplats och publicera roboten till Azure genom att skicka ändringarna från den lokala lagringsplatsen till en fjärrlagringsplats som är ansluten till den Azure Web App som är värd för roboten – en process som kallas [kontinuerlig intergrering](https://en.wikipedia.org/wiki/Continuous_integration).</span><span class="sxs-lookup"><span data-stu-id="bb725-104">In this exercise, you will use Visual Studio Code to place the code in a local Git repository and publish the bot to Azure by pushing changes from the local repository to a remote repository connected to the Azure web app that hosts the bot — a process known as [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration).</span></span>

1. <span data-ttu-id="bb725-105">Om [Git](https://git-scm.com/) är inte installerat på din dator går du till https://git-scm.com/downloads och installerar Git-klienten för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="bb725-105">If [Git](https://git-scm.com/) isn't installed on your PC, go to https://git-scm.com/downloads and install the Git client for your operating system.</span></span> <span data-ttu-id="bb725-106">Git är ett kostnadsfritt och distribuerat versionskontrollsystem med öppen källkod som integreras sömlöst i Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bb725-106">Git is a free and open-source distributed version control system, and it integrates seamlessly into Visual Studio Code.</span></span> <span data-ttu-id="bb725-107">Om du inte vet om Git är installerat öppnar du en kommandotolk eller ett terminalfönster och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bb725-107">If you aren't sure whether Git is installed, open a Command Prompt or terminal window and execute the following command:</span></span>

    ``` 
    git --version
    ```

    <span data-ttu-id="bb725-108">Om ett versionsnummer visas är Git-klienten installerad.</span><span class="sxs-lookup"><span data-stu-id="bb725-108">If a version number is displayed, then the Git client is installed.</span></span>

1. <span data-ttu-id="bb725-109">Om Node.js inte finns installerat på datorn går du till https://nodejs.org/ och installerar den senaste LTS-versionen.</span><span class="sxs-lookup"><span data-stu-id="bb725-109">If Node.js isn't installed on your PC, go to https://nodejs.org/ and install the latest LTS version.</span></span> <span data-ttu-id="bb725-110">Du kan fastställa om Node.js har installerats genom att öppna en kommandotolk eller ett terminalfönster och skriva följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bb725-110">You can determine whether Node.js is installed by opening a Command Prompt or terminal window and typing the following command:</span></span>

    ```
    node --version
    ```

    <span data-ttu-id="bb725-111">Om Node finns installerat visas versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="bb725-111">If Node is installed, the version number will be displayed.</span></span>

1. <span data-ttu-id="bb725-112">Om Visual Studio Code inte finns installerat på din dator går du till https://code.visualstudio.com/ och installerar den nu.</span><span class="sxs-lookup"><span data-stu-id="bb725-112">If Visual Studio Code isn't installed on your PC, go to https://code.visualstudio.com/ and install it now.</span></span>

1. <span data-ttu-id="bb725-113">Skapa en mapp med namnet ”Factbot” på valfri plats på hårddisken för att rymma robotens källkod.</span><span class="sxs-lookup"><span data-stu-id="bb725-113">Create a folder named "Factbot" in the location of your choice on your hard disk to hold the bot's source code.</span></span>

1. <span data-ttu-id="bb725-114">Gå tillbaka till Azure Portal och öppna resursgruppen ”data-science-rg”.</span><span class="sxs-lookup"><span data-stu-id="bb725-114">Return to the Azure portal and open the "factbot-rg" resource group.</span></span> <span data-ttu-id="bb725-115">Klicka sedan på Web App-roboten som du skapade i [övning 1](#Exercise1).</span><span class="sxs-lookup"><span data-stu-id="bb725-115">Then, click the web app bot you created in [Exercise 1](#Exercise1).</span></span>

    ![Öppna Azure Web App-roboten](../images/open-web-app-bot.png)

    <span data-ttu-id="bb725-117">_Öppna Azure Web App-roboten_</span><span class="sxs-lookup"><span data-stu-id="bb725-117">_Opening the web app bot_</span></span>

1. <span data-ttu-id="bb725-118">Klicka på **Skapa** i menyn till vänster och klicka sedan på **Hämta som ZIP-fil** för att förbereda en ZIP-fil som innehåller källkoden till roboten.</span><span class="sxs-lookup"><span data-stu-id="bb725-118">Click **Build** in the menu on the left, and then click **Download zip file** to prepare a zip file containing the bot's source code.</span></span> <span data-ttu-id="bb725-119">När ZIP-filen är klar klickar du på knappen **Hämta som ZIP-fil** för att ladda ned den.</span><span class="sxs-lookup"><span data-stu-id="bb725-119">Once the zip file is prepared, click the **Download zip file** button to download it.</span></span> <span data-ttu-id="bb725-120">När hämtningen är slutförd kan du kopiera innehållet i ZIP-filen till mappen ”Factbot” som du skapade i steg 4.</span><span class="sxs-lookup"><span data-stu-id="bb725-120">When the download is complete, copy the contents of the zip file to the "Factbot" folder that you created in Step 4.</span></span>

    ![Ladda ned källkoden](../images/download-source.png)

    <span data-ttu-id="bb725-122">_Ladda ned källkoden_</span><span class="sxs-lookup"><span data-stu-id="bb725-122">_Downloading the source code_</span></span>
  
1. <span data-ttu-id="bb725-123">Stanna kvar på bladet Skapa och klicka på **Konfigurera kontinuerlig distribution**.</span><span class="sxs-lookup"><span data-stu-id="bb725-123">Still on the "Build" blade, click **Configure continuous deployment**.</span></span> <span data-ttu-id="bb725-124">Klicka på **Konfiguration** högst upp på bladet och sedan på **Välj källa**.</span><span class="sxs-lookup"><span data-stu-id="bb725-124">Click **Setup** at the top of the ensuing blade, followed by **Choose Source**.</span></span> <span data-ttu-id="bb725-125">Välj **Lokal Git-lagringsplats** som distributionskälla och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb725-125">Then, select **Local Git Repository** as the deployment source and click **OK**.</span></span> 
 
    ![Ange en lokal Git-lagringsplats som distributionskälla](../images/portal-set-local-git.png)

    <span data-ttu-id="bb725-127">_Ange en lokal Git-lagringsplats som distributionskälla_</span><span class="sxs-lookup"><span data-stu-id="bb725-127">_Specifying a local Git repository as the deployment source_</span></span>  

1. <span data-ttu-id="bb725-128">Stäng bladet Distributioner och klicka på **All App service settings** (Alla apptjänstinställningar) i menyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="bb725-128">Close the "Deployments" blade and click **All App service settings** in the menu on the left.</span></span>

1. <span data-ttu-id="bb725-129">Klicka på **Autentiseringsuppgifter för distribution** och ange ett användarnamn och ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="bb725-129">Click **Deployment credentials**, and then enter a user name and password.</span></span> <span data-ttu-id="bb725-130">Du kommer förmodligen att behöva ange ett annat användarnamn än ”FactbotAdministrator” eftersom namnet måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="bb725-130">You will probably have to enter a user name other than "FactbotAdministrator" because the name must be unique within Azure.</span></span> <span data-ttu-id="bb725-131">Klicka på **Spara** och stäng bladet.</span><span class="sxs-lookup"><span data-stu-id="bb725-131">Then, click **Save** and close the blade.</span></span>

    ![Ange autentiseringsuppgifter för distribution](../images/portal-enter-ci-creds.png)

    <span data-ttu-id="bb725-133">_Ange autentiseringsuppgifter för distribution_</span><span class="sxs-lookup"><span data-stu-id="bb725-133">_Entering deployment credentials_</span></span>  

1. <span data-ttu-id="bb725-134">Starta Visual Studio Code och använd kommandot **File** > **Open Folder...** (Arkiv > Öppna mapp ...) för att öppna mappen ”Factbot” som du kopierade robotens källkoden till i steg 6.</span><span class="sxs-lookup"><span data-stu-id="bb725-134">Start Visual Studio Code and use the **File** > **Open Folder...** command to open the "Factbot" folder that you copied the bot's source code to in Step 6.</span></span>

1. <span data-ttu-id="bb725-135">Klicka på knappen **Source Control** (Källkontroll) i aktivitetsfältet på vänster sida av Visual Studio Code och klicka sedan på ikonen **Initialize Repository** (Initiera lagringsplatsen) längst upp.</span><span class="sxs-lookup"><span data-stu-id="bb725-135">Click the **Source Control** button in the activity bar on the left side of Visual Studio Code, and click the **Initialize Repository** icon at the top.</span></span> <span data-ttu-id="bb725-136">Klicka sedan på knappen **Intialize Repository** (Initiera lagringsplatsen) i följande dialogruta.</span><span class="sxs-lookup"><span data-stu-id="bb725-136">Then, click the **Initialize Repository** button in the ensuing dialog.</span></span>

    ![Initiera en lokal Git-lagringsplats](../images/vs-init-git-repo.png)

    <span data-ttu-id="bb725-138">_Initiera en lokal Git-lagringsplats_</span><span class="sxs-lookup"><span data-stu-id="bb725-138">_Initializing a local Git repository_</span></span>  

1. <span data-ttu-id="bb725-139">Skriv ”First commit.”</span><span class="sxs-lookup"><span data-stu-id="bb725-139">Type "First commit."</span></span> <span data-ttu-id="bb725-140">i meddelanderutan och klicka på bockmarkeringen för att bekräfta ändringarna.</span><span class="sxs-lookup"><span data-stu-id="bb725-140">into the message box, and then click the check mark to commit your changes.</span></span>

    ![Checka in ändringar på den lokala lagringsplatsen](../images/vs-first-git-commit.png)

    <span data-ttu-id="bb725-142">_Checka in ändringar på den lokala lagringsplatsen_</span><span class="sxs-lookup"><span data-stu-id="bb725-142">_Committing changes to the local repository_</span></span>  

1. <span data-ttu-id="bb725-143">Välj **Integrated Terminal** (Integrerad terminal) i menyn **View** (Visa) i Visual Studio Code för att öppna den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="bb725-143">Select **Integrated Terminal** from Visual Studio Code's **View** menu to open an integrated terminal.</span></span> <span data-ttu-id="bb725-144">Kör följande kommando i den integrerade terminalen och ersätt BOT_NAME på två platser med robotnamnet som du angav i övning 1, steg 3.</span><span class="sxs-lookup"><span data-stu-id="bb725-144">Then, execute the following command in the integrated terminal, replacing BOT_NAME in two places with the bot name you entered in Exercise 1, Step 3.</span></span>

    ```
    git remote add qna-factbot https://BOT_NAME.scm.azurewebsites.net:443/BOT_NAME.git
    ```

1. <span data-ttu-id="bb725-145">Klicka på ellipsen (tre punkter) längst upp i panelen SOURCE CONTROL (KÄLLKONTROLL) och välj **Publish Branch** (Publicera gren) i menyn för att skicka robotkoden från den lokala lagringsplatsen till Azure.</span><span class="sxs-lookup"><span data-stu-id="bb725-145">Click the ellipsis (the three dots) at the top of the SOURCE CONTROL panel and select **Publish Branch** from the menu to push the bot code from the local repository to Azure.</span></span> <span data-ttu-id="bb725-146">Om du ombeds uppge autentiseringsuppgifter ska du uppge det användarnamn och det lösenord som du angav i steg 9 i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="bb725-146">If prompted for credentials, enter the user name and password you specified in Step 9 of this exercise.</span></span>

<span data-ttu-id="bb725-147">Din robot har nu publicerats till Azure.</span><span class="sxs-lookup"><span data-stu-id="bb725-147">Your bot has been published to Azure.</span></span> <span data-ttu-id="bb725-148">Men innan du testar den ska vi köra den lokalt. Du ska också få lära dig att felsöka roboten i Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bb725-148">But before you test it there, let's run it locally and learn how to debug it in Visual Studio Code.</span></span>