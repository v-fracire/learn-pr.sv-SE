Out of box experience med ett Azure DevOps-projekt skapas version och versionen rörledningar som passar de tekniker som är valt. För den här modulen skapat du en skapa och släpp-pipeline som passar för en node.js-app som körs i en behållare i ett Kubernetes-kluster. 

Ofta behöver vi anpassa skapa och släpp pipelines för att utföra vissa åtgärder för vårt projekt. Skapa och släpp pipelines i VSTS är 100% anpassningsbara. Du kan göra vad du behöver göra pipelines.

Lär dig hur du anpassar din build and release-pipelines i den här enheten.

## <a name="customize-the-build-pipeline"></a>Anpassa build-pipeline

Build-motor i VSTS är bara en uppgiftsköraren, göra en aktivitet efter en annan. Om du vill anpassa versionen du bara lägga till eller ta bort uppgifter och Fyll i rätt parametrarna för aktiviteten.

Direkt levereras VSTS med cirka 100 uppgifter som du kan använda. Om du behöver göra något som inte finns direkt ur lådan Kontrollera marketplace där det finns över 700 skapa och släpp uppgifter som är redo att hämtas och används. Du har också möjlighet att skriva din egen anpassade aktiviteter.

För den här enheten ska du anpassa build-pipeline genom att installera marketplace uppgifter WhiteSource bult göra säkerhetsgenomsökningar av vår kod.

1. Bläddra till Azure DevOps-projekt i Azure-portalen och klicka på länken för build-definition  
![Skapa länk](../media-drafts/3-buildlink.png)

2. Då kommer du till sidan Skapa pipelines. Klicka på versionen och välj `Edit`  
![Redigera Build](../media-drafts/3-editbuild2.png)

3. Detta tar dig till build-definition. Klicka på den `+` att lägga till en uppgift i agenten jobb 1  
![Lägg till aktivitet](../media-drafts/3-addtask2.png)

4. Skriv i textfältet `bolt` och klicka på `Get it free`  
![Hämta vit källa bult](../media-drafts/3-getwhitesourcebolt.png)

5. Detta tar dig till WhiteSource bult Marketplace-sidan. Klicka på `Get it free`  
![Hämta WhiteSource bult kostnadsfritt](../media-drafts/3-getwhitesourceboltfree2.png)

6. Välj din Azure DevOps-organisation och klickar sedan på `Install`  
![Installera](../media-drafts/3-installwsb.png)

7. Aktivera WhiteSource bult genom att följa anvisningarna här <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate>

8. Gå tillbaka till din Azure-portalen med DevOps projektet läses in och klicka på länken för build-pipeline  
![Build Pipeline Link](../media-drafts/3-buildpipelinelink.png)

9. Välj din version och klicka på `Edit`  
![Redigera Build](../media-drafts/3-editbuild2.png)

10. Klicka på den `+` lägger till agentjobbet 1, Skriv `bolt` i sökfältet och klicka på `Add`  
![Lägg till bult](../media-drafts/3-addwsbolt.png)

11. Detta lägger till aktiviteten WhiteSource bult längst ned i listan, drar den till högst upp  
![Flytta bult till början](../media-drafts/3-moveboltototopoftasklist.png)

12. Klicka på `Save & queue` och välj `Save & queue`  
![Spara och kö](../media-drafts/3-saveandqueue2.png)

13. Klicka på `Save & queue`  
![Spara och köa knappen](../media-drafts/3-saveandqueuebutton.png)

Detta sparar ändrade build-pipeline och placerar versionen. När bygget har slutförts granskar bygget `WhiteSource Bolt Build Report`, du kan se källkoden söktes igenom per WhiteSource bult letar du efter säkerhetsrisker.

![Vit källrapporten](../media-drafts/3-whitesourcereport.png)

## <a name="customize-the-release-pipeline"></a>Anpassa versionspipelinen

Precis som build, versionspipelinen är uppgiftsköraren och kan anpassas på samma sätt. För den här enheten ska du lägga till ett webbtest för prestanda i slutet av versionen. Detta gör att du verifierar att din app har distribuerats och körs i Kubernetes-klustret.

1. Bläddra till DevOps-projekt i Azure Portal och klicka på länken för versionspipelinen  
![Versionen länk](../media-drafts/3-releaselink.png)

2. Då kommer du till sidan versionen pipeline. Klicka på din releasepipeline och klicka på `Edit`  
![Redigera version](../media-drafts/3-editreleasebutton.png)

3. Klicka på aktiviteter i din version `Dev` steg  
![Versionen steg](../media-drafts/3-releasestage2.png)

4. Klicka på den `+` lägger till Phase1, typ `web test` i sökfältet och klicka på `Add` för aktiviteten molnbaserade Web prestandatest  
![Lägg till Webbtest](../media-drafts/3-addwebtest2.png)

5. Redigeringsuppgift snabbtest för Web-prestanda genom att klicka på den och lägga till URL: en till din app i webbplats-URL (för att hitta URL: en och gå till Azure portal DevOps-projektsidan och till höger, högerklickar på dina exempel app extern slutpunkt och Kopiera länk) och sedan för Teringar tName, ange i `Ping Test` och klicka på `Save`  
![Kopiera URL](../media-drafts/3-copyurl.png)  
![Spara Webbtest](../media-drafts/3-savewebtest.png)

Nu när du kör en version när du har distribuerat det nya helm-paketet, körs ett webbtest har nått app-url.

![Web testkörning](../media-drafts/3-webtestrun.png)

## <a name="summary"></a>Sammanfattning

I den här enheten lärde du dig att anpassa din build and release-pipelines. Du också lärt dig hur du installerar och använder uppgifter från Marketplace.