Du har bestämt dig för att flytta SQL Server-databaserna till Azure SQL Database eftersom du vill utnyttja skalbarheten och tillgängligheten som fås med Azure. Du vill ändå verifiera att Azure SQL Server har samma funktioner som du för närvarande använder med din lokala SQL Server.

Du använder dig av ett verktyg som heter Data Migration Assistant och som hjälper dig att bedöma kompatibiliteten mellan din nuvarande databas och Azure SQL Server.

## <a name="what-is-the-data-migration-assistant-dma"></a>Vad är Data Migration Assistant (DMA)?

Verktyget Data Migration Assistant är speciellt utformat för att analysera lokala SQL Server-instanser. Det identifierar och rapporterar vanliga problem som kan hindra migreringen av SQL-databaserna till Azure SQL Server.

Utöver att identifiera problem som kan hindra migrering kan verktyget också identifiera funktioner i dina lokala servrar som delvis stöds eller inte stöds alls.

Du får också detaljerade rekommendationer över vad som behöver utföras på de lokala servrarna före migreringen.

### <a name="why-do-you-need-data-migration-assistant"></a>Varför behöver du Data Migration Assistant?

Azure SQL Database utvecklas hela tiden och har för närvarande inte stöd för alla funktioner i en SQL Server.

Mer information finns i den uppdaterade [listan över funktioner i Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-features).

## <a name="how-to-assess-your-database-using-data-migration-assistant"></a>Hur utvärderar du databasen med hjälp av Data Migration Assistant?

Du utvärderar vanligtvis databasen med hjälp av Data Migration Assistant genom att utföra följande steg:

- **Installera Data Migration Assistant** – Ladda ned och installera Data Migration Assistant från https://www.microsoft.com/download/details.aspx?id=53595. Azure SQL Database uppdateras regelbundet, och även Data Migration Assistant uppdateras för att återspegla nya funktioner. Vi rekommenderar att du kör installationsprogrammet för att kontrollera att du har installerat den senaste versionen.
- **Skapa en utvärdering** – Du skapar en ny utvärdering som definierar din lokala databas och måldatabasen. Du kan också utvärdera ett annat mål för SQL Server på Azure Virtual Machines och på det sättet jämföra alternativa migreringar.
- **Välja alternativ för utvärdering och databaskällor** – Du väljer dina alternativ, till exempel om du vill kontrollera kompatibilitet eller funktionsparitet mellan de två databaserna. Välj sedan källdatabas. Du kan välja flera källor.
- **Granska resultaten** – Du får detaljerade resultat, kan granska fel och vidta åtgärder. I resultaten visas funktioner som inte stöds med referenser över flera databaser och SQL Server Agent. Du får också en lista över funktioner som delvis stöds, med fulltextsökning och granskning. I resultaten visas möjliga fel och råd för hur du åtgärder felen. Du kan exportera resultaten från Data Migration Assistant som en .json-fil.
