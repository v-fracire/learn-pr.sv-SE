### <a name="exercise-3-deploy-a-bot-with-visual-studio-code"></a><span data-ttu-id="ce1b5-101">Övning 3: Distribuera en robot med Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ce1b5-101">Exercise 3: Deploy a bot with Visual Studio Code</span></span>

<span data-ttu-id="ce1b5-102">När du skapade en Azure Web App-robot i [övning 1](#Exercise1) distribuerades även en Azure Web App som fungerar som värd.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-102">When you created an Azure Web App Bot in [Exercise 1](#Exercise1), an Azure Web App was deployed to host it.</span></span> <span data-ttu-id="ce1b5-103">Roboten kräver dock lite en del kodning och den måste även distribueras till Azures webbapp.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-103">But the bot does require some code, and it still needs to be deployed to the Azure Web app.</span></span> <span data-ttu-id="ce1b5-104">Som tur är kan har koden redan genererats åt dig av Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-104">Fortunately, the code was generated for you by the Azure Bot Service.</span></span> <span data-ttu-id="ce1b5-105">I den här övningen ska du använda Visual Studio Code för att placera koden på en lokal Git-lagringsplats och publicera roboten till Azure genom att skicka ändringarna från den lokala lagringsplatsen till en fjärrlagringsplats som är ansluten till den Azure Web App som är värd för roboten – en process som kallas [kontinuerlig intergrering](https://en.wikipedia.org/wiki/Continuous_integration).</span><span class="sxs-lookup"><span data-stu-id="ce1b5-105">In this exercise, you will use Visual Studio Code to place the code in a local Git repository and publish the bot to Azure by pushing changes from the local repository to a remote repository connected to the Azure Web App that hosts the bot — a process known as [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration).</span></span>

1. <span data-ttu-id="ce1b5-106">Om [Git](https://git-scm.com/) är inte installerat på din dator går du till https://git-scm.com/downloads och installerar Git-klienten för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-106">If [Git](https://git-scm.com/) isn't installed on your PC, go to https://git-scm.com/downloads and install the Git client for your operating system.</span></span> <span data-ttu-id="ce1b5-107">Git är ett kostnadsfritt och distribuerat versionskontrollsystem med öppen källkod som integreras sömlöst i Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-107">Git is a free and open-source distributed version-control system, and it integrates seamlessly into Visual Studio Code.</span></span> <span data-ttu-id="ce1b5-108">Om du inte vet om Git är installerat öppnar du en kommandotolk eller ett terminalfönster och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ce1b5-108">If you aren't sure whether Git is installed, open a Command Prompt or terminal window and execute the following command:</span></span>

    ``` 
    git --version
    ```

    <span data-ttu-id="ce1b5-109">Om ett versionsnummer visas är Git-klienten installerad.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-109">If a version number is displayed, then the Git client is installed.</span></span>

1. <span data-ttu-id="ce1b5-110">Om Node.js inte finns installerat på datorn går du till https://nodejs.org/ och installerar den senaste LTS-versionen.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-110">If Node.js isn't installed on your PC, go to https://nodejs.org/ and install the latest LTS version.</span></span> <span data-ttu-id="ce1b5-111">Du kan fastställa om noden har installerats genom att öppna en kommandotolk eller ett terminalfönster och skriva följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ce1b5-111">You can determine whether Node is installed by opening a Command Prompt or terminal window and typing the following command:</span></span>

    ```
    node --version
    ```

    <span data-ttu-id="ce1b5-112">Om Node finns installerat visas versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-112">If Node is installed, the version number will be displayed.</span></span>

1. <span data-ttu-id="ce1b5-113">Om Visual Studio Code inte finns installerat på din dator går du till https://code.visualstudio.com/ och installerar den nu.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-113">If Visual Studio Code isn't installed on your PC, go to https://code.visualstudio.com/ and install it now.</span></span>

1. <span data-ttu-id="ce1b5-114">Skapa en mapp med namnet ”Factbot” på valfri plats på hårddisken för att rymma robotens källkod.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-114">Create a folder named "Factbot" in the location of your choice on your hard disk to hold the bot's source code.</span></span>

1. <span data-ttu-id="ce1b5-115">Gå tillbaka till Azure-portalen och öppna resursgruppen ”data-science-rg”.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-115">Return to the Azure Portal and open the "factbot-rg" resource group.</span></span> <span data-ttu-id="ce1b5-116">Klicka sedan på Web App-roboten som du skapade i [övning 1](#Exercise1).</span><span class="sxs-lookup"><span data-stu-id="ce1b5-116">Then click the Web App Bot you created in [Exercise 1](#Exercise1).</span></span>

    ![Öppna Azure Web App-roboten](../images/open-web-app-bot.png)

    <span data-ttu-id="ce1b5-118">_Öppna Azure Web App-roboten_</span><span class="sxs-lookup"><span data-stu-id="ce1b5-118">_Opening the Web App Bot_</span></span>

1. <span data-ttu-id="ce1b5-119">Klicka på **Skapa** i menyn till vänster och klicka sedan på **Hämta som ZIP-fil** för att förbereda en ZIP-fil som innehåller källkoden till roboten.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-119">Click **Build** in the menu on the left, and then click **Download zip file** to prepare a zip file containing the bot's source code.</span></span> <span data-ttu-id="ce1b5-120">När ZIP-filen är klar klickar du på knappen **Hämta som ZIP-fil** för att ladda ned den.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-120">Once the zip file is prepared, click the **Download zip file** button to download it.</span></span> <span data-ttu-id="ce1b5-121">När hämtningen är slutförd kan du kopiera innehållet i ZIP-filen till mappen ”Factbot” som du skapade i steg 4.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-121">When the download is complete, copy the contents of the zip file to the "Factbot" folder that you created in Step 4.</span></span>

    ![Ladda ned källkoden](../images/download-source.png)

    <span data-ttu-id="ce1b5-123">_Ladda ned källkoden_</span><span class="sxs-lookup"><span data-stu-id="ce1b5-123">_Downloading the source code_</span></span>
  
1. <span data-ttu-id="ce1b5-124">Stanna kvar på bladet Skapa och klicka på **Konfigurera kontinuerlig distribution**.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-124">Still on the "Build" blade, click **Configure continuous deployment**.</span></span> <span data-ttu-id="ce1b5-125">Klicka på **Konfiguration** högst upp på bladet och sedan på **Välj källa**.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-125">Click **Setup** at the top of the ensuing blade, followed by **Choose Source**.</span></span> <span data-ttu-id="ce1b5-126">Välj **Lokal Git-lagringsplats** som distributionskälla och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-126">Then select **Local Git Repository** as the deployment source and click **OK**.</span></span> 
 
    ![Ange en lokal Git-lagringsplats som distributionskälla](../images/portal-set-local-git.png)

    <span data-ttu-id="ce1b5-128">_Ange en lokal Git-lagringsplats som distributionskälla_</span><span class="sxs-lookup"><span data-stu-id="ce1b5-128">_Specifying a local Git repository as the deployment source_</span></span>  

1. <span data-ttu-id="ce1b5-129">Stäng bladet Distributioner och klicka på **All App service settings** (Alla apptjänstinställningar) i menyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-129">Close the "Deployments" blade and click **All App service settings** in the menu on the left.</span></span>

1. <span data-ttu-id="ce1b5-130">Klicka på **Autentiseringsuppgifter för distribution** och ange ett användarnamn och ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-130">Click **Deployment credentials**, and then enter a user name and password.</span></span> <span data-ttu-id="ce1b5-131">Du kommer förmodligen att behöva ange ett annat användarnamn än ”FactbotAdministrator” eftersom namnet måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-131">You will probably have to enter a user name other than "FactbotAdministrator" because the name must be unique within Azure.</span></span> <span data-ttu-id="ce1b5-132">Klicka på **Spara** och stäng bladet.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-132">Then click **Save** and close the blade.</span></span>

    ![Ange autentiseringsuppgifter för distribution](../images/portal-enter-ci-creds.png)

    <span data-ttu-id="ce1b5-134">_Ange autentiseringsuppgifter för distribution_</span><span class="sxs-lookup"><span data-stu-id="ce1b5-134">_Entering deployment credentials_</span></span>  

1. <span data-ttu-id="ce1b5-135">Starta Visual Studio Code och använd kommandot **File** > **Open Folder...** (Arkiv > Öppna mapp ...) för att öppna mappen ”Factbot” som du kopierade robotens källkoden till i steg 6.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-135">Start Visual Studio Code and use the **File** > **Open Folder...** command to open the "Factbot" folder that you copied the bot's source code to in Step 6.</span></span>

1. <span data-ttu-id="ce1b5-136">Klicka på knappen **Source Control** (Källkontroll) i aktivitetsfältet på vänster sida av Visual Studio Code och klicka sedan på ikonen **Initialize Repository** (Initiera lagringsplatsen) längst upp.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-136">Click the **Source Control** button in the activity bar on the left side of Visual Studio Code, and click the **Initialize Repository** icon at the top.</span></span> <span data-ttu-id="ce1b5-137">Klicka sedan på knappen **Intialize Repository** (Initiera lagringsplatsen) i följande dialogruta.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-137">Then click the **Intialize Repository** button in the ensuing dialog.</span></span>

    ![Initiera en lokal Git-lagringsplats](../images/vs-init-git-repo.png)

    <span data-ttu-id="ce1b5-139">_Initiera en lokal Git-lagringsplats_</span><span class="sxs-lookup"><span data-stu-id="ce1b5-139">_Initializing a local Git repository_</span></span>  

1. <span data-ttu-id="ce1b5-140">Skriv ”First commit.”</span><span class="sxs-lookup"><span data-stu-id="ce1b5-140">Type "First commit."</span></span> <span data-ttu-id="ce1b5-141">i meddelanderutan och klicka på bockmarkeringen för att bekräfta ändringarna.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-141">into the message box, and then click the check mark to commit your changes.</span></span>

    ![Checka in ändringar på den lokala lagringsplatsen](../images/vs-first-git-commit.png)

    <span data-ttu-id="ce1b5-143">_Checka in ändringar på den lokala lagringsplatsen_</span><span class="sxs-lookup"><span data-stu-id="ce1b5-143">_Committing changes to the local repository_</span></span>  

1. <span data-ttu-id="ce1b5-144">Välj **Integrated Terminal** (Integrerad terminal) i menyn **View** (Visa) i Visual Studio Code för att öppna den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-144">Select **Integrated Terminal** from Visual Studio Code's **View** menu to open an integrated terminal.</span></span> <span data-ttu-id="ce1b5-145">Kör följande kommando i den integrerade terminalen och ersätt BOT_NAME på två platser med robotnamnet som du angav i övning 1, steg 3.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-145">Then execute the following command in the integrated terminal, replacing BOT_NAME in two places with the bot name you entered in Exercise 1, Step 3.</span></span>

    ```
    git remote add qna-factbot https://BOT_NAME.scm.azurewebsites.net:443/BOT_NAME.git
    ```

1. <span data-ttu-id="ce1b5-146">Klicka på ellipsen (tre punkter) längst upp i panelen SOURCE CONTROL (KÄLLKONTROLL) och välj **Publish Branch** (Publicera gren) i menyn för att skicka robotkoden från den lokala lagringsplatsen till Azure.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-146">Click the ellipsis (the three dots) at the top of the SOURCE CONTROL panel and select **Publish Branch** from the menu to push the bot code from the local repository to Azure.</span></span> <span data-ttu-id="ce1b5-147">Om du ombeds uppge autentiseringsuppgifter ska du uppge det användarnamn och det lösenord som du angav i steg 9 i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-147">If prompted for credentials, enter the user name and password you specified in Step 9 of this exercise.</span></span>

<span data-ttu-id="ce1b5-148">Din robot har nu publicerats till Azure.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-148">Your bot has been published to Azure.</span></span> <span data-ttu-id="ce1b5-149">Men innan du testar den ska vi köra den lokalt. Du ska också få lära dig att felsöka roboten i Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ce1b5-149">But before you test it there, let's run it locally and learn how to debug it in Visual Studio Code.</span></span>
