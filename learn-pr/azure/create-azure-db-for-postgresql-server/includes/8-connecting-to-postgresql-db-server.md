Anta att du använder en lokal PostgreSQL-databas. Du hanterar alla säkerhetsaspekter och du har låst all åtkomst till dina servrar med hjälp av standardregler för PostgreSQL-brandväggen på servernivå. Nu har du en god förståelse för hur du konfigurerar samma brandväggsreglerna på servernivå i Azure.

Nu har du möjlighet att ansluta till en tidigare Azure Database for PostgreSQL-servrar som du skapat med `psql`.

## <a name="allow-azure-service-access"></a>Tillåt Azure-tjänståtkomst

Innan vi börjar, måste du tillåta åtkomst till Azure-tjänster om du vill använda PowerShell och `psql` att ansluta till servern. Kom ihåg att du kan tillåta åtkomst på två sätt.

Det första alternativet är att aktivera **Tillåt åtkomst till Azure-tjänster**. Tillåter åtkomst skapas en brandväggsregel även om regeln inte är har angett i listan över anpassade regler som du skapar.

Det andra alternativet är att skapa en brandväggsregel som tillåter åtkomst till alla IP-adresser. Regeln innehåller IP-adressen för den klient som kör PowerShell som du använder för att köra `psql` från.

Du måste också inaktivera den **framtvinga SSL-anslutning** alternativet.

Vi börjar.

Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true). Gå till serverresursen som du vill skapa en brandväggsregel.

Välj den **anslutningssäkerhet** möjligheten att öppna anslutningsbladet till höger.

![Skärmbild av Azure-portalen som visar avsnittet anslutning säkerhet på resursbladet för PostgreSQL-databas](../media-draft/7-db-security-settings.png)

Kom ihåg att du vill tillåta åtkomst till PowerShell-klienter som kör `psql`.

Du kan välja att antingen:

- Ange **Tillåt åtkomst till Azure-tjänster** till **vidare**
- Ange **framtvinga SSL-anslutning** till **inaktiverad**
- Klicka på den **spara** för att spara dina ändringar

Eller du kan lägga till en brandväggsregel som tillåter åtkomst till alla IP-adresser genom att lägga till en brandväggsregel. Ange följande värden:

- Regelnamn: `AllowAll`
- Start-IP: `0.0.0.0`
- Slut-IP: `255.255.255.255`
- Ange **framtvinga SSL-anslutning** till **inaktiverad**
- Klicka på den **spara** för att spara dina ändringar

> [!Warning]
> Skapa den här brandväggsregeln tillåter alla IP-adresser på internet att ansluta till servern. Även om klienter inte kan få åtkomst till servern utan användarnamn och lösenord, aktivera den här regeln med försiktighet och kontrollera att du förstår säkerhetsriskerna. I produktionsmiljöer, kommer du bara tillåter åtkomst till specifika klient-IP-adresser.

I Azure Cloud Shell, ansluter du psql till servern med följande kommando:

```bash
psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
```

Kom ihåg, `server-name`, och `admin-user` är de värden som du valde för administratörskontot när du skapade servern. `postgres` är standarddatabas för hantering av varje PostgreSQL server skapas med. Du uppmanas lösenordet du angav när du skapade servern.

När du har anslutit kör den `\l` kommando för att lista alla databaser. Det här kommandot resulterar i två eller flera standarddatabaser som returneras.

När du är klar kör psql-åtgärder på servern, kör du kommandot `\q` att avsluta `psql`.

Slutligen, om du vill avsluta Cloud Shell kör du kommandot `exit`.
