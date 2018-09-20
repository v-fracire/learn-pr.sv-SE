Licensiering är ett annat område som kan ha stor inverkan på dina utgifter. Nu ska vi titta på några sätt att sänka dina licenskostnader.

## <a name="azure-hybrid-benefit-for-windows-server"></a>Azure Hybrid-förmånen för Windows Server

Många kunder har investerat i Windows Server-licenser och vill kunna dra nytta av den här investeringen i Azure. Azure Hybrid-förmånen ger kunderna rätt att använda dessa licenser för virtuella datorer i Azure. Det innebär att du inte debiteras för Windows Server-licensen utan istället betalar Linux-priset.

För att få tillgång till den här förmånen måste dina Windows-licenser omfattas av Software Assurance. Följande riktlinjer gäller också:

- Varje licens för två processorer eller varje uppsättning licenser för 16 kärnor är berättigade till två instanser med upp till 8 kärnor eller en instans med upp till 16 kärnor.
- Standard Edition-licenser kan bara användas en gång, antingen lokalt eller i Azure. Det innebär att du inte kan använda samma licens för en virtuell Azure-dator och en lokal dator.
- Datacenter Edition-förmånen innebär att du kan använda licenser både lokalt och i Azure samtidigt, så licensen täcker två aktiva Windows-datorer.

> [!NOTE]
> De flesta av våra kunder använder licenser per kärna, så använd den modellen i dina beräkningar. Om du har frågor om vilka licenser du har kan du kontakta din återförsäljare för licenser eller ditt kontoteam hos Microsoft.

Det är enkelt att använda förmånen. Du kan aktivera och inaktivera den när som helst för befintliga virtuella datorer eller tillämpa den när nya virtuella datorer distribueras. Hybrid-förmånen (särskilt när den kombineras med reserverade instanser) kan ge betydligt lägre licensutgifter.

## <a name="azure-hybrid-benefit-for-sql-server"></a>Azure Hybrid-förmånen för SQL Server

Azure Hybrid-förmånen för SQL Server hjälper dig att få ut mesta möjliga värde från nuvarande licensinvesteringar och låter dig migrera till molnet snabbare. Azure Hybrid-förmånen för SQL Server är en Azure-baserad förmån som gör att du kan använda dina SQL Server-licenser med aktiv Software Assurance och då betala en lägre avgift.

Du kan använda den här förmånen även om Azure-resursen är aktiv, men det reducerade priset tillämpas från när du väljer det i portalen. Krediter utfärdas inte retroaktivt.

### <a name="azure-sql-database-vcore-based-options"></a>Alternativ för virtuella kärnor i Azure SQL Database

Azure Hybrid-förmånen fungerar så här för Azure SQL Database:

- Om du har Standard Edition-licenser per kärna med aktiv Software Assurance kan du få en virtuell kärna på servicenivån General Purpose för varje licenskärna du äger lokalt.
- Om du har Enterprise Edition-licenser per kärna med aktiv Software Assurance kan du få en virtuell kärna på servicenivån Business Critical för varje licenskärna du äger lokalt. Observera att Azure Hybrid-förmånen för SQL Server på servicenivån Business Critical bara är tillgänglig för kunder som har Enterprise Edition-licenser.
- Om du har starkt virtualiserade Enterprise Edition-licenser per kärna med aktiv Software Assurance kan du få fyra virtuella kärnor på servicenivån General Purpose för varje licenskärna du äger lokalt. Det här är en unik virtualiseringsförmån som endast är tillgänglig för Azure SQL Database.

Följande illustration visar de alternativ för virtuella kärnor som är tillgängliga på varje tjänstnivå med Azure Hybrid-förmånen för SQL Server-licenser.

![En illustration som visar ett exempel på hur du kan maximera värdet av din befintliga SQL Server-licens med hjälp av Azure Hybrid-förmånen.](../media/5-sql-tradein-value.png)

För SQL Server i Azure Virtual Machines fungerar Azure Hybrid-förmånen så här:

- Om du har Enterprise Edition-licenser per kärna med aktiv Software Assurance kan du få en kärna med SQL Server Enterprise Edition i Azure Virtual Machines för varje licenskärna du äger lokalt.
- Om du har Standard Edition-licenser per kärna med aktiv Software Assurance kan du få en kärna med SQL Server Standard Edition i Azure Virtual Machines för varje licenskärna du äger lokalt.

Det här kan ha stor inverkan på dina Azure-utgifter för SQL Server-arbetsbelastningar.

