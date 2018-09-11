Ett tjänstföretags framgång är ofta direkt relaterad till det serviceavtal företaget har med sina kunder. Kunderna förväntar sig att de tjänster du tillhandahåller alltid är tillgängliga och att deras data alltid är skyddade. Det här är något som Microsoft tar på stort allvar. Azure innehåller verktyg som du kan använda för att hantera tillgänglighet, datasäkerhet och övervakning, så att du vet att dina tjänster alltid är tillgängliga för dina kunder.

Administrationen av en Azure-dator är inte begränsad till att hantera operativsystem eller programvara som körs på den virtuella datorn. Det hjälper att känna till vilka tjänster i Azure som säkerställer tjänsternas tillgänglighet och som har stöd för automatisering. Dessa tjänster hjälper dig att planera organisationens strategi för affärskontinuitet och haveriberedskap.

I det här avsnittet beskriver vi Azure-tjänster som hjälper dig att förbättra tillgängligheten av virtuella datorer, effektivisera hanteringen av virtuella datorer och att säkerhetskopiera och skydda data på virtuella datorer. Vi börjar med att definiera vad tillgänglighet är.

## <a name="what-is-availability"></a>Vad är tillgänglighet?

Tillgänglighet är andelen tid i procent som en tjänst är tillgänglig för användning.

Anta att du har en webbplats och att du vill att kunderna ska kunna komma åt informationen hela tiden. Du förväntar dig 100 % tillgänglighet vad gäller åtkomsten till din webbplats.

### <a name="why-do-i-need-to-think-about-availability-when-using-azure"></a>Varför behöver jag bry mig om tillgänglighet när jag använder Azure?

Virtuella datorer i Azure körs på fysiska servrar som finns på Microsofts datacenter. Som med de flesta fysiska enheter finns det en risk att det uppstår ett fel. Om den fysiska servern slutar fungera, kommer även de virtuella datorer som körs på servern att sluta fungera. Om detta inträffar flyttar Azure automatiskt den virtuella datorn till en felfri värdserver. Den här självåterställande migreringen kan emellertid ta flera minuter, och under den perioden är programmet eller programmen som körs på den virtuella datorn inte tillgängliga.

De virtuella datorerna kan också påverkas av regelbundna uppdateringar som initieras av Azure. Dessa underhållshändelser kan gälla allt från programvaruuppdateringar till maskinvaruuppgraderingar och krävs för att förbättra plattformens tillförlitlighet och prestanda. Dessa händelser utförs vanligtvis utan att virtuella gästdatorer påverkas, men ibland behöver de virtuella datorerna startas om för att en uppdatering eller uppgradering ska slutföras.

> [!NOTE]
> Microsoft uppdaterar inte automatiskt operativsystem eller programvara på dina virtuella datorer. Du har fullständig kontroll och fullständigt ansvar för det. Den underliggande programvaruvärden och maskinvaran behöver dock korrigeras med jämna mellanrum för att säkerställa hög tillförlitlighet och höga prestanda hela tiden.

För att undvika tjänstavbrott och felkritiska systemdelar rekommenderar vi att du distribuerar minst två instanser av varje virtuell dator. Den här funktionen kallas för en _tillgänglighetsuppsättning_.

### <a name="what-is-an-availability-set"></a>Vad är en tillgänglighetsuppsättning?

En **tillgänglighetsuppsättning** är en logisk funktion som ser till att en grupp relaterade virtuella datorer distribueras så att inte alla slås ut på grund av en enda felkritisk systemdel och så att inte alla uppgraderas på samma gång när värdoperativsystemet uppgraderas på datacentret. Virtuella datorer i en tillgänglighetsuppsättning bör utföra en identisk uppsättning funktioner och ha samma programvara installerad.

> [!TIP]
> Microsoft erbjuder ett serviceavtal (SLA) med 99,95 % garanterad extern anslutning för virtuella datorer med flera instanser som distribuerats i en tillgänglighetsuppsättning. Det innebär att för att serviceavtalet ska gälla måste det finnas minst två instanser av den virtuella datorn i en tillgänglighetsuppsättning. 

