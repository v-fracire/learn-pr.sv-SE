I Azure PowerShell kan du skriva kommandon och köra dem direkt. Detta kallas för **interaktivt läge**.

Kom ihåg att huvudmålet i CRM-exemplet Customer Relationship Management) är att skapa tre testmiljöer med virtuella datorer. Du kommer att använda resursgrupper för att se till att de virtuella datorerna är uppdelade i separata miljöer: en för enhetstestning, en för integrationstestning och en för acceptanstestning. Du behöver bara skapa resursgrupperna en gång, vilket gör det interaktiva läget för PowerShell till ett bra val.

I det här avsnittet visas några exempel på hur du kan använda PowerShell interaktivt till att logga in på din Azure-prenumeration och skapa resursgrupper.

## <a name="what-are-powershell-cmdlets"></a>Vad är PowerShell-cmdletar?
Ett PowerShell-kommando kallas för en **cmdlet** (uttalas ”command-let”). En cmdlet är ett kommando som manipulerar en enskild funktion. Termen **cmdlet** är tänkt att avse ett ”litet kommando”. Enligt konventionen uppmuntras de som skriver cmdletar att hålla dem enkla med ett enda syfte.

PowerShell-grundprodukten levereras med cmdletar för funktioner som sessioner och bakgrundsjobb. Du lägger till moduler i PowerShell-installationen för att hämta cmdletar som manipulerar andra funktioner. Det finns till exempel moduler från tredje part för att arbeta med ftp, administrera operativsystemet, använda filsystemet och så vidare.

Cmdletar följer en namngivningskonvention med verb-substantiv, till exempel **Get-Process**, **Format-Table** eller **Start-Service**. Dessutom finns en konvention för valet av verb: ”get” för att hämta data, ”set” för att infoga eller uppdatera data, ”format” för att formatera data, ”out” för att skicka utdata till ett mål och så vidare.

De som skriver cmdletar uppmanas att inkludera en hjälpfil för varje cmdlet. Cmdleten **Get-Help** visar hjälpfilen för en cmdlet:

```powershell
Get-Help <cmdlet-name> -detailed
```

## <a name="what-is-azurerm"></a>Vad är AzureRM?
**AzureRM** är det formella namnet på Azure PowerShell-modulen som innehåller cmdletar för arbete med Azure-funktioner (**RM** i namnet står för **Resource Manager**). Den innehåller hundratals cmdletar som gör att du kan styra nästan alla aspekter av varje Azure-resurs. Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory, containers, maskininlärning och så vidare.

## <a name="how-to-create-a-resource-group"></a>Så skapar du en resursgrupp
I nästa del kommer vi att skapa en resursgrupp med en lokal installation av Azure PowerShell. 

Det görs i fyra steg: 

1. Importera Azure-cmdletarna.

1. Ansluta till Azure-prenumerationen.

1. Skapa resursgruppen.

1. Kontrollera att resursgruppen skapades korrekt (se nedan).

Följande illustration visar en översikt över dessa steg.

![En bild som visar hur du skapar en resursgrupp.](../media/5-create-resource-overview.png)

Varje steg motsvarar en specifik cmdlet.

### <a name="import"></a>Importera
Vid starten laddar PowerShell som standard bara de viktigaste cmdletarna. Det innebär att de cmdletar du behöver till att arbeta med Azure inte är inlästa. Det säkraste sättet att läsa in de cmdletar du behöver är att importera dem manuellt i början av PowerShell-sessionen.

Du använder cmdleten **Import-Module** till att läsa in moduler. Den här cmdleten har många parametrar för att kunna hantera olika situationer. Den kan till exempel läsa in flera moduler, en viss modulversion, en del av en modul och så vidare. Om du vill läsa in en modul i sin helhet är syntaxen helt enkelt följande:

```powershell
Import-Module <module-name>
```

> [!TIP]
> Om du märker att du jobbar med Azure PowerShell ofta finns det två sätt att automatisera modulinläsningen. Du kan lägga till en post i din PowerShell-profil för import av Azure-moduler vid starten, eller så kan du använda den senaste versionen av PowerShell som läser in tillhörande modul automatiskt när du använder en cmdlet.

### <a name="connect"></a>Ansluta
Eftersom du arbetar med en lokal installation av Azure PowerShell måste du autentisera dig innan du kan köra Azure-kommandon. Cmdleten **Connect-AzureRmAccount** frågar efter dina Azure-autentiseringsuppgifter och ansluter sedan till din Azure-prenumeration. Den har många valfria parametrar, men om du bara behöver en interaktiv prompt behövs inga parametrar:

```powershell
Connect-AzureRmAccount
```

### <a name="create"></a>Skapa
Cmdleten **New-AzureRmResourceGroup** skapar en resursgrupp. Du måste ange ett namn och en plats. Namnet måste vara unikt inom prenumerationen. Platsen avgör var metadata för resursgruppen lagras (det här kan vara viktigt när det gäller regelefterlevnad). Du anger platsen med strängar som ”USA, västra”, ”Europa, norra” eller ”västra Indien”. Precis som de flesta Azure-cmdletar så har **New-AzureRmResourceGroup** många valfria parametrar, men syntaxen är i grunden följande:

```powershell
New-AzureRmResourceGroup -Name <name> -Location <location>
```

### <a name="verify"></a>Verifiera
**Get-AzureRmResource** visar en lista med dina Azure-resurser. Det här är användbart när du ska kontrollera att resursgruppen har skapats.

```powershell
Get-AzureRmResource
```

Du kan få en tydligare vy om du skickar utdata från **Get-AzureRmResource** till cmdleten **Format-Table** med ett lodstreck ”|”.

```powershell
Get-AzureRmResource | Format-Table
```

## <a name="summary"></a>Sammanfattning
Det interaktiva läget i PowerShell passar för engångsuppgifter. I vårt exempel använder vi samma resursgrupp i hela projektet, så det är rimligt att skapa den interaktivt. Det interaktiva läget är ofta snabbare och enklare än att skriva och köra ett skript för den här sortens uppgift.