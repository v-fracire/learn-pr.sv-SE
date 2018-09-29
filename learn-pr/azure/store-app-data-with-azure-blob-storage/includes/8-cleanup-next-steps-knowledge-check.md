<span data-ttu-id="d80da-101">I den här modulen har du lärt dig hur du använder Azure Blob Storage för att lagra data för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d80da-101">In this module, you learned how to use Azure Blob storage to store web application data.</span></span> <span data-ttu-id="d80da-102">Vi gick igenom tips för att skapa en strategi för att använda Blob Storage i en webbapp och hur du använder Azure Storage SDK för .NET Core för att skriva till och läsa från blobar.</span><span class="sxs-lookup"><span data-stu-id="d80da-102">We discussed tips for creating a strategy to use Blob storage in a web app and how to use the Azure Storage SDK for .NET Core to write to and read from blobs.</span></span> <span data-ttu-id="d80da-103">Den app som vi skapade accepterar uppladdade filer från användare, lagrar dem i Blob Storage och gör dem tillgängliga för nedladdning.</span><span class="sxs-lookup"><span data-stu-id="d80da-103">The app we made accepts uploaded files from users, stores them in Blob storage, and makes them available for download.</span></span>

[!include[](../../../includes/azure-sandbox-cleanup.md)]

<span data-ttu-id="d80da-104">Du kan rensa Cloud Shell-lagringen genom att ta bort katalogen `mslearn-store-data-in-azure`.</span><span class="sxs-lookup"><span data-stu-id="d80da-104">To clean up your Cloud Shell storage, delete the `mslearn-store-data-in-azure` directory.</span></span>

<!---TODO: Remove further reading
## Further reading

- **Securely storing secrets like connection strings**: The most robust end-to-end solution for storing secret configuration values is Azure Key Vault. See [here](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x) for information about using Key Vault in an ASP.NET Core application. Alternatively, you can safely store connection strings in App Service application settings and use the [ASP.NET Core Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows) to support developer environments.
- [Uploading large files with streaming in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads?view=aspnetcore-2.1#uploading-large-files-with-streaming)
- [Blob concurrency: AccessConditions and blob leases](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/)
- [Granting limited access to Azure Storage object with shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Indexing Blob storage with Azure Search](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage)
- [Container and blob name restrictions](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#resource-names)
--->