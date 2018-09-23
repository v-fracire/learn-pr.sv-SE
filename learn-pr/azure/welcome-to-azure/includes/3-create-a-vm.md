Som teknikproffs har du förmodligen specialkunskaper inom ett visst område. Kanske är du lagringsadministratör eller virtualiseringsexpert, eller kanske fokuserar du på de senaste säkerhetsrutinerna. Om du studerar kanske du fortfarande utforskar vad som intresserar dig mest.

::: zone pivot="windows-cloud"

Oavsett vilken roll du har brukar de flesta som kommer igång med molnet skapa en virtuell dator. Här får du sätta igång en virtuell dator som kör Windows Server 2016.

::: zone-end

::: zone pivot="linux-cloud"

Oavsett vilken roll du har brukar de flesta som kommer igång med molnet skapa en virtuell dator. Här får du sätta igång en virtuell dator som kör Ubuntu 16.04.

::: zone-end

Det finns många sätt att skapa en virtuell dator i Azure. Här får du sätta upp en virtuell Windows- eller Linux-dator med hjälp av den interaktiva terminalen Cloud Shell. Om du arbetar på terminalen dagligen vet du att det oftast är det snabbaste sättet att få jobbet gjort.

::: zone pivot="windows-cloud"

> [!TIP]
> Föredrar du Linux eller vill du testa något nytt? Välj **Linux** högst upp på den här sidan om du vill köra en virtuell Linux-dator.

::: zone-end

::: zone pivot="linux-cloud"

> [!TIP]
> Föredrar du Windows eller vill du testa något nytt? Välj **Windows** högst upp på den här sidan om du vill köra en virtuell Windows Server-dator.

::: zone-end

Nu ska vi gå igenom några grundläggande termer och få igång din första virtuella dator.

## <a name="what-is-a-virtual-machine"></a>Vad är en virtuell dator?

En virtuell dator, eller VM, är en programvaruemulering av en fysisk dator. Eftersom virtuella datorer finns som programvara kan dussintals, hundratals eller till och med tusentals virtuella Azure-datorer genereras på några minuter och sedan tas bort när du inte längre behöver dem. Med debitering per minut till en låg kostnad betalar du bara för de datorresurser du använder, så länge du använder dem. Dessutom finns det många sätt att konfigurera de virtuella datorerna så att de passar dina behov.

::: zone pivot="windows-cloud"

En ögonblicksbild av en virtuell dator som körs kallas för _avbildning_. Azure ger avbildningar för Windows och flera versioner av Linux. Du kan även skapa egna förkonfigurerade avbildningar så att distributioner går snabbare. Här skapar du en virtuell Windows Server 2016-dator, som tillhandahålls av Microsoft.

::: zone-end

::: zone pivot="linux-cloud"

En ögonblicksbild av en virtuell dator som körs kallas för _avbildning_. Azure ger avbildningar för Windows och flera versioner av Linux. Du kan även skapa egna förkonfigurerade avbildningar så att distributioner går snabbare. Här skapar du en virtuell Ubuntu 16.04-dator, som tillhandahålls av Canonical.

::: zone-end

## <a name="what-defines-a-virtual-machine-on-azure"></a>Vad definierar en virtuell dator på Azure?

En virtuell dator definieras av ett antal faktorer, till exempel storlek och plats. Innan du skapar den virtuella datorn tar vi en snabb titt vad som ingår.

:::row:::
    :::column:::
        **Storlek**
    :::column-end:::
    :::column span="3"::: En virtuell dators _storlek_ definierar dess processorhastighet, mängd minne och förväntade nätverksbandbredd. Vissa storlekar inbegriper även specialiserad maskinvara som grafikprocessorer för tung grafikrendering och videoredigering.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Region**
    :::column-end:::
    :::column span="3"::: Azure utgörs av datacenter över hela världen. En _region_ är en uppsättning Azure-datacenter på en namngiven geografisk plats. Varje Azure-resurs, bland annat virtuella datorer, tilldelas en region. USA, östra och Europa, norra är exempel på regioner.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Nätverk**
    :::column-end:::
    :::column span="3"::: En _virtuellt nätverk_ är ett logiskt isolerat nätverk på Azure. Varje virtuella dator på Azure associeras med ett virtuellt nätverk. Azure har brandväggar på molnnivå för dina virtuella nätverk som kallas för _nätverkssäkerhetsgrupper_.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Resursgrupper**
    :::column-end:::
    :::column span="3"::: Virtuella datorer och andra molnresurser grupperas i logiska containrar som kallas för _resursgrupper_. Grupper används vanligtvis för att organisera uppsättningar av resurser som distribueras tillsammans som en del av ett program eller en tjänst. Du refererar till en resursgrupp med dess namn.
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a>Vad är Azure Cloud Shell?

Azure Cloud Shell är en webbläsarbaserad kommandoradsmiljö för hantering och utveckling av Azure-resurser. Tänk på Cloud Shell som en interaktiv konsol som du kör i molnet.

Du kan välja mellan två olika miljöer i Cloud Shell: Bash och PowerShell. Båda har åtkomst till Azure CLI, kommandoradsgränssnittet för Azure.

Du kan använda valfritt Azure-hanteringsgränssnitt, som Azure Portal, Azure CLI och Azure PowerShell, för att hantera alla typer av virtuella datorer. I utbildningssyfte använder vi här Azure CLI för att skapa och hantera en virtuell Windows- eller Linux-dator.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="creating-resources-in-azure"></a>Skapa resurser i Azure

