<span data-ttu-id="e2fa3-101">I den här övningen laddar du upp ditt ASP.NET Core-program till Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-101">In this exercise, you'll upload your ASP.NET Core application to Azure.</span></span>

## <a name="create-a-staging-deployment-slot"></a><span data-ttu-id="e2fa3-102">Skapa ett distributionsfack för mellanlagring</span><span class="sxs-lookup"><span data-stu-id="e2fa3-102">Create a staging deployment slot</span></span>

1. <span data-ttu-id="e2fa3-103">Leta upp och klicka på menykommandot **Distributionsfack** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-103">Locate and click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="e2fa3-104">Azure öppnar bladet **Distributionsfack** enligt vad som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-104">Azure opens the **Deployment slots** blade as shown below.</span></span>

    ![Distributionsfack](../media-draft/7-deployment-slot-blade.png)

1. <span data-ttu-id="e2fa3-106">Leta upp och klicka på knappen **+ Lägg till plats** i det övre navigeringsfältet i bladet distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-106">Locate and click the **+ Add Slot** button on the top navigation bar of the deployment slots blade.</span></span>

1. <span data-ttu-id="e2fa3-107">Azure-portalen öppnar bladet **Lägg till en plats** enligt vad som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-107">Azure portal opens the **Add a slot** blade as shown below.</span></span>

    ![Lägga till en plats](../media-draft/7-new-deployment-slot-blade.png)

    <span data-ttu-id="e2fa3-109">Namnge ditt distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-109">Give your deployment slot a name.</span></span> <span data-ttu-id="e2fa3-110">I det här fallet använder du **Staging**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-110">In this case, use **Staging**.</span></span>

    <span data-ttu-id="e2fa3-111">I valet av **konfigurationskälla** har du två alternativ.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-111">To choose a **Configuration Source** you have 2 options.</span></span>

    1. <span data-ttu-id="e2fa3-112">Välj antingen att klona konfigurationselementen från en annan plats eller från den webbapp som skapades på Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-112">Either select to clone the configuration elements from any other slot or Web app created on Azure.</span></span>

    1. <span data-ttu-id="e2fa3-113">Eller så kan du välja att inte klona några konfigurationselement. I så fall väljer du alternativet **Don't clone configuration from an existing slot** (Klona inte konfiguration från en befintlig plats).</span><span class="sxs-lookup"><span data-stu-id="e2fa3-113">Or, you can choose not to clone any configuration elements, in this case, you select the option **Don't clone configuration from an existing slot**.</span></span>

    <span data-ttu-id="e2fa3-114">Välj det andra alternativet, **Don't clone configuration from an existing slot** (Klona inte konfiguration från en befintlig plats).</span><span class="sxs-lookup"><span data-stu-id="e2fa3-114">Choose the second option, **Don't clone configuration from an existing slot**.</span></span>

1. <span data-ttu-id="e2fa3-115">Leta upp och klicka på knappen **OK** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-115">Locate and click the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="e2fa3-116">När distributionsfacket har skapats navigerar Azure-portalen tillbaka till bladet **Distributionsfack** för webbappen.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-116">Once the deployment slot is successfully created, Azure Portal navigates you back to the **Deployment slots** blade of your Web app.</span></span>

    <span data-ttu-id="e2fa3-117">Du kan nu se det nya distributionsfacket som du just skapade.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-117">Now, you can see the new deployment slot that you have just created.</span></span>

    ![Distributionsfack har skapats](../media-draft/7-deployment-slot-created.png)

1. <span data-ttu-id="e2fa3-119">Leta upp och klicka på det nya distributionsfacket.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-119">Locate and click the new deployment slot.</span></span>