## <a name="use-devtest-subscription-offers"></a>Använda prenumerationserbjudanden för utveckling/testning

Erbjudandena [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/) och [Betala per användning – utveckling/testning](https://azure.microsoft.com/offers/ms-azr-0023p/) är en förmån som du kan använda för att spara pengar på miljöer som inte används i produktion. De här förmånerna ger dig flera rabatter, särskilt för Windows-arbetsbelastningar. Du betalar inga licensavgifter och faktureras endast Linux-priset för virtuella datorer. Det här gäller även SQL Server och andra Microsoft-program som täcks under en prenumeration på Visual Studio (kallades tidigare för MSDN). 

Det finns några krav för att den här förmånen ska gälla. Den gäller bara för arbetsbelastningar utanför produktion, och användare i de här miljöerna (förutom testare) måste täckas under en Visual Studio-prenumeration. För arbetsbelastningar utanför produktionsmiljö kan du alltså sänka dina kostnader för Windows, SQL Server och andra virtuella Microsoft-datorer.

Nedan kan du läsa fullständig information om respektive erbjudande. Om du är en kund med ett Enterprise-avtal skulle du använda erbjudandet Enterprise Dev/Test, och om du är en kund utan Enterprise-avtal som använder PAYG-konton skulle du använda erbjudandet Betala per användning – utveckling/testning.

## <a name="bring-your-own-sql-server-license"></a>Använd din egen SQL Server-licens

Om du är en kund med ett Enterprise-avtal och redan har investerat i SQL Server-licenser, som sedan har frigjorts när du flyttat resurser till Azure, kan du etablera **BYOL-avbildningar** (använd din egen licens) från Azure Marketplace. Då får du användning för dina licenser och minskar kostnaden för dina virtuella Azure-datorer. Du har alltid kunnat göra det här genom att etablera en virtuell Windows-dator och installera SQL Server manuellt, men när du använder certifierade avbildningar från Microsoft blir processen enklare. Sök efter **BYOL** på Marketplace om du vill hitta de här avbildningarna.

![Skärmbild av Azure-portalen som visar BYOL-alternativ för SQL Server.](../media/5-byol-sql-server.png)

> [!IMPORTANT]
> Du behöver en Enterprise Agreement-prenumeration för att använda de här certifierade BYOL-avbildningarna.

## <a name="use-sql-server-developer-edition"></a>Använda SQL Server Developer Edition

Det är många som inte känner till att SQL Server Developer Edition är en kostnadsfri produkt **när den inte används i produktion**. Developer Edition har samma funktioner som Enterprise Edition, men för miljöer utanför produktion. Det här gör att du kan sänka dina licenskostnader avsevärt.

Leta efter SQL Server-avbildningar för Developer Edition på Azure Marketplace och använd dem för utveckling eller testning. Då eliminerar du den extra kostnaden för SQL Server i de här fallen.

> [!TIP]
> Mer information finns i [dokumentationen om prissättning](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

## <a name="use-constrained-instance-sizes-for-database-workloads"></a>Använda begränsade instansstorlekar för databasarbetsbelastningar

Många kunder har stora krav på minne, lagring och I/O-bandbredd, men ett litet antal processorkärnor. På grund av detta så har Microsoft gjort de mest populära storlekarna (DS, ES, GS och MS) tillgängliga i nya storlekar där antalet kärnor begränsas till hälften eller en fjärdedel av den ursprungliga VM-storleken, men med samma mängd minne lagring och I/O-bandbredd.

| Storlek på virtuell dator | Virtuella processorer | Minne | Maximalt antal diskar | Maximalt I/O-dataflöde | Licenskostnad för SQL Server Enterprise per år | Total kostnad per år (databehandling och licenser) |
|---------|-------|--------|-----------|--------------------|-----------------------------------------------|---------------------------|
| Standard_DS14v2   | 16 | 112 GB | 32 | 51 200 IOPS eller 768 MB/s |           |           |
| Standard_DS14-4v2 | **4**  | 112 GB | 32 | 51 200 IOPS eller 768 MB/s | 75 % lägre | 57 % lägre |
| Standard_GS5      | 32 | 448    | 64 | 80 000 IOPS eller 2 MB/s   |           |           |
| Standard_GS5-8    | **8**  | 448    | 64 | 80 000 IOPS eller 2 MB/s   | 75 % lägre | 42 % lägre |

Eftersom databasprodukter som SQL Server och Oracle licensieras per processor så kan kunderna sänka sina licenskostnader med upp till 75 procent och samtidigt få tillgång till de databasprestanda som behövs.