Du hanterar infrastrukturen företagets databas i SQL Server-datorer som körs i Azure. Tiderna är bra och du behöver skala upp din åtgärd, medan du fortfarande hanterar kostnader. Vissa databasåtgärder innebära många läsningar av befintliga data. Vanliga faktura och rapportering körningar är skrivintensiv åtgärder. Du vill hitta ett sätt att optimera din infrastruktur för att hantera alla åtgärdstyper av. Innan du investerar i förbättringar av infrastrukturen, som du vill utforska virtuell disk cachelagringsalternativ först.

Cachelagring är ett vanligt sätt att förbättra bearbetningsresurser. Azure stöder en mängd cachelagring tekniker för att optimera dataåtkomst över Azure liggande, inklusive specifika cachealternativ för Azure storage och diskar som används av virtuella Azure-datorer (VM).

Vi kommer att utforska alla diskcachelagringstypen alternativ i Azure och hantera diskcachelagringstypen med portalen och PowerShell.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Beskriv de viktiga övervägandena kring diskprestanda i Azure (IOPS)
- Beskriv effekterna av cachelagring på diskprestanda i Azure
- Aktivera och hantera inställningar för cachelagring med Azure portal
- Aktivera och hantera inställningar för cachelagring med PowerShell