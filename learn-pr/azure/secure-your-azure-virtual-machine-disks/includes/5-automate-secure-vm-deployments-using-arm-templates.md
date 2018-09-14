Tänk dig att ditt företag distribuerar flera servrar som en del av övergången till molnet. Virtuella datordiskar måste krypteras under distributionen så att de inte är sårbara under något skede. Du vill automatisera processen och behöver ändra Azure Resource Manager-mallar för att automatiskt aktivera kryptering.

Här ska vi titta på hur du använder en Azure Resource Manager-mall för att automatiskt aktivera kryptering för nya virtuella Windows-datorer.

## <a name="what-are-azure-resource-manager-templates"></a>Vad är Azure Resource Manager-mallar?

Dessa mallar är JSON-filer som används för att definiera en resurs för att distribuera till Azure, till exempel en virtuell dator. Du kan skriva dem från början, och för vissa Azure-resurser som virtuella datorer kan du generera dem genom att använda Azure-portalen. Du måste fylla i all nödvändig information för en manuell distribution av virtuella datorer, men i stället för att distribuera den virtuella datorn till Azure sparar du mallen.

Det finns exempelmallar i GitHub.

## <a name="using-github-templates"></a>Använda GitHub-mallar

GitHub har en mall för att aktivera kryptering med namnet på **Aktivera kryptering på en aktiv Windows VM ARM**. Du hittar den i lagringsplatsen för [Azure-snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates). Sidan Viktigt-filen för mallen innehåller en **distribuera till Azure** knapp som öppnar mallen i Azure-portalen.

Med hjälp av mallen kan du distribuera en virtuell Windows Server-dator som har kryptering färdigt aktiverat. Innan du använder mallen, måste du se till att alla kryptering krav är uppfyllda. Du måste också den konfigurationsinformation som tillhandahålls av skriptet förutsättningar, till exempel Azure Active Directory klient-ID och Klienthemlighet för Azure Active Directory.

Du skapar en ny virtuell dator genom att ange den nödvändiga informationen i mallen. Du startar sedan distributionen genom att klicka på **Köp** (kostnaden är vanligtvis den normala beräkningsavgiften för Azure).

## <a name="deploy-an-encrypted-vm-by-using-a-template"></a>Distribuera en krypterad virtuell dator med hjälp av en mall

De huvudsakliga stegen för att distribuera en krypterad virtuell dator med hjälp av en mall är:

1. Få tillgång till och kör mallen **Enable encryption on a running Windows VM ARM** (Aktivera kryptering på en Windows VM ARM som körs) från GitHub.

1. Lägg till den nödvändiga informationen på skriptets konfigurationssida.

1. Distribuera en ny virtuell dator med hjälp av skriptet genom att klicka på **Köp**.

1. Verifiera diskens krypteringsstatus genom att använda Azure-portalen.
