Du måste ofta uppdatera flera dokument i dina databaser samtidigt. Här går vi igenom hur du skapar, registrerar och kör lagrade procedurer från dina .NET-konsolprogram.

## <a name="create-a-stored-procedure-in-your-app"></a>Skapa en lagrad procedur i din app

I den här lagrade proceduren används OrderId, som innehåller en lista med alla artiklar i ordern, till att beräkna en ordersumma. Ordersumman beräknas genom att artiklarna i ordern summeras, minus eventuell kredit kunden har och med hänsyn till eventuella rabattkoder.

1. I Visual Studio Code på fliken Azure expanderar du **inlärningsmodulen (SQL)** > **Användare** > **WebCustomers**. Högerklicka sedan på **Lagrade procedurer** och klicka på **Skapa lagrad procedur**.

1. I textrutan längst upp på skärmen skriver du *UpdateOrderTotal*. Klicka på Retur för att ge den lagrade proceduren ett namn.

1. Expandera **Lagrade procedurer** och klicka på **UpdateOrderTotal**.

1. En lagrad procedur hämtar som standard det första objektet.

1. Om du vill köra den lagrade proceduren från ditt program, lägger du till följande kod i filen Program.cs.

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        try
        {
            await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "sample"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            Console.WriteLine("Stored procedure complete");
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }
    ```
    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->

1. Kopiera sedan nedanstående kod och klistra in den i slutet av metoden **BasicOperations**.

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. I den integrerade terminalen använder du följande kommando för att köra exemplet med den lagrade proceduren.

    ```
    dotnet run
    ```
    Konsolen visar utdata som anger att den lagrade proceduren har slutförts.

## <a name="clean-up"></a>Rensa

Om du planerar att fortsätta med modulerna i den här utbildningsvägen kan du hoppa över rensningsprocessen. Annars använder du följande steg för att ta bort dina resurser så att du inte debiteras för användning av tjänsten.

1. Välj **Resursgrupper** i Azure Portal längst till vänster och välj sedan den resursgrupp som du skapade.  

    Om den vänstra menyn döljs klickar du på ![Knappen Expandera](../media/5-javascript-programming/expand.png) för att expandera den.

   ![Mått i Azure Portal](../media/5-javascript-programming/delete-resources-select.png)

1. Markera resursgruppen i det nya fönstret och klicka sedan på **Ta bort resursgrupp**.

   ![Mått i Azure Portal](../media/5-javascript-programming/delete-resources.png)

1. I det nya fönstret skriver du namnet på resursgruppen som ska tas bort. Klicka sedan på **Ta bort**.

## <a name="summary"></a>Sammanfattning

I den här modulen har du skapat ett .NET Core-konsolprogram som skapar, uppdaterar och tar bort användarposter, ställer frågor till användarna med hjälp av SQL och LINQ, samt kör en lagrad procedur för att fråga efter objekt i databasen.