1. <span data-ttu-id="e2fa3-120">Azure-portalen navigerar till sidan **Översikt** för det nyligen skapade distributionsfacket.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-120">Azure Portal navigates to the **Overview** page of the newly created deployment slot.</span></span>

    ![Distributionsfack för mellanlagring](../media-draft/7-deployment-slot-staging.png)

    <span data-ttu-id="e2fa3-122">Observera **URL:en** för distributionsfacket för mellanlagring; den är exakt samma som den du såg tidigare i början av den här enheten.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-122">Notice the **URL** of the staging deployment slot, it is exactly the one you saw previously at the beginning of this unit.</span></span>

    <span data-ttu-id="e2fa3-123">Ett distributionsfack behandlas som en webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-123">A deployment slot is treated as a Web app inside Azure.</span></span> <span data-ttu-id="e2fa3-124">Men det är en särskild typ som är underordnad den ursprungliga webbappen och därför kan växlas med den ursprungliga webbappen.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-124">However, it is a special type that is a child of the original Web app and can be swapped with the original Web app.</span></span>

    <span data-ttu-id="e2fa3-125">Om du klickar på **URL:en** visas samma standardsida som den Azure skapade för webbappen första gången vi skapade den på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-125">If you click the **URL**, you will see the same default page that Azure created for the Web app, the first time we created it on the Azure Portal.</span></span>

<span data-ttu-id="e2fa3-126">Nu när distributionsfacket för mellanlagring har skapats behöver du konfigurera **autentiseringsuppgifterna för distribution**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-126">Now that the staging deployment slot is created successfully, you need to configure **Deployment credentials**.</span></span>

## <a name="create-deployment-credentials"></a><span data-ttu-id="e2fa3-127">Skapa autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="e2fa3-127">Create deployment credentials</span></span>

<span data-ttu-id="e2fa3-128">Azure kräver att autentiseringsuppgifterna för distribution konfigureras innan du kan börja faktiska distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-128">Azure requires deployment credentials to be setup before you can start the actual deployment process.</span></span> <span data-ttu-id="e2fa3-129">Därför kommer du att lära dig hur du skapar dina egna autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-129">For that reason, you will learn, how to create your own deployment credentials.</span></span>

1. <span data-ttu-id="e2fa3-130">Leta upp och klicka på menykommandot **Autentiseringsuppgifter för distribution** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-130">Locate and click the **Deployment credentials** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="e2fa3-131">Azure-portalen navigerar till bladet **Autentiseringsuppgifter för distribution** enligt vad som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-131">Azure Portal navigates to the **Deployment credentials** blade as shown below.</span></span>

    ![Autentiseringsuppgifter för distribution](../media-draft/7-deployment-credentials.png)

    <span data-ttu-id="e2fa3-133">Ange ett **användarnamn** och ett **lösenord** och bekräfta sedan lösenordet igen.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-133">Enter a **username** and **password** of your choice and then confirm your password once again.</span></span>

    > <span data-ttu-id="e2fa3-134">Se till att skriva ned användarnamnet och lösenordet så att du inte glömmer dem.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-134">Make sure you write down your username and password so that you don't forget them!</span></span> <span data-ttu-id="e2fa3-135">Du behöver dem senare när vi börjar ladda upp och distribuera koden till Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-135">You will need them later when we start uploading and deploying our code to Azure.</span></span>

    <span data-ttu-id="e2fa3-136">När du har valt användarnamn och lösenord letar du upp och klickar på knappen **Spara** längst upp på bladet **Autentiseringsuppgifter för distribution**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-136">Once you decided on the username and password, locate and click the **Save** button at the top of the **Deployment credentials** blade.</span></span>

<span data-ttu-id="e2fa3-137">Nu när autentiseringsuppgifterna för distribution har skapats behöver du konfigurera **distributionsalternativen**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-137">Now that the deployment credentials are created successfully, you need to configure **Deployment options**.</span></span>

## <a name="use-a-local-git-repository-as-your-deployment-option"></a><span data-ttu-id="e2fa3-138">Använda en lokal Git-lagringsplats som distributionsalternativ</span><span class="sxs-lookup"><span data-stu-id="e2fa3-138">Use a local Git repository as your deployment option</span></span>

<span data-ttu-id="e2fa3-139">Nu är det dags att skapa en lokal Git-lagringsplats i Azure så att du startar processen med att ladda upp din kod.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-139">Now, it is time to create a local Git repository in Azure so that you start the process of uploading your code.</span></span>

1. <span data-ttu-id="e2fa3-140">Leta upp och klicka på menykommandot **Distributionsalternativ** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-140">Locate and click the **Deployment options** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="e2fa3-141">Azure-portalen navigerar till bladet **Distributionsalternativ** enligt vad som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-141">Azure Portal navigates to the **Deployment options** blade as shown below.</span></span>

    ![Distributionsalternativ](../media-draft/7-deployment-options.png)

