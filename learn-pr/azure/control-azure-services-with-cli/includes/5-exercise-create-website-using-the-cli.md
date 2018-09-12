Nu ska vi använda Azure CLI för att skapa en resursgrupp och sedan distribuera en webbapp i den här resursgruppen. 

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

1. Öppna ett bash-gränssnitt i Linux eller macOS, eller öppna kommandotolken eller PowerShell om du arbetar i Windows.

1. Starta Azure CLI och kör inloggningskommandot.

    ```azurecli
    az login
    ```
    Om du inte ser en inloggningssida för Azure i webbläsaren följer du anvisningarna på kommandoraden och anger en auktoriseringskod på [https://aka.ms/devicelogin](https://aka.ms/devicelogin).

1. Skapa en resursgrupp.

    ```azurecli
    az group create --location westeurope --name popupResGroup
    ```

1. Kontrollera att resursgruppen har skapats genom att visa alla resursgrupper i en tabell.

    ```azurecli
    az group list --output table
    ```

> [!TIP]
> Du kan också kontrollera att resursen har skapats på Azure Portal. Öppna en webbläsare, logga in på portalen och gå till avsnittet **Resursgrupper**. Den nya resursgruppen bör visas i listan.

1. Om det finns många objekt i gruppen kan du filtrera returvärdena genom att lägga till ett `--query`-alternativ. Prova det här kommandot:

    ```azurecli
    az group list --query '[?name == popupResGroup]'
    ```

    Frågan är formaterad med **JMESPath**, som är ett standardfrågespråk för JSON-begäranden. Du kan läsa mer om det här kraftfulla filterspråket på <http://jmespath.org/>. Vi tar även upp frågor mer ingående i modulen **Hantera virtuella datorer med Azure CLI**.

### <a name="steps-to-create-a-service-plan"></a>Steg för att skapa en serviceplan

När du kör webbappar med Azure App Service betalar du för de Azure-beräkningsresurser som används av apparna, och beloppet beror på den App Service-plan som är associerad med webbapparna. Serviceplaner avgör vilken region som används för appens datacenter, hur många virtuella datorer som används och prisnivån.

1. Skapa en App Service-plan för att köra din app. Följande kommando anger inte någon prisnivå eller information om VM-instansen, så som standard får du en plan på **Basic**-nivå med en **Liten** VM-instans.

    > [!WARNING]
    > Namnet på appen och planen måste vara _unika_. Lägg därför till ett suffix till namnet och ersätt texten `<unique>` i kommandot nedan med några siffror, dina initialer eller en textsträng så att namnet blir unikt i hela Azure. 

    ```azurecli
    az appservice plan create --name popupapp-<unique> --resource-group popupResGroup --location westeurope
    ```

1. Kontrollera att serviceplanen har skapats genom att visa alla dina planer i en tabell.

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Steg för att skapa en webbapp

Nu ska vi skapa webbappen i din serviceplan. Du kan distribuera koden på samma gång, men i vårt exempel gör vi det i separata steg.

1. Skapa webbappen och ange namnet på planen som du skapade ovan. **Precis som planen måste appnamnet vara unikt. Ersätt därför markören `<unique>` med en textsträng så att namnet blir unikt globalt.**
    ```azurecli
    az webapp create --name popupapp-<unique> --resource-group popupResGroup --plan popupapp-<unique>
    ```

1. Kontrollera att appen har skapats genom att visa alla dina appar i en tabell.

    ```azurecli
    az webapp list --output table
    ```

1. Anteckna **DefaultHostName**, du behöver det här värdet senare.

### <a name="steps-to-deploy-code-from-github"></a>Steg för att distribuera kod från GitHub

1. Det sista steget är att distribuera kod från en GitHub-lagringsplats till webbappen. Vi ska använda en enkel PHP-sida som är tillgänglig på Github-lagringsplatsen för Azure-exempel och som visar ”HelloWorld!” när den körs. Var noga med att använda namnet på webbappen som du skapat.

    ```azurecli
    az webapp deployment source config --name popupapp-<unique> --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. När den har distribuerats blir din webbplats tillgänglig via det unika appnamnet i domänen `azurewebsites.net`. Om appnamnet till exempel är ”popupapp mslearn123” blir webbadressen: <http://popupapp-mslearn123.azurewebsites.net>. Du måste ange rätt URL för att komma till din specifika instans.

1. ”HelloWorld!” visas på sidan.

1. Stäng webbläsarfönstret.

## <a name="summary"></a>Sammanfattning

I den här övningen fick du se ett vanligt mönster för en interaktiv Azure CLI-session. Först använde du ett standardkommando för att skapa en ny resursgrupp. Sedan använde du en uppsättning kommandon för att distribuera en resurs (i det här exemplet en webbapp) till resursgruppen. Du kan enkelt samla de här kommandona i ett kommandoskript och köra det varje gång du behöver skapa samma resurs.
