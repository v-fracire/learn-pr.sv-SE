
I den här övningen använder du Azure CLI lokalt till att skapa en resursgrupp och distribuerar sedan en webbapp till resursgruppen. 

### <a name="steps-to-create-a-resource-group"></a>Steg för att skapar en resursgrupp
1. Öppna ett bash-skal i Linux eller macOS, eller öppna Kommandotolken eller PowerShell om du arbetar i Windows.

1. Starta Azure CLI och kör inloggningskommandot.

    ```bash
    az login
    ```
    Om du inte ser en inloggningssida för Azure i webbläsaren följer du anvisningarna på kommandoraden och anger en auktoriseringskod på [https://aka.ms/devicelogin](https://aka.ms/devicelogin).

1. Skapa en resursgrupp.

    ```bash
    az group create --location westeurope --name popupResGroup
    ```

1. Kontrollera att resursgruppen har skapats genom att visa alla resursgrupper i en tabell.

    ```bash
    az group list --output table
    ```
1. Du kan också kontrollera att resursen har skapats i Azure Portal. Öppna en webbläsare, logga in i portalen och gå till avsnittet **Resursgrupper** (se nedan). Den nya resursgruppen ska visas i listan.

![Använda portalen till att visa resursgrupper](../media-drafts/5-listing-resource-groups.png)

### <a name="steps-to-create-a-service-plan"></a>Steg för att skapa en serviceplan
När du kör webbappar med Azure App Service betalar du för de Azure-beräkningsresurser som används av apparna, och beloppet beror på den App Service-plan som är associerad med webbapparna. Serviceplaner avgör vilken region som används för appens datacenter, hur många virtuella datorer som används och prisnivån.

1. Skapa en App Service-plan för att köra din app. Namnen på appen och planen måste vara unika, så ändra strängen ”12345” till ett slumptal. Följande kommando anger inte någon prisnivå eller information om VM-instansen, så som standard får du en plan på **Basic**-nivå med en **Liten** VM-instans.

    ```bash
    az appservice plan create --name popupapp12345 --resource-group popupResGroup --location westeurope
    ```

1. Kontrollera att serviceplanen har skapats genom att visa alla planer i en tabell.

    ```bash
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Steg för att skapa en webbapp
Nu ska vi skapa webbappen i din serviceplan. Du kan distribuera koden på samma gång, men i vårt exempel gör vi det i separata steg.

1. Skapa webbappen, och kom ihåg att ändra strängen ”12345” till samma slumptal som du använde tidigare.
    ```bash
    az webapp create --name popupapp12345 --resource-group popupResGroup --plan popupapp12345
    ```

1. Kontrollera att appen har skapats genom att visa alla appar i en tabell.

    ```bash
    az webapp list --output table
    ```

1. Anteckna **DefaultHostName**, du behöver det här värdet senare.

### <a name="steps-to-deploy-code-from-github"></a>Steg för att distribuera kod från GitHub
1. Det sista steget är att distribuera kod från en GitHub-repo till webbappen. Glöm inte att ändra strängen ”12345” till samma slumptal som du använde tidigare.
    ```bash
    az webapp deployment source config --name popupapp12345 --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. Kopiera följande webbadress till en webbläsare så att du ser webbappen.
http://popupapp12345.azurewebsites.net

1. Du ser ”HelloWorld!” på sidan

1. Stäng webbläsarfönstret.

## <a name="summary"></a>Sammanfattning
I den här övningen fick du se ett vanligt mönster för en interaktiv Azure CLI-session. Först använde du ett standardkommando för att skapa en ny resursgrupp. Sedan använde du en uppsättning kommandon för att distribuera en resurs (i det här exemplet en webbapp) till resursgruppen. Du kan enkelt samla de här kommandona i ett kommandoskript och köra det varje gång du behöver skapa samma resurs.
