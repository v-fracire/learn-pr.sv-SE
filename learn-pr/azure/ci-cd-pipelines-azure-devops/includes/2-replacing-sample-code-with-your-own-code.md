<span data-ttu-id="dc461-101">När du har skapat ett Azure DevOps-projekt är den första frågan oftast hur du byter ut exempelappen mot din egen app?</span><span class="sxs-lookup"><span data-stu-id="dc461-101">After creating an Azure DevOps project, the first thing everyone ask is how do you replace the sample app with your own app?</span></span> <span data-ttu-id="dc461-102">Det är ganska enkelt, och i den här lektionen får du lära dig två sätt att göra det.</span><span class="sxs-lookup"><span data-stu-id="dc461-102">It's quite simple and in this unit, you will learn two ways to do it.</span></span>

1. <span data-ttu-id="dc461-103">Byt ut koden i VSTS git-lagringsplatsen mot den riktiga koden</span><span class="sxs-lookup"><span data-stu-id="dc461-103">Replacing the code in the VSTS git repository with your real code</span></span>

2. <span data-ttu-id="dc461-104">Peka om pipelinen för byggen till en extern git-lagringsplats som innehåller den riktiga koden</span><span class="sxs-lookup"><span data-stu-id="dc461-104">Pointing your build pipeline to an external git repo holding your real code</span></span>

## <a name="replacing-code-in-vsts-git-repository"></a><span data-ttu-id="dc461-105">Byta ut koden i VSTS git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="dc461-105">Replacing code in VSTS git repository</span></span>

<span data-ttu-id="dc461-106">Ett enkelt sätt är att klona VSTS git-lagringsplatsen till hårddisken så att du byter ut allt mot din egen kod, och sedan ladda upp allt till VSTS igen.</span><span class="sxs-lookup"><span data-stu-id="dc461-106">One simple way is by cloning the git repo in VSTS onto your hard drive, replacing everything with your own code, uploading back to VSTS and voila.</span></span> <span data-ttu-id="dc461-107">Koden ska nu kompileras och distribueras via CI/CD-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="dc461-107">Your code will now be built and deployed through the CI/CD pipeline.</span></span>

<span data-ttu-id="dc461-108">I den här lektionen börjar du genom att ladda ned och lagra källkoden för node.js-appen på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="dc461-108">For this unit, you will start by downloading and storing the source code to the node.js app onto your hard drive.</span></span>

1. <span data-ttu-id="dc461-109">Ladda ned källkoden från <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip></span><span class="sxs-lookup"><span data-stu-id="dc461-109">Download the source code from <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip></span></span>

