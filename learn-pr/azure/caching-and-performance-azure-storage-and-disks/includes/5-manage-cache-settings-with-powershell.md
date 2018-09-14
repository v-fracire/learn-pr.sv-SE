Att skapa administrationsskript är ett kraftfullt sätt att optimera ditt arbetsflöde på. Du kan automatisera vanliga, repetitiva uppgifter. När ett skript har verifierats, körs den enhetligt sätt, som kommer troligen att minska antalet fel. I den föregående övningen vi skapat en virtuell dator har lagts till en datadisk och ändrats cacheinställningarna allt via Azure-portalen. Vad händer om vi behövde för att upprepa dessa uppgifter på flera virtuella datorer i många regioner? Nu ska vi använda Azure PowerShell!

## <a name="what-is-azure-powershell"></a>Vad är Azure PowerShell?

Azure PowerShell är en modul som du lägger till Windows PowerShell så att du ansluter till din Azure-prenumeration och hantera resurser. Azure PowerShell kräver Windows PowerShell för att fungera. Windows PowerShell tillhandahåller tjänster som shell-fönstret, kommandot parsning och så vidare. Azure PowerShell lägger till Azure-specifika kommandon.

Till exempel Azure PowerShell tillhandahåller den **New-AzureRmVM** kommando som skapar en virtuell dator åt dig i din Azure-prenumeration. För att använda den, skulle du starta PowerShell-programmet och sedan utfärdar ett kommando som i följande exempel:

```powershell
New-AzureRmVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Azure PowerShell är tillgänglig på två sätt: i en webbläsare via Azure Cloud Shell eller med en lokal installation på Linux, Mac eller Windows.

## <a name="what-are-powershell-cmdlets"></a>Vad är PowerShell-cmdletar?

Ett PowerShell-kommando kallas för en **cmdlet** (uttalas ”command-let”). En cmdlet är ett kommando som manipulerar en enskild funktion. Termen **cmdlet** är avsedd att en ”små kommandot”. Enligt konventionen uppmuntras de som skriver cmdletar att hålla dem enkla med ett enda syfte.

PowerShell-grundprodukten levereras med cmdletar för funktioner som sessioner och bakgrundsjobb. Du lägger till moduler i PowerShell-installationen för att hämta cmdletar som manipulerar andra funktioner. Det finns till exempel moduler från tredje part för att arbeta med ftp, administrera operativsystemet, använda filsystemet och så vidare.

Cmdletar följer en namngivningskonvention med verb-substantiv, till exempel **Get-Process**, **Format-Table** eller **Start-Service**. Det finns också en konvention för verbet val: ”hämta” ”ange” för att infoga eller uppdatera data, ”format” att formatera data ”ut” om du vill hämta data till direkt utdata till ett mål och så vidare.

De som skriver cmdletar uppmanas att inkludera en hjälpfil för varje cmdlet. Cmdleten **Get-Help** visar hjälpfilen för en cmdlet:

```powershell
Get-Help <cmdlet-name> -detailed
```

## <a name="what-is-azurerm"></a>Vad är AzureRM?

**AzureRM** är formella namnet för Azure PowerShell-modulen som har en uppsättning cmdlets för att arbeta med Azure-funktioner (den **RM** i namnet står för **Resource Manager**). Den innehåller hundratals cmdletar som gör att du kan styra nästan alla aspekter av varje Azure-resurs. Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory, containers, maskininlärning och så vidare.

## <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>PowerShell-cmdletar för att hantera Azure diskcachelagring

Azure PowerShell har specifika cmdletar för att hantera virtuella datorer och diskar.

I följande tabell visas några av de cmdletar som vi ska använda i nästa övning:

|Kommando  |Beskrivning  |
|---------|---------|
|`Get-AzureRmVM`     |  Hämtar egenskaperna för en virtuell dator.       |
|`Update-AzureRmVM`     |  Uppdaterar tillståndet för en Azure virtuell dator.       |
|`New-AzureRmDiskConfig`     |  Skapar ett konfigurerbart disk-objekt.       |
|`Add-AzureRmVMDataDisk`     |  Lägger till en datadisk till en virtuell dator.   |

Vi använder dessa cmdlets i nästa övning för att hantera cachelagring på våra virtuella datorn.
