Design för hög tillgänglighet bidrar till att hålla igång ett program eller en process trots ogynnsamma händelser och förhållanden. Men vad gör du när det händer något betydande så att du har förlorat data och det är omöjligt att undvika att dina appar och processer stängs av? När katastrofen är ett faktum så behöver du ha en plan för att få igång dina tjänster igen. Du bör veta vad du har för mål och förväntningar för återställning, vad din plan har för kostnader och begränsningar och hur du ska implementera den.

## <a name="what-is-disaster-recovery"></a>Vad är haveriberedskap?

Haveriberedskap handlar om att *återställa från händelser med stor inverkan* som leder till driftstopp och dataförlust. En katastrof är en enskild, stor händelse vars påverkan är mycket större och mer varaktig än vad programmet kan hantera via designdelen för hög tillgänglighet.

Ordet katastrof leder ofta tanken till *naturkatastrofer* och externa händelser (jordbävningar, översvämningar, tropiska stormar med mera) men det finns även många andra typer av katastrofer. En misslyckad distribution eller uppgradering kan göra en app oigenkännlig. Hackare kan kryptera eller ta bort data och vålla andra typer av skador som kan försätta en app i offlineläge eller eliminera en del av dess funktioner.

Oavsett orsak är den bästa lösningen på en katastrof när den har inträffat en väldefinierad, testad haveriberedskapsplan och ett program vars design aktivt stöder haveriberedskapsförsök.

## <a name="how-to-create-a-disaster-recovery-plan"></a>Skapa en haveriberedskapsplan

En haveriberedskapsplan är ett enskilt dokument som beskriver procedurerna som krävs för att återställa efter dataförlust och stilleståndstid som orsakas av en katastrof och identifierar vem som ansvarar för att dirigera sådana procedurer. Operatörer bör kunna använda planen som en handbok för att återställa anslutningen och data efter en katastrof. En detaljerad, skriftlig plan som är dedikerad för katastrofåterställning är viktigt för att garantera ett bra utfall. Processen med att skapa planen kommer att hjälpa att sammanställa en fullständig bild av programmet. De resulterande skriftliga stegen kommer att hjälpa vid beslutsfattande och uppföljning under de panikartade och kaotiska eftermälet vid en katastrofåterställning.

Att skapa en haveriberedskapsplan kräver goda kunskaper om programmets arbetsflöden, data, infrastruktur och beroenden.

### <a name="risk-assessment-and-process-inventory"></a>Riskbedömning och processinventering

Det första steget i att skapa en haveriberedskapsplan är att utföra en riskanalys som undersöker programmets påverkan av olika typer av katastrofer. Den exakta typen av en katastrof är inte lika viktig för riskanalysen som dess potentiella påverkan via dataförlust och driftstopp i programmet. Utforska olika typer av hypotetiska katastrofer och försök vara specifik om deras inverkan. Ett riktat skadligt angrepp kan ändra kod eller data vilket leder till andra typer av påverkan än en jordbävning som avbryter nätverksanslutningen och tillgängligheten för datacentret.

Riskbedömningen behöver överväga *varje* process som inte klarar ett obegränsat driftstopp, och varje kategori med data som inte klarar obegränsad förlust. När det inträffar en katastrof som påverkar flera programkomponenter är det viktigt att planens ägare kan använda planen för att göra en fullständig förteckning över vad som kräver åtgärder och hur varje del ska prioriteras.

Vissa appar kanske endast utgör en enda process eller klassificering av data. Det är ändå viktigt att notera, eftersom det är sannolikt att programmet är en komponent av en större haveriberedskapsplan som inbegriper flera program i organisationen.

### <a name="recovery-objectives"></a>Återställningsmål

En fullständig plan måste ange två viktiga affärskrav för varje process som implementeras av programmet:

