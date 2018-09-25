Anta att du arbetar för ett varuhus som håller på att flytta till molnet. För närvarande använder du en hybridmiljö med lokala Windows-servrar, virtuella Azure-datorer (VM) och Azure Active Directory (AD). Företaget har utvecklat sin egen infrastruktur för business-to-business (B2B), med stöd för säker orderhantering gentemot leverantörer. Vissa av dina leverantörer använder Linux-servrar och du kör flera Linux-servrar i Azure för att stödja dessa leverantörer.

Dina säkerhetsprinciper kräver att data måste vara krypterad med era egna krypteringsnycklar, och att företaget ansvarar för hanteringen av dessa nycklar.

Administrationen använder redan PowerShell för hantering av lokala servrar. Du ska distribuera och testa många virtuella Azure-datorer och planerar att använda Azure Resource Manager-mallar för att automatisera processen.

Här tittar vi på typer av skydd som är tillgängliga för virtuella datordiskar, så att du kan avgöra om Azure Disk Encryption (ADE) är det bästa valet för ett visst scenario. Vi aktiverar därefter ADE på befintliga virtuella datordiskar och använder mallar för att aktivera ADE på nya VM-distributioner.


## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Fastställa vilken krypteringsmetod som är bäst för din virtuella dator
- Kryptera befintliga virtuella datordiskar via Azure-portalen
- Kryptera befintliga virtuella datordiskar med hjälp av PowerShell
- Ändra Azure Resource Manager-mallar för att automatisera diskkryptering på nya virtuella datorer
