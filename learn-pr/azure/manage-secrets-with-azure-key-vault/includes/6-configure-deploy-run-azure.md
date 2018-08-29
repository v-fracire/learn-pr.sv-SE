Nu är det dags att köra vår app i Azure. Vi måste skapa en Azure App Service-app, konfigurera den med MSI och vår valvkonfiguration, samt distribuera vår kod.

## <a name="exercise"></a>Övning

### <a name="create-the-app-service-plan-and-app"></a>Skapa App Service-planen och appen

Att skapa en App Service-app är en tvåstegsprocess: Först skapas *planen*, sedan *appen*.

Namnet på *planen* behöver bara vara unikt inom din prenumeration, så du kan använda samma namn som vi har använt: **keyvault-exercise-plan**. Appnamnet måste däremot vara globalt unikt, så du måste välja ett eget.

Kör följande i Azure Cloud Shell:

```azurecli
az appservice plan create --name keyvault-exercise-plan --resource-group keyvault-exercise-group
az webapp create --name <your-unique-app-name> --plan keyvault-exercise-plan --resource-group keyvault-exercise-group
```

### <a name="add-configuration-to-the-app"></a>Lägga till konfigurationen i appen

När vi distribuerar till Azure följer vi rekommenderade metoder för App Service och placerar konfigurationen i programinställningarna i stället för i en konfigurationsfil.

```azurecli
az webapp config appsettings set --name <your-unique-app-name> --resource-group keyvault-exercise-group --settings VaultName=<your-unique-vault-name>
```

### <a name="enable-msi"></a>Aktivera MSI

Aktivering av MSI i en app görs på en rad:

```azurecli
az webapp identity assign --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

Kopiera värdet **principalId** från de JSON-utdata du får. PrincipalId är det unika ID:t för appens nya identitet i Azure Active Directory och vi ska använda det i nästa steg.

### <a name="grant-access-to-the-vault"></a>Bevilja åtkomst till valvet

Nu måste du ge din appidentitet behörighet att hämta och lista hemligheter från valvet för produktionsmiljön. Använd värdet **principalId** som du kopierade i föregående steg som värde för **object-id** i kommandot nedan.

```azurecli
az keyvault set-policy --name <your-unique-vault-name> --object-id <your-msi-principleid> --secret-permissions get list
```

### <a name="deploy-the-app-and-try-it-out"></a>Distribuera appen och testa den

Din konfiguration är klar och du är redo att distribuera! Nedanstående kommandon publicerar webbplatsen till mappen `pub`, zippar upp den till `site.zip` och distribuerar zip-filen till App Service.

> [!NOTE]
> Du måste `cd` tillbaka till katalogen KeyVaultDemoApp om du inte redan har gjort det.

```console
dotnet publish -o pub
zip -j site.zip pub/*
az webapp deployment source config-zip --src site.zip --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

När du har fått ett resultat som visar att webbplatsen har distribuerats, öppnar du `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` i en webbläsare. Du bör nu se det hemliga värdet **reindeer_flotilla**.

Din app är klar och distribuerad!