* **Mål för återställningspunkter (RPO)**: Den maximala mängden acceptabel dataförlust. RPO mäts i tidsenheter, inte volym: ”30 minuters data”, ”fyra timmars data” och så vidare. RPO handlar om att begränsa och återställa från *dataförlust*, inte *datastöld*.
* **Mål för återställningstid (RTO)**: Den maximala längden på ett acceptabelt driftstopp, där ”driftstopp” måste definieras av din specifikation. Om den acceptabla stilleståndstiden är åtta timmar vid katastrofåterställning är ditt mål för återställningspunkt åtta timmar.

![En bild som visar varaktigheten i timmar för återställningspunktens mål och återställningstidens mål från haveriets tidpunkt.](../media/rto-rpo.png)

Varje större process eller arbetsbelastning som implementeras av en app bör ha separata RPO- och RTO-värden. Även om du kommer till samma värden för olika processer ska vart och ett genereras via en separat analys som undersöker olika katastrofscenariers risker och potentiella återställningsstrategier för varje respektive process.

Processen för att ange RPO och RTO är i praktiken skapandet av krav på haveriberedskap för ditt program. Det kräver att du upprättar prioriteten för varje arbetsbelastning och datakategori och utför en kostnadsanalys. Analysen innehåller frågor, till exempel kostnad för implementering och underhåll, driftskostnad, processomkostnad, prestandapåverkan och effekt av driftstopp och förlorade data. Du måste definiera exakt vad avbrottstid innebär för ditt program, och i vissa fall kan du upprätta separata RPO- och RTO-värden för olika funktionsnivåer. Att ange RPO och RTO ska innebära mer än att bara välja valfria värden. En stor del av värdet av en haveriberedskapsplan kommer från forskning och analys som leder till upptäckter av eventuell påverkan från en katastrof och kostnaderna för att minimera riskerna.

### <a name="detailing-recovery-steps"></a>Information om åtgärder för återställning

Den slutgiltiga planen ska ange i detalj exakt vilka åtgärder som ska vidtas för att återställa förlorade data och förlorad programanslutning. Stegen omfattar ofta information om:

* **Säkerhetskopior**: hur ofta de skapas, var de finns och hur du återställer data från dem.
* **Datarepliker**: Antal repliker och deras platser, karaktärs- och konsekvensegenskaper för replikerade data och hur man växlar över till en annan replik.
* **Distributioner**: Hur distributioner körs, hur återställningar inträffar och felscenarier för distributioner.
* **Infrastruktur**: resurser lokalt och i molnet, nätverksinfrastruktur och maskinvaruinventering.
* **Beroenden**: Externa tjänster som används av programmet, inklusive serviceavtal och kontaktinformation.
* **Konfiguration och meddelanden**: Flaggor eller alternativ som kan ställas in för att nedgradera programmet på ett snyggt sätt och tjänster som används för att meddela användare om programpåverkan.

De exakta stegen som krävs beror mycket på appens implementeringsinformation, vilket gör det viktigt att hålla planen uppdaterad. Regelbundna tester av planen hjälper till att identifiera luckor och inaktuella avsnitt.

## <a name="designing-for-disaster-recovery"></a>Utforma för haveriberedskap

Haveriberedskap är ingen automatisk funktion. Den måste utformas, skapas och testas. En app som behöver stödja en solid strategi för haveriberedskap måste vara skapad från grunden med haveriberedskap i åtanke. Azure erbjuder tjänster, funktioner och vägledning för att hjälpa dig att skapa appar som stöder haveriberedskap, men det är upp till dig att lägga till dem i din design.

Det finns två huvudsakliga frågeställningar när det gäller att utforma för haveriberedskap:

* **Dataåterställning**: Använda säkerhetskopiering och replikering för att återställa förlorade data.
* **Process för återställning**: Återställa tjänster och distribuera kod för att återställa efter avbrott.

### <a name="data-recovery-and-replication"></a>Återställning av data och replikering

Replikering duplicerar lagrade data mellan flera datalagringsrepliker. Till skillnad från *säkerhetskopiering*, som skapar långlivade, skrivskyddade ögonblicksbilder av data för användning vid återställning skapar replikering kopior av livedata i realtid eller nästan realtid. Målet med replikering är att hålla repliker synkroniserade med så låg fördröjning som möjligt samtidigt som programmets svarstider bibehålls. Replikering är en viktig del av att utforma för hög tillgänglighet och haveriberedskap och är en vanlig funktion i program i produktionsklassen.

