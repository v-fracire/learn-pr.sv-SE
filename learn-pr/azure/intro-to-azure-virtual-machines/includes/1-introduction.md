Anta att du arbetar i IT-avdelningen på ett företag som sysslar med läkemedelsforskning. Du kan administrera en grupp med servrar som kör företags hela infrastruktur från webbservrar till databaser. Dock blir maskinvaran äldre och får kämpa för att hålla jämna steg med en del av de nya dataanalysprogram som distribueras till den.

Du skulle kunna uppgradera all maskinvara, men det är mindre fördelaktigt av flera skäl:

1. Servrarna är fysiskt utspridda över hela världen med minimal personal på varje plats. Vi vill centralisera uppgraderingen till vårt hemkontor.

1. Företaget körs anpassad programvara för dataanalys på flera versioner och varianter av Windows och Linux, ibland med udda konfigurationer som inte är helt dokumenterade. Vi behöver ett sätt att testa våra distributioner helt och prova olika konfigurationer för att kontrollera att allt fungerar innan vi överför arbetet.

1. Verksamheten frodas och företaget växer snabbt. Det är troligt att belastningen på de interna servrarna, särskilt databaserna, fortsätter att växa. Detta gör att vi antingen måste köpa nytt inför framtiden eller ta fram en skalningsplan för att hantera tillväxten.

På grund av de här anledningarna anser du att det är dags att utforska molnet och se om det kan lösa problemet med belastning och skala. Eftersom du har en mängd blandade servrar och anpassad programvara är det logiskt att försöka flytta servrarna en i taget till Azure med hjälp av virtuella Azure-datorer (VM).

Virtuella Azure-datorer är en av flera typer av skalbara datorresurser på begäran som Azure erbjuder. Med virtuella datorer får du fullständig kontroll över konfigurationen och kan installera allt du behöver för att utföra arbetet. Du behöver inte köpa fysisk maskinvara när du vill skala eller utöka datacentret. Azure tillhandahåller dessutom ytterligare tjänster för att övervaka, skydda och hantera uppdateringar och korrigeringar till operativsystemet.

Vi går igenom de beslut som fattas innan en virtuell dator skapas, alternativen för att skapa och hantera den virtuella datorn och de tillägg och tjänster du använder för att hantera den virtuella datorn.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Kompilera en checklista för att skapa en virtuell dator
- Beskriv alternativen för att skapa och hantera virtuella datorer
- Beskriv de ytterligare tjänster som är tillgängliga för att administrera virtuella datorer