Ditt logistikföretag har bestämt sig för att man ska stå ut från mängden, men naturligtvis utan att spräcka budgeten. Nu gäller för dig att du kan konfigurera databasen och tillhandahålla bästa möjliga prestanda utan att kostnaderna skenar.

[!include[](../../../includes/azure-sandbox-activate.md)]

Detta får du får lära dig:

- Vilka överväganden som du behöver göra när du skapar en Azure SQL Database, inklusive:
  - Hur en logisk server kan fungera som en administrativ container för dina databaser.
  - Skillnader mellan prismodeller.
  - Hur elastiska pooler gör det möjligt att dela bearbetningskapaciteten mellan olika databaser.
  - Hur sorteringsreglerna påverkar hur data jämförs och sorteras.
- Hur du öppnar Azure SQL Database från portalen.
- Hur du lägger till brandväggsregler så att databasen är tillgänglig från endast betrodda källor.

Låt oss ta en snabb titt på några saker som du behöver tänka på när du skapar en Azure SQL-databas.

## <a name="one-server-many-databases"></a>En server, många databaser

När du skapar din första Azure SQL-databas skapas även en _logisk Azure SQL-server_. Tänk dig den logiska servern som en administrativ container för dina databaser. Med den logiska servern kan du styra över saker som inloggningar, brandväggsregler och säkerhetsprinciper. Du kan också åsidosätta dessa principer för varje enskild databas på den logiska servern.

För tillfället behöver du bara en enda databas. Men eftersom du har en logisk server kan du lägga till fler databaser senare och finjustera prestanda hos alla dina databaser.

## <a name="choose-performance-dtus-versus-vcores"></a>Välj prestanda: DTU eller Virtuell kärna

Azure SQL Database har två prismodeller: DTU och Virtuell kärna.

### <a name="what-are-dtus"></a>Vad är DTU:er?

DTU står för Database Transaction Unit (databastransaktionsenhet) och är ett kombinerat mått på compute, lagring och IO-resurser. Tänk dig DTU-modellen som ett enkelt och förkonfigurerat inköpsalternativ.

Eftersom din logiska server kan innehålla mer än en databas finns också varianten eDTU, alltså elastiska databastransaktionsenheter. Det här alternativet gör det möjligt att välja en prisnivå, men varje databas i poolen kan ändå använda färre eller fler resurser beroende på aktuell belastning.

### <a name="what-are-vcores"></a>Vad är en virtuell kärna?

Med virtuell kärna får du mer kontroll över vilka beräknings- och lagringsresurser som du skapar och betalar för.

Medan DTU-modellen har fasta kombinationer för beräkning, lagring och I/O-resurser kan du med Virtuell kärna konfigurera resurser oberoende av varandra. Med Virtuell kärna kan du till exempel öka lagringskapaciteten men behålla den befintliga mängden beräkningar och I/O-operationer.

Din transport och logistik-prototyp behöver bara en Azure SQL Database-instans. Du bestämmer dig för DTU-alternativet eftersom det ger en bra balans mellan compute, lagring och I/O-prestanda. Dessutom blir det lättare att komma igång till en låg inledande kostnad.

## <a name="what-are-sql-elastic-pools"></a>Vad är elastiska SQL-pooler?

När du skapar din Azure SQL-databas kan du även skapa en _elastisk SQL-pool_.

Elastiska SQL-pooler är nära besläktade med eDTU:er. Du köper en viss mängd beräknings- och lagringsresurser som delas mellan alla databaser i poolen. Varje databas kan använda de resurser som den behöver, inom de gränser du ställer in och beroende på aktuell belastning.

För prototypen behöver du inte någon elastisk SQL-pool eftersom du bara behöver en enda SQL-databas.

## <a name="what-is-collation"></a>Vad är sortering?

Sortering syftar här på regler som sorterar och jämför data. Definiera sorteringsregler för fall där skiftlägeskänslighet, accenter och andra särdrag har stor betydelse.

Låt oss titta närmare på standardsorteringen **SQL_Latin1_General_CP1_CI_AS** en stund.

- **Latin1_General** står för västeuropeiska språk.
- **CP1** står för teckentabellen 1252, en populär teckenkodning för det latinska alfabetet.
- **CI** betyder att ingen hänsyn tas till skiftläget. ”HEJ” är alltså jämställt med ”hej”.
- **AS** betyder att hänsyn tas till användningen av accenter. Som exempel kan nämnas att ”résumé” inte är samma sak som ”resume”.

Eftersom du inte har några särskilda krav på hur data sorteras och jämförs kan du välja standardsortering.

