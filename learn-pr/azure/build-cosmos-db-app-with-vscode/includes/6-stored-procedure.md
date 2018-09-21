Du måste ofta uppdatera flera dokument i dina databaser samtidigt. Här går vi igenom hur du skapar, registrerar och kör lagrade procedurer från dina .NET-konsolprogram.

## <a name="create-a-stored-procedure-in-your-app"></a>Skapa en lagrad procedur i din app

I den här lagrade proceduren används OrderId, som innehåller en lista med alla artiklar i ordern, till att beräkna en ordersumma. Ordersumman beräknas genom att artiklarna i ordern summeras, minus eventuell kredit kunden har och med hänsyn till eventuella rabattkoder.

1. I Visual Studio Code går du till fliken **Azure: Cosmos DB**, expanderar ditt Azure Cosmos DB-konto > **Användare** > **WebCustomers**, högerklickar på **Lagrade procedurer** och klickar sedan på **Skapa lagrad procedur**.

1. I textrutan längst upp på skärmen skriver du `UpdateOrderTotal`. Klicka på Retur för att ge den lagrade proceduren ett namn.

1. På fliken **Azure: Cosmos DB** expanderar du **Lagrade procedurer** och klickar på **UpdateOrderTotal**.

    En lagrad procedur hämtar som standard det första objektet.

1. Om du vill köra den lagrade standardproceduren från ditt program lägger du till följande kod i filen **Program.cs**.

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "UpdateOrderTotal"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
        Console.WriteLine("Stored procedure complete");
    }
    ```

1. Kopiera nedanstående kod och klistra in den före raden `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` i metoden **BasicOperations**.

    ```csharp
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. I den integrerade terminalen använder du följande kommando för att köra exemplet med den lagrade proceduren.

    ```bash
    dotnet run
    ```

Konsolen visar utdata som anger att den lagrade proceduren har slutförts.
