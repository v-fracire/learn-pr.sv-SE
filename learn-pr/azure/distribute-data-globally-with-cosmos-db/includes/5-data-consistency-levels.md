Med Azure Cosmos DB kan utvecklare välja mellan fem väldefinierade konsekvensmodeller längs konsekvensspektrumet: stark, begränsad föråldring, session, konsekvent prefix och slutlig. Dessa konsekvensnivåer gör att du kan maximera tillgänglighet och prestanda för din databas, beroende på dina behov. I de fall då data måste bearbetas i en viss ordning kan stark konsekvens vara rätt val. I de fall då data inte behöver vara omedelbart konsekventa kan slutlig konsekvens vara rätt val. 

I den här enheten bekantar du dig med de tillgängliga konsekvensnivåerna i Azure Cosmos DB och bestämmer rätt konsekvensnivå för din webbplats med kläder.

## <a name="consistency-basics"></a>Grunderna om konsekvens

Varje databaskonto har en standardkonsekvensnivå som avgör konsekvensen för data i kontot. I den ena änden av spektrumet finns stark konsekvens, som ger en linjärbarhetsgaranti där läsningarna garanterat returnerar den senaste versionen av ett objekt. I den andra änden av spektrumet finns slutlig konsekvens, som garanterar att om det inte förekommer ytterligare skrivningar så konvergeras repliker i gruppen så småningom. I mitten finns sessionskonsekvens, som är den mest populära eftersom det garanterar monotoniska läsningar, monotona skrivningar och garantier om ”läs dina egna skrivningar” (RYW, Read Your Own Writes).

![The fem konsekvensnivåerna i Azure Cosmos DB](../media/5-change-consistency/five-consistency-levels.png)

Garantier för varje konsekvensnivå visas i följande tabell.
 
**Konsekvensnivåer och garantier**

| Konsekvensnivå | Garantier |
| --- | --- |
| Stark | Linjärbarhet. Läsningar är garanterade att returnera den senaste versionen av ett objekt.|
| Begränsad föråldring | Konsekvent prefix. Läsningar släpar efter skrivningar med som mest k-prefix eller t-intervall. |
| Session   | Konsekvent prefix. Monotoniska läsningar, monotoniska skrivningar, läs-dina-skrivningar, skrivning-följer-läsning. |
| Konsekvent prefix | De uppdateringar som returneras är något prefix av alla uppdateringar, utan några mellanrum. |
| Slutlig  | Oordnade läsningar. |

Du kan konfigurera standardkonsekvensnivån på ditt Azure Cosmos DB-konto (och senare åsidosätta konsekvens för en specifik läsbegäran) på Azure-portalen. Internt gäller standardkonsekvensnivån för data inom partitionsuppsättningarna, som kan sträcka sig över flera regioner.

I Azure Cosmos DB är läsningar som ges vid session, konsekvent prefix och slutlig konsekvens dubbelt så billiga när det gäller förbrukning av enheter för programbegäran jämfört med läsningar med stark konsekvens eller konsekvens med begränsad föråldring.

### <a name="use-of-consistency-levels"></a>Användning av konsekvensnivåer

Cirka 73 % av Azure Cosmos DB-klientorganisationer använder sessionskonsekvens, och 20 % föredrar begränsad föråldring. Cirka 3 % av Azure Cosmos DB-kunderna experimenterar med olika konsekvensnivåer inledningsvis innan de väljer en viss konsekvens för sitt program. Endast 2 %  av Azure Cosmos DB-klientorganisationer åsidosätter konsekvensnivåer per begäran.

## <a name="consistency-levels-in-detail"></a>Ingående information om konsekvensnivåer

Om du vill ha mer information om konsekvensnivåerna tittar du på de notskriftsbaserade konsekvensexempel som finns i Azure-portalen och granskar sedan informationen nedan om varje nivå.

1. I Azure-portalen klickar du på **Standardkonsekvens**.
2. Klicka dig igenom var och en av de olika konsekvensmodellerna och titta på musikexemplen. Se hur data skrivs till olika regioner och hur valet av konsekvens påverkar hur notdata skrivs. Observera att Strong (Stark) är nedtonat eftersom det bara är tillgängligt för data som skrivs till en enda region.

    ![Lär dig mer om konsekvensinställningar i portalen](../media/5-change-consistency/consistency.gif)

Nu tittar vi närmare på konsekvensnivåerna. Tänk på hur var och en av dessa konsekvensnivåer kan fungera för produkten och användardata för din webbaserade klädbutik.

### <a name="strong-consistency"></a>Stark konsekvens

