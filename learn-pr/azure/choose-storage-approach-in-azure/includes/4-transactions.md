Program kan behöva gruppera en serie datauppdateringar, eftersom en ändring av en dataenhet måste resultera i en ändring till en annan typ av data. Med transaktioner kan du gruppera de här uppdateringarna. Om en uppdatering i serien misslyckas kan du då ångra eller återställa hela serien. 

Som en näthandelsföretagen kan du använda en transaktion för placering av en order och betalning-autentisering. När du grupperar de här relaterade händelserna ser du till att inte lagernivån minskas förrän du har tagit emot en godkänd form av betalning.

Här kan lära du dig om transaktioner och oavsett om de är nödvändiga för dina data.

## <a name="what-is-a-transaction"></a>Vad är en transaktion?

En transaktion är en logisk enhet som körs oberoende när data ska hämtas eller uppdateras.

Här är frågan till fråga dig själv om om du behöver använda transaktioner i ditt program: en ändring av en typ av data i datauppsättningen påverkar en annan? Om svaret är Ja, du behöver stöd för transaktioner i din databastjänst.

Transaktioner definieras ofta i termer av fyra olika krav, som kallas för ACID-garantier. ACID står för atomicitet, konsekvens, isolering och varaktighet:

- Atomicitet innebär att alla data har uppdaterats eller alla data som återställs till sitt ursprungliga tillstånd.
- Konsekvens säkerställer att om något händer delvis via transaktionen inte är en del av data tas bort från uppdateringarna. I alla gäller transaktionen konsekvent eller inte alls.
- Isolering innebär att en transaktion inte påverkas av en annan transaktion.
- Varaktighet innebär att ändringar som görs på grund av transaktionen sparas permanent i systemet. Allokerade data sparas av systemet så att även i händelse av fel och systemet startas om, data är tillgängliga i rätt tillstånd.

När en databas erbjuder ACID-garantier, tillämpas dessa principer alla transaktioner på ett konsekvent sätt.

## <a name="oltp-vs-olap"></a>OLTP eller OLAP

Transaktionsdatabaser kallas ofta för OLTP-system (Online Transaction Processing). OLTP-system stöder ofta många användare, har snabba svarstider och hanterar stora mängder data. De har även hög tillgänglighet (dvs. de har mycket minimal avbrottstid) och hanterar vanligtvis små eller relativt enkla transaktioner.

OLAP-system (Online Analytical Processing) däremot har normalt stöd för färre användare, har längre svarstider, kan vara mindre tillgängliga och hanterar ofta stora och komplexa transaktioner.

Villkoren OLTP och OLAP används inte så ofta som de används för att vara, men förstår dem gör det enklare att kategorisera programmets behov. 

Nu när du är bekant med transaktioner, OLTP och OLAP, ska vi gå igenom var och en av de uppgifter som i onlinebutiker scenariot och avgör om du behöver för transaktioner.

## <a name="product-catalog-data"></a>Data i produktkatalogen

Data i produktkatalogen bör lagras i en transaktionsdatabas. När användare lägger en order och betalningen har verifierats artikelns lagerstatus uppdateras. Om kundens kreditkort nekas ska ordern återställas och lagerstatusen ska inte uppdateras. För de här relationerna bör du använda transaktioner.

## <a name="photos-and-videos"></a>Foton och videor

Foton och videoklipp i en produktkatalog kräver inte transaktionell. Dessa filer ändras endast när en uppdatering görs eller nya filer har lagts till. Även om det finns en relation mellan bilden och faktiska produktdata är inte relationen transaktionsbaserad.

## <a name="business-data"></a>Affärsdata

För affärsdata, eftersom alla data är historiska och oföränderlig, krävs transaktionell inte. Affärsanalytiker som arbetar med data har också unika behov eftersom de normalt behöver arbeta med aggregeringar i sina frågor, så att de kan hantera summor snarare än mindre datapunkter.

## <a name="summary"></a>Sammanfattning

Det kan vara svårt att se till att dina data har rätt tillstånd. Du kan använda transaktioner till att garantera dataintegriteten. Om dina data förmåner från ACID principer, väljer du en lagringslösning som stöder transaktioner.