<span data-ttu-id="e1592-101">Den färdiga funktionen med ett Azure DevOps-projekt skapar bygg- och versionspipelines som passar för de tekniker som valts.</span><span class="sxs-lookup"><span data-stu-id="e1592-101">The out of the box experience with an Azure DevOps Project creates build and release pipelines that make sense for the technologies picked.</span></span> <span data-ttu-id="e1592-102">För den här modulen skapade du en bygg- och versionspipeline som passar för en Node.js-app som körs i en container i ett Kubernetes-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1592-102">For this module, you created a build and release pipeline that makes sense for a node.js app running in a container hosted in a Kubernetes cluster.</span></span> 

<span data-ttu-id="e1592-103">Ofta behöver vi anpassa bygg- och versionspipelines för att utföra vissa åtgärder för projektet.</span><span class="sxs-lookup"><span data-stu-id="e1592-103">Often times we need to customize the build and release pipelines to do specific things for our project.</span></span> <span data-ttu-id="e1592-104">Bygg- och versionspipelines i VSTS är 100 % anpassningsbara.</span><span class="sxs-lookup"><span data-stu-id="e1592-104">The build and release pipelines in VSTS are 100% customizable.</span></span> <span data-ttu-id="e1592-105">Du kan göra så att pipelines utför vad du än behöver.</span><span class="sxs-lookup"><span data-stu-id="e1592-105">You can make the pipelines do whatever you need it to do.</span></span>

<span data-ttu-id="e1592-106">I den här enheten lär du dig att anpassa dina bygg- och versionspipelines.</span><span class="sxs-lookup"><span data-stu-id="e1592-106">In this unit, learn how to customize your build and release pipelines.</span></span>

## <a name="customize-the-build-pipeline"></a><span data-ttu-id="e1592-107">Anpassa bygg-pipelinen</span><span class="sxs-lookup"><span data-stu-id="e1592-107">Customize the build pipeline</span></span>

<span data-ttu-id="e1592-108">Byggmotorn i VSTS är bara en uppgiftskörare som utför uppgifter en efter en.</span><span class="sxs-lookup"><span data-stu-id="e1592-108">The build engine in VSTS is just a task runner, doing one task, after another.</span></span> <span data-ttu-id="e1592-109">Om du vill anpassa bygget behöver du bara lägga till eller ta bort uppgifter och fylla i rätt parametrar för uppgiften.</span><span class="sxs-lookup"><span data-stu-id="e1592-109">To customize the build, you just add or remove tasks and fill out the correct parameters for the task.</span></span>

<span data-ttu-id="e1592-110">VSTS levereras med cirka 100 uppgifter som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="e1592-110">Out of the box, VSTS comes with about 100 tasks that you can use.</span></span> <span data-ttu-id="e1592-111">Om du behöver göra något som inte finns inbyggt kan du söka på marknadsplatsen, där det finns över 700 bygg- och versionsuppgifter att ladda ned och använda.</span><span class="sxs-lookup"><span data-stu-id="e1592-111">If you need to do something that doesn't exist out of the box, check the marketplace where there are over 700 build and release tasks ready to be downloaded and used.</span></span> <span data-ttu-id="e1592-112">Du kan även skriva dina egna anpassade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="e1592-112">You also have the ability to write your own custom tasks.</span></span>

<span data-ttu-id="e1592-113">För den här enheten anpassar du bygg-pipelinen genom att installera marknadsplatsuppgifterna WhiteSource Bolt för att genomföra säkerhetsgenomsökning av koden.</span><span class="sxs-lookup"><span data-stu-id="e1592-113">For this unit, you will customize the build pipeline by installing the marketplace tasks WhiteSource Bolt to do security scanning of our code.</span></span>

1. <span data-ttu-id="e1592-114">Bläddra till Azure DevOps-projektet i Azure-portalen och klicka på länken för byggdefinition</span><span class="sxs-lookup"><span data-stu-id="e1592-114">Browse to the Azure DevOps Project in the Azure portal and click on the build definition link</span></span>  
![Bygglänk](/media-draft/3-buildlink.png)

