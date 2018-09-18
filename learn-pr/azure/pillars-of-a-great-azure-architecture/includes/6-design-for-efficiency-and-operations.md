Din organisation har flyttat en majoritet av sina system till molnet, men ser nu ökade kostnader inom områden de inte förväntat sig. Det visar sig att arbetet sker ineffektivt och att många moment fortfarande utförs manuellt. 

Vi ska titta på några sätt att minimera svinn och förbättra effektiviteten.

## <a name="importance-of-efficiency-and-operations"></a>Vikten av effektivitet och drift

Effektivitet fokuserar på att identifiera och eliminera svinn i din arbetsmiljö. Molnet är en tjänst med betalning per användning och ofta handlar svinn om att mer kapacitet än nödvändigt har avsatts. Utöver detta tillkommer driftskostnader. Dessa driftskostnader tar sig uttryck i spilltid och fler fel. Genom att fokusera på dessa när du utformar din arkitektur kommer du att kunna identifiera och minimera svinn i din miljö.

Det finns olika typer av svinn. Låt oss titta på några exempel:

* En virtuell dator som alltid är 90 % inaktiv
* Betala för en licens som ingår i en virtuell dator när en licens redan ägs
* Spara sällan använda data på ett lagringsmedium som anpassats för frekvent åtkomst
* Manuellt upprätthålla en version av en icke-produktionsmiljö

I dessa fall används mer pengar än nödvändigt, och det öppnar för möjligheter att minska kostnaderna.

Driftsmässigt är det viktigt att ha en genomtänkt strategi för övervakning. Detta hjälper dig att identifiera områden med svinn, underlätta felsökning av problem och optimera programmets prestanda. Ett angreppssätt på flera nivåer är nödvändigt. Genom att hämta datapunkter från komponenter på varje nivå kan du snabbt identifiera värden som ligger utanför angivna ramar och spåra olika utgifter över tid.

## <a name="efficiency-best-practices"></a>Metodtips för effektivitet

Det finns flera sätt att optimera din miljö och maximera effektiviteten. Ett bra ställe att börja är att titta på kostnadsoptimering, som att ge virtuella datorer lämplig storlek, och att ta bort virtuella datorer som inte används. Eftersom du betalar för det du använder är det viktigt att inte slösa bort dessa resurser.

När det är möjligt bör du flytta från IaaS- till PaaS-tjänster. PaaS-tjänster kostar ofta mindre än IaaS och innebär vanligtvis även minskade driftskostnader. Med PaaS-tjänster behöver du inte bekymra dig om uppdatering eller underhåll av virtuella datorer eftersom dessa aktiviteter vanligtvis hanteras av molnleverantören. Alla program kan inte flyttas till PaaS, men med kostnadsbesparingarna som PaaS-tjänster innebär är det värt en närmare undersökning.

## <a name="operational-best-practices"></a>Metodtips för drift

Automatisera så mycket som möjligt. Den mänskliga faktorn är kostsam, och skapar alla möjliga problem med driften. Du kan använda automation för att bygga, distribuera och administrera resurser. Genom att automatisera vanliga aktiviteter kan du eliminera spilltiden i väntan på ett mänskligt ingripande.

Du bör införa noggrann övervakning, loggning och instrumentering i hela din arkitektur. På så vis kan du alltid ta reda på vad som händer. Det gör att du uppmärksammas på problem innan dina användare påverkas. Med ett heltäckande angreppssätt kommer du att kunna identifiera prestandaproblem, kostnadsineffektivitet, korrelera händelser och få större möjlighet att felsöka problem.

Till sist bör moderna arkitekturer utformas med DevOps och kontinuerlig integrering i åtanke. Detta ger dig möjlighet att automatisera distributioner med infrastruktur som kod, automatisera programtestning och skapa nya miljöer efter behov. DevOps är lika mycket kultur som teknik, men kan medföra många fördelar för organisationer som tar sig an det.

Effektivitet handlar om att identifiera och eliminera svinn i din arbetsmiljö. Med molnbaserad databehandling betalar du för det du använder, så det gäller att inte slösa bort resurserna. Säkerställ effektiviteten genom att automatisera så mycket som möjligt, flytta system från IaaS till PaaS och utforma din arkitektur med DevOps i åtanke.