Hittills i den här modulen vi har lyckats få ett Slack _snedstreck_ kommandot fungerar men bara när du kör på localhost och till vår dator med ngrok-tunneltrafiken. Detta är bra för utveckling, men om du vill frigöra vi vill att vår kod körs i molnet.

I den här lektionen ska vi distribuera vår Azure-funktion till molnet och uppdatera slack-konfigurationen om du vill använda den nya produktionsversionen av vår kod.

I slutet av den här föreläsning lär du dig:

- Så här skapar du en Azure Function-App på Azure
- Hur du distribuerar till Azure med hjälp av Visual Studio Code
- Hur du använda dina lokala inställningar för produktion

## <a name="create-an-azure-function-app-on-azure"></a>Skapa en Azure Function-App i Azure

Vi ska skapa azure-funktionen med hjälp av VS Code och tillägget för Azure-funktion.

1. Öppna kommandopaletten med <kbd>Ctrl</kbd>+<kbd>P</kbd> och välj `Azure Function: Deploy to Function App`.

   ![Distribuera till Funktionsapp](/media-drafts/10.deploy-to-function-app.png)

2. Uppmanas du att bekräfta att du vill ladda upp det aktuella projektet och välj som.

   ![Välj mapp att distribuera](/media-drafts/10.select-folder-to-deploy.png)

3. Välj den prenumeration som du vill associera med appen.

   Om du är nybörjare på Azure bör du har en prenumeration, välja detta.

4. Välj Skapa ny funktionsapp

   ![Skapa ny Funktionsapp](/media-drafts/10.create-new-function-app.png)

5. Välj ett namn för din app

   Uppmanas du att ange ett namn, anropar det vad som helst. Jag ansluter utvinna data från `mojifier-slack-function-app`.

   ![Välj namn](/media-drafts/10.choose-app-name.png)

6. Skapa en ny resursgrupp

   Resursgrupper är hur vi gruppera flera tjänster i Azure till ett _app_. Du kan skapa en ny eller Använd en befintlig du redan har skapat.

   Om du skapar en ny en, kommer dess fylls i automatiskt ett namn för dig, kan du antingen välja som eller Skriv en annan.

7. Välj _Skapa nytt lagringskonto_.

   En funktionsapp måste ett lagringskonto, en mer permanent plats för att lagra data och filer. Du kan skapa en ny eller Använd en befintlig du redan har skapat.

   Det fylls i automatiskt ett namn. Du kan välja som, eller så kan du ange en annan.

8. Välj en region

   Funktionsappen har TTL-värde någonstans. Jag rekommenderar att du väljer en plats som är närmast dig eller dina användare. Jag valt `West Europe`

   ![Välj namn](/media-drafts/10.select-region.png)

9. Vänta tills guiden Överför till klar när du är klar utdatafönstret visar en URL liknande så:

```
Deployment to "mojifier-slack-function-app" completed.
HTTP Trigger Urls:
  MojifyImage: https://mojifier-slack-function-app.azurewebsites.net/api/MojifyImage
```

## <a name="try-it-out"></a>Prova

Nu kan vi förvänta minst den `RespondToSlackCommand` funktionen att arbeta eftersom den inte beroende av andra beroenden.

Gå till `[deployed-app-name].azurefunctions.com/api/RespondToSlashCommand?text=[image-url]`

Om allt fungerar bra sedan du bör vissa JSON som returneras i webbläsarfönstret som detta:

```json
{
  "response_type": "in_channel",
  "text": "You must provide an image to mojify",
  "attachments": [
    {
      "image_url": "https://mojifier-slack.azurewebsites.net/api/MojifyImage?imageUrl=undefined"
    }
  ]
}
```

## <a name="upload-settings"></a>Överföra inställningar

Kom ihåg att vi har vissa inställningar som vi `local.settings.json` dessa har nycklar och URL: en för cognitive services.

`local.settings.json` är precis det lokala, den aldrig kopieras till produktion när du distribuerar.

Vanligtvis behöver du gå till portalen och kanske manuellt till i inställningarna via Användargränssnittet eller Använd den `func` CLI för att göra samma sak.

Men eftersom vi använder VS Code, kan vi använda tillägget Azure Func och kommandopaletten för att överföra de lokala inställningarna för oss.

1.  Öppna kommandopaletten med <kbd>Ctrl</kbd>+<kbd>P</kbd> och välj `Azure Functions: Upload Local Settings`.

    ![Ladda upp lokala inställningar](/media-drafts/10.upload-local-settings.png)

2.  Välj `local.settings.json`

    ![Välj lokala inställningar](/media-drafts/10.choose-localsettings.png)

3.  Välj den prenumeration som funktionsappen är associerad med.

4.  Välj den funktionsapp som du vill överföra till, utvinna data från kallas `mojifier-slack-function-app`

5.  Om den ber dig att skriva över eventuella inställningar, Välj `No To All`

    Detta bara överför nya inställningar som är vad vi vill, annars riskerar du åsidosätta inställningarna för produktion med inställningar från utveckling.

    ![Välj lokala inställningar](/media-drafts/10.choose-no-to-all.png)

## <a name="try-it-out"></a>Prova

Nu om allt fungerar som förväntat bör sedan nu MojifyImage slutpunkten fungerar.

Gå till `[deployed-app-name].azurefunctions.com/api/MojifyImage?imageUrl=[image-url]`

Om allt fungerar bra, bör du se mojified bilden i fönstret.

Om det inte fungerar, försöker du felsökning av problem med här.

## <a name="update-slack"></a>Uppdatera Slack

Nu har vi distribuerat våra funktioner till molnet kan vi uppdatera inställningarna i Slack så att den pekar till den nya _offentliga_ URL: en.

1. Uppdatera Slack, så att den använder en offentlig URL för din `RespondToSlackCommand`.

![Uppdatera Slack med Prod URL](/media-drafts/10.deploy-update-url.png)

> **OBS!**
>
> Jag uppdaterar kommandot för att använda produktion, _offentliga_, version av den `RespondToSlackCommand` finns på `*.azurewebsites.net`

## <a name="try-it-out"></a>Prova

Du kan kontrollera att allt är korrekt konfigurerad och att Slack begär data från den distribuerade appen i stället för lokal app, kan du stänga av `ngrok` för tillfället.

Gå sedan för att slack-fönstret och Skriv din favorit slack snedstreck-kommando.

`/mojify <image>`

Om det är alla fungerar som förväntat så bör du se en bild som visas i fönstret slack t.ex:

![Win](/media-drafts/10.publish-success.png)
