Nu ska vi använda Azure CLI för att skapa en resursgrupp och sedan distribuera en webbapp i den här resursgruppen.

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="using-a-resource-group"></a>Använda en resursgrupp

När du arbetar med din egen dator och Azure-prenumeration behöver du först logga in på Azure med kommandot `az login`. Det här behövs inte med Cloud Shell-miljön.

Därefter skulle du normalt skapa en resursgrupp för alla dina relaterade Azure-resurser med ett `az group create`-kommando, men för de här övningarna har en resursgrupp redan skapats åt dig. Använd **<rgn>[resursgruppsnamn för sandbox-miljö]</rgn>** som resursgrupp.

1. Du kan be Azure CLI att lista alla dina resursgrupper i en tabell. Det bör bara finnas en när du arbetar med den kostnadsfria sandbox-miljön i Azure.

    ```azurecli
    az group list --output table
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. I takt med att du utvecklar mer i Azure kan du ha många resursgrupper. Om det finns flera objekt i grupplistan kan du filtrera returvärdena genom att lägga till ett `--query`-alternativ. Testa följande kommando:

    ```azurecli
    az group list --query "[?name == '<rgn>[sandbox resource group name]</rgn>']"
    ```

    Frågan är formaterad med **JMESPath**, som är ett standardfrågespråk för JSON-begäranden. Du kan läsa mer om det här kraftfulla filterspråket på <http://jmespath.org/>. Vi tar även upp frågor mer ingående i modulen **Hantera virtuella datorer med Azure CLI**.

### <a name="steps-to-create-a-service-plan"></a>Steg för att skapa en serviceplan

När du kör webbappar med Azure App Service betalar du för de Azure-beräkningsresurser som används av apparna, och beloppet beror på den App Service-plan som är associerad med webbapparna. Serviceplaner avgör vilken region som används för appens datacenter, hur många virtuella datorer som används och prisnivån.

1. Skapa en App Service-plan för att köra din app. Följande kommando anger inte någon prisnivå eller information om VM-instansen, så som standard får du en plan på **Basic**-nivå med en **Liten** VM-instans.

    > [!WARNING]
    > Namnet på appen och planen måste vara _unika_. Lägg därför till ett suffix till namnet och ersätt texten `<unique>` i kommandot nedan med några siffror, dina initialer eller en textsträng så att namnet blir unikt i hela Azure.

    För parametern `--location` använder du ett av platsvärdena nedan.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --location <location>
    ```

    Det här kommandot kan ta flera minuter att slutföra.

1. Kontrollera att serviceplanen har skapats genom att visa alla dina planer i en tabell.

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Steg för att skapa en webbapp

Nu ska vi skapa webbappen i din serviceplan. Du kan distribuera koden på samma gång, men i vårt exempel gör vi det i separata steg.

1. Skapa webbappen och ange namnet på den plan som du skapade ovan. **Precis som planen måste appnamnet vara unikt**. Ersätt därför markören `<unique>` med en textsträng så att namnet blir unikt globalt.

    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --plan popupappplan-<unique>
    ```

1. Kontrollera att appen har skapats genom att visa alla dina appar i en tabell.

    ```azurecli
    az webapp list --output table
    ```

1. Anteckna det **DefaultHostName** som visas i tabellen; det här är den nåbara webbadressen för den nya webbplatsen. Azure gör att din webbplats blir tillgänglig via det unika appnamnet i domänen `azurewebsites.net`. Om appnamnet till exempel är ”popupwebapp-mslearn123” blir URL:en: `http://popupwebapp-mslearn123.azurewebsites.net`.

1. Webbplatsen har en ”QuickStart”-sida (Snabbstart) som skapas av Azure och som du kan se antingen i en webbläsare eller med CURL. Använd bara **DefaultHostName**:

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
### <a name="steps-to-deploy-code-from-github"></a>Steg för att distribuera kod från GitHub

1. Det sista steget är att distribuera kod från en GitHub-lagringsplats till webbappen. Vi ska använda en enkel PHP-sida som är tillgänglig på Github-lagringsplatsen för Azure-exempel och som visar ”HelloWorld!” när den körs. Var noga med att använda namnet på den webbapp som du skapade.

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. Besök din webbplats igen med en webbläsare eller CURL när den har distribuerats.

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
    ”Hello World!” visas på sidan.

    ```output
    Hello World!
    ```

I den här övningen fick du se ett vanligt mönster för en interaktiv Azure CLI-session. Först använde du ett standardkommando för att skapa en ny resursgrupp. Sedan använde du en uppsättning kommandon för att distribuera en resurs (i det här exemplet en webbapp) till resursgruppen. Du kan enkelt samla de här kommandona i ett kommandoskript och köra det varje gång du behöver skapa samma resurs.