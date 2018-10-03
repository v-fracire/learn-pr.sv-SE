Som en sista enhet i den här modulen ska vi prata om hur du går vidare om du vill utforska SRE. 

## <a name="reading-and-watching"></a>Läsa och titta

Den bästa källan för mer detaljerad information om SRE är tre böcker som har publicerats om ämnet

1. [_Site Reliability Engineering: How Google Runs Production Systems_](http://shop.oreilly.com/product/0636920041528.do) (kallas för ”SRE-boken”)
1. [_The Site Reliability Workbook: Practical Ways to Implement SRE_](http://shop.oreilly.com/product/0636920132448.do) (kallas ”SRE-arbetsboken”)
1. [_Seeking SRE: Conversations About Running Production Systems at Scale_](http://shop.oreilly.com/product/0636920063964.do)

(Som en snabb upplysning är den primära författaren till den här modulen kurator/redaktör för den tredje boken)

Var och en av dessa böcker innehåller en viktig uppsättning information:

- SRE-boken – ger en detaljerad förklaring av hur Google implementerat SRE under åren.

- SRE-arbetsboken – en följeslagare till SRE-boken som ger en mer detaljerad förklaring av inte bara ”vad” för SRE på Google och några andra platser, utan även ”hur” och ”varför”.

- Seeking SRE – ger en mer omfattande vy över SRE-världen utöver dess ursprung, som information om hur den har implementerats i andra miljöer.

Läs de tre böckerna kritiskt. Allt som står i böckerna gäller inte för dig och din organisation. Ägna en stund åt att identifiera informationen som du är säker på kan ge dig positivt värde. Tänk på vilka delar av din organisations kultur och värden som kan ge stöd för SRE-arbete enligt beskrivningen och som kan göra det mer utmanande.

Om du föredrar det visuella kan du titta på föredraget [Keys to SRE](https://www.usenix.org/conference/srecon14/technical-sessions/presentation/keys-sre) av Ben Treynor på SREcon14-konferensen. Treynor ger en riktigt övertygande förklaring av vad han tror att SRE (åtminstone när det gäller Google) är. Andra inspelade föredrag om SRE från [den här konferensserien](https://www.usenix.org/conferences/byname/925) och andra kan också vara till nytta.

## <a name="talk-to-other-interested-people"></a>Prata med andra berörda personer

Det kan ofta vara viktigare att diskutera med kollegor än att läsa på om SRE. Att diskutera era utmaningar, framgångar och misslyckanden kring SRE kan vara avgörande för att få en nyanserade förståelse för ämnet. 

Det finns ett antal träffar och konferenser kring SRE-innehåll. Den som är mest direkt relevant är kanske de globalt distribuerade [SREcon-konferenserna](https://www.usenix.org/conferences/byname/925) på USENIX (friskrivning: modulens primära författare är en av grundarna av SREcon).

Mer och mer SRE-innehåll finns med på konferenser som [Velocity](https://conferences.oreilly.com/velocity), [LISA](https://www.usenix.org/conferences/byname/5) och lokala DevOps-konferenser som [DevOps Days](https://www.devopsdays.org). Sök upp det här innehållet och annat intressant där du hittar det.

## <a name="first-steps-at-work"></a>De första spadtagen

Om du vill börja utforska hur det skulle vara att introducera SRE i din miljö är det viktigt att komma ihåg att SRE inte är en ”allt eller inget”-lösning.  Du kan börja använda SRE-principer och -metoder i små steg.

Mikey Dickerson, en välkänd SRE från sitt arbete som skulle bli USDS (United States Digital Service) (som ansvarar för att spara healthcare.gov), har föreslagit en hierarki för tillförlitlighet som en motsvarighet till Maslows behovshierarki. Det citeras i [övningsavsnittet](https://landing.google.com/sre/book/chapters/part3.html) i den första SRE-boken.

Den här hierarkin föreslår att man först måste ha en fungerande och betrodd övervakning i miljön. Detta måste vara ett första steg mot SRE även för din miljö. Du kan inte avgöra om något är tillförlitligt (eller blir bättre eller sämre) om du inte kan mäta det.

När du har en övervakningsplattform som du kan lita på är nästa uppnåeliga steg att välja en tjänst och börja ha SLI- och SLO-samtal om den. Börja enkelt. Skapa servicenivåindikatorer (SLI) och servicenivåmål (SLO) för tjänsten, implementera dem i dina övervakningssystem och se vad som händer när du börjar uppmärksamma tillförlitlighet med SRE. Det här är ett bra ställe att börja på.
