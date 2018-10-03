Innan vi utforskar några av de metoder som är associerade med SRE kan det vara bra att placera några av de idéer vi lärt oss i den föregående enheten i kontext. I den här korta delen får vi lite av historien bakom SRE och hur den relaterar till andra operationsåtgärder som du kanske är bekant med. Detta förbereder oss för större chans att lyckas senare eftersom dessa metoder kommer att klarna mer i kontexten. Dessutom, när dina vänner undrar ”hur skiljer sig SRE från...” har du ett svar att ge.

## <a name="history"></a>Historik

En mycket komprimerad historik över SRE börjar med dess ursprung på Google 2003. Ben Treynor, nu Treynor Sloss, tog över ledarskapet för Googles produktionsteam (då bara sju programvarutekniker) och fick tanken som han beskrivit som ”vad händer när du ber en programvarutekniker att utforma en operationsfunktion”. Historiken här är bra för att öka förståelsen, eftersom den hjälper till att förklara varför operationspersoner som stöter på det här för första gången, kan uppfatta det som att SRE är starkt förknippat med det arbete programvarutekniker som utför. Det har hämtat värden och verktyg från det fältet, som vikten av att koda och källkontrollsystem som ett grundläggande verktyg. Den inledande och aktuella implementeringen av Google SRE är väldokumenterad i deras två böcker som publicerats av O'Reilly (se delen Komma igång).

När personer från Google lämnade företaget (och anställda vid företaget började prata om deras metoder mer offentligt), började SRE sprida sig till fler organisationer inom branschen. I takt med att SRE spred sig till nya organisationer, införlivade och implementerade dessa organisationer SRE-principer och metoder efter den lokala kulturen. Den här expansionsprocessen har resulterat i ett antal olika implementeringar av SRE i fältet. 

## <a name="devops-and-sre"></a>DevOps och SRE

Den större branschen står inför samma utmaningar när det gäller skalning, utvecklingshastighet kontra operativ stabilitet och andra problem med programvarudistribution som gav upphov till SRE-rörelsen. Parallella insatser för att bemöta dem utanför Google (och några få större företag då) gav upphov till DevOps. 

Du hittar mycket bra information om DevOps i [https://docs.microsoft.com/azure/devops/learn/](https://docs.microsoft.com/azure/devops/learn/).

> [!NOTE]
> Det är viktigt att notera att DevOps och SRE är två olika, parallella försök att lösa samma utmaningar. SRE är inte nästa evolutionära steg efter DevOps. SRE skapades inte för att vara ”det framtida DevOps”.

Hur SRE och DevOps skiljer sig åt är en fråga som fortfarande diskuteras i stor utsträckning inom fältet. Det finns vissa skillnader som man är överens om, bland annat:

- SRE är en teknikdisciplin som fokuserar på tillförlitlighet, medan DevOps är en kulturell rörelse som uppstod ur en vilja att dela upp silor som vanligen associerats med olika DevOps-organisationer.
- SRE kan vara namnet på en roll som i ”jag är en platstillförlitlighetstekniker”, men det kan inte DevOps vara. Ingen kan kallas sig för ”en DevOps”.
- SRE tenderar att vara mer regelstyrd, syftet med DevOps är att det inte ska vara det. Nästan universellt införande av kontinuerlig integrering/kontinuerlig leverans och flexibla principer är så nära de kommer varandra i det här avseendet.

De två disciplinerna, DevOps och SRE, delar en förkärlek för övervakning/observerande och automatisering (kanske av olika skäl). Det här är en anledning till varför det ofta kan vara enklare att införa SRE-metoder och principer för en organisation med en befintlig DevOps-rutin. Den här processen måste göras med försiktighet och avsikt. Dessutom kan och bör den implementeras stegvis, man behöver inte göra ett plötsligt byte.

> [!WARNING]
> Att bara byta titlar på personer inom organisationen är en implementeringsstrategi som nästan aldrig lyckas. Det ger inte de fördelar SRE har att erbjuda. Se avsnittet Komma igång i den här delen för några bättre förslag.

## <a name="conclusion"></a>Slutsats

Den här korta delen har försökt att ge lite kontext runt SRE och DevOps. SRE och DevOps är närliggande skolor och de bör betraktas som sådana. 

Nu när vi har tittat på en kort historik bakom SRE kan vi gå vidare till några av dess grundläggande principer.
