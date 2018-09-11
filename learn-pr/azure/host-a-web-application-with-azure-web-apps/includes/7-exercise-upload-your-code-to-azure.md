<span data-ttu-id="1d5ae-101">I den här övningen laddar du upp ditt ASP.NET Core-program till Azure.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-101">In this exercise, you'll upload your ASP.NET Core application to Azure.</span></span>

## <a name="create-a-staging-deployment-slot"></a><span data-ttu-id="1d5ae-102">Skapa ett distributionsfack för mellanlagring</span><span class="sxs-lookup"><span data-stu-id="1d5ae-102">Create a staging deployment slot</span></span>

1. <span data-ttu-id="1d5ae-103">Leta upp och klicka på menyalternativet **Distributionsfack** i navigeringsfönstret till vänster.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-103">Locate and click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="1d5ae-104">Azure öppnar bladet **Distributionsfack** enligt vad som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-104">Azure opens the **Deployment slots** blade as shown below.</span></span>

    ![Distributionsfack](../media-draft/7-deployment-slot-blade.png)

1. <span data-ttu-id="1d5ae-106">Leta upp och klicka på knappen **+ Lägg till plats** i det övre navigeringsfältet på bladet med distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-106">Locate and click the **+ Add Slot** button on the top navigation bar of the deployment slots blade.</span></span>

1. <span data-ttu-id="1d5ae-107">Azure-portalen öppnar bladet **Lägg till ett fack**, se nedan.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-107">The Azure portal opens the **Add a slot** blade as shown below.</span></span>

    ![Lägga till ett fack](../media-draft/7-new-deployment-slot-blade.png)

    <span data-ttu-id="1d5ae-109">Namnge ditt distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-109">Give your deployment slot a name.</span></span> <span data-ttu-id="1d5ae-110">I det här fallet använder du **Staging**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-110">In this case, use **Staging**.</span></span>

    <span data-ttu-id="1d5ae-111">I valet av **konfigurationskälla** har du två alternativ.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-111">To choose a **Configuration Source**, you have 2 options.</span></span>

    1. <span data-ttu-id="1d5ae-112">Välj om du vill klona konfigurationselementen från ett annat fack eller en webbapp som skapats i Azure.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-112">Select to clone the configuration elements from any other slot or web app created on Azure.</span></span>

    1. <span data-ttu-id="1d5ae-113">Du kan välja att inte klona några konfigurationselement.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-113">You can choose not to clone any configuration elements.</span></span> <span data-ttu-id="1d5ae-114">I så fall väljer du alternativet **Klona inte konfigurationen från ett befintligt fack**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-114">In this case, you select the option **Don't clone configuration from an existing slot**.</span></span>

    <span data-ttu-id="1d5ae-115">Välj det andra alternativet, **Klona inte konfigurationen från ett befintligt fack**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-115">Choose the second option, **Don't clone configuration from an existing slot**.</span></span>

1. <span data-ttu-id="1d5ae-116">Leta upp och klicka på knappen **OK** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-116">Locate and click the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="1d5ae-117">När distributionsfacket har skapats navigerar Azure-portalen tillbaka till bladet **Distributionsfack** för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-117">Once the deployment slot is successfully created, the Azure portal navigates you back to the **Deployment slots** blade of your web app.</span></span>

    <span data-ttu-id="1d5ae-118">Du kan nu se det nya distributionsfacket du just skapade.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-118">Now, you can see the new deployment slot that you have just created.</span></span>

    ![Distributionsfacket har skapats](../media-draft/7-deployment-slot-created.png)

1. <span data-ttu-id="1d5ae-120">Leta upp och klicka på det nya distributionsfacket.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-120">Locate and click the new deployment slot.</span></span>

