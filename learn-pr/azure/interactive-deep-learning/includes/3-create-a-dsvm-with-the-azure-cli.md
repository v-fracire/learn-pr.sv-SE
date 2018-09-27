Som programvaruutvecklare på ditt företag har du möjligheten att utveckla dina kunskaper och bli en del av det interna AI-teamet. Medan du arbetar med den här nya, spännande rollen behöver du fortfarande sköta ditt vanliga jobb. Den seniora AI-ingenjören i teamet har berättat för dig om några användbara Jupyter-notebooks som har PyTorch-baserade labbar för att träna en modell för klassificering av bilder. Det låter spännande, men du vill inte installera en uppsättning ramverk på din utvecklingsdator. I stället skapar du en virtuell dator baserat på avbildningen DSVM-avbildningen (Data Science Virtual Machine). 

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="what-is-the-azure-cli"></a>Vad är Azure CLI

Azure CLI är Microsofts plattformsoberoende kommandoradsverktyg för att hantera Azure-resurser. Det finns för macOS, Linux och Windows eller i webbläsaren med [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). Vi har fullständig information om användning av det här verktyget i modulen **Kontrollera Azure-tjänster med CLI**.

## <a name="managing-deployments"></a>Hantera distributioner

Azure CLI innehåller kommandot `az group deployment` för att hantera Azure Resource Manager-distributioner. Det finns flera underkommandon för att utföra specifika aktiviteter. De vanligaste är:

| Underkommando | Beskrivning |
|-------------|-------------|
| `create` | Starta en distribution. |
| `list` | Hämta alla distributioner för en resursgrupp. |
| `export` | Exportera den mall som används för en distribution. |

En fullständig lista över tillgängliga distributionskommandon finns i [referensen för kommandon för az-gruppdistribution](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)

Vi använder `az group deployment create` för att etablera vår virtuella dator.

## <a name="create-a-json-deployment-parameters-file"></a>Skapa en fil med JSON-distributionsparametrar

Vi skapar vår virtuella dator med hjälp av en Azure Resource Manager-mall. Mallen definierar den Linux DSVM-avbildning som vi vill etablera. Vi behöver ange vissa parametrar i mallen, till exempel den VM-storlek som ska användas, administratörens användarnamn och lösenord samt datorns värdnamn. 

1. Kör följande kommando i Azure Cloud Shell till höger om den här enheten:

    ```bash
    code parameter_file.json
    ```
    <!-- TODO add a link to official doc that explains the built-in editor when it becomes available --> Det här kommandot öppnar en tom fil som heter `parameter_file.json` i den inbyggda redigeraren. 

1. Klistra in följande JSON-kodavsnitt i den tomma filen i kodredigeraren.

    ```json
    { 
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "adminUsername": { "value" : "<USERNAME>"},
         "adminPassword": { "value" : "<PASSWORD>"},
         "vmName": { "value" : "<HOSTNAME>"},
         "vmSize": { "value" : "Standard_DS2_v2"},
      }
    }
    ```

