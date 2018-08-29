Du måste ofta uppdatera flera dokument i dina databaser samtidigt. Här går vi igenom hur du skapar, registrerar och kör lagrade procedurer från dina .NET-konsolprogram.

## <a name="create-a-stored-procedure-in-your-app"></a>Skapa en lagrad procedur i din app

I den här lagrade proceduren används OrderId, som innehåller en lista med alla artiklar i ordern, till att beräkna en ordersumma. Ordersumman beräknas genom att artiklarna i ordern summeras, minus eventuell kredit kunden har och med hänsyn till eventuella rabattkoder.

1. Kopiera följande kod och klistra in den i slutet av filen Program.cs.

    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->
    ```csharp
    private async Task UpdateOrderTotal(string databaseName, string collectionName, Order orderId)
    {
    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "ValidateDocumentAge"), document, 1920);
    }
    ```

2. Kopiera sedan följande kod och klistra in den i slutet av metoden **BasicOperations**.

    ```
    await this.UpdateOrderTotal("Users", "WebCustomers", "5-6235");
    ```

3. Spara filen Program.cs och kör följande kommando i den integrerade terminalen.

    ```
    dotnet run
    ```

    Konsolen visar följande resultat.

    ```
    Order total is 90.85.
    Press any key to continue ...
    ```

## <a name="clean-up"></a>Rensa

Om du planerar att fortsätta med modulerna i den här utbildningsvägen kan du hoppa över rensningsprocessen. Annars använder du följande steg för att ta bort dina resurser så att du inte debiteras för användning av tjänsten.

1. Välj **Resursgrupper** i Azure Portal längst till vänster och välj sedan den resursgrupp du skapat.  

    Om den vänstra menyn döljs klickar du på ![Knappen Expandera](../media/5-javascript-programming/expand.png) för att expandera den.

   ![Mått i Azure Portal](../media/5-javascript-programming/delete-resources-select.png)

2. Markera resursgruppen i det nya fönstret och klicka sedan på **Ta bort resursgrupp**.

   ![Mått i Azure Portal](../media/5-javascript-programming/delete-resources.png)

3. I det nya fönstret, skriv namnet på resursgruppen som ska tas bort och klicka sedan på **Ta bort**.

## <a name="summary"></a>Sammanfattning

I den här modulen har du skapat ett .NET Core-konsolprogram som skapar, uppdaterar och tar bort användarposter, ställer frågor mot användarna med hjälp av SQL och LINQ samt kör en lagrad procedur för att skapa en ordersumma som tar hänsyn till krediter och rabatter.