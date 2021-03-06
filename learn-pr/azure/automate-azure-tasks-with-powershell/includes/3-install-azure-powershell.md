Anta att du har valt Azure PowerShell som automatiseringslösning. Dina Administratörer föredrar att köra sina skript lokalt i stället för i Azure Cloud Shell. Teamet använder datorer som kör Linux, macOS och Windows. Du måste få igång Azure PowerShell på alla deras enheter. 

## <a name="what-must-be-installed"></a>Vad måste finnas installerat?
Vi kommer att gå igenom de faktiska installationsinstruktionerna i nästa utbildningsenhet, men nu ska vi titta på de båda komponenter som utgör Azure PowerShell.

- **PowerShell-basprodukten** Den här produkten finns i två varianter: PowerShell för Windows och PowerShell Core för macOS och Linux.
- **Azure PowerShell-modulen** Den här extramodulen måste installeras för att du ska få tillgång till Azure-specifika kommandon i PowerShell.

> [!TIP]
> PowerShell ingår i Windows (men du kan behöva uppdatera det). Däremot måste du installera PowerShell Core i Linux och macOS.

När basprodukten är installerad lägger du till Azure PowerShell-modulen i installationen.

## <a name="how-to-install-powershell-core"></a>Så installerar du PowerShell Core
Du använder en pakethanterare till att installera PowerShell Core i både Linux och macOS. Vilken pakethanterare som rekommenderas beror på operativsystemet och distributionen.

> [!NOTE]
> PowerShell Core är tillgängligt från Microsofts datalager, så du måste först lägga till det här datalagret i pakethanteraren.

### <a name="linux"></a>Linux
I Linux använder du olika pakethanterare beroende på vilken Linux-distribution du väljer.

| Distribution  | Pakethanterare |
|------------------|-----------------|
| Ubuntu, Debian   | `apt-get`       |
| Red Hat, CentOS  | `yum`           |
| OpenSUSE         | `zypper`        |
| Fedora           | `dnf`           |

### <a name="mac"></a>Mac
I macOS använder du `Homebrew` för att installera PowerShell Core.

I nästa avsnitt går vi igenom installationsstegen för några vanliga plattformar i detalj.