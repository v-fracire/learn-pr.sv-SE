Ditt företag har valt Azure Cosmos DB för att uppfylla kraven från en växande produkt- och kundbas. Du har fått i uppgift att skapa databasen.

Det första steget är att skapa ett Azure Cosmos DB-konto.

## <a name="what-is-an-azure-cosmos-db-account"></a>Vad är ett Azure Cosmos DB-konto?

Ett Azure Cosmos DB-konto är en Azure-resurs som fungerar som en organisationsentitet för dina databaser. Det kopplar din användning till din Azure-prenumeration för faktureringsändamål.

Varje Azure Cosmos DB-konto är associerat med någon av flera datamodeller som Azure Cosmos DB har stöd för, och du kan skapa så många konton du behöver. 

SQL-API:n är den rekommenderade datamodellen om du skapar ett nytt program. Om du arbetar med diagram eller tabeller, eller om du migrerar MongoDB- eller Cassandra-data till Azure, skapar du ytterligare konton och väljer relevanta datamodeller.

När du skapar ett konto väljer du ett ID som passar dig. Det är så du identifierar ditt konto. Skapa också kontot i den Azure-region som är närmast dina användare för att minimera fördröjningen mellan datacentret och dina användare.

Om du vill kan du konfigurera virtuella nätverk och georedundans när kontot skapas, men det kan också göras senare. I den här modulen aktiverar vi inte dessa inställningar.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="creating-an-azure-cosmos-db-account-in-the-portal"></a>Skapa ett Azure Cosmos DB-konto på portalen

1. Logga in på [Azure-portalen för begränsat läge](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du har aktiverat sandbox-miljön med.

    > [!IMPORTANT]
    > Logga in på Azure-portalen med länken ovan för att se till att du är ansluten till sandbox-miljön som ger åtkomst till en Concierge-prenumeration.

1. Klicka på **Skapa en resurs** > **Databaser** > **Azure Cosmos DB**.
   
   ![Fönstret Databaser på Azure Portal](../media/2-create-nosql-db-databases-json-tutorial.png)

1. På sidan **Skapa Azure Cosmos DB-konto** anger du inställningarna för det nya Azure Cosmos DB-kontot, inklusive platsen.

    [!INCLUDE[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]
 
    Inställning|Värde|Beskrivning
    ---|---|---
    Prenumeration|*Concierge-prenumeration*|Välj Concierge-prenumerationen. Om du inte ser Concierge-prenumerationen har du flera klienter aktiverade på din prenumeration och du måste ändra klienter. Logga in igen med följande portallänk: [Azure-portalen för begränsat läge](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) för att göra detta.
    Resursgrupp|Använd befintlig<br><br>**<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**.|Här skapar du sedan en ny resursgrupp eller väljer en befintlig på din prenumeration. 
    Kontonamn|*Ange ett unikt namn*|Ange ett unikt namn som identifierar Azure Cosmos DB-kontot. Eftersom *documents.azure.com* läggs till det ID du anger för att skapa din URI ska du använda ett unikt men identifierbart ID.<br><br>Ditt ID får bara innehålla gemener, siffror och bindestreck och måste innehålla mellan 3 och 50 tecken.
    API|SQL|API:n avgör vilken typ av konto som skapas. Azure Cosmos DB innehåller fem API:er för ditt program: SQL (dokumentdatabas), Gremlin (grafdatabas), MongoDB (dokumentdatabas), Azure-tabell och Cassandra, där var och en för närvarande kräver ett separat konto. <br><br>Välj **SQL** eftersom du i den här modulen ska skapa en dokumentdatabas som stöder frågekörning med SDL-syntax och som kan nås med SQL-API:et.|
    Plats|*Välj den region som är närmast från ovanstående lista*|Välj den plats där databasen ska finnas.
    Geo-replikering| Inaktivera | Den här inställningen skapar en replikerad version av databasen i en andra (parad) region. Lämna detta som inaktiverat tills vidare, eftersom databasen kan replikeras senare.
    Flera original | Inaktivera | Den här inställningen låter dig skriva till flera regioner samtidigt. Inställningen kan bara konfigureras när kontot skapas. Lämna detta som inaktiverat för den här enheten för tillfället.
    Virtual Network|Lämna tomt|Lämna virtuella nätverk tomt för tillfället. Detta kan konfigureras senare.

1. Klicka på **Granska + Skapa**.

    ![Den nya kontosidan för Azure Cosmos DB](../media/2-azure-cosmos-db-create-new-account.png)

1. När inställningarna har verifieras klickar du på **Skapa** för att skapa kontot. 

1. Det tar några minuter att skapa kontot. Vänta tills portalen visar meddelandet om att distributionen är klar och klicka på meddelandet. 

    ![Avisering](../media/2-azure-cosmos-db-notification.png)

1. Klicka på **Gå till resurs** i meddelandefönstret.

    ![Gå till resurs](../media/2-azure-cosmos-db-go-to-resource.png)

    Portalen visar sidan **Grattis! Azure Cosmos DB-kontot har skapats**.

    ![Meddelandefönstret i Azure-portalen](../media/2-azure-cosmos-db-account-created.png)

## <a name="summary"></a>Sammanfattning

Du har skapat ett Azure Cosmos DB-konto, som är det första steget när du ska skapa en Azure Cosmos DB-databas. Du har valt lämpliga inställningar för dina datatyper och angett platsen för kontot för att minimera fördröjningen för dina användare.
