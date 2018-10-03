Att ge kunderna snabb tillgång till produkterna i din klädbutik på webben är avgörande för dina kunder och för att ditt företag ska bli framgångsrikt. Genom att minska avståndet som data måste färdas till dina användare kan du leverera mer innehåll snabbare. Om dina data lagras i Azure Cosmos DB handlar replikering av webbplatsens data till flera regioner över hela världen bara om att peka och klicka.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

I den här kursdelen lär du dig fördelarna med global distribution och en databastjänst med flera original inbyggt, och sedan att replikera ditt konto i ytterligare tre regioner.

## <a name="global-distribution-basics"></a>Grunderna för global distribution

Med global distribution kan du replikera data från en region i flera Azure-regioner. Du kan lägga till eller ta bort regioner där databasen replikeras när som helst och Azure Cosmos DB ser till att när du lägger till en ytterligare region är dina data tillgängliga för åtgärder inom 30 minuter, förutsatt att dina data är högst 100 TB.

Det finns två vanliga scenarier för att replikera data i två eller flera regioner:

1. Leverera dataåtkomst med låg latens för slutanvändare oavsett var i världen de befinner sig
2. Lägga till regional återhämtning till affärskontinuitet och haveriberedskap (BCDR)

Om du vill leverera åtkomst med låg latens till kunder bör du replikerar data till regioner som är närmast den plats där dina användare finns. För ditt företag med klädbutik online har du kunder i Los Angeles, New York och Tokyo. Ta en titt på sidan med [Azure-regioner](https://azure.microsoft.com/global-infrastructure/regions/) och fastställ de närmaste regionerna för dessa kunder, eftersom de är de platser som du ska replikera användare till.

Om du vill tillhandahålla en BCDR-lösning bör du lägga till regioner utifrån de regionpar som beskrivs i artikeln [Affärskontinuitet och haveriberedskap (BCDR): parade Azure-regioner](https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/).

När en databas replikeras så replikeras även dataflöde och lagring. Så om den ursprungliga databasen hade 10 GB lagringsutrymme och ett dataflöde på 1 000 RU/s, och om du har replikerat det till ytterligare tre regioner, skulle varje region ha 10 GB data och 1 000 RU/s dataflöde. Eftersom lagringsutrymmet och dataflödet replikeras i varje region är kostnaden för replikering till en region densamma som för den ursprungliga regionen, så att replikera till tre ytterligare regioner skulle kosta ungefär fyra gånger så mycket som den ursprungliga ej replikerade originaldatabasen.

## <a name="creating-an-azure-cosmos-db-account-in-the-portal"></a>Skapa ett Azure Cosmos DB-konto på portalen

1. Logga in på [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

    > [!IMPORTANT]
    > Logga in på Azure Portal och sandbox-miljön med samma konto.
    >
    > Logga in på Azure Portal med länken ovan för att se till att du är ansluten till sandbox-miljön som ger åtkomst till en Concierge-prenumeration.

1. Klicka på **Skapa en resurs** > **Databaser** > **Azure Cosmos DB**.

   ![Fönstret Databaser på Azure Portal](../media/2-global-distribution/2-create-nosql-db-databases-json-tutorial.png)

1. På sidan **Skapa Azure Cosmos DB-konto** anger du inställningarna för det nya Azure Cosmos DB-kontot, inklusive platsen.

    <!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    Inställning|Värde|Beskrivning
    ---|---|---
    Prenumeration|*Concierge-prenumeration*|Välj din Concierge-prenumeration. Om du inte ser Concierge-prenumerationen har du flera klienter aktiverade på din prenumeration och du måste ändra klienter. Logga in igen med följande portallänk: [Azure-portalen för sandbox-miljö](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) för att göra detta.
    Resursgrupp|Använd befintlig<br><br><rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>|Välj **Använd befintlig** och ange därefter <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>.
    Kontonamn|*Ange ett unikt namn*|Ange ett unikt namn som identifierar Azure Cosmos DB-kontot. Eftersom *documents.azure.com* läggs till det ID du anger för att skapa din URI ska du använda ett unikt men identifierbart ID.<br><br>Ditt ID får bara innehålla gemener, siffror och bindestreck (-) och det måste innehålla mellan 3 och 31 tecken.
    API|SQL|API:n avgör vilken typ av konto som skapas. Azure Cosmos DB innehåller fem API:er för ditt program: SQL (dokumentdatabas), Gremlin (grafdatabas), MongoDB (dokumentdatabas), Azure-tabell och Cassandra, där var och en för närvarande kräver ett separat konto. <br><br>Välj **SQL** eftersom du i den här modulen ska skapa en dokumentdatabas som stöder frågekörning med SDL-syntax och som kan nås med SQL-API:et.|
    Plats|*Välj den region som är närmast dig*|Välj den region som är närmast dig i listan över regioner ovan.
    Geo-redundans| Inaktivera | Den här inställningen skapar en replikerad version av databasen i en andra (parad) region. Låt det här vara inaktiverat just nu eftersom du ska replikera databasen senare.
    Skrivåtgärder för flera regioner | Aktivera | Den här inställningen låter dig skriva till flera regioner samtidigt. Inställningen kan bara konfigureras när kontot skapas.

1. Klicka på **Granska + Skapa**.

    ![Den nya kontosidan för Azure Cosmos DB](../media/2-global-distribution/2-azure-cosmos-db-create-new-account.png)

1. När inställningarna har verifierats klickar du på **Skapa** för att skapa kontot.

1. Det tar några minuter att skapa kontot. Vänta tills portalen visar meddelandet om att distributionen är klar och klicka på meddelandet.

    ![Avisering](../media/2-global-distribution/2-azure-cosmos-db-notification.png)

1. Klicka på **Gå till resurs** i meddelandefönstret.

    ![Gå till resurs](../media/2-global-distribution/2-azure-cosmos-db-go-to-resource.png)

    Portalen visar sidan **Grattis! Azure Cosmos DB-kontot har skapats**.

    ![Meddelandefönstret i Azure Portal](../media/2-global-distribution/2-azure-cosmos-db-account-created.png)

## <a name="replicate-data-in-multiple-regions"></a>Replikera data i flera regioner

Nu ska vi replikera din databas närmast dina globala användare i Los Angeles, New York och Tokyo.

1. Klicka på **Replikera data globalt** på menyn på kontosidan.
1. På sidan **Replikera data globalt** väljer du regionerna USA, västra 2, USA, östra och Japan, östra och klickar sedan på **Spara**.

    Om du inte ser kartan i Azure Portal kan du minimera menyerna till vänster på skärmen för att visa den.

    Sidan visar meddelandet **Uppdaterar** när data skrivs till de nya regionerna. Data i de nya regionerna blir tillgängliga inom 30 minuter.

    ![Klicka på regionerna på kartan för att lägga till dem](../media/2-global-distribution/2-global-replication.gif)

## <a name="summary"></a>Sammanfattning

I den här kursdelen har du replikerat din databas till olika regioner i världen där du har flest användare, vilket ger dem åtkomst med låg latens till data på din webbplats.