* Stark konsekvens erbjuder en [linjärbarhetsgaranti](https://aphyr.com/posts/313-strong-consistency-models) där läsningarna garanterat returnerar den senaste versionen av ett objekt.
* Stark konsekvens garanterar att en skrivning endast är synlig när den genomförs varaktigt av majoritetskvorumet av repliker. En skrivning genomförs antingen synkront och varaktigt av både de primära och kvorumet av de sekundära, eller så avbryts den. En läsning bekräftas alltid av majoritetskvorumet för läsning. En klient kan aldrig se en ogenomförd eller partiell skrivning, och läser garanterat alltid den senaste bekräftade skrivningen. 
* Azure Cosmos DB-konton som är konfigurerade för att använda stark konsekvens kan inte associera mer än en Azure-region med ett Azure Cosmos DB-konto.  
* Kostnaden för en läsåtgärd (vad gäller förbrukade enheter för programbegäran) med stark konsekvens är högre än med sessionskonsekvens och slutlig konsekvens, men den är samma som för begränsad föråldring.

### <a name="bounded-staleness-consistency"></a>Konsekvens med begränsad föråldring

* Konsekvens med begränsad föråldring garanterar att läsningarna kan släpa efter skrivningar med minst *K* versioner eller prefix för ett objekt eller ett tidsintervall på *t*.
* När begränsad föråldring väljs kan ”föråldringen” därför konfigureras på två sätt: det antal versioner *K* för objektet som läsningarna släpar efter skrivningarna, och tidsintervallet *t*.
* Begränsad föråldring erbjuder total global ordning förutom i ”föråldringsfönstret”. Garantierna för monoton läsning finns i en region både inuti och utanför ”föråldringsfönstret”.
* Begränsad föråldring ger en starkare konsekvensgaranti än session, konsekvent prefix eller slutlig konsekvens. För globalt distribuerade program rekommenderar vi du använder begränsad föråldring för scenarier där du vill ha stark konsekvens men även vill ha 99,99 % tillgänglighet och korta svarstider.
* Azure Cosmos DB-konton som är konfigurerade med konsekvens med begränsad föråldring kan associera valfritt antal Azure-regioner till sitt Azure Cosmos DB-konto. 
* Kostnaden för en läsåtgärd (vad gäller förbrukade enheter för programbegäran) med begränsad föråldring är högre än med sessionskonsekvens och slutlig konsekvens, men den är samma som för stark konsekvens.

### <a name="session-consistency"></a>Sessionskonsekvens

* Till skillnad för de modeller för global konsekvens som erbjuds av de starka konsekvensnivåerna och konsekvensnivåerna med begränsad föråldring har sessionskonsekvens omfång för en klientsession.
* Sessionskonsekvens är utmärkt för alla scenarier där en enhets- eller en användarsession ingår eftersom det garanterar monotoniska läsningar, monotona skrivningar och garantier för ”läs dina egna skrivningar” (RYW).
* Sessionskonsekvens ger förutsägbar konsekvens för en session och maximalt läsningsdataflöde. Det erbjuder även skrivningar och läsningar med den kortaste svarstiden.
* Azure Cosmos DB-konton som är konfigurerade med sessionskonsekvens kan associera valfritt antal Azure-regioner till sitt Azure Cosmos DB-konto.
* Kostnaden för en läsåtgärd (vad gäller förbrukade enheter för programbegäran) med sessionskonsekvens är högre än med stark konsekvens och konsekvens med begränsad föråldring, men den är högre än med slutlig konsekvens.

### <a name="consistent-prefix-consistency"></a>Konsekvens med konsekvent prefix

* Konsekvent prefix garanterar att om det inte förekommer ytterligare skrivningar så konvergeras repliker i gruppen så småningom. 
* Konsekvent prefix garanterar att läsningar aldrig ser skrivningar i oordning. Om skrivningar utfördes i ordningen `A, B, C` ser klienten antingen `A`, `A,B` eller `A,B,C`, men aldrig i oordning, till exempel `A,C` eller `B,A,C`.
* Azure Cosmos DB-konton som är konfigurerade med konsekvens med konsekvent prefix kan associera valfritt antal Azure-regioner till sitt Azure Cosmos DB-konto. 

### <a name="eventual-consistency"></a>Slutlig konsekvens

* Slutlig konsekvens garanterar att om det inte förekommer ytterligare skrivningar så konvergeras repliker i gruppen så småningom.
* Slutlig konsekvens är den svagaste formen av konsekvens, där en klient kan hämta de värden som är äldre än dem som den hade sett förut.
* Slutlig konsekvens ger den svagaste läsningskonsekvensen men erbjuder kortast svarstid för både läsningar och skrivningar.
* Azure Cosmos DB-konton som är konfigurerade med slutlig konsekvens kan associera valfritt antal Azure-regioner till sitt Azure Cosmos DB-konto. 
* Kostnaden för en läsåtgärd (vad gäller förbrukade enheter för programbegäran) med nivån slutlig konsekvens är den lägsta för alla Azure Cosmos DB-konsekvensnivåer.

## <a name="summary"></a>Sammanfattning

I den här enheten har du lärt dig hur konsekvensnivåer kan användas för att maximera hög tillgänglighet och minimera svarstiden.