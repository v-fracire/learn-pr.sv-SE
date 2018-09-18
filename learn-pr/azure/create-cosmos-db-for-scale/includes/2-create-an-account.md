Ditt företag har valt Azure Cosmos DB för att uppfylla kraven från en växande produkt- och kundbas. Du har fått i uppgift att skapa databasen.

Det första steget är att skapa ett Azure Cosmos DB-konto.

## <a name="what-is-an-azure-cosmos-db-account"></a>Vad är ett Azure Cosmos DB-konto?

Ett Azure Cosmos DB-konto är en Azure-resurs som fungerar som en organisationsentitet för dina databaser. Det kopplar din användning till din Azure-prenumeration för faktureringsändamål.

Varje Azure Cosmos DB-konto är associerat med någon av flera datamodeller som Azure Cosmos DB har stöd för, och du kan skapa så många konton du behöver. 

SQL-API:n är den rekommenderade datamodellen om du skapar ett nytt program. Om du arbetar med diagram eller tabeller, eller om du migrerar MongoDB- eller Cassandra-data till Azure, skapar du ytterligare konton och väljer relevanta datamodeller.

När du skapar ett konto väljer du ett ID som passar dig. Det är så du identifierar ditt konto. Skapa också kontot i den Azure-region som är närmast dina användare för att minimera fördröjningen mellan datacentret och dina användare.

Om du vill kan du konfigurera virtuella nätverk och georedundans när kontot skapas, men det kan också göras senare. I den här modulen aktiverar vi inte dessa inställningar.

## <a name="creating-an-azure-cosmos-db-account-in-the-portal"></a>Skapa ett Azure Cosmos DB-konto på portalen

1. Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true).

1. Klicka på **Skapa en resurs** > **Databaser** > **Cosmos DB**.
   
   ![Fönstret Databaser på Azure Portal](../media-draft/2-create-nosql-db-databases-json-tutorial.png)

1. På sidan **Nytt konto** anger du inställningarna för det nya Azure Cosmos DB-kontot.
 
    Inställning|Värde|Beskrivning
    ---|---|---
    ID|*Ange ett unikt namn*|Ange ett unikt namn som identifierar Azure Cosmos DB-kontot. Eftersom *documents.azure.com* läggs till det ID du anger för att skapa din URI ska du använda ett unikt men identifierbart ID.<br><br>Ditt ID får bara innehålla gemener, siffror och bindestreck och måste innehålla mellan 3 och 50 tecken.
    API|SQL|API:n avgör vilken typ av konto som skapas. Azure Cosmos DB innehåller fem API:er för ditt program: SQL (dokumentdatabas), Gremlin (grafdatabas), MongoDB (dokumentdatabas), Azure-tabell och Cassandra, där var och en för närvarande kräver ett separat konto. <br><br>Välj **SQL** eftersom du i den här modulen ska skapa en dokumentdatabas som stöder frågekörning med SDL-syntax och som kan nås med SQL-API:et.|
    Prenumeration|*Din prenumeration*|Välj den Azure-prenumeration som du vill använda för det här Azure Cosmos DB-kontot.
    Resursgrupp|Skapa ny<br><br>*Ange sedan samma unika namn som angavs ovan i ID:t*|Välj **Skapa ny** och ange sedan ett nytt resursgruppsnamn för ditt konto. För enkelhetens skull kan samma namn användas som ditt-ID. 
    Plats|*Välj den region som är närmast dina användare*|Välj den geografiska plats som ska vara värd för ditt Azure Cosmos DB-konto. Använd den plats som är närmast dina användare så att de får så snabb åtkomst till data som möjligt.
    Aktivera geo-redundans| Lämna tomt | Den här inställningen skapar en replikerad version av databasen i en andra (parad) region. Lämna detta tomt tills vidare, eftersom databasen kan replikeras senare.
    Virtuella nätverk|Inaktiverad|Lämna virtuella nätverk inaktiverade för tillfället. Du kan aktivera det senare.

1. Klicka på **Skapa**.

    ![Den nya kontosidan för Azure Cosmos DB](../media-draft/2-azure-cosmos-db-create-new-account.png)

1. Det tar några minuter att skapa kontot. Vänta tills portalen visar meddelandet om att distributionen är klar och klicka på meddelandet. 

    ![Avisering](../media-draft/2-azure-cosmos-db-notification.png)

1. Klicka på **Gå till resurs** i meddelandefönstret.

    ![Gå till resurs](../media-draft/2-azure-cosmos-db-go-to-resource.png)

    Portalen visar sidan **Grattis! Azure Cosmos DB-kontot har skapats**.

    ![Meddelandefönstret i Azure-portalen](../media-draft/2-azure-cosmos-db-account-created.png)

## <a name="summary"></a>Sammanfattning

Du har skapat ett Azure Cosmos DB-konto, som är det första steget när du ska skapa en Azure Cosmos DB-databas. Du har valt lämpliga inställningar för dina datatyper och angett platsen för kontot för att minimera fördröjningen för dina användare.
