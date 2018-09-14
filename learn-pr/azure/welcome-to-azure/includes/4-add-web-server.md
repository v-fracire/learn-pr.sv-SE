Nu när den virtuella datorn är igång kan du nu ska vi göra något med den. Här du installerar en webbserver och leverera en grundläggande webbsida som visar den Virtuella datorns värdnamn.

Om du vill konfigurera en virtuell dator, har du flera alternativ. Du kan ansluta direkt och interaktivt konfigurera ditt system. Du kan till exempel skapa en fjärrskrivbordssession för att ansluta till din fjärrdatorn i Windows-Gränssnittet som om du har monterat på det på Windows-System. Du kan skapa en SSH-anslutning att arbeta på ett säkert sätt med din fjärranslutna Linux-system från terminalen i Linux-system.

Manuell konfiguration är en bra början, men när du lägger till system kan du automatisera dina distributioner. Automation involverar körning av upprepade processer, till exempel program och skript som tar hand om grovjobbet åt dig.

::: zone pivot="windows-cloud"

Här kan konfigurerar du IIS via en fjärranslutning från Cloud Shell-sessionen med hjälp av en funktion i Windows-baserade virtuella datorer i Azure kallas tillägget för anpassat skript.

::: zone-end

::: zone pivot="linux-cloud"

Här får du konfigurera Nginx via en fjärranslutning från Cloud Shell-sessionen med hjälp av en funktion i Linux-baserade virtuella datorer i Azure kallas tillägget för anpassat skript.

::: zone-end

::: zone pivot="windows-cloud"

## <a name="what-is-iis"></a>Vad är IIS?

Internet Information Services eller IIS, är en webbserver som körs på Windows. Du kan använda IIS för att hantera standard webbinnehåll (HTML, CSS och JavaScript) eller kör ASP.NET och andra typer av webbprogram. IIS levereras med Windows Server, men du måste aktivera den för att starta webbsidor.

## <a name="whats-the-custom-script-extension"></a>Vad är det anpassade Skripttillägget?

Tillägget för anpassat skript är ett enkelt sätt att hämta och kör skript på virtuella datorer i Azure. Det är bara en av de många sätt som du kan konfigurera den virtuella datorn när den virtuella datorn är igång.

Du kan lagra dina skript i Azure storage eller på en offentlig plats, till exempel GitHub. Du kan köra skript manuellt eller som en del av en mer automatiserad distribution. Här kör du ett Azure CLI-kommando för att hämta ett PowerShell-skript från GitHub och kör den på den virtuella datorn. Skriptet konfigurerar IIS.

## <a name="configure-iis"></a>Konfigurera IIS

Här kommer du använda tillägget för anpassat skript för att konfigurera IIS via fjärranslutning på den virtuella datorn från Cloud Shell. Du måste också konfigurera brandväggen att tillåta inkommande åtkomst på port 80 (HTTP).

1. Från Cloud Shell kör du det här `az vm extension set` kommando för att hämta och köra ett PowerShell-skript som installerar IIS och konfigurerar en grundläggande startsida.

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --vm-name myWindowsVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    Processen tar några minuter att slutföra.

    Under tiden kan du [granska PowerShell-skriptet](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) från en separat webbläsarflik om du vill ha. Skriptet installerar IIS och konfigurerar på sidan om du vill visa ett välkomstmeddelande tillsammans med den Virtuella datornamn, ”myWindowsVM”.

1. Kör det här `az vm open-port` kommando för att öppna port 80 (HTTP) genom brandväggen.

    ```azurecli
    az vm open-port \
      --name myWindowsVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>Kontrollera konfigurationen

Nu när IIS är konfigurerat kan vi Kontrollera att den körs.

1. Kör det här `az vm list-ip-addresses` kommando för att lista den Virtuella datorns offentliga IP-adresser.

    ```azurecli
    az vm list-ip-addresses \
      --name myWindowsVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > Det här kommandot är ganska komplexa, särskilt `--query` argumentet. Men du kommer att ett proffs på den här som du titta närmare på och utforskar Azure.

    Du ser den Virtuella datorns offentliga IP-adress. Här är ett exempel.

    ```console
    104.211.9.245
    ```

1. Navigera till den Virtuella datorns IP-adress i en ny webbläsarflik. Du ser ditt välkomstmeddelande och den Virtuella datorns namn.

    ![](../media/iis-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## <a name="what-is-nginx"></a>Vad är Nginx?

Nginx (uttalas ”engine-x”) är en populära kostnadsfria, open source-webbserver som körs på Unix, Linux, macOS och Windows. Här ska du använda Nginx för att hantera en grundläggande webbsida.

## <a name="whats-the-custom-script-extension"></a>Vad är det anpassade Skripttillägget?

Tillägget för anpassat skript är ett enkelt sätt att hämta och kör skript på virtuella datorer i Azure. Det är bara en av de många sätt som du kan konfigurera den virtuella datorn när den virtuella datorn är igång.

Du kan lagra dina skript i Azure storage eller på en offentlig plats, till exempel GitHub. Du kan köra skript manuellt eller som en del av en mer automatiserad distribution. Här kör du ett Azure CLI-kommando för att hämta ett Bash-skript från GitHub och köra den på den virtuella datorn. Skriptet konfigurerar Nginx.

## <a name="configure-nginx"></a>Konfigurera Nginx

Här ska du använda tillägget för anpassat skript för att konfigurera Nginx via fjärranslutning på den virtuella datorn från Cloud Shell. Du måste också konfigurera brandväggen att tillåta inkommande åtkomst på port 80 (HTTP).

1. Från Cloud Shell kör du det här `az vm extension set` kommando för att hämta och köra ett Bash-skript som installerar Nginx och konfigurerar en grundläggande startsida.

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --vm-name myLinuxVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/20fca2517fa5913150abab66b51b0d88aa3077d8/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    Processen tar några minuter att slutföra.

    Under tiden kan du [undersöka Bash-skript](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/20fca2517fa5913150abab66b51b0d88aa3077d8/configure-nginx.sh?azure-portal=true) från en separat webbläsarflik om du vill ha. Skriptet installerar Nginx och konfigurerar på sidan om du vill visa ett välkomstmeddelande tillsammans med den Virtuella datornamn, ”myLinuxVM”.

1. Kör det här `az vm open-port` kommando för att öppna port 80 (HTTP) genom brandväggen.

    ```azurecli
    az vm open-port \
      --name myLinuxVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>Kontrollera konfigurationen

Nu när Nginx är konfigurerat kan vi Kontrollera att den körs.

1. Kör det här `az vm list-ip-addresses` kommando för att lista den Virtuella datorns offentliga IP-adresser.

    ```azurecli
    az vm list-ip-addresses \
      --name myLinuxVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > Det här kommandot är ganska komplexa, särskilt `--query` argumentet. Men du kommer att ett proffs på den här som du titta närmare på och utforskar Azure.

    Du ser den Virtuella datorns offentliga IP-adress. Här är ett exempel.

    ```console
    137.135.110.210
    ```

1. Navigera till den Virtuella datorns IP-adress i en ny webbläsarflik. Du ser ditt välkomstmeddelande och den Virtuella datorns namn.

    ![](../media/nginx-browser.png)

::: zone-end

## <a name="summary"></a>Sammanfattning

Den virtuella datorn körs och kan nu hantera upp webbsidor, men vad betyder det åt dig?

Kom ihåg att varje resa börjar med grunderna och nästan alla bra innovation föddes i molnet, från andra företag stora och små, igång med en liknande konfiguration till dig. Eftersom din idé utvecklas, börjar den att göra en positiv inverkan på verksamheten och dina användare.