2. <span data-ttu-id="e1592-116">Det här leder till sidan för bygg-pipelines.</span><span class="sxs-lookup"><span data-stu-id="e1592-116">This takes you to the build pipelines page.</span></span> <span data-ttu-id="e1592-117">Klicka på bygget och välj `Edit`</span><span class="sxs-lookup"><span data-stu-id="e1592-117">Click on the build and select `Edit`</span></span>  
<span data-ttu-id="e1592-118">![Edit Build](/media-draft/3-editbuild.png) (Redigera bygge)</span><span class="sxs-lookup"><span data-stu-id="e1592-118">![Edit Build](/media-draft/3-editbuild.png)</span></span>

3. <span data-ttu-id="e1592-119">Det här leder till sidan för byggdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="e1592-119">This takes you to the build definition.</span></span> <span data-ttu-id="e1592-120">Klicka på `+` för att lägga till en uppgift i Agent Job 1 (Agentjobb 1)</span><span class="sxs-lookup"><span data-stu-id="e1592-120">Click on the `+` to add a task to Agent Job 1</span></span>  
<span data-ttu-id="e1592-121">![Add Task](/media-draft/3-addtask.png) (Lägg till uppgift)</span><span class="sxs-lookup"><span data-stu-id="e1592-121">![Add Task](/media-draft/3-addtask.png)</span></span>

4. <span data-ttu-id="e1592-122">I textfältet skriver du `bolt` och klickar på `Get it free`</span><span class="sxs-lookup"><span data-stu-id="e1592-122">In the text field, type `bolt` and click `Get it free`</span></span>  
<span data-ttu-id="e1592-123">![Get it free](/media-draft/3-getitfree.png) (Skaffa det kostnadsfritt)</span><span class="sxs-lookup"><span data-stu-id="e1592-123">![Get it free](/media-draft/3-getitfree.png)</span></span>

5. <span data-ttu-id="e1592-124">Det här leder till marknadsplatssidan för WhiteSource Bolt.</span><span class="sxs-lookup"><span data-stu-id="e1592-124">This takes you to the WhiteSource Bolt Marketplace page.</span></span> <span data-ttu-id="e1592-125">Klicka på `Get it free`</span><span class="sxs-lookup"><span data-stu-id="e1592-125">Click `Get it free`</span></span>  
<span data-ttu-id="e1592-126">![Get White Source Bolt Free](/media-draft/3-getwhitesourceboltfree.png) (Skaffa WhiteSource Bolt kostnadsfritt)</span><span class="sxs-lookup"><span data-stu-id="e1592-126">![Get White Source Bolt Free](/media-draft/3-getwhitesourceboltfree.png)</span></span>

6. <span data-ttu-id="e1592-127">Välj din VSTS-organisation och klicka sedan på `Install`</span><span class="sxs-lookup"><span data-stu-id="e1592-127">Choose your VSTS organization and then click `Install`</span></span>  
<span data-ttu-id="e1592-128">![Install](/media-draft/3-install.png) (Installera)</span><span class="sxs-lookup"><span data-stu-id="e1592-128">![Install](/media-draft/3-install.png)</span></span>

7. <span data-ttu-id="e1592-129">Aktivera WhiteSource Bolt genom att följa anvisningarna här <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate></span><span class="sxs-lookup"><span data-stu-id="e1592-129">Activate WhiteSource Bolt by following the directions here <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate></span></span>

8. <span data-ttu-id="e1592-130">Gå tillbaka till Azure-portalen med DevOps-projektet inläst och klicka på länken för bygg-pipeline</span><span class="sxs-lookup"><span data-stu-id="e1592-130">Go back to your Azure portal with the DevOps project loaded and click the build pipeline link</span></span>  
![Länk för bygg-pipeline](/media-draft/3-buildpipelinelink.png)

9. <span data-ttu-id="e1592-132">Välj ditt bygge och klicka på `Edit`</span><span class="sxs-lookup"><span data-stu-id="e1592-132">Select your build and click `Edit`</span></span>  
<span data-ttu-id="e1592-133">![Edit Build](/media-draft/3-editbuild.png) (Redigera bygge)</span><span class="sxs-lookup"><span data-stu-id="e1592-133">![Edit Build](/media-draft/3-editbuild.png)</span></span>

