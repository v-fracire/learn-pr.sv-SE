Vi började genom att tala om hur du kan kontrollera att du har kontroll över din förbrukning, samt hur du optimerar den. Här följer några av de viktigaste aspekterna som beskrevs:

- Spåra molnutgifter: Vikten av att samla in och granska molnets förbrukningsdata för att förstå molnutgifterna.
- Ordna för att optimera: Relevansen för en prenumeration och resursgruppens hierarki, samt hur taggar kan ge fler dimensioner.
- Det finns flera alternativ för att optimera kostnaderna för IaaS: Rätt storlek, minska antalet körningstimmar och beräkna kostnadsrabatter.
- Några exempel visades på hur man optimerar kostnaderna för PaaS: Att använda elastiska pooler i Azure SQL Database för att optimera kostnaderna för lagringsnivåerna i Azure SQL eller Azure Blob Storage och optimera Azure Storage-kostnaderna.

Därefter gick vi igenom den åtgärdsinformation som du kan få genom att förstå och implementera övervakning och analys. Vi såg att det finns olika nivåer där vi kan tillämpa övervakning.

- Övervakning innebär att samla in och analysera data för att avgöra prestanda, hälsotillstånd och tillgänglighet för de företagsprogram och resurser som man är beroende av.
- Det finns olika djup för övervakningen. Kärnövervakningen omfattar aspekter nära Azure-plattformen, till exempel aktivitetsloggning, molntjänsthälsa samt mått och diagnostik.
- Med övervakning av den djupa infrastrukturen kan du samla in information som är utöver vad Azure-infrastrukturen kan ge. Vi kan övervaka på nätverks- eller operativsystemnivå för vanliga IaaS-arbetsbelastningar.
- Djupgående programövervakning tar det ännu längre genom att samla in information om mått och diagnostik nära programmet. Detta innebär att du kan identifiera prestandaproblem, användningstrender och information om övergripande tillgänglighet.

Vi avslutade genom att visa att det finns olika sätt att automatisera uppgifter på när du vill förbättra dina operativa funktioner. Här följer några av de viktiga saker som vi har gått igenom:

- Att automatisera distributionen av resurser kan göras på två olika sätt: imperativt (med skript) eller deklarativt (med mallar).
- När den virtuella datorn har distribuerats ska vi se hur du anpassar den. Antingen använder vi anpassade avbildningar och slipper därmed anpassningen. Eller så kör vi ett skript efter distributionen.
- Automatisering av uppgifter i Azure: Med hjälp av Azure Automation kan du installera uppdateringar eller stänga av virtuella datorer enligt ett schema.
- Labbmiljöer för automatisering: Azure DevTest Labs tar automatisering ett steg längre genom att tillhandahålla ett lättanvänt gränssnitt där du kan använda olika automatiseringsfunktioner, som t.ex. avbildningar eller avstängning av virtuella datorer, utan att behöva bekymra dig om den faktiska implementeringen av dessa åtgärder.
