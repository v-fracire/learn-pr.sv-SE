I den här enheten skapar du en Azure Redis-cache för att lagra och returnera värden som används ofta.

## <a name="create-a-cache"></a>Skapa en cache

1. Logga in på Azure-portalen. Klicka sedan på **Skapa en resurs**, klicka på **Databaser** och klicka på **Redis Cache**.

    Följande skärmbild visar Redis Cache-platsen i de olika alternativen för databasresurser på Azure-portalen.

    ![Skärmbild som visar Azure-portalens databasalternativ, med Skapa en resurs, Databas och Redis Cache-alternativet markerade.](../media/4-create-a-cache-1.png)

## <a name="configure-your-cache"></a>Konfigurera din cache

1. **DNS-namn:** Skapa ett globalt unikt namn, till exempel **ContosoSportsApp1028**.

1. **Prenumeration:** Välj din prenumeration.

1. **Resursgrupp**: Skapa en ny resursgrupp och ge den ett namn.

1. **Plats:** Eftersom de flesta av dina kunder finns i New York du bör välja **USA, östra**.

1. **Prisnivå:** Välj **Basic C0**.

1. Klicka på **Skapa**.

    Följande skärmbild visar en typisk konfiguration som används för att skapa en ny Redis Cache-resurs.

    ![Skärmbild som visar bladet för Azure-portalen när du skapar en ny Redis Cache-resurs, med exempel på DNS-namn, prenumeration, ny resursgrupp, plats och prisnivå.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> Du måste vänta tills cachen distribueras innan du fortsätter. Den här processen kan ta lite tid.

## <a name="retrieve-the-access-keys-and-host-name"></a>Hämta åtkomstnycklarna och värdnamnet

1. I Azure-portalen bläddrar du till din cache och väljer **Inställningar** > **Åtkomstnycklar**. Anteckna **Primär anslutningssträng (StackExchange.Redis)**.

    Den här nyckeln innehåller din primära nyckel och värdnamnet i en fullständig anslutningssträng, för användning i programinställningarna för StackExchange.Redis-paketet som vi använder.

1. Starta Anteckningar eller något annat textredigeringsprogram på datorn.

1. Lägg till följande innehåll:

    Ersätt `<connection-string>` med cachens primära anslutningssträng från ovan.

    ```xml
    <appSettings>
        <add key="CacheConnection" value="<connection-string>"/>
    </appSettings>
    ```

1. Spara filen på en plats där den inte inkluderas i källkoden till exempelprogrammet. I den här självstudiekursen sparar vi filen som `C:\AppSecrets\CacheSecrets.config`.

## <a name="create-a-console-app"></a>Skapa en konsolapp

1. Starta Visual Studio och klicka på menykommandot **Arkiv** > **Nytt** > **Projekt...** .

1. Välj mallen **Visual C#** > **Windows Desktop** > **Konsolprogram**.

1. Ange ett namn och klicka på **OK** för att skapa ett nytt konsolprogram.

## <a name="configure-the-cache-client"></a>Konfigurera cacheklienten

I det här avsnittet konfigurerar du konsolprogrammet att använda **StackExchange.Redis**-klienten för .NET.

1. I Visual Studio klickar du på **Verktyg** pekar på **NuGet Package Manager**, klickar på **Package Manager Console** och kör följande kommando från Package Manager Console-fönstret:

    ```powershell
    Install-Package StackExchange.Redis
    ```

När installationen är klar är StackExchange.Redis-klienten redo för användning med ditt projekt.

## <a name="connect-to-the-cache"></a>Ansluta till cachen

1. I Visual Studio öppnar du filen **App.config** i **Solution Explorer** och uppdaterar den så att den inkluderar ett **appSettings**-filattribut som refererar till filen **CacheSecrets.config**.

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.1" />
        </startup>

        <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>

    </configuration>
    ```

1. I Solution Explorer högerklickar du på **Referenser** och klickar på **Lägg till referens...**. Välj **System.Configuration** från listan **Sammansättningar** > **Ramverk** och klicka på **OK**.

1. Lägg till följande `using`-instruktioner i **Program.cs**:

    ```csharp
    using StackExchange.Redis;
    using System.Configuration;
    ```

Anslutningen till Azure Redis Cache hanteras av klassen ConnectionMultiplexer. Den här klassen ska delas och återanvändas i hela ditt klientprogram. Skapa inte en ny anslutning för varje åtgärd.

Lagra aldrig autentiseringsuppgifterna i källkoden. För att hålla det här exemplet enkelt använder vi endast en config-fil för externa hemligheter. En bättre metod är att använda Azure Key Vault med certifikat.

1. I **Program.cs**, lägger du till följande medlemmar i klassen Program för ditt konsolprogram:

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

Den här metoden för att dela en ConnectionMultiplexer-instans i ditt program använder en statisk egenskap som returnerar en ansluten instans. Koden ger ett trådsäkert sätt att endast initiera en enda ansluten ConnectionMultiplexer-instans. **abortConnect** är inställt på falskt, vilket innebär att anropet lyckas även om en anslutning till Azure Redis Cache inte etableras. En viktig egenskap i ConnectionMultiplexer är att anslutningen till cachen återställs automatiskt när nätverksproblemet eller andra fel har åtgärdats.

Värdet för appSetting **CacheConnection** används för att referera till cache-anslutningssträngen från Azure-portalen som lösenordsparameter.