Replikering används för att minska ett misslyckat eller otillgängligt datalager genom att köra en *redundans*: ändra programmets konfiguration för att dirigera databegäranden till en aktiv replik. Redundans är ofta automatiserat, utlöst av felidentifiering som skapats i en produkt för lagring av data eller identifiering som du implementerar via din lösning för övervakning. Beroende på implementering och scenario kan redundans köras manuellt av systemoperatörer.

Replikering är inget du implementerar från grunden. De flesta funktionsrika databassystem och andra datalagringsprodukter och tjänster innehåller någon typ av replikering som en väl integrerad funktion på grund av dess krav på funktionalitet och prestanda. Men det är upp till dig att inkludera de här funktionerna i programmets design och använda den på ett lämpligt sätt.

Olika Azure-tjänster stöder olika nivåer och begrepp för replikering. Till exempel:

* **Azure Storage** replikeringsfunktionerna beror på vilken replikeringstyp som valts för lagringskontot. Den här replikeringen kan vara lokal (inom ett datacenter), zonindelad (mellan datacenter inom en region) eller regionala (mellan regioner). Varken ditt program eller dina operatörer kan interagera med det direkt. Redundanser är automatiska och genomskinliga, och du måste helt enkelt välja en replikeringsnivå som balanserar kostnad och risk.
* **Azure SQL Database**-replikering är automatisk i liten skala, men återställning från ett fullständigt Azure-datacenter eller regionalt strömavbrott kräver geo-replikering. Konfiguration av georeplikering görs manuellt men är en förstaklassfunktion för tjänsten och stöds väl av dokumentationen.
* **Cosmos DB** är ett globalt distribuerat databassystem och replikering är centralt för dess implementering. Med Azure Cosmos DB konfigurerar du alternativ som relaterar till partitionering och datakonsekvens istället för att konfigurera replikering direkt.

Det finns många olika replikeringsutformningar som har olika prioriteringar för datakonsekvens, prestanda och kostnad. *Aktiv* replikering kräver att uppdateringar sker på flera repliker samtidigt, vilket garanterar konsekvens på bekostnad av dataflöde. Däremot utför *passiv* replikering synkronisering i bakgrunden, vilket tar bort replikering som en begränsning av programprestanda men ökar återställningspunktmålet (RPO). Med *Aktiv-aktiv*-replikering eller replikering med *flera original* är det möjligt för flera repliker att användas samtidigt, vilket möjliggör lastbalansering till priset av mer komplicerad datakonsekvens, medan *aktiv-passiv*-replikering reserverar repliker för liveanvändning endast under redundans.

![Ett bildexempel på geografisk replikering. Den primära uppdateringsbara databasen placeras i södra centrala USA, medan de läsbara sekundära databaserna placeras på två olika platser: västra USA och östra USA.](../media/geo-replication.png)

> [!IMPORTANT]
> **Varken replikering eller säkerhetskopiering är fullständiga lösningar för haveriberedskap på egen hand**. Dataåterställning är enbart en komponent av haveriberedskap, och för många typer av haveriberedskapsscenarier räcker inte bara replikering. I till exempel ett scenario med skadade data kan typen av skada göra att skadan sprider sig från det primära datalagret till replikerna. Det gör alla repliker oanvändbara och kräver en säkerhetskopia för återställning.

### <a name="process-recovery"></a>Process för återställning

Efter en katastrof är inte affärsdata den enda tillgången som behöver återställas. Katastrofscenarier leder även till driftstopp, oavsett om det beror på problem med nätverksanslutning, datacenteravbrott eller skadade VM-instanser eller skadad distribution av programvara. Programmets design måste göra det möjligt för dig att återställa det till ett fungerande tillstånd.

