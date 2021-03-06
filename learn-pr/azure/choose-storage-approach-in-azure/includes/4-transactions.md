Det kan ofta hända att du behöver gruppera en serie datauppdateringar eftersom en ändring kan leda till att en annan dataenhet också behöver ändras. Med transaktioner kan du gruppera de här uppdateringarna. Om en uppdatering i serien misslyckas kan du då ångra eller återställa hela serien. 

I en onlinebutik kan du till exempel använda en transaktion för processen att lägga en order och verifiera betalningen. När du grupperar de här relaterade händelserna ser du till att inte lagernivån minskas förrän du har tagit emot en godkänd form av betalning.

Här får du lära dig om transaktioner och när de är bra att använda för dina data.

## <a name="what-is-a-transaction"></a>Vad är en transaktion?

En transaktion är en logisk grupp av databasåtgärder som körs tillsammans.

När du ska avgöra om ditt program behöver använda transaktioner du fråga dig det här: Kommer uppdateringar påverka andra datapunkter i datamängden? Om svaret är Ja så behöver du transaktionsstöd i databastjänsten.

Transaktioner definieras ofta i termer av fyra olika krav, som kallas för ACID-garantier. ACID står för atomicitet, konsekvens, isolering och varaktighet:

- Atomicitet innebär att alla eller inga åtgärder i transaktionen körs.
- Konsekvens innebär att om det händer något medan transaktionen utförs så blir inte vissa data uppdaterade medan andra inte blir det. I alla fall gäller transaktionen konsekvent eller inte alls.
- Isolering innebär att en transaktion inte påverkas av en annan transaktion.
- Varaktighet innebär att ändringar som görs på grund av transaktionen sparas permanent i systemet. Bekräftade data sparas i systemet så att de är tillgängliga i korrekt tillstånd även om systemet måste startas om efter ett fel.

När en databas erbjuder ACID-garantier, tillämpas dessa principer konsekvent på alla transaktioner.

## <a name="oltp-vs-olap"></a>OLTP eller OLAP

Transaktionsdatabaser kallas ofta för OLTP-system (Online Transaction Processing). OLTP-system stöder ofta många användare, har snabba svarstider och hanterar stora mängder data. De har även hög tillgänglighet (dvs. de har mycket minimal avbrottstid) och hanterar vanligtvis små eller relativt enkla transaktioner.

OLAP-system (Online Analytical Processing) däremot har normalt stöd för färre användare, har längre svarstider, kan vara mindre tillgängliga och hanterar ofta stora och komplexa transaktioner.

Begreppen OLTP och OLAP används inte så ofta som förut, men jämförelsen gör det enklare att kategorisera behoven i din app. 

Nu när du bekantat dig med transaktioner, OLTP och OLAP ska vi gå igenom var och en av datauppsättningarna i scenariot med onlinebutiken och avgöra om du behöver använda transaktioner.

## <a name="product-catalog-data"></a>Data i produktkatalogen

Data i produktkatalogen bör lagras i en transaktionsdatabas. När användare lägger en order och betalningen har verifierats artikelns lagerstatus uppdateras. Om kundens kreditkort nekas ska ordern återställas och lagerstatusen ska inte uppdateras. För de här relationerna bör du använda transaktioner.

## <a name="photos-and-videos"></a>Foton och videor

Foton och videor i produktkatalogen behöver inget transaktionsstöd. Dessa filer ändras endast när en uppdatering sker eller nya filer har lagts till. Även om det finns en relation mellan bilden och faktiska produktdata är inte relationen transaktionsbaserad.

## <a name="business-data"></a>Affärsdata

Eftersom alla affärsdata är historiska och aldrig ändras krävs inget stöd för transaktioner. Affärsanalytiker som arbetar med data har också unika behov eftersom de normalt behöver arbeta med aggregeringar i sina frågor, så att de kan hantera summor snarare än mindre datapunkter.

## <a name="summary"></a>Sammanfattning

Det kan vara svårt att se till att dina data har rätt tillstånd. Du kan använda transaktioner till att garantera dataintegriteten. Om dina data skulle ha nytta av ACID-principerna bör du välja en lagringslösning med transaktionsstöd.