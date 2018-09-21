I PowerShell kan du skriva kommandon och köra dem direkt. Detta kallas för **interaktivt läge**.

Kom ihåg att huvudmålet i CRM-exemplet Customer Relationship Management) är att skapa tre testmiljöer med virtuella datorer. Du kommer att använda resursgrupper för att se till att de virtuella datorerna är uppdelade i separata miljöer: en för enhetstestning, en för integrationstestning och en för acceptanstestning. Du behöver bara skapa resursgrupperna en gång, vilket gör det interaktiva läget för PowerShell till ett bra val.

När du anger ett kommando i PowerShell matchar den till en _cmdlet_ som sedan utför den begärda åtgärden. Vi ska gå igenom några vanliga kommandon som du kan använda och sedan titta på hur du installerar Azure-stöd för PowerShell.

## <a name="what-are-powershell-cmdlets"></a>Vad är PowerShell-cmdletar?
Ett PowerShell-kommando kallas för en **cmdlet** (uttalas ”command-let”). En cmdlet är ett kommando som manipulerar en enskild funktion. Termen **cmdlet** är tänkt att avse ett ”litet kommando”. Enligt konventionen uppmuntras de som skriver cmdletar att hålla dem enkla med ett enda syfte.

PowerShell-grundprodukten levereras med cmdletar för funktioner som sessioner och bakgrundsjobb. Du lägger till moduler i PowerShell-installationen för att hämta cmdletar som manipulerar andra funktioner. Det finns till exempel moduler från tredje part för att arbeta med ftp, administrera operativsystemet, använda filsystemet och så vidare.

Cmdletar följer en namngivningskonvention med verb-substantiv, till exempel **Get-Process**, **Format-Table** eller **Start-Service**. Dessutom finns en konvention för valet av verb: ”get” för att hämta data, ”set” för att infoga eller uppdatera data, ”format” för att formatera data, ”out” för att skicka utdata till ett mål och så vidare.

De som skriver cmdletar uppmanas att inkludera en hjälpfil för varje cmdlet. Cmdleten **Get-Help** visar hjälpfilen för en cmdlet. Vi kan till exempel få hjälp om cmdleten `Get-ChildItem` med följande uttryck:

```powershell
Get-Help Get-ChildItem -detailed
```

## <a name="what-is-a-powershell-module"></a>Vad är en PowerShell-modul?

Cmdletar levereras i _moduler_. En PowerShell-modul är en DLL-fil som innehåller kod för att behandla varje tillgänglig cmdlet. Du läser in cmdletar i PowerShell genom att läsa in den modul som de finns i. Du kan hämta en lista över inlästa moduler med kommandot `Get-Module`:

```powershell
Get-Module
```

Utdata blir ungefär så här:

```output
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Con...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Binary     1.0.0.1    PackageManagement                   {Find-Package, Find-PackageProvider, Get-Package, Get-Pack...
Script     1.0.0.1    PowerShellGet                       {Find-Command, Find-DscResource, Find-Module, Find-RoleCap...
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...
```

