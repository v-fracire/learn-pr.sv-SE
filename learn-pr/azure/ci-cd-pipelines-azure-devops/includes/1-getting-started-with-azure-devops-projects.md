Hur du konfigurerar en CI/CD-pipeline har alltid svårt för att göra snabbt. Nu, är det otroligt lätt att gå från ingenting alls till en fullständig från slutpunkt till slutpunkt Azure DevOps-projekt. Och i Azure DevOps-projekt får du följande:

1. Infrastruktur etablerat för dig i Azure.

2. Ett Teamprojekt i en VSTS-instans.

3. Källkoden för en exempelapp på det språk som du valt i lagringsplatsen för din VSTS-instans.

4. En skapa och släpp pipeline som passar för de tekniker som är valt.

Och när den är klar, Azure DevOps projekt tar exempelappen, bygger och frigör den via pipelines i infrastruktur som etableras för dig in i Azure. Och du får allt detta med ett par klick.

## <a name="create-an-azure-devops-project"></a>Skapa ett Azure DevOps-projekt

Du kan skapa ett Azure DevOps-projekt från Azure-portalen.

1. Gå till den [Azure-portalen](https://portal.azure.com) och klicka på `Create a resource`  
![AzurePortal](../media-drafts/1-azureportal.png)

2. Klicka på `DevOps Project`  
![Pick DevOps Project](../media-drafts/1-pickdevopsproject.png)

3. Nästa skärm är där du får välja vilket språk som du vill använda. Observera hur du väljer .NET (naturligtvis), java, node, php, python, ruby och go. Du kan även importera din egen kod från en git-lagringsplats. Vi går vidare och skapa en Node.js-app för den här enheten. Node.js och klicka på nästa  
![Välj Node.js för språk](../media-drafts/1-picknodejsforlang.png)

4. Bredvid kommer det att be dig vilket node.js-ramverk som du vill använda. För den här enheten, Välj enkel Node.js-app och klicka på nästa  
![Välj enkel nod](../media-drafts/1-picksimplenode.png)

5. Sedan det kommer att be dig vilken infrastruktur du vill etablera och kör appen i? För den här enheten nu ska vi etablera och distribuera till ett Kubernetes-kluster med hjälp av Azure Kubernetes Service. Välj Kubernetes Service och klicka på nästa  
![Pick Kubernetes](../media-drafts/1-pickkubernetes.png)

6. Och nu kan du kan skapa en helt ny instans av VSTS eller välj en befintlig. Du får också konfigurera var och hur du vill ha kubernetes-klustret att köra. För den här enheten, gå vidare och skapa en ny VSTS-instans genom att välja Välj ny och ge ett unikt namn för din VSTS-instans. Ange Läs som projektnamn, Välj din azure-prenumeration, namnge kluster namn LearnCluster, Välj USA, östra för platsen och klickar du på klar  
![](../media-drafts/1-finalconfirmation2.png)

Och är verkligen det klart! Detta tar en stund så startar tillbaka och bara låta Azure göra dess sak. I de flesta fall kommer att gå åt etablering och konfigurera Azure-infrastrukturen. För den här modulen är det här din Azure Kubernetes Service och Azure Container Registry.

## <a name="a-lap-around-the-finished-azure-devops-project"></a>Ett varv runt klar Azure DevOps-projekt

När Azure är klart kommer du att meddelas i dina aviseringar

1. Klicka på aviseringen och går sedan till resursen  
![Gå till resurs från avisering](../media-drafts/1-gotoresourcefromalert.png)

2. Detta tar dig till en portal som visar allt etableras. Är dina CI/CD-pipeline till vänster. Du har din kodcentrallager, build-definition och även din versionsdefinition. Alla länkar är djupgående länkar som vägleder dig direkt till resursen i VSTS. Och till höger visas all infrastruktur som är distribuerade åt dig i Azure. Har du Kubernetes-kluster med din exempelapp som redan har distribuerats och det också en application Insights. Alla dessa länkar är återigen djuplänkar till resurser i Azure.  
![Portalen DevOps-projekt](../media-drafts/1-portaldevopsproj.png)

3. Klicka på länken för din källkod  
![Länk till källan](../media-drafts/1-linktosource.png)

4. Detta tar dig till git-lagringsplats i Azure-kod. Observera att detta är bara en normal git-lagringsplats som innehåller node.js-exempelapp med helm-diagram.  
![Azure Repo](../media-drafts/1-azurerepo.png)

5. Gå till versionerna  
![Länka till versioner](../media-drafts/1-linktobuild.png)

6. Redigera skapade versionen genom att klicka på versionen och sedan klicka på Redigera  
![Redigera build-länk](../media-drafts/1-editbuildlink2.png)

7. Du kan se en build-pipeline som passar för de tekniker som är valt. Eftersom vi har valt en node.js-app till ett Kubernetes-kluster får vi en build-pipeline som använder Docker-uppgifter för att skapa Node.js-app, skapa behållaravbildningen bild och sedan Helm uppgifter för att skapa ett Helm-paket.  
![Skapa Pipeline](../media-drafts/1-buildpipeline2.png)

8. Gå till versionspipelinen genom att klicka på `Releases`  
![Versionen länk](../media-drafts/1-gotoreleaselink.png)

9. Du ser versionspipelinen skapas. Redigera den genom att klicka på versionen och välja `Edit pipeline`  
![Redigera version](../media-drafts/1-editrelease2.png)

10. Azure har skapat en releasepipeline som passar för de tekniker som är valt. En node-app som körs i en behållare i ett Kubernetes-kluster.
![Releasepipeline](../media-drafts/1-releasepipeline2.png)  
![Versionen Pipeline steg](../media-drafts/1-pipelinesteps.png)

11. Gå tillbaka till Azure-portalen och klicka på den externa slutpunkten för kubernetes-tjänst  
![](../media-drafts/1-clickonendpoint.png)

12. Du bör se exempelappen bygga och distribuerats i AKS-kluster  
![App som körs](../media-drafts/1-apprunning.png)

## <a name="summary"></a>Sammanfattning

I den här enheten skapade du ett Azure DevOps-projekt som består av:

1. Infrastrukturen för din app – Azure Kubernetes Service och Application Insights

2. Ett Teamprojekt i en VSTS-instans.

3. Källkoden för ett Node.js-exempelapp som körs i en behållare i lagringsplatsen för din VSTS-instans.

4. Skapa och släpp pipeline för en app för Node.js-behållare som körs i Azure Kubernetes Service-instans.

Därefter lär du dig hur du ersätter exempelappen med din verklig app.