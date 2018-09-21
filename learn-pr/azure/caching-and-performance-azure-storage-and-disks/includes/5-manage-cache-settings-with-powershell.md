Att skapa administrationsskript är ett kraftfullt sätt att optimera ditt arbetsflöde. Du kan automatisera återkommande, repetitiva uppgifter. När ett skript har verifierats körs det konsekvent, vilket troligen minskar förekomsten av fel. I den föregående övningen skapade vi en virtuell dator, lade till en datadisk och ändrade cacheinställningarna – allt via Azure Portal. Hur går det till om vi behöver upprepa dessa uppgifter för många virtuella datorer i många regioner? Vi kan göra de med Azure PowerShell.

> [!TIP]
> Vi beskriver Azure PowerShell i detalj i modulen **Automatisera uppgifter för Azure med PowerShell**. Kolla in modulen om du vill ha mer information om att installera, konfigurera och använda PowerShell.

## <a name="what-is-azure-powershell"></a>Vad är Azure PowerShell?

Azure PowerShell är ett plattformsoberoende kommandoradsverktyg för att ansluta till din Azure-prenumeration och hantera resurser. Det är en kombination av två saker: **PowerShell**, vilket ger stöd för kommandoradsverktyget och **AzureRM**, som tillhandahåller kommandon (kallas ”cmdletar”) för att fungera med Azure. 

Azure PowerShell har cmdletar för att styra det mesta när det gäller Azure-resurser. Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory, containers, maskininlärning och så vidare. Allt detta tas upp i andra utbildningsmoduler.

### <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>PowerShell-cmdletar för att hantera Azure-diskcachelagring

Azure PowerShell har specifika cmdletar som hjälper till med hanteringen av virtuella datorer och diskar.

|Kommando  | Beskrivning |
|---------|-------------|
| `Get-AzureRmVM`         | Hämtar egenskaperna för en virtuell dator.       |
| `Update-AzureRmVM`      | Uppdaterar tillståndet för en virtuell Azure-dator.  |
| `New-AzureRmDiskConfig` | Skapar ett konfigurerbart diskobjekt.             |
| `Add-AzureRmVMDataDisk` | Lägger till en datadisk till en virtuell dator.          |

Med dessa kan utföra alla aktiviteter som vi gjorde i Azure Portal. Nu ska vi prova på vår virtuella dator.
