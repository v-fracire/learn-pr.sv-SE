<span data-ttu-id="dc917-101">I den här delen överför du ditt ASP.NET Core-program till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="dc917-101">In this unit, you'll upload your ASP.NET Core application to Azure App Service.</span></span>

## <a name="create-a-staging-deployment-slot"></a><span data-ttu-id="dc917-102">Skapa ett distributionsfack för mellanlagring</span><span class="sxs-lookup"><span data-stu-id="dc917-102">Create a staging deployment slot</span></span>

1. <span data-ttu-id="dc917-103">Växla tillbaka till [Azure Portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="dc917-103">Switch back to the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true).</span></span>

1. <span data-ttu-id="dc917-104">Öppna den apptjänsteresurs (webbappen) som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="dc917-104">Open the App Service resource (the web app) you created previously.</span></span> <span data-ttu-id="dc917-105">Du kan hitta den igen genom att söka efter appen i **Alla resurser** eller den innehållande resursgruppen i **Resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="dc917-105">You can find it again by searching for the app in **All resources** or the containing resource group in **Resource groups**.</span></span>

1. <span data-ttu-id="dc917-106">Klicka på menyalternativet **Distributionsfack** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="dc917-106">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="dc917-107">På sidan **Distributionsfack** klickar du på knappen**Lägg till fack** i det övre navigationsfältet på sidan för distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="dc917-107">Inside the **Deployment slots** page, click the **Add Slot** button on the top navigation bar of the deployment slots page.</span></span>

1. <span data-ttu-id="dc917-108">Azure-portalen öppnar sidan **Lägg till ett fack** på det sätt som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="dc917-108">The Azure portal opens the **Add a slot** page as shown below.</span></span>

    1. <span data-ttu-id="dc917-109">Namnge ditt distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="dc917-109">Give your deployment slot a name.</span></span> <span data-ttu-id="dc917-110">I det här fallet använder du `staging`.</span><span class="sxs-lookup"><span data-stu-id="dc917-110">In this case, use `staging`.</span></span>

    2. <span data-ttu-id="dc917-111">När du väljer **Konfigurationskälla** har du två alternativ.</span><span class="sxs-lookup"><span data-stu-id="dc917-111">To choose a **Configuration Source**, you have two options.</span></span>

        * <span data-ttu-id="dc917-112">Du kan välja att klona konfigurationselementen från ett befintligt distributionsfack eller en App Service-app.</span><span class="sxs-lookup"><span data-stu-id="dc917-112">You can choose to clone the configuration elements from any existing deployment slot or App Service app.</span></span>
        * <span data-ttu-id="dc917-113">Eller så kan du välja att inte klona några konfigurationselement.</span><span class="sxs-lookup"><span data-stu-id="dc917-113">Or you can choose not to clone any configuration elements.</span></span> <span data-ttu-id="dc917-114">Välj alternativet **Klona inte konfigurationen från ett befintligt fack**.</span><span class="sxs-lookup"><span data-stu-id="dc917-114">Select the option **Don't clone configuration from an existing slot**.</span></span>

        <span data-ttu-id="dc917-115">För det här distributionsfacket, väljer du det andra alternativet **Klona inte konfigurationen från ett befintligt fack**.</span><span class="sxs-lookup"><span data-stu-id="dc917-115">For this deployment slot, choose the second option: **Don't clone configuration from an existing slot**.</span></span> <span data-ttu-id="dc917-116">Du kommer att konfigurera det direkt.</span><span class="sxs-lookup"><span data-stu-id="dc917-116">You will configure it directly.</span></span>

    ![Skärmbild av Azure-portalen som visar konfigurationen för ett nytt distributionsfack för mellanlagring.](../media/7-new-deployment-slot-blade.png)

1. <span data-ttu-id="dc917-118">Klicka på **OK** längst ned på sidan så skapas ditt nya distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="dc917-118">Click the **OK** button at the bottom of the page to create your new deployment slot.</span></span>

1. <span data-ttu-id="dc917-119">När distributionsfacket har skapats navigerar Azure-portalen tillbaka till sidan **Distributionsfack** för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="dc917-119">Once the deployment slot is successfully created, the Azure portal navigates you back to the **Deployment slots** page of your web app.</span></span>

    <span data-ttu-id="dc917-120">Du kan nu se det nya distributionsfack som du just skapade.</span><span class="sxs-lookup"><span data-stu-id="dc917-120">Now, you can see the new deployment slot that you have just created.</span></span>

    ![Skärmbild av Azure-portalen som visar sidan Distributionsfack med det nya facket som har skapats.](../media/7-deployment-slot-created.png)