1. <span data-ttu-id="1d5ae-121">Azure Portal navigerar till sidan **Översikt** för distributionsfacket du skapade.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-121">The Azure portal navigates to the **Overview** page of the newly created deployment slot.</span></span>

    ![Distributionsfack för mellanlagring](../media-draft/7-deployment-slot-staging.png)

    <span data-ttu-id="1d5ae-123">Observera **webbadressen** till distributionsfacket för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-123">Notice the **URL** of the staging deployment slot.</span></span> <span data-ttu-id="1d5ae-124">Det är samma webbadress som du såg tidigare i början av utbildningsenheten.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-124">It is the exact same URL you saw previously at the beginning of this unit.</span></span>

    <span data-ttu-id="1d5ae-125">Ett distributionsfack behandlas som en webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-125">A deployment slot is treated as a web app inside Azure.</span></span> <span data-ttu-id="1d5ae-126">Det är dock en särskild typ som är underordnad den ursprungliga webbappen och kan växlas med den ursprungliga webbappen.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-126">However, it is a special type that is a child of the original web app and can be swapped with the original web app.</span></span>

    <span data-ttu-id="1d5ae-127">Om du klickar på **webbadressen** visas samma standardsida som Azure skapade för webbappen första gången vi skapade den i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-127">If you click the **URL**, you will see the same default page that Azure created for the web app the first time we created it in the Azure portal.</span></span>

<span data-ttu-id="1d5ae-128">Nu när distributionsfacket för mellanlagring har skapats måste du konfigurera **autentiseringsuppgifter för distributionen**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-128">Now that the staging deployment slot is created successfully, you need to configure **deployment credentials**.</span></span>

## <a name="create-deployment-credentials"></a><span data-ttu-id="1d5ae-129">Skapa autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="1d5ae-129">Create deployment credentials</span></span>

<span data-ttu-id="1d5ae-130">I Azure måste du ställa in autentiseringsuppgifter för distribution innan du kan börja den faktiska distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-130">Azure requires deployment credentials to be set up before you can start the actual deployment process.</span></span> <span data-ttu-id="1d5ae-131">Därför får du lära dig hur du skapar egna autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-131">For that reason, you will learn how to create your own deployment credentials.</span></span>

1. <span data-ttu-id="1d5ae-132">Leta upp och klicka på menyalternativet **Autentiseringsuppgifter för distribution** i navigeringsfönstret till vänster.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-132">Locate and click the **Deployment credentials** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="1d5ae-133">Azure Portal navigerar till bladet **Autentiseringsuppgifter för distribution**, se nedan.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-133">The Azure portal navigates to the **Deployment credentials** blade as shown below.</span></span>

    ![Autentiseringsuppgifter för distribution](../media-draft/7-deployment-credentials.png)

    <span data-ttu-id="1d5ae-135">Ange ett **användarnamn** och ett **lösenord** och bekräfta sedan lösenordet.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-135">Enter a **username** and **password** of your choice, and then confirm your password once again.</span></span>

    > <span data-ttu-id="1d5ae-136">Se till att skriva ned användarnamnet och lösenordet så att du inte glömmer dem.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-136">Make sure you write down your username and password so that you don't forget them!</span></span> <span data-ttu-id="1d5ae-137">Du behöver dem senare när vi börjar ladda upp och distribuera koden till Azure.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-137">You will need them later when we start uploading and deploying our code to Azure.</span></span>

    <span data-ttu-id="1d5ae-138">När du har valt användarnamn och lösenord letar du upp och klickar på knappen **Spara** längst upp på bladet **Autentiseringsuppgifter för distribution**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-138">Once you decide on the username and password, locate and click the **Save** button at the top of the **Deployment credentials** blade.</span></span>

<span data-ttu-id="1d5ae-139">Nu när autentiseringsuppgifterna för distribution har skapats ska du konfigurera **distributionsalternativen**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-139">Now that the deployment credentials are created successfully, you need to configure **deployment options**.</span></span>

## <a name="use-a-local-git-repository-as-your-deployment-option"></a><span data-ttu-id="1d5ae-140">Använda en lokal Git-lagringsplats som distributionsalternativ</span><span class="sxs-lookup"><span data-stu-id="1d5ae-140">Use a local Git repository as your deployment option</span></span>

<span data-ttu-id="1d5ae-141">Nu är det dags att skapa en lokal Git-lagringsplats i Azure så att du kan komma igång med att ladda upp kod.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-141">Now, it is time to create a local Git repository in Azure so that you can start the process of uploading your code.</span></span>

