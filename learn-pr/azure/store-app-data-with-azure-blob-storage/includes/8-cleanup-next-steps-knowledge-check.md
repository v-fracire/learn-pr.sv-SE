I den här modulen har du lärt dig hur du använder Azure Blob Storage för att lagra data för webbprogrammet. Vi gick igenom tips för att skapa en strategi för att använda Blob Storage i en webbapp och hur du använder Azure Storage SDK för .NET Core för att skriva till och läsa från blobar. Appen som vi skapade accepterar uppladdade filer från användare, lagrar dem i Blob Storage och gör dem tillgängliga för nedladdning.

## <a name="cleanup"></a>Rensa

Om du vill rensa i din Azure-prenumeration kör du följande i Azure Cloud Shell för att ta bort resursgruppen som innehåller alla de resurser som vi skapade i den här modulen.

```console
az group delete --name blob-exercise-group --yes --no-wait
```

Du kan rensa Cloud Shell-lagringen genom att ta bort katalogen `FileUploader`.

## <a name="further-reading"></a>Ytterligare läsning

- **Securely storing secrets like connection strings** (Lagra hemligheter som anslutningssträngar på ett säkert sätt): den mest robusta slutpunkt-till-slutpunkt-lösningen för att lagra hemliga konfigurationsvärden är Azure Key Vault. Information om hur du använder Key Vault i ett ASP.NET Core-program finns [här](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x). Du kan även lagra anslutningssträngar på ett säkert sätt i inställningarna för App Service-programmet och använda [ASP.NET Core Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows) (ASP.NET Core Secret Manager-verktyget) för utvecklarmiljöer.
- [Uploading large files with streaming in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads?view=aspnetcore-2.1#uploading-large-files-with-streaming) (Ladda upp stora filer med direktuppspelning i ASP.NET Core)
- [Blob concurrency: AccessConditions and blob leases](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/) (Blob-samtidighet: AccessConditions och blob-lån)
- [Granting limited access to Azure Storage object with shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) (Bevilja begränsad åtkomst till Azure Storage-objekt med signaturer för delad åtkomst)
- [Indexing Blob storage with Azure Search](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage) (Indexera Blob Storage med Azure Search)
- [Container and blob name restrictions](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#resource-names) (Begränsningar för container och blobnamn)