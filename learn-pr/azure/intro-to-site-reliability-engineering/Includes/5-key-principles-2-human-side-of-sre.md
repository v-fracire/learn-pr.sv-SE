En process för lyckade åtgärder där du uppnår önskad tillförlitlighet och behåller den beror lika mycket på hur vi behandlar datorerna som hur vi behandlar människorna som ansvarar för miljön. Site Reliability Engineering bevisar den här sanningen på flera olika sätt som är avgörande för sin praxis.

## <a name="toil"></a>Enahanda arbete

Först är fokus på begreppet ”enahanda arbete”. I en SRE-kontext avser enahanda arbete uppgifter som görs av en person och som har vissa egenskaper. Enahanda arbete har inget långvarigt inlösande värde. Det tar inte tjänsten framåt på något meningsfullt sätt. Det är ofta återkommande och huvudsakligen manuellt (även om det kan automatiseras). När tjänsten eller system blir större över tid ökar förmodligen också antalet begäranden för systemet i antal med en proportionell hastighet och kräva ännu mer manuellt arbete.

Om en tjänst exempelvis kräver att SRE-teamet återställer något varje vecka eller att det tillhandahåller nya konton och diskutrymme för hand, eller startar om det upprepade gånger för hand är detta en driftsbelastning som är enahanda arbete. Genomförandet av de här åtgärderna har inte gjort tjänsten bättre på något långsiktigt, beständigt sätt. Dessa åtgärder måste förmodligen upprepas flera gånger.

> [!NOTE]
> Även om du behåller förfrågningar av den här typen i någon form av biljettsystem som många ställen gör är fortfarande utförandet av åtgärden och hur du löser ett ärende fortfarande enahanda arbete. Det är bara väl spårat enahanda arbete.

SRE:er hatar enahanda arbete. De arbetar för att ta bort det när det är möjligt och lämpligt. Det här är en av de platser där automatisering blir aktuellt i SRE. Om dessa begäranden kan hanteras automatiskt frigör det teamet så att de kan arbeta med mer tillfredsställande och effektfulla saker än att tömma kön med begäranden.

Det bör nämnas att användningen av ordet ”lämplig” här liknar dess användning när det gäller tillförlitlighet. Det finns situationer där eliminering av enahanda arbete har lägre prioritet än annat arbete, men på det stora hela är det viktigt att ta bort enahanda arbete från en tjänst.

## <a name="project-work-vs-reactive-ops-work"></a>Projektarbete kontra reaktivt driftsarbete

Om du vill göra arbetet som krävs för att ta bort enahanda arbete eller öka tillförlitligheten för ett system måste en SRE-tid allokeras så att de inte ägnar all sin tid åt att släcka bränder, svara på sökningar eller bara bearbeta en biljettkö. De måste ha avsatt tid för att skriva kod för att eliminera enahanda arbete, konstruera automatisering med självbetjäning så att biljetter inte är nödvändiga samt skapa projekt som gör tjänsten och personerna mer effektiva. Den allmänna bilden (som kommer från den ursprungliga Google-modellen) är en driftsbelastning som inte överstiger 50 % för ett team.

> [!NOTE]
> 50 % är ett något godtyckligt tal, men i praktiken verkar det fungera som ett rimligt mål för många användare.

Det finns en tid under SRE-cykeln när all tid ägnas åt att släcka bränder, men det kan inte vara ett ständigt tillstånd. Om ett teams reaktiva ”åtgärder” (mycket av det är enahanda arbete) tar upp mer än 50 % av tiden under en längre period är det ett recept på kollaps och dålig tillförlitlighet. I så fall kan de positiva cykler vi beskriv tidigare inte fungera eller konstrueras. SRE fokuserar även på att felaktigt fördelad jourbelastning eftersom det kan ha en mycket stark negativ inverkan på teamet.

Nu när vi har haft möjlighet att se några av de främsta metoderna och principerna för SRE kan vi prata lite mer om hur du kommer igång.
