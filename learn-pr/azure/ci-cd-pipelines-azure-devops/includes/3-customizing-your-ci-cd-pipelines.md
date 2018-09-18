Den färdiga funktionen med ett Azure DevOps-projekt skapar bygg- och versionspipelines som passar för de tekniker som valts. För den här modulen skapade du en bygg- och versionspipeline som passar för en Node.js-app som körs i en container i ett Kubernetes-kluster. 

Ofta behöver vi anpassa bygg- och versionspipelines för att utföra vissa åtgärder för projektet. Bygg- och versionspipelines i VSTS är 100 % anpassningsbara. Du kan göra så att pipelines utför vad du än behöver.

I den här enheten lär du dig att anpassa dina bygg- och versionspipelines.

## <a name="customize-the-build-pipeline"></a>Anpassa bygg-pipelinen

Byggmotorn i VSTS är bara en uppgiftskörare som utför uppgifter en efter en. Om du vill anpassa bygget behöver du bara lägga till eller ta bort uppgifter och fylla i rätt parametrar för uppgiften.

VSTS levereras med cirka 100 uppgifter som du kan använda. Om du behöver göra något som inte finns inbyggt kan du söka på marknadsplatsen, där det finns över 700 bygg- och versionsuppgifter att ladda ned och använda. Du kan även skriva dina egna anpassade uppgifter.

För den här enheten anpassar du bygg-pipelinen genom att installera marknadsplatsuppgifterna WhiteSource Bolt för att genomföra säkerhetsgenomsökning av koden.

1. Bläddra till Azure DevOps-projektet i Azure-portalen och klicka på länken för byggdefinition  
![Bygglänk](/media-draft/3-buildlink.png)

2. Det här leder till sidan för bygg-pipelines. Klicka på bygget och välj `Edit`  
![Edit Build](/media-draft/3-editbuild.png) (Redigera bygge)

3. Det här leder till sidan för byggdefinitionen. Klicka på `+` för att lägga till en uppgift i Agent Job 1 (Agentjobb 1)  
![Add Task](/media-draft/3-addtask.png) (Lägg till uppgift)

4. I textfältet skriver du `bolt` och klickar på `Get it free`  
![Get it free](/media-draft/3-getitfree.png) (Skaffa det kostnadsfritt)

5. Det här leder till marknadsplatssidan för WhiteSource Bolt. Klicka på `Get it free`  
![Get White Source Bolt Free](/media-draft/3-getwhitesourceboltfree.png) (Skaffa WhiteSource Bolt kostnadsfritt)

6. Välj din VSTS-organisation och klicka sedan på `Install`  
![Install](/media-draft/3-install.png) (Installera)

7. Aktivera WhiteSource Bolt genom att följa anvisningarna här <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate>

8. Gå tillbaka till Azure-portalen med DevOps-projektet inläst och klicka på länken för bygg-pipeline  
![Länk för bygg-pipeline](/media-draft/3-buildpipelinelink.png)

9. Välj ditt bygge och klicka på `Edit`  
![Edit Build](/media-draft/3-editbuild.png) (Redigera bygge)

10. Klicka på `+` för att lägga till en uppgift i agentjobb 1, skriv `bolt` i sökfältet och klicka på `Add`  
![Add Bolt](/media-draft/3-addbolt.png) (Lägg till Bolt)

11. Det här lägger till WhiteSource Bolt-uppgiften längst ned i uppgiftslistan; dra upp den längst upp  
![Bolt längst upp](/media-draft/3-boltattop.png)

12. Klicka på `Save & queue` och välj `Save & queue`  
![Save and Queue](/media-draft/3-saveandqueue.png) (Spara och placera i kö)

13. Klicka på `Save & queue`  
![Dialogrutan Save and Queue](/media-draft/3-saveandqueuedialog.png) (Spara och placera i kö)

Det här sparar den ändrade bygg-pipelinen och placerar bygget i kö. När bygget är klart kan du se i byggets `WhiteSource Bolt Build Report` att källkoden söktes igenom av WhiteSource Bolt för att hitta eventuella säkerhetsrisker.

![Byggrapport](/media-draft/3-buildreport.png)

## <a name="customize-the-release-pipeline"></a>Anpassa versionspipelinen

Liksom bygget är versionspipelinen en uppgiftskörare och kan anpassas på samma sätt. För den här enheten läggar du till ett webbprestandatest i slutet av versionen. Det här verifierar att appen har distribuerats och körts i Kubernetes-klustret.

1. Bläddra till DevOps-projektet i Azure-portalen och klicka på länken för versiondefinition  
![Versionslänk](/media-draft/3-releaselink.png)

2. Det här leder till sidan för versionspipelinen. Klicka på releasepipelinen och klicka på `Edit`  
![Edit Release Pipeline](/media-draft/3-editreleasepipeline.png) (Redigera versionspipeline)

3. Klicka på uppgifterna i `Dev`-steget i versionen  
![Release Stage](/media-draft/3-releasestage.png) (Versionssteg)

4. Klicka på `+` för att lägga till en uppgift i Phase1 (Fas1), skriv `web test` i sökfältet och klicka på `Add` för den molnbaserade uppgiften Web Performance Test (Webbprestandatest)  
![Add Web Test](/media-draft/3-addwebtest.png) (Lägg till webbtest)

5. Redigera uppgiften Quick Web Performance Test (Snabbt webbprestandatest) genom att klicka på den och lägga till URL:en till din app i webbplats-URL:en (du hittar URL:en genom att gå till Azure-portalens DevOps-projektsida, högerklicka på den externa slutpunkten för exempelappen till höger och kopiera länken) och sedan ange `Ping Test` för TestName (Testnamn)  
![Copy URL](/media-draft/3-copyurl.png) (Kopiera URL)  
![Edit Web Test Task](/media-draft/3-editwebtesttask.png) (Redigera uppgiften Webbtestning)

6. Klicka på `Save`  
![Save Release](/media-draft/3-saverelease.png) (Spara version)

När du nu kör en version körs ett webbtest som besöker app-URL:en efter att det nya Helm-paketet har distribuerats.

![Web Test (Webbtest)](/media-draft/3-webtest.png)


## <a name="summary"></a>Sammanfattning

I den här enheten lärde du dig att anpassa dina bygg- och versionspipelines. Du lärde dig även att installera och använda uppgifter från marknadsplatsen.