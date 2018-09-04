I den här övningen letar du upp och kopierar anslutningssträngen. Sedan lägger du till den i det tillhandahållna startkodsprojektet.

## <a name="get-your-connection-string"></a>Hämta anslutningssträngen

Din anslutningssträng är tillgänglig i avsnittet Inställningar på ditt lagringskonto.

1. I en webbläsare navigerar du till [Azure-portalen](https://portal.azure.com?azure-portal=true) och loggar in på ditt konto.

1. På startsidan klickar du på **Alla resurser**.

1. Välj det lagringskonto som du skapade tidigare.

1. Leta upp avsnittet **Inställningar** och väljer **Åtkomstnycklar**.

1. Under **key1** och till höger om textrutan **Anslutningssträng** klickar du på knappen **Klicka för att kopiera**.

1. Spara anslutningssträngen för användning senare i den här övningen.

## <a name="clone-the-starter-application"></a>Stäng startprogrammet

Du kommer att slutföra två konsolprogram i Visual Studio Code. Det första placerar meddelanden i en kö och det andra hämtar dem. Programmen är en del av en enda .NET Core-lösning som har tillhandahållits. Börja genom att klona lösningen:

1. Starta en kommandotolk och ändra till den katalog där du vill hantera källkoden för programmet.

1. Ange följande kommando och tryck på **Enter**:

```
git clone https:\\ TODO (add git URL)
```

## <a name="add-your-connection-string-to-the-project"></a>Lägga till anslutningssträngen i projektet

Slutligen lägger du till anslutningssträngen till både sändningsappen och mottagningsappen.

> [!NOTE]
> För enkelhetens skull placerar du anslutningssträngen i filen **Program.cs** för båda konsolprogrammen. I ett produktionsprogram ska du lagra den på en säker plats.

1. Öppna lösningen i **Visual Studio Code**.

1. I **Explorer**-fönsterrutan i mappen **sendarticle** klickar du på filen **Program.cs**.

1. Leta upp följande kodrad:

    ```C#
    static string connectionString = "";
    ```

1. Kopiera anslutningssträngen till variabeln `connectionString`.

1. I **Explorer**-fönsterrutan i mappen **receivearticle** klickar du på filen **Program.cs**.

1. Leta upp följande kodrad:

    ```C#
    static string connectionString = "";
    ```

1. Kopiera anslutningssträngen till variabeln `connectionString`.

1. Klicka på **Arkiv** och sedan på **Spara alla**.

## <a name="summary"></a>Sammanfattning

Den kod som konfigurerar en anslutningssträng för ett lagringskonto är nu slutförd.