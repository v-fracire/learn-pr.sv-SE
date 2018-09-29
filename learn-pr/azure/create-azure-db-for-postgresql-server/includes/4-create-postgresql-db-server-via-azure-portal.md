Med Azure Portal kan du hantera och skala PostgreSQL-databasservrar. Du bestämmer dig för att skapa en Azure Database for PostgreSQL-server där du ska lagra löparnas resultatdata. Baserat på tidigare fångade datavolymer vet du att kraven på serverlagringen ska vara inställt på 10 GB. För att stödja dina bearbetningskrav måste du ha stöd för Compute Gen 5 med 1 virtuell kärna. Du vet också att du vanligtvis lagrar säkerhetskopior i 25 dagar.

[!include[](../../../includes/azure-sandbox-activate.md)]

Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du aktiverade sandbox-miljön.

Till vänster ser du menyn för att skapa och hantera Azure-resurser. Instrumentpanelen tar upp resten av skärmen.

## <a name="create-an-azure-database-for-postgresql-server"></a>Skapa en Azure Database för PostgreSQL-server

När du har loggat in ser du standardinstrumentpanelen. Det finns några tillgängliga alternativ för att skapa en Azure Database for PostgreSQL-server. Från instrumentpanelen kan du antingen:

- Välja alternativet **Alla tjänster** och sedan söka efter alternativet **Azure Database for PostgreSQL-server**. På den här skärmen visas alla konfigurerade servrar som redan finns i ditt konto. Härifrån väljer du **Lägg till**, som tar dig till bladet för att skapa en ny server.

**eller**

- Välja alternativet **Create a resource** (Skapa en resurs) som visar dig alternativen för Azure Marketplace-resurser. Här väljer du alternativet **Databaser** och sedan **Azure Database for PostgreSQL**.

### <a name="configure-the-server"></a>Konfigurera servern

Nu ser du bladet för att skapa PostgreSQL-servern, som liknar följande bild.

![Skärmbild av Azure Portal som visar bladet Skapa för en ny PostgreSQL-databas](../media/4-create-blade.png)

> [!NOTE]
> Du måste komma ihåg viss information när du skapar PostgreSQL-servern. Till exempel användarnamnet och lösenordet för att få åtkomst till servern. Du använder den här informationen för att ansluta till servern senare.

1. Välj ett unikt namn för servern. Tänk på att namnet bara får innehålla gemener och att det får innehålla siffror och bindestreck.

1. Välj en prenumeration. Kontrollera att fältet är inställt på den prenumeration som du vill använda.

1. Nu kan du skapa eller återanvända en befintlig resursgrupp. Välj **Använd befintlig** och därefter “<rgn>[Resursgruppnamn för sandbox-miljö]</rgn>” i listrutan. Du använder den här gruppen i resten av den här modulen.

1. Välj källa för den nya servern. För den här labbuppgiften lämnar du alternativet _tomt_. Kom ihåg att du kan ändra alternativet till _Säkerhetskopiera_ om du vill återställa en befintlig serversäkerhetskopia.

1. Välj ett inloggningsnamn som du ska använda som administratörsinloggning för den nya servern. Kom ihåg att inloggningsnamnet för administratören inte får vara azure_superuser, azure_pg_admin, admin, administrator, root, guest, eller public. Det får inte börja med pg_. Kom ihåg att anteckna namnet för framtida användning.

1. Välj ett lösenord som ska användas med administratörsinloggningsnamnet ovan. Kom ihåg,= att vårt lösenord måste innehålla tecken från tre av följande kategorier:
   - Engelska versala bokstäver
   - Engelska gemena bokstäver
   - Siffror (0 till 9)
   - Icke-alfanumeriska tecken (!, $, #, % osv.)

1. Bekräfta lösenordet genom att skriva det igen.

1. Ange en plats för servern. Välj platsen som är närmast dig i följande lista.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]


1. Nu ska du välja serverns version. Välj den senaste PostgreSQL-versionen.

1. I det näst sista steget väljer du alternativet **Prisnivå**.

    Kom ihåg att du måste konfigurera servern med specifika alternativ för lagring och beräkning:

    - 10 GB diskutrymme
    - Stöd för Compute Gen 5
    - Kvarhållningsperiod på 25 dagar

    Klicka på **Prisnivå** för att få åtkomst till bladet Prisnivå och gör följande ändringar:

    - Välj flikalternativet **Basic**.
    - Välj alternativet för **beräkningsgeneration 5**.
    - Välj en virtuell kärna från skjutreglaget **Virtuell kärna**. Lägg märke till hur ändringarna i skjutreglaget påverkar **Prissammanfattning**.
    - Välj 10 GB från skjutreglaget **Storage**. Om du har problem med att dra till exakt 10 GB kan du använda tangentbordets vänstra och högra piltangenter för att få ett exakt värde.
    - Välj 25 dagar med skjutreglaget **Kvarhållningsperiod för säkerhetskopiering**.
    - Lämna redundansalternativen för säkerhetskopior som **Lokalt redundant**.

    ![Skärmbild av Azure Portal som visar databasens prisnivå för en ny PostgreSQL-databas](../media/4-azure-db-pricing-tier.png)

1. Kontrollera prissammanfattningen på höger sida. Den innehåller en kostnadsuppdelning för servern tillsammans med en beräknad månatlig kostnad.

1. Klicka på **OK** när du är nöjd med ditt val för att tillämpa det och stänga alternativen för prisnivån.

1. Allt som återstår nu är att granska värdena du angav och klicka på **Skapa**. Det kan ta flera minuter att skapa. Du kan välja meddelandeikonen (en bjällra) högst upp på skärmen i Azure-portalen för att övervaka förloppet.

Nu har du en tillgänglig PostgreSQL-server. I nästa del får du se hur man skapar samma server med Azure CLI.
