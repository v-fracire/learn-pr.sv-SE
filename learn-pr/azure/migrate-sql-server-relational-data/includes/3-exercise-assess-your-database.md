I den här övningen utvärderar du en befintlig databas med hjälp av Data Migration Assistant och granskar funktioner som används i den lokala SQL Server-instansen som för närvarande inte stöds i Azure SQL Database.

## <a name="setup"></a>Konfiguration

1. [Installera **Data Migration Assistant**](https://www.microsoft.com/en-us/download/details.aspx?id=53595) om du inte redan har gjort det.

1. Du behöver en SQL Server-instans som körs. Kontrollera att du har anslutningsinformationen tillgänglig.
1. [*** ersätt sannolikt med en virtuell LOD-dator ***] <!-- TODO: -->

1. Öppna en webbläsare och navigera till https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017.

1. I **OLTP downloads** (OLTP-nedladdningar) klickar du på **AdventureWorks2008R2.bak** och sparar den till den lokala datorn.

1. I Management Studio återställer du *AdventureWorks 2008R2* till din standardinstans.

## <a name="create-an-assessment"></a>Skapa en utvärdering

1. Starta **Microsoft Data Migration Assistant**.

1. I appens vänstra navigeringsfält klickar du på __+__ för att skapa ett nytt DMA-projekt.

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

1. Klicka på **Lägg till källor** för att öppna anslutningsmenyn.
2. Ange följande information
    - Ange ditt befintliga SQL-serverinstansnamn
    - Välj **Autentiseringstyp**
    - Ange anslutningsegenskaper för servern
3. Klicka på **Anslut**.
4. I **Lägg till källor** väljer du de databaser som ska utvärderas. För den här övningen väljer du **AdventureWorks2008R2**.
5. Klicka på **Lägg till**.
    > [!NOTE]
    > Om du vill lägga till databaser från flera SQL Server-instanser använder du knappen **Lägg till källor**. Du kan ta bort flera databaser genom att hålla ned tangenterna SKIFT + CTRL, markera de databaser som ska tas bort och sedan klicka på **Ta bort källor**.

6. Klicka på **Starta utvärderingen**.

## <a name="view-results"></a>Visa resultat

Om det finns flera databaser visas resultaten för varje databas när de blir tillgängliga. Du behöver inte vänta tills alla databasutvärderingar är klara.

1. När utvärderingen för **AdventureWorks** är klar klickar du på **Kompatibilitetsproblem** och **Funktionsrekommendationer** för att visa resultatet.
    - Kategorin för SQL Server-funktionsparitet visar en lista över funktioner som kanske inte stöds fullständigt samt åtgärder för att lösa dessa problem. Problem med funktionsparitet stoppar inte en migrering.
    - Kategorin för kompatibilitetsproblem visar en lista över funktioner som skulle blockera en migrering samt åtgärder för att lösa dessa problem.

## <a name="summary"></a>Sammanfattning

I den här övningen utvärderade en lokalt installerad SQL Server-databas för att kontrollera om några funktioner skulle bli tillgängliga när du migrerar databasen till Azure SQL Database.