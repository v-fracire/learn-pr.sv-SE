Nu ska vi använda Azure CLI för att skapa en resursgrupp och sedan distribuera en webbapp i den här resursgruppen.

Du kan använda Azure CLI som du har installerat med din egen Azure-prenumeration, men det finns en kostnadsfri sandbox-miljö som är tillgänglig för den här övningen med Azure Cloud Shell där Azure CLI redan är installerat. Följande instruktioner gäller om du använder den kostnadsfria sandbox-miljön.

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

[!include[](../../../includes/azure-sandbox-activate.md)]

> [!NOTE]
> Normalt sett skulle du skapa en resursgrupp för alla dina relaterade Azure-resurser med ett `az group create ...`-kommando, men för de här övningarna har en resursgrupp redan skapats åt dig. Den här resursgruppens namn kommer att användas för kommandona längre fram i den här övningen.

<!-- TODO: This is original text prior to updates to use the sandbox. These can be worked back in as instructions for people using their own subscriptions. There is one more block like this below. Note that the assignment of RESOURCE_GROUP below would need to be different as well. -->

<!-- 1. Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows. -->

<!-- 1. Start the Azure CLI and run the login command.

    ```azurecli
    az login
    ```
    If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin). -->

<!-- 1. Create a resource group.

    ```azurecli
    az group create --location westeurope --name popupResGroup
    ``` -->

1. Kontrollera att resursgruppen har skapats genom att visa alla resursgrupper i en tabell.

    ```azurecli
    az group list --output table
    ```

    Du kanske bara kan se den `<rgn>[Sandbox resource group name]</rgn>` resursgrupp som skapades för den kostnadsfria sandbox-användningen.

<!-- > [!TIP]
> You can also confirm the resource was created in the Azure portal. Open a web browser, sign in to the portal and navigate to the **Resource Groups** section. The new resource group should be displayed in the list. -->

1. I takt med att du utvecklar mer i Azure kan du ha många resursgrupper. Om det finns flera objekt i grupplistan kan du filtrera returvärdena genom att lägga till ett `--query`-alternativ. Testa följande kommando:

    ```azurecli
    az group list --query "[?name == '<rgn>[Sandbox resource group name]</rgn>']"
    ```

    Frågan är formaterad med **JMESPath**, som är ett standardfrågespråk för JSON-begäranden. Du kan läsa mer om det här kraftfulla filterspråket på <http://jmespath.org/>. Vi tar även upp frågor mer ingående i modulen **Hantera virtuella datorer med Azure CLI**.

### <a name="steps-to-create-a-service-plan"></a>Steg för att skapa en serviceplan

När du kör webbappar med Azure App Service betalar du för de Azure-beräkningsresurser som används av apparna, och beloppet beror på den App Service-plan som är associerad med webbapparna. Serviceplaner avgör vilken region som används för appens datacenter, hur många virtuella datorer som används och prisnivån.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Skapa en App Service-plan för att köra din app. Följande kommando anger inte någon prisnivå eller information om VM-instansen, så som standard får du en plan på **Basic**-nivå med en **Liten** VM-instans.

    > [!WARNING]
    > Namnet på appen och planen måste vara _unika_. Lägg därför till ett suffix till namnet och ersätt texten `<unique>` i kommandot nedan med några siffror, dina initialer eller en textsträng så att namnet blir unikt i hela Azure.

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --location eastus
    ```

    Det här kommandot kan ta flera minuter att slutföra.

1. Kontrollera att serviceplanen har skapats genom att visa alla dina planer i en tabell.

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Steg för att skapa en webbapp

Nu ska vi skapa webbappen i din serviceplan. Du kan distribuera koden på samma gång, men i vårt exempel gör vi det i separata steg.

1. Skapa webbappen och ange namnet på planen som du skapade ovan. **Precis som planen måste appnamnet vara unikt. Ersätt därför markören `<unique>` med en textsträng så att namnet blir unikt globalt.**
    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group popupResGroup --plan popupappplan-<unique>
    ```

1. Kontrollera att appen har skapats genom att visa alla dina appar i en tabell.

    ```azurecli
    az webapp list --output table
    ```

1. Anteckna **DefaultHostName**, du behöver det här värdet senare.

### <a name="steps-to-deploy-code-from-github"></a>Steg för att distribuera kod från GitHub

1. Det sista steget är att distribuera kod från en GitHub-lagringsplats till webbappen. Vi ska använda en enkel PHP-sida som är tillgänglig på Github-lagringsplatsen för Azure-exempel och som visar ”HelloWorld!” när den körs. Var noga med att använda namnet på webbappen som du skapat.

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. När den har distribuerats blir din webbplats tillgänglig via det unika appnamnet i domänen `azurewebsites.net`. Om appnamnet till exempel är ”popupapp mslearn123” blir webbadressen: <http://popupapp-mslearn123.azurewebsites.net>. Du måste ange rätt URL för att komma till din specifika instans.

1. ”Hello World!” visas på sidan.

1. Stäng webbläsarfönstret.

I den här övningen fick du se ett vanligt mönster för en interaktiv Azure CLI-session. Först använde du ett standardkommando för att skapa en ny resursgrupp. Sedan använde du en uppsättning kommandon för att distribuera en resurs (i det här exemplet en webbapp) till resursgruppen. Du kan enkelt samla de här kommandona i ett kommandoskript och köra det varje gång du behöver skapa samma resurs.
