Nu ska vi installera Azure CLI på din lokala dator och därefter köra ett enkelt kommando för att verifiera installationen. Vilken metod du använder för att installera Azure CLI beror på datorns operativsystem. Välj anvisningarna för ditt operativsystem.

> [!NOTE]
> I den här övningen får du hjälp att installera verktyget Azure CLI lokalt. Resten av modulen använder Azure Cloud Shell så att du kan utnyttja den kostnadsfria prenumerationssupporten i Microsoft Learn. Du kan se den här övningen som en valfri aktivitet och bara läsa anvisningarna om du vill.

::: zone pivot="linux"

## <a name="linux"></a>Linux

Här ska du installera Azure CLI på **Ubuntu Linux** med (**APT**) och Bash-kommandoraden.

> [!TIP]
> Kommandona nedan är för Ubuntu version 18.04. Andra versioner och distributioner av Linux har andra anvisningar. Läs i den [officiella dokumentationen](https://docs.microsoft.com/cli/azure/install-azure-cli) om du använder en annan version av Linux.

1. Ändra din källista så att Microsoft-lagringsplatsen registreras, och så att pakethanteraren kan hitta Azure CLI-paketet.

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

1. Importera krypteringsnyckeln för Microsoft Ubuntu-lagringsplatsen. Då kan pakethanteraren verifiera att Azure CLI-paketet som du installerar kommer från Microsoft.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. Installera Azure CLI.

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

::: zone-end

::: zone pivot="macos"

## <a name="macos"></a>macOS

Här installerar du Azure CLI på macOS med Homebrew-pakethanteraren.

> [!IMPORTANT]
> Om kommandot **brew** är otillgängligt kanske du måste installera Homebrew-pakethanteraren. Mer information finns på [Homebrews webbplats](https://brew.sh/).

1. Uppdatera brew-lagringsplatsen för att se till att du får det senaste Azure CLI-paketet.

    ```bash
    brew update
    ```

1. Installera Azure CLI.

    ```bash
    brew install azure-cli
    ```

::: zone-end

::: zone pivot="windows"

## <a name="windows"></a>Windows

Här installerar du Azure CLI på Windows med hjälp av MSI-installationsprogrammet.

1. Gå till [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), och i webbläsarens dialogruta för säkerhet klickar du på **Kör**.
1. I installationsprogrammet godkänner du licensvillkoren och klickar sedan på **Installera**.
1. I dialogrutan **User Account Control** väljer du **Ja**.

::: zone-end

## <a name="running-the-azure-cli"></a>Köra Azure CLI

Du kör Azure CLI genom att öppna ett bash-gränssnitt (Linux och macOS), eller från kommandotolken eller PowerShell (Windows).

1. Starta Azure CLI och verifiera installationen genom att köra versionskontrollen.

    ```azurecli
    az --version
    ```

::: zone pivot="windows"

> [!TIP]
> Det finns några fördelar med att köra Azure CLI från PowerShell jämfört med köra Azure CLI från Kommandotolken för Windows. PowerShell ger ytterligare funktioner än de som är tillgängliga från Kommandotolken. 

::: zone-end

## <a name="summary"></a>Sammanfattning

Du konfigurerar dina lokala datorer för att administrera Azure-resurser med Azure CLI. Du kan nu använda Azure CLI lokalt för att ange kommandon eller köra skript. Azure CLI vidarebefordrar dina kommandon till Azure-datacenter där de körs i din Azure-prenumeration.