1. <span data-ttu-id="dc917-122">Välj det nya distributionsfacket.</span><span class="sxs-lookup"><span data-stu-id="dc917-122">Select the new deployment slot.</span></span>

1. <span data-ttu-id="dc917-123">Azure-portalen navigerar till **Översikt**-sidan för det nyligen skapade distributionsfacket.</span><span class="sxs-lookup"><span data-stu-id="dc917-123">The Azure portal navigates to the **Overview** page of the newly created deployment slot.</span></span>

    ![Distributionsfack för mellanlagring](../media/7-deployment-slot-staging.png)

    <span data-ttu-id="dc917-125">Observera **URL** för distributionsfacket för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="dc917-125">Notice the **URL** of the staging deployment slot.</span></span> <span data-ttu-id="dc917-126">Det är en annan URL än den du såg tidigare, med facknamnet sist.</span><span class="sxs-lookup"><span data-stu-id="dc917-126">It is a different URL from what you saw previously, with the slot name appended.</span></span>

    <span data-ttu-id="dc917-127">Ett distributionsfack behandlas som en fullvärdig App Service-app i Azure.</span><span class="sxs-lookup"><span data-stu-id="dc917-127">A deployment slot is treated as a full App Service app inside Azure.</span></span> <span data-ttu-id="dc917-128">Det är dock en särskild typ som är underordnad den ursprungliga appen och kan växlas med den ursprungliga appen.</span><span class="sxs-lookup"><span data-stu-id="dc917-128">However, it is a special type that is a child of the original app and can be swapped with the original app.</span></span>

    <span data-ttu-id="dc917-129">Om du klickar på **webbadressen** visas samma standardsida som Azure skapade för ”appen” till distributionsfacket första gången vi skapade den i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc917-129">If you click the **URL**, you will see the same default page that Azure created for the deployment slot "app" the first time we created it in the Azure portal.</span></span>

<span data-ttu-id="dc917-130">Nu när distributionsfacket för mellanlagring har skapats måste du konfigurera **autentiseringsuppgifter för distributionen**.</span><span class="sxs-lookup"><span data-stu-id="dc917-130">Now that the staging deployment slot is created successfully, you need to configure **deployment credentials**.</span></span>

## <a name="create-deployment-credentials"></a><span data-ttu-id="dc917-131">Skapa autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="dc917-131">Create deployment credentials</span></span>

<span data-ttu-id="dc917-132">I Azure måste du ställa in autentiseringsuppgifter för distribution innan du kan börja den faktiska distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="dc917-132">Azure requires deployment credentials to be set up before you can start the actual deployment process.</span></span> <span data-ttu-id="dc917-133">Därför kommer du att lära dig hur du skapar dina egna autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="dc917-133">For that reason, you will learn how to create your own deployment credentials.</span></span>

1. <span data-ttu-id="dc917-134">Klicka på menykommandot **Autentiseringsuppgifter för distribution** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="dc917-134">Click the **Deployment credentials** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="dc917-135">Azure Portal navigerar till sidan **Autentiseringsuppgifter för distribution** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="dc917-135">The Azure portal navigates to the **Deployment credentials** page as shown below.</span></span>

    <span data-ttu-id="dc917-136">Ange ett **användarnamn** och ett **lösenord** och bekräfta sedan lösenordet igen.</span><span class="sxs-lookup"><span data-stu-id="dc917-136">Enter a **username** and **password** of your choice, and then confirm your password once again.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc917-137">Se till att du inte glömmer ditt användarnamn och lösenord!</span><span class="sxs-lookup"><span data-stu-id="dc917-137">Make sure you don't forget your username and password!</span></span> <span data-ttu-id="dc917-138">Du kommer att behöva dem senare när vi börjar ladda upp och distribuera koden till Azure.</span><span class="sxs-lookup"><span data-stu-id="dc917-138">You will need them later when we start uploading and deploying our code to Azure.</span></span>

    ![Skärmbild av Azure-portalen som visar sidan Autentiseringsuppgifter för distribution på mellanlagringsfacket med exempel på autentiseringsuppgifter i de obligatoriska fälten.](../media/7-deployment-credentials.png)

