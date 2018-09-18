Att skapa administrationsskript är ett kraftfullt sätt att optimera ditt arbetsflöde. Du kan automatisera återkommande, repetitiva uppgifter. När ett skript har verifierats körs det konsekvent, vilket troligen minskar förekomsten av fel. I den föregående övningen skapade vi en virtuell dator, lade till en datadisk och ändrade cacheinställningarna – allt via Azure-portalen. Hur går det till om vi behöver upprepa dessa uppgifter för många virtuella datorer i många regioner? Vi använder Azure Powershell!

## <a name="what-is-azure-powershell"></a>Vad är Azure PowerShell?
Azure PowerShell är en modul som du lägger till i PowerShell så att du kan ansluta till din Azure-prenumeration och hantera resurser. Azure PowerShell kräver PowerShell för att fungera. PowerShell tillhandahåller tjänster som gränssnittsfönster, kommandoparsning och så vidare. Azure PowerShell lägger till Azure-specifika kommandon.

Azure PowerShell tillhandahåller till exempel kommandot **New-AzureRmVM**, som skapar en virtuell dator åt dig i din Azure-prenumeration. För att använda det startar du PowerShell-programmet och sedan anger ett kommando som i det här exemplet:

```powershell
New-AzureRmVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Azure PowerShell finns tillgängligt på två sätt: i en webbläsare via Azure Cloud Shell eller via en lokal installation på Linux, Mac eller Windows.

## <a name="what-are-powershell-cmdlets"></a>Vad är PowerShell-cmdletar?

Ett PowerShell-kommando kallas för en **cmdlet** (uttalas ”command-let”). En cmdlet är ett kommando som manipulerar en enskild funktion. Termen **cmdlet** är tänkt att avse ett ”litet kommando”. Enligt konventionen uppmuntras de som skriver cmdletar att hålla dem enkla med ett enda syfte.

PowerShell-grundprodukten levereras med cmdletar för funktioner som sessioner och bakgrundsjobb. Du lägger till moduler i PowerShell-installationen för att hämta cmdletar som manipulerar andra funktioner. Det finns till exempel moduler från tredje part för att arbeta med ftp, administrera operativsystemet, använda filsystemet och så vidare.

Cmdletar följer en namngivningskonvention med verb-substantiv, till exempel **Get-Process**, **Format-Table** eller **Start-Service**. Dessutom finns en konvention för valet av verb: ”get” för att hämta data, ”set” för att infoga eller uppdatera data, ”format” för att formatera data, ”out” för att skicka utdata till ett mål och så vidare.

De som skriver cmdletar uppmanas att inkludera en hjälpfil för varje cmdlet. Cmdleten **Get-Help** visar hjälpfilen för en cmdlet:

```
Get-Help <cmdlet-name> -detailed
```
## <a name="what-is-azurerm"></a>Vad är AzureRM?

**AzureRM** är det formella namnet på Azure PowerShell-modulen som har en uppsättning cmdletar för arbete med Azure-funktioner (**RM** i namnet står för **Resource Manager**). Den innehåller hundratals cmdletar som gör att du kan styra nästan alla aspekter av varje Azure-resurs. Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory, containers, maskininlärning och så vidare.

## <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>PowerShell-cmdletar för att hantera Azure-diskcachelagring

Azure PowerShell har specifika cmdletar som hjälper till med hanteringen av virtuella datorer och diskar. 

I följande tabell visas några av de cmdlets som vi kommer att använda i nästa övning.

|Kommando  |Beskrivning  |
|---------|---------|
|`Get-AzureRmVM`     |  Hämtar egenskaperna för en virtuell dator.       |        $myVM
|`Update-AzureRmVM`     |  Uppdaterar tillståndet för en virtuell Azure-dator.       |        
|`New-AzureRmDiskConfig`     |  Skapar ett konfigurerbart diskobjekt.       |        
|`Add-AzureRmVMDataDisk`     |  Lägger till en datadisk till en virtuell dator.   |      


Vi använder dessa cmdlets i nästa övning för att hantera cachelagring på den virtuella datorn.
