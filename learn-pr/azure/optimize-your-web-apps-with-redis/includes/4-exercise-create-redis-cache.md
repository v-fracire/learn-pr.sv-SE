Nu ska vi skapa en Azure Redis Cache-instans för att lagra och returnera vanliga värden.

<!-- TODO: do we need to activate the sandbox here? -->

## <a name="create-a-redis-cache-in-azure"></a>Skapa en Redis-cache i Azure

1. Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true).

1. Klicka på **skapa en resurs**, klickar du på **databaser**, och klicka på **Redis Cache**.

    Följande skärmbild visar Redis Cache-platsen i de olika alternativen för databasresurser på Azure-portalen.

    ![Skärmbild som visar Azure-portalens databasalternativ, med Skapa en resurs, Databas och Redis Cache-alternativet markerade.](../media/4-create-a-cache-1.png)

### <a name="identify-the-location-for-the-cache"></a>Identifiera plats för cache

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="configure-your-cache"></a>Konfigurera din cache

Använda följande inställningar på cacheminnet.

1. **DNS-namn:** Skapa ett globalt unikt namn, till exempel **ContosoSportsApp1028**.

1. **Prenumeration:** väljer du den Azure-Sandbox-prenumerationen.

1. **Resursgrupp:** Välj <rgn>[Sandbox resursgruppens namn]</rgn> för resursgruppen.

1. **Plats:** normalt väljer du en plats nära dina kunder – i det här fallet östkust. Sandbox-miljön för Azure kan dock endast specifika regioner som ska väljas för resurser som anges ovan. Välj någon av dessa platser.

1. **Prisnivå:** Välj **Basic C0**. Det här är den lägsta nivån som du kan använda. Produktionsappar förmodligen vill lagra mer data och använda några av Premium-funktioner, till exempel klustring som kräver en högre nivåval.

1. Klicka på **Skapa**.

    Följande skärmbild visar en typisk konfiguration som används för att skapa en ny Redis Cache-resurs. Observera att din kommer att bli annorlunda ut på grund av sandbox-miljön för Azure.

    ![Skärmbild som visar bladet för Azure-portalen när du skapar en ny Redis Cache-resurs, med exempel på DNS-namn, prenumeration, ny resursgrupp, plats och prisnivå.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> Du måste vänta tills cachen distribueras innan du fortsätter. Den här processen kan ta lite tid.

## <a name="verify-your-data"></a>Verifiera dina data

Du kan använda den **konsolen** funktion i Azure portal för att ge kommandon till din Redis cache-instans när den har skapats.

1. Leta upp din Redis cache genom att välja **alla resurser** i vänster sidofält och använda filterfältet till vänster för att välja Redis Cache-instanser. Du kan också använda sökrutan högst upp och skriver du namnet på cachen.

1. Välj din Redis cache-instans.

1. I den **översikt** bladet för din Redis Cache, Välj **konsolen**. Då öppnas en Redis-konsol, där du kan ange på låg nivå Redis-kommandon.

1. Typ **ping**. Kontrollera att värdet som returneras är **PONG**.

1. Typ **set test en**. Kontrollera att värdet som returneras är **OK**.

1. Typ **hämta**. Kontrollera att värdet som returneras är **”en”**.

1. Gå tillbaka till den **översikt** panelen genom navigeringsfältet längst upp i eller använda rullningslisten till bildläge tillbaka till vänster.

## <a name="retrieve-the-access-keys-and-host-name"></a>Hämta åtkomstnycklarna och värdnamnet

1. Välj **inställningar** > **åtkomstnycklar**. 

1. Kopiera den **primär anslutningssträng (StackExchange.Redis)** till en säker plats du behöver den för nästa övning.

    Den här nyckeln innehåller namnet på din primära nyckel och värden i en fullständig anslutningssträng för användning i programinställningarna för den **StackExchange.Redis** paket som vi ska använda.

Nu ska vi Lär dig mer om några av de kommandon som vi kan använda för att förfråga cachen.