1. <span data-ttu-id="e2fa3-143">Klicka på **Choose source / Configure required settings** (Välj källa/konfigurera nödvändiga inställningar).</span><span class="sxs-lookup"><span data-stu-id="e2fa3-143">Click on **Choose source / Configure required settings**.</span></span>

    ![Autentiseringsuppgifter för distribution](../media-draft/7-deployment-sources.png)

1. <span data-ttu-id="e2fa3-145">Azure-portalen visar de tillgängliga alternativen som du kan konfigurera och använda.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-145">Azure Portal displays the available options that you can configure and use.</span></span> <span data-ttu-id="e2fa3-146">I det här fallet väljer du alternativet **Local Git Repository** (Lokal Git-lagringsplats).</span><span class="sxs-lookup"><span data-stu-id="e2fa3-146">For our case, choose the **Local Git Repository** option.</span></span>

    ![Lokal Git-lagringsplats](../media-draft/7-local-git-repo.png)

1. <span data-ttu-id="e2fa3-148">Leta upp och klicka på knappen **OK** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-148">Locate and click the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="e2fa3-149">Leta upp och klicka på menykommandot **Översikt** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-149">Locate and click the **Overview** menu item on the left-side navigation.</span></span>

    ![Distributionsfack med Git konfigurerat](../media-draft/7-staging-after-setting-git.png)

    <span data-ttu-id="e2fa3-151">Lägg märke till två viktiga uppgifter:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-151">Notice two important pieces of information:</span></span>
    - <span data-ttu-id="e2fa3-152">**Användarnamn för Git/distribution**: det här är de autentiseringsuppgifter som du använder senare för att ansluta till den lokala Git-lagringsplatsen på Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-152">**Git/Deployment username**: These are the credentials that you will use later on to connect to the local Git repository on Azure.</span></span>

    - <span data-ttu-id="e2fa3-153">**URL för Git-klon**: det här är den lokala Git-lagringsplatsens URL som du ska använda för att ställa in som en **fjärranslutning** för din lokala webbprogramlagringsplats.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-153">**Git clone url**: This is the local Git repository URL that you will use to set as a **remote** for your local Web application repository.</span></span>

<span data-ttu-id="e2fa3-154">Det är dags att börja ladda upp din kod till ett distributionsfack för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-154">It is time to start uploading your code to the staging deployment slot.</span></span>

## <a name="install-git-on-your-machine"></a><span data-ttu-id="e2fa3-155">Installera Git på datorn</span><span class="sxs-lookup"><span data-stu-id="e2fa3-155">Install Git on your machine</span></span>

<span data-ttu-id="e2fa3-156">Jag kommer att installera Git på min Ubuntu 18.04-dator.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-156">I will be installing Git on my Ubuntu 18.04 machine.</span></span>

1. <span data-ttu-id="e2fa3-157">Öppna ett nytt **terminalfönster**</span><span class="sxs-lookup"><span data-stu-id="e2fa3-157">Open a new **Terminal** window</span></span>

1. <span data-ttu-id="e2fa3-158">Ange följande kommando.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-158">Type the following commend.</span></span> <span data-ttu-id="e2fa3-159">Du uppmanas att ange ditt Ubuntu-användarlösenord.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-159">You will be prompted to enter your Ubuntu user password.</span></span>

    ```console
    sudo apt-get update
    ```

1. <span data-ttu-id="e2fa3-160">När uppdateringen är klar anger du följande kommando för att installera Git lokalt.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-160">Once the update is successful, type the following command to install Git locally.</span></span> <span data-ttu-id="e2fa3-161">Du uppmanas att godkänna installationen av Git på datorn.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-161">you will be prompt to accept the installation of Git on your machine.</span></span>

    ```console
    sudo apt-get install git-core
    ```

1. <span data-ttu-id="e2fa3-162">För att kontrollera att Git nu är installerat anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-162">To verify that Git is now installed, type the following command:</span></span>

    ```console
    git --version
    ```

   <span data-ttu-id="e2fa3-163">Om installationen lyckas visas följande utdata:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-163">If the installation is successful, you will see the following output:</span></span>

    ```console
    git version 2.17.1
    ```

