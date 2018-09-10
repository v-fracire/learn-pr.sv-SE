I den här övningen använder du Azure-portalen för att skapa ett lagringskonto som är lämpligt för en fiktiv webbapp för surfrapporter i södra Kalifornien.

## <a name="design-goals"></a>Designmål

Webbplatsen med surfrapporter kan användas till att ladda upp foton och videoklipp som visar förhållandena på lokala surfplatser. Tittarna använder innehållet för att välja den strand som har de bästa surfvillkoren. Din lista över design- och funktionsmål är:

- Videoinnehåll måste läsas in snabbt
- Platsen måste kunna hantera oväntade toppar i uppladdningsvolym
- Inaktuellt innehåll tas bort allteftersom surfvillkoren ändras så att webbplatsen alltid visar aktuella villkor

Du väljer en implementering som buffrar uppladdat innehåll i en Azure-kö för bearbetning och sedan flyttar det till en Azure-blob för lagring. Du behöver ett lagringskonto som kan innehålla både köer och blobar samtidigt som det ger åtkomst med kort svarstid till ditt innehåll.

## <a name="exercise-steps"></a>Övningssteg

### <a name="launch-the-blade"></a>Starta bladet

1. I en webbläsare navigerar du till [Azure-portalen](https://portal.azure.com?azure-portal=true) och loggar in på ditt konto.

1. I sidofältet till vänster väljer du **Skapa en resurs**.

1. Välj rubriken **Lagring** på Azure Marketplace.

1. Välj **Lagringskonto**. Portalen visar bladet **Skapa lagringskonto**.

### <a name="configure-the-basic-options"></a>Konfigurera de grundläggande alternativen

1. Välj fliken **Grundläggande inställningar** längst upp på bladet.

1. **Prenumeration:** Välj en av dina prenumerationer.

1. **Resursgrupp**: Skapa en ny resursgrupp med namnet **SurfReportResourceGroup**.

1. **Namn på lagringskonto**: Ange ett globalt unikt värde som `surfreport` plus dina initialer plus en siffra.

 1. **Plats**: Välj **USA, västra**.

    Anledning: Programmet är avsett för användare i södra Kalifornien. För att minimera svarstiden vid inläsning av videor ska blobarna hanteras nära dessa användare. Detta gör **USA, västra** till ett bra alternativ.

1. **Distributionsmodell**: Välj **Resource Manager**.
    
    Anledning: **Resurshanterare** är lämplig eftersom den gör att du kan använda en resursgrupp för att hantera webbappen, lagringskontot osv. för programmet.

1. **Prestanda**: Välj **Standard**.

    Anledning: Du kan inte använda alternativet **Premium** eftersom det skulle begränsa lagringskontot sidblobar. Du behöver blockblobar för dina videor och en kö för buffring. Båda dessa är bara tillgängliga i alternativet **Standard**.

1. **Typ av konto**: Välj **StorageV2 (generell användning v2)**.

    Anledning: **StorageV2 (generell användning v2)** är det rätta valet i det här fallet. Du behöver en blandning av blobar och en kö, så alternativet **Blob-lagring** fungerar inte. För det här programmet skulle det inte finnas någon fördel med att välja ett konto för **Storage (generell användning v1)** eftersom det skulle begränsa de funktioner du kan komma åt och förmodligen inte skulle minska kostnaden för den förväntade arbetsbelastningen.

1. **Replikering**: Välj **Lokalt redundant lagring (LRS)**.

    Anledning: Bilderna och videorna blir snabbt inaktuella och tas bort från platsen. Det innebär att det inte finns någon större poäng med att betala extra för global redundans. I händelse av en katastrof som leder till dataförlust kan du starta om webbplatsen med nytt innehåll från användarna.

1. **Åtkomstnivå (Standard)**: Välj **Frekvent**.
   
    Anledning: Du vill att videor ska läsas in snabbt, och därför använder du alternativet med höga prestanda för dina blobar.
   
Följande skärmbild visar slutförda inställningar för fliken **Grundläggande inställningar**.

![Skärmbild av ett blad för att skapa ett lagringskonto med fliken **Grundläggande inställningar** markerad.](../media-drafts/5-create-storage-account-basics.png)

### <a name="configure-the-advanced-options"></a>Konfigurera avancerade alternativ

1. Välj fliken **Avancerat** längst upp på bladet.

1. **Säker överföring krävs**: Välj **Aktiverat**.

    Anledning: Https via kabeln anses allmänt vara bästa praxis.

1. **Virtuella nätverk**: Välj **Inaktiverat**.

    Anledning: Innehållet är offentligt, och du behöver tillåta åtkomst från offentliga klienter.

Följande skärmbild visar slutförda inställningar för fliken **Avancerat**.

![Skärmbild av ett blad för att skapa ett lagringskonto med fliken **Avancerat** markerad.](../media-drafts/5-create-storage-account-advanced.png)

### <a name="create"></a>Skapa

1. Klicka på knappen **Granska + skapa** längst ned på bladet.

1. På nästa skärm klickar du på knappen **Skapa** längst ned på bladet.

1. Vänta tills resursen skapas.

### <a name="verify"></a>Verifiera

1. Välj länken **Lagringskonton** i det vänstra sidofältet.

1. Leta upp det nya lagringskontot i listan för att verifiera skapandet.

### <a name="clean-up"></a>Rensa

1. Välj länken **Resursgrupper** i det vänstra sidofältet.

1. Leta upp **SurfReportResourceGroup** i listan.

1. Högerklicka på posten **SurfReportResourceGroup** och välj **Ta bort resursgrupp** på snabbmenyn.

1. Ange resursgruppens namn i bekräftelsefältet.

1. Klicka på knappen **Ta bort**.

## <a name="summary"></a>Sammanfattning

Du har skapat ett lagringskonto med inställningar som styrs av dina affärsbehov. Till exempel valde du ett datacenter i USA, västra eftersom dina kunder främst fanns i södra Kalifornien. Det här är ett typiskt flöde: först analyserar du dina data och mål och sedan konfigurerar du alternativen för lagringskonton så att de matchar.