1. <span data-ttu-id="1d5ae-142">Leta upp och klicka på menyalternativet **Distributionsalternativ** i navigeringsfönstret till vänster.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-142">Locate and click the **Deployment options** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="1d5ae-143">Azure Portal navigerar till bladet **Distributionsalternativ**, se nedan.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-143">The Azure portal navigates to the **Deployment options** blade as shown below.</span></span>

    ![Distributionsalternativ](../media-draft/7-deployment-options.png)

1. <span data-ttu-id="1d5ae-145">Klicka på **Choose source / Configure required settings** (Välj källa/konfigurera nödvändiga inställningar).</span><span class="sxs-lookup"><span data-stu-id="1d5ae-145">Click on **Choose source / Configure required settings**.</span></span>

    ![Autentiseringsuppgifter för distribution](../media-draft/7-deployment-sources.png)

1. <span data-ttu-id="1d5ae-147">Azure Portal visar de alternativ du kan konfigurera och använda.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-147">The Azure portal displays the available options that you can configure and use.</span></span> <span data-ttu-id="1d5ae-148">I vårt fall väljer du alternativet **Lokal Git-lagringsplats**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-148">In our case, choose the **Local Git Repository** option.</span></span>

    ![Lokal Git-lagringsplats](../media-draft/7-local-git-repo.png)

1. <span data-ttu-id="1d5ae-150">Leta upp och klicka på knappen **OK** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-150">Locate and click the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="1d5ae-151">Leta upp och klicka på menykommandot **Översikt** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-151">Locate and click the **Overview** menu item on the left-side navigation.</span></span>

    ![Distributionsfack med Git konfigurerat](../media-draft/7-staging-after-setting-git.png)

    <span data-ttu-id="1d5ae-153">Lägg märke till två viktiga uppgifter:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-153">Notice two important pieces of information:</span></span>
    - <span data-ttu-id="1d5ae-154">**Användarnamn för Git/distribution**: det här är de autentiseringsuppgifter som du använder senare för att ansluta till den lokala Git-lagringsplatsen i Azure.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-154">**Git/Deployment username**: These are the credentials that you will use later on to connect to the local Git repository on Azure.</span></span>

    - <span data-ttu-id="1d5ae-155">**URL för Git-klon**: det här är webbadressen till den lokala Git-lagringsplatsen som du kommer använda för **fjärranslutning** till webbappens lokala lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-155">**Git clone url**: This is the local Git repository URL that you will use as a **remote** for your local web application repository.</span></span>

<span data-ttu-id="1d5ae-156">Det är dags att börja ladda upp kod till ett distributionsfack för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-156">It is time to start uploading your code to the staging deployment slot.</span></span>

## <a name="install-git-on-your-machine"></a><span data-ttu-id="1d5ae-157">Installera Git på datorn</span><span class="sxs-lookup"><span data-stu-id="1d5ae-157">Install Git on your machine</span></span>

<span data-ttu-id="1d5ae-158">Jag kommer att installera Git på min Ubuntu 18.04-dator.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-158">I will be installing Git on my Ubuntu 18.04 machine.</span></span>

1. <span data-ttu-id="1d5ae-159">Öppna ett nytt **terminalfönster**</span><span class="sxs-lookup"><span data-stu-id="1d5ae-159">Open a new **Terminal** window</span></span>

1. <span data-ttu-id="1d5ae-160">Ange följande kommando.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-160">Type the following command.</span></span> <span data-ttu-id="1d5ae-161">Du uppmanas att ange ditt Ubuntu-användarlösenord.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-161">You will be prompted to enter your Ubuntu user password.</span></span>

    ```console
    sudo apt-get update
    ```

1. <span data-ttu-id="1d5ae-162">När uppdateringen är färdig anger du följande kommando för att installera Git lokalt.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-162">Once the update is successful, type the following command to install Git locally.</span></span> <span data-ttu-id="1d5ae-163">Du uppmanas att godkänna installationen av Git på din dator.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-163">You will be prompted to accept the installation of Git on your machine.</span></span>

    ```console
    sudo apt-get install git-core
    ```