1. <span data-ttu-id="e2fa3-164">Det är alltid en bra idé att konfigurera Git-inställningar genom att ange ditt namn och din e-post.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-164">It is always a good practice to configure Git settings by providing your name and email.</span></span> <span data-ttu-id="e2fa3-165">För det behöver du ange följande kommandon och ersätta platshållarna för namn och e-post utan klamrarna (`{}`):</span><span class="sxs-lookup"><span data-stu-id="e2fa3-165">For that, you need to issue the following commands, replacing the placeholders for name and email without the curly braces (`{}`):</span></span>

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. <span data-ttu-id="e2fa3-166">För att verifiera att din information har registrerats av Git anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-166">To verify that your information has been recorded by Git, type the following command:</span></span>

    ```console
    cat ~/.gitconfig
    ```

   <span data-ttu-id="e2fa3-167">Du bör se följande med ditt namn och din e-post som visas:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-167">You should be seeing the following with your name and email shown:</span></span>

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a><span data-ttu-id="e2fa3-168">Initiera en lokal Git-lagringsplats för webbappen</span><span class="sxs-lookup"><span data-stu-id="e2fa3-168">Initialize a local Git repository for your web app</span></span>

<span data-ttu-id="e2fa3-169">För att börja använda Git behöver du initiera en lokal Git-lagringsplats för webbappen.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-169">To start using Git, you need to initialize a local Git repository for the Web app.</span></span>

1. <span data-ttu-id="e2fa3-170">Öppna ett nytt **terminalfönster**</span><span class="sxs-lookup"><span data-stu-id="e2fa3-170">Open a new **Terminal** window</span></span>

1. <span data-ttu-id="e2fa3-171">Navigera till innehållsrotmappen för på webbappen; du kan skriva följande:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-171">Navigate to the content root folder of your Web app, you can type the following:</span></span>

    ```console
    cd ~/Documents/BestBikeApp/
    ```

    ![Innehållsrotmapp för webbapp](../media-draft/7-web-app-content-root-folder.png)

1. <span data-ttu-id="e2fa3-173">Initiera en ny Git-lagringsplats genom att ange följande kommando</span><span class="sxs-lookup"><span data-stu-id="e2fa3-173">Initialize a new Git repository by issuing the following command</span></span>

    ```console
    git init
    ```

    <span data-ttu-id="e2fa3-174">Om kommandot lyckas visas ett meddelande som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-174">If the command is successful, you receive a message like the following:</span></span>

    ```console
    Initialized empty Git repository in /home/your-user/Documents/BestBikeApp/.git/
    ```

1. <span data-ttu-id="e2fa3-175">Mellanlagra alla webbappfiler till Git</span><span class="sxs-lookup"><span data-stu-id="e2fa3-175">Stage all the Web app files to Git</span></span>

   <span data-ttu-id="e2fa3-176">Nästa steg är att informera Git om dina webbappfiler.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-176">The next step is to let Git know about your Web app files.</span></span> <span data-ttu-id="e2fa3-177">Göra det genom att lägga till alla filer i arbetskatalogen så att de **mellanlagras** av Git.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-177">Do that by adding all the files of the working directory so that they get **Staged** by Git.</span></span> <span data-ttu-id="e2fa3-178">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-178">Type the following command:</span></span>

    ```console
    git add .
    ```

    <span data-ttu-id="e2fa3-179">Kommandot ovan lägger till alla filer, vilket representeras av ".", till mellanlagringstillståndet för Git.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-179">The command above add all files, represented by the "." to the staging state of Git.</span></span>

