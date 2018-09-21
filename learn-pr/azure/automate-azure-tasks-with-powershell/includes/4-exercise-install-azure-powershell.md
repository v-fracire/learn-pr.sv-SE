I den här delen installerar du **PowerShell** på den lokala datorn.

> [!NOTE]
> I den här övningen får du hjälp att installera PowerShell-verktygen lokalt. Resten av modulen använder Azure Cloud Shell så att du kan utnyttja den kostnadsfria prenumerationssupporten i Microsoft Learn. Du kan se den här övningen som en valfri aktivitet och bara läsa anvisningarna om du vill.

::: zone pivot="linux"

## <a name="linux"></a>Linux

Installation av PowerShell för Linux inbegriper användning av en pakethanterare. Vi kommer att använda **Ubuntu 18.04** i exemplet här, men det finns [detaljerade instruktioner för andra versioner och distributioner i vår dokumentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).

Du ska installera PowerShell Core på Ubuntu Linux med (**APT**) och Bash-kommandoraden. 

1. Importera krypteringsnyckeln för Microsoft Ubuntu-lagringsplatsen. Då kan pakethanteraren verifiera att PowerShell Core-paketet som du installerar kommer från Microsoft.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. Registrera **Microsoft Ubuntu-lagringsplatsen** så att pakethanteraren kan hitta PowerShell Core-paketet.

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. Uppdatera listan över paket.

    ```bash
    sudo apt-get update
    ```

1. Installera PowerShell Core.

    ```bash
    sudo apt-get install -y powershell
    ```

1. Starta PowerShell för att verifiera att det har installerats korrekt.

    ```bash
    pwsh
    ```
::: zone-end

::: zone pivot="macos"

## <a name="macos"></a>MacOS

På macOS är det första steget att installera **PowerShell Core**. Detta görs med hjälp av Homebrew-pakethanteraren.

> [!IMPORTANT]
> Om kommandot **brew** inte är tillgängligt behöver du kanske installera Homebrew-pakethanteraren. Mer information finns på [Homebrews webbplats](https://brew.sh/).

1. Installera Homebrew-Cask för att hämta fler paket som PowerShell Core-paketet:

    ```bash
    brew tap caskroom/cask
    ```

1. Installera PowerShell Core:

    ```bash
    brew cask install powershell
    ```

1. Starta PowerShell Core för att verifiera att det har installerats korrekt:

    ```bash
    pwsh
    ```

::: zone-end

::: zone pivot="windows"

## <a name="windows"></a>Windows
PowerShell ingår i Windows, men det kan finnas en uppdatering tillgänglig för din dator. Den Azure-support som vi ska använda kräver PowerShell version 5.0 eller senare. Du kan kontrollera vilken version du har installerat med följande steg:

1. Öppna **Start-menyn** och skriv **Windows PowerShell**. Det kan finnas flera länkgenvägar:
    - Windows PowerShell – det här är 64-bitarsversionen och är allmänt det som du bör välja.
    - Windows PowerShell (x86) – det här är en 32-bitars version som är installerad på 64-bitars Windows.
    - Windows PowerShell ISE – Integrated Scripting Environment (ISE) används för att skriva skript i PowerShell. 
    - Windows PowerShell ISE (x86) – en 32-bitars version av ISE.

1. Välj Windows PowerShell ISE-ikonen.

1. Skriv följande kommando för att kontrollera vilken version av PowerShell som är installerad.

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
Om versionsnumret är lägre än 5.0 följer du dessa instruktioner för att [uppgradera det befintliga Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

::: zone-end

Du har konfigurerat dina lokala datorer för att stödja PowerShell. Därefter går vi igenom ytterligare kommandon som du kan lägga till, inklusive Azure-modulen.