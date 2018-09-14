Du har nu skapat en CI/CD-pipeline för ditt anpassade program med Azure DevOps-projekt. 

## <a name="clean-up-resources"></a>Rensa resurser

När du är klar arbetar med den här CI/CD-pipeline, kan du ta bort alla resurser som skapades med självstudien.

1. Från Azure-portalen bläddrar du till `Resource Groups` och klicka på `VstsResourceGroup-LearnCluster-xxxx` där xxx är slumpmässig grupper av bokstäver och siffror  
![LearnClusterRG](../media-drafts/4-learnclusterrg.png)

2. Klicka på DevOps-projekt med namnet `Learn`  
![Läs länk](../media-drafts/4-learnlink.png)

3. Klicka på `Delete`  
![Ta bort](../media-drafts/4-deleteproj.png)

4. Klicka på `Yes`  
![Ja](../media-drafts/4-yes.png)

Detta tar bort alla resurser som skapats i Azure.

## <a name="summary"></a>Sammanfattning

I den här självstudiekursen lärde du dig att:
> [!div class="checklist"]
> * Skapa ett Azure DevOps-projekt
> * Ersätt exempelprogram i Azure DevOps-projekt med dina egna
> * Anpassa Buld och Release-pipelines
