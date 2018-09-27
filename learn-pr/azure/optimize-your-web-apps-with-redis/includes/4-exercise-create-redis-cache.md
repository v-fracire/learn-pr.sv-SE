Nu skapar vi en Azure Redis Cache för att lagra och returnera värden som används ofta.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-redis-cache-in-azure"></a>Skapa en Redis-cache på Azure

1. Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. Klicka på **Skapa en resurs**, klicka på **Databaser** och klicka på **Redis Cache**.

    Följande skärmbild visar Redis Cache-platsen i de olika alternativen för databasresurser på Azure-portalen.

    ![Skärmbild som visar Azure-portalens databasalternativ, med Skapa en resurs, Databas och Redis Cache-alternativet markerade.](../media/4-create-a-cache-1.png)

### <a name="configure-your-cache"></a>Konfigurera din cache

Använd följande inställningar för cachen.

1. **DNS-namn:** skapar ett globalt unikt namn som **ContosoSportsApp [nnn]**, där `[nnn]` ersätts med slumpmässiga siffror.

1. **Prenumeration:** välj Concierge-prenumerationen.

1. **Resursgrupp**: välj <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn> för resursgruppen.

1. **Plats:** normalt väljer du en plats nära dina kunder – i det här fallet östkusten.

    [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]

5. **Prisnivå:** välj **Basic C0**. Det här är den lägsta nivån som du kan använda. Produktionsappar vill förmodligen lagra mer data och använda vissa Premium-funktioner, till exempel klustring, vilket kräver val av en högre nivå.

1. Klicka på **Skapa**. Azure skapar och distribuerar Redis Cache-instansen åt dig.

    > [!IMPORTANT]
    > Redis Cache-resursen skapas och visas vanligtvis mycket snabbt i Azure Portal, men själva cacheminnet är inte tillgänglig under några minuter. Nästa steg visar hur du kontrollerar ditt cacheminnes status.

## <a name="verify-your-data"></a>Verifiera dina data

Du kan använda **konsol**-funktion i Azure-portalen för att utfärda kommandon till din Redis cache-instans när den har skapats.

1. Hitta din Redis cache via popupmenyn **Meddelanden** när den är klar med distributionen, eller genom att välja **Alla resurser** i det vänstra sidofältet och använda filterrutan till vänster för att välja Redis Cache-instanser. Du kan alternativt använda sökrutan högst upp och skriva namnet på cacheminnet.

1. Välj din Redis Cache-instans.

1. Kontrollera statusfältets värde. Cacheminnet är inte klart förrän statusen är ”Körs”. Du får kanske vänta några minuter innan du kan fortsätta.

1. När cacheminnet väl körs klickar du på `>_ Console`-knappen i verktygsfältet på bladet **Översikt**-bladet för din Redis Cache. Redis-konsolen öppnas, där du kan ange Redis-kommandon på låg nivå. Prova något av följande:

    ```console
    ping
    ```

    ```output
    PONG
    ```

    ```console
    set test one
    ```

    ```output
    OK
    ```

    ```console
    get test
    ```

    ```output
    "one"
    ```

Gå tillbaka till den **Översikt**-panelen genom navigeringsfältet längst upp eller använda rullningslisten för att dra tillbaka vyn till vänster.

## <a name="retrieve-the-access-keys-and-host-name"></a>Hämta åtkomstnycklarna och värdnamnet

1. Välj **Inställningar** > **Åtkomstnycklar**.

1. Kopiera den **Primära anslutningssträngen (StackExchange.Redis)** till en säker plats. Du behöver den i nästa övning.

    Den här nyckeln innehåller din primära nyckel och värdnamnet i en fullständig anslutningssträng, för användning i programinställningarna för **StackExchange.Redis**-paketet som vi kommer att använda.

Nu går vi igenom några av de kommandon som vi kan använda för att fråga ut cachen.