Du kan konfigurera inställningar för virtuell datorcachelagring med något av följande verktyg:

- Azure-portalen
- Resource Manager-mallar
- Azure CLI
- Azure PowerShell

I nästa övning använder vi portalen för att skapa en virtuell dator och konfigurera cachelagring på dess diskar. Här är lite information att ha i åtanke. 

När du etablerar en ny virtuell dator med hjälp av Azure-portalen kan du inte ändra standardkonfigurationen för cachelagring för OS-disken från Läsa/skriva förrän den virtuella datorn har distribuerats.

När du lägger till en datadisk till en befintlig virtuell dator kan du konfigurera cachelagringsalternativet innan disken distribueras till den virtuella datorn.

När du ändrar cacheinställningen för en Azure-disk så frånkopplas och återansluts måldisken. Om det är operativsystemdisken startas den virtuella datorn om. Stoppa alla program/tjänster som kan påverkas av det här avbrottet innan du ändrar inställningen för diskcachelagring.

Nu skapar vi en virtuell dator och ändrar inställningar för cachelagring med hjälp av Azure-portalen.
