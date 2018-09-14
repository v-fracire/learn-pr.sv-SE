Design för hög tillgänglighet bidrar till att hålla igång ett program eller en process trots ogynnsamma händelser och förhållanden. Men vad gör du när det händer något betydande så att du har förlorat data och det är omöjligt att undvika att dina appar och processer stängs av? När katastrofen är framme måste du ha en plan. Du bör känna till vad dina mål och förväntningar är för att återställa, vilka är de kostnader och begränsningar i din plan och hur du kör på den.

## <a name="what-is-disaster-recovery"></a>Vad är haveriberedskap?

Haveriberedskap handlar om *återställning från hög inverkan händelser* som resulterar i driftstopp och förlust eller data. En katastrof är en enskild, stor händelse vars påverkan är mycket större och varaktig än hur mycket programmet kan minimera via designdelen.

Ordet ”haveriberedskap” ofta ordet tankar av *naturlig* katastrofer och externa händelser (jordbävningar, översvämningar, stormar tropisk och så vidare) men många andra typer av katastrofer finns också. En distribution eller uppgradering som går väldigt fel kan göra en app oigenkännlig. Hackare kan kryptera eller ta bort data och orsaka andra typer av skador som koppla från en app eller ta bort några av dess funktioner.

Oavsett orsak är den enda lösningen på en katastrof när den har inträffat en väldefinierad, testad haveriberedskapsplan och ett program vars design aktivt stöder haveriberedskapsförsök.

## <a name="how-to-create-a-disaster-recovery-plan"></a>Skapa en haveriberedskapsplan

En plan för katastrofåterställning är ett enskilt dokument som ger detaljerad information de procedurer som krävs för att återställa från dataförlust och driftstopp som orsakas av en katastrof och identifierar som är ansvarig för dirigera dessa procedurer. Operatörer bör kunna använda planen som en handbok för att återställa anslutningen och data efter en katastrof. En detaljerad, skriftliga plan som är dedikerad för katastrofåterställning är viktiga för att garantera ett goda resultat. Processen för att skapa planen kan sätta ihop en bild av programmet. De resulterande skriftliga steg flyttar upp bra beslutsfattande och follow-through i panicked, kaotiska jordskalv av ett haveri.

Att skapa en haveriberedskapsplan kräver goda kunskaper om programmets arbetsflöden, data, infrastruktur och beroenden.

### <a name="risk-assessment-and-process-inventory"></a>Riskbedömning och processinventering

Det första steget i att skapa en haveriberedskapsplan är att utföra en riskanalys som undersöker programmets påverkan av olika typer av katastrofer. Den exakta typen av en katastrof är inte lika viktig för riskanalysen som dess potentiella påverkan via dataförlust och driftstopp i programmet. Utforska olika typer av hypotetiska katastrofer och prova att vara specifik när deras inverkan. Riktade skadlig kod kan till exempel ändra kod eller data som resulterar i en annan typ av utsträckning än en jordbävning som stör Nätverkstillgänglighet för anslutning och datacenter.

Riskbedömningen behöver överväga *varje* process som inte klarar ett obegränsat driftstopp, och varje kategori med data som inte klarar obegränsad förlust. När det inträffar en katastrof som påverkar flera programkomponenter är det viktigt att planens ägare kan använda planen för att göra en fullständig förteckning över vad som kräver åtgärder och hur varje del ska prioriteras.

Vissa appar kanske endast utgör en enda process eller klassificering av data. Det är ändå viktigt att notera, eftersom det är sannolikt att programmet är en komponent av en större haveriberedskapsplan son inbegriper flera program i organisationen.

### <a name="recovery-objectives"></a>Återställningsmål

En fullständig plan måste ange två viktiga affärskrav för varje process som implementeras av programmet:

* **Mål för återställningspunkter (RPO)**: Den maximala mängden acceptabel dataförlust. RPO mäts i tidsenheter, inte volymen: ”30 minuters data”, ”fyra timmars data”, och så vidare. RPO handlar om att begränsa och återställa från *dataförlust*, inte *datastöld*.
* **Mål för återställningstid (RTO)**: Den maximala längden på ett acceptabelt driftstopp, där ”driftstopp” måste definieras av din specifikation. Om varaktighet för godkända stilleståndstid är åtta timmar vid katastrofåterställning, är ditt mål för Återställningspunkt åtta timmar.

![Mål för återställningstid och återställningspunkter (RTO och RPO)](../media/rto-rpo.png)

Varje större process eller en arbetsbelastning som implementeras av en app bör ha separata RPO och RTO-värden. Även om du kommer till samma värden för olika processer ska vart och ett genereras via en separat analys som undersöker olika katastrofscenariers risker och potentiella återställningsstrategier för varje respektive process.

Processen för att ange RPO och RTO är i praktiken skapandet av krav på haveriberedskap för ditt program. Det kräver upprättar prioriteten för varje arbetsbelastning och datakategori och utför en kostnadsfördel analys. Analysen innehåller frågor, till exempel kostnaden för implementering och underhåll, driftskostnaden, process omkostnader, prestandapåverkan och effekten av driftstopp och förlorade data. Du måste definiera exakt vilka ”avbrottstid” innebär för ditt program och i vissa fall kan du upprätta separat RPO och RTO värden för olika funktionsnivåer. Att ange RPO och RTO ska innebära mer än att bara välja valfria värden. En stor del av värdet av en haveriberedskapsplan kommer från forskning och analys som leder till upptäckter av eventuell påverkan från en katastrof och kostnaderna för att minimera riskerna.

### <a name="detailing-recovery-steps"></a>Information om åtgärder för återställning

Den slutgiltiga planen ska ange i detalj exakt vilka åtgärder som ska vidtas för att återställa förlorade data och förlorad programanslutning. Stegen omfattar ofta information om:

* **Säkerhetskopior**: hur ofta de skapas, placering och hur du återställer data från dem.
* **Data repliker**: antal och placering av repliker, natur och konsekvens egenskaperna för replikerade data och hur du byter till en annan replik.
* **Distributioner**: hur distributioner utförs, hur återställningar inträffar och scenarier för distribution.
* **Infrastruktur**: lokalt och molnresurser, nätverkets infrastruktur och maskinvaruinventering.
* **Beroenden**: externa tjänster som används av programmet och serviceavtal och kontaktinformation.
* **Konfiguration och meddelande**: flaggor eller alternativ som kan ställas in gradvis kan sänka programmet och tjänster som används för att meddela användare om program påverkan.

Det exakta tillvägagångssätt som krävs beror mycket på implementeringsdetaljer för appen, vilket gör det viktigt att hålla planen uppdateras. Regelbundna tester av planen hjälper till att identifiera luckor och inaktuella delar.

## <a name="designing-for-disaster-recovery"></a>Utforma för haveriberedskap

Haveriberedskap är ingen automatisk funktion. Den måste utformas, skapas och testas. En app som behöver stödja en solid strategi för haveriberedskap måste vara skapad från grunden med haveriberedskap i åtanke. Azure erbjuder tjänster, funktioner och vägledning för att hjälpa dig att skapa appar som stöder haveriberedskap, men det är upp till dig att lägga till dem i din design.

Det finns två huvudsakliga frågeställningar när det gäller att utforma för haveriberedskap:

* **Dataåterställning**: Använda säkerhetskopiering och replikering för att återställa förlorade data.
* **Process för återställning**: Återställa tjänster och distribuera kod för att återställa efter avbrott.

### <a name="data-recovery-and-replication"></a>Återställning av data och replikering