1. <span data-ttu-id="dc917-140">Klicka på **Spara** längst upp på sidan **Autentiseringsuppgifter för distribution**.</span><span class="sxs-lookup"><span data-stu-id="dc917-140">Click the **Save** button at the top of the **Deployment credentials** page.</span></span>

<span data-ttu-id="dc917-141">Nu när autentiseringsuppgifterna för distribution har skapats behöver du konfigurera övriga distributionsalternativ.</span><span class="sxs-lookup"><span data-stu-id="dc917-141">Now that the deployment credentials are created successfully, you need to configure other deployment options.</span></span>

## <a name="use-a-local-git-repository-as-your-deployment-option"></a><span data-ttu-id="dc917-142">Använda en lokal Git-lagringsplats som distributionsalternativ</span><span class="sxs-lookup"><span data-stu-id="dc917-142">Use a local Git repository as your deployment option</span></span>

<span data-ttu-id="dc917-143">Därefter skapar vi en lokal Git-lagringsplats i Azure så att du kan börja ladda upp din kod.</span><span class="sxs-lookup"><span data-stu-id="dc917-143">Next, we'll create a local Git repository in Azure, so you can start uploading your code.</span></span>

1. <span data-ttu-id="dc917-144">I appen till distributionsfacket för **mellanlagring** klickar du på menyalternativet **Distributionsalternativ** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="dc917-144">Within the **staging** deployment slot "app", click the **Deployment options** menu item on the left-hand navigation.</span></span>

1. <span data-ttu-id="dc917-145">Azure-portalen navigerar till sidan **Distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="dc917-145">The Azure portal navigates to the **Deployment options** page.</span></span>

1. <span data-ttu-id="dc917-146">Klicka på **Välj källa** för att konfigurera de nödvändiga inställningarna.</span><span class="sxs-lookup"><span data-stu-id="dc917-146">Click on the **Choose Source** to configure the required settings.</span></span>

1. <span data-ttu-id="dc917-147">Azure-portalen visar de tillgängliga alternativen som du kan konfigurera och använda.</span><span class="sxs-lookup"><span data-stu-id="dc917-147">The Azure portal displays the available options that you can configure and use.</span></span> <span data-ttu-id="dc917-148">I vårt fall väljer du alternativet **Lokal Git-lagringsplats**.</span><span class="sxs-lookup"><span data-stu-id="dc917-148">In our case, choose the **Local Git Repository** option.</span></span>

1. <span data-ttu-id="dc917-149">Du kommer tillbaka till sidan **Distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="dc917-149">You will be returned to the **Deployment option** page.</span></span> <span data-ttu-id="dc917-150">Klicka på **OK** längst ned på sidan för att konfigurera distributionskällan.</span><span class="sxs-lookup"><span data-stu-id="dc917-150">Click the **OK** button at the bottom of the page to set up the deployment source.</span></span>

1. <span data-ttu-id="dc917-151">Navigera nu till avsnittet **Översikt** i navigeringen till vänster.</span><span class="sxs-lookup"><span data-stu-id="dc917-151">Now, navigate to the **Overview** section on the left-side navigation.</span></span>

    <span data-ttu-id="dc917-152">Den viktiga informationen att notera här är **URI för Git Clone** som är den lokala Git-lagringsplatsens URL, och som du kommer att använda som en **fjärranslutning** för din lokala programkodslagringsplats.</span><span class="sxs-lookup"><span data-stu-id="dc917-152">The important information to note here is the **Git Clone Uri**, which is the local Git repository URL that you will use as a **remote** for your local application code repository.</span></span>

<span data-ttu-id="dc917-153">Det är nu dags att börja ladda upp koden till ett distributionsfack för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="dc917-153">It is time to start uploading your code to the staging deployment slot.</span></span>

## <a name="set-up-git-on-cloud-shell"></a><span data-ttu-id="dc917-154">Ställ in git i Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="dc917-154">Set up git on Cloud Shell</span></span>

<span data-ttu-id="dc917-155">Git är redan installerat i Azure Cloud Shell, men du vill ställa in ditt användarnamn och din e-post för ditt cloud shell-konto.</span><span class="sxs-lookup"><span data-stu-id="dc917-155">Git is already installed Azure Cloud Shell but you'll want to set your username and email for your cloud shell account.</span></span>

