När du har skapat ett Azure DevOps-projekt är den första frågan oftast hur du byter ut exempelappen mot din egen app? Det är ganska enkelt, och i den här lektionen får du lära dig två sätt att göra det.

1. Byt ut koden i VSTS git-lagringsplatsen mot den riktiga koden

2. Peka om pipelinen för byggen till en extern git-lagringsplats som innehåller den riktiga koden

## <a name="replacing-code-in-vsts-git-repository"></a>Byta ut koden i VSTS git-lagringsplatsen

Ett enkelt sätt är att klona VSTS git-lagringsplatsen till hårddisken så att du byter ut allt mot din egen kod, och sedan ladda upp allt till VSTS igen. Koden ska nu kompileras och distribueras via CI/CD-pipelinen.

I den här lektionen börjar du genom att ladda ned och lagra källkoden för node.js-appen på hårddisken.

1. Ladda ned källkoden från <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip>

2. Extrahera innehållet i MicrosoftLearnDevOps.zip någonstans på hårddisken. I det här exemplet använder vi `C:\users\abel\Downloads\MicrosoftLearnDevOps`.  
![Uppackad katalog](/media-draft/2-unzippedfolder.png)

Sedan måste du klona lagringsplatsen till hårddisken och byta ut exempelappen med den riktiga node.js-appen. I den här lektionen förutsätter vi att du redan har git installerat i datorn.

1. Bläddra till Azure DevOps-projektet i Azure Portal och klicka på länken till lagringsplatsen med koden.  
![](/media-draft/2-browsetorepolink.png)

2. Klicka på Klona och kopiera git-lagringsplatsens webbadress uppe till höger på sidan.  
![Kopiera webbadressen för kloning](/media-draft/2-copycloneurl.png)  
Med det här kopierar du lagringsplatsens webbadress till Urklipp

3. Klona lagringsplatsen till hårddisken  
![Git-klon](/media-draft/2-gitclone.png)  
I det här exemplet klonades lagringsplatsen till C:\Users\abel\Source\TripleCrown\DevOps

4. Ta bort allt från den lokala lagringsplatsen förutom katalogen `.git`  
![Ta bort allt från lagringsplatsen](/media-draft/2-deleterepoofeverything.png)

5. Kopiera källkoden för den nedladdade node.js-appen till mappen för lagringsplatsen  
![Utbytt kod](/media-draft/2-replacedeverything.png)

6. Lägg till alla ändringar i den lokala lagringsplatsen genom att skriva `git add *` på kommandoraden  
![Git Add för allt](/media-draft/2-gitaddall.png)

7. Checka in ändringar i din lokala lagringsplats genom att skriva `git commit -m "replace sample app with real code"`  
![Checka in Git](/media-draft/2-gitcommit.png)

8. Push-överför ändringarna till git-lagringsplatsen i VSTS med `git push`  
![push-överför till Git](/media-draft/2-gitpush.png)

9. När du har skickat ändringarna till VSTS ska den faktiska appkoden genomgå pipelinen för kompilering  
![Kompilering har startats](/media-draft/2-buildkickedoff.png)  
![Kompilering pågår](/media-draft/2-buildinaction.png) och lansering hela vägen till Azure  
 ![Lansering pågår](/media-draft/2-releaserunning.png)

 När distributionen är färdig kan du kontrollera att den riktiga appen har distribuerats genom att gå tillbaka till Azure Portal

 1. Gå till Azure Portal, bläddra till Azure DevOps-projektet och klicka på den distribuerade appen till höger  
 ![Länk för att starta exempelapp](/media-draft/2-launchapp.png)

 2. Det här startar appen som körs i din webbläsare  
 ![Appen körs](/media-draft/2-apprunning.png)

## <a name="using-external-git-repo"></a>Använda en extern git-lagringsplats

Ett annat sätt att byta ut exempelappen mot den riktiga appkoden är att peka om pipelinen för kompilering mot en extern git-lagringsplats som innehåller din appkod. I det här exemplet laddar du upp den riktiga appkoden till en lagringsplats på github.

När du har laddat upp den riktiga koden till github gör du så här för att peka om pipelinen till github-lagringsplatsen

1. Bläddra till Azure DevOps-projektet i Azure Portal och klicka på kompileringslänken  
![Komplieringslänk](/media-draft/2-buildlink.png)

2. Nu kommer du till dina pipelines för kompilering. Klicka på pipelinen och sedan på `Edit`  
![Klicka på redigera kompilering](/media-draft/2-clickeditbuildlink.png)

3. Nu öppnas kompileringsredigeraren, klicka på `Get sources`  
![Klicka på Hämta källkod](/media-draft/2-clickgetsource.png)

4. Nu kommer du till sidan Välj källa. Observera att du inte behöver använda VSTS Git, utan även kan använda GitHub, GitHub Enterprise, Subversion, Bitbucket Cloud eller någon annan extern Git-baserad lagringsplats i pipelinen för kompilering. I den här övningen väljer du GitHub  
![Välj GitHub](/media-draft/2-selectgithub.png)

5. Nu kommer du till anslutningssidan för GitHub. Använd antingen OAuth eller en personlig åtkomsttoken för att ansluta till ditt GitHub-konto

6. Välj github-lagringsplatsen som innehåller den riktiga koden och klicka på `Save & queue`  
![Spara och placera i kö](/media-draft/2-saveandqueue.png)

7. Klicka på knappen `Save & queue`  
![](/media-draft/2-saveandqueuedialog.png)

8. Den här åtgärden sparar och sätter igång kompileringen så att den riktiga appkoden skickas via våra pipelines för kompilering och lansering hela vägen till Azure  
![Kompilering pågår](/media-draft/2-buildrunning.png)

## <a name="summary"></a>Sammanfattning

I den här lektionen har du lärt dig två olika sätt att ersätta exempelkoden i DevOps-projektet med den riktiga appkoden. Det kan du göra genom att ersätta koden i VSTS git-lagringsplatsen eller genom att länka pipelinen för kompilering till en annan extern lagringsplats som innehåller din appkod.

I nästa lektion får du lära dig att anpassa dina pipelines för kompilering och lansering.