Replikering duplicerar lagrade data mellan flera datalagringsrepliker. Till skillnad från *säkerhetskopiering*, som skapar långlivade, skrivskyddade ögonblicksbilder av data för användning vid återställning skapar replikering kopior av livedata i realtid eller nästan realtid. Målet med replikering är att hålla repliker synkroniserade med så låg fördröjning som möjligt samtidigt som programmets svarstider bibehålls. Replikering är en viktig del av att utforma för hög tillgänglighet och haveriberedskap och är en vanlig funktion i program i produktionsklassen.

Replikering används för att minska ett misslyckat eller otillgängligt datalager genom att köra en *redundans*: ändra programmets konfiguration för att dirigera databegäranden till en aktiv replik. Redundans är ofta automatiserat, utlöst av felidentifiering som skapats i en produkt för lagring av data eller identifiering som du implementerar via din lösning för övervakning. Beroende på implementering och scenario kan redundans köras manuellt av systemoperatörer.

Replikering är inget du implementerar från grunden. De flesta funktionsrika databassystem och andra datalagringsprodukter och tjänster innehåller någon typ av replikering som en väl integrerad funktion på grund av dess krav på funktionalitet och prestanda. Men det är upp till dig att inkludera de här funktionerna i programmets design och använda den på ett lämpligt sätt.

Olika Azure-tjänster stöder olika nivåer och begrepp för replikering. Exempel:

* **Azure Storage** replikeringsfunktionerna beror på vilken replikering valt för lagringskontot. Replikeringen kan vara lokalt (inom ett datacenter), zonindelad (mellan datacenter inom en region) eller regionala (mellan regioner). Varken ditt program eller dina operatörer kan interagera med det direkt. Redundanser är automatiska och genomskinliga, och du måste helt enkelt välja en replikeringsnivå som balanserar kostnad och risk.
* **Azure SQL Database**-replikering är automatisk i liten skala, men återställning från ett fullständigt Azure-datacenter eller regionalt strömavbrott kräver geo-replikering. Konfigurera geo-replikering är manuell, men det är en förstklassig funktion för tjänsten och även stöds av dokumentationen.
* **Cosmos DB** är ett globalt distribuerat databassystem och replikering är centralt för dess implementering. Med Azure Cosmos DB konfigurerar i stället för att konfigurera replikering direkt, du alternativ för konsekvens för partitionering och data.

Det finns många olika replikeringsutformningar som har olika prioriteringar för datakonsekvens, prestanda och kostnad. *Aktiv* replikering kräver att uppdateringar sker på flera repliker samtidigt, vilket garanterar konsekvens på bekostnad av dataflöde. Däremot *passiva* replikering utför synkronisering i bakgrunden, ta bort replikering som en begränsning på programmets prestanda, men ökar rpo-mål. *Aktiv-aktiv* replikering eller replikering med *flera original* gör det möjligt för flera repliker att användas samtidigt, vilket möjliggör belastningsutjämning till priset av mer komplicerad datakonsekvens, medan *aktiv-passiv* replikering reserverar repliker för liveanvändning endast under redundans.

![Azure SQL Database Geo-replikering](../media/geo-replication.png)

> [!IMPORTANT]
> **Varken replikering eller säkerhetskopiering är fullständiga lösningar för haveriberedskap på egen hand**. Dataåterställning är enbart en komponent av haveriberedskap, och replikering uppfyller inte helt många typer av haveriberedskapsscenarier. I till exempel ett scenario med skadade data kan typen av skada göra att skadan sprider sig från det primära datalagret till replikerna. Det gör alla repliker oanvändbara och kräver en säkerhetskopia för återställning.

### <a name="process-recovery"></a>Process för återställning

Efter en katastrof är inte affärsdata den enda tillgången som behöver återställas. Katastrofscenarier leder även till driftstopp, oavsett om det beror på problem med nätverksanslutning, datacenteravbrott eller skadade VM-instanser eller skadad distribution av programvara. Programmets design måste göra det möjligt för dig att återställa det till ett fungerande tillstånd.

