Nu ska vi tillämpa en nätverkssäkerhetsgrupp på nätverket så att vi bara tillåter HTTP-trafik genom servern.

## <a name="create-a-network-security-group"></a>Skapa en nätverkssäkerhetsgrupp

Azure bör ha skapat en säkerhetsgrupp åt oss eftersom vi angav att vi ville ha SSH-åtkomst. Men vi ska ändå skapa en ny säkerhetsgrupp så att du ser hur processen ser ut. Detta är särskilt viktigt om du bestämmer dig för att skapa det virtuella nätverket _innan_ de virtuella datorerna. Som vi nämnt tidigare är säkerhetsgrupper _valfria_ och skapas inte nödvändigtvis med nätverket.

1. Skapa en ny resurs genom att klicka på knappen **Skapa en resurs** på den vänstra sidopanelen på [Azure Portal](https://portal.azure.com?azure-portal=true).

1. Skriv ”Nätverkssäkerhetsgrupp” i filterrutan och välj det matchande objektet i listan.

1. Kontrollera att distributionsmodellen **Resource Manager** är vald och klicka på **Skapa**.

1. Ange ett **namn** för din säkerhetsgrupp. Namngivningskonventioner är som sagt en bra idé här, vi använder "test-web-eus-nsg1" för "Test Web Network Security Group #1 in East US". Du vill troligen ändra platsdelen så att den speglar var du placerar säkerhetsgruppen.

1. Välj rätt **prenumeration** och använd din befintliga **resursgrupp**.

1. Placera den sedan på samma **plats** som den virtuella datorn/det virtuella nätverket. Obs! Du kan inte använda den här resursen om den finns på en annan plats.

1. Skapa gruppen genom att klicka på **Skapa**.

## <a name="add-a-new-inbound-rule-to-our-network-security-group"></a>Lägga till en ny inkommande regel i nätverkssäkerhetsgruppen

Distributionen bör slutföras snabbt, och när den är klar kan vi lägga till nya regler i säkerhetsgruppen.

1. Leta upp den nya säkerhetsgruppsresursen och välj den på Azure Portal.

1. På översiktssidan ser du att den har några standardregler som låser nätverket.

    På sidan för inkommande trafik:

    - All inkommande trafik från ett virtuellt nätverk till ett annat tillåts. På så sätt kan resurser i det virtuella nätverket prata med varandra
    - Azure Load Balancer ”avsöker” begäranden för att säkerställa att den virtuella datorn är aktiv
    - All annan inkommande trafik nekas på den utgående sidan:
    - All nätverkstrafik i det virtuella nätverket tillåts
    - All utgående trafik till Internet tillåts
    - All annan utgående trafik nekas

> [!NOTE]
> Dessa standardregler är inställda med höga prioritetsvärden, vilket innebär att de utvärderas _sist_. De kan inte ändras eller tas bort, men du kan _åsidosätta_ dem genom att skapa mer specifika regler som matchar din trafik med ett lägre prioritetsvärde.

1. Klicka på avsnittet **Inkommande säkerhetsregler** på panelen **Inställningar** för säkerhetsgruppen.

1. Lägg till en ny säkerhetsregel genom att klicka på **+ Lägg till**.

    ![Lägg till en säkerhetsregel](../media-drafts/8-add-rule.png)

    Du kan ange informationen som behövs för en säkerhetsregel med någon av två metoder: grundläggande och avancerat. Du kan växla mellan dem genom att klicka på knappen längst upp på panelen ”Lägg till”.

    ![Grundläggande och avancerad regelinmatning](../media-drafts/8-advanced-create-rule.png)

    I avancerat läge kan du anpassa regeln helt, men om du behöver konfigurera ett känt protokoll är det lite enklare att arbeta i grundläggande läge.

1. Växla till grundläggande läge.

1. Lägg till informationen för vår HTTP-regel.

    - Ange HTTP för **Tjänst**. När du gör det anges portintervallet åt dig.
    - Ange ”1000” för **Prioritet**. Det måste vara ett lägre värde än för standardregeln **Neka**. Du kan starta intervallet vid valfritt värde, men vi rekommenderar att du lägger till marginaler om ett undantag måste skapas senare.
    - Ge regeln ett namn, vi använder "allow-http-traffic".
    - Ge regeln en beskrivning.

1. Gå tillbaka till läget **Avancerat**. Observera att inställningarna finns kvar. Vi kan använda den här panelen för att skapa mer detaljerade inställningar. Mer specifikt vill vi antagligen begränsa **Källa** till en bestämd IP-adress eller ett intervall med IP-adresser som är specifika för kamerorna. Om du känner till den aktuella IP-adressen för den lokala datorn kan du prova med den. Annars lämnar du inställningen som ”Alla” så att du kan testa regeln.

1. Klicka på **Lägg till** för att skapa regeln. När du gör det uppdateras listan över regler för inkommande trafik – observera att de visas i prioritetsordning, vilket är den ordning som de kommer att granskas i.
    
## <a name="apply-the-security-group"></a>Tillämpa säkerhetsgruppen

Som du kanske minns kan vi tillämpa säkerhetsgruppen på ett nätverksgränssnitt för att skydda en enskild virtuell dator, eller på ett undernät där den i så fall gäller för alla resurser i undernätet. Den sista metoden är den vanligaste, så vi använder den. Vi kan komma till den här resursen i Azure antingen via den virtuella nätverksresursen eller indirekt via den virtuella datorn som använder det virtuella nätverket.

1. Växla till panelen **Översikt** för den virtuella datorn. Du hittar den virtuella datorn under **Alla resurser**.

1. Välj objektet **Nätverk** i avsnittet **Inställningar**.

    ![Nätverksobjekt i inställningarna för virtuella datorer](../media-drafts/8-network-settings.png)

1. I nätverksegenskaperna hittar du information om nätverksinställningarna som används för den virtuella datorn, inklusive det **virtuella nätverket/undernätet**. Det här är en klickbar länk till resursen. Klicka på den för att öppna det virtuella nätverket. Den här länken är _även_ tillgänglig på panelen **Översikt** för den virtuella datorn. Båda länkarna öppnar en **översikt** över det virtuella nätverket.

1. Välj objektet **Undernät** i avsnittet **Inställningar**.

1. Vi bör redan ha ett undernät (standard) sedan tidigare då vi skapade den virtuella datorn och nätverket. Klicka på objektet i listan för att öppna informationen.

1. Klicka på posten **Nätverkssäkerhetsgrupp**.

1. Välj den nya säkerhetsgruppen: **test-web-eus-nsg1**. Det bör också finnas en annan grupp här, som skapades med den virtuella datorn.

1. Spara ändringen genom att klicka på **Spara**. Det tar någon minut att tillämpa ändringen i nätverket.

## <a name="verify-the-rules"></a>Verifiera reglerna

Nu ska vi bekräfta ändringen.

1. Växla tillbaka till panelen **Översikt** för den virtuella datorn. Du hittar den virtuella datorn under **Alla resurser**.

1. Välj objektet **Nätverk** i avsnittet **Inställningar**.

1. På panelen **Översikt** för nätverket finns en länk för **Gällande säkerhetsregler** som snabbt visar hur regler utvärderas. Klicka på länken för att öppna analysen och kontrollera att du ser din nya regel.

    ![Gällande säkerhetsregler för vårt nätverk](../media-drafts/8-effective-rules.png)

1. Det bästa sättet att verifiera att allt fungerar är såklart att skicka en HTTP-begäran till servern. Det bör fortfarande fungera. Du kan även ta bort **HTTP**-regeln för att kontrollera att reglerna för nätverkssäkerhetsgrupper nu används.

## <a name="one-more-thing"></a>En till sak

Det kan vara knepigt att få till säkerhetsreglerna så att de blir rätt. Vi gjorde ett misstag när vi tillämpade den här nya säkerhetsgruppen – vi förlorar vår SSH-åtkomst! För att åtgärda det här problemet kan du lägga till en annan regel till säkerhetsgruppen som ger stöd för SSH-åtkomst. Se till att begränsa de inkommande TCP/IP-adresserna för regeln till de som du äger.

> [!WARNING]
> Var alltid noga med att låsa portar som används för administrativ åtkomst. En ännu bättre metod är att skapa ett VPN för att länka det virtuella nätverket till ditt privata nätverk och endast tillåta RDP- eller SSH-begäranden från det adressintervallet. Du kan också ändra den port som används av SSH till något annat än standardvärdet. Tänk på att det inte räcker att ändra portarna för att stoppa attacker, det gör det bara lite svårare att upptäcka dem.