1. <span data-ttu-id="e2fa3-180">Nu behöver du checka in ändringarna till Git</span><span class="sxs-lookup"><span data-stu-id="e2fa3-180">Now, you need to commit your changes to the Git</span></span>

   <span data-ttu-id="e2fa3-181">När du mellanlagrar filerna med Git behöver du checka in filerna till **Git-incheckningshistoriken** på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-181">Once you stage the files with Git, you need to commit your files to the **Git Commit History** on your local machine.</span></span> <span data-ttu-id="e2fa3-182">Det gör du genom att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-182">You do that, by typing the following command:</span></span>

   `git commit -m "Initial create"`

   <span data-ttu-id="e2fa3-183">Kommandot **commit** tar argumentet **-m** för att inkludera ett meddelande med den incheckning som du skapar.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-183">The **commit** command accepts  **-m** argument to include a message with the commit you are creating.</span></span> <span data-ttu-id="e2fa3-184">Vid ett senare tillfälle när du har överfört koden till Azure kommer du att kunna se samma meddelande lagrat med den här specifika incheckningen.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-184">Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.</span></span>

## <a name="add-a-remote-for-the-local-git-repository"></a><span data-ttu-id="e2fa3-185">Lägga till en fjärranslutning för den lokala Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="e2fa3-185">Add a remote for the local Git repository</span></span>

<span data-ttu-id="e2fa3-186">Fram till nu har du initierat en ny lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-186">Up until now, you have successfully initialized a new local Git repository.</span></span> <span data-ttu-id="e2fa3-187">Du har dessutom checkat in alla dina webbappfiler till Git.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-187">In addition, you've committed all of your Web app files to Git.</span></span> <span data-ttu-id="e2fa3-188">Det som är kvar att göra är att lägga till en **fjärranslutning** för att ansluta din lokala Git-lagringsplats till den som hanteras i Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-188">What remains is to add a **Remote** to connect your local Git repository, to that hosted on Azure.</span></span>

<span data-ttu-id="e2fa3-189">För att göra det behöver du:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-189">To do so, you need to:</span></span>

1. <span data-ttu-id="e2fa3-190">Kopiera den **Git-klon-URL** som du såg ovan.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-190">Copy the **Git clone url** that you saw above.</span></span>

1. <span data-ttu-id="e2fa3-191">När den har kopierats går du tillbaka till **terminalfönstret** och anger följande Git-kommando:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-191">Once copied, you go back to the **Terminal** window and issue the following Git command:</span></span>

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    <span data-ttu-id="e2fa3-192">Got-kommandot ovan kopplar din lokala Git-lagringsplats till den som hanteras i Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-192">The above Git command hooks your local Git repository to the one hosted on Azure.</span></span> <span data-ttu-id="e2fa3-193">Nu kan du börja skicka och hämta mellan den lokala och den fjärranslutna Git-lagringsplatsen!</span><span class="sxs-lookup"><span data-stu-id="e2fa3-193">Now you can start pushing and pulling between the local and remote Git repositories!</span></span>

1. <span data-ttu-id="e2fa3-194">Kontrollera att kommandot ovan har körts genom att ange följande Git-kommando:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-194">To verify that the above command, type the following Git command:</span></span>

    ```
    git remote -v
    ```

    <span data-ttu-id="e2fa3-195">Ovanstående kommando genererar följande utdata:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-195">The command above generates the following output:</span></span>

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a><span data-ttu-id="e2fa3-196">Skicka koden till Azure</span><span class="sxs-lookup"><span data-stu-id="e2fa3-196">Push your code to Azure</span></span>

<span data-ttu-id="e2fa3-197">Nu när du har kopplat din lokala Git-lagringsplats till den fjärranslutna Git-lagringsplatsen på Azure kommer du att utveckla och bygga appen och sedan skicka webbappen till Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-197">Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app and then push your Web app to Azure.</span></span>

1. <span data-ttu-id="e2fa3-198">Skriv följande Git-kommando för att skicka din **huvudgren** till den fjärranslutna Git-lagringsplatsen på Azure:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-198">Type the following Git command to push your **master** branch to the remote Git repository on Azure:</span></span>

    ```console
    git push origin master
    ```

1. <span data-ttu-id="e2fa3-199">Du uppmanas att ange det lösenord som du har konfigurerat i avsnittet **Autentiseringsuppgifter för distribution** ovan.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-199">You will be prompted to enter the password that you have configured in the **Deployment credentials** section above.</span></span> <span data-ttu-id="e2fa3-200">Ange lösenordet och tryck på [Retur].</span><span class="sxs-lookup"><span data-stu-id="e2fa3-200">Enter your password and hit [Enter].</span></span> <span data-ttu-id="e2fa3-201">Git börjar ladda upp dina incheckade filer till den fjärranslutna Git-lagringsplatsen på Azure som konfigurerats under distributionsfacket för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-201">Git starts uploading your committed files to Azure remote Git repository configured under Staging deployment slot.</span></span>

