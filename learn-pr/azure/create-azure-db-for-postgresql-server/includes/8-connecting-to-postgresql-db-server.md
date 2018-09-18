Anta att du använder en lokal PostgreSQL-databas. Du hanterar alla säkerhetsaspekter och låser all åtkomst till dina servrar med brandväggsregler på PostgreSQL-standardservernivå. Nu har du goda kunskaper om hur du konfigurerar brandväggsregler på samma servernivå i Azure.

Nu har du chansen att ansluta till någon av de tidigare Azure-databaserna för PostgreSQL-servrar du har skapat med `psql`.

## <a name="allow-azure-service-access"></a>Tillåta åtkomst till Azure-tjänst

Innan vi börjar. Du måste tillåta åtkomst till Azure-tjänster om du vill använda PowerShell och `psql` för att ansluta till din server. Kom ihåg att du kan tillåta åtkomst på två sätt.

Ditt första alternativ är att aktivera **Allow access to Azure services** (Tillåt åtkomst till Azure-tjänster). Om du tillåter åtkomst skapas en brandväggsregel även om regeln inte finns med på listan över anpassade regler som du skapar.

Ditt andra alternativ är att skapa en brandväggsregel som tillåter åtkomst till alla IP-adresser. Regeln innehåller IP-adressen för den klient som kör PowerShell som du använder för att köra `psql` från.

Du måste också inaktivera **Framtvinga SSL-anslutning**.

Nu sätter vi igång.

Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true). Gå till serverresursen du vill skapa en brandväggsregel för.

Välj alternativet **Anslutningssäkerhet** för att öppna bladet Anslutningssäkerhet till höger.

![Säkerhetsinställningar för PostgreSQL-databas](../media-draft/7-db-security-settings.png)

Kom ihåg att du inte vill tillåta åtkomst till PowerShell-klienter som kör `psql`.

Du kan välja att antingen:

- Ställ in **Allow access to Azure services** (Tillåt åtkomst till Azure-tjänster) på **PÅ**
- Ställ in **Framtvinga SSL-anslutning** på **INAKTIVERAD**
- Klicka på knappen **Spara** för att spara ändringarna

Eller så kan du lägga till en brandväggsregel för att tillåta åtkomst till alla IP-adresser genom att lägga till en brandväggsregel. Ange följande värden:

- Regelnamn: `AllowAll`
- Start-IP: `0.0.0.0`
- Slut-IP: `255.255.255.255`
- Ställ in **Framtvinga SSL-anslutning** på **INAKTIVERAD**
- Klicka på knappen **Spara** för att spara ändringarna

> [!Warning]
> Om du skapar den här brandväggsregeln får alla IP-adresser på Internet tillåtelse att försöka ansluta till din server. Trots att klienter inte får åtkomst till servern utan användarnamnet och lösenordet ska du aktivera den här regeln med försiktighet och kontrollera att du förstår säkerhetsriskerna. I produktionsmiljöer tillåter du endast åtkomst till specifika IP-adresser.

För nästa steg startar du en Azure Cloud Shell-session. I den här labbuppgiften används `bash` som kommandoradsmiljö.

- Öppna Open Shell från Azure-portalen. Gå till [Azure-portalen ](https://portal.azure.com?azure-portal=true) och klicka på Open Cloud Shell-knappen:

  ![Cloud Shell-knapp](../media-draft/cloud-shell-button.png)

- Om du har flera prenumerationer kontrollerar du att du använder rätt prenumeration när du blir ombedd. Du kan även köra följande kommando för att ange den aktiva prenumerationen. Kom ihåg att ersätta nollorna med ditt prenumerations-ID.

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

- Anslut psql till din server med följande kommando:

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
  ```

   Kom ihåg att `server-name` och `admin-user` är värdena du valde för administratörskontot när du skapade servern. `postgres` är standarddatabasen för hantering som varje PostgreSQL-server skapas med. Du blir uppmanad att ange lösenordet som du angav när du skapade servern.

- När du är ansluten kör du kommandot `\l` för att lista alla databaser.

   Det här kommandot leder till två eller fler standarddatabaser som returneras.

- När du är klar med att köra psql-åtgärder på servern kör du kommandot `\q` för att avsluta `psql`.

- För att till slut avsluta Cloud Shell kör du kommandot `exit`.
