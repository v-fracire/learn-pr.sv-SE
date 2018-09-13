Datahanteringen är en extremt viktig del av affärsverksamheten idag. Relationsdatabaser, och mer specifikt Microsoft SQL Server, har i flera årtionden varit de kanske vanligaste verktygen för datahantering. 

Om man vill lägga datahanteringen i molnet _räcker det_ med att använda virtuella datorer i Azure som värdar för de egna Microsoft SQL Server-instanserna. För många är detta den enda rätta lösningen, men Azure erbjuder även en annan metod som ofta är mycket enklare och mer effektiv. Azure SQL-databaserna är ett PaaS-erbjudande (Platform-as-a-Service), vilket betyder att du slipper sköta infrastrukturen och underhållet själv.

Vi kan försöka göra det hela tydligare med ett exempel. Tänk dig att du är chefsutvecklare på logistikföretaget Contoso Transport.

I transportbranschen är det en hel del som måste samordnas och synkas: administration, orderavdelning, chaufförer och kunder.

Den nuvarande processen innefattar mängder av pappersformulär och många timmar i telefon för att försöka samordna alla leveranser. Ofta upptäcker du att dokumenten saknar signaturer och att det är svårt att få tag på någon på orderavdelningen. Det är den här typen av flaskhalsar som leder till att chaufförerna sitter och rullar tummarna medan viktiga leveranser blir kraftigt försenade. Kundnöjdhet och återkommande kunder är avgörande för lönsamheten i ett företag.

Ledningen beslutar sig därför för att övergå från pappersformulär och telefonsamtal till digitala dokument och onlinekommunikation. Tack vare digitaliseringen kan alla samordna och spåra leveranser via sina webbläsare eller mobilappar.

Nu vill du skapa en snabb prototyp och visa upp den för ditt team. Prototypen innehåller en databas som lagrar information om chaufförer, kunder och beställningar. Prototypen utgör grunden för din produktionsapp. Den teknik du väljer nu bör därför ha relevans för er verksamhet.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att lära dig:

- Varför Azure SQL Database är ett fullgott alternativ till en relationsdatabas
- Vilka konfigurationer och priser som finns för din Azure SQL-databas
- Hur du öppnar en Azure SQL-databas från portalen
- Hur du använder Azure Cloud Shell för att ansluta till din Azure SQL-databas, lägga till en tabell och arbeta med data