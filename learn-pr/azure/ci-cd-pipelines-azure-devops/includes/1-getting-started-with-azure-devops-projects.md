Det har alltid varit svårt att konfigurera en CI/CD-pipeline på ett snabbt sätt. Nu är det otroligt lätt att börja från noll och skapa ett fullständigt Azure DevOps-projekt från slutpunkt till slutpunkt. Och i Azure DevOps-projektet får du följande:

1. Infrastruktur som etablerats åt dig i Azure.

2. Ett teamprojekt i en VSTS-instans.

3. Källkoden för en exempelapp på det språk som du valde i lagringsplatsen för VSTS-instansen.

4. En bygg- och versionspipeline som passar för de tekniker som valts.

Och när det är klart bygger och lanserar Azure DevOps-projektet exempelappen via pipelines i den infrastruktur som det etablerar i Azure. Du får allt detta med ett par klick.

## <a name="create-an-azure-devops-project"></a>Skapa ett Azure DevOps-projekt

Du skapar ett Azure DevOps Project i Azure-portalen.

1. Gå till [Azure-portalen](https://portal.azure.com) och klicka på `Create a resource`  
![](/media-draft/1-azureportal.png)

2. Klicka på `DevOps Project`  
![Välja DevOps-projekt](/media-draft/1-pickdevopsproject.png)

3. På nästa skärm väljer du vilket språk som du vill använda. Observera att du kan välja .NET (naturligtvis), Java, Node, PHP, Python, Ruby och Go. Du kan även importera din egen kod från en Git-lagringsplats. För den här enheten skapar vi en Node.js-app. Klicka på Node.js och sedan på Next (Nästa)  
![Välj Node.js som språk](/media-draft/1-picknodejsforlang.png)

4. Sedan får du frågan om vilket Node.js-ramverk du vill använda. För den här enheten väljer du Simple Node.js app (Enkel Node.js-app) och klickar på nästa  
![Välj Simple Node (Enkel Nod)](/media-draft/1-picksimplenode.png)

5. Sedan ombes du välja vilken infrastruktur som du vill etablera och köra appen i. För den här enheten etablerar och distribuerar vi till ett Kubernetes-kluster med hjälp av Azure Kubernetes Service. Välj Kubernetes Service (Kubernetes-tjänst) och klicka på Next (Nästa)  
![Välj Kubernetes](/media-draft/1-pickkubernetes.png)

6. Nu kan du antingen skapa en helt ny instans av VSTS eller välj en befintlig. Du kan även konfigurera var och hur du vill att Kubernetes-klustret ska köras. För den här enheten skapar du en ny VSTS-instans genom att välja Select new (Välj ny) och ger VSTS-instansen ett nytt namn. Ange Learn som projektnamn, välj din Azure-prenumeration, ge klustret namnet LearnCluster, välj USA, östra för platsen och klicka på Done (Klar)  
![Skärmen för slutlig bekräftelse](/media-draft/1-finalconfirmation.png)

Nu är det klart! Detta tar en stund, så nu behöver du bara låta Azure slutföra processen. Den största delen av tiden går åt till att etablera och konfigurera Azure-infrastrukturen. För den här modulen blir det här din Azure Kubernetes Service och ditt Azure Container Registry.

## <a name="a-lap-around-the-finished-azure-devops-project"></a>Ett varv runt det färdiga Azure DevOps-projektet

När Azure är klart meddelas du i dina aviseringar

1. Klicka på aviseringen och sedan på Go to resource (Gå till resurs)  
![Gå till resurs från avisering](/media-draft/1-gotoresourcefromalert.png)

2. Detta tar dig till ett portalblad som visar allt som etableras. På vänster sida finns din CI/CD-pipeline. Där har du din kodlagringsplats, bygg-definition och versionsdefinition. Alla länkar är djuplänkar som leder direkt till resursen i VSTS. På höger sida visas all infrastruktur som har distribuerats i Azure. Där finns Kubernetes-klustret med din exempelapp som redan har distribuerats och en programinsikt. Alla dessa länkar är alltså djuplänkar till resurserna i Azure.  
![DevOps-projekt i portalen](/media-draft/1-pickdevopsproject.png)

3. Klicka på länken för din källkod  
![Länk till källan](/media-draft/1-linktosource.png)

4. Detta tar dig till Git-lagringsplatsen i VSTS-projektet. Observera att det här bara är en normal Git-lagringsplats som innehåller Node.js-exempelappen med Helm-diagram.  
![VSTS-lagringsplats](/media-draft/1-vstsrepo.png)

5. Gå till byggena  
![Länk till byggen](/media-draft/1-navtobuild.png)

6. Redigera det skapade bygget genom att klicka på bygget och sedan klicka på Redigera  
![](/media-draft/1-editbuildlink.png)

7. Det visas en bygg-pipeline som passar för de tekniker som valts. Eftersom vi valde en Node.js-app till ett Kubernetes-kluster får vi en bygg-pipeline som använder Docker-uppgifter för att bygga Node.js-appen och skapa containeravbildningen samt Helm-uppgifter för att skapa ett Helm-paket.  
![Bygg-pipeline](/media-draft/1-buildpipeline.png)

8. Gå till versionspipelinen genom att klicka på `Releases`  
![Go to Releases](/media-draft/1-gotoreleases.png) (Gå till versioner)

9. Versionspipelinen skapas. Redigera den genom att klicka på versionen och välja `Edit pipeline`  
![Redigera version](/media-draft/1-editrelease.png)

10. Azure har skapat en versionspipeline som passar för de tekniker som valts. En node-app som körs i en container i ett Kubernetes-kluster.
![Versionspipeline](/media-draft/1-releasepipeline.png)

11. Gå tillbaka till Azure-portalen och klicka på den externa slutpunkten för Kubernetes-tjänsten  
![](/media-draft/1-clickonendpoint.png)

12. Du bör se exempelappen byggas och distribueras i AKS-klustret  
![Appen körs](/media-draft/1-apprunning.png)

## <a name="summary"></a>Sammanfattning

I den här enheten skapade du ett Azure DevOps-projekt som består av:

1. Infrastruktur för appen – Azure Kubernetes Service och Application Insights

2. Ett teamprojekt i en VSTS-instans.

3. Källkoden för en Node.js-exempelapp som körs i en container i lagringsplatsen för din VSTS-instans.

4. En bygg- och versionspipeline för en Node.js-containerapp som körs i din Azure Kubernetes Service-instans.

I nästa steg lär du dig hur du ersätter exempelappen med din verkliga app.