I de flesta fall inbegriper processåterställning redundans till en separat fungerande distribution. Beroende på scenario kan geografisk plats vara en viktig aspekt. Till exempel kan en storskalig naturkatastrof som leder till att en hel Azure-region försätts i offlineläge kräva att tjänsten återställs i en annan region. Programmets krav på haveriberedskap, särskilt RTO, bör påverka din design och hjälpa dig att bestämma hur många replikerade miljöer du ska ha, var de ska finnas och om de ska underhållas i ett körklart tillstånd eller om de ska vara redo att godkänna en distribution vid en katastrof.

Beroende på programmets design finns det några olika strategier och Azure-tjänster och -funktioner som du kan använda för att förbättra appens stöd för processåterställning efter en katastrof.

#### <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery är en tjänst som är dedikerad för att hantera processåterställning för arbetsbelastningar som körs på virtuella datorer som är distribuerade i Azure, virtuella datorer som körs på fysiska servrar och arbetsbelastningar som körs direkt på fysiska servrar. Site Recovery replikerar arbetsbelastningar till alternativa platser och hjälper dig att växla över när ett avbrott sker och stöder testning av en haveriberedskapsplan.

![En bild som visar rollen för Azure-webbplatsåterställning vid replikering av arbetsbelastningarna på tre virtuella datorer som finns i USA, östra och USA, västra.](../media/asr.png)

Site Recovery stöder replikering av hela virtuella datorer och avbildningar av fysiska servrar och enskilda *arbetsbelastningar*, där en arbetsbelastning kan vara ett enskilt program eller en hel virtuell dator eller operativsystem med sina program. Alla programs arbetsbelastningar kan replikeras, men Site Recovery har ett förstklassigt inbyggt stöd för många Microsoft Server-program, som SQL Server och SharePoint, samt en mängd tredjepartsprogram som SAP.

Alla appar som körs på virtuella datorer eller fysiska servrar bör minst undersöka användning av Azure Site Recovery. Det är ett bra sätt att identifiera och utforska scenarier och möjligheter för processåterställning.

#### <a name="service-specific-features"></a>Tjänstspecifika funktioner

För appar som körs på Azure PaaS-erbjudanden, som App Service, erbjuder de flesta sådana tjänster funktioner och vägledning för att stödja haveriberedskap. För vissa scenarier kan du använda tjänstspecifika funktioner för att stödja snabb återställning. Azure SQL Server till exempel stöder geo-replikering för att snabbt återställa tjänsten i en annan region. I Azure App Service finns en funktion för säkerhetskopiering och återställning, och dokumentationen innehåller vägledning om hur du använder Azure Traffic Manager som stöd vid dirigering av trafik till en sekundär region.

![En bild som visar ett regionalt par i ett geografiskt område som består enskilda datacentra.](../media/AzRegionPairs.png)

## <a name="testing-a-disaster-recovery-plan"></a>Testa en haveriberedskapsplan

Planering för haveriberedskap slutar inte när du har en färdig plan i handen. Det är viktigt för att testa haveriberedskapsplanen för att garantera att anvisningarna och förklaringarna är tydliga och uppdaterade.

Välj intervaller för att utföra tester av olika typer och omfattningar, som att testa mekanismer för säkerhetskopiering och redundans varje månad och genomföra en fullskalig simulering av haveriberedskap var sjätte månad. Följ alltid anvisningarna och informationen exakt som de står i planen, och överväg att låta någon som är obekant med planen ge sitt perspektiv på vad som kan göras tydligare. När du kör testet bör du identifiera brister, förbättringar och platser där du kan automatisera och lägga till dessa förbättringar till din prenumeration.

Se även till att testa ditt övervakningssystem. Om ditt program till exempel stöder automatisk redundans kan du introducera fel i ett beroende eller någon annan kritisk komponent för att garantera att programmet beter sig korrekt från slutpunkt till slutpunkt, och att det identifierar felet och utlöser den automatiska redundansen.

 Genom att noggrant identifiera dina krav och utforma en plan kan du avgöra vilka typer av tjänster du behöver använda för att uppfylla dina återställningsmål. Azure tillhandahåller flera tjänster och funktioner för att hjälpa dig att uppnå dessa mål.
