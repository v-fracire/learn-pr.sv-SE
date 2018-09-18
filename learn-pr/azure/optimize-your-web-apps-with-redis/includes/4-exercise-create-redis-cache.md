Nu skapar vi en Azure Redis Cache för att lagra och returnera värden som används ofta.

<!-- TODO: do we need to activate the sandbox here? -->

## <a name="create-a-redis-cache-in-azure"></a>Skapa en Redis-cache på Azure

1. Logga in på [Azure-portalen](https://portal.azure.com?azure-portal=true).

1. Klicka på **Skapa en resurs**, klicka på **Databaser** och klicka på **Redis Cache**.

    Följande skärmbild visar Redis Cache-platsen i de olika alternativen för databasresurser på Azure-portalen.

    ![Skärmbild som visar Azure-portalens databasalternativ, med alternativen Skapa en resurs, Databas och Redis Cache markerade.](../media/4-create-a-cache-1.png)

### <a name="identify-the-location-for-the-cache"></a>Identifiera platsen för cachen

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="configure-your-cache"></a>Konfigurera din cache

Använd följande inställningar för cachen.

1. **DNS-namn:** Skapa ett globalt unikt namn, till exempel **ContosoSportsApp1028**.

1. **Prenumeration**: välj Azure Sandbox-prenumerationen.

1. **Resursgrupp**: välj <rgn>[Namn på Sandbox-resursgrupp]</rgn> för resursgruppen.

1. **Plats:** normalt väljer du en plats nära dina kunder – i det här fallet östkusten. Azure Sandbox tillåter dock bara att specifika regioner väljs för resurser enligt det som anges ovan. Välj någon av dessa platser.

1. **Prisnivå:** Välj **Basic C0**. Det här är den lägsta nivån som du kan använda. Produktionsappar vill förmodligen lagra mer data och använda vissa Premium-funktioner, till exempel klustring, vilket kräver val av en högre nivå.

1. Klicka på **Skapa**.

    Följande skärmbild visar en typisk konfiguration som används för att skapa en ny Redis Cache-resurs. Observera att din kommer att se något annorlunda ut på grund av Azure Sandbox.

    ![Skärmbild som visar bladet för Azure-portalen när du skapar en ny Redis Cache-resurs, med exempel på DNS-namn, prenumeration, ny resursgrupp, plats och prisnivå.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> Du måste vänta tills cachen distribueras innan du fortsätter. Den här processen kan ta lite tid.

## <a name="retrieve-the-access-keys-and-host-name"></a>Hämta åtkomstnycklarna och värdnamnet

1. Navigera till din nya cacheinstans i Azure-portalen och välj **Inställningar** > **Åtkomstnycklar**. 

1. Kopiera den **Primära anslutningssträngen (StackExchange.Redis)** till en säker plats. Du behöver den i nästa övning.

    Den här nyckeln innehåller din primära nyckel och värdnamnet i en fullständig anslutningssträng, för användning i programinställningarna för **StackExchange.Redis**-paketet som vi kommer att använda.

Nu går vi igenom några av de kommandon som vi kan använda för att fråga ut cachen.