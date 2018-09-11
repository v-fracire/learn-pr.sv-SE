---
zone_pivot_groups: platform
ms.openlocfilehash: 5e0a236b9cf0c3c0b23beb1324f35a34dade2e92
ms.sourcegitcommit: 926510a198d738c5726081f6d7994fe9b6fc6edb
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43179836"
---
Nu ska vi installera Azure CLI på din lokala dator och därefter köra ett enkelt kommando för att verifiera installationen. Vilken metod du använder för att installera Azure CLI beror på datorns operativsystem. Välj anvisningarna för ditt operativsystem.

::: zone pivot="linux"

### <a name="linux"></a>Linux
Här ska du installera Azure CLI på **Ubuntu Linux** med (**APT**) och Bash-kommandoraden.

> [!WARNING]
> Kommandona nedan är för Ubuntu version 18.04. Om du använder en annan version av Ubuntu måste du lägga till en annan lagringsplats. Mer information finns i [Installera Azure CLI 2.0 med apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).

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

### <a name="macos"></a>macOS
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

### <a name="windows"></a>Windows
Här installerar du Azure CLI på Windows med hjälp av MSI-installationsprogrammet.

1. Gå till [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), och i webbläsarens dialogruta för säkerhet klickar du på **Kör**.
1. I installationsprogrammet godkänner du licensvillkoren och klickar sedan på **Installera**.
1. I dialogrutan **User Account Control** väljer du **Ja**.

::: zone-end

## <a name="running-the-azure-cli"></a>Köra Azure CLI
Du kör Azure CLI genom att öppna ett bash-gränssnitt (Linux och macOS), eller från kommandotolken eller PowerShell (Windows).

1. Starta Azure CLI och verifiera installationen genom att köra versionskontrollen.

    ```bash
    az --version
    ```

::: zone pivot="windows"

> [!NOTE]
> Det finns några fördelar med att köra Azure CLI från PowerShell jämfört med köra Azure CLI från Kommandotolken för Windows. PowerShell ger ytterligare funktioner än de som är tillgängliga från Kommandotolken. 

::: zone-end

## <a name="summary"></a>Sammanfattning
Du konfigurerar dina lokala datorer för att administrera Azure-resurser med Azure CLI. Du kan nu använda Azure CLI lokalt för att ange kommandon eller köra skript. Azure CLI vidarebefordrar dina kommandon till Azure-datacenter där de körs i din Azure-prenumeration.