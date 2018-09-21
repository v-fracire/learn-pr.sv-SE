Vi börjar med att skapa en Azure Redis Cache-instans och sedan skapar vi en enkel transaktion som infogar två datavärden i cacheminnet.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a>Skapa en Azure Redis Cache

Vi börjar med en skapa en Azure Redis Cache med Azure CLI. Använd Cloud Shell till höger i webbläsarfönstret för att interagera med Azure.

Vi använder kommandot `az redis create` till att skapa en ny Azure Redis Cache. Det tar flera parametrar. Här är de vanligaste (en fullständig lista finns i dokumentationen.

> [!div class="mx-tableFixed"]
> | Parameter | Beskrivning |
> |-----------|-------------|
> | `--name`    | Namnet på cachen – det här måste vara globalt unik och bestå av bokstäver, siffror och bindestreck. |
> | `--resource-group` | Använd den färdiga resursgruppen **<rgn>[namn på sandbox-resursgrupp]</rgn>** som är en del av Azure-sandboxmiljön. |
> | `--location` | Ange den plats där cacheminnet ska finnas. Normalt väljer du en plats nära datakonsumenterna. I det här fallet begränsas du till de platser som är tillgängliga i Azure-sandboxmiljön. Välj den som är närmast dig. |
> | `--size` | Storleken på Azure Redis Cache. Giltiga värden är [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]. |
> | `--sku` | SKU för Azure Redis Cache. Giltiga värden är [Basic, Standard, Premium]. |

### <a name="select-a-location"></a>Välja en plats
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Skapa ett cacheminne med följande alternativ:
    - Storlek: C0
    - SKU: Basic

1. Här är ett exempel på en kommandorad. Ersätt parametern `[name]` med ett unikt namn. Du kan ersätta platsen om du vill använda en annan region än USA, östra.

    ```azurecli
    REDIS_NAME=[name]

    az redis create \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location eastus \
        --vm-size C0 \
        --sku Basic \
    ```

## <a name="create-a-net-core-console-application"></a>Skapa en .NET Core-konsolapp

Skapa sedan en .NET Core-konsolapp, som används till att infoga datavärden i Azure Redis Cache.

1. Skapa en ny .NET Core-app med integrerat Cloud Shell till höger i fönstret. Ge den namnet ”RedisData”.

    ```bash
    cd ~
    dotnet new console --name RedisData
    ```

1. Ändra i den nya katalogen som skapats för appen.

    ```bash
    cd RedisData
    ```

1. Skapa och kör appen – utdata bör vara ”Hello World!”

    ```bash
    dotnet run
    ```

## <a name="add-the-servicestackredis-nuget-package"></a>Lägg till ServiceStack.Redis NuGet-paketet

Nu när vi har en konsolapp måste vi lägga till **ServiceStack.Redis** NuGet-paketet. Då kan vi ansluta till Azure Redis Cache och utfärda kommandon i C#.

1. Lägg till NuGet-paketet **ServiceStack.Redis** med hjälp av terminalgränssnittet.

    ```bash
    dotnet add package ServiceStack.Redis
    ```

1. Skapa och kör appen igen för att kontrollera att allt kompileras. Utdata bör fortfarande vara ”Hello World!”

## <a name="get-your-azure-redis-cache-connection-string"></a>Hämta Azure Redis Cache-anslutningssträngen

För att ansluta till Azure Redis Cache behöver du en anslutningssträng som innehåller lösenordet och URL:en. **ServiceStack.Redis** har sitt eget format för anslutningssträng: `[password]@[hostname]:[sslport]?ssl=true`.

Du kan hämta lösenordet med Azure Portal eller med kommandoraden. Vi använder det senare här eftersom vi använde portalmetoden i modulen ”Optimera webbprogram genom att cachelagra skrivskyddade data med Redis”.

Hämta åtkomstnycklarna med kommandot `az redis list-keys`. Kör dessa kommandon för att hämta den primära nyckeln, lagra den i en variabel med namnet `REDIS_KEY` och visa den:

```azurecli
REDIS_KEY=$(az redis list-keys \
    --name "$REDIS_NAME" \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --query primaryKey \
    --output tsv)

echo $REDIS_KEY
```

Kör sedan detta kommando för att samla anslutningssträngen och visa den på kommandoraden. Observera att värdnamnet är namnet på cachen följt av `.redis.cache.windows.net` och porten är **6380**, standardporten för Redis SSL.

```bash
echo "$REDIS_KEY"@"$REDIS_NAME".redis.cache.windows.net:6380?ssl=true
```

Kopiera anslutningssträngen till urklipp &mdash; du kommer att använda det i nästa steg.

## <a name="add-the-connection-string-to-your-app"></a>Lägga till anslutningssträngen i appen

1. Anslut Cloud Shell-redigeraren från programmappen.

    ```bash
    cd ~/RedisData
    code .
    ```

1. Välj källfilen **Program.cs**.

1. Skapa följande fält i klassen `Program` och klistra in anslutningssträngen som värde.

    ```csharp
    static string redisConnectionString = "<connection string>";
    ```

    Här är ett exempel:

    ```csharp
    static string redisConnectionString = "ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6380?ssl=true";
    ```

## <a name="insert-two-data-values-into-your-azure-redis-cache"></a>Infoga två datavärden i Azure Redis Cache

Slutligen ska vi lägga till data i Azure Redis Cache.

1. Lägg till följande `using`-instruktion högst upp i filen **Program.cs**.

    ```csharp
    using ServiceStack.Redis;
    ```

1. Ersätt innehållet i metoden `Main` med följande kod. Detta använder en transaktion för att lägga till två värden.

    ```csharp
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    if (transactionResult)
    {
        Console.WriteLine("Transaction committed");
    }
    else
    {
        Console.WriteLine("Transaction failed to commit");
    }
    ```

1. Innan du bygger och kör programmet bör du kontrollera att Redis-cachen har etablerats fullständigt. Det kan ta några minuter efter att `az redis create` har slutförts. Kör följande kommando för att kontrollera statusen.

    ```azcli
    az redis show \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --query provisioningState
    ```

    Om statusen är `Creating` ska du kontrollera igen efter ett par minuter. Den visar `Succeeded` när den är klar.

1. Kör programmet och kontrollera att konsolen säger **Transaktionen har checkats in**.

    ```bash
    dotnet run
    ```

## <a name="verify-your-data"></a>Verifiera dina data

Vi slutför genom att verifiera att data vi har lagt till finns i Redis Cache.

1. Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. Leta reda på Azure Redis Cache genom att välja **Alla resurser** i sidofältet till vänster och med hjälp av filterfältet till vänster för att välja Azure Redis Cache-instanser. Du kan alternativt använda sökrutan högst upp och skriva namnet på cacheminnet.

1. Välj din Azure Redis Cache-instans.

1. På bladet **Översikt** för Azure Redis Cache väljer du **Konsol**. Azure Redis Cache-konsolen öppnas, där du kan ange Azure Redis Cache-kommandon på låg nivå.

1. Ange **get MyKey1**. Kontrollera att det returnerade värdet är **MyValue1**.

1. Ange **get MyKey2**. Kontrollera att det returnerade värdet är **MyValue2**.

    ![Skärmbild av Azure Redis Cache-konsolen som visar värdena MyKey1 och MyKey2.](../media/4-redis-console.png)