## <a name="verify-the-web-app-is-uploaded-to-azure"></a><span data-ttu-id="e2fa3-202">Kontrollera att webbappen har laddats upp till Azure</span><span class="sxs-lookup"><span data-stu-id="e2fa3-202">Verify the Web app is uploaded to Azure</span></span>

1. <span data-ttu-id="e2fa3-203">Logga in på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e2fa3-203">Login to the Azure portal</span></span>

1. <span data-ttu-id="e2fa3-204">Leta upp och klicka på menykommandot **Alla resurser** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-204">Locate and click on the **All Resources** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="e2fa3-205">Azure-portalen navigerar till listan över alla resurser som skapats på Azure hittills.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-205">Azure Portal navigates you to the list of all resources created on Azure so far.</span></span>

1. <span data-ttu-id="e2fa3-206">Leta upp och klicka på den mellanlagringsplats som skapades ovan.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-206">Locate and click on the staging slot created above.</span></span> <span data-ttu-id="e2fa3-207">Kom ihåg att ett distributionsfack betraktas som en webbapp och därför visas som en webbappresurs under **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-207">Remember, a deployment slot is considered a Web App and hence it will appear as a Web App resource under **All Resources**.</span></span>

1. <span data-ttu-id="e2fa3-208">När du kommer till bladet distributionsfack för mellanlagring går du till **Distributionsalternativ**</span><span class="sxs-lookup"><span data-stu-id="e2fa3-208">Once you arrive to the staging deployment slot blade, go to **Deployment options**</span></span>

    ![Distributionsfacket för mellanlagring efter uppladdning av webbappen](../media-draft/7-staging-deployment-slot-after-uploading-files.png)

    <span data-ttu-id="e2fa3-210">Du kommer att se att din första incheckning som du har lokalt på datorn nu har laddats upp till Azure-portalen!</span><span class="sxs-lookup"><span data-stu-id="e2fa3-210">You will see your first commit that you have locally on your machine, is now uploaded to Azure Portal!</span></span>

    <span data-ttu-id="e2fa3-211">Genom att skicka koden lokalt till den fjärranslutna Git-lagringsplatsen som hanteras i Azure har Azure registrerat den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-211">By pushing your code locally to the remote Git repository hosted on Azure, Azure has recorded down this operation.</span></span>

    <span data-ttu-id="e2fa3-212">Varje gång du skickar koden till Azure visas en ny post tillsammans med det meddelande som du anger när du checkar in ändringarna lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-212">Every time you push your code to Azure, you will see a new record together with the message that you type when committing your changes locally on your machine.</span></span>

1. <span data-ttu-id="e2fa3-213">Vi går till URL:en för **mellanlagringsplatsen**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-213">Let's visit the **Staging slot** URL.</span></span> <span data-ttu-id="e2fa3-214">URL:en har nämnts ovan, men om du glömmer den URL:en kan du alltid gå till sidan **Översikt** i distributionsfacket för mellanlagring och hämta URL:en.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-214">The URL was mentioned above, however, if your forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.</span></span>

1. <span data-ttu-id="e2fa3-215">Ange följande URL i webbläsarens adressfält: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/)</span><span class="sxs-lookup"><span data-stu-id="e2fa3-215">Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/)</span></span>

    ![Mellanlagringsplatsen som hanteras online](../media-draft/7-staging-slot-hosted-online.png)

<span data-ttu-id="e2fa3-217">Du har laddat upp dina lokala webbappfiler till en distributionsfacket för mellanlagring på Azure.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-217">You have successfully uploaded your local Web app files to the staging deployment slot on Azure.</span></span>

## <a name="swapping-the-staging-and-production-deployment-slots"></a><span data-ttu-id="e2fa3-218">Växla distributionsfack för mellanlagring och produktion</span><span class="sxs-lookup"><span data-stu-id="e2fa3-218">Swapping the Staging and Production deployment slots</span></span>