10. <span data-ttu-id="e1592-134">Klicka på `+` för att lägga till en uppgift i agentjobb 1, skriv `bolt` i sökfältet och klicka på `Add`</span><span class="sxs-lookup"><span data-stu-id="e1592-134">Click the `+` Add a task to Agent job 1, type in `bolt` in the search field and click `Add`</span></span>  
<span data-ttu-id="e1592-135">![Add Bolt](/media-draft/3-addbolt.png) (Lägg till Bolt)</span><span class="sxs-lookup"><span data-stu-id="e1592-135">![Add Bolt](/media-draft/3-addbolt.png)</span></span>

11. <span data-ttu-id="e1592-136">Det här lägger till WhiteSource Bolt-uppgiften längst ned i uppgiftslistan; dra upp den längst upp</span><span class="sxs-lookup"><span data-stu-id="e1592-136">This adds the WhiteSource Bolt task to the bottom of the task list, drag it to the top</span></span>  
![Bolt längst upp](/media-draft/3-boltattop.png)

12. <span data-ttu-id="e1592-138">Klicka på `Save & queue` och välj `Save & queue`</span><span class="sxs-lookup"><span data-stu-id="e1592-138">Click `Save & queue` and select `Save & queue`</span></span>  
<span data-ttu-id="e1592-139">![Save and Queue](/media-draft/3-saveandqueue.png) (Spara och placera i kö)</span><span class="sxs-lookup"><span data-stu-id="e1592-139">![Save and Queue](/media-draft/3-saveandqueue.png)</span></span>

13. <span data-ttu-id="e1592-140">Klicka på `Save & queue`</span><span class="sxs-lookup"><span data-stu-id="e1592-140">Click `Save & queue`</span></span>  
<span data-ttu-id="e1592-141">![Dialogrutan Save and Queue](/media-draft/3-saveandqueuedialog.png) (Spara och placera i kö)</span><span class="sxs-lookup"><span data-stu-id="e1592-141">![Save and Queue Dialog](/media-draft/3-saveandqueuedialog.png)</span></span>

<span data-ttu-id="e1592-142">Det här sparar den ändrade bygg-pipelinen och placerar bygget i kö.</span><span class="sxs-lookup"><span data-stu-id="e1592-142">This saves the modified build pipeline and queues the build.</span></span> <span data-ttu-id="e1592-143">När bygget är klart kan du se i byggets `WhiteSource Bolt Build Report` att källkoden söktes igenom av WhiteSource Bolt för att hitta eventuella säkerhetsrisker.</span><span class="sxs-lookup"><span data-stu-id="e1592-143">After the build finishes, looking at the build `WhiteSource Bolt Build Report`, you can see the source code was scanned by WhiteSource Bolt looking for security vulnerabilities.</span></span>

![Byggrapport](/media-draft/3-buildreport.png)

## <a name="customize-the-release-pipeline"></a><span data-ttu-id="e1592-145">Anpassa versionspipelinen</span><span class="sxs-lookup"><span data-stu-id="e1592-145">Customize the release pipeline</span></span>

<span data-ttu-id="e1592-146">Liksom bygget är versionspipelinen en uppgiftskörare och kan anpassas på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="e1592-146">Like the build, the release pipeline is task runner and can be customized the same way.</span></span> <span data-ttu-id="e1592-147">För den här enheten läggar du till ett webbprestandatest i slutet av versionen.</span><span class="sxs-lookup"><span data-stu-id="e1592-147">For this unit, you will add a web performance test at the end of the release.</span></span> <span data-ttu-id="e1592-148">Det här verifierar att appen har distribuerats och körts i Kubernetes-klustret.</span><span class="sxs-lookup"><span data-stu-id="e1592-148">This will verifying that your app is deployed and running successfully in the Kubernetes cluster.</span></span>

1. <span data-ttu-id="e1592-149">Bläddra till DevOps-projektet i Azure-portalen och klicka på länken för versiondefinition</span><span class="sxs-lookup"><span data-stu-id="e1592-149">Browse to the DevOps project in the Azure Portal and click on the link for the release pipeline</span></span>  
![Versionslänk](/media-draft/3-releaselink.png)

2. <span data-ttu-id="e1592-151">Det här leder till sidan för versionspipelinen.</span><span class="sxs-lookup"><span data-stu-id="e1592-151">This takes you to the release pipeline page.</span></span> <span data-ttu-id="e1592-152">Klicka på releasepipelinen och klicka på `Edit`</span><span class="sxs-lookup"><span data-stu-id="e1592-152">Click your release pipeline and click on `Edit`</span></span>  
<span data-ttu-id="e1592-153">![Edit Release Pipeline](/media-draft/3-editreleasepipeline.png) (Redigera versionspipeline)</span><span class="sxs-lookup"><span data-stu-id="e1592-153">![Edit Release Pipeline](/media-draft/3-editreleasepipeline.png)</span></span>

