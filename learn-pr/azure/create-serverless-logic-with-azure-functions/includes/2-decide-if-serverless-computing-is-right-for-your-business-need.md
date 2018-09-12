För att avgöra om serverlös databehandling passar för dig ska vi först lära oss vad serverlös handlar om.

## <a name="what-is-serverless-compute"></a>Vad är serverlös beräkning?

Serverlös beräkning kan betraktas som en funktion som en tjänst (FaaS) eller en mikrotjänst som finns på en molnplattform. Din affärslogik körs som funktioner och du behöver inte etablera eller skala infrastruktur manuellt. Molnleverantören hanterar infrastrukturen. Din app skalas automatiskt ut eller ned beroende på belastningen. Azure har flera sätt att skapa den här typen av arkitektur. De två vanligaste metoderna är Azure Logic Apps och Azure Functions som vi fokuserar på i den här modulen.

## <a name="what-is-azure-functions"></a>Vad är Azure Functions?

Azure Functions är en serverlös programplattform. Den gör det möjligt för utvecklare att lagra kod för affärslogik som kan köras utan att etablera infrastruktur. Functions har inbyggda funktioner för skalbarhet och du debiteras bara för de resurser som används. Du kan skriva funktionskoden på det språk du föredrar, bland annat C#, F# och JavaScript. Stöd för NuGet och NPM ingår också, så du kan använda populära bibliotek i din affärslogik.

## <a name="benefits-of-a-serverless-compute-solution"></a>Fördelar med en serverlös beräkningslösning

Serverlös beräkning är ett bra alternativ för att lagra kod för affärslogik i molnet. Med serverlösa erbjudanden, till exempel Azure Functions, kan du skriva affärslogiken på det språk du föredrar. Du får automatisk skalning, du har inga servrar att hantera och du debiteras baserat på vad som används – inte på reserverad tid. Här följer några ytterligare kännetecken för en serverlös lösning att ta hänsyn till.

### <a name="avoids-over-allocation-of-infrastructure"></a>Undviker överallokering av infrastruktur

Anta att du har etablerat VM-servrar och konfigurerat dem med tillräckliga resurser för att hantera dina högsta belastningstider. När belastningen är lätt betalar du eventuellt för infrastruktur som du inte använder. Serverlös databehandling löser allokeringsproblem genom att skala upp eller ned automatiskt och du debiteras bara när funktionen bearbetar arbete.

### <a name="stateless-logic"></a>Tillståndslös logik

Tillståndslösa funktioner är bra kandidater för serverlös beräkning. funktionsinstanser skapas och förstörs på begäran. Om ett tillstånd krävs kan det lagras i en associerad lagringstjänst.

### <a name="event-driven"></a>Händelsebaserad

Funktioner är _händelsebaserade_. Det innebär att de endast körs som svar på en händelse (kallas en ”utlösare”), till exempel för att ta emot en HTTP-begäran eller när ett meddelande läggs till i en kö. Du konfigurerar en utlösare som en del av funktionsdefinitionen. Den här metoden förenklar din kod genom att låta dig deklarera var data kommer från (utlösare/indatabindning) och var de ska (utdatabindning). Du behöver inte skriva kod för att se köer, blobar, hubbar osv. Du kan fokusera enbart på affärslogiken.

### <a name="functions-can-be-used-in-traditional-compute-environments"></a>Funktioner kan användas i traditionella beräkningsmiljöer

Funktioner är en viktig del av serverlös databehandling, men de är också en allmän beräkningsplattform för att köra alla typer av kod. Om behoven för din app ändras kan du ta projektet och distribuera det i en icke-serverlös miljö som ger dig flexibiliteten att hantera skalning, köra i virtuella nätverk och även helt isolera dina funktioner.

## <a name="drawbacks-of-a-serverless-compute-solution"></a>Nackdelar med en serverlös beräkningslösning

Serverlös beräkning är inte alltid rätt lösning för att lagra din affärslogik. Här följer några kännetecken som funktioner har som kan påverka ditt beslut att lagra dina tjänster i beräkning utan server. 

### <a name="execution-time"></a>Körningstid

Funktioner har som standard en tidsgräns på 5 minuter. Denna tidsgräns kan konfigureras till högst 10 minuter. Om din funktion kräver mer än 10 minuter för att köras kan du lägga upp den på en virtuell dator. Om tjänsten initieras via en HTTP-begäran och du förväntar dig att det värdet ska vara ett HTTP-svar begränsas tidsgränsen ytterligare, till 2,5 minuter. Slutligen finns också alternativet **Durable Functions** som gör det möjligt att samordna körningar av flera funktioner utan någon tidsgräns.

### <a name="execution-frequency"></a>Körningsfrekvens

Det andra kännetecknet är körningsfrekvens. Om du räknar med att funktionen ska köras kontinuerligt av flera klienter är det klokt att uppskatta förbrukningen och beräkna kostnaden för att använda funktioner. Det kan vara billigare att lagra tjänsten på en virtuell dator.

När du skalar kan endast en instans av en funktionsapp skapas var tionde sekund, för upp till totalt 200 instanser. Tänk på att varje instans kan hantera flera körningar samtidigt, så det finns ingen tydlig begränsning för hur mycket trafik en instans kan hantera. Olika typer av utlösare har olika skalningskrav, så du bör undersöka ditt val av utlösare och dess begränsningar.