1. <span data-ttu-id="dc917-156">I Cloud Shell till höger skriver du följande kommandon, där du ersätter platshållarna `[your name]` och `[your email]` med ditt eget namn och e-postadress (utan klammerparanteserna):</span><span class="sxs-lookup"><span data-stu-id="dc917-156">In the Cloud Shell on the right, type the following commands, replacing the `[your name]` and `[your email]` placeholders with your own name and email (without the braces):</span></span>

    ```bash
    git config --global user.name "[your name]"
    git config --global user.email "[your email]"
    ```

1. <span data-ttu-id="dc917-157">För att verifiera att din information har registrerats av Git anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dc917-157">To verify that your information has been recorded by Git, type the following command:</span></span>

    ```bash
    cat ~/.gitconfig
    ```

   <span data-ttu-id="dc917-158">Du bör se följande utdata med ditt namn och din e-postadress:</span><span class="sxs-lookup"><span data-stu-id="dc917-158">You should be seeing the following, with your name and email shown:</span></span>

    ```output
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-code"></a><span data-ttu-id="dc917-159">Initiera en lokal Git-lagringsplats för koden</span><span class="sxs-lookup"><span data-stu-id="dc917-159">Initialize a local Git repository for your code</span></span>

<span data-ttu-id="dc917-160">När du ska börja använda Git behöver du initiera en lokal Git-lagringsplats för din .NET Core-programkod.</span><span class="sxs-lookup"><span data-stu-id="dc917-160">To start using Git, you need to initialize a local Git repository for your .NET Core application code.</span></span>

1. <span data-ttu-id="dc917-161">Kontrollera att du är i projektmappen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="dc917-161">Make sure you are in the project folder you created earlier.</span></span>

    ```bash
    cd ~/BestBikeApp/
    ```

1. <span data-ttu-id="dc917-162">Initiera en ny Git-lagringsplats genom att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dc917-162">Initialize a new Git repository by issuing the following command:</span></span>

    ```bash
    git init
    ```

    <span data-ttu-id="dc917-163">Om kommandot lyckas visas ett meddelande som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="dc917-163">If the command is successful, you receive a message like the following:</span></span>

    ```output
    Initialized empty Git repository in /home/{your-user}/BestBikeApp/.git/
    ```

1. <span data-ttu-id="dc917-164">Mellanlagra alla programfiler till Git.</span><span class="sxs-lookup"><span data-stu-id="dc917-164">Stage all the application files to Git.</span></span>

   <span data-ttu-id="dc917-165">Nästa steg är att informera Git om dina programfiler.</span><span class="sxs-lookup"><span data-stu-id="dc917-165">The next step is to let Git know about your application files.</span></span> <span data-ttu-id="dc917-166">Det gör du genom att lägga till alla filer i arbetskatalogen så att de **mellanlagras** av Git.</span><span class="sxs-lookup"><span data-stu-id="dc917-166">Do that by adding all the files of the working directory so that they get **staged** by Git.</span></span> <span data-ttu-id="dc917-167">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dc917-167">Type the following command:</span></span>

    ```bash
    git add .
    ```

    <span data-ttu-id="dc917-168">När du anger ”.” i kommandot ovan läggs alla filer till för mellanlagring i Git.</span><span class="sxs-lookup"><span data-stu-id="dc917-168">The command above adds all files, represented by the ".", to the staging state of Git.</span></span>

1. <span data-ttu-id="dc917-169">Nu behöver du checka in ändringarna i Git.</span><span class="sxs-lookup"><span data-stu-id="dc917-169">Now, you need to commit your changes to Git.</span></span>

   <span data-ttu-id="dc917-170">När du mellanlagrar filerna med Git behöver du checka in filerna i den lokala **Git-incheckningshistoriken**.</span><span class="sxs-lookup"><span data-stu-id="dc917-170">Once you stage the files with Git, you need to commit your files to the **Git commit history** on your local machine.</span></span> <span data-ttu-id="dc917-171">Det gör du med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dc917-171">You do that by typing the following command:</span></span>

    ```bash
   git commit -m "Initial create"
    ```

   <span data-ttu-id="dc917-172">Kommandot `commit` kan ta argumentet `-m` med ett meddelande om incheckningen du skapar.</span><span class="sxs-lookup"><span data-stu-id="dc917-172">The `commit` command accepts  `-m` argument to include a message with the commit you are creating.</span></span> <span data-ttu-id="dc917-173">När du senare överför koden till Azure kan du se samma meddelande för just den här incheckningen.</span><span class="sxs-lookup"><span data-stu-id="dc917-173">Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.</span></span>

## <a name="add-a-remote-for-the-local-git-repository"></a><span data-ttu-id="dc917-174">Lägga till en fjärranslutning till den lokala Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="dc917-174">Add a remote for the local Git repository</span></span>

<span data-ttu-id="dc917-175">Hittills har du initierat en ny lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="dc917-175">At this point, you have successfully initialized a new local Git repository.</span></span> <span data-ttu-id="dc917-176">Du har dessutom skickat alla dina programfiler till Git.</span><span class="sxs-lookup"><span data-stu-id="dc917-176">In addition, you've committed all of your application files to Git.</span></span> <span data-ttu-id="dc917-177">Det som återstår är att lägga till en **fjärranslutning** som ansluter din lokala Git-lagringsplats till den som hanteras i Azure.</span><span class="sxs-lookup"><span data-stu-id="dc917-177">What remains is to add a **remote** to connect your local Git repository to that hosted on Azure.</span></span>

<span data-ttu-id="dc917-178">För att göra det behöver du:</span><span class="sxs-lookup"><span data-stu-id="dc917-178">To do so, you need to:</span></span>

1. <span data-ttu-id="dc917-179">Kopiera den **Git-klon-URL** som du såg ovan.</span><span class="sxs-lookup"><span data-stu-id="dc917-179">Copy the **Git clone url** that you saw above.</span></span>

1. <span data-ttu-id="dc917-180">När den har kopierats går du tillbaka till **terminalfönstret** och anger följande Git-kommando med din URL:</span><span class="sxs-lookup"><span data-stu-id="dc917-180">Once copied, you go back to the **Terminal** window and issue the following Git command with your url:</span></span>

    ```bash
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    <span data-ttu-id="dc917-181">Git-kommandot ovan kopplar din lokala Git-lagringsplats till den som hanteras i Azure.</span><span class="sxs-lookup"><span data-stu-id="dc917-181">The above Git command hooks your local Git repository to the one hosted on Azure.</span></span> <span data-ttu-id="dc917-182">Nu kan du börja skicka och hämta mellan den lokala och den fjärranslutna Git-lagringsplatsen!</span><span class="sxs-lookup"><span data-stu-id="dc917-182">Now, you can start pushing and pulling between the local and remote Git repositories!</span></span>

