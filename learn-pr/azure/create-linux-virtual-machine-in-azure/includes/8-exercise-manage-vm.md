Nu ska vi tillämpa en nätverkssäkerhetsgrupp på nätverket så att vi bara tillåter HTTP-trafik genom servern.

## <a name="create-a-network-security-group"></a>Skapa en nätverkssäkerhetsgrupp

Azure bör ha skapat en säkerhetsgrupp åt oss eftersom vi angav att vi ville ha SSH-åtkomst. Men vi ska ändå skapa en ny säkerhetsgrupp så att du ser hur processen ser ut. Detta är särskilt viktigt om du bestämmer dig för att skapa det virtuella nätverket _innan_ de virtuella datorerna. Som vi nämnt tidigare är säkerhetsgrupper _valfria_ och skapas inte nödvändigtvis med nätverket.

1. Skapa en ny resurs genom att klicka på knappen **Skapa en resurs** i den vänstra sidopanelen i [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. Skriv **Nätverkssäkerhetsgrupp** i filterrutan och välj det matchande objektet i listan.

1. Kontrollera att distributionsmodellen **Resource Manager** är vald och klicka på **Skapa**.

1. Ange ett **namn** på säkerhetsgruppen. Återigen är namngivningskonventioner en bra idé. Vi använder **test-web-eus-nsg1** för **Test Web Network Security Group #1 in East US**. Du vill troligen ändra platsdelen i namnet så att den speglar var du placerar säkerhetsgruppen.

1. Välj rätt **prenumeration** och använd din befintliga **resursgrupp**.

1. Placera den sedan på samma **plats** som den virtuella datorn/det virtuella nätverket. Obs! Du kan inte använda den här resursen om den finns på en annan plats.

1. Skapa gruppen genom att klicka på **Skapa**.

## <a name="add-a-new-inbound-rule-to-our-network-security-group"></a>Lägg till en ny inkommande regel i nätverkssäkerhetsgruppen

Distributionen bör slutföras snabbt. När den är klar kan vi lägga till nya regler i säkerhetsgruppen:

1. Leta upp den nya säkerhetsgruppsresursen och välj den i Azure Portal.

1. På översiktssidan ser du att den har några standardregler som låser nätverket.

    På sidan för inkommande trafik:

    - All inkommande trafik från ett virtuellt nätverk till ett annat tillåts. På så sätt kan resurser i det virtuella nätverket prata med varandra.
    - Azure Load Balancer **avsöker** begäranden för att kontrollera att den virtuella datorn är aktiv.
    - All annan inkommande trafik nekas.

    På sidan för utgående trafik:
    - All inkommande nätverkstrafik i det virtuella nätverket tillåts.
    - All utgående trafik till Internet tillåts.
    - All annan utgående trafik nekas.

    > [!NOTE]
    > Dessa standardregler har höga prioritetsvärden, vilket innebär att de utvärderas _sist_. De kan inte ändras eller tas bort, men du kan _åsidosätta_ dem genom att skapa mer specifika regler som matchar din trafik med ett lägre prioritetsvärde.

1. Klicka på avsnittet **Inkommande säkerhetsregler** på panelen **Inställningar** för säkerhetsgruppen.

1. Lägg till en ny säkerhetsregel genom att klicka på **+ Lägg till**.

    ![Skärmbild av Azure Portal med inställningarna för inkommande säkerhetsregler med knappen Lägg till markerad.](../media/8-add-rule.png)

    Du kan ange informationen som behövs för en säkerhetsregel med någon av två metoder: grundläggande och avancerad. Du kan växla mellan dem genom att klicka på knappen längst upp på panelen **Lägg till**.

    ![Ett par skärmbilder i Azure Portal visar växlingen mellan grundläggande och avancerad regelinmatning, med en pil som länkar mellan de två lägena för växlingsknappen markerad.](../media/8-advanced-create-rule.png)

    Avancerat läge ger möjlighet att anpassa regeln helt. Om du vill konfigurera ett känt protokoll är dock ett grundläggande läge lite enklare att arbeta med.

1. Växla till det grundläggande läget.

1. Lägg till informationen för vår HTTP-regel:

    - Ange HTTP för **Tjänst**. När du gör det anges portintervallet åt dig.
    - Ange **Prioritet** till **1000**. Det måste vara ett lägre värde än för standardregeln **Neka**. Du kan starta intervallet med valfritt värde, men vi rekommenderar att du inte är för snäv om du skulle behöva skapa ett undantag senare.
    - Ge regeln ett namn. Vi använder **allow-http-traffic**.
    - Ge regeln en beskrivning.

1. Gå tillbaka till läget **Avancerat**. Observera att inställningarna finns kvar. Vi kan använda den här panelen för att skapa mer detaljerade inställningar. Mer specifikt vill vi antagligen begränsa **Källa** till en bestämd IP-adress eller ett intervall med IP-adresser som är specifika för kamerorna. Om du känner till den aktuella IP-adressen för den lokala datorn kan du prova med den. Annars lämnar du inställningen som **Alla** så att du kan testa regeln.

1. Klicka på **Lägg till** för att skapa regeln. När du gör det uppdateras listan över regler för inkommande trafik – observera att de visas i prioritetsordning, vilket är den ordning som de kommer att granskas i.

## <a name="apply-the-security-group"></a>Tillämpa säkerhetsgruppen

Som du kanske minns kan vi tillämpa säkerhetsgruppen på ett nätverksgränssnitt för att skydda en enskild virtuell dator, eller på ett undernät där den i så fall gäller för alla resurser i undernätet. Den sista metoden är den vanligaste, så vi använder den. Vi kan komma till den här resursen i Azure antingen via den virtuella nätverksresursen, eller indirekt via den virtuella datorn som använder det virtuella nätverket.

1. Växla till panelen **Översikt** för den virtuella datorn. Du hittar den virtuella datorn under **Alla resurser**.

1. Välj objektet **Nätverk** i avsnittet **Inställningar**.

1. I nätverksegenskaperna hittar du information om nätverksinställningarna som används för den virtuella datorn, inklusive **Virtuellt nätverk/undernät**. Det här är en klickbar länk till resursen. Klicka på den för att öppna det virtuella nätverket. Den här länken är _även_ tillgänglig på panelen **Översikt** för den virtuella datorn. Båda länkarna öppnar en **översikt** över det virtuella nätverket.

1. Välj objektet **Undernät** i avsnittet **Inställningar**.

1. Vi bör redan ha ett undernät (standard) sedan tidigare då vi skapade den virtuella datorn och nätverket. Klicka på objektet i listan för att öppna informationen.

1. Klicka på posten **Nätverkssäkerhetsgrupp**.

1. Välj den nya säkerhetsgruppen: **test-web-eus-nsg1**. Det bör också finnas en annan grupp här, som skapades med den virtuella datorn.

1. Spara ändringen genom att klicka på **Spara**. Det tar någon minut att tillämpa ändringen i nätverket.

## <a name="update-the-nsg-on-the-network-interface"></a>Uppdatera NSG:n på nätverksgränssnittet

Port 80 är öppen på NSG:n som används på undernätet, men den kommer fortfarande att blockeras eftersom den för tillfället inte tillåts på den NSG som används för nätverksgränssnittet. Vi åtgärdar det så ska vi kunna ansluta.

1. Växla tillbaka till panelen **Översikt** för den virtuella datorn. Du hittar den virtuella datorn under **Alla resurser**.

1. I avsnittet **Inställningar** väljer du objektet **Nätverk**.

1. I avsnittet **regler för inkommande portar** bör du se NSG-reglerna för undernätet och NSG-reglerna för nätverksgränssnittet under den. I NSG-reglerna för nätverksgränssnittet väljer du **Lägg till regel för inkommande portar**.

1. Växla till det grundläggande läget.

1. Lägg till informationen för vår HTTP-regel:

    - Ange HTTP för **Tjänst**. När du gör det anges portintervallet åt dig.
    - Ange **Prioritet** till **310**.
    - Ge regeln ett namn. Vi använder **allow-http-traffic**.
    - Ge regeln en beskrivning.

1. Klicka på **Lägg till** för att skapa regeln.

## <a name="verify-the-rules"></a>Verifiera reglerna

Nu ska vi bekräfta ändringen:

1. Växla tillbaka till panelen **Översikt** för den virtuella datorn. Du hittar den virtuella datorn under **Alla resurser**.

1. Välj objektet **Nätverk** i avsnittet **Inställningar**.

1. I nätverkgränssnittsinformationen så finns det en länk för **Gällande säkerhetsregler** som snabbt visar hur regler kommer att utvärderas. Klicka på länken för att öppna analysen och kontrollera att du ser dina nya regler.

    ![Skärmbild av Azure-portalen som visar bladet Gällande säkerhetsregler för vårt nätverk.](../media/8-effective-rules.png)

1. Det bästa sättet att verifiera att allt fungerar är såklart att skicka en HTTP-begäran till servern. Nu bör det fungera.

    ![Skärmbild av en webbläsare som visar Apaches standardwebbsida för IP-adressen till den nya virtuella Linux-datorn.](../media/6-apache-works.png)

## <a name="one-more-thing"></a>En sak till

Det kan vara knepigt att få till säkerhetsreglerna så att de blir rätt. Vi gjorde ett misstag när vi tillämpade den här nya säkerhetsgruppen – vi förlorar vår SSH-åtkomst! För att åtgärda det här problemet kan du lägga till en annan regel till säkerhetsgruppen som gäller för undernätet för att tillåta SSH-åtkomst. Se till att begränsa de inkommande TCP/IP-adresserna för regeln till de som du äger.

> [!WARNING]
> Var alltid noga med att låsa portar som används för administrativ åtkomst. En ännu bättre metod är att skapa ett VPN för att länka det virtuella nätverket till ditt privata nätverk och endast tillåta RDP- eller SSH-begäranden från det adressintervallet. Du kan också ändra den port som används av SSH till något annat än standardvärdet. Kom ihåg att det inte är tillräckligt att ändra portarna för att stoppa attacker. De blir bara lite svårare att upptäcka.