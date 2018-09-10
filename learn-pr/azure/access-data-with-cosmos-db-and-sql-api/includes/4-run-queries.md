Nu när du har lärt dig om vilka typer av frågor du kan skapa ska vi använda Datautforskaren i Azure Portal till att hämta och filtrera dina produktdata.

Observera i fönstret för Datautforskaren att frågan som standard är inställd på `SELECT * FROM c` på fliken Dokument. Den här standardfrågan hämtar och visar alla dokument i samlingen.

![Standardfrågan i Datautforskaren är ”SELECT * FROM c”](../media-draft/4-run-queries/azure-cosmosdb-data-explorer-query.png)

## <a name="create-a-new-query"></a>Skapa en ny fråga

1. I Datautforskaren klickar du på fliken **Ny SQL-fråga**. Observera att standardfrågan på den nya fliken **Fråga 1** är `SELECT * from c` och klicka sedan på **Kör fråga**. Frågan returnerar alla resultat i databasen.

    ![Ändra standardfrågan genom att lägga till ORDER BY c._ts DESC och klicka på Tillämpa filter](../media-draft/4-run-queries/azure-cosmosdb-data-explorer-edit-query.png)

2. Nu ska vi köra några av de frågor som diskuterades i föregående del. Ta bort `SELECT * from c` på frågefliken, kopiera och klistra in följande fråga och klicka sedan på **Kör fråga**.

    ```
    SELECT *
    FROM Products p
    WHERE p.id ="1"
    ```

    Resultaten returnerar produkten vars `productId` är 1.

    ![Ändra standardfrågan genom att lägga till ORDER BY c._ts DESC och klicka på Tillämpa filter](../media-draft/4-run-queries/azure-cosmosdb-data-explorer-query-by-id.png)

3. Ta bort den föregående frågan, kopiera och klistra in följande fråga, och klicka på **Kör fråga**. Den här frågan returnerar pris, beskrivning och produkt-ID för alla produkter, ordnade efter pris i stigande ordning.
 
    ```
    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC
    ```

## <a name="summary"></a>Sammanfattning

Nu har du slutfört några grundläggande frågor på dina data i Azure Cosmos DB. 