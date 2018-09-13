Nu när du känner till fördelarna med Azure Storage och vilka funktioner som är tillgängliga ska vi titta närmare på hur Azure Storage skiljer sig från lokal lagring.

## <a name="azure-storage-versus-on-premises-storage"></a>Azure Storage jämfört med lokal lagring

Termen ”lokal” syftar på lagring och underhåll av data på lokala maskiner och servrar. Det finns flera faktorer som du bör överväga när du jämför lokal lagring och Azure Storage.

### <a name="cost-effectiveness"></a>Kostnadseffektivitet
En lokal lagringslösning kräver dedikerad maskinvara som måste köpas, installeras, konfigureras och underhållas. Detta kan innebära en betydande investeringskostnad (eller kapitalkostnad). Kommande ändringar av kraven kan kräva investeringar i ny maskinvara. Och eventuella tillfälliga toppar i efterfrågan kan medföra att du måste investera i maskinvara som kan hantera sådana toppar, men som kommer att vara inaktiv eller outnyttjad i perioder med låg belastning.

Azure Storage tillhandahåller en prismodell där du betalar per användning, vilket ofta är tilltalande för företag eftersom det rör sig om löpande kostnader snarare än initiala investeringskostnader. Det är också skalbart, vilket innebär att du kan skalanpassa lösningen i takt med efterfrågan. Du debiterar endast för de datatjänster du utnyttjar.

### <a name="reliability"></a>Tillförlitlighet 
Lokal lagring kräver säkerhetskopiering av data, belastningsutjämning och strategier för haveriberedskap. Detta kan vara utmanande och dyrt, och ofta krävs dedikerade servrar som kräver en betydande investering i både maskinvara och IT-resurser.

Azure Storage tillhandahåller säkerhetskopiering av data, belastningsutjämning, katastrofåterställning och replikering av data som tjänst för att garantera datasäkerhet och hög tillgänglighet.

### <a name="storage-types"></a>Lagringstyper
Ibland krävs flera olika lagringstyper för en lösning, t.ex fil- och databaslagring. En lokal metod kräver ofta flera servrar och administrativa verktyg för resektive lagringstyp.

Azure Storage tillhandahåller en mängd olika lagringsalternativ, däribland distribuerad åtkomst och nivåindelad lagring. Detta gör det möjligt att integrera en kombination av lagringstekniker som ger det bästa lagringsalternativet för varje del av din lösning.

### <a name="agility"></a>Flexibilitet
Krav och tekniker ändras. För en lokal distribution kan detta innebära att nya servrar och delar av infrastrukturen måste etableras och distribueras, vilket är både tidskrävande och dyrt.

Azure Storage ger dig möjlighet att skapa nya tjänster på några minuter. Med den här flexibiliteten kan du snabbt ändra lagringsservrar utan att behöva göra någon betydande maskinvaruinvestering.

### <a name="other-requirements"></a>Övriga krav
I tabellen nedan beskrivs vad du kan behöva tänka på angående några andra vanliga krav:

![Jämförelse](../media-draft/Comparison.png)
