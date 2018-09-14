När du har skapat ett Azure DevOps-projekt är det första alla fråga hur byter du exempelappen med din egen app? Det är enkelt och i den här enheten, kommer du lära dig två sätt att göra det.

1. Ersätt Koden i VSTS git-lagringsplats med verkliga koden

2. Peka din build-pipeline på en extern git-lagringsplats som innehåller verkliga koden

## <a name="replacing-code-in-vsts-git-repository"></a>Ersätt Koden i VSTS git-lagringsplats

Det är ett enkelt sätt genom att klona git-lagringsplats i VSTS till hårddisken, ersätter allt med din egen kod laddar upp tillbaka till VSTS och vips. Koden ska nu skapats och distribueras via CI/CD-pipeline.

För den här enheten startas genom att hämta och lagra källkoden för node.js-appen till hårddisken.

1. Ladda ned källkoden från <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip>

2. Extrahera innehållet i MicrosoftLearnDevOps.zip någonstans på hårddisken. I det här exemplet `C:\users\abel\Downloads\MicrosoftLearnDevOps` har använts  
![Uppzippade Directory](../media-drafts/2-unzippedfolder.png)

Därefter måste du klona lagringsplatsen till hårddisken och ersätta exempelappen med verkliga node.js-app. Den här enheten förutsätter att du redan har git installerat på datorn.

1. Bläddra till din Azure DevOps-projekt och klicka på länken kod databasen från Azure-portalen.  
![](../media-drafts/2-browsetorepolink.png)

2. Klicka på klonen och kopiera URL: en för git-lagringsplats i övre högra sida.  
![Klona lagringsplatsen](../media-drafts/2-clonerepo2.png) detta kommer att kopiera repo-URL: en till Urklipp

3. Klona lagringsplatsen till hårddisken  
![Git-klonen](../media-drafts/2-gitclone.png)  
I det här exemplet klonades lagringsplatsen till C:\Users\abel\Source\TripleCrown\DevOps

4. Tar bort allt från ditt lokala lager undantag för den `.git` directory  
![Ta bort lagringsplatsen i allt](..//media-drafts/2-deleterepoofeverything.png)

5. Kopiera källkoden för den nedladdade node.js-app till mappen lagringsplats  
![Ersatt kod](../media-drafts/2-replacedeverything.png)

6. Lägg till alla ändringar i din lokala lagringsplats genom att skriva `git add *` från kommandoraden  
![Git Lägg till alla](../media-drafts/2-gitaddall.png)

7. Genomför ändringar i din lokala lagringsplats genom att skriva `git commit -m "replace sample app with real code"`  
![Git-incheckning](../media-drafts/2-gitcommit.png)

8. Skicka tillbaka ändringar till git-lagringsplats i VSTS med `git push`  
![Git-Push](../media-drafts/2-gitpush.png)

9. När du har skickat ändringarna tillbaka till VSTS, ska detta skicka koden verklig app via versionen  
![Skapa går ut](../media-drafts/2-buildkickedoff2.png)
![Build körs](../media-drafts/2-buildrunning2.png) och releasepipeline hela vägen till azure  
 ![Versionen som körs](../media-drafts/2-releaserunning2.png)

 När distributionen är klar kan du kontrollera verkliga appen har distribuerats genom att gå tillbaka till Azure-portalen

 1. Gå till Azure portal, bläddra till din Azure DevOps-projekt och klicka på den distribuerade appen till höger  
 ![Starta länk till appen](../media-drafts/2-launchapp.png)

 2. Detta startar app som körs i webbläsaren  
 ![App som körs](../media-drafts/2-apprunning.png)

## <a name="using-external-git-repo"></a>Med hjälp av extern git-lagringsplats

Ett annat sätt att byta ut exempelapp med koden verklig app är genom att peka build-pipeline på en extern git-lagringsplats som innehåller din Appkod. I det här exemplet överför den verkliga kod till en github-lagringsplats.

När du har överfört verkliga koden till github, gör du följande för att peka build-pipeline på den här github-lagringsplatsen

1. Från Azure portal, bläddra till din Azure DevOps-projekt och klicka på länken build  
![Skapa länk](../media-drafts/2-buildlink.png)

2. Detta tar dig till de skapandet av pipelines, klickar du på build-pipeline och sedan `Edit`  
![Klicka på Redigera Build Pipeline](../media-drafts/2-editbuildpipelinelink.png)

3. Detta tar dig till build-redigeraren, klicka på `Get sources`  
![Klicka på Hämta källan uppgift](../media-drafts/2-clickgetsourcetask.png)

4. Detta tar dig till Markera en källsidan. Lägg märke till hur du kan inte bara använda VSTS Git, men även GitHub, GitHub Enterprise, Subversion, Bitbucket-molnet och alla extern Git baserat lagringsplatsen för build-pipelinen. I den här övningen Välj GitHub  
![Välj GitHub](../media-drafts/2-selectgithub2.png)

5. Detta tar dig till GitHub-anslutningssidan. Använda OAuth eller en personlig åtkomsttoken för att ansluta till ditt GitHub-konto

6. Välj github-lagringsplatsen som innehåller den verkliga kod och klicka på `Save & queue`  
![Spara och kö](../media-drafts/2-saveandqueue2.png)

7. Klicka på den `Save & queue` knappen  
![Spara och köa knappen](../media-drafts/2-saveandqueuebutton.png)

8. Den här åtgärden sparar och sparkar av build, skickar appen verkliga code värdbaserad i GitHub via vår build and release-pipelines hela vägen till Azure  
![Skapa körs](../media-drafts/2-buildpipelinerunning.png)

## <a name="summary"></a>Sammanfattning

I den här enheten beskrivs två olika sätt att ersätta exempelkoden i DevOps-projekt med verkliga Appkod. Detta kan göras genom att ersätta koden i VSTS git-lagringsplats eller genom att länka build-pipeline med en annan extern lagringsplats som innehåller din Appkod.

Därefter lär du dig hur du anpassar build and release-pipelines.