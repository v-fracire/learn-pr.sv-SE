Du har bestämt dig för att flytta SQL Server-databaserna till Azure SQL Database eftersom du vill utnyttja skalbarheten och tillgänglighet som fås med Azure. Du vill ändå verifiera att Azure SQL Server har samma funktioner som du för närvarande använder med din lokala SQL Server.

Du använder dig av ett verktyg som heter Data Migration Assistant (DMA) och som hjälper dig att bedöma kompatibiliteten mellan din nuvarande databas och Azure SQL Server.

## <a name="what-is-the-data-migration-assistant-dma"></a>Vad är Data Migration Assistant (DMA)?

Verktyget Data Migration Assistant (DMA) är speciellt utformat för att analysera lokala SQL Server-instanser. Det identifierar och rapporterar vanliga problem som kan hindra migreringen av SQL-databaserna till Azure SQL Server.

Utöver att identifiera problem som kan hindra migrering kan verktyget även identifiera funktioner i dina lokala servrar som inte stöds eller delvis stöds.

Du får också detaljerade rekommendationer över vad som behöver utföras på de lokala servrarna före migreringen.

### <a name="why-do-you-need-data-migration-assistant"></a>Varför behöver du Data Migration Assistant?

Azure SQL Database utvecklas hela tiden och har för närvarande inte stöd för alla funktioner i en SQL Server.

Mer information finns i den uppdaterade [listan över funktioner i Azure SQL Database](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-features).

## <a name="how-to-assess-your-database-using-data-migration-assistant"></a>Hur utvärderas databasen med hjälp av Data Migration Assistant?

Utvärderingen av din databas med hjälp av Data Migration Assistant innehåller vanligtvis följande steg:

- **Installera Data Migration Assistant** – Ladda ned och installera DMA från https://www.microsoft.com/en-us/download/details.aspx?id=53595. Azure SQL Database uppdateras regelbundet, och även DMA uppdateras för att återspegla nya funktioner. Vi rekommenderar att du kör installationsprogrammet för att kontrollera att du har installerat den senaste versionen.
- **Skapa en utvärdering** – Du skapar en ny utvärdering som definierar din lokala databas och måldatabasen. Du kan även utvärdera ett annat mål för SQL Server på Azure Virtual Machines och på det sättet jämföra alternativa migreringar.
- **Välja alternativ för utvärdering och databaskällor** – Du väljer alternativ, till exempel huruvida du vill kontrollera kompatibilitet eller funktionsparitet mellan de två databaserna. Välj sedan din källdatabas. Du kan välja flera källor.
- **Granska resultaten** – Du får detaljerade resultat och kan granska fel och vidta åtgärder. I resultaten visas funktioner som inte stöds med referenser över flera databaser och SQL Server Agent. Du får även en lista över funktioner som delvis stöds, med fulltextsökning och granskning. I resultaten visas möjliga fel och råd för hur du åtgärdar felen. Du kan exportera DMA-resultat som en .json-fil.