I de flesta fall inbegriper processåterställning redundans till en separat fungerande distribution. Beroende på scenario, kan geografisk plats vara en viktig aspekt. Till exempel kommer en storskalig naturkatastrof som ger en hel Azure-region offline kräver återställer tjänsten i en annan region. Programmets krav för haveriberedskap, särskilt RTO ska styra din design- och hjälper dig att avgöra hur många replikerade miljöer som du bör ha, där de ska finnas, och om de ska finnas kvar i tillståndet redo att köra eller bör vara redo att ta emot en distribution i händelse av katastrof.

Beroende på utformning av ditt program finns det några olika strategier och Azure-tjänster och funktioner som du kan dra nytta av för att förbättra appens stöd för processen för återställning efter en katastrof.

#### <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery är en tjänst som används för att hantera processen återställning för arbetsbelastningar som körs på virtuella datorer som distribueras till Azure, virtuella datorer som körs på fysiska servrar och arbetsbelastningar som körs direkt på fysiska servrar. Site Recovery replikerar arbetsbelastningar till alternativa platser och hjälper dig att växla över när ett avbrott sker och stöder testning av en haveriberedskapsplan.

![Azure Site Recovery](../media/asr.png)

Site Recovery stöder replikering av hela virtuella datorer och avbildningar av fysiska servrar och enskilda *arbetsbelastningar*, där en arbetsbelastning kan vara ett enskilt program eller en hel virtuell dator eller operativsystem med sina program. Alla programs arbetsbelastningar kan replikeras, men Site Recovery har ett förstklassigt inbyggt stöd för många Microsoft Server-program som SQL Server och SharePoint, samt en mängd tredjepartsprogram som SAP.

Alla appar som körs på virtuella datorer eller fysiska servrar bör minst undersöka användningen av Azure Site Recovery. Det är ett bra sätt att identifiera och utforska scenarier och möjligheter för återställning av processen.

#### <a name="service-specific-features"></a>Tjänstspecifika funktioner

För appar som körs på Azure PaaS-erbjudanden som App Service erbjuder de flesta sådana tjänster funktioner och vägledning för att stödja haveriberedskap. För vissa scenarier kan du använda tjänstspecifika funktioner för att stödja snabb återställning. Azure SQL Server till exempel stöder geo-replikering för att snabbt återställa tjänsten i en annan region. I Azure App Service finns en funktion för säkerhetskopiering och återställning, och dokumentationen innehåller vägledning för att använda Azure Traffic Manager för att stödja dirigering av trafik till en sekundär region.

![Regionpar](../media/AzRegionPairs.png)

## <a name="testing-a-disaster-recovery-plan"></a>Testa en haveriberedskapsplan

Planering för haveriberedskap slutar inte när du har en färdig plan i handen. Det är viktigt för att testa haveriberedskapsplanen för att garantera att anvisningarna och förklaringarna är tydliga och uppdaterade.

Välj intervaller för att utföra tester av olika typer och omfattningar, som att testa mekanismer för säkerhetskopiering och redundans varje månad och genomföra en fullskalig simulering av haveriberedskap var sjätte månad. Alltid följer du stegen och information om exakt som de är dokumenterade i planen och Överväg att någon bekant med planen ge perspektiv på vad som helst som kan göras tydligare. När du kör testet identifiera brister, förbättring och platser där du kan automatisera och lägga till dessa förbättringar för din prenumeration.

Se till att inkludera din övervakningssystemet gav också. Om ditt program till exempel stöder automatisk redundans kan du introducera fel i ett beroende eller någon annan kritisk komponent för att garantera att programmet beter sig korrekt från slutpunkt till slutpunkt, och att det identifierar felet och utlöser den automatiska redundansen.

 Genom att noggrant identifiera dina krav och utforma en plan kan du avgöra vilka typer av tjänster du behöver använda för att uppfylla dina återställningsmål. Azure tillhandahåller flera tjänster och funktioner för att hjälpa dig att uppnå dessa mål.