1. <span data-ttu-id="1d5ae-164">För att kontrollera att Git är installerat anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-164">To verify that Git is now installed, type the following command:</span></span>

    ```console
    git --version
    ```

   <span data-ttu-id="1d5ae-165">Om installationen lyckas visas följande utdata:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-165">If the installation is successful, you will see the following output:</span></span>

    ```console
    git version 2.17.1
    ```

1. <span data-ttu-id="1d5ae-166">Det är alltid en bra idé att konfigurera Git-inställningar genom att ange namn och e-postadress.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-166">It is always a good practice to configure Git settings by providing your name and email.</span></span> <span data-ttu-id="1d5ae-167">För det behöver du ange följande kommandon och ersätta platshållarna för namn och e-post utan klamrar (`{}`):</span><span class="sxs-lookup"><span data-stu-id="1d5ae-167">For that, you need to issue the following commands, replacing the placeholders for name and email, without the curly braces (`{}`):</span></span>

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. <span data-ttu-id="1d5ae-168">Verifiera att informationen har registrerats av Git med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-168">To verify that your information has been recorded by Git, type the following command:</span></span>

    ```console
    cat ~/.gitconfig
    ```

   <span data-ttu-id="1d5ae-169">Du bör se följande utdata med ditt namn och din e-postadress:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-169">You should be seeing the following, with your name and email shown:</span></span>

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a><span data-ttu-id="1d5ae-170">Initiera en lokal Git-lagringsplats för webbappen</span><span class="sxs-lookup"><span data-stu-id="1d5ae-170">Initialize a local Git repository for your web app</span></span>

<span data-ttu-id="1d5ae-171">När du ska börja använda Git behöver du initiera en lokal Git-lagringsplats för webbappen.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-171">To start using Git, you need to initialize a local Git repository for the web app.</span></span>

1. <span data-ttu-id="1d5ae-172">Öppna ett nytt **terminalfönster**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-172">Open a new **Terminal** window.</span></span>

1. <span data-ttu-id="1d5ae-173">Navigera till innehållsrotmappen för på webbappen.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-173">Navigate to the content root folder of your web app.</span></span> <span data-ttu-id="1d5ae-174">Du kan skriva så här:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-174">You can type the following:</span></span>

    ```console
    cd ~/Documents/BestBikeApp/
    ```

    ![Innehållsrotmapp för webbapp](../media-draft/7-web-app-content-root-folder.png)

1. <span data-ttu-id="1d5ae-176">Initiera en ny Git-lagringsplats med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-176">Initialize a new Git repository by issuing the following command:</span></span>

    ```console
    git init
    ```

    <span data-ttu-id="1d5ae-177">Om kommandot lyckas visas ett meddelande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-177">If the command is successful, you receive a message like the following:</span></span>

    ```console
    Initialized empty Git repository in /home/your-user/Documents/BestBikeApp/.git/
    ```

1. <span data-ttu-id="1d5ae-178">Mellanlagra alla webbappsfiler i Git.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-178">Stage all the web app files to Git.</span></span>

   <span data-ttu-id="1d5ae-179">Nästa steg är att informera Git om dina webbappsfiler.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-179">The next step is to let Git know about your web app files.</span></span> <span data-ttu-id="1d5ae-180">Det gör du genom att lägga till alla filer i arbetskatalogen så att de **mellanlagras** av Git.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-180">Do that by adding all the files of the working directory so that they get **staged** by Git.</span></span> <span data-ttu-id="1d5ae-181">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-181">Type the following command:</span></span>

    ```console
    git add .
    ```

    <span data-ttu-id="1d5ae-182">När du anger ”.” i kommandot ovan läggs alla filer till för mellanlagring i Git.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-182">The command above adds all files, represented by the ".", to the staging state of Git.</span></span>

