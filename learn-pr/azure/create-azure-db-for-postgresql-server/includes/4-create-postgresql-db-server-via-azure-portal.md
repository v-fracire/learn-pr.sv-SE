Azure-portalen kan du hantera och skala PostgreSQL database-servrar. Du bestämmer dig för att skapa en Azure Database for PostgreSQL-server för att lagra runner prestandadata. Baserat på historisk avbildade datavolymer, vet du dina server-lagringsutrymmen ska vara inställd på 10 GB. För att stödja kraven för bearbetning, måste du beräkna Gen 5 support med 1 virtuell kärna. Du kan också veta att du vanligtvis lagrar säkerhetskopior i 25 dagar.

[!include[](../../../includes/azure-sandbox-activate.md)]

Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true). Azure-resurs-menyn för skapande och hantering visas på vänster och instrumentpanelen fylla resten av skärmen.

## <a name="create-an-azure-database-for-postgresql-server"></a>Skapa en Azure Database för PostgreSQL-server

När du har loggat in visas standard instrumentpanelen visas. Du har ett par alternativ som är tillgängliga för dig att skapa en Azure Database for PostgreSQL-server. Från instrumentpanelen kan du antingen:

- Välj den **alla tjänster** alternativet och söker sedan efter **Azure Database for PostgreSQL-server**. Den här skärmen visas alla konfigurerade servrar som redan finns i ditt konto. Härifrån kan du välja **Lägg till**, som tar dig till bladet skapa en ny server.

eller

- Välj den **skapa en resurs** alternativ, vilket visar alternativ för Azure Marketplace. Härifrån kan du välja den **databaser** och väljer **Azure Database for PostgreSQL**.

### <a name="configure-the-server"></a>Konfigurera servern

PostgreSQL-servern skapa bladet som liknar följande bild visas nu.

![Skärmbild av Azure-portalen som visar bladet skapa för en ny databas för PostgreSQL](../media-draft/4-create-blade.png)

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

> [!NOTE]
> Du måste komma ihåg eller Skriv ned viss information när du skapar PostgreSQL-servern. Till exempel det användarnamn och lösenord för att få åtkomst till servern. Du använder den här informationen för att ansluta till servern senare.

1. Välj ett unikt namn för servern. Kom ihåg att namnet får bara innehålla gemener och kan ha siffror och bindestreck.

1. Välj en prenumeration. Kontrollera att det här fältet har angetts till den prenumeration som du vill använda.

1. Nu har du alternativet för att skapa eller återanvända en befintlig resursgrupp. Välj **Använd befintlig** och välj <rgn>[Sandbox resursgruppens namn]</rgn> i listrutan. Du använder den här gruppen för resten av den här modulen.

1. Välj källa för den nya servern. Den här övningen ska du lämna alternativet inställt på _tom_. Kom ihåg att du kan ändra alternativet för att _säkerhetskopiera_ om du vill återställa en befintlig server-säkerhetskopia.

1. Välj ett inloggningsnamn som ska användas som en administratör logga in för den nya servern. Kom ihåg att inloggningsnamnet för administratören inte får vara azure_superuser, azure_pg_admin, admin, administrator, root, Gäst eller offentlig. Det får inte börja med pg_. Kom ihåg eller Skriv ned namnet för framtida användning.

1. Välj ett lösenord ska användas med ovanstående inloggningsnamnet för administratören. Kom ihåg, = att vår lösenord måste innehålla tecken från tre av följande kategorier:
   - Engelska versala bokstäver
   - Engelska gemena bokstäver
   - Siffror (0 till 9)
   - Icke-alfanumeriska tecken (!, $, #, % och så vidare)

   Kom ihåg eller Skriv ned lösenordet för framtida användning.

1. Ange lösenordet för att bekräfta ditt lösenord på nytt.

1. Välj en plats för din server. Du vill välja en plats som är närmast dig.

1. Nu väljer du versionen av din server. Välj den senaste versionen av PostgreSQL.

1. Näst sista steget, väljer den **prisnivå** alternativet.

    Kom ihåg att du måste konfigurera servern med specifika lagringsutrymmen och alternativ:

    - 10 GB disklagring
    - Compute Generation 5-stöd
    - Kvarhållningsperiod på 25 dagar

    Klicka på **prisnivå** att komma åt prisnivåbladet och gör följande ändringar:

    - Välj den **grundläggande** på fliken Alternativ.
    - Välj den **Gen 5 beräkning Generation** alternativet.
    - Välj 1 virtuell kärna från den **vCore** skjutreglaget. Observera hur ändringar i skjutreglaget påverkar den **Prissammanfattning**.
    - Välj 10 GB från den **Storage** skjutreglaget. Om du har problem med glidande exakt 10 GB, kan du använda tangenterna vänster och höger markören på tangentbordet för att hämta ett exakt värde.
    - Välj 25 dagar från det **kvarhållningsperiod** skjutreglaget.

        ![Skärmbild av Azure-portalen som visar databasprisnivå för en ny databas för PostgreSQL](../media-draft/4-azure-db-pricing-tier.png)

    - Klicka på **OK** när du är nöjd med ditt val att bekräfta dina val och stänga alternativen för prisnivå.

1. Allt som återstår nu är att granska de värden som du har angett och klicka på **skapa**. Skapa kan ta flera minuter. Du kan välja meddelandeikonen (en bjällra) högst upp på skärmen för Azure portal för att övervaka förloppet.

Nu har du en PostgreSQL-server som är tillgängliga. I nästa enhet visas hur du skapar samma server med Azure CLI.
