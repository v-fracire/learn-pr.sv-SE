Du kan konfigurera inställningar för cachelagring av VM-disk med någon av följande verktyg:

- Azure Portal
- Mallar för Resurshanteraren
- Azure CLI
- Azure PowerShell

I nästa övning kommer vi att använda portalen för att skapa en virtuell dator och konfigurerar cachelagring på dess diskar. Här är lite information att tänka på.

När du etablerar en ny virtuell dator med Azure portal kan ändra du inte standardinställningarna för cachelagring konfiguration för OS-disken från skrivbara tills den virtuella datorn har distribuerats.

När du lägger till en datadisk till en befintlig virtuell dator kan du konfigurera alternativet cache innan disken har distribuerats till den virtuella datorn.

Ändra cache-inställningen för en Azure-disk frånkopplas och återansluts sedan måldisken. Om det är ingen operativsystemdisk startas den virtuella datorn. Stoppa alla program/tjänster som kan påverkas av den här avbrott innan du ändrar inställningen för disk-cache.

Nu ska vi skapa en virtuell dator och ändra inställningar för cachelagring med Azure portal.
