## <a name="motivation"></a>Motivering
Anta att ditt företag har valt Azure CLI som kommandoradslösning för hantering av Azure-resurser. Dina administratörer och utvecklare föredrar att köra sina kommandon och skript lokalt i stället för via en webbläsare. Teamet använder datorer som kör Linux, macOS och Windows. Du måste få igång Azure CLI på alla deras enheter.

## <a name="what-is-the-azure-cli"></a>Vad är Azure CLI?
Azure CLI är ett kommandoradsprogram som används för att ansluta till Azure och köra administrativa kommandon på Azure-resurser. Om du till exempel vill starta om en virtuell dator (VM) använder du ett kommando liknande följande:

 ```bash
 az vm restart -g MyResourceGroup -n MyVm
 ```

Azure CLI tillhandahåller plattformsoberoende kommandoradsverktyg för hantering av Azure-resurser och kan installeras lokalt på Linux-, Mac- eller Windows-datorer. Azure CLI kan också användas från en webbläsare via Azure Cloud Shell. I båda fallen kan det användas interaktivt eller skriptas. För interaktiv användning startar du först ett gränssnitt som cmd.exe i Windows eller Bash i Linux eller macOS och anger sedan kommandot i gränssnittets kommandotolk. Om du vill automatisera repetitiva uppgifter sätter du samman CLI-kommandona till ett kommandoskript med hjälp av skriptsyntax för det valda gränssnittet och kör skriptet.

## <a name="how-to-install-azure-cli"></a>Installera Azure CLI
Du använder en pakethanterare för att installera PowerShell Core i både Linux och macOS. Den rekommenderade pakethanteraren skiljer sig beroende på operativsystem och distribution:
- Linux: **apt-get** i Ubuntu, **yum** i Red Hat och **zypper** i OpenSUSE
- Mac: **Homebrew**

Azure CLI är tillgängligt i Microsofts databas, så du måste först lägga till databasen till pakethanteraren.

I Windows installerar du Azure CLI genom att hämta och köra en MSI-fil.

## <a name="using-the-azure-cli-in-scripts"></a>Använda Azure CLI i skript
Om du vill använda Azure CLI-kommandona i skript måste du vara medveten om eventuella problem relaterade till ”servergränssnittet” eller miljön som ska användas för att köra skriptet. I ett bash-gränssnitt används exempelvis följande syntax för att ställa in variabler:

 ```bash
 variable="string"
 variable=integer
 ```

Om du använder en PowerShell-miljö för att köra Azure CLI-skript måste du använda den här syntaxen för variabler:

 ```powershell
 $variable="string"
 $variable=integer
 ```

## <a name="summary"></a>Sammanfattning
Azure CLI måste installeras innan det kan användas för att hantera Azure-resurser från en lokal dator. Installationsstegen varierar för Windows, Linux och macOS, men när det har installerats är kommandona samma för alla plattformar. 
