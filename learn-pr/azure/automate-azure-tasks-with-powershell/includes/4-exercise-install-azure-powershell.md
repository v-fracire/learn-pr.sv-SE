I den här delen installerar du **Azure PowerShell** på den lokala datorn. Välj rätt avsnitt för ditt operativsystem.

## <a name="linux-and-mac"></a>Linux och Mac
På Linux och macOS är det första steget att installera **PowerShell Core**.

### <a name="linux"></a>Linux
Som vi nämnde i den sista delen inbegriper installation av PowerShell för Linux användning av en pakethanterare. Vi kommer att använda **Ubuntu 18.04** i vårt exempel här, men vi har [detaljerade instruktioner för andra versioner och distributioner i vår dokumentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).

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

1. Starta PowerShell för att verifiera att det har installerats ordentligt.

    ```bash
    pwsh
    ```

### <a name="macos"></a>macOS
Installera därefter **PowerShell Core** på macOS med Homebrew-pakethanteraren.

> [!IMPORTANT]
> Om kommandot **brew** är otillgängligt kanske du måste installera Homebrew-pakethanteraren. Mer information finns på [Homebrews webbplats](https://brew.sh/).

1. Installera Homebrew-Cask för att hämta fler paket som PowerShell Core-paketet:

    ```bash
    brew tap caskroom/cask
    ```

1. Installera PowerShell Core:

    ```bash
    brew cask installs powershell
    ```

1. Starta PowerShell Core för att verifiera att det har installerats ordentligt:

    ```bash
    pwsh
    ```

## <a name="install-azure-powershell"></a>Installera Azure PowerShell
När du har installerat den grundläggande **PowerShell**-produkten installerar du **Azure PowerShell** för att lägga till de Azure-specifika kommandona.

### <a name="windows"></a>Windows
Installera Azure PowerShell på Windows med `Install-Module` PowerShell-kommandot.

> [!IMPORTANT]
> Du måste ha PowerShell-version 5.0 eller senare för att installera Azure PowerShell. Kontrollera din version av PowerShell med följande kommando: 
>
> `$PSVersionTable.PSVersion` 
>
>Om versionsnumret är lägre än 5.0 följer du instruktionerna för att [uppgradera din befintliga Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

1. Öppna **startmenyn** och skriv **Windows PowerShell**.

1. Högerklicka på ikonen för **Windows PowerShell** och välj **Kör som administratör**.

1. I dialogrutan **User Account Control** väljer du **Ja**.

1. Ange följande kommando och tryck på Enter:

    ```powershell
    Install-Module -Name AzureRM
    ```

1. Om du blir tillfrågad om du litar på moduler från PSGallery svarar du **Ja** eller **Ja till alla**.

> [!TIP]
> Om du får ett felmeddelande som anger att en version av Azure PowerShell-modulen redan är installerad kan du uppdatera till den _senaste_ versionen genom att köra kommandot:
> 
> `Update-Module -Name AzureRM`
> 
> Precis som för kommandot `Install-Module` ska du svara **Ja** eller **Ja till alla** när du blir uppmanad att lita på modulen.

### <a name="linux-or-macos"></a>Linux eller macOS
Vi använder samma grundläggande process för att installera Azure PowerShell på antingen Linux eller macOS. Proceduren är densamma för båda operativsystemen. Skillnaden gäller hämtning av en utökad PowerShell Core-session.

1. I en terminal skriver du följande kommando för att starta PowerShell Core med utökad behörighet.

    ```bash
    sudo pwsh
    ```

1. Kör följande kommando i Azure PowerShell-kommandotolken för att installera Azure PowerShell.

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. Om du blir tillfrågad om du litar på moduler från **PSGallery** svarar du **Ja** eller **Ja till alla**.

## <a name="summary"></a>Sammanfattning
Du har konfigurerat dina lokala datorer för att administrera Azure-resurser med Azure PowerShell. Du kan nu använda Azure PowerShell lokalt för att ange kommandon eller köra skript. Azure PowerShell vidarebefordrar dina kommandon till Azure-datacenter där de körs i din Azure-prenumeration.