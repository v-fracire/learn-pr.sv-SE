<span data-ttu-id="62361-101">Det har alltid varit svårt att konfigurera en CI/CD-pipeline på ett snabbt sätt.</span><span class="sxs-lookup"><span data-stu-id="62361-101">Setting up a CI/CD pipeline has always been challenging to do quickly.</span></span> <span data-ttu-id="62361-102">Nu är det otroligt lätt att börja från noll och skapa ett fullständigt Azure DevOps-projekt från slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="62361-102">Now, it is incredibly easy to go from nothing at all to a full end to end Azure DevOps project.</span></span> <span data-ttu-id="62361-103">Och i Azure DevOps-projektet får du följande:</span><span class="sxs-lookup"><span data-stu-id="62361-103">And in the Azure DevOps project, you get the following:</span></span>

1. <span data-ttu-id="62361-104">Infrastruktur som etablerats åt dig i Azure.</span><span class="sxs-lookup"><span data-stu-id="62361-104">Infrastructure provisioned for you in Azure.</span></span>

2. <span data-ttu-id="62361-105">Ett teamprojekt i en VSTS-instans.</span><span class="sxs-lookup"><span data-stu-id="62361-105">A Team Project in a VSTS instance.</span></span>

3. <span data-ttu-id="62361-106">Källkoden för en exempelapp på det språk som du valde i lagringsplatsen för VSTS-instansen.</span><span class="sxs-lookup"><span data-stu-id="62361-106">Source code for a sample app in the language that you picked in the repo of your VSTS instance.</span></span>

4. <span data-ttu-id="62361-107">En bygg- och versionspipeline som passar för de tekniker som valts.</span><span class="sxs-lookup"><span data-stu-id="62361-107">A build and release pipeline that makes sense for the technologies picked.</span></span>

<span data-ttu-id="62361-108">Och när det är klart bygger och lanserar Azure DevOps-projektet exempelappen via pipelines i den infrastruktur som det etablerar i Azure.</span><span class="sxs-lookup"><span data-stu-id="62361-108">And when its's done, the Azure DevOps project takes the sample app, builds and releases it through the pipelines into the infrastructure it provisions for you up in Azure.</span></span> <span data-ttu-id="62361-109">Du får allt detta med ett par klick.</span><span class="sxs-lookup"><span data-stu-id="62361-109">And you get all of this with just a couple of clicks.</span></span>

## <a name="create-an-azure-devops-project"></a><span data-ttu-id="62361-110">Skapa ett Azure DevOps-projekt</span><span class="sxs-lookup"><span data-stu-id="62361-110">Create an Azure DevOps project</span></span>

<span data-ttu-id="62361-111">Du skapar ett Azure DevOps Project i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="62361-111">You create an Azure DevOps project from the Azure portal.</span></span>