Normalt sett är det första vi gör att skapa en _resursgrupp_ för allt som behövs för att skapa. Detta låter oss administrera alla virtuella datorer, diskar, nätverksgränssnitt och andra element som utgör vår lösning som en enhet. Vi kan använda Azure CLI för att skapa en resursgrupp med kommandot `az group create`. Det tar en `--name` för att ge det ett unikt namn i vår prenumeration och en `--location` för att säga till Azure vilket område i världen där vi vill att resursen ska finnas som standard.

Eftersom det är en kostnadsfri Azure-Sandbox-miljö så behöver du inte göra det här steget, istället kommer du att använda den förskapade resursgruppen **<rgn>[Resursgruppens namn]</rgn>**.

## <a name="choosing-a-location"></a>Välj en plats

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

::: zone pivot="windows-cloud"

## <a name="create-a-windows-vm"></a>Skapa en virtuell Windows-dator

Nu ska vi sätta igång din virtuella Windows-dator.

1. I Cloud Shell på höger sida om den här sidan, kör du följande `az vm create`-kommando för att skapa din virtuella dator. Kommandot skapar den virtuella datorn på platsen USA, östra, du kan ändra till valfri annan plats som listas ovanför &mdash; vi rekommenderar att du väljer en nära dig.

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --location eastus \
      --admin-username azureuser
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    Även om du kan ange administratörslösenordet för Windows med kommandot, kommer du här att uppmanas ange ett. Välj ett lösenord som innehåller minst 12 tecken med en kombination av gemener och versaler, siffror och symboler. Använd inte ett lösenord som du använder någon annanstans.

    Det tar fyra till fem minuter för den virtuella datorn att bli aktiv. Jämför det med tiden det tar att köpa, sätta upp och konfigurera ett system i ditt datacenter. Stor skillnad!

Medan du väntar går vi igenom kommandot du precis har kört.

* Den virtuella datorn heter **myVM**. Namnet identifierar den virtuella datorn i Azure. Det blir även den virtuella datorns interna värdnamn, eller datornamn.
* Resursgruppen, eller den virtuella datorns logiska container, heter **<rgn>[Namn för Sandbox-resursgruppen]</rgn>**.
* **Win2016Datacenter** Anger den virtuella Windows Server 2016-datorns avbildning.
* **Standard_DS2_v2** anger storleken på den virtuella datorn. Den här storleken har två processorer och 7 GB minne.
* Med användarnamn och lösenord kan du ansluta till den virtuella datorn senare. Du kan till exempel ansluta via fjärrskrivbordet eller WinRM för att arbeta med och konfigurera systemet.

Som standard tilldelar Azure en offentlig IP-adress till den virtuella datorn. Du kan konfigurera en virtuell dator så att den är åtkomlig via internet eller bara via det interna nätverket.

Du kan även titta på den här korta videon om några av alternativen du har för att skapa och hantera virtuella datorer.

#### <a name="options-to-create-and-manage-vms"></a>Alternativ för att skapa och hantera virtuella datorer

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

När den virtuella datorn är klar visas informationen om den. Här är ett exempel.

```json
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1E-1B-3B",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "104.211.9.245",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

::: zone-end

::: zone pivot="linux-cloud"

## <a name="create-a-linux-vm"></a>Skapa en virtuell Linux-dator

Nu ska vi sätta igång din virtuella Linux-dator.

1. Från Cloud Shell på höger sida om den här sidan, kör du `az vm create`-kommandot för att skapa din virtuella dator. Följande kommando skapar den virtuella datorn på platsen USA, östra, du kan ändra till valfri annan plats som listas ovanför. Vi rekommenderar att du väljer en nära dig.

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image UbuntuLTS \
      --location eastus \
      --size Standard_DS2_v2 \
      --generate-ssh-keys
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    Det tar cirka två minuter för den virtuella datorn att bli aktiv. Jämför det med tiden det tar att köpa, sätta upp och konfigurera ett system i ditt datacenter. Stor skillnad!

Medan du väntar går vi igenom kommandot du precis har kört.

* Den virtuella datorn heter **myVM**. Namnet identifierar den virtuella datorn i Azure. Det blir även den virtuella datorns interna värdnamn, eller datornamn.
* Resursgruppen, eller den virtuella datorns logiska container, heter **<rgn>[Namn för Sandbox-resursgruppen]</rgn>**.
* **UbuntuLTS** anger den virtuella Ubuntu 16.04 LTS-datorns avbildning.
* **Standard_DS2_v2** anger storleken på den virtuella datorn. Den här storleken har två processorer och 7 GB minne.
* Alternativet `--generate-ssh-keys` skapar ett SSH-nyckelpar så att du kan logga in på den virtuella datorn.

Som standard tilldelar Azure en offentlig IP-adress till den virtuella datorn. Du kan konfigurera en virtuell dator så att den är åtkomlig via internet eller bara via det interna nätverket.

Du kan även titta på den här korta videon om några av alternativen du har för att skapa och hantera virtuella datorer.

#### <a name="options-to-create-and-manage-vms"></a>Alternativ för att skapa och hantera virtuella datorer

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

När den virtuella datorn är klar visas informationen om den. Här är ett exempel.

```json
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

::: zone-end

## <a name="summary"></a>Sammanfattning

Med hjälp av några få begrepp kan du skapa en virtuell dator i Azure på bara några minuter. Många av begreppen, som en virtuell dators storlek och brandväggsregler, känner du troligen redan till.

Nu ska du installera en webbserver på den virtuella datorn och konfigurera webbservern att leverera en grundläggande webbplats.