2. <span data-ttu-id="dc461-110">Extrahera innehållet i MicrosoftLearnDevOps.zip någonstans på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="dc461-110">Extract the contents of MicrosoftLearnDevOps.zip somewhere on your hard drive.</span></span> <span data-ttu-id="dc461-111">I det här exemplet använder vi `C:\users\abel\Downloads\MicrosoftLearnDevOps`.</span><span class="sxs-lookup"><span data-stu-id="dc461-111">For this example `C:\users\abel\Downloads\MicrosoftLearnDevOps` was used</span></span>  
<span data-ttu-id="dc461-112">![Uppackad katalog](/media-draft/2-unzippedfolder.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-112">![Unzipped Directory](/media-draft/2-unzippedfolder.png)</span></span>

<span data-ttu-id="dc461-113">Sedan måste du klona lagringsplatsen till hårddisken och byta ut exempelappen med den riktiga node.js-appen.</span><span class="sxs-lookup"><span data-stu-id="dc461-113">Next, you need to clone the repo onto your hard drive and replace the sample app with the real node.js app.</span></span> <span data-ttu-id="dc461-114">I den här lektionen förutsätter vi att du redan har git installerat i datorn.</span><span class="sxs-lookup"><span data-stu-id="dc461-114">This unit assumes you already have git installed on your computer.</span></span>

1. <span data-ttu-id="dc461-115">Bläddra till Azure DevOps-projektet i Azure Portal och klicka på länken till lagringsplatsen med koden.</span><span class="sxs-lookup"><span data-stu-id="dc461-115">From Azure portal browse to your Azure DevOps Project and click on the code repository link.</span></span>  
![](/media-draft/2-browsetorepolink.png)

2. <span data-ttu-id="dc461-116">Klicka på Klona och kopiera git-lagringsplatsens webbadress uppe till höger på sidan.</span><span class="sxs-lookup"><span data-stu-id="dc461-116">Click on Clone and copy the url for the git repo in the upper right-hand side.</span></span>  
![Kopiera webbadressen för kloning](/media-draft/2-copycloneurl.png)  
<span data-ttu-id="dc461-118">Med det här kopierar du lagringsplatsens webbadress till Urklipp</span><span class="sxs-lookup"><span data-stu-id="dc461-118">This will copy the repo url to your clipboard</span></span>

3. <span data-ttu-id="dc461-119">Klona lagringsplatsen till hårddisken</span><span class="sxs-lookup"><span data-stu-id="dc461-119">Clone the repo to your hard drive</span></span>  
![Git-klon](/media-draft/2-gitclone.png)  
<span data-ttu-id="dc461-121">I det här exemplet klonades lagringsplatsen till C:\Users\abel\Source\TripleCrown\DevOps</span><span class="sxs-lookup"><span data-stu-id="dc461-121">In this example the repo was cloned to C:\Users\abel\Source\TripleCrown\DevOps</span></span>

4. <span data-ttu-id="dc461-122">Ta bort allt från den lokala lagringsplatsen förutom katalogen `.git`</span><span class="sxs-lookup"><span data-stu-id="dc461-122">Delete everything from your local repo except for the `.git` directory</span></span>  
<span data-ttu-id="dc461-123">![Ta bort allt från lagringsplatsen](/media-draft/2-deleterepoofeverything.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-123">![Delete Repo of Everything](/media-draft/2-deleterepoofeverything.png)</span></span>

5. <span data-ttu-id="dc461-124">Kopiera källkoden för den nedladdade node.js-appen till mappen för lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="dc461-124">Copy the source code for the downloaded node.js app into the repo folder</span></span>  
![Utbytt kod](/media-draft/2-replacedeverything.png)

6. <span data-ttu-id="dc461-126">Lägg till alla ändringar i den lokala lagringsplatsen genom att skriva `git add *` på kommandoraden</span><span class="sxs-lookup"><span data-stu-id="dc461-126">Add all the changes to your local repo by typing `git add *` from the command line</span></span>  
<span data-ttu-id="dc461-127">![Git Add för allt](/media-draft/2-gitaddall.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-127">![Git Add All](/media-draft/2-gitaddall.png)</span></span>

7. <span data-ttu-id="dc461-128">Checka in ändringar i din lokala lagringsplats genom att skriva `git commit -m "replace sample app with real code"`</span><span class="sxs-lookup"><span data-stu-id="dc461-128">Commit changes to your local repo by typing `git commit -m "replace sample app with real code"`</span></span>  
<span data-ttu-id="dc461-129">![Checka in Git](/media-draft/2-gitcommit.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-129">![Git Commit](/media-draft/2-gitcommit.png)</span></span>

8. <span data-ttu-id="dc461-130">Push-överför ändringarna till git-lagringsplatsen i VSTS med `git push`</span><span class="sxs-lookup"><span data-stu-id="dc461-130">Push changes back to the git repo in VSTS with `git push`</span></span>  
<span data-ttu-id="dc461-131">![push-överför till Git](/media-draft/2-gitpush.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-131">![Git Push](/media-draft/2-gitpush.png)</span></span>

9. <span data-ttu-id="dc461-132">När du har skickat ändringarna till VSTS ska den faktiska appkoden genomgå pipelinen för kompilering</span><span class="sxs-lookup"><span data-stu-id="dc461-132">After pushing changes back to VSTS, this should send the real app code through the build</span></span>  
<span data-ttu-id="dc461-133">![Kompilering har startats](/media-draft/2-buildkickedoff.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-133">![Build Kicked Off](/media-draft/2-buildkickedoff.png)</span></span>  
<span data-ttu-id="dc461-134">![Kompilering pågår](/media-draft/2-buildinaction.png) och lansering hela vägen till Azure</span><span class="sxs-lookup"><span data-stu-id="dc461-134">![Build in Action](/media-draft/2-buildinaction.png) and release pipeline all the way to azure</span></span>  
 <span data-ttu-id="dc461-135">![Lansering pågår](/media-draft/2-releaserunning.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-135">![Release Running](/media-draft/2-releaserunning.png)</span></span>

 <span data-ttu-id="dc461-136">När distributionen är färdig kan du kontrollera att den riktiga appen har distribuerats genom att gå tillbaka till Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dc461-136">After the deployment finishes, you can verify the real app was deployed by going back to the Azure portal</span></span>

 1. <span data-ttu-id="dc461-137">Gå till Azure Portal, bläddra till Azure DevOps-projektet och klicka på den distribuerade appen till höger</span><span class="sxs-lookup"><span data-stu-id="dc461-137">Go to the Azure portal, browse to your Azure DevOps project, and click on your deployed app on the right-hand side</span></span>  
 ![Länk för att starta exempelapp](/media-draft/2-launchapp.png)

 2. <span data-ttu-id="dc461-139">Det här startar appen som körs i din webbläsare</span><span class="sxs-lookup"><span data-stu-id="dc461-139">This launches the running app in your browser</span></span>  
 ![Appen körs](/media-draft/2-apprunning.png)

## <a name="using-external-git-repo"></a><span data-ttu-id="dc461-141">Använda en extern git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="dc461-141">Using external git repo</span></span>

<span data-ttu-id="dc461-142">Ett annat sätt att byta ut exempelappen mot den riktiga appkoden är att peka om pipelinen för kompilering mot en extern git-lagringsplats som innehåller din appkod.</span><span class="sxs-lookup"><span data-stu-id="dc461-142">Another way to swap out the sample app with your real app code is by pointing the build pipeline to an external git repository that holds your app code.</span></span> <span data-ttu-id="dc461-143">I det här exemplet laddar du upp den riktiga appkoden till en lagringsplats på github.</span><span class="sxs-lookup"><span data-stu-id="dc461-143">For this example, upload the real app code to a github repository.</span></span>

<span data-ttu-id="dc461-144">När du har laddat upp den riktiga koden till github gör du så här för att peka om pipelinen till github-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="dc461-144">After uploading the real code to github, do the following to point the build pipeline to this github repository</span></span>

1. <span data-ttu-id="dc461-145">Bläddra till Azure DevOps-projektet i Azure Portal och klicka på kompileringslänken</span><span class="sxs-lookup"><span data-stu-id="dc461-145">From the Azure portal, browse to your Azure DevOps project and click on the build link</span></span>  
![Komplieringslänk](/media-draft/2-buildlink.png)

2. <span data-ttu-id="dc461-147">Nu kommer du till dina pipelines för kompilering. Klicka på pipelinen och sedan på `Edit`</span><span class="sxs-lookup"><span data-stu-id="dc461-147">This takes you to the build pipelines, click your build pipeline and then `Edit`</span></span>  
<span data-ttu-id="dc461-148">![Klicka på redigera kompilering](/media-draft/2-clickeditbuildlink.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-148">![Click Edit Build](/media-draft/2-clickeditbuildlink.png)</span></span>

3. <span data-ttu-id="dc461-149">Nu öppnas kompileringsredigeraren, klicka på `Get sources`</span><span class="sxs-lookup"><span data-stu-id="dc461-149">This takes you to the build editor, click on `Get sources`</span></span>  
<span data-ttu-id="dc461-150">![Klicka på Hämta källkod](/media-draft/2-clickgetsource.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-150">![Click Get Source](/media-draft/2-clickgetsource.png)</span></span>

4. <span data-ttu-id="dc461-151">Nu kommer du till sidan Välj källa.</span><span class="sxs-lookup"><span data-stu-id="dc461-151">This takes you to the Select a source page.</span></span> <span data-ttu-id="dc461-152">Observera att du inte behöver använda VSTS Git, utan även kan använda GitHub, GitHub Enterprise, Subversion, Bitbucket Cloud eller någon annan extern Git-baserad lagringsplats i pipelinen för kompilering.</span><span class="sxs-lookup"><span data-stu-id="dc461-152">Notice how you can not only use VSTS Git, but also GitHub, GitHub Enterprise, Subversion, Bitbucket Cloud, and any external Git based repo for the build pipeline.</span></span> <span data-ttu-id="dc461-153">I den här övningen väljer du GitHub</span><span class="sxs-lookup"><span data-stu-id="dc461-153">For this exercise, select GitHub</span></span>  
![Välj GitHub](/media-draft/2-selectgithub.png)

5. <span data-ttu-id="dc461-155">Nu kommer du till anslutningssidan för GitHub.</span><span class="sxs-lookup"><span data-stu-id="dc461-155">This takes you to the GitHub connection page.</span></span> <span data-ttu-id="dc461-156">Använd antingen OAuth eller en personlig åtkomsttoken för att ansluta till ditt GitHub-konto</span><span class="sxs-lookup"><span data-stu-id="dc461-156">Either use OAuth or a Personal Access Token to connect with your GitHub account</span></span>

6. <span data-ttu-id="dc461-157">Välj github-lagringsplatsen som innehåller den riktiga koden och klicka på `Save & queue`</span><span class="sxs-lookup"><span data-stu-id="dc461-157">Select the github repo holding the real app code and click `Save & queue`</span></span>  
<span data-ttu-id="dc461-158">![Spara och placera i kö](/media-draft/2-saveandqueue.png)</span><span class="sxs-lookup"><span data-stu-id="dc461-158">![Save and Queue](/media-draft/2-saveandqueue.png)</span></span>

7. <span data-ttu-id="dc461-159">Klicka på knappen `Save & queue`</span><span class="sxs-lookup"><span data-stu-id="dc461-159">Click the `Save & queue` button</span></span>  
![](/media-draft/2-saveandqueuedialog.png)

8. <span data-ttu-id="dc461-160">Den här åtgärden sparar och sätter igång kompileringen så att den riktiga appkoden skickas via våra pipelines för kompilering och lansering hela vägen till Azure</span><span class="sxs-lookup"><span data-stu-id="dc461-160">This action saves and kicks off the build, sending the real app code hosted in GitHub through our build and release pipelines all the way to Azure</span></span>  
![Kompilering pågår](/media-draft/2-buildrunning.png)

## <a name="summary"></a><span data-ttu-id="dc461-162">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="dc461-162">Summary</span></span>

<span data-ttu-id="dc461-163">I den här lektionen har du lärt dig två olika sätt att ersätta exempelkoden i DevOps-projektet med den riktiga appkoden.</span><span class="sxs-lookup"><span data-stu-id="dc461-163">In this unit, you learned two different ways to replace the sample code in the DevOps project with real app code.</span></span> <span data-ttu-id="dc461-164">Det kan du göra genom att ersätta koden i VSTS git-lagringsplatsen eller genom att länka pipelinen för kompilering till en annan extern lagringsplats som innehåller din appkod.</span><span class="sxs-lookup"><span data-stu-id="dc461-164">This can be done either by replacing the code in the VSTS git repo, or by linking the build pipeline with another external repo which holds your app code.</span></span>

<span data-ttu-id="dc461-165">I nästa lektion får du lära dig att anpassa dina pipelines för kompilering och lansering.</span><span class="sxs-lookup"><span data-stu-id="dc461-165">Next learn how to customize the build and release pipelines.</span></span>