1. <span data-ttu-id="1d5ae-183">Nu behöver du checka in ändringarna i Git.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-183">Now, you need to commit your changes to Git.</span></span>

   <span data-ttu-id="1d5ae-184">När du mellanlagrar filerna med Git behöver du checka in filerna i den lokala **Git-incheckningshistoriken**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-184">Once you stage the files with Git, you need to commit your files to the **Git commit history** on your local machine.</span></span> <span data-ttu-id="1d5ae-185">Det gör du med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-185">You do that by typing the following command:</span></span>

   `git commit -m "Initial create"`

   <span data-ttu-id="1d5ae-186">Kommandot `commit` kan ta argumentet `-m` med ett meddelande om incheckningen du skapar.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-186">The `commit` command accepts  `-m` argument to include a message with the commit you are creating.</span></span> <span data-ttu-id="1d5ae-187">När du senare överför koden till Azure kan du se samma meddelande för just den här incheckningen.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-187">Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.</span></span>

## <a name="add-a-remote-for-the-local-git-repository"></a><span data-ttu-id="1d5ae-188">Lägga till en fjärranslutning till den lokala Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="1d5ae-188">Add a remote for the local Git repository</span></span>

<span data-ttu-id="1d5ae-189">Hittills har du initierat en ny lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-189">At this point, you have successfully initialized a new local Git repository.</span></span> <span data-ttu-id="1d5ae-190">Du har dessutom checkat in alla dina webbappsfiler till Git.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-190">In addition, you've committed all of your web app files to Git.</span></span> <span data-ttu-id="1d5ae-191">Det som återstår är att lägga till en **fjärranslutning** som ansluter din lokala Git-lagringsplats till den som hanteras i Azure.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-191">What remains is to add a **remote** to connect your local Git repository to that hosted on Azure.</span></span>

<span data-ttu-id="1d5ae-192">För att göra det behöver du:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-192">To do so, you need to:</span></span>

1. <span data-ttu-id="1d5ae-193">Kopiera den **Git-klon-URL** som du såg ovan.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-193">Copy the **Git clone url** that you saw above.</span></span>

1. <span data-ttu-id="1d5ae-194">När den har kopierats går du tillbaka till **terminalfönstret** och anger följande Git-kommando:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-194">Once copied, you go back to the **Terminal** window and issue the following Git command:</span></span>

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    <span data-ttu-id="1d5ae-195">Git-kommandot ovan kopplar din lokala Git-lagringsplats till den som hanteras i Azure.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-195">The above Git command hooks your local Git repository to the one hosted on Azure.</span></span> <span data-ttu-id="1d5ae-196">Nu kan du börja skicka och hämta mellan den lokala och den fjärranslutna Git-lagringsplatsen!</span><span class="sxs-lookup"><span data-stu-id="1d5ae-196">Now, you can start pushing and pulling between the local and remote Git repositories!</span></span>

1. <span data-ttu-id="1d5ae-197">Verifiera ovanstående kommando genom att ange följande Git-kommando:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-197">To verify the above command, type the following Git command:</span></span>

    ```
    git remote -v
    ```

    <span data-ttu-id="1d5ae-198">Ovanstående kommando genererar följande utdata:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-198">The command above generates the following output:</span></span>

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a><span data-ttu-id="1d5ae-199">Skicka koden till Azure</span><span class="sxs-lookup"><span data-stu-id="1d5ae-199">Push your code to Azure</span></span>

<span data-ttu-id="1d5ae-200">Nu när du har kopplat din lokala Git-lagringsplats till den fjärranslutna Git-lagringsplatsen i Azure kommer du att skriva och kompilera appen och sedan skicka den till Azure.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-200">Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app, and then push your web app to Azure.</span></span>

1. <span data-ttu-id="1d5ae-201">Skriv följande Git-kommando för att skicka din **huvudgren** till den fjärranslutna Git-lagringsplatsen i Azure:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-201">Type the following Git command to push your **master** branch to the remote Git repository on Azure:</span></span>

    ```console
    git push origin master
    ```

