Du hanterar infrastrukturen i företagets databas för virtuella SQL Server-datorer som körs i Azure. Det går bra och du behöver skala upp verksamheten och samtidigt hantera kostnader. Vissa databasåtgärder innebär många läsningar av befintliga data. Vanliga körningar för fakturering och rapportering är skrivintensiva åtgärder. Du vill hitta ett sätt att optimera din infrastruktur för att hantera alla åtgärdstyper. Innan du investerar i förbättringar av infrastrukturen beslutar du dig för att utforska alternativen för VM-diskcachelagring.

Cachelagring är en vanlig metod för att påskynda beräkningsresurser. Azure stöder en mängd tekniker för cachelagring som hjälper till att optimera dataåtkomst i Azure-lands, inklusive specifika cachelagringsalternativ för Azure-lagring och -diskar som används av virtuella Azure-datorer.

Vi ska titta på alla alternativ för diskcachelagring i Azure och hantera diskcachelagring med portalen och PowerShell.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att:

- Beskriv de viktiga övervägandena kring diskprestanda i Azure (IOPS)
- Beskriv hur cachelagring påverkar diskprestanda i Azure
- Aktivera och hantera inställningar för cachelagring med Azure Portal
- Hantera cacheinställningar med PowerShell