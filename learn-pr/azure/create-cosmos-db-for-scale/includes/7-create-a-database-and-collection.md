Nu när du förstår hur enheter för programbegäran används för att fastställa databasens dataflöde och hur partitionsnyckeln skapar utskalningsstrategin för din databas, är du redo att skapa din databas och samling.

## <a name="creating-your-database-and-collection"></a>Skapa databas och samling

1. Klicka på **Datautforskaren** > **Ny samling** i Azure-portalen.
    
    Området **Lägg till samling** visas längst till höger. Du kan behöva rulla till höger för att se den.

    ![Datautforskaren i Azure-portalen, bladet Lägg till samling](../media/5-create-a-database-and-collection/azure-cosmosdb-data-explorer.png)

2. På sidan **Lägg till samling** anger du inställningarna för den nya samlingen.

    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    Databas-id|Användare|Ange *Användare* som namn på den nya databasen. Databasnamn måste innehålla mellan 1 och 255 tecken och får inte innehålla /, \\, #, ? eller avslutande blanksteg.
    Samlings-id|WebCustomers|Ange *WebCustomers* som namn på din nya samling. Samma teckenkrav gäller för samlings-ID:n som databasnamn.
    Lagringskapacitet| Obegränsat |Använd standardvärdet **Obegränsat**. Det här värdet är databasens lagringskapacitet som gör det möjligt att skala upp din databas vid behov.
    Partitionsnyckeln|/UserId|Användar-ID är en bra partitionsnyckel i ett scenario med nätbutiker, eftersom så många frågor baseras på kund-ID:n.
    Dataflöde|1 000 RU|Ändra genomflödet till 1 000 enheter för programbegäran per sekund (RU/s). 1 000 är det minsta RU/s-värde som du kan ange för att aktivera automatisk skalning.
    
    För tillfället markerar vi inte alternativet Etablera databasens dataflöde och vi lägger inte heller till några unika nycklar i samlingen. 
    
3. Klicka på **OK**.

    Datautforskaren visar den nya databasen och samlingen.

    ![Datautforskaren i Azure-portalen visar den nya databasen och samlingen](../media/5-create-a-database-and-collection/azure-cosmos-db-new-collection.png)

## <a name="summary"></a>Sammanfattning

I den här enheten använde du dina kunskaper om partitionsnycklar och enheter för programbegäran till att skapa en databas och en samling med ett dataflöde och en skalning som passar dina affärsbehov.