## <a name="what-is-azurerm"></a>Vad är AzureRM?
**AzureRM** är det formella namnet på Azure PowerShell-modulen som innehåller cmdletar för arbete med Azure-funktioner (**RM** i namnet står för **Resource Manager**). Den innehåller hundratals cmdletar som gör att du kan styra nästan alla aspekter av varje Azure-resurs. Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory, containrar, maskininlärning och så vidare. Denna modul är en komponent med öppen källkod som [finns på GitHub](https://github.com/Azure/azure-powershell).

> [!NOTE]
> Azure PowerShell-modulen är en valfri installation – cmdletarna är inte tillgängliga förrän du importerar modulen.

### <a name="install-the-azurerm-module"></a>Installera AzureRM-modulen

AzureRM-modulen är tillgänglig från en global lagringsplats som kallas PowerShell-galleriet. Du kan installera modulen på din lokala dator via kommandot `Install-Module`. Du behöver ha ett upphöjt PowerShell för att installera moduler från PowerShell-galleriet. 

::: zone pivot="windows"

För att installera den senaste Azure PowerShell-modulen kör du följande kommandon:

1. Öppna **Start-menyn** och skriv **Windows PowerShell**.

1. Högerklicka på ikonen för **Windows PowerShell** och välj **Kör som administratör**.

1. I dialogrutan **User Account Control** väljer du **Ja**.

1. Ange följande kommando och tryck på Enter:

    ```powershell
    Install-Module -Name AzureRM
    ```

Detta installerar modulen för alla användare som standard (styrs av omfångsparametern). 

Kommandot är beroende av NuGet för att hämta komponenter. Beroende på vilken version av NuGet du har installerat kan du få en uppmaning om att ladda ned och installera den senaste versionen av NuGet.

```output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
 provider must be available in 'C:\Program Files (x86)\PackageManagement\ProviderAssemblies' or
'C:\Users\<username>\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
 the NuGet provider now?
```

Som standard konfigureras inte PowerShell-galleriet som en betrodd lagringsplats för PowerShellGet. Första gången du använder PSGallery visas följande meddelande:

```output
You are installing the modules from an untrusted repository. If you trust this repository, change its
InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
'PSGallery'?
```

#### <a name="script-execution-failed"></a>Skriptkörningen misslyckades
Beroende på din säkerhetskonfiguration kan `Import-Module` misslyckas med något som liknar följande.

```output
import-module : File C:\Program Files (x86)\WindowsPowerShell\Modules\azurerm\6.8.1\AzureRM.psm1 cannot be loaded
because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ import-module azurerm
+ ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [Import-Module], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.PowerShell.Commands.ImportModuleCommand
```

Detta betyder vanligen att körningsprincipen är ”begränsad”, vilket innebär att du inte kan köra moduler som du laddar ned från en extern källa – inklusive PowerShell-galleriet. Du kan kontrollera om så är fallet genom att köra kommandot `Get-ExecutionPolicy`. Om det returnerar ”Restricted” (begränsad) gör du följande:

1. Öppna en upphöjd PowerShell-kommandotolk.
1. Använd cmdleten `SetExecutionPolicy` för att ändra principen till ”RemoteSigned”:

```powershell
Set-ExecutionPolicy RemoteSigned
```

Du uppmanas att ange behörighet:

```output
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
```

Sedan bör du kunna använda `Import-Module` för att läsa in cmdletarna.

:::zone-end

::: zone pivot="linux,macos"

Vi använder samma kommandon för att installera Azure PowerShell på antingen Linux eller macOS.

1. I en terminal skriver du följande kommando för att starta PowerShell Core med utökad behörighet.

    ```bash
    sudo pwsh
    ```

1. Kör följande kommando i Azure PowerShell-kommandotolken för att installera Azure PowerShell.

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. Om du blir tillfrågad om huruvida du litar på moduler från **PSGallery** svarar du **Ja** eller **Ja till alla**.

:::zone-end

### <a name="update-a-module"></a>Uppdatera en modul

Om du får en eller ett felmeddelande som anger att en version av Azure PowerShell-modulen redan är installerad kan du uppdatera till den _senaste_ versionen genom att köra kommandot:

:::zone pivot="windows"

```powershell
Update-Module -Name AzureRM
```

:::zone-end

::: zone pivot="linux,macos"

```powershell
Update-Module -Name AzureRM.NetCore
```

:::zone-end

Precis som för kommandot `Install-Module` ska du svara **Ja** eller **Ja till alla** när du blir uppmanad att lita på modulen. Du kan även använda kommandot `Update-Module` för att installera om en modul om du har problem med den.

## <a name="example-how-to-create-a-resource-group-with-azure-powershell"></a>Exempel: Så skapar du en resursgrupp med Azure PowerShell
När Azure-modulen har lästs in kan du börja arbeta med Azure. Vi utför en vanlig uppgift – skapa en resursgrupp. Som du vet använder vi resursgrupper för att administrera relaterade resurser tillsammans. Att skapa en ny resursgrupp är en av de första uppgifterna som du gör när du startar en ny Azure-lösning.

Det finns fyra steg som vi behöver utföra:

1. Importera Azure-cmdletarna.

1. Ansluta till Azure-prenumerationen.

1. Skapa resursgruppen.

1. Kontrollera att resursgruppen skapades korrekt (se nedan).

Följande illustration visar en översikt över dessa steg.

![En bild som visar hur du skapar en resursgrupp.](../media/5-create-resource-overview.png)

Varje steg motsvarar en specifik cmdlet.

### <a name="import-the-azure-cmdlets"></a>Importera Azure-cmdletarna
Vid starten läser PowerShell som standard bara in de viktigaste cmdletarna. Det innebär att de cmdletar du behöver till att arbeta med Azure inte är inlästa. Det säkraste sättet att läsa in de cmdletar du behöver är att importera dem manuellt i början av PowerShell-sessionen.

Du använder cmdleten **Import-Module** till att läsa in moduler. Den här cmdleten har många parametrar för att kunna hantera olika situationer. Den kan till exempel läsa in flera moduler, en viss modulversion, en del av en modul och så vidare.

Till exempel kan vi läsa in alla cmdletar för AzureRM med hjälp av följande kommando **i en upphöjd PowerShell-session**:

:::zone pivot="windows"

```powershell
Import-Module AzureRM
```

:::zone-end

::: zone pivot="linux,macos"

```powershell
Import-Module AzureRM.NetCore
```

:::zone-end

> [!TIP]
> Om du märker att du jobbar med Azure PowerShell ofta finns det två sätt att automatisera modulinläsningen. Du kan lägga till en post i din PowerShell-profil för import av Azure-moduler vid starten, eller så kan du använda den senaste versionen av PowerShell som läser in tillhörande modul automatiskt när du använder en cmdlet.

### <a name="connect"></a>Ansluta
När du arbetar med en lokal installation av Azure PowerShell måste du autentisera dig innan du kan köra Azure-kommandon. Cmdleten **Connect-AzureRmAccount** frågar efter dina Azure-autentiseringsuppgifter och ansluter sedan till din Azure-prenumeration. Den har många valfria parametrar, men om du bara behöver en interaktiv prompt behövs inga parametrar:

```powershell
Connect-AzureRmAccount
```

Du måste upprepa de här stegen för varje ny PowerShell-session som du startar eftersom den här modulen inte är en del av den grundläggande uppsättningen.


### <a name="working-with-subscriptions"></a>Arbeta med prenumerationer
Om du är nybörjare på Azure har du förmodligen bara en enda prenumeration. Men om du har använt Azure ett tag har du kanske skapat flera Azure-prenumerationer. Du kan konfigurera Azure PowerShell för att köra kommandon mot en viss prenumeration.

Du kan bara finnas i en prenumeration i taget. Använd cmdleten `Get-AzureRmContext` för att avgöra vilken prenumeration som är aktiv. Du kan ändra om det inte är korrekt.

1. Hämta en lista över alla prenumerationsnamn i ditt konto med kommandot `Get-AzureRmSubscription`. 

2. Ändra prenumerationen genom att skicka namnet på den som ska väljas.

```powershell
Select-AzureRmSubscription -Subscription "Visual Studio Enterprise"
```

### <a name="get-a-list-of-all-resource-groups"></a>Hämta en lista över alla resursgrupper

Du kan hämta en lista över alla resursgrupper i den aktiva prenumerationen:

```powershell
Get-AzureRmResourceGroup
```

Du kan få en tydligare vy om du skickar utdata från `Get-AzureRmResourceGroup` till cmdleten `Format-Table` med ett lodstreck ”|”.

```powershell
Get-AzureRmResourceGroup | Format-Table
```

Utdata blir ungefär så här:

```output
ResourceGroupName                  Location       ProvisioningState Tags TagsTable ResourceId
-----------------                  --------       ----------------- ---- --------- ----------
cloud-shell-storage-southcentralus southcentralus Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
ExerciseResources                  eastus         Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
```

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

När du skapar resurser i Azure kommer du alltid att placera dem i en resursgrupp för hanteringsändamål. En resursgrupp är ofta en av de första sakerna som du skapar när du startar ett nytt program.

Du kan skapa resursgrupper med cmdleten `New-AzureRmResourceGroup`. Du måste ange ett namn och en plats. Namnet måste vara unikt inom prenumerationen. Platsen avgör var metadata för resursgruppen lagras (det här kan vara viktigt när det gäller regelefterlevnad). Du anger platsen med strängar som ”USA, västra”, ”Europa, norra” eller ”Indien, västra”. Som de flesta Azure-cmdletar har `New-AzureRmResourceGroup` många valfria parametrar, men den grundläggande syntaxen är:

```powershell
New-AzureRmResourceGroup -Name <name> -Location <location>
```

> [!NOTE]
> Kom ihåg att vi kommer att arbeta i den Azure-sandbox-miljö som skapar en resursgrupp åt dig. Kommandot ovan skulle användas om du arbetar i din egen prenumeration.

### <a name="verify-the-resources"></a>Verifiera resurserna
`Get-AzureRmResource` visar en lista över dina Azure-resurser. Det här är användbart när du ska kontrollera att resursgruppen har skapats.

```powershell
Get-AzureRmResource
```

Som med kommandot `Get-AzureRmResourceGroup` kan du få en tydligare vy via cmdleten `Format-Table`. Här använder vi en förkortad version, `ft`:

```powershell
Get-AzureRmResource | ft
```

Du kan även filtrera det till specifika resursgrupper för att bara lista resurser som är associerade med de grupperna:

```powershell
Get-AzureRmResource -ResourceGroup ExerciseResources
```

### <a name="creating-an-azure-virtual-machine"></a>Skapa en virtuell Azure-dator

En annan gemensam uppgift som kan göras med PowerShell är att skapa virtuella datorer.

I Azure PowerShell kan du använda cmdleten `New-AzureRmVm` till att skapa en virtuell dator. Cmdleten har många parametrar för att hantera det stora antalet konfigurationsinställningar för virtuella datorer. De flesta av parametrarna har rimliga standardvärden, så vi behöver bara ange fem saker:

- **ResourceGroupName**: resursgruppen som den nya virtuella datorn ska placeras i.
- **Name**: namnet på den virtuella datorn i Azure.
- **Location**: den geografiska plats där den virtuella datorn ska etableras.
- **Credential**: ett objekt som innehåller användarnamnet och lösenordet för den virtuella datorns administratörskonto. Vi använder cdmleten `Get-Credential`. Denna cmdlet frågar efter ett användarnamn och ett lösenord och paketerar dem i ett autentiseringsuppgiftsobjekt.
- **Avbildning**: den operativsystemavbildning som ska användas för den virtuella datorn. Det här är ofta en Linux-distribution eller en Windows Server.

```powershell
   New-AzureRmVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

Du kan ange dessa parametrar direkt till cmdleten enligt ovan. Det går även att använda andra cmdletar för att konfigurera den virtuella datorn, till exempel `Set-AzureRmVMOperatingSystem`, `Set-AzureRmVMSourceImage`, `Add-AzureRmVMNetworkInterface` och `Set-AzureRmVMOSDisk`.

Här är ett exempel som kopplar ihop cmdleten `Get-Credential` med parametern `-Credential`:

```powershell
New-AzureRmVM -Name MyVm -ResourceGroupName ExerciseResources -Credential (Get-Credential) ...
```

Suffixet `AzureRmVM` är specifikt för VM-baserade kommandon i PowerShell. Det finns flera andra som du kan använda:

| Kommando | Beskrivning |
|---------|-------------|
| `Remove-AzureRmVM` | Tar bort en virtuell Azure-dator. |
| `Start-AzureRmVM` | Startar en stoppad virtuell dator. |
| `Stop-AzureRmVM` | Stoppar en virtuell dator som körs. |
| `Restart-AzureRmVM` | Startar om en virtuell dator. |
| `Update-AzureRmVM` | Uppdaterar konfigurationen för en virtuell dator. |

#### <a name="example-getting-the-information-for-a-vm"></a>Exempel: hämta informationen för en virtuell dator

Du kan lista de virtuella datorerna i din prenumeration med kommandot `Get-AzureRmVM -Status`. Detta kan även ange en virtuell dator med egenskapen `-Name`. Här tilldelar vi den till en PowerShell-variabel:

```powershell
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName ExerciseResources
```

Det intressanta är att det här är ett _objekt_ som du kan interagera med. Exempelvis kan du göra ändringar på objektet och sedan skicka tillbaka ändringarna till Azure med kommandot `Update-AzureRmVM`:

```powershell
$ResourceGroupName = "ExerciseResources"
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName $ResourceGroupName
$vm.HardwareProfile.vmSize = "Standard_DS3_v2"

Update-AzureRmVM -ResourceGroupName $ResourceGroupName  -VM $vm
```

Det interaktiva läget i PowerShell passar för engångsuppgifter. I vårt exempel använder vi troligtvis samma resursgrupp i hela projektet, så det är rimligt att skapa den interaktivt. Det interaktiva läget är ofta snabbare och enklare än att skriva och köra ett skript för den här sortens uppgift.