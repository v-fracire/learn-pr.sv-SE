Vissa program har större krav på datalagring än andra. Appar som Dynamics CRM, Exchange Server, SAP Business Suite, SQL Server, Oracle och SharePoint kräver konstant hög prestanda och kort svarsstid för optimal prestanda.

När du skapar dina virtuella datorer eller att lägger till nya diskar har vissa val en stor inverkan på diskprestandan, till exempel vilken _typ_ av lagring som du använder.

## <a name="types-of-disks"></a>Typer av diskar 
Azure-diskar är utformade för 99,999% tillgänglighet. 

Det finns tre prestandanivåer för lagring som du kan välja när du skapar diskar – Premium SSD-diskar, Standard HDD-lagring och Standard SSD. Beroende på de virtuella datorernas storlek kan du blanda dessa disktyper.

### <a name="premium-ssd-disks"></a>Premium SSD-diskar 

Premium SSD-diskar stöds av solid state-hårddiskar och levererar högpresterande disksupport med låg fördröjning för virtuella datorer som kör I/O-intensiva arbetsbelastningar. De här enheterna tenderar att vara mer tillförlitliga eftersom de inte har rörliga delar. Ett läs- eller skrivhuvud behöver inte flyttas till rätt plats på en disk för att kunna hitta de data som efterfrågas. 

Du kan använda Premium SSD-diskar med VM-storlekar som inkluderar ”s” i dataseriens namn. Till exempel, av **Dv3-serien** och **Dsv3-serien**, kan **Dsv3-serien** användas med Premium SSD-diskar.

### <a name="standard-hdd-storage"></a>Standard HDD-lagring

Standard HDD-diskar backas upp av traditionella hårddiskar (HDD). Standard HDD-diskar kostar mindre än Premium-diskar. Standard HDD-diskar kan användas med virtuella datorer av valfri storlek.

### <a name="standard-ssd"></a>Standard SSD

Premiumlagring är begränsat till specifika storlekar av virtuella datorer. Typen av virtuell dator som du skapar kommer att påverka lagringskapaciteten: storlek, maxkapacitet och lagringstyp. Vad händer om du har en enklare virtuell dator, men du behöver SSD-lagring för I/O-prestanda? Det är det som Standard SDD-enheter är till för. Standard SSD ligger mellan Standard HDD och Premium SSD ur ett prestanda- och kostnadsperspektiv.

Du kan använda Standard SSD:er med alla virtuella datorstorlekar, inklusive storlekar som saknar stöd för Premium Storage. Standard SSD:er är det enda sättet att använda SSD:er med dessa virtuella datorer. Den här disktypen är endast tillgänglig i vissa regioner och endast med _hanterade diskar_.

### <a name="unmanaged-vs-managed-disks"></a>Ohanterade och hanterade diskar

När du skapar virtuella datorer eller virtuella hårddiskar har du möjlighet att använda **ohanterade** eller **hanterade** diskar.

Med ohanterade diskar ansvarar du för de lagringskonton som används för att lagra de virtuella hårddiskar som motsvarar diskarna på din virtuella dator. Du betalar lagringskontoavgifter för den mängd utrymme du använder. Ett lagringskonto har en fast gräns på 20 000 I/O-åtgärder per sekund. Det betyder att ett enda lagringskonto kan hantera 40 virtuella standardhårddiskar vid maxkapacitet. Om du behöver skala ut behöver du fler än ett lagringskonto, något som kan vara komplicerat.

Hanterade diskar är den **nyare och rekommenderade disklagringsmodellen**. De löser elegant den här komplexiteten genom att lägga hanteringen av lagringskonton på Azure. Du anger disktypen och storleken på disken, så skapar och hanterar Azure både disken _och_ den lagring som disken använder. Du behöver inte bekymra dig om begränsningar i lagringskontot, vilket gör det lättare att skala ut. Här är några av fördelarna du får jämfört med äldre ohanterade diskar:

- **Ökad tillförlitlighet**: Azure ser till att virtuella hårddiskar associerade med virtuella datorer med hög tillförlitlighet placeras i olika delar av Azure Storage för att ge motsvarande återhämtningsnivåer.
- **Bättre säkerhet**: Hanterade diskar är hanterade resurser i resursgruppen. Det betyder att de kan använda rollbaserad åtkomstkontroll för att begränsa vem som kan arbeta med VHD-data.
- **Stöd för ögonblicksbilder**: Ögonblicksbilder kan användas för att skapa en skrivskyddad kopia av en VHD. Du måste stänga av den ägande virtuella datorn men att skapa ögonblicksbilden tar bara några sekunder. När det är gjort kan du slå på den virtuella datorn och använda ögonblicksbilden till att skapa en dubblett av den virtuella datorn för att felsöka ett produktionsproblem eller återställa den virtuella datorn till den tidpunkt då ögonblicksbilden togs.
- **Stöd för säkerhetskopiering**: Hanterade diskar kan automatiskt säkerhetskopieras till olika regioner för haveriberedskap med Azure Backup, utan att påverka driften av den virtuella datorn.

