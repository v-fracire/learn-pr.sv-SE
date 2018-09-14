Webbservern är igång och körs, men du inser du behöver mer bearbetningskraft att göra upplevelsen bra för dina användare. Hur kan du skapa den virtuella datorn körs snabbare?

Du kan flytta webbservern till kraftfullare maskinvara för att lösa problem med prestanda i ditt datacenter. Problemet är att du behöver köpa, rack och starta nya systemet. Svaret är mycket enklare med Azure.

Nu kan du skala upp din virtuella dator till en kraftfullare storlek. Innan du ändrar den Virtuella datorns storlek, måste du stänga av eller frigör den.

Först ska vi definiera vilken skala innebär och vad som händer när du frigör den virtuella datorn.

## <a name="what-is-scale"></a>Vad är skala?

_Skala_ avser att lägga till nätverkets bandbredd, minne, lagring eller datorkraft för att få bättre prestanda.  

Du kanske har hört villkoren _skala upp_ och _skala ut_.

Skala upp eller vertikal skalning innebär att öka minne, lagring, eller beräkningskraft på en befintlig virtuell dator. Exempelvis kan du lägga till ytterligare minne till en webb- eller database-server så att de blir snabbare.

> [!TIP]
> Du kan också _Nedskalning_ systemet om du behöver skala upp bara tillfälligt.

Skala ut eller horisontell skalning innebär att ytterligare virtuella datorer att köra ditt program. Du kan till exempel skapa många virtuella datorer som konfigurerats på exakt samma sätt och använder en belastningsutjämnare för att fördela arbetet mellan dem.

## <a name="what-is-deallocation"></a>Vad är frigörs?

Flyttningen är den process som stänger av den virtuella datorn och släpper dess beräkningsresurser.

När du stänger av datorn på arbetet eller i hemmet, operativsystemet stänger program och sedan meddelar power management maskinvara att stänga av datorn.

Frigörs en virtuell dator är liknande. När den virtuella datorn stängs av, återtar Azure den maskinvara som används för att starta den. Dina diskar och lagring förblir intakta. När du startar den virtuella datorn säkerhetskopiera, ta vid där du slutade, som precis med din dator.

När datorn är frigjord kan debiteras du inte för de resurser för beräkning och nätverk som den virtuella datorn använder. Du betala för alla associerade diskar direkt på lagring, men den totala kostnaden är mycket lägre än det skulle vara om den virtuella datorn kördes.

Här får du frigör den virtuella datorn kort så att du kan ändra storlek på den. Men du kan också Frigör dina virtuella datorer under en längre period för att minska kostnaderna. Anta att du har en bank för virtuella datorer som du använder för att testa under arbetstid. Du kan schemalägga dina virtuella datorer ska frigöras automatiskt på nätter och helger. Om du vill hålla sent kan du manuellt starta om dem.

## <a name="scale-up-your-vm"></a>Skala upp din virtuella dator

Kom ihåg att du har angett storleken **Standard_DS2_v2** när du skapade den virtuella datorn. Den virtuella datorn har för närvarande två virtuella processorer och 7 GB minne.

Nu ska vi uppgraderar nästa storlek **Standard_DS3_v2**. Den virtuella datorn kommer då ha fyra virtuella processorer och 14 GB minne.

::: zone pivot="windows-cloud"

1. Kör från Cloud Shell `az vm deallocate` att frigöra eller stoppa den virtuella datorn.

    ```azurecli
    az vm deallocate \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM
    ```
    Processen tar några minuter att slutföra.
1. Kör `az vm resize` att öka storleken på den virtuella datorn till **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM \
      --size Standard_DS3_v2
    ```
    Uppdateringen tar ungefär en minut.
1. Kör `az vm start` att starta om den virtuella datorn.

    ```azurecli
    az vm start \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM
    ```
    Processen tar ungefär en minut.
1. Kör `az vm show` att verifiera att Virtuellt datorn körs den nya storleken.

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Du ser din nya VM-storleken **Standard_DS3_v2**.
    ```console
    Standard_DS3_v2
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. Kör från Cloud Shell `az vm deallocate` att frigöra eller stoppa den virtuella datorn.

    ```azurecli
    az vm deallocate \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM
    ```
    Processen tar några minuter att slutföra.
1. Kör `az vm resize` att öka storleken på den virtuella datorn till **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM \
      --size Standard_DS3_v2
    ```
    Uppdateringen tar ungefär en minut.
1. Kör `az vm start` att starta om den virtuella datorn.

    ```azurecli
    az vm start \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM
    ```
    Processen tar ungefär en minut.
1. Kör `az vm show` att verifiera att Virtuellt datorn körs den nya storleken.

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Du ser din nya VM-storleken **Standard_DS3_v2**.
    ```console
    Standard_DS3_v2
    ```

::: zone-end

> [!NOTE]
> Som standard tilldelar Azure en ny offentlig IP-adress till den virtuella datorn när du stoppar och startar du om den. Det här är OK i utbildningssyfte. I praktiken, kan du reservera en offentlig IP-adress som ligger med den virtuella datorn även om den virtuella datorn har startats om.

## <a name="summary"></a>Sammanfattning

Bra jobbat! Med bara några få kommandon, den virtuella datorn är nu två gånger så kraftfulla.

Skala upp och skala ut finns två sätt att öka prestandan. Här skalas upp den virtuella datorn att öka sin beräkningskraft.

Du kan frigöra en virtuell dator innan du kan ändra storlek på den. Du kan också Frigör dina virtuella datorer när de inte används för att sänka kostnaderna.