Komplexa och repetitiva aktiviteter kan ofta kräva mycket administration. Organisationer vill gärna automatisera sådana uppgifter för att minska kostnaderna och undvika fel.

Det här är viktigt i CRM-exemplet (Customer Relationship Management). Där får du testa din programvara på flera virtuella Linux-datorer (VM) som du tar bort och återskapar upprepade gånger. Det är en bra idé att skapa de virtuella datorerna automatiskt med ett PowerShell-skript.

Förutom grundåtgärden att skapa en virtuell dator finns ytterligare några krav på skriptet. 
- Du kommer att skapa flera virtuella datorer, så det är bra att placera åtgärden i en loop
- Du skapar de virtuella datorerna i tre olika resursgrupper, så namnet på resursgruppen bör överföras till skriptet som en parameter

I det här avsnittet visas hur du skriver och kör ett Azure PowerShell-skript som uppfyller de här kraven.

## <a name="what-is-a-powershell-script"></a>Vad är ett PowerShell-skript?
Ett PowerShell-skript är en textfil som innehåller kommandon och styrelement. Kommandona anropar cmdletar. Styrelementen är programkonstruktioner som loopar, variabler, parametrar, kommentarer och annat som finns i PowerShell.

PowerShell-skriptfiler har filtillägget **.ps1**. Du kan skapa och spara de här filerna i valfri textredigerare. 

> [!TIP]
> Om du skriver PowerShell-skript i Windows kan du använda Windows integrerade skriptmiljö för PowerShell (ISE). Den här redigeraren har funktioner som syntaxfärgning och en lista med tillgängliga cmdletar.
>
I följande bild visas Windows PowerShell Integrated Scripting Environment (ISE) med ett exempelskript som ansluter till Azure och skapar en virtuell dator.

>![Skärmbild av Windows PowerShell Integrated Scripting Environment med ett skript som skapar en virtuell dator i redigeringsfönstret.](../media/7-windows-powershell-ise-screenshot.png)

När du har skrivit skriptet kör du det från PowerShell-kommandoraden genom att skriva en punkt och ett bakstreck följt av namnet på filen:

```powershell
.\myScript.ps1
```

## <a name="powershell-techniques"></a>PowerShell-tekniker
PowerShell har många funktioner som finns i vanliga programmeringsspråk. Du kan definiera variabler, använda grenar och loopar, inhämta kommandoradsparametrar, skriva funktioner, lägga till kommentarer och så vidare. Vi kommer att använda tre funktioner i vårt skript: variabler, loopar och parametrar.

### <a name="variables"></a>Variabler
PowerShell har stöd för variabler. Använd **$** för att deklarera en variabel och **=** för att tilldela ett värde. Exempel:

```powershell
$loc = "East US"
$iterations = 3
```

Variabler kan innehålla objekt. Följande definition anger exempelvis variabeln **adminCredential** till objektet som returneras av cmdleten **Get-Credential**.

```powershell
$adminCredential = Get-Credential
```

Du kan komma åt värdet som lagras i en variabel genom att använda prefixet **$** och variabelns namn som i exemplet nedan: 

```powershell
$loc = "East US"
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location $loc
```

### <a name="loops"></a>Loopar
PowerShell har flera loopar: **For**, **Do...While**, **For...Each** och så vidare. **For**-loopen passar bäst för våra behov eftersom vi ska köra en cmdlet ett visst antal gånger.

Den huvudsakliga syntaxen visas nedan. Exemplet körs i två iterationer och skriver ut värdet för **i** varje gång. Jämförelseoperatorerna skrivs som **-lt** för ”mindre än”, **-le** för ”mindre än eller lika med”, **eq** för ”lika med”, **ne** för ”inte lika med” och så vidare.

```powershell
For ($i = 1; $i -lt 3; $i++)
{
    $i
}
```

### <a name="parameters"></a>Parametrar
När du kör ett skript kan du skicka argument på kommandoraden. Du kan ange namn för varje parameter så att deras värden kan användas i skriptet. Exempel:

```powershell
.\setupEnvironment.ps1 -size 5 -location "East US"
```

I skriptet tar du värdena och gör dem till variabler. I det här exemplet matchas parametrarna med hjälp av namnen:

```powershell
param([string]$location, [int]$size)
```

Du kan utelämna namnen på kommandoraden. Exempel:

```powershell
.\setupEnvironment.ps1 5 "East US"
```

När parametrarna inte har namn används deras placering till matchning i skriptet:

```powershell
param([int]$size, [string]$location)
```

## <a name="how-to-create-a-linux-virtual-machine"></a>Skapa en virtuell Linux-dator
I Azure PowerShell kan du använda cmdleten **New-AzureRmVm** till att skapa en virtuell dator. Cmdleten har många parametrar för alla konfigurationsinställningar för virtuella datorer. De flesta av parametrarna har rimliga standardvärden, så vi behöver bara ange fem saker:

- **ResourceGroupName**: resursgruppen som den nya virtuella datorn ska placeras i.
- **Name**: namnet på den virtuella datorn i Azure.
- **Location**: den geografiska plats där den virtuella datorn ska etableras.
- **Credential**: ett objekt som innehåller användarnamn och lösenord för den virtuella datorns administratörskonto. Vi använder cmdleten **Get-Credential** till att fråga efter användarnamn och lösenord. **Get-Credential** placerar användarnamnet och lösenordet i ett credential-objekt som returneras som resultat.
- **Image**: identiteten för operativsystemet som ska användas i den virtuella datorn. Vi använder ”UbuntuLTS”.

Syntaxen för cmdleten visas nedan:

```powershell
   New-AzureRmVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

## <a name="summary"></a>Sammanfattning
I PowerShell och Azure PowerShell har du alla verktyg du behöver för att automatisera Azure. I vårt CRM-exempel kommer vi att kunna skapa flera virtuella Linux-datorer. Vi använder en parameter som gör skriptet generellt och en loop som gör att vi inte behöver upprepa koden. Det innebär att en åtgärd som tidigare var komplicerad nu kan köras i ett enda steg.