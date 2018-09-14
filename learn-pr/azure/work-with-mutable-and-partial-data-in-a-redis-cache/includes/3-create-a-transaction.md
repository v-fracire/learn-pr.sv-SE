Låt oss börja med att skapa en instans av Azure Redis Cache och sedan skapa en enkel transaktion som infogar två datavärden i cacheminnet.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a>Skapa en Azure Redis Cache

Låt oss börja med att skapa en Azure Redis Cache med Azure CLI. Använda Cloud Shell till höger i webbläsarfönstret för att interagera med Azure.

Vi använder den `azure rediscache create` kommando för att skapa en ny Azure Redis Cache. Det tar flera parametrar. Här är de vanligaste (om du vill hämta en fullständig lista finns i dokumentationen).

> [!div class="mx-tableFixed"]
> | Parameter | Beskrivning |
> |-----------|-------------|
> | `--name`    | Namnet på cachen: Det måste vara globalt unikt och består av bokstäver, siffror och bindestreck. |
> | `--resource-group` | Använd färdiga resursgrupp <rgn>[Sandbox resursgruppens namn]</rgn>, vilket är en del av Azure-Sandbox. |
> | `--location` | Ange platsen där cachen ska vara belägen. Normalt ska du välja en plats nära datakonsumenterna. I det här fallet är du begränsad till platserna som är tillgängliga i Azure-Sandboxen. Välj det närmaste till dig. |
> | `--size` | Storleken på Azure Redis Cache. Giltiga värden är [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]. |
> | `--sku` | The Azure Redis Cache SKU. Giltiga värden är [Basic, Standard, Premium]. |
> | `--enable-non-ssl-port` | Lägg till den här flaggan om du vill aktivera porten utan SSL för cacheminnet. |

### <a name="selecting-a-location"></a>Att välja en plats
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Skapa ett cacheminne med hjälp av följande alternativ:
    - Storlek: C0
    - SKU: Basic
    - EnableNonSslPort
    
1. Här är ett exempel från kommandoraden. Ersätt den **[name]** och **[plats]** med giltiga värden.

    ```azurecli
    azure rediscache create \
        --name [name] \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location [location] \
        --size C0 --sku Basic
    ```

Det tar några minuter att skapa cachen.

## <a name="create-a-net-core-console-application"></a>Skapa en .NET Core-konsolapp

Skapa sedan ett .NET Core C#-baserade konsolprogram som ska användas för att infoga värden i vårt Azure Redis Cache.

1. Skapa ett nytt .NET Core-program med integrerad Cloud Shell på höger sida av fönstret. Ge den namnet ”RedisData”.

    ```bash
    dotnet new console --name RedisData
    ```
    
1. Ändra till den nya katalogen som skapats för din app.

    ```bash
    cd RedisData
    ```
    
1. Du bör få en projektfil och en enda **Program.cs** källfilen.

1. Skapa och köra programmet – den returnera ”Hello, World”!

    ```bash
    dotnet run
    ```
    
## <a name="add-the-servicestackredis-nuget-package"></a>Lägg till ServiceStack.Redis NuGet-paketet

Nu när vi har vår konsolprogram behöver vi lägga till den **ServiceStack.Redis** NuGet-paketet. Detta gör att vi kan ansluta till Azure Redis Cache och ge kommandon i C#.

1. Lägg till NuGet-paketet **ServiceStack.Redis** med terminal shell.

    ```bash
    dotnet add package ServiceStack.Redis
    ```
    
1. Skapa och kör programmet igen för att se till att alla kompileras. Det bör fortfarande utdata ”Hello, World”!

## <a name="get-your-azure-redis-cache-connection-string"></a>Hämta din Azure Redis Cache-anslutningssträng

Om du vill ansluta till din Azure Redis Cache, behöver du en anslutningssträng som innehåller ditt lösenord och URL: en. Den här anslutningssträngen är unik för **ServiceStack.Redis**, och är i form av: `[password]@[host name]:[port]`.

Du kan hämta den här nyckeln med Azure portal eller med kommandoraden. Vi använder det senare här eftersom vi har använt portalen metoden i modulen ”optimera dina webbprogram genom att cachelagra skrivskyddade data med Redis”.

Använd den `azure rediscache list-keys` kommando för att hämta åtkomstnycklarna. Du måste ange namn och resursgrupp gruppen, enligt nedan:

```azurecli
azure rediscache list-keys \
    --name [name] \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

Detta returnerar ungefär så här:

```output
Primary Key   : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
Secondary Key : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
```

1. Kopiera din **primära** nyckel till Urklipp.

1. Värdnamnet är det namn du gav till cachen när du har skapat den med suffixet `.redis.cache.windows.net`.

1. Porten ska vara **6379**.

1. Du kan kontrollera all denna information i konsolen med den `azure rediscache list` kommando. Det här kommandot visar alla instanser av Azure Redis Cache i aktiv prenumeration (i det här fallet Azure Sandbox).

## <a name="add-the-connection-string-to-your-app"></a>Lägger till anslutningssträngen för din app

1. Kontrollera att du är i mappen. Den **Program.cs** bör finnas i den aktuella mappen om du skriver `ls` eller `dir`.

1. Öppna Redigeraren för inbyggda genom att skriva `code .` i mappen.

1. Välj den **Program.cs** källfilen.

1. Skapa följande fält i den `Program` klass.

    ```csharp
    static string redisConnectionString = "";
    ```

1. Klistra in anslutningssträngen i den **redisConnectionString** fält som du just har skapat och använda **6379** som din portnummer. Kom ihåg att anslutningssträngen är i form av: `[password]@[host name]:[port]`.

En exempel-anslutningssträng skulle se ut ungefär så här:

```output
ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6379
```
    
## <a name="insert-two-data-values-into-your-azure-redis-cache"></a>Infoga två datavärden i din Azure Redis Cache

Slutligen ska vi lägga till data i din Azure Redis Cache.

1. Lägg till följande `using` instruktion överst i **Program.cs** fil.

    ```csharp
    using ServiceStack.Redis;
    ```

1. Lägg till följande kodstycke i din **Main** metod. Detta lägger till två värden övergångsvis.

    ```csharp
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create the transaction
        var transaction = redisClient.CreateTransaction();

        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        var transactionResult = transaction.Commit();

        if(transactionResult)
            Console.WriteLine("Transaction committed");
        else
            Console.WriteLine("Transaction failed to commit");
    }
    ```
1. Kör programmet med hjälp av Kommandotolken längst ned i fönstret redigeraren och verifiera att konsolen säger **Transakce byla potvrzena**. 

    ```bash
    dotnet run
    ```
    
## <a name="verify-your-data"></a>Verifiera dina data

Slutför genom att vi ska kontrollera att informationen som vi har lagt till finns i vårt Azure Redis Cache.

1. Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true).

1. Leta upp din Azure Redis Cache genom att välja **alla resurser** i vänster sidofält och använda filterfältet till vänster för att välja Azure Redis Cache-instanser. Du kan också använda sökrutan högst upp och skriver du namnet på cachen.

1. Välj din Azure Redis Cache-instans.

1. I den **översikt** bladet för din Azure Redis Cache, Välj **konsolen**. Då öppnas en Azure Redis Cache-konsol, där du kan ange på den låga nivån Azure Redis Cache-kommandon.

1. Typ **hämta MyKey1**. Kontrollera att värdet som returneras är **MyValue1**.

1. Typ **hämta MyKey2**. Kontrollera att värdet som returneras är **MyValue2**.

    ![Skärmbild av Azure Redis Cache-konsolen som visar värdena för MyKey1 och MyKey2.](../media/4-redis-console.png)