1. <span data-ttu-id="62361-112">Gå till [Azure-portalen](https://portal.azure.com) och klicka på `Create a resource`</span><span class="sxs-lookup"><span data-stu-id="62361-112">Head to the [Azure Portal](https://portal.azure.com) and click `Create a resource`</span></span>  
![](/media-draft/1-azureportal.png)

2. <span data-ttu-id="62361-113">Klicka på `DevOps Project`</span><span class="sxs-lookup"><span data-stu-id="62361-113">Click `DevOps Project`</span></span>  
<span data-ttu-id="62361-114">![Välja DevOps-projekt](/media-draft/1-pickdevopsproject.png)</span><span class="sxs-lookup"><span data-stu-id="62361-114">![Pick DevOps Project](/media-draft/1-pickdevopsproject.png)</span></span>

3. <span data-ttu-id="62361-115">På nästa skärm väljer du vilket språk som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="62361-115">The next screen is where you get to pick what language you want to use.</span></span> <span data-ttu-id="62361-116">Observera att du kan välja .NET (naturligtvis), Java, Node, PHP, Python, Ruby och Go.</span><span class="sxs-lookup"><span data-stu-id="62361-116">Notice how you can choose .NET (of course), java, node, php, python, ruby, and go.</span></span> <span data-ttu-id="62361-117">Du kan även importera din egen kod från en Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="62361-117">You can even bring your own code in from a git repo.</span></span> <span data-ttu-id="62361-118">För den här enheten skapar vi en Node.js-app.</span><span class="sxs-lookup"><span data-stu-id="62361-118">For this unit, let’s go ahead and create a Node.js app.</span></span> <span data-ttu-id="62361-119">Klicka på Node.js och sedan på Next (Nästa)</span><span class="sxs-lookup"><span data-stu-id="62361-119">Click Node.js and click Next</span></span>  
![Välj Node.js som språk](/media-draft/1-picknodejsforlang.png)

4. <span data-ttu-id="62361-121">Sedan får du frågan om vilket Node.js-ramverk du vill använda.</span><span class="sxs-lookup"><span data-stu-id="62361-121">Next it's going to ask you what node.js framework you want to use.</span></span> <span data-ttu-id="62361-122">För den här enheten väljer du Simple Node.js app (Enkel Node.js-app) och klickar på nästa</span><span class="sxs-lookup"><span data-stu-id="62361-122">For this unit, pick Simple Node.js app and click Next</span></span>  
![Välj Simple Node (Enkel Nod)](/media-draft/1-picksimplenode.png)

5. <span data-ttu-id="62361-124">Sedan ombes du välja vilken infrastruktur som du vill etablera och köra appen i.</span><span class="sxs-lookup"><span data-stu-id="62361-124">Next, it's going to ask you what infrastructure do you want to provision and run your app in?</span></span> <span data-ttu-id="62361-125">För den här enheten etablerar och distribuerar vi till ett Kubernetes-kluster med hjälp av Azure Kubernetes Service.</span><span class="sxs-lookup"><span data-stu-id="62361-125">For this unit, let’s provision and deploy into a Kubernetes cluster using Azure Kubernetes Service.</span></span> <span data-ttu-id="62361-126">Välj Kubernetes Service (Kubernetes-tjänst) och klicka på Next (Nästa)</span><span class="sxs-lookup"><span data-stu-id="62361-126">Select Kubernetes Service and click Next</span></span>  
![Välj Kubernetes](/media-draft/1-pickkubernetes.png)

6. <span data-ttu-id="62361-128">Nu kan du antingen skapa en helt ny instans av VSTS eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="62361-128">And now, you can either create a brand new instance of VSTS or choose an existing one.</span></span> <span data-ttu-id="62361-129">Du kan även konfigurera var och hur du vill att Kubernetes-klustret ska köras.</span><span class="sxs-lookup"><span data-stu-id="62361-129">You also get to set up where and how you want your kubernetes cluster to run.</span></span> <span data-ttu-id="62361-130">För den här enheten skapar du en ny VSTS-instans genom att välja Select new (Välj ny) och ger VSTS-instansen ett nytt namn.</span><span class="sxs-lookup"><span data-stu-id="62361-130">For this unit, go ahead and create a new VSTS instance by choosing Select new and give your VSTS instance a unique name.</span></span> <span data-ttu-id="62361-131">Ange Learn som projektnamn, välj din Azure-prenumeration, ge klustret namnet LearnCluster, välj USA, östra för platsen och klicka på Done (Klar)</span><span class="sxs-lookup"><span data-stu-id="62361-131">Enter Learn for the Project name, pick your azure subscription, name the Cluster Name LearnCluster, select East US for the location, and click Done</span></span>  
![Skärmen för slutlig bekräftelse](/media-draft/1-finalconfirmation.png)

<span data-ttu-id="62361-133">Nu är det klart!</span><span class="sxs-lookup"><span data-stu-id="62361-133">And that is literally it!</span></span> <span data-ttu-id="62361-134">Detta tar en stund, så nu behöver du bara låta Azure slutföra processen.</span><span class="sxs-lookup"><span data-stu-id="62361-134">This takes a while so kick back, relax and just let Azure do its thing.</span></span> <span data-ttu-id="62361-135">Den största delen av tiden går åt till att etablera och konfigurera Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="62361-135">Most of the time will be spent provisioning and configuring your Azure infrastructure.</span></span> <span data-ttu-id="62361-136">För den här modulen blir det här din Azure Kubernetes Service och ditt Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="62361-136">For this module, this will be your Azure Kubernetes Service and Azure Container Registry.</span></span>

## <a name="a-lap-around-the-finished-azure-devops-project"></a><span data-ttu-id="62361-137">Ett varv runt det färdiga Azure DevOps-projektet</span><span class="sxs-lookup"><span data-stu-id="62361-137">A lap around the finished Azure DevOps Project</span></span>

<span data-ttu-id="62361-138">När Azure är klart meddelas du i dina aviseringar</span><span class="sxs-lookup"><span data-stu-id="62361-138">When Azure is done, you will be notified in your Alerts</span></span>

1. <span data-ttu-id="62361-139">Klicka på aviseringen och sedan på Go to resource (Gå till resurs)</span><span class="sxs-lookup"><span data-stu-id="62361-139">Click on the alert and then Go to resource</span></span>  
![Gå till resurs från avisering](/media-draft/1-gotoresourcefromalert.png)

2. <span data-ttu-id="62361-141">Detta tar dig till ett portalblad som visar allt som etableras.</span><span class="sxs-lookup"><span data-stu-id="62361-141">This takes you to a portal blade that displays everything provisioned.</span></span> <span data-ttu-id="62361-142">På vänster sida finns din CI/CD-pipeline.</span><span class="sxs-lookup"><span data-stu-id="62361-142">On the left-hand side is your CI/CD pipeline.</span></span> <span data-ttu-id="62361-143">Där har du din kodlagringsplats, bygg-definition och versionsdefinition.</span><span class="sxs-lookup"><span data-stu-id="62361-143">You have your code repository, your build definition, and also your release definition.</span></span> <span data-ttu-id="62361-144">Alla länkar är djuplänkar som leder direkt till resursen i VSTS.</span><span class="sxs-lookup"><span data-stu-id="62361-144">All the links are deep links that take you directly into the resource in VSTS.</span></span> <span data-ttu-id="62361-145">På höger sida visas all infrastruktur som har distribuerats i Azure.</span><span class="sxs-lookup"><span data-stu-id="62361-145">And on the right-hand side, you see all the infrastructure deployed for you in Azure.</span></span> <span data-ttu-id="62361-146">Där finns Kubernetes-klustret med din exempelapp som redan har distribuerats och en programinsikt.</span><span class="sxs-lookup"><span data-stu-id="62361-146">You have your Kubernetes cluster with your sample app already deployed and also application insight.</span></span> <span data-ttu-id="62361-147">Alla dessa länkar är alltså djuplänkar till resurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="62361-147">Once again, all these links are deep links to the resources in Azure.</span></span>  
![DevOps-projekt i portalen](/media-draft/1-pickdevopsproject.png)

3. <span data-ttu-id="62361-149">Klicka på länken för din källkod</span><span class="sxs-lookup"><span data-stu-id="62361-149">Click on the link for your source code</span></span>  
![Länk till källan](/media-draft/1-linktosource.png)

4. <span data-ttu-id="62361-151">Detta tar dig till Git-lagringsplatsen i VSTS-projektet.</span><span class="sxs-lookup"><span data-stu-id="62361-151">This takes you to the git repo in your VSTS project.</span></span> <span data-ttu-id="62361-152">Observera att det här bara är en normal Git-lagringsplats som innehåller Node.js-exempelappen med Helm-diagram.</span><span class="sxs-lookup"><span data-stu-id="62361-152">Notice this is just a normal git repo holding the sample node.js app with helm charts.</span></span>  
![VSTS-lagringsplats](/media-draft/1-vstsrepo.png)

5. <span data-ttu-id="62361-154">Gå till byggena</span><span class="sxs-lookup"><span data-stu-id="62361-154">Go into the builds</span></span>  
![Länk till byggen](/media-draft/1-navtobuild.png)

6. <span data-ttu-id="62361-156">Redigera det skapade bygget genom att klicka på bygget och sedan klicka på Redigera</span><span class="sxs-lookup"><span data-stu-id="62361-156">Edit the created build by clicking on the build and then click Edit</span></span>  
![](/media-draft/1-editbuildlink.png)

7. <span data-ttu-id="62361-157">Det visas en bygg-pipeline som passar för de tekniker som valts.</span><span class="sxs-lookup"><span data-stu-id="62361-157">You will see a build pipeline that makes sense for the technologies picked.</span></span> <span data-ttu-id="62361-158">Eftersom vi valde en Node.js-app till ett Kubernetes-kluster får vi en bygg-pipeline som använder Docker-uppgifter för att bygga Node.js-appen och skapa containeravbildningen samt Helm-uppgifter för att skapa ett Helm-paket.</span><span class="sxs-lookup"><span data-stu-id="62361-158">Since we picked a node.js app into a Kubernetes cluster, we get a build pipeline that uses Docker tasks to build the Node.js app, create the image container image and then Helm tasks to create a Helm package.</span></span>  
![Bygg-pipeline](/media-draft/1-buildpipeline.png)

8. <span data-ttu-id="62361-160">Gå till versionspipelinen genom att klicka på `Releases`</span><span class="sxs-lookup"><span data-stu-id="62361-160">Go to the Release pipeline by clicking `Releases`</span></span>  
<span data-ttu-id="62361-161">![Go to Releases](/media-draft/1-gotoreleases.png) (Gå till versioner)</span><span class="sxs-lookup"><span data-stu-id="62361-161">![Go to Releases](/media-draft/1-gotoreleases.png)</span></span>

9. <span data-ttu-id="62361-162">Versionspipelinen skapas.</span><span class="sxs-lookup"><span data-stu-id="62361-162">You'll see the release pipeline created.</span></span> <span data-ttu-id="62361-163">Redigera den genom att klicka på versionen och välja `Edit pipeline`</span><span class="sxs-lookup"><span data-stu-id="62361-163">Edit it by clicking on the release and selecting `Edit pipeline`</span></span>  
<span data-ttu-id="62361-164">![Redigera version](/media-draft/1-editrelease.png)</span><span class="sxs-lookup"><span data-stu-id="62361-164">![Edit Release](/media-draft/1-editrelease.png)</span></span>

10. <span data-ttu-id="62361-165">Azure har skapat en versionspipeline som passar för de tekniker som valts.</span><span class="sxs-lookup"><span data-stu-id="62361-165">Azure has created a release pipeline that makes sense for the technologies picked.</span></span> <span data-ttu-id="62361-166">En node-app som körs i en container i ett Kubernetes-kluster.</span><span class="sxs-lookup"><span data-stu-id="62361-166">A node app running in a container hosted in a Kubernetes cluster.</span></span>
<span data-ttu-id="62361-167">![Versionspipeline](/media-draft/1-releasepipeline.png)</span><span class="sxs-lookup"><span data-stu-id="62361-167">![Release Pipeline](/media-draft/1-releasepipeline.png)</span></span>

11. <span data-ttu-id="62361-168">Gå tillbaka till Azure-portalen och klicka på den externa slutpunkten för Kubernetes-tjänsten</span><span class="sxs-lookup"><span data-stu-id="62361-168">Go back to the Azure portal and click on the external endpoint for the kubernetes service</span></span>  
![](/media-draft/1-clickonendpoint.png)

12. <span data-ttu-id="62361-169">Du bör se exempelappen byggas och distribueras i AKS-klustret</span><span class="sxs-lookup"><span data-stu-id="62361-169">You should see the sample app build and deployed into your AKS cluster</span></span>  
![Appen körs](/media-draft/1-apprunning.png)

## <a name="summary"></a><span data-ttu-id="62361-171">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="62361-171">Summary</span></span>

<span data-ttu-id="62361-172">I den här enheten skapade du ett Azure DevOps-projekt som består av:</span><span class="sxs-lookup"><span data-stu-id="62361-172">In this unit, you created an Azure DevOps project that consists of:</span></span>

1. <span data-ttu-id="62361-173">Infrastruktur för appen – Azure Kubernetes Service och Application Insights</span><span class="sxs-lookup"><span data-stu-id="62361-173">Infrastructure for your app - Azure Kubernetes Service and Application Insight</span></span>

2. <span data-ttu-id="62361-174">Ett teamprojekt i en VSTS-instans.</span><span class="sxs-lookup"><span data-stu-id="62361-174">A Team Project in a VSTS instance.</span></span>

3. <span data-ttu-id="62361-175">Källkoden för en Node.js-exempelapp som körs i en container i lagringsplatsen för din VSTS-instans.</span><span class="sxs-lookup"><span data-stu-id="62361-175">Source code for a Node.js sample app running in a container in the repo of your VSTS instance.</span></span>

4. <span data-ttu-id="62361-176">En bygg- och versionspipeline för en Node.js-containerapp som körs i din Azure Kubernetes Service-instans.</span><span class="sxs-lookup"><span data-stu-id="62361-176">A build and release pipeline for a Node.js container app running in your Azure Kubernetes Service instance.</span></span>

<span data-ttu-id="62361-177">I nästa steg lär du dig hur du ersätter exempelappen med din verkliga app.</span><span class="sxs-lookup"><span data-stu-id="62361-177">Next, learn how to replace the sample app with your real app.</span></span>