1. Uppdatera följande parametrar i det JSON som du klistrade in i redigeraren:

    |Parameter  |Aktuellt värde  |Ditt värde  |
    |---------|---------|---------|
    |adminUsername     |  `<USERNAME>`       |    Välj ett namn för administratörsanvändaren för den nya datorn, t.ex. *azuser*.     |
    |adminPassword     |  `<PASSWORD>`       |   Välj ett lösenord för det här administratörsanvändarkontot. Mer information om lösenordskrav finns i avsnittet med [vanliga frågor och svar om virtuella Linux-datorer](https://docs.microsoft.com/azure/virtual-machines/linux/faq?azure-portal=true)     |
    |vmName     |   `<HOSTNAME>`      |  Välj ett namn för den nya virtuella datorn. Namnet måste börja med en bokstav och får bara innehålla gemener och siffror. Prova ett unikt namn, till exempel ett som innehåller dina initialer och ditt födelseår. |
    |vmSize     |  Standard_DS2_v2       |  Den här VM-storleken fungerar bra för den här övningen, men du kan ändra den om du vill. En lista över tillgängliga VM-storlekar finns i avsnittet om [storlekar för Linux-datorer i Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?azure-portal=true)       |

1. Spara dina ändringar i `parameter_file.json` och stäng textredigeraren.

    > [!IMPORTANT]
    > Kom ihåg de värden som du valde för adminUsername, adminPassword och vmName. Vi använder dem igen senare i den här övningen.

## <a name="create-a-resource-group"></a>Skapa en resursgrupp 

> [!IMPORTANT]
> Normalt skulle du skapa en resursgrupp i en valfri region. Dock tillhandahålls en resursgrupp som du kan använda av den sandbox-session som du är i för närvarande. Din resursgrupp för den här sessionen är **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**.

## <a name="deploy-the-dsvm-to-your-resource-group"></a>Distribuera DSVM till resursgruppen

Vi nu har en resursgrupp och har definierat parametrar för DSVM Resource Manager-mallen i en fil med namnet `parameter_file.json`. Vi kör `az group deployment create` för att etablera vår virtuella dator.

1. Kör följande kommando i Azure Cloud Shell:

    ```azurecli
    az group deployment create \
    --resource-group  <rgn>[sandbox resource group name]</rgn> \
    --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json \
    --parameters parameter_file.json
    ```

    Kommandot använder Resource Manager-mallen och våra parametrar för att skapa den virtuella datorn i vår resursgrupp. 

2. Det tar några minuter att distribuera den virtuella datorn. Konsolen visar ` - Running ..` och inte mycket annat förrän åtgärden har slutförts. När åtgärden har slutförts matas ett JSON-svar ut till skärmen. Rulla ned till slutet av JSON och kontrollera att fältet **"provisioningState"** har värdet *Succeeded* (Lyckades).

    > [!TIP]
    > Om du får ett felmeddelande om att DNS-posten redan används av en annan offentlig IP-adress provar du att ändra **vmName** i `parameter_file.json` till ett annat namn som är unikt.

3. Kör följande kommando för att få information om den virtuella datorn. Ersätt `<HOSTNAME>` med det värdnamn som du har definierat för den virtuella datorn.

    ```azurecli
    az vm get-instance-view \
    --name <HOSTNAME> \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query instanceView.statuses[1] \
    --output table
    ```

    Det här kommandot visar status för den virtuella datorn. Det ska stå *VM running* (Den virtuella dator körs).

Grattis! Du har skapat och distribuerat en virtuell Linux-dator baserat på DSVM-avbildningen.

## <a name="open-the-vm-to-ssh-traffic-on-port-22"></a>Öppna den virtuella datorn för att SSH trafik på port 22

Som standard har vår virtuella dator inte några portar öppna. Vårt mål är att fjärransluta, starta en Jupyter Notebook-server och köra andra lokala kommandon på datorn. För att kunna fjärransluta till den virtuella datorn med SSH-protokollet (Secure Shell) behöver vi öppna en port. Post 22 är standardporten för SSH.  

1. Kör följande kommando i Azure Cloud Shell. Ersätt `<HOSTNAME>` med det namn som du gav den virtuella DSV-datorn under konfigurationen. 

    ```azurecli
    az vm open-port \
    -g <rgn>[sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 22 \
    --priority 900
    ```

Kommandot kan ta upp till en minut att slutföra. När kommandot är klart returnerar det ett JSON-svar till kommandoraden. Kontrollera att fältet **"provisioningState"** har värdet *Succeeded* (Lyckades). Vi kommer att testa om SSH fungerar korrekt, men först öppnar vi en till port.

## <a name="open-the-vm-to-access-the-jupyter-notebook-server-remotely"></a>Öppna den virtuella datorn för att fjärransluta till Jupyter Notebook-servern

Som tidigare nämnts levereras DSVM-avbildningen med programvara, verktyg och exempel förinstallerat som stöder dina projekt om dataforskning, maskininlärning och djupinlärning. Jupyter installeras i avbildningen tillsammans med exempelnotebooks. Vi vill starta en Jupyter Notebook-server på den virtuella datorn och sedan fjärransluta till notebook-servern från våra lokala dator. Som standard körs notebook-servern på port 8888. För att fjärransluta till servern behöver vi öppna den porten på vår virtuella dator. 

1. Kör följande kommando i Azure Cloud Shell. Ersätt `<HOSTNAME>` med det namn som du gav den virtuella DSVM-datorn under konfigurationen.

    ```azurecli
    az vm open-port \
    -g <rgn>[sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 8888 \
    --priority 901
    ```

Kommandot kan återigen ta upp till en minut att slutföra. När kommandot är klart returnerar det ett JSON-svar till kommandoraden. Kontrollera att fältet **"provisioningState"** har värdet *Succeeded* (Lyckades).  

## <a name="connect-to-the-vm-with-secure-shell-ssh"></a>Ansluta till den virtuella datorn med SSH (Secure Shell)

1. Kör följande kommando i Azure Cloud Shell för att hitta den virtuella datorns offentliga IP-adress. Ersätt `<HOSTNAME>` med det namn som du gav din virtuella DSVM-dator under konfigurationen.

    ```azurecli
    az vm list-ip-addresses \
    -g <rgn>[sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --output table
    ```

1. Kör följande kommando i Cloud Shell för att logga in på den virtuella datorn. Ersätt `<USERNAME>` med det användarnamn som du valde i början av den här övningen. Ersätt `<IP>` med värdet från kolumnen **PublicIPAddresses** i föregående steg.

    Om användarnamnet som du valde till exempel var *azuser* och PublicIPAddresses hade värdet 33.165.103.23 skulle det kommandot se ut på följande vis:
    
    `ssh azuser@33.165.103.23`
    
    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 

1. När du uppmanas anger du lösenordet för den administratörsanvändare som du valde i början av den här övningen. När du har loggat in bör uppmaningen ändras till formatet `username@hostname`, till exempel `azuser@js1982`.

Nästa steg är att starta Jupyter Notebook-servern på vår virtuella dator och öppna en notebook via en fjärranslutning.

## <a name="start-the-jupyter-notebook-server-on-the-vm"></a>Starta Jupyter Notebook-servern på den virtuella datorn

Det finns en uppsättning notebooks i mappen `~/notebooks` i den virtuella datorn. Om vi antar att du fortfarande är inloggad via en SSH-session startar du notebook-servern och öppnar en av dessa notebooks för att kontrollera att allt fungerar.


1. Kör följande kommando i kommandotolken i den virtuella datorn:

    ```bash
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root
    ```

> [!CAUTION]
> Åtkomst till notebook-servern i den här övningen sker via `http://`. Om du vill köra en notebook-server offentligt bör du skydda den. Mer information om hur du skyddar en notebook-server finns i den officiella Jupyter-dokumentationen på webben. 

I det föregående kommandot startar vi Jupyter Notebook-server med kommandot `jupyter notebook`. Vi ange tre viktiga kommandoradsargument. Kom ihåg att vi är fjärrinloggade i den här datorn via en konsol. Notebooks hanteras i en webbläsare. 

 - `--ip=0.0.0.0` Som standard körs en notebook-server lokalt på 127.0.0.1:8888 och är enbart tillgänglig från localhost. Du kan komma åt notebook-servern från webbläsaren lokalt med hjälp av http://127.0.0.1:8888. När IP-adressen anges till 0.0.0.0 instrueras servern att lyssna efter trafik på alla IP-adresser. Om notebook-servern lyssnar på 0.0.0.0 kan den nås via värddatorns IP-adress.  
 - `--no-browser`  Eftersom vi vill ansluta till notebook från en annan dator via Internet konfigurerar vi notebook-servern till att inte öppna webbläsaren, vilket är standardbeteendet. 
 - `--allow-root`  I den här övningen har vi bara ett administratörskonto på den virtuella datorn, så vi vill kunna köra notebooks som rot.

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a>Ansluta till Jupyter Notebook-servern från en fjärransluten webbläsare

När ovanstående kommando körs på den virtuella datorn startas notebook-servern och konsolen visar en URL som du kan använda i en webbläsare. 

![Skärmbild av det meddelande som en notebook-server som körs visar i konsolen med URL:en för att komma åt servern från värddatorn](../media/notebook-url.png)

1. Kopiera den URL som notebook-servern visar i adressfältet i din favoritwebbläsare. Du kan även klicka på URL:en för att öppna i standardwebbläsaren. 

    Du får ett meddelande om att webbplatsen inte kan nås eftersom den URL som du fick är anslutningen till notebook-servern från värddatorn.

1. För att fjärransluta till servern ersätter du värdnamnet i URL:en med IP-adressen för den virtuella dator som du sparade tidigare. 

    Här är en exempel-URL för en notebook-server.

    "http://**ab-dsvm-4**:8888/?token={some token}"

    I det här ersätter vi **ab-dsvm-4** med datorns IP-adress. Om vår IP-adress är `52.175.199.43` blir URL:en:

    "http://**52.175.199.43**:8888/?token={some token}"

    Se till att `:8888`, portadressen, behålls i URL:en.

    > [!TIP]
    > Om du inte vill använda IP-adressen kan du även använda det fullständigt kvalificerade namnet för servern, som är i formatet `<HOST NAME>.<REGION>.cloudapp.azure.com`

    Följande skärmbild visar hur Jupyter-instrumentpanelen ser ut i din webbläsare.

    ![Skärmbild som visar instrumentpanelen för Jupyter Notebooks. ](../media/jupyter-in-browser.png)

1. Gå till **notebooks/IntroToJupyterPython.ipynb** och välj den. Prova den här notebook för att kontroller att allt fungerar korrekt.

    Grattis! Du har nu en DSVM-baserad virtuell dator som körs och kan arbeta fjärranslutet med Jupyter. I den här övningen kör vi den programvara som installerades på den virtuella datorn. I nästa övning isolerar vi programvaran i en container på den virtuella datorn så att vi tryggt kan experimentera.

4. När du är klar med notebook kan du stoppa Jupyter-servern med `Control-C` i Cloud Shell.
