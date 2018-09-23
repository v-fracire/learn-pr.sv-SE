Nu när den virtuella datorn är igång ska vi göra något med den. Här installerar du en webbserver och levererar en grundläggande webbsida som visar den virtuella datorns värdnamn.

Du har flera alternativ när du ska konfigurera en virtuell dator. Du kan ansluta direkt och interaktivt konfigurera systemet. I till exempel Windows-system kan du skapa en fjärrskrivbordssession för att ansluta till användargränssnittet för din Windows-fjärrdator, som om du skulle sitta vid den. På Linux-system kan du skapa en SSH-anslutning för att arbeta säkert med Linux-fjärrsystemet via terminalen.

Manuell konfiguration är en bra början men när du lägger till system kan du automatisera dina distributioner. Automatisering involverar att köra upprepade processer som program och skript som tar hand om grovjobbet åt dig.

::: zone pivot="windows-cloud"

Här fjärrkonfigurerar du IIS via en Cloud Shell-session med hjälp av en funktion i Windows-baserade virtuella Azure-datorer som kallas för tillägget för anpassat skript.

::: zone-end

::: zone pivot="linux-cloud"

Här fjärrkonfigurerar du Nginx via en Cloud Shell-session med hjälp av en funktion i Linux-baserade virtuella Azure-datorer som kallas för tillägget för anpassat skript.

::: zone-end

::: zone pivot="windows-cloud"

## <a name="what-is-iis"></a>Vad är IIS?

Internet Information Services, eller IIS, är en webb som körs på Windows. Du kan använda IIS till att leverera standardwebbinnehåll (HTML, CSS och JavaScript) eller köra ASP.NET och andra typer av webbprogram. IIS levereras med Windows Server men du måste aktivera det för att börja leverera webbsidor.

## <a name="whats-the-custom-script-extension"></a>Vad är tillägget för anpassat skript?

Tillägget för anpassat skript är ett enkelt sätt att ladda ned och köra skript på virtuella Azure-datorer. Det är bara ett av många sätt som du kan konfigurera systemet när din virtuella dator är igång.

Du kan lagra skript i Azure Storage eller på en offentlig plats som GitHub. Du kan köra skript manuellt eller som en del av en mer automatiserad distribution. Här kör du ett Azure CLI-kommando för att ladda ned ett PowerShell-skript från GitHub och köra det på den virtuella datorn. Skriptet konfigurerar IIS.

## <a name="configure-iis"></a>Konfigurera IIS

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

När använder du tillägget för anpassat skript till att fjärrkonfigurera IIS på den virtuella datorn via Cloud Shell. Du konfigurera även brandväggen att tillåta inkommande nätverksåtkomst på port 80 (HTTP).

1. Kör det här `az vm extension set`-kommandot via Cloud Shell för att ladda ned och köra ett PowerShell-skript som installerar IIS och konfigurerar en grundläggande startsida.

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    Processen för att konfigurera Nginx, ange innehållet för startsidan och starta tjänsten tar ett par minuter att slutföra.

    Under tiden kan du [undersöka PowerShell-skriptet](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) på en separat webbläsarflik om du vill. Skriptet installerar IIS och konfigurerar startsidan att visa ett välkomstmeddelande tillsammans med datornamnet för den virtuell datorn, myVM.

1. Kör det här `az vm open-port`-kommandot för att öppna port 80 (HTTP) via brandväggen.

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>Kontrollera konfigurationen

Nu när IIS har konfigurerats ska vi verifiera att den körs.

1. Kör det här `az vm list-ip-addresses`-kommandot för att lista den virtuella datorns offentliga IP-adresser.

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > Detta `--query`-argument gör det här kommandot lite komplext. Men du kommer att bli ett proffs på det här när du utforskar Azure djupare.

    Du ser den virtuella datorns offentliga IP-adress. Här är ett exempel.

    ```output
    104.211.9.245
    ```

1. Öppna en ny webbläsarflik och navigera till den virtuella datorns IP-adress. Du ser ett välkomstmeddelande och namnet på den virtuella datorn.

    ![](../media/4-iis-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## <a name="what-is-nginx"></a>Vad är Nginx?

Nginx (uttalas ”engine-x”) är en populär webbserver med öppen källkod som körs på Unix, Linux, macOS och Windows. Här använder du Nginx till att leverera en grundläggande webbsida.

## <a name="whats-the-custom-script-extension"></a>Vad är tillägget för anpassat skript?

Tillägget för anpassat skript är ett enkelt sätt att ladda ned och köra skript på virtuella Azure-datorer. Det är bara ett av många sätt som du kan konfigurera systemet när din virtuella dator är igång.

Du kan lagra skript i Azure Storage eller på en offentlig plats som GitHub. Du kan köra skript manuellt eller som en del av en mer automatiserad distribution. Här kör du ett Azure CLI-kommando för att ladda ned ett Bash-skript från GitHub och köra det på den virtuella datorn. Skriptet konfigurerar Nginx.

## <a name="configure-nginx"></a>Konfigurera Nginx

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

När använder du tillägget för anpassat skript till att fjärrkonfigurera Nginx på den virtuella datorn via Cloud Shell. Du konfigurera även brandväggen att tillåta inkommande nätverksåtkomst på port 80 (HTTP).

1. Kör det här `az vm extension set`-kommandot via Cloud Shell för att ladda ned och köra ett Bash-skript som installerar Nginx och konfigurerar en grundläggande startsida.

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    Processen för att konfigurera IIS, ange innehållet för startsidan och starta tjänsten tar ett par minuter att slutföra.

    Under tiden kan du [undersöka Bash-skriptet](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true) på en separat webbläsarflik om du vill. Skriptet installerar Nginx och konfigurerar startsidan att visa ett välkomstmeddelande tillsammans med datornamnet för den virtuell datorn, myVM.

1. Kör det här `az vm open-port`-kommandot för att öppna port 80 (HTTP) via brandväggen.

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>Kontrollera konfigurationen

Nu när Nginx har konfigurerats ska vi verifiera att den körs.

1. Kör det här `az vm list-ip-addresses`-kommandot för att lista den virtuella datorns offentliga IP-adresser.

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > Detta `--query`-argument gör det här kommandot lite komplext. Men du kommer att bli ett proffs på det här när du utforskar Azure djupare.

    Du ser den virtuella datorns offentliga IP-adress. Här är ett exempel.

    ```output
    137.135.110.210
    ```

1. Öppna en ny webbläsarflik och navigera till den virtuella datorns IP-adress. Du ser ett välkomstmeddelande och namnet på den virtuella datorn.

    ![](../media/4-nginx-browser.png)

::: zone-end

## <a name="summary"></a>Sammanfattning

Din virtuella dator är igång kan nu leverera webbsidor, men vad betyder det för dig?

Kom ihåg att varje resa börjar med grunderna, och nästan alla bra innovationer som föds i molnet, hos både stora och små företag, startar med en ett upplägg som liknar ditt. När din idé utvecklas börjar den få en positiv effekt på verksamheten och dina användare.