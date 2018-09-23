Varje system, arkitektur och program måste utformas med säkerhet i åtanke. Det är helt enkelt för mycket som riskeras. En DoS-angrepp hindrar dig till exempel från att göra affärer. Vandalisering av din webbplats skadar ditt rykte. Och en dataöverträdelse är kanske det värsta av alla &mdash; eftersom det kan förstöra svårförtjänat förtroende, samtidigt som det orsakar betydande personlig och ekonomisk skada. Som administratörer, utvecklare och IT-chefer måste vi alla arbeta för att garantera säkerheten i våra system.

Säg att du arbetar på ett företag som heter Contoso Shipping och du driver utvecklingen av drönarleveranser på landsbygden – samtidigt som du har lastbilschaufförer med mobilappar i stadsområden. Du håller på att flytta en massa av deras infrastruktur till molnet för att maximera effektiviteten, samt flytta flera fysiska servrar i deras datacenter till virtuella Azure-datorer. Ditt team planerar att skapa en hybridlösning med några lokala servrar kvar så de vill ha en säker högkvalitetsanslutning mellan de nya virtuella datorerna och det befintliga nätverket.

![Koncept med att säkra molnet i ett lokalt nätverk](../media/1-heading.png)

Dessutom har Contoso Shipping vissa enheter utanför nätverket som utgör en del din verksamhet. Du använder nätverksaktiverad sensorer i dina drönare som skickar data till Azure Event Hubs, medan chaufförerna använder sig av mobilappar för att hämta vägenkartor maps och registrera signaturer för mottagande av leveranser. Dessa enheter och appar måste autentiseras på ett säkert sätt innan data kan skickas till eller från dem.

Så hur håller du dina data säkra?

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att lära dig hur:

- säkerhetsansvar delas med Azure
- identitetshantering ger skydd även utanför ditt nätverk
- krypteringsfunktioner som är inbyggda i Azure kan skydda dina data
- du skyddar ditt nätverk och virtuella nätverk

## <a name="prerequisites"></a>Krav  

Ingen