Du kan skapa tillgänglighetsuppsättningar via Azure Portal i avsnittet om haveriberedskap. Du kan också skapa dem med hjälp av Resource Manager-mallar eller något av API- eller skriptverktygen. När du lägger till virtuella datorer i en tillgänglighetsuppsättning distribuerar Azure dem mellan **feldomäner** och **uppdateringsdomäner**.

#### <a name="what-is-a-fault-domain"></a>Vad är en feldomän?

En feldomän är en logisk grupp med maskinvara i Azure som delar samma strömkälla och nätverksswitch. Den kan liknas vid ett rack i ett lokalt datacenter. De två första virtuella datorerna i en tillgänglighetsuppsättning etableras i två olika rack så att bara en virtuell dator påverkas i händelse av ett fel i nätverket eller ett strömavbrott. Feldomäner definieras även för hanterade diskar som är anslutna till virtuella datorer.

![Feldomäner](../media-draft/5-fault-domains.png)

#### <a name="what-is-an-update-domain"></a>Vad är en uppdateringsdomän?

En uppdateringsdomän är en logisk grupp med maskinvara som kan underhållas eller startas om samtidigt. Azure placerar automatiskt tillgänglighetsuppsättningar i uppdateringsdomäner för att minimera effekten när Azure-plattformen introducerar värdoperativsystemsändringar. Azure bearbetar sedan varje uppdateringsdomän en i taget.

Tillgänglighetsuppsättningar är en kraftfull funktion som ser till att de tjänster som körs på dina virtuella datorer alltid är tillgängliga för dina kunder. De är dock inte felsäkra. Vad händer om det uppstår problem med data eller programvaran som körs på den virtuella datorn? För att svara på det måste vi titta på andra katastrofåterställnings- och säkerhetskopieringstekniker.

## <a name="failover-across-locations"></a>Redundansväxling mellan platser

Du kan också replikera din infrastruktur mellan platser för att hantera regional redundans. **Azure Site Recovery** (ASR) replikerar arbetsbelastningar från en primär plats till en sekundär plats. Om ett avbrott inträffar på den primära platsen kan du växla över till en sekundär plats. Tack vare den här redundansväxlingen kan användarna fortsätta att komma åt dina program utan avbrott. Du kan sedan växla tillbaka till den primära platsen när den är igång och körs igen. Azure Site Recovery handlar om replikering av virtuella eller fysiska datorer. Tjänsten ser till att dina arbetsbelastningar är tillgängliga i händelse av ett avbrott.

Det finns en rad intressanta tekniska funktioner i ASR, och när det gäller affärsfördelar sticker två viktiga funktioner ut:

1. Med ASR kan du använda Azure som mål för återställning, vilket eliminerar kostnaden och komplexiteten med att underhålla ett sekundärt fysiska datacenter.

2. ASR gör det otroligt enkelt att testa redundansen i samband med återställningstester utan att påverka produktionsmiljön. På så vis kan du enkelt testa planerade eller oplanerade redundansväxlingar. Hur vet du om din katastrofåterställningsplan fungerar om du aldrig har provat att redundansväxla?

De återställningsplaner som du skapar med ASR kan vara så enkla eller så komplexa som ditt scenario kräver. De kan innehålla anpassade PowerShell-skript, Azure Automation-runbooks eller manuella åtgärdssteg. Du kan använda återställningsplaner för att replikera arbetsbelastningar till Azure och enkelt dra nytta av nya möjligheter i fråga om migrering, tillfälliga trafiktoppar eller utveckling och testning av nya program.

Azure Site Recovery kan användas med Azure-resurser eller med Hyper-V-servrar, VMware-servrar och fysiska servrar i din lokala infrastruktur och kan utgöra en viktig del av din organisations strategi för affärskontinuitet och haveriberedskap (BCDR) genom att samordna replikering, redundans och återställning av arbetsbelastningar och program om den primära platsen slutar fungera.