1. <span data-ttu-id="dc917-183">Verifiera ovanstående kommando genom att ange följande Git-kommando:</span><span class="sxs-lookup"><span data-stu-id="dc917-183">To verify the above command, type the following Git command:</span></span>

    ```bash
    git remote -v
    ```

    <span data-ttu-id="dc917-184">Ovanstående kommando genererar följande utdata:</span><span class="sxs-lookup"><span data-stu-id="dc917-184">The command above generates the following output:</span></span>

    ```output
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a><span data-ttu-id="dc917-185">Skicka koden till Azure</span><span class="sxs-lookup"><span data-stu-id="dc917-185">Push your code to Azure</span></span>

<span data-ttu-id="dc917-186">Nu när du har kopplat din lokala Git-lagringsplats till den fjärranslutna Git-lagringsplatsen i Azure kommer du att utveckla och kompilera appen och sedan skicka programkoden till Azure.</span><span class="sxs-lookup"><span data-stu-id="dc917-186">Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app, and then push your application code to Azure.</span></span>

1. <span data-ttu-id="dc917-187">Skriv följande Git-kommando för att skicka din **huvudgren** till den fjärranslutna Git-lagringsplatsen i Azure:</span><span class="sxs-lookup"><span data-stu-id="dc917-187">Type the following Git command to push your **master** branch to the remote Git repository on Azure:</span></span>

    ```bash
    git push origin master
    ```

1. <span data-ttu-id="dc917-188">Du får ange lösenordet du konfigurerade i avsnittet **Autentiseringsuppgifter för distribution** ovan.</span><span class="sxs-lookup"><span data-stu-id="dc917-188">You will be prompted to enter the password that you have configured in the **Deployment credentials** section above.</span></span> <span data-ttu-id="dc917-189">Ange lösenordet och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="dc917-189">Enter your password and hit Enter.</span></span> <span data-ttu-id="dc917-190">Git börjar överföra dina incheckade filer till den fjärranslutna Git-lagringsplatsen i Azure som konfigurerats under distributionsfacket för mellanlagring.</span><span class="sxs-lookup"><span data-stu-id="dc917-190">Git starts uploading your committed files to the Azure remote Git repository configured under the staging deployment slot.</span></span>

## <a name="verify-the-code-is-uploaded-to-azure"></a><span data-ttu-id="dc917-191">Verifiera att koden har överförts till Azure</span><span class="sxs-lookup"><span data-stu-id="dc917-191">Verify the code is uploaded to Azure</span></span>

1. <span data-ttu-id="dc917-192">Växla tillbaka till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="dc917-192">Switch back to the Azure portal.</span></span>

1. <span data-ttu-id="dc917-193">Klicka på menykommandot **Alla resurser** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="dc917-193">Click on the **All Resources** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="dc917-194">Azure-portalen navigerar till listan över alla resurser som skapats på Azure hittills.</span><span class="sxs-lookup"><span data-stu-id="dc917-194">The Azure portal navigates you to the list of all resources created on Azure so far.</span></span>

1. <span data-ttu-id="dc917-195">Klicka på det mellanlagringsfack som skapades ovan.</span><span class="sxs-lookup"><span data-stu-id="dc917-195">Click on the staging slot created above.</span></span> <span data-ttu-id="dc917-196">Kom ihåg att ett distributionsfack ses som en app och därför visas som webbappsresurser under **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="dc917-196">Remember, a deployment slot is considered as an app, and hence, it will appear as an App Service resource under **All Resources**.</span></span>

1. <span data-ttu-id="dc917-197">När du kommer till sidan distributionsfack för mellanlagring går du till **Distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="dc917-197">Once you arrive to the staging deployment slot page, go to **Deployment options**.</span></span>

    <span data-ttu-id="dc917-198">Du kommer att se att din första incheckning som du har lokalt på datorn nu har laddats upp till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc917-198">You will see that your first commit that you have locally on your machine is now uploaded to the Azure portal.</span></span>

    <span data-ttu-id="dc917-199">När du har överfört din kod lokalt till Git-fjärrlagringsplatsen i App Service registrerar Azure den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="dc917-199">When you push your code locally to the remote Git repository in App Service, Azure records this operation.</span></span>

    <span data-ttu-id="dc917-200">Varje gång du skickar kod till Azure så visas en ny post tillsammans med meddelandet du angav när du checkade in ändringarna lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="dc917-200">Every time you push your code to Azure, you will see a new record, together with the message that you type when committing your changes locally on your machine.</span></span>

    ![Skärmbild av Azure-portalen som visar en nyligen genomförd Git-lagringsplatsdistribution på sidan Utvecklingsalternativ.](../media/7-staging-deployment-slot-after-uploading-files.png)

1. <span data-ttu-id="dc917-202">Vi går till webbadressen för **mellanlagringsfacket**.</span><span class="sxs-lookup"><span data-stu-id="dc917-202">Let's visit the **staging slot** URL.</span></span> <span data-ttu-id="dc917-203">URL:en nämndes ovan, men om du glömmer den så kan du alltid gå till sidan **Översikt** i distributionsfacket för mellanlagring och hämta URL:en.</span><span class="sxs-lookup"><span data-stu-id="dc917-203">The URL was mentioned above, however, if you forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.</span></span>

1. <span data-ttu-id="dc917-204">Ange följande URL i webbläsarens adressfält: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="dc917-204">Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span></span>

    ![Skärmbild som visar en webbläsarvy av webbplatsen för distributionsfack för mellanlagring.](../media/7-staging-slot-hosted-online.png)

<span data-ttu-id="dc917-206">Du har nu laddat upp dina lokala programfiler till distributionsfacket för mellanlagring i Azure.</span><span class="sxs-lookup"><span data-stu-id="dc917-206">You have successfully uploaded your local application files to the staging deployment slot on Azure.</span></span>

## <a name="swapping-the-staging-and-production-deployment-slots"></a><span data-ttu-id="dc917-207">Växla mellan distributionsfacken för mellanlagring och produktion</span><span class="sxs-lookup"><span data-stu-id="dc917-207">Swapping the staging and production deployment slots</span></span>

<span data-ttu-id="dc917-208">Nu när programmet är igång och körs i distributionsfacket för mellanlagring som hanteras i Azure är det dags att växla till produktionsfacket.</span><span class="sxs-lookup"><span data-stu-id="dc917-208">Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one.</span></span> <span data-ttu-id="dc917-209">Det gör du så här:</span><span class="sxs-lookup"><span data-stu-id="dc917-209">To do so, follow these steps:</span></span>

1. <span data-ttu-id="dc917-210">Gå till den ursprungliga appsida som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="dc917-210">Navigate to the original app page created earlier.</span></span> <span data-ttu-id="dc917-211">Du hittar den ursprungliga webbappen från sidan **Alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="dc917-211">You can find the original web app from the **All resources** page.</span></span>

1. <span data-ttu-id="dc917-212">Klicka på menyalternativet **Distributionsfack** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="dc917-212">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="dc917-213">Klicka på knappen **Växla** längst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="dc917-213">Click on the **Swap** button at the top of the page.</span></span>

1. <span data-ttu-id="dc917-214">Azure Portal tar dig till sidan **Växla**.</span><span class="sxs-lookup"><span data-stu-id="dc917-214">The Azure portal navigates you to the **Swap** page.</span></span>

1. <span data-ttu-id="dc917-215">I fältet **Växla** väljer du **Växla**.</span><span class="sxs-lookup"><span data-stu-id="dc917-215">For the **Swap** field, select **Swap**.</span></span>

1. <span data-ttu-id="dc917-216">I fältet **Källa** väljer du **Mellanlagring**.</span><span class="sxs-lookup"><span data-stu-id="dc917-216">For the **Source** field, select **Staging**.</span></span>

1. <span data-ttu-id="dc917-217">I fältet **Mål** väljer du **Produktion**.</span><span class="sxs-lookup"><span data-stu-id="dc917-217">For the **Destination** field, select **Production**.</span></span>

    ![Skärmbild av Azure-portalen som visar sidan för distributionsfacksväxling.](../media/7-swap-blade.png)

1. <span data-ttu-id="dc917-219">Klicka på knappen **OK** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="dc917-219">Click on the **OK** button at the bottom of the page.</span></span>

1. <span data-ttu-id="dc917-220">Azure startar växlingsprocessen.</span><span class="sxs-lookup"><span data-stu-id="dc917-220">Azure starts the swapping process.</span></span> <span data-ttu-id="dc917-221">Den här åtgärden tar vanligtvis några sekunder beroende på hur stor webbappen som växlas är.</span><span class="sxs-lookup"><span data-stu-id="dc917-221">Usually, this operation takes a few seconds, depending on the size of the web app being swapped.</span></span>

1. <span data-ttu-id="dc917-222">När åtgärden avslutas, gå till webbappens URL. Du hittar den på översiktssidan för apptjänsten i portalen: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="dc917-222">Once the operation ends, visit your web app URL; you can find it in the Overview page for your app service in the portal: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span></span>

    ![Skärmbild som visar en webbläsarvy över det tidigare distributionsfacket för mellanlagring som nu finns som primär webbapp.](../media/7-web-app-page.png)

    <span data-ttu-id="dc917-224">Växlingsåtgärden är nu klar!</span><span class="sxs-lookup"><span data-stu-id="dc917-224">The swapping operation has been successful!</span></span> <span data-ttu-id="dc917-225">Du kan nu se att den kod som du överförde till distributionsfacket för mellanlagring även finns på produktionsfacket.</span><span class="sxs-lookup"><span data-stu-id="dc917-225">You can now see the code that you uploaded to the staging deployment slot also being hosted on the production slot.</span></span>

1. <span data-ttu-id="dc917-226">Besök nu URL:en för mellanlagringsfacket: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="dc917-226">Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span></span>

    ![Skärmbild som visar en webbläsarvy över det tidigare primära distributionsfacket som nu finns som en webbapp med distributionsfack för mellanlagring.](../media/7-staging-after-swapping.png)

    <span data-ttu-id="dc917-228">Distributionsfacket för mellanlagring innehåller nu HTML-filernas ursprungliga standardfiler som tidigare hanterades i produktionsfacket.</span><span class="sxs-lookup"><span data-stu-id="dc917-228">The staging deployment slot now serves the original, default HTML files that were previously served from the production slot.</span></span>

<span data-ttu-id="dc917-229">Grattis!</span><span class="sxs-lookup"><span data-stu-id="dc917-229">Congratulations!</span></span> <span data-ttu-id="dc917-230">Du har överfört din programkod till Azure och växlat distributionsfack.</span><span class="sxs-lookup"><span data-stu-id="dc917-230">You have successfully uploaded your application code to Azure and swapped deployment slots.</span></span>