Med tanke på dessa fördelar, till exempel de garanterade prestandaegenskaperna, bör du alltid välja hanterade diskar för nya virtuella datorer.

### <a name="disk-comparison"></a>Diskjämförelse
Följande tabell innehåller en jämförelse av Standard HDD, Standard SSD och Premium SSD för att hjälpa dig att avgöra vad som passar dig bäst.

|    | Azure Premium-disk |Azure Standard SSD-Disk (förhandsversion)| Azure Standard HDD-disk 
|--- | ------------------ | ------------------------------- | ----------------------- 
| **Disktyp** | Solid State-hårddiskar (SSD) | Solid State-hårddiskar (SSD) | Hårddiskar (HDD)  
| **Översikt**  | SSD-baserad högpresterande disksupport med låg fördröjning för virtuella datorer som kör I/O-intensiva arbetsbelastningar eller är värd för verksamhetskritisk produktionsmiljö |Mer konsekventa prestanda och tillförlitlighet än HDD. Optimerad för arbetsbelastningar med låg IOPS| HDD-baserad kostnadseffektiv disk för lågfrekvent åtkomst
| **Scenario**  | Produktion och prestandakänsliga arbetsbelastningar |Webbservrar, företagsprogram med lätt användning och Dev/Test| Säkerhetskopiering, icke-kritisk, lågfrekvent åtkomst 
| **Diskstorlek** | 32-4095 GiB | 128-4095 GiB | 32-4095 GiB 
| **Maxdataflöde per disk** | 250 MiB/s | Upp till 60 MiB/s | Upp till 60 MiB/s 
| **Max-IOPS per disk** | 7500 IOPS | Upp till 500 IOPS | Upp till 500 IOPS 

Det finns mer information om diskprestandan nedan.

## <a name="data-replication"></a>Datareplikering

Datan på ditt Microsoft Azure-lagringskonto replikeras alltid för att säkerställa hållbarhet och hög tillgänglighet. Azure Storage-replikering kopierar dina data så att de är skyddade mot planerade och oplanerade händelser som tillfälliga maskinvarufel, elavbrott, naturkatastrofer och så vidare. Du kan välja att replikera dina data inom samma datacenter, över zonindelade datacentra inom samma region, och till och med i olika regioner. Det finns fyra typer av replikering:

- **Lokalt redundant lagring (LRS)** – Azure replikerar datan inom samma Azure-datacenter. Datan förblir tillgänglig om en nod slutar att fungera. Men om hela datacentret slutar att fungera, kanske datan inte är tillgänglig.
- **Geo-redundant lagring (GRS)** – Azure replikerar datan till en sekundär region som ligger hundratals kilometer från den primära regionen. Om ditt lagringskonto har GRS aktiverat, är dina data beständiga även om det sker ett fullständig regionalt strömavbrott eller en katastrof där den primära regionen inte återställas.
- **Geo-redundant lagring med läsåtkomst (RA-GRS)** – Azure tillhandahåller skrivskyddad åtkomst till data på den sekundära platsen och geo-replikering mellan två regioner. Om ett datacenter slutar att fungera förblir datan läsbar, men den kan inte ändras.
- **Zonredundant lagring (ZRS)** – Azure replikerar dina data synkront över tre lagringskluster i en enda region. Varje lagringskluster är fysiskt avgränsat från de andra och ligger i sin egen tillgänglighetszon (AZ). Med den här typen av replikering har du fortfarande åtkomst till och kan hantera dina data även om en zon blir otillgänglig.

Standard Storage-konton stöder alla typer av replikering, men Premium Storage-konton har endast stöd för lokalt redundant lagring (LRS). Eftersom de virtuella datorerna körs i en enda region, är den här begränsningen normalt inte något problem vid VHD-lagring.

## <a name="disk-performance"></a>Diskprestanda

Prestanda för dina diskar beror på vilken typ av disk som du har valt. Varje disk klassificeras enligt antal I/O-åtgärder per sekund eller IOPS (uttalas ”aj-åps”). Dessutom har varje disk ett värde för dataflöde, vilket avgör hur mycket data du kan läsa eller skriva per sekund. Kombinationen av dessa två bestämmer hur snabb disken är.

Med standardlagring, får du maximalt **500 IOPS och 60 MB per sekund** dataflöde per disk (även på SSD-enheter). Med premiumlagring beror IOPS på vilka premiumdiskar du har valt samt storleken på den virtuella datorn.

|  | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
|--|----|----|-----|-----|-----|-----|-----|
| **Diskstorlek** | 32 GiB | 64 GiB | 128 GiB | 512 GiB | 1 TiB | 2 TiB | 4 TiB |
| **Max-IOPS per disk** | 120 | 240 | 500 | 2 300 | 5 000 | 7 500 | 7 500 |
| **Maxdataflöde per disk** | 25 MB/sek. | 50 MB/sek. | 100 MB/sek. | 200 MB/sek. | 250 MB/sek. | 250 MB/sek. |

Som du ser kan du gå från **25 MB/sek** och **120 IOPS** till **250 MB/sek** och **7500 IOPS**.