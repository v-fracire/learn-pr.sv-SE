I den här kursdelen utvärderar du en befintlig databas med hjälp av Data Migration Assistant och granskar funktioner som används i den lokala SQL Server-instansen som för närvarande inte stöds av Azure SQL Database.

## <a name="setup"></a>Konfiguration

1. [Installera **Data Migration Assistant**](https://www.microsoft.com/download/details.aspx?id=53595) om du inte redan har gjort det.

1. Du behöver en SQL Server-instans som körs. Kontrollera att du har anslutningsinformationen tillgänglig.

<!-- TODO: replace with an LOD VM -->

1. Öppna en webbläsare och navigera till https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.

1. I **OLTP downloads** (OLTP-nedladdningar) klickar du på **AdventureWorks2008R2.bak** och sparar den till den lokala datorn.

1. I Management Studio återställer du *AdventureWorks 2008R2* till din standardinstans.

## <a name="create-an-assessment"></a>Skapa en utvärdering

1. Starta **Microsoft Data Migration Assistant**.

1. I appens vänstra navigeringsfönstret klickar du på __+__ för att skapa ett nytt Data Migration Assistant-projekt.

1. Ange följande alternativ:

    - **Projekttyp** – välj *Utvärdering*
    - **Projektnamn** – ange ett namn för ditt projekt, till exempel ”Bicycle DB Assessment”
    - **Typ av källserver** – välj *SQL Server*
    - **Typ av målserver** – välj *Azure SQL Database*

1. Klicka på **Skapa**.
    ![Skärmbild som visar den konfiguration som beskrivs i Data Migration Assistant för dina AdventureWorks SQL Server-data.](../media-draft/3-create-assessment.png)

1. Välj typ av utvärderingsrapport – markera båda:
    - Kontrollera databaskompatibilitet
    - Kontrollera funktionsparitet

1. Klicka på **Nästa**.

## <a name="add-databases-to-assess"></a>Lägg till databaser att utvärdera

1. Om **Anslut till en Server** inte visas till höger klickar du på **Lägg till källor** så att anslutningsmenyn öppnas.

1. Gör följande:
    - Ange ditt befintliga SQL-serverinstansnamn
    - Välj **Autentiseringstyp**
    - Ange anslutningsegenskaper för servern

1. Klicka på **Anslut**.

1. I **Lägg till källor** väljer du de databaser som ska utvärderas. För den här övningen väljer du **AdventureWorks2008R2**.

1. Klicka på **Lägg till**.
    > [!NOTE]
    > Om du vill lägga till databaser från flera SQL Server-instanser använder du knappen **Lägg till källor**. Du kan ta bort flera databaser genom att hålla ned tangenterna SKIFT + CTRL, markera de databaser som ska tas bort och sedan klicka på **Ta bort källor**.

1. Klicka på **Starta utvärderingen**.

## <a name="view-results"></a>Visa resultat

Om det finns flera databaser visas resultaten för varje databas när de blir tillgängliga. Du behöver inte vänta tills alla databasutvärderingar är klara.

1. När utvärderingen för **AdventureWorks** är klar klickar du på alternativknapparna **Kompatibilitetsproblem** och **Funktionsparitet för SQL Server** för att visa resultatet.
    - Kategorin för SQL Server-funktionsparitet visar en lista över funktioner som kanske inte stöds fullständigt samt åtgärder för att lösa dessa problem. Problem med funktionsparitet stoppar inte en migrering.
    - Kategorin för kompatibilitetsproblem visar en lista över funktioner som skulle blockera en migrering samt åtgärder för att lösa dessa problem.

## <a name="summary"></a>Sammanfattning

I den här kursdelen utvärderade en lokalt installerad SQL Server-databas för att kontrollera om några funktioner skulle bli tillgängliga när du migrerar databasen till Azure SQL Database.