1. <span data-ttu-id="1d5ae-202">Du får ange lösenordet du konfigurerade i avsnittet **Autentiseringsuppgifter för distribution** ovan.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-202">You will be prompted to enter the password that you have configured in the **Deployment credentials** section above.</span></span> <span data-ttu-id="1d5ae-203">Ange lösenordet och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-203">Enter your password and hit Enter.</span></span> <span data-ttu-id="1d5ae-204">Git börjar överföra dina incheckade filer till den fjärranslutna Git-lagringsplatsen i Azure som konfigurerats under distributionsfacket för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-204">Git starts uploading your committed files to the Azure remote Git repository configured under the staging deployment slot.</span></span>

## <a name="verify-the-web-app-is-uploaded-to-azure"></a><span data-ttu-id="1d5ae-205">Verifiera att webbappen har överförts till Azure</span><span class="sxs-lookup"><span data-stu-id="1d5ae-205">Verify the web app is uploaded to Azure</span></span>

1. <span data-ttu-id="1d5ae-206">Logga in på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-206">Log in to the Azure portal.</span></span>

1. <span data-ttu-id="1d5ae-207">Leta upp och klicka på menyalternativet **Alla resurser** i navigeringsfönstret till vänster.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-207">Locate and click on the **All Resources** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="1d5ae-208">Azure Portal navigerar till listan med alla resurser som skapats i Azure hittills.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-208">The Azure portal navigates you to the list of all resources created on Azure so far.</span></span>

1. <span data-ttu-id="1d5ae-209">Leta upp och klicka på mellanlagringsplatsen du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-209">Locate and click on the staging slot created above.</span></span> <span data-ttu-id="1d5ae-210">Kom ihåg att distributionsfack ses som webbappar och därför visas som webbappsresurser under **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-210">Remember, a deployment slot is considered a web app, and hence, it will appear as a web app resource under **All Resources**.</span></span>

1. <span data-ttu-id="1d5ae-211">När du kommer till bladet med distributionsfacket för mellanlagring går du till **Distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-211">Once you arrive to the staging deployment slot blade, go to **Deployment options**.</span></span>

    ![Distributionsfack för mellanlagring efter uppladdning av webbappen](../media-draft/7-staging-deployment-slot-after-uploading-files.png)

    <span data-ttu-id="1d5ae-213">Du kommer att se att din första lokala incheckning nu har laddats upp till Azure Portal!</span><span class="sxs-lookup"><span data-stu-id="1d5ae-213">You will see that your first commit that you have locally on your machine is now uploaded to the Azure portal!</span></span>

    <span data-ttu-id="1d5ae-214">Genom att skicka koden lokalt till den fjärranslutna Git-lagringsplatsen som hanteras i Azure har Azure registrerat den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-214">By pushing your code locally to the remote Git repository hosted on Azure, Azure has recorded this operation.</span></span>

    <span data-ttu-id="1d5ae-215">Varje gång du skickar kod till Azure så visas en ny post tillsammans med meddelandet du angav när du checkade in ändringarna lokalt.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-215">Every time you push your code to Azure, you will see a new record, together with the message that you type when committing your changes locally on your machine.</span></span>

1. <span data-ttu-id="1d5ae-216">Vi går till webbadressen för **mellanlagringsfacket**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-216">Let's visit the **staging slot** URL.</span></span> <span data-ttu-id="1d5ae-217">Vi nämnde webbadressen ovan, men om du glömmer den kan du alltid se adressen på sidan **Översikt** i distributionsfacket för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-217">The URL was mentioned above, however, if your forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.</span></span>

1. <span data-ttu-id="1d5ae-218">Ange följande webbadress i webbläsarens adressfält: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="1d5ae-218">Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span></span>

    ![Mellanlagringsfack som hanteras online](../media-draft/7-staging-slot-hosted-online.png)

<span data-ttu-id="1d5ae-220">Du har nu laddat upp dina lokala webbappsfiler till distributionsfacket för mellanlagring i Azure.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-220">You have successfully uploaded your local web app files to the staging deployment slot on Azure.</span></span>

## <a name="swapping-the-staging-and-production-deployment-slots"></a><span data-ttu-id="1d5ae-221">Växla mellan distributionsfack för mellanlagring och produktion</span><span class="sxs-lookup"><span data-stu-id="1d5ae-221">Swapping the staging and production deployment slots</span></span>

