Nu är din webbplats igång på Azure. Hur kan du se till att din webbplats alltid är igång?

Vad händer till exempel när du behöver utföra veckounderhåll? Tjänsten kommer fortfarande att vara otillgänglig under underhållsperioden. Och eftersom webbplatsen når användare över hela världen finns det ingen bra tidpunkt att ta ned systemen för underhåll. Du kan också stöta på prestandaproblem om för många användare ansluter samtidigt.

## <a name="what-are-availability-and-high-availability"></a>Vad är tillgänglighet och hög tillgänglighet?

_Tillgänglighet_ avser hur länge tjänsten är igång utan avbrott. _Hög_ _tillgänglighet_ avser en tjänst som är igång under lång tid.

Tänk på en webbplats för sociala medier eller nyheter som du besöker dagligen. Kommer du alltid åt webbplatsen eller visas ofta felmeddelanden, till exempel ”503 – tjänsten är inte tillgänglig”? Du vet hur frustrerande det är när du inte kommer åt den information du behöver.

Du kanske har hört termer som ”fem nines tillgänglighet”. ”Five nines” (fem nior) innebär att tjänsten garanterat är igång 99,999 procent av tiden. Det är svårt att uppnå 100 procent tillgänglighet, men många team strävar efter minst fem nior.

## <a name="what-is-resiliency"></a>Vad är återhämtning?

_Återhämtning_ avser ett systems förmåga att fortsätta fungera under avvikande förhållanden.

Exempel på sådana förhållanden:

- Naturkatastrofer.
- Systemunderhåll, både planerade och oplanerade, inklusive uppdateringar och säkerhetsuppdateringar.
- Toppar i trafiken på din webbplats.
- Hot som ställs av skadliga part, t.ex. datorn för tjänsten eller DDoS, attacker.

Föreställ dig marknadsavdelningen vill ha en utförsäljning att flytta upp en rad nya produkter. Du kan förvänta dig en hög topp i trafiken under den här tiden. Den här toppen kan överbelasta bearbetningssystemet, så att det blir långsamt eller stannar helt och dina användare blir besvikna. Du kan ha upplevt den här besvikelsen själv. Har du någonsin önskat biljetter för en händelse endast att hitta webbplatsen inte svarar?

## <a name="what-is-a-load-balancer"></a>Vad är en belastningsutjämnare?

En _belastningsutjämnare_ distribuerar trafik jämnt mellan alla system i en pool. Med hjälp av en belastningsutjämnare kan du uppnå både hög tillgänglighet och återhämtning.

Anta att du börjar med att lägga till ytterligare virtuella datorer, med identisk konfiguration, på varje nivå. Tanken är att ha ytterligare system som är redo, om en kraschar eller fungerar som värd för många användare samtidigt.

Problemet här är att varje virtuell dator har en egen IP-adress. Dessutom går det inte att distribuera trafik om ett system blir otillgängligt eller är upptaget. Hur kopplar du samman de virtuella datorerna så att de visas som ett enda system för användaren?

Lösningen är att använda en belastningsutjämnare för att distribuera trafik. Belastningsutjämnaren blir användarens startpunkt. Användaren vet inte (och behöver inte veta) vilket system belastningsutjämnaren väljer för att ta emot begäran.

Följande bild visar rollen för en belastningsutjämnare.

![En bild som visar webbnivån i en arkitektur med tre nivåer. Datanivån har flera virtuella datorer för att betjäna förfrågningar från användare. Det finns en belastningsutjämnare som distribuerar användarbegäranden mellan de virtuella datorerna.](../media/3-load-balancer.png)

Du ser att belastningsutjämnaren tar emot användarens begäran. Belastningsutjämnaren dirigerar begäran till någon av de virtuella datorerna på webbnivån. Om en virtuell dator inte är tillgänglig eller inte svarar slutar belastningsutjämnaren att skicka trafik till den. Belastningsutjämnaren dirigerar sedan trafik till en av de tillgängliga servrarna.

Med hjälp av belastningsutjämning kan du köra underhållsuppgifter utan att avbryta tjänsten. Du kan exempelvis sprida ut underhållsperioden för alla virtuella datorer. Underhållsfönster identifierar att den virtuella datorn inte svarar och dirigeras trafiken till andra virtuella datorer i poolen i belastningsutjämnaren.

För din näthandelswebbplats kan app- och datanivåerna också ha en belastningsutjämnare. Det beror helt på vad tjänsten kräver.

## <a name="what-is-azure-load-balancer"></a>Vad är Azure Load Balancer?

Azure Load Balancer är en tjänst för belastningsutjämning som Microsoft tillhandahåller.

Du kan manuellt konfigurera programvara för belastningsutjämning på en virtuell dator. Nackdelen är att du nu har ett ytterligare system som du behöver underhålla. Om belastningsutjämnaren blir otillgänglig eller kräver rutinunderhåll kommer du tillbaka till det ursprungliga problemet.

Du kan i stället använda Azure Load Balancer eftersom det finns ingen infrastruktur eller programvara att upprätthålla. Azure hand tar om underhåll åt dig.

Följande bild visar rollen för Azure-belastningsutjämnare i en arkitektur med flera nivåer.

![En bild som visar webbnivån i en arkitektur med tre nivåer. Datanivån har flera virtuella datorer för att betjäna förfrågningar från användare. Det finns en belastningsutjämnare som distribuerar användarbegäranden mellan de virtuella datorerna.](../media/3-azure-load-balancer.png)

## <a name="what-about-dns"></a>Hur är det med DNS?

DNS (Domain Name System) är ett sätt att mappa användarvänliga namn till deras IP-adresser. Du kan tänka dig DNS som telefonlistan i internet.

Domännamnet contoso.com kan till exempel mappas till belastningsutjämnarens IP-adress på webbnivå, 40.65.106.192.

Du kan ta med din egen DNS-server eller använda Azure DNS, en värdtjänst för DNS-domäner som körs på Azure-infrastruktur.

Följande bild visar Azure DNS. När användaren navigerar till contoso.com dirigerar Azure DNS trafik till belastningsutjämnaren.

![En bild som visar Azure domain name system placeras framför belastningsutjämnaren.](../media/3-dns.png)

## <a name="summary"></a>Sammanfattning

Med belastningsutjämning har din näthandelswebbplats nu högre tillgänglighet och återhämtning. När du utför underhåll eller när trafiken ökar tillfälligt kan belastningsutjämnaren distribuera trafik till ett annat tillgängligt system.

Du kan konfigurera en egen belastningsutjämnare på en virtuell dator, minskar Azure Load Balancer Underhåll eftersom det finns ingen infrastruktur eller programvara att underhålla.

DNS mappar användarvänliga namn till deras IP-adresser, ungefär som en telefonkatalog mappar namn på personer och företag till telefonnummer. Du kan ta med din egen DNS-server eller använda Azure DNS.
