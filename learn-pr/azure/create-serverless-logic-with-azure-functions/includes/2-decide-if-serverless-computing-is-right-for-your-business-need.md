Innan du skapar din temperaturdatatjänst bör du kontrollera om serverlös beräkning är den bästa lösningen för jobbet. 

## <a name="what-is-serverless-compute"></a>Vad är serverlös beräkning?
Serverlös beräkning kan betraktas som en funktion som en tjänst (FaaS) eller en mikrotjänst som finns på en molnplattform. Den här funktionen innehåller kod för affärslogik som kan köras utan manuell etablering eller skalningsinfrastruktur. Värdplattformen är abstraherad. Det finns inga servrar att komma åt direkt och ingen skalning att beräkna. 

## <a name="what-is-azure-functions"></a>Vad är Azure Functions?
Azure Functions är en serverlös programplattform. Den gör det möjligt för utvecklare att lagra kod för affärslogik som kan köras utan att etablera infrastruktur. Azure Functions tillhandahåller inbyggd skalbarhet och fakturerar endast för de resurser som används. Azure Functions kan kodas på flera olika språk, inklusive C#, F# och JavaScript. Det har stöd för NuGet och NPM så att du kan inkludera många populära bibliotek. 

## <a name="when-do-you-use-serverless-compute"></a>När använder du serverlös beräkning?
Serverlös beräkning är ett bra alternativ för att lagra kod för affärslogik i molnet. Det har automatisk skalning, inga servrar att hantera och snabb fakturering. Det tillhandahåller även ett urval av flera olika programmeringsspråk för implementering. Här följer några ytterligare egenskaper som också är typiskt för serverlös beräkning.

### <a name="avoid-overallocation-of-infrastructure"></a>Undvika överbeläggning av infrastruktur
Anta att du har etablerat VM-servrar och konfigurerat dem med tillräckliga resurser för att hantera dina högsta inläsningstider. När belastningen är lätt kan du eventuellt behöva betala för infrastruktur som du inte använder. Serverlös beräkning löser överbeläggningsproblem genom att skala upp eller ned automatiskt och du faktureras bara när funktionen körs.

### <a name="stateless-logic"></a>Tillståndslös logik
Tillståndslösa funktioner är bra kandidater för serverlös beräkning. funktionsinstanser skapas och förstörs på begäran. Om ett tillstånd krävs kan det lagras i en associerad lagringstjänst.

### <a name="event-driven"></a>Händelsebaserad
Azure-funktioner är händelsebaserade. De körs endast som svar på en händelse, till exempel för att ta emot en HTTP-begäran eller när ett meddelande läggs till i en kö. Konfiguration av Azure-funktioner förenklar din kodbas genom att låta dig deklarera var data kommer från (utlösare/indatabindning) och var den ska (utdatabindning). Du behöver inte skriva kod för att se köer, blobar, hubbar osv. Du behöver bara fokusera på affärslogiken.

## <a name="when-do-you-not-use-serverless-compute"></a>När bör du inte använda serverlös beräkning?
Serverlös beräkning är inte alltid rätt lösning för att lagra din affärslogik. Här följer några kännetecken som Azure-funktioner har som kan påverka ditt beslut att lagra dina tjänster i beräkning utan server. 

### <a name="execution-time"></a>Körningstid
Azure-funktioner har som standard en tidsgräns på 5 minuter. Detta kan konfigureras till högst 10 minuter. Om din funktion kräver mer än 10 minuter för att köras kan du lägga upp den på en virtuell dator. Om tjänsten initieras via en HTTP-begäran och du förväntar dig att det värdet ska vara ett HTTP-svar kan tidsgränsen begränsas ytterligare, till 2,5 minuter.

### <a name="execution-frequency"></a>Körningsfrekvens
Det andra kännetecknet är körningsfrekvens. Om du räknar med att funktionen ska köras kontinuerligt av flera klienter är det klokt att uppskatta förbrukningen och beräkna kostnaden för att använda Azure-funktioner. Det är möjligt att det är billigare lagra tjänsten på en virtuell dator.

När du skalar kan endast en instans av en funktionsapp skapas var tionde sekund, för upp till totalt 200 instanser. Tänk på att varje instans kan hantera flera körningar samtidigt, så det finns ingen tydlig begränsning för hur mycket trafik en instans kan hantera. Olika typer av utlösare har olika skalningskrav, så du bör undersöka ditt val av utlösare och dess begränsningar.
