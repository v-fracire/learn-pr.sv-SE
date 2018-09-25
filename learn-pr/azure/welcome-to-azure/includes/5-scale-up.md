Din webbserver är igång men du inser att du behöver mer datorkraft för att göra upplevelsen bra för dina användare. Hur kan du få den virtuella datorn att fungera snabbare?

I ditt datacenter kan du flytta webbservern till kraftfullare maskinvara för att lösa prestandaproblemen. Problemet är att du måste köpa, sätta upp och driva det nya systemet. Med Azure är svaret mycket enklare.

Innan du skalar upp din virtuella dator till en kraftfullare storlek, låt oss först definiera vad skala innebär.

## <a name="what-is-scale"></a>Vad är skalning?

_Skalning_ innebär att lägga till bandbredd, minne eller datorkraft för att få bättre prestanda.  

Du kanske har hört termerna _skala upp_ och _skala ut_.

Uppskalning, eller vertikal skalning betyder att öka minnet, lagringsutrymmet eller datorkraften på en befintlig virtuell dator. Du kan till exempel lägga till mer minne på en webb- eller databasserver så att den körs snabbare.

Utskalning, eller horisontell skalning, betyder att driva ett program genom flera virtuella datorer. Du kan till exempel skapa många virtuella datorer som konfigureras på exakt samma sätt och använda en belastningsutjämnare för att fördela arbetet på datorerna.

> [!TIP]
> Molnet är elastisk. Du kan _skala ned_ eller _skala in_ din distribution om du bara behöver skala upp eller skala ut tillfälligt. Att skala ned eller skala in kan hjälpa dig att spara pengar.<br><br>**Azure Advisor** och **Azure Cost Management** är två tjänster som hjälper dig optimera molnutgifter. Du kan använda de här tjänsterna för att identifiera var du använder mer än du behöver och sedan skala tillbaka till den kapacitet som du faktiskt använder.

## <a name="scale-up-your-vm"></a>Skala upp din virtuella dator

Kom ihåg att du angav storleken **Standard_DS2_v2** när du skapade din virtuella dator. Den virtuella datorn har två virtuella processorer och 7 GB minne.

Vi uppgraderar till nästa storlek **Standard_DS3_v2**. Den virtuella datorn har då fyra virtuella processorer och 14 GB minne.

::: zone pivot="windows-cloud"

1. Från Cloud Shell kör du `az vm resize` för att öka storleken på din virtuella dator till **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    Uppdateringen tar ungefär en minut. Din virtuella dator startas om under processen.

1. Kör `az vm show` för att verifiera att den virtuella datorn körs med den nya storleken.

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Du ser den nya storleken på den virtuella datorn, **Standard_DS3_v2**.
    ```output
    Standard_DS3_v2
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. Från Cloud Shell kör du `az vm resize` för att öka storleken på din virtuella dator till **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    Uppdateringen tar ungefär en minut. Din virtuella dator startas om under processen.

1. Kör `az vm show` för att verifiera att den virtuella datorn körs med den nya storleken.

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Du ser den nya storleken på den virtuella datorn, **Standard_DS3_v2**.
    ```output
    Standard_DS3_v2
    ```

::: zone-end

## <a name="summary"></a>Sammanfattning

Bra jobbat! Med bara ett kommando har din virtuella datorn nu blivit dubbelt så kraftfull.

Att skala upp och skala ut är två sätt att öka prestandan. Här har du skalat upp den virtuella datorn för att öka dess datorkraft.