## <a name="create-your-azure-sql-database"></a>Skapa din Azure SQL-databas

Här får du konfigurera en egen databas, vilket även innefattar att skapa den logiska servern. Du väljer inställningar som stöder en logistikapp. I praktiken kan man välja inställningar som stöder typ av app som du håller på att skapa.

Om du över tid inser att du behöver ytterligare beräkningskraft för att hålla jämna steg med efterfrågan kan du justera prestandaalternativen eller växla mellan prestandamodellerna DTU och vCore.

1. Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. Klicka på **Skapa en resurs** längst upp till vänster i Azure-portalen. Välj **Databaser** och välj sedan **SQL Database**.

   ![Skärmbild av Azure-portalen som visar bladet Skapa en resurs där avsnittet Databaser har valts och knapparna Skapa en resurs, Databaser och SQL Database har markerats.](../media/3-create-db.png)

1. Gå till **Server**, klicka på **Konfigurera inställningarna**, fyll i formuläret och klicka sedan på **Välj**. Här får du lite hjälp med att fylla i formuläret:

    | Inställning      | Värde |
    | ------------ | ----- |
    | **Servernamn** | Ett unikt [servernamn](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) globalt. |
    | **Inloggning för serveradministratör** | Ett [databas-ID](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) som fungerar som din primära administratörsinloggning. |
    | **Lösenord** | Ett giltigt lösenord som innehåller minst åtta tecken och innehåller tecken från tre av följande kategorier: versaler, gemener, siffror och icke-alfanumeriska tecken. |
    | **Plats** | Valfri giltig plats i listan nedan. |

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. Klicka på **Prisnivå** att välja tjänstnivå. Välj prisnivån **Basic** och klicka sedan på **Använd**.

1. Använd dessa värden för att fylla i resten av formuläret.

    | Inställning      | Värde |
    | ------------ | ----- |
    | **Databasnamn** | **Logistik** |
    | **Prenumeration** | Din prenumeration |
    | **Resursgrupp** |  Använd den befintliga gruppen <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn> |
    | **Välj källa** | **Tom databas** |
    | **Vill du använda elastisk SQL-pool?** | **Inte nu** |
    | **Sortering** | **SQL_Latin1_General_CP1_CI_AS** |

1. Klicka på **Skapa** för att skapa din Azure SQL-databas.

    > [!IMPORTANT]
    > Kom ihåg ditt servernamn, administratörsinloggning och lösenord för senare bruk.

1. Klicka på **Aviseringar** i verktygsfältet för att övervaka distributionsprocessen.

När processen slutförs, klickar du på **Fäst på instrumentpanelen** för att fästa din databasserver på instrumentpanelen så du får snabb åtkomst till den när du behöver den.

   ![Skärmbild av Azure Portal som visar menyn Meddelanden med knappen Fäst på instrumentpanelen från ett tidigare meddelande om lyckad distribution markerad.](../media/3-notifications-complete.png)

## <a name="set-the-server-firewall"></a>Konfigurera serverns brandvägg

Din Azure SQL-databas är nu aktiverad och igång. Du har många alternativ för att ytterligare konfigurera, skydda, övervaka och felsöka din nya databas.

Du kan också ange vilka system som ska få åtkomst till databasen via brandväggen. Brandväggen förhindrar först all åtkomst till databasservern från system utanför Azure.

För din prototyp behöver du bara åtkomst till databasen från din bärbara dator. Senare kan du godkänna ytterligare system, till exempel din mobilapp.

Nu ska vi aktivera utvecklingsdatorn för att få åtkomst till databasen via brandväggen.

1. Gå till översiktsbladet i logistikdatabasen. Om du har fäst databasen sedan tidigare kan du klicka på panelen **Logistik** på instrumentpanelen för att komma dit.

1. Klicka på **Konfigurera serverns brandvägg**.

    ![Skärmbild av Azure Portal som visar bladet för en översikt över en SQL-databas med knappen Konfigurera serverns brandvägg markerad.](../media/3-set-server-firewall.png)

1. Klicka på **Lägg till klient-IP** och klicka därefter på **Spara**.

    ![Skärmbild av Azure Portal som visar bladet en SQL-databas brandväggsinställningar med knappen Lägg till klient-IP markerad.](../media/3-add-client-ip.png)

I nästa del får du prova på några praktiska övningar med den nya databasen och Azure Cloud Shell. Du kommer att få ansluta till databasen, skapa en tabell, lägga till exempeldata och köra några SQL-uttryck.