<span data-ttu-id="e2fa3-219">Nu när programmet är igång och körs på det distributionsfack för mellanlagring som hanteras i Azure är det dags att växla den här platsen med platsen för produktion.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-219">Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one.</span></span> <span data-ttu-id="e2fa3-220">För att göra det följer du dessa steg:</span><span class="sxs-lookup"><span data-stu-id="e2fa3-220">To do so, follow these steps:</span></span>

1. <span data-ttu-id="e2fa3-221">Leta upp och navigera till den ursprungliga webbappsbladet som skapades i enhet 2.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-221">Locate and navigate to the original Web app blade, created in Unit #2.</span></span>

1. <span data-ttu-id="e2fa3-222">Leta upp och klicka på menykommandot **Distributionsfack** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-222">Locate and click the **Deployment slots** menu item on the left-side navigation.</span></span>

    ![Bladet för distributionsfack för webbapp](../media-draft/7-web-app-slots.png)

1. <span data-ttu-id="e2fa3-224">Leta upp och klicka på knappen **Växla** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-224">Locate and click on the **Swap** button at the top of the blade.</span></span>

1. <span data-ttu-id="e2fa3-225">Azure-portalen navigerar till bladet **Växla**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-225">Azure Portal navigates you to the **Swap** blade.</span></span>

    ![Bladet Växla](../media-draft/7-swap-blade.png)

1. <span data-ttu-id="e2fa3-227">Ställ in fältet **Växla** till **Växla**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-227">Select the **Swap** field to be **Swap**.</span></span>

1. <span data-ttu-id="e2fa3-228">Ställ in fältet **Källa** till **Mellanlagring**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-228">Select the **Source** field to be **Staging**.</span></span>

1. <span data-ttu-id="e2fa3-229">Ställ in fältet **Mål** till **Produktion**.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-229">Select the **Destination** field to be **Production**</span></span>

1. <span data-ttu-id="e2fa3-230">Leta upp och klicka på knappen **OK** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-230">Locate and click on the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="e2fa3-231">Azure startar växlingsprocessen.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-231">Azure starts the swapping process.</span></span> <span data-ttu-id="e2fa3-232">Den här åtgärden behöver vanligtvis några avsnitt beroende på storleken på den webbapp som växlas.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-232">Usually, this operation takes a few sections depending on the size of the Web app being swapped.</span></span>

1. <span data-ttu-id="e2fa3-233">När åtgärden är klar går du till webbappens URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/)</span><span class="sxs-lookup"><span data-stu-id="e2fa3-233">Once the operation ends, visit the Web App URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/)</span></span>

    ![Sida för webbapp](../media-draft/7-web-app-page.png)

    <span data-ttu-id="e2fa3-235">Växlingsåtgärden har är klar!</span><span class="sxs-lookup"><span data-stu-id="e2fa3-235">The swapping operation has been successful!</span></span> <span data-ttu-id="e2fa3-236">Du kan nu se den kod som du laddade upp till en distributionsfacket för mellanlagring som även hanteras på produktionsplatsen!</span><span class="sxs-lookup"><span data-stu-id="e2fa3-236">You can now see the code that you have uploaded to the staging deployment slot, being hosted also on the production slot!</span></span>

1. <span data-ttu-id="e2fa3-237">Gå till URL:en för mellanlagringsplatsen: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/)</span><span class="sxs-lookup"><span data-stu-id="e2fa3-237">Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/)</span></span>

    ![Mellanlagring för webbapp](../media-draft/7-staging-after-swapping.png)

    <span data-ttu-id="e2fa3-239">Distributionsfacket för mellanlagring innehåller nu de ursprungliga standardwebbprogramfiler som tidigare hanterades under produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="e2fa3-239">The staging deployment slot now contains the original default web application files that were previously hosted under the production slot.</span></span>

<span data-ttu-id="e2fa3-240">Grattis!</span><span class="sxs-lookup"><span data-stu-id="e2fa3-240">Congratulations!</span></span> <span data-ttu-id="e2fa3-241">Du har laddat upp din webbapp till Azure!</span><span class="sxs-lookup"><span data-stu-id="e2fa3-241">You have successfully uploaded your Web app to the Azure!</span></span>