<span data-ttu-id="1d5ae-222">Nu när programmet är igång och körs i distributionsfacket för mellanlagring som hanteras i Azure är det dags att växla till produktionsfacket.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-222">Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one.</span></span> <span data-ttu-id="1d5ae-223">Det gör du så här:</span><span class="sxs-lookup"><span data-stu-id="1d5ae-223">To do so, follow these steps:</span></span>

1. <span data-ttu-id="1d5ae-224">Leta upp och navigera till det ursprungliga webbappsbladet som du skapade i utbildningsenhet 2.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-224">Locate and navigate to the original web app blade, created in Unit #2.</span></span>

1. <span data-ttu-id="1d5ae-225">Leta upp och klicka på menyalternativet **Distributionsfack** i navigeringsfönstret till vänster.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-225">Locate and click the **Deployment slots** menu item on the left-side navigation.</span></span>

    ![Bladet för distributionsfack för webbapp](../media-draft/7-web-app-slots.png)

1. <span data-ttu-id="1d5ae-227">Leta upp och klicka på knappen **Växla** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-227">Locate and click on the **Swap** button at the top of the blade.</span></span>

1. <span data-ttu-id="1d5ae-228">Azure Portal tar dig till bladet **Växla**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-228">The Azure portal navigates you to the **Swap** blade.</span></span>

    ![Bladet Växla](../media-draft/7-swap-blade.png)

1. <span data-ttu-id="1d5ae-230">I fältet **Växla** väljer du **Växla**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-230">For the **Swap** field, select **Swap**.</span></span>

1. <span data-ttu-id="1d5ae-231">I fältet **Källa** väljer du **Mellanlagring**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-231">For the **Source** field, select **Staging**.</span></span>

1. <span data-ttu-id="1d5ae-232">I fältet **Mål** väljer du **Produktion**.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-232">For the **Destination** field, select **Production**.</span></span>

1. <span data-ttu-id="1d5ae-233">Leta upp och klicka på knappen **OK** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-233">Locate and click on the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="1d5ae-234">Azure startar växlingsprocessen.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-234">Azure starts the swapping process.</span></span> <span data-ttu-id="1d5ae-235">Den här åtgärden tar vanligtvis några sekunder beroende på hur stor webbappen som växlas är.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-235">Usually, this operation takes a few seconds, depending on the size of the web app being swapped.</span></span>

1. <span data-ttu-id="1d5ae-236">När åtgärden är färdig går du till webbappens URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="1d5ae-236">Once the operation ends, visit the web app URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span></span>

    ![Sida för webbapp](../media-draft/7-web-app-page.png)

    <span data-ttu-id="1d5ae-238">Växlingsåtgärden är färdig!</span><span class="sxs-lookup"><span data-stu-id="1d5ae-238">The swapping operation has been successful!</span></span> <span data-ttu-id="1d5ae-239">Du kan nu se att koden du överförde till distributionsfacket för mellanlagring även finns i produktionsfacket!</span><span class="sxs-lookup"><span data-stu-id="1d5ae-239">You can now see the code that you uploaded to the staging deployment slot also being hosted on the production slot!</span></span>

1. <span data-ttu-id="1d5ae-240">Gå nu till webbadressen för mellanlagringsfacket: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="1d5ae-240">Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span></span>

    ![Mellanlagring för webbapp](../media-draft/7-staging-after-swapping.png)

    <span data-ttu-id="1d5ae-242">Distributionsfacket för mellanlagring innehåller nu webbappens ursprungliga standardfiler som tidigare hanterades i produktionsfacket.</span><span class="sxs-lookup"><span data-stu-id="1d5ae-242">The staging deployment slot now contains the original, default web application files that were previously hosted under the production slot.</span></span>

<span data-ttu-id="1d5ae-243">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1d5ae-243">Congratulations!</span></span> <span data-ttu-id="1d5ae-244">Du har laddat upp din webbapp till Azure!</span><span class="sxs-lookup"><span data-stu-id="1d5ae-244">You have successfully uploaded your web app to Azure!</span></span>
