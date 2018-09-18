Du har nu skapat en CI/CD-rörledning för ditt anpassade program med hjälp av Azure DevOps-projekt. 

## <a name="clean-up-resources"></a>Rensa resurser

När du har arbetat klart med CI/CD-rörledningen, kan du ta bort alla resurser som skapades under självstudien.

1. I Azure Portal bläddrar du till `Resource Groups` och klickar på `VstsResourceGroup-LearnCluster-xxxx`, där xxx är slumpmässiga grupper av bokstäver och siffror  
![LearnClusterRG](/media-draft/4-learnclusterrg.png)

2. Klicka på DevOps-projektet med namnet `Learn`  
![Läs länk](/media-draft/4-learnlink.png)

3. Klicka på `Delete`  
![Ta bort](/media-draft/4-deleteproj.png)

4. Klicka på `Yes`  
![Ja](/media-draft/4-yes.png)

Detta tar bort alla resurser som skapats i Azure.

## <a name="summary"></a>Sammanfattning

I den här självstudien lärde du dig att:
> [!div class="checklist"]
> * Skapa ett Azure DevOps-projekt
> * Ersätta exempelprogrammet i Azure DevOps-projektet med ditt eget
> * Anpassa din versionsrörledning
