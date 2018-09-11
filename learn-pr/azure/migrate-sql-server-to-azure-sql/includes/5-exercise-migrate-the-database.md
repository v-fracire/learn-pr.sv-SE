Nu när du har utvärderat din databas och åtgärdat eventuella rapporterade fel är du redo att migrera din databas. I den här delen kommer du att migrera din SQL-databas till Azure SQL Database.

## <a name="setup"></a>Konfiguration

Ladda ned exempeldata om du inte redan har gjort det.

1. Öppna en webbläsare och navigera till https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.

1. I **OLTP downloads** (OLTP-nedladdningar) klickar du på **AdventureWorks2008R2.bak**.

1. I Management Studio återställer du **AdventureWorks 2008R2** till din standardinstans.

## <a name="create-an-azure-sql-database"></a>Skapa en Azure SQL Database

1. Logga in på ditt Azure Portal-konto.

1. Klicka på **Skapa en resurs** längst upp till vänster i portalen.

1. Klicka på **Databaser** > **SQL Database**.

1. Ange följande fält i SQL Database-formuläret:

    |Fält|Beskrivning|
    |-----|---|
    |Databasnamn|Ange ett namn för din databas. Detta kan vara namnet på den befintliga databas som du migrerar eller valfritt nytt namn|
    |Resursgrupp|Välj ett resursgruppsnamn eller ange ett nytt resursgruppsnamn|
    |Välj källa|Välj *Tom databas*|

1. Under **Server** väljer du antingen en befintlig server eller anger ett globalt unikt **servernamn** en **inloggning för serveradministratör** ett **lösenord** (som du ska bekräfta) samt en **plats**.

    > [!NOTE]
    > Du använder servernamnet, inloggningsnamnet och lösenordet för att ansluta till databasen.

1. Klicka på **Välj**.

1. I **Prisnivå** kan du välja en databas med högre prestanda, men för den här självstudien väljer du standardinställningen.

1. Klicka på **Skapa**. Vänta tills databasen har etablerats innan du fortsätter med självstudien.

    Följande skärmbild visar en potentiell konfiguration för den nya SQL-databasen.

    ![Skärmbild av ett nytt SQL Database-blad i Azure-portalen med en exempelkonfiguration.](../media-draft/5-create-azure-sql-db.png)

## <a name="create-a-new-migration-project"></a>Skapa ett nytt migreringsprojekt

1. Starta **Data Migration Assistant**.

1. Klicka på ikonen **Nytt** och ange följande alternativ:

    - **Projekttyp** – Välj alternativet *Migrering*.
    - **Projektnamn** – ange ett namn som är lätt att komma ihåg för projektet.
    - **Typ av källserver** – välj *SQL Server*.
    - **Typ av målserver** – välj *Azure SQL Database*.
    - **Migreringsomfång** – välj *Schema och data*.

1. Klicka på **Skapa**.

## <a name="specify-the-source-server-and-database"></a>Ange källserver och databas

1. Under **Connect to source server** (Anslut till källserver) går du till fältet **Servernamn** och anger namnet på SQL Server-källinstansen.

1. Välj den **autentiseringstyp** som stöds av SQL Server-källinstansen.
    > [!TIP]
    > Vi rekommenderar att du markerar kryssrutan **Kryptera anslutning** Anslutningsegenskaper för att kryptera anslutningen.

1. Välj **Anslut**.

1. Välj en enskild källdatabas att migrera till Azure SQL Database och klicka på **Nästa**.

## <a name="specify-the-target-server-and-database"></a>Ange målserver och databas

1. Under **Connect to target server** (Anslut till målserver) går du till fältet **Servernamn** och anger namnet på Azure SQL Server Database-instansen. Lägg till **database.windows.net** till databasinstansen.

1. Välj den autentiseringstyp som stöds av Azure SQL Database-målinstansen. För den här självstudien väljer du **SQL Server-autentisering**.
    > [!TIP]
    > Vi rekommenderar att du markerar kryssrutan **Kryptera anslutning** Anslutningsegenskaper för att kryptera anslutningen.

1. Ange **användarnamnet** och **lösenordet** som du definierade när du skapade Azure SQL-databasen.

1. Klicka på **Anslut**.

1. Välj en enkel måldatabas att migrera till.
    > [OBS] Om du planerar att migrera Windows-användare ska du i textrutan för målets externa användares domännamn se till att målets externa användares domännamn anges korrekt.

1. Klicka på **Nästa**.

## <a name="select-schema-objects"></a>Välj schemaobjekt

1. Välj de schemaobjekt från källdatabasen som du vill migrera till Azure SQL Database.

    > [!NOTE]
    > Några av de objekt som inte kan konverteras som de är erbjuds möjligheter för automatisk korrigering. Om du klickar på objekten i den vänstra fönsterrutan visas de föreslagna korrigeringarna i den högra fönsterrutan. Granska korrigeringarna och välj att tillämpa eller ignorera alla ändringar, objekt för objekt. Observera att om du tillämpar eller ignorerar alla ändringar för ett objekt så påverkas inte ändringar för andra databasobjekt. Instruktioner som inte kan konverteras eller repareras automatiskt reproduceras till måldatabasen och kommenteras.

    ![Skärmbild av Data Migration Assistant som visar en lista med migreringsproblem som påverkar AdventureWorks SQL Server-data med felet ”Stored Procedure: dbo.uspSearchCandidateResumes” (Lagrad procedur: dbo.uspSearchCandidateResumes) markerat och probleminformation som visas.](../media-draft/5-suggested-fix.png)

1. Klicka på **Generera SQL-skript**.

## <a name="deploy-schema"></a>Distribuera schema

1. Klicka på **Distribuera schema**.

1. Granska resultatet av schemadistributionen.

## <a name="migrate-data"></a>Migrera data

1. Klicka på **Migrera data** för att datamigreringsprocessen.

1. Välj tabellerna med de data som du vill migrera.

1. Klicka på **Starta datamigrering**.

Den sista skärmen visar övergripande status.

## <a name="summary"></a>Sammanfattning

I den här delen skapade du en tom Azure SQL Database och migrerade en lokal SQL Server-databas till den nya databasen.