3. <span data-ttu-id="e1592-154">Klicka på uppgifterna i `Dev`-steget i versionen</span><span class="sxs-lookup"><span data-stu-id="e1592-154">Click on the tasks in your release `Dev` stage</span></span>  
<span data-ttu-id="e1592-155">![Release Stage](/media-draft/3-releasestage.png) (Versionssteg)</span><span class="sxs-lookup"><span data-stu-id="e1592-155">![Release Stage](/media-draft/3-releasestage.png)</span></span>

4. <span data-ttu-id="e1592-156">Klicka på `+` för att lägga till en uppgift i Phase1 (Fas1), skriv `web test` i sökfältet och klicka på `Add` för den molnbaserade uppgiften Web Performance Test (Webbprestandatest)</span><span class="sxs-lookup"><span data-stu-id="e1592-156">Click on the `+` Add a task to Phase1, type `web test` in the search field and click `Add` for the Cloud-based Web Performance Test task</span></span>  
<span data-ttu-id="e1592-157">![Add Web Test](/media-draft/3-addwebtest.png) (Lägg till webbtest)</span><span class="sxs-lookup"><span data-stu-id="e1592-157">![Add Web Test](/media-draft/3-addwebtest.png)</span></span>

5. <span data-ttu-id="e1592-158">Redigera uppgiften Quick Web Performance Test (Snabbt webbprestandatest) genom att klicka på den och lägga till URL:en till din app i webbplats-URL:en (du hittar URL:en genom att gå till Azure-portalens DevOps-projektsida, högerklicka på den externa slutpunkten för exempelappen till höger och kopiera länken) och sedan ange `Ping Test` för TestName (Testnamn)</span><span class="sxs-lookup"><span data-stu-id="e1592-158">Edit the Quick Web Performance Test task by clicking on it and adding the url to your app in the Website URL (to find the url, go to the Azure portal DevOps project page and on the right-hand side, right-click your sample app external endpoint and copy link) and then for TestName, enter in `Ping Test`</span></span>  
<span data-ttu-id="e1592-159">![Copy URL](/media-draft/3-copyurl.png) (Kopiera URL)</span><span class="sxs-lookup"><span data-stu-id="e1592-159">![Copy URL](/media-draft/3-copyurl.png)</span></span>  
<span data-ttu-id="e1592-160">![Edit Web Test Task](/media-draft/3-editwebtesttask.png) (Redigera uppgiften Webbtestning)</span><span class="sxs-lookup"><span data-stu-id="e1592-160">![Edit Web Test Task](/media-draft/3-editwebtesttask.png)</span></span>

6. <span data-ttu-id="e1592-161">Klicka på `Save`</span><span class="sxs-lookup"><span data-stu-id="e1592-161">Click `Save`</span></span>  
<span data-ttu-id="e1592-162">![Save Release](/media-draft/3-saverelease.png) (Spara version)</span><span class="sxs-lookup"><span data-stu-id="e1592-162">![Save Release](/media-draft/3-saverelease.png)</span></span>

<span data-ttu-id="e1592-163">När du nu kör en version körs ett webbtest som besöker app-URL:en efter att det nya Helm-paketet har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="e1592-163">Now, when you run a release, after deploying the new helm package, a web test is run hitting the app url successfully.</span></span>

![Web Test (Webbtest)](/media-draft/3-webtest.png)


## <a name="summary"></a><span data-ttu-id="e1592-165">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e1592-165">Summary</span></span>

<span data-ttu-id="e1592-166">I den här enheten lärde du dig att anpassa dina bygg- och versionspipelines.</span><span class="sxs-lookup"><span data-stu-id="e1592-166">In this unit, you learned how to customize your build and release pipelines.</span></span> <span data-ttu-id="e1592-167">Du lärde dig även att installera och använda uppgifter från marknadsplatsen.</span><span class="sxs-lookup"><span data-stu-id="e1592-167">You also learned how to install and use tasks from the Marketplace.</span></span>