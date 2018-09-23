Din vårdorganisationen lagrar personlig och potentiellt känslig kundinformation. En säkerhetsincident kan avslöja känsliga data, vilket kan orsaka personliga svårigheter eller ekonomisk skada. Hur säkerställer du dataintegriteten och ser till att dina system är säkra? 

Här talar vi om hur du hanterar säkerheten för en arkitektur.

## <a name="what-should-i-protect"></a>Vad ska jag skydda?

De data din organisation lagrar eller hanterar är hjärtat i din skyddbara tillgångar. Dessa data kan vara känslig information om kunder, ekonomisk information om din organisation eller kritiska affärsdata som stödjer organisationen. Tillsammans med data är det även avgörande att skydda infrastrukturen som data finns på och de identiteter som används för att få åtkomst till dem.

Dina data kan omfattas av ytterligare juridiska och bestämmelsemässiga krav beroende på var du finns, vilken typ av data du lagrar eller vilken bransch som ditt program verkar i. Inom sjukvården i USA finns det till exempel en lag som kallas HIPAA (Health Insurance Portability and Accountability Act). I finansbranschen handlar Payment Card Industry Data Security Standard om hanteringen av kreditkortsuppgifter. Organisationer som lagrar data som omfattas av de här lagarna och standarderna måste se till att skyddsmått finns på plats för att skydda dessa data. I Europa anger den allmänna dataskyddsförordningen, GDPR, reglerna för hur personuppgifter skyddas och definierar personers rättigheter som rör lagrade data. Vissa länder kräver att vissa typer av data inte lämnar landets gränser.

När ett intrång sker kan det leda till betydande effekter för ekonomin och ryktet för både organisationer och kunder. Det bryter ned det förtroende som kunderna har för din organisation och kan påverka tillståndet på lång sikt.

## <a name="defense-in-depth"></a>Skydd på djupet

En metod med flera lager för att säkra miljön ökar dess säkerhetsposition. I det som ofta kallas _skydd på djupet_ kan vi dela upp lagren på följande sätt:

* Data
* Program
* VM/beräkning
* Nätverk
* Perimeternätverk
* Principer och åtkomst
* Fysisk säkerhet

Varje lager fokuserar på ett område där attacker kan ske och skapar ett djupgående skydd, om ett lager misslyckas eller kringgås av angriparen. Om vi bara skulle fokusera på ett lager skulle angriparna ha ohämmad åtkomst till din miljö om de tar sig igenom det lagret. Att hantera säkerheten i flera lager ökar arbetet som en angripare måste utföra för att få åtkomst till dina system och data. Varje lager skulle ha olika säkerhetskontroller, tekniker och funktioner som gäller. Vid identifiering av vilka skydd som ska tillämpas tas det ofta hänsyn till kostnaden, som måste balanseras mot affärskraven och den övergripande risken för företaget.

![En bild som visar djupskyddet med Data i mitten. Säkerhetsringarna kring dessa data är: program, beräkning, nätverk, perimeter, identitet och åtkomst samt fysisk säkerhet.](../media/security-layers.png)

Det finns inga enskilda system, kontroller eller tekniker som skyddar din arkitektur fullt ut. Säkerhet handlar inte bara om teknik, utan även om personer och processer. Att skapa en miljö som ser holistiskt på säkerhet och gör det till ett krav som standard hjälper till att skydda din organisation så mycket som möjligt.

## <a name="common-attacks"></a>Vanliga attacker

I varje lager finns det några vanligt förekommande attacker som du vill skydda dig mot. Dessa är inte heltäckande men kan ge dig en uppfattning om hur varje lager kan angripas och vilka typer av skydd du kan behöva titta på.

* **Datalager**: Exponering av krypteringsnyckeln eller användning av en svag kryptering kan göra dina data sårbara för obehörig åtkomst.
* **Programlager**: Inmatning och körning av skadlig kod är kännetecknande för attacker på programlagret. Vanliga attacker är SQL-inmatning och skriptkörning över flera webbplatser (XSS).
* **VM/beräkning-nivå**: Skadlig kod är en vanlig metod för att angripa en miljö, vilket inbegriper körning av skadlig kod för att kompromettera ett system. När den skadliga koden finns i systemet leder vidare attacker till avslöjande av autentiseringsuppgifter och laterala rörelser i hela miljö kan förekomma.
* **Nätverksnivå**: Onödiga öppna portar till internet är en vanlig attackmetod. Det kan vara att lämna SSH eller RDP öppet för virtuella datorer. När dessa är öppna kan det ske råstyrkeattacker mot dina system när angripare försöker få åtkomst.
* **Perimeternivå**: DoS-attacker sker ofta på den här nivån. Dessa attacker försöker överbelasta nätverksresurserna och tvinga dem att koppa från eller göra dem oförmögna att svara på legitima begäranden.
* **Princip- och åtkomstnivå**: Här sker autentiseringen för ditt program. Det kan inkludera moderna autentiseringsprotokoll som OpenID Connect, OAuth eller Kerberos-baserad autentisering som Active Directory. Exponerade autentiseringsuppgifter är en risk här och det är viktigt att begränsa behörigheterna för identiteter. Vi vill även ha övervakning på plats för att leta efter möjliga kapade konton, till exempel inloggningar från ovanliga platser.
* **Fysisk nivå**: Obehörig åtkomst till resurser genom metoder som ”door drafting” och stöld av säkerhetsmärken kan ske på den här nivån.

## <a name="shared-security-responsibility"></a>Delat säkerhetsansvar

När vi går tillbaka till modellen för delat ansvar kan vi ändra detta för säkerheten. Beroende på vilken typ av tjänst som du har valt kan vissa säkerhetsskydd byggas in i tjänsten, medan andra förblir ditt ansvar. Det är viktigt att du noga utvärderar de tjänster och tekniker som du väljer om du vill säkerställa att du har säkerhetskontroller som passar för din arkitektur.

![En bild som visar hur molnleverantörer och kunder delar ansvarsområden under olika typer av molntjänstodeller: lokalt, infrastruktur som en tjänst, plattform som en tjänst och programvara som en tjänst. ](../media/shared_responsibilities.png)
