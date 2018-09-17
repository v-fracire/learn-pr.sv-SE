<span data-ttu-id="ff2ea-101">I den här övningen överför du ditt ASP.NET Core-program till en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-101">In this exercise, you'll upload your ASP.NET Core application to an Azure Web App.</span></span>

## <a name="create-a-staging-deployment-slot"></a><span data-ttu-id="ff2ea-102">Skapa ett distributionsfack för mellanlagring</span><span class="sxs-lookup"><span data-stu-id="ff2ea-102">Create a staging deployment slot</span></span>

1. <span data-ttu-id="ff2ea-103">Öppna den webbapp-resurs som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-103">Open the Web App resource you created previously.</span></span> <span data-ttu-id="ff2ea-104">Om du har stängt resursbladet, kan du hitta det igen genom att söka efter appen i **Alla resurser** eller den innehållande resursgruppen i **Resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-104">If you have closed its resource blade, you can find it again by searching for the app in **All resources** or the containing resource group in **Resource groups**.</span></span>

1. <span data-ttu-id="ff2ea-105">Klicka på menykommandot **Distributionsfack** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-105">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="ff2ea-106">I bladet **Distributionsfack**, klickar du på knappen**Lägg till fack** i det övre navigationsfältet på bladet.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-106">Inside the **Deployment slots** blade, click the **Add Slot** button on the top navigation bar of the deployment slots blade.</span></span>

1. <span data-ttu-id="ff2ea-107">Azure-portalen öppnar bladet **Lägg till ett fack** som det visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-107">The Azure portal opens the **Add a slot** blade as shown below.</span></span>

    1. <span data-ttu-id="ff2ea-108">Namnge ditt distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-108">Give your deployment slot a name.</span></span> <span data-ttu-id="ff2ea-109">I det här fallet använder du `staging`.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-109">In this case, use `staging`.</span></span>

    2. <span data-ttu-id="ff2ea-110">När du väljer **Konfigurationskälla** har du två alternativ.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-110">To choose a **Configuration Source**, you have two options.</span></span>

        * <span data-ttu-id="ff2ea-111">Du kan välja att klona konfigurationselementen från ett befintligt distributionsfack eller webbapp som skapats i Azure.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-111">You can choose to clone the configuration elements from any existing deployment slot or Web App created on Azure.</span></span>
        * <span data-ttu-id="ff2ea-112">Eller så kan du välja att inte klona några konfigurationselement.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-112">Or you can choose not to clone any configuration elements.</span></span> <span data-ttu-id="ff2ea-113">Välj alternativet **Klona inte konfigurationen från ett befintligt fack**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-113">Select the option **Don't clone configuration from an existing slot**.</span></span>

        <span data-ttu-id="ff2ea-114">För det här distributionsfacket, väljer du det andra alternativet **Klona inte konfigurationen från ett befintligt fack**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-114">For this deployment slot, choose the second option: **Don't clone configuration from an existing slot**.</span></span> <span data-ttu-id="ff2ea-115">Du kommer att konfigurera det direkt.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-115">You will configure it directly.</span></span>

    ![Skärmbild av Azure-portalen som visar konfigurationen för ett nytt distributionsfack för mellanlagring.](../media/7-new-deployment-slot-blade.png)

1. <span data-ttu-id="ff2ea-117">Klicka på **OK** längst ned på bladet för att skapa ditt nya distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-117">Click the **OK** button at the bottom of the blade to create your new deployment slot.</span></span>

1. <span data-ttu-id="ff2ea-118">När distributionsfacket har skapats navigerar Azure-portalen tillbaka till bladet **Distributionsfack** för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-118">Once the deployment slot is successfully created, the Azure portal navigates you back to the **Deployment slots** blade of your web app.</span></span>

    <span data-ttu-id="ff2ea-119">Du kan nu se det nya distributionsfacket som du just skapade.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-119">Now, you can see the new deployment slot that you have just created.</span></span>

    ![Skärmbild av Azure-portalen som visar bladet Distributionsfack med det nya facket som har skapats.](../media/7-deployment-slot-created.png)

1. <span data-ttu-id="ff2ea-121">Välj det nya distributionsfacket.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-121">Select the new deployment slot.</span></span>

