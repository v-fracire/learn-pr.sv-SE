Med Azure Portal kan du hantera och skala PostgreSQL-databasservrar. Du bestämmer dig för att skapa en Azure Database for PostgreSQL-server för att lagra en löpares resultatdata. Baserat på tidigare fångade datavolymer vet du att kraven på serverlagringen ska vara inställt på 10 GB. För att stödja dina bearbetningskrav måste du ha stöd för Compute Gen 5 med 1 virtuell kärna. Du vet också att du vanligtvis lagrar säkerhetskopior i 25 dagar.

> [!TIP]
> Alla övningar i Microsoft Learn är kostnadsfria. När du börjar utforska på egen hand behöver du dock en Azure-prenumeration. Om du inte har någon än kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Det tar bara några minuter.

Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true). Till vänster ser du menyn som du kan använda för att skapa och hantera Azure-resurser. Instrumentpanelen tar upp resten av skärmen.

## <a name="create-an-azure-database-for-postgresql-server"></a>Skapa en Azure Database för PostgreSQL-server

När du har loggat in ser du standardinstrumentpanelen. Det finns några tillgängliga alternativ för att skapa en Azure Database for PostgreSQL-server. Från instrumentpanelen kan du antingen:

- Välja alternativet **Alla tjänster** och söka efter alternativet **Azure Database for PostgreSQL server** (Azure Database for PostgreSQL-server). På den här skärmen visas alla konfigurerade servrar som redan finns i ditt konto. Här väljer du **Lägg till**, som gör att du kommer till bladet för att skapa en ny server.

eller

- Välj alternativet **Create a resource** (Skapa en resurs) som visar dig alternativen för Azure Marketplace-resurser. Här väljer du databasalternativet och sedan **Azure Database for PostgreSQL**.

### <a name="configure-the-server"></a>Konfigurera servern

Nu ser du bladet för att skapa PostgreSQL-servern, som liknar följande bild.

![Blad med användargränssnitt för skapande av PostgreSQL-server i Azure-portalen](../media-draft/4-create-blade.png)

> [!NOTE]
> Du måste komma ihåg eller anteckna viss information när du skapar PostgreSQL-servern. Till exempel användarnamnet och lösenordet för att få åtkomst till servern. Du använder den här informationen för att ansluta till servern senare.

1. Välj ett unikt namn för servern. Tänk på att namnet bara får innehålla gemener och får innehålla siffror och bindestreck.

1. Välj en prenumeration och kontrollera att fältet är inställt på den prenumeration du vill använda.

1. Nu kan du skapa eller återanvända en befintlig resurs. Om du vill skapa en ny resurs trycker du på alternativknappen **Skapa ny** och anger ett namn på den nya resursgruppen. Du använder den här gruppen för resten av den här modulen. Ge resursen ett beskrivande namn så att det är lätt att ta bort resursen senare.

1. Välj källa för den nya servern. För den här labbuppgiften lämnar du alternativet _tomt_. Kom ihåg att du kan ändra alternativet för att _säkerhetskopiera_ om du vill återställa befintlig serversäkerhetskopiering.

1. Välj ett inloggningsnamn som du ska använda som administratörsinloggning för den nya servern. Kom ihåg att inloggningsnamnet för administratören inte får vara azure_superuser, azure_pg_admin, admin, administrator, root, guest, eller public. Det får inte börja med pg_. Kom ihåg att anteckna namnet för framtida användning.

1. Skapa ett lösenord som du ska använda med administratörsinloggningsnamnet ovan. Tänk på att lösenordet måste innehålla tecken från tre av följande kategorier: engelska versala bokstäver, engelska gemena bokstäver, siffror (0 till och med 9) och icke-alfanumeriska tecken (!, $, #, % osv.). Kom ihåg att anteckna lösenordet för framtida användning.

1. Bekräfta lösenordet genom att skriva det igen.

1. Ange en plats för servern. Välj platsen som är närmast dig.

1. Nu ska du välja serverns version. Välj den senaste PostgreSQL-versionen.

1. I det näst sista steget väljer du alternativet **Prisnivå**.

    Kom ihåg att du måste konfigurera servern med specifika alternativ för lagring och beräkning.

    - 10 GB diskutrymme
    - Stöd för Compute Gen 5
    - Kvarhållningsperiod på 25 dagar

    Klicka på **Prisnivå** för att få åtkomst till bladet Prisnivå och gör följande ändringar: ![knappen Prisnivå](../media-draft/4-azure-db-pricing-tier-button.png)

    - Välj fliken **Basic**.
    - Välj alternativet för **beräkningsgeneration 5**.
    - Välj en virtuell kärna från skjutreglaget **Virtuell kärna**. Lägg märke till hur ändringarna i skjutreglaget påverkar **Prissammanfattning**.
    - Välj 10 GB från skjutreglaget **Storage**. Om du har problem med att dra till exakt 10 GB kan du använda tangentbordets vänstra och högra markörknappar för att få ett exakt värde.
    - Välj 25 dagar från skjutreglaget **Kvarhållningsperiod för säkerhetskopiering**.

    ![Prisnivå](../media-draft/4-azure-db-pricing-tier.png)
    - Klicka på **OK** när du är nöjd med ditt val för att genomföra dina val och stänga alternativen för prisnivå.

1. Allt som återstår är att granska värdena du angav och klicka på **Skapa**. Det kan ta flera minuter att skapa. Du kan välja meddelandeikonen (en bjällra) högst upp på skärmen i Azure-portalen för att övervaka förloppet.

Nu har du en tillgänglig PostgreSQL-server. I nästa del får du se hur man skapar samma server med Azure CLI.
