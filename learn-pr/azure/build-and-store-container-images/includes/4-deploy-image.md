Du kan hämta containeravbildningar från Azure Container Registry från flera plattformar för containerhantering, som Azure Container Instances, Azure Kubernetes Registry och Docker för Windows eller Mac. När du kör containeravbildningar från Azure Container Registry kan du behöva autentiseringsuppgifter. Du bör använda ett huvudnamn för Azure-tjänsten till autentisering i Container Registry. Dessutom bör du skydda autentiseringsuppgifterna för Azure-tjänstens huvudnamn i Azure Key Vault.

I den här utbildningsenheten får du skapa ett huvudnamn för ditt Azure-containerregister, lagra det i Key Vault och sedan distribuera containern till Azure Container Instances med hjälp av autentiseringsuppgifterna för tjänstens huvudnamn.

## <a name="configure-registry-authentication"></a>Konfigurera registerautentisering

Du bör använda huvudnamn för åtkomst till Azure-containerregister i alla produktionsscenarier. Med ett huvudnamn för tjänsten får du rollbaserad åtkomstkontroll (RBAC) för containeravbildningarna. Du kan till exempel konfigurera ett huvudnamn för tjänsten som enbart har hämtningsåtkomst till ett register.

Om du inte redan har ett valv i Azure Key Vault skapar du ett med följande kommandon i Azure CLI.

Skapa först en variabel med namnet på ditt containerregister. Den här variabeln används i hela utbildningsenheten.

```azurecli
ACR_NAME=<acrName>
```

Skapa ett Azure-nyckelvalv med kommandot `az keyvault create`.

```azurecli
az keyvault create --resource-group myResourceGroup --name $ACR_NAME-keyvault
```

Nu måste du skapa ett huvudnamn för tjänsten och lagra autentiseringsuppgifterna för det i nyckelvalvet.

Använd cmdleten `az ad sp create-for-rbac` till att skapa tjänstens huvudnamn. Argumentet `--role` konfigurerar huvudnamnet för tjänsten med rollen *läsare*, vilket endast ger hämtningsåtkomst till registret. Om du vill bevilja både push- och hämtningsåtkomst ändrar du argumentet `--role` till *deltagare*.

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

Så här ser utdata ut när du har skapat tjänsten huvudnamn. Anteckna värdena för `appId` och `password`. De här värdena lagras i Azure-nyckelvalvet.

```bash
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

Använd sedan kommandot `az keyvault secret set` till att lagra *appId*-värdet för tjänstens huvudnamn i valvet. Ersätt `<appId>` med `appId`-värdet för tjänstens huvudnamn.

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <appId>
```

Använd nu kommandot `az keyvault secret set` till att lagra *password*-värdet för tjänstens huvudnamn i valvet. Ersätt `<password>` med `password`-värdet för tjänstens huvudnamn.

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
```

Du har skapat ett Azure-nyckelvalv och lagrat två hemligheter i det:

* `$ACR_NAME-pull-usr`: ID:t för tjänstens huvudnamn som ska användas som **användarnamn** för containerregistret.
* `$ACR_NAME-pull-pwd`: lösenordet för tjänstens huvudnamn som ska användas som **lösenord** för containerregistret.

Nu kan du referera till de här hemligheterna med namn när du eller dina program och tjänster hämtar avbildningar från registret.

### <a name="deploy-a-container-with-azure-cli"></a>Distribuera en container med Azure CLI

Nu när autentiseringsuppgifterna för tjänstens huvudnamn lagras i Azure Key Vault kan dina program och tjänster använda dem för att få åtkomst till ditt privata register.

Kör kommandot `az container create` för att distribuera en containerinstans. I kommandot används de autentiseringsuppgifter för tjänstens huvudnamn som lagras i Azure Key Vault för autentisering i containerregistret.

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

Hämta Azure-containerinstansens IP-adress.

```azurecli
az container show --resource-group myResourceGroup --name acr-build --query ipAddress.ip --output table
```

Öppna en webbläsare och navigera till containerns IP-adress. Om allt är konfigurerat på rätt sätt bör du se följande resultat:

![Exempel på ett webbprogram med texten Hello World](../media/hello.png)