1. <span data-ttu-id="ff2ea-122">Azure-portalen navigerar till **Översikt**-sidan för det nyligen skapade distributionsfacket.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-122">The Azure portal navigates to the **Overview** page of the newly created deployment slot.</span></span>

    ![Distributionsfack för mellanlagring](../media/7-deployment-slot-staging.png)

    <span data-ttu-id="ff2ea-124">Observera **URL** för distributionsfacket för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-124">Notice the **URL** of the staging deployment slot.</span></span> <span data-ttu-id="ff2ea-125">Det är en annan URL från den du såg tidigare, med facknamnet sist.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-125">It is a different URL from what you saw previously, with the slot name appended.</span></span>

    <span data-ttu-id="ff2ea-126">Ett distributionsfack behandlas som en fullvärdig webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-126">A deployment slot is treated as a full Web App inside Azure.</span></span> <span data-ttu-id="ff2ea-127">Det är dock en särskild typ som är underordnad den ursprungliga webbappen och kan växlas med den ursprungliga webbappen.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-127">However, it is a special type that is a child of the original web app and can be swapped with the original web app.</span></span>

    <span data-ttu-id="ff2ea-128">Om du klickar på **webbadressen** visas samma standardsida som Azure skapade för webbappen första gången vi skapade den i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-128">If you click the **URL**, you will see the same default page that Azure created for the web app the first time we created it in the Azure portal.</span></span>

<span data-ttu-id="ff2ea-129">Nu när distributionsfacket för mellanlagring har skapats måste du konfigurera **autentiseringsuppgifter för distributionen**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-129">Now that the staging deployment slot is created successfully, you need to configure **deployment credentials**.</span></span>

## <a name="create-deployment-credentials"></a><span data-ttu-id="ff2ea-130">Skapa autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="ff2ea-130">Create deployment credentials</span></span>

<span data-ttu-id="ff2ea-131">I Azure måste du ställa in autentiseringsuppgifter för distribution innan du kan börja den faktiska distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-131">Azure requires deployment credentials to be set up before you can start the actual deployment process.</span></span> <span data-ttu-id="ff2ea-132">Därför kommer du att lära dig hur du skapar dina egna autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-132">For that reason, you will learn how to create your own deployment credentials.</span></span>

1. <span data-ttu-id="ff2ea-133">Klicka på menykommandot **Autentiseringsuppgifter för distribution** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-133">Click the **Deployment credentials** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="ff2ea-134">Azure-portalen navigerar till bladet **Autentiseringsuppgifter för distribution** som det visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-134">The Azure portal navigates to the **Deployment credentials** blade as shown below.</span></span>

    <span data-ttu-id="ff2ea-135">Ange ett **användarnamn** och ett **lösenord** och bekräfta sedan lösenordet igen.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-135">Enter a **username** and **password** of your choice, and then confirm your password once again.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ff2ea-136">Se till att skriva ned användarnamnet och lösenordet så att du inte glömmer dem!</span><span class="sxs-lookup"><span data-stu-id="ff2ea-136">Make sure you write down your username and password so that you don't forget them!</span></span> <span data-ttu-id="ff2ea-137">Du behöver dem senare när vi börjar ladda upp och distribuera koden till Azure.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-137">You will need them later when we start uploading and deploying our code to Azure.</span></span>

    ![Skärmbild av Azure-portalen som visar bladet Autentiseringsuppgifter för distribution på mellanlagringsfacket med exempel på autentiseringsuppgifter i de obligatoriska fälten.](../media/7-deployment-credentials.png)

1. <span data-ttu-id="ff2ea-139">Klicka på **Spara** längst upp på bladet **Autentiseringsuppgifter för distribution**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-139">Click the **Save** button at the top of the **Deployment credentials** blade.</span></span>

<span data-ttu-id="ff2ea-140">Nu när autentiseringsuppgifterna för distribution har skapats behöver du konfigurera övriga distributionsalternativ.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-140">Now that the deployment credentials are created successfully, you need to configure other deployment options.</span></span>

## <a name="use-a-local-git-repository-as-your-deployment-option"></a><span data-ttu-id="ff2ea-141">Använd en lokal Git-lagringsplats som distributionsalternativ</span><span class="sxs-lookup"><span data-stu-id="ff2ea-141">Use a local Git repository as your deployment option</span></span>

<span data-ttu-id="ff2ea-142">Därefter skapar vi en lokal Git-lagringsplats i Azure så att du kan börja ladda upp din kod.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-142">Next, we'll create a local Git repository in Azure so you can start uploading your code.</span></span>

1. <span data-ttu-id="ff2ea-143">Inom webbappen distributionsfack för **mellanlagring**, klickar du på menyalternativet **Distributionsalternativ** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-143">Within the **staging** deployment slot Web App, click the **Deployment options** menu item on the left-hand navigation.</span></span>

1. <span data-ttu-id="ff2ea-144">Azure-portalen navigerar till bladet **Distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-144">The Azure portal navigates to the **Deployment options** blade.</span></span>

1. <span data-ttu-id="ff2ea-145">Klicka på **Välj källa** för att konfigurera de nödvändiga inställningarna.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-145">Click on the **Choose Source** to configure the required settings.</span></span>

1. <span data-ttu-id="ff2ea-146">Azure-portalen visar de tillgängliga alternativen som du kan konfigurera och använda.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-146">The Azure portal displays the available options that you can configure and use.</span></span> <span data-ttu-id="ff2ea-147">I vårt fall väljer vi alternativet **Lokal Git-lagringsplats**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-147">In our case, choose the **Local Git Repository** option.</span></span>

1. <span data-ttu-id="ff2ea-148">Du kommer tillbaka till bladet **Lägg till distributionslösning**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-148">You will be returned to the **Deployment option** blade.</span></span> <span data-ttu-id="ff2ea-149">Klicka på **OK** längst ned på bladet för att konfigurera distributionskällan.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-149">Click the **OK** button at the bottom of the blade to set up the deployment source.</span></span>

1. <span data-ttu-id="ff2ea-150">Navigera nu till avsnittet **Distributionscenter (förhandsversion)** till vänster för att visa den nya distributionsinformationen.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-150">Now, navigate to the **Deployment Center (Preview)** section on the left-side navigation to view the new deployment details.</span></span>

    ![Skärmbild av Azure-portalen som visar distributionsfackets Distributionscenter-blad med URI för Git Clone för det markerade facket.](../media/7-staging-after-setting-git.png)

    <span data-ttu-id="ff2ea-152">Den viktiga informationen att notera här är **URI för Git Clone** som är den lokala Git-lagringsplatsens URL som du kommer att använda som en **fjärranslutning** för din lokala webbapplagringsplats.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-152">The important information to note here is the **Git Clone Uri**, which is the local Git repository URL that you will use as a **remote** for your local web application repository.</span></span>

<span data-ttu-id="ff2ea-153">Det är dags att börja ladda upp din kod till ett distributionsfack för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-153">It is time to start uploading your code to the staging deployment slot.</span></span>

## <a name="install-git-on-your-machine"></a><span data-ttu-id="ff2ea-154">Installera Git på datorn</span><span class="sxs-lookup"><span data-stu-id="ff2ea-154">Install Git on your machine</span></span>

<span data-ttu-id="ff2ea-155">Om du inte redan har gjort det, installerar du Git på din Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-155">If you don't already have it, install Git on your Linux machine.</span></span>

> [!NOTE]
> <span data-ttu-id="ff2ea-156">De här instruktionerna är för Ubuntu 18.04 så stegen för att installera Git kan skilja sig för din distribution och version.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-156">These instructions are for Ubuntu 18.04, so the steps to install Git may differ for your distribution and version.</span></span> <span data-ttu-id="ff2ea-157">Ta en titt på [Linux Git Installationsinstruktioner](https://git-scm.com/download/linux) för de steg som krävs.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-157">Check out the [Linux Git installation instructions](https://git-scm.com/download/linux) for the appropriate steps.</span></span>

1. <span data-ttu-id="ff2ea-158">Öppna ett nytt **terminalfönster**</span><span class="sxs-lookup"><span data-stu-id="ff2ea-158">Open a new **Terminal** window</span></span>

1. <span data-ttu-id="ff2ea-159">Ange följande kommando.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-159">Type the following command.</span></span> <span data-ttu-id="ff2ea-160">Du uppmanas att ange ditt Ubuntu-användarlösenord.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-160">You will be prompted to enter your Ubuntu user password.</span></span>

    ```console
    sudo apt-get update
    ```

1. <span data-ttu-id="ff2ea-161">När uppdateringen är färdig anger du följande kommando för att installera Git lokalt.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-161">Once the update is successful, type the following command to install Git locally.</span></span> <span data-ttu-id="ff2ea-162">Du uppmanas att godkänna installationen av Git på din dator.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-162">You will be prompted to accept the installation of Git on your machine.</span></span>

    ```console
    sudo apt-get install git-core
    ```

1. <span data-ttu-id="ff2ea-163">För att kontrollera att Git är installerat anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-163">To verify that Git is now installed, type the following command:</span></span>

    ```console
    git --version
    ```

   <span data-ttu-id="ff2ea-164">Om installationen lyckas visas följande utdata:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-164">If the installation is successful, you will see the following output:</span></span>

    ```console
    git version 2.17.1
    ```

1. <span data-ttu-id="ff2ea-165">Det är alltid en bra idé att konfigurera Git-inställningar genom att ange ditt namn och din e-post.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-165">It is always a good practice to configure Git settings by providing your name and email.</span></span> <span data-ttu-id="ff2ea-166">För att göra det så behöver du utfärda följande kommandon, där du ersätter platshållarna `{your name}` och `{your email}` med ditt eget namn och e-postadress (utan `{}` klammerparanteserna):</span><span class="sxs-lookup"><span data-stu-id="ff2ea-166">For that, you need to issue the following commands, replacing the `{your name}` and `{your email}` placeholders with your own name and email (without the `{}` curly braces):</span></span>

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. <span data-ttu-id="ff2ea-167">För att verifiera att din information har registrerats av Git anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-167">To verify that your information has been recorded by Git, type the following command:</span></span>

    ```console
    cat ~/.gitconfig
    ```

   <span data-ttu-id="ff2ea-168">Du bör se följande utdata med ditt namn och din e-postadress:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-168">You should be seeing the following, with your name and email shown:</span></span>

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a><span data-ttu-id="ff2ea-169">Initiera en lokal Git-lagringsplats för webbappen</span><span class="sxs-lookup"><span data-stu-id="ff2ea-169">Initialize a local Git repository for your web app</span></span>

<span data-ttu-id="ff2ea-170">När du ska börja använda Git behöver du initiera en lokal Git-lagringsplats för webbappen.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-170">To start using Git, you need to initialize a local Git repository for the web app.</span></span>

1. <span data-ttu-id="ff2ea-171">Öppna ett nytt **terminalfönster**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-171">Open a new **Terminal** window.</span></span>

1. <span data-ttu-id="ff2ea-172">Navigera till innehållsrotmappen för den webbapp som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-172">Navigate to the content root folder of the web app you created previously.</span></span> <span data-ttu-id="ff2ea-173">Du kan skriva följande:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-173">You can type the following:</span></span>

    ```console
    cd ~/Documents/BestBikeApp/
    ```

1. <span data-ttu-id="ff2ea-174">Initiera en ny Git-lagringsplats genom att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-174">Initialize a new Git repository by issuing the following command:</span></span>

    ```console
    git init
    ```

    <span data-ttu-id="ff2ea-175">Om kommandot lyckas visas ett meddelande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-175">If the command is successful, you receive a message like the following:</span></span>

    ```console
    Initialized empty Git repository in /home/{your-user}/Documents/BestBikeApp/.git/
    ```

1. <span data-ttu-id="ff2ea-176">Mellanlagra alla webbappsfiler i Git.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-176">Stage all the web app files to Git.</span></span>

   <span data-ttu-id="ff2ea-177">Nästa steg är att informera Git om dina webbappsfiler.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-177">The next step is to let Git know about your web app files.</span></span> <span data-ttu-id="ff2ea-178">Det gör du genom att lägga till alla filer i arbetskatalogen så att de **mellanlagras** av Git.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-178">Do that by adding all the files of the working directory so that they get **staged** by Git.</span></span> <span data-ttu-id="ff2ea-179">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-179">Type the following command:</span></span>

    ```console
    git add .
    ```

    <span data-ttu-id="ff2ea-180">När du anger ”.” i kommandot ovan läggs alla filer till för mellanlagring i Git.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-180">The command above adds all files, represented by the ".", to the staging state of Git.</span></span>

1. <span data-ttu-id="ff2ea-181">Nu behöver du checka in ändringarna i Git.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-181">Now, you need to commit your changes to Git.</span></span>

   <span data-ttu-id="ff2ea-182">När du mellanlagrar filerna med Git behöver du checka in filerna i den lokala **Git-incheckningshistoriken**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-182">Once you stage the files with Git, you need to commit your files to the **Git commit history** on your local machine.</span></span> <span data-ttu-id="ff2ea-183">Det gör du med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-183">You do that by typing the following command:</span></span>

    ```console
   git commit -m "Initial create"
    ```

   <span data-ttu-id="ff2ea-184">Kommandot `commit` kan ta argumentet `-m` med ett meddelande om incheckningen du skapar.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-184">The `commit` command accepts  `-m` argument to include a message with the commit you are creating.</span></span> <span data-ttu-id="ff2ea-185">När du senare överför koden till Azure kan du se samma meddelande för just den här incheckningen.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-185">Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.</span></span>

## <a name="add-a-remote-for-the-local-git-repository"></a><span data-ttu-id="ff2ea-186">Lägga till en fjärranslutning till den lokala Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="ff2ea-186">Add a remote for the local Git repository</span></span>

<span data-ttu-id="ff2ea-187">Hittills har du initierat en ny lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-187">At this point, you have successfully initialized a new local Git repository.</span></span> <span data-ttu-id="ff2ea-188">Du har dessutom checkat in alla dina webbappsfiler till Git.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-188">In addition, you've committed all of your web app files to Git.</span></span> <span data-ttu-id="ff2ea-189">Det som återstår är att lägga till en **fjärranslutning** som ansluter din lokala Git-lagringsplats till den som hanteras i Azure.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-189">What remains is to add a **remote** to connect your local Git repository to that hosted on Azure.</span></span>

<span data-ttu-id="ff2ea-190">För att göra det behöver du:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-190">To do so, you need to:</span></span>

1. <span data-ttu-id="ff2ea-191">Kopiera den **Git-klon-URL** som du såg ovan.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-191">Copy the **Git clone url** that you saw above.</span></span>

1. <span data-ttu-id="ff2ea-192">När den har kopierats går du tillbaka till **terminalfönstret** och anger följande Git-kommando:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-192">Once copied, you go back to the **Terminal** window and issue the following Git command:</span></span>

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    <span data-ttu-id="ff2ea-193">Git-kommandot ovan kopplar din lokala Git-lagringsplats till den som hanteras i Azure.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-193">The above Git command hooks your local Git repository to the one hosted on Azure.</span></span> <span data-ttu-id="ff2ea-194">Nu kan du börja skicka och hämta mellan den lokala och den fjärranslutna Git-lagringsplatsen!</span><span class="sxs-lookup"><span data-stu-id="ff2ea-194">Now, you can start pushing and pulling between the local and remote Git repositories!</span></span>

1. <span data-ttu-id="ff2ea-195">Verifiera ovanstående kommando genom att ange följande Git-kommando:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-195">To verify the above command, type the following Git command:</span></span>

    ```console
    git remote -v
    ```

    <span data-ttu-id="ff2ea-196">Ovanstående kommando genererar följande utdata:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-196">The command above generates the following output:</span></span>

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a><span data-ttu-id="ff2ea-197">Skicka koden till Azure</span><span class="sxs-lookup"><span data-stu-id="ff2ea-197">Push your code to Azure</span></span>

<span data-ttu-id="ff2ea-198">Nu när du har kopplat din lokala Git-lagringsplats till den fjärranslutna Git-lagringsplatsen i Azure kommer du att skriva och kompilera appen och sedan skicka den till Azure.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-198">Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app, and then push your web app to Azure.</span></span>

1. <span data-ttu-id="ff2ea-199">Skriv följande Git-kommando för att skicka din **huvudgren** till den fjärranslutna Git-lagringsplatsen i Azure:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-199">Type the following Git command to push your **master** branch to the remote Git repository on Azure:</span></span>

    ```console
    git push origin master
    ```

1. <span data-ttu-id="ff2ea-200">Du får ange lösenordet du konfigurerade i avsnittet **Autentiseringsuppgifter för distribution** ovan.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-200">You will be prompted to enter the password that you have configured in the **Deployment credentials** section above.</span></span> <span data-ttu-id="ff2ea-201">Ange lösenordet och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-201">Enter your password and hit Enter.</span></span> <span data-ttu-id="ff2ea-202">Git börjar överföra dina incheckade filer till den fjärranslutna Git-lagringsplatsen i Azure som konfigurerats under distributionsfacket för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-202">Git starts uploading your committed files to the Azure remote Git repository configured under the staging deployment slot.</span></span>

## <a name="verify-the-web-app-is-uploaded-to-azure"></a><span data-ttu-id="ff2ea-203">Verifiera att webbappen har överförts till Azure</span><span class="sxs-lookup"><span data-stu-id="ff2ea-203">Verify the web app is uploaded to Azure</span></span>

1. <span data-ttu-id="ff2ea-204">Logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-204">Sign in to the Azure portal.</span></span>

1. <span data-ttu-id="ff2ea-205">Klicka på menykommandot **Alla resurser** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-205">Click on the **All Resources** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="ff2ea-206">Azure-portalen navigerar till listan över alla resurser som skapats på Azure hittills.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-206">The Azure portal navigates you to the list of all resources created on Azure so far.</span></span>

1. <span data-ttu-id="ff2ea-207">Klicka på det mellanlagringsfack som skapades ovan.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-207">Click on the staging slot created above.</span></span> <span data-ttu-id="ff2ea-208">Kom ihåg att ett distributionsfack betraktas som en webbapp och därför visas som en webbappresurs under **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-208">Remember, a deployment slot is considered a web app, and hence, it will appear as a web app resource under **All Resources**.</span></span>

1. <span data-ttu-id="ff2ea-209">När du kommer till bladet distributionsfack för mellanlagring går du till **Distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-209">Once you arrive to the staging deployment slot blade, go to **Deployment options**.</span></span>

    <span data-ttu-id="ff2ea-210">Du kommer att se att din första incheckning som du har lokalt på datorn nu har laddats upp till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-210">You will see that your first commit that you have locally on your machine is now uploaded to the Azure portal.</span></span>

    <span data-ttu-id="ff2ea-211">Genom att skicka koden lokalt till den fjärranslutna Git-lagringsplatsen som hanteras i Azure har Azure registrerat den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-211">By pushing your code locally to the remote Git repository hosted on Azure, Azure has recorded this operation.</span></span>

    <span data-ttu-id="ff2ea-212">Varje gång du skickar koden till Azure så visas en ny post tillsammans med det meddelande som du anger när du checkar in ändringarna lokalt på din dator.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-212">Every time you push your code to Azure, you will see a new record, together with the message that you type when committing your changes locally on your machine.</span></span>

    ![Skärmbild av Azure-portalen som visar en nyligen genomförd Git-lagringsplatsdistribution på bladet Utvecklingsalternativ.](../media/7-staging-deployment-slot-after-uploading-files.png)

1. <span data-ttu-id="ff2ea-214">Vi går till URL:en för **mellanlagringsfacket**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-214">Let's visit the **staging slot** URL.</span></span> <span data-ttu-id="ff2ea-215">URL:en nämndes ovan, men om du glömmer den så kan du alltid gå till sidan **Översikt** i distributionsfacket för mellanlagring och hämta URL:en.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-215">The URL was mentioned above, however, if you forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.</span></span>

1. <span data-ttu-id="ff2ea-216">Ange följande URL i webbläsarens adressfält: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="ff2ea-216">Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span></span>

    ![Skärmbild som visar en webbläsarvy för webbplatsen för distributionsfack för mellanlagring.](../media/7-staging-slot-hosted-online.png)

<span data-ttu-id="ff2ea-218">Du har överfört dina lokala webbappfiler till distributionsfacket för mellanlagring på Azure.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-218">You have successfully uploaded your local web app files to the staging deployment slot on Azure.</span></span>

## <a name="swapping-the-staging-and-production-deployment-slots"></a><span data-ttu-id="ff2ea-219">Växla mellan distributionsfack för mellanlagring och produktion</span><span class="sxs-lookup"><span data-stu-id="ff2ea-219">Swapping the staging and production deployment slots</span></span>

<span data-ttu-id="ff2ea-220">Nu när programmet är igång och körs i distributionsfacket för mellanlagring som hanteras i Azure är det dags att växla till produktionsfacket.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-220">Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one.</span></span> <span data-ttu-id="ff2ea-221">För att göra det följer du dessa steg:</span><span class="sxs-lookup"><span data-stu-id="ff2ea-221">To do so, follow these steps:</span></span>

1. <span data-ttu-id="ff2ea-222">Gå till det ursprungliga webbappbladet som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-222">Navigate to the original Web App blade created early.</span></span> <span data-ttu-id="ff2ea-223">Du hittar den ursprungliga Webbappen från bladet **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-223">You can find the original Web App from the **All resources** blade.</span></span>

1. <span data-ttu-id="ff2ea-224">Klicka på menykommandot **Distributionsfack** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-224">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="ff2ea-225">Klicka på knappen **Växla** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-225">Click on the **Swap** button at the top of the blade.</span></span>

1. <span data-ttu-id="ff2ea-226">Azure-portalen tar dig till bladet **Växla**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-226">The Azure portal navigates you to the **Swap** blade.</span></span>

1. <span data-ttu-id="ff2ea-227">I fältet **Växla** väljer du **Växla**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-227">For the **Swap** field, select **Swap**.</span></span>

1. <span data-ttu-id="ff2ea-228">I fältet **Källa** väljer du **Mellanlagring**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-228">For the **Source** field, select **Staging**.</span></span>

1. <span data-ttu-id="ff2ea-229">I fältet **Mål** väljer du **Produktion**.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-229">For the **Destination** field, select **Production**.</span></span>

    ![Skärmbild av Azure-portalen som visar bladet distributionsfackväxling.](../media/7-swap-blade.png)

1. <span data-ttu-id="ff2ea-231">Klicka på knappen **OK** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-231">Click on the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="ff2ea-232">Azure startar växlingsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-232">Azure starts the swapping process.</span></span> <span data-ttu-id="ff2ea-233">Den här åtgärden tar vanligtvis några sekunder beroende på hur stor webbappen som växlas är.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-233">Usually, this operation takes a few seconds, depending on the size of the web app being swapped.</span></span>

1. <span data-ttu-id="ff2ea-234">När åtgärden är klar går du till webbappens URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="ff2ea-234">Once the operation ends, visit the web app URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span></span>

    ![Skärmbild som visar en webbläsarvy över det tidigare distributionsfacket för mellanlagring som nu finns som primär webbapp.](../media/7-web-app-page.png)

    <span data-ttu-id="ff2ea-236">Växlingsåtgärden är nu klar!</span><span class="sxs-lookup"><span data-stu-id="ff2ea-236">The swapping operation has been successful!</span></span> <span data-ttu-id="ff2ea-237">Du kan nu se att den kod som du överförde till distributionsfacket för mellanlagring även finns på produktionsfacket.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-237">You can now see the code that you uploaded to the staging deployment slot also being hosted on the production slot.</span></span>

1. <span data-ttu-id="ff2ea-238">Besök nu URL:en för mellanlagringsfacket: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="ff2ea-238">Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span></span>

    ![Skärmbild som visar en webbläsarvy över det tidigare primära distributionsfacket som nu finns som en webbapp med distributionsfack för mellanlagring.](../media/7-staging-after-swapping.png)

    <span data-ttu-id="ff2ea-240">Distributionsfacket för mellanlagring innehåller nu de ursprungliga standardwebbappfiler som tidigare hanterades under produktionsfacket.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-240">The staging deployment slot now contains the original, default web application files that were previously hosted under the production slot.</span></span>

<span data-ttu-id="ff2ea-241">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ff2ea-241">Congratulations!</span></span> <span data-ttu-id="ff2ea-242">Du har överfört din webbapp till Azure och växlat distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="ff2ea-242">You have successfully uploaded your web app to Azure and swapped deployment slots.</span></span>
