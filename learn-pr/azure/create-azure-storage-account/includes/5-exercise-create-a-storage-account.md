I den här kursdelen använder du Azure Portal för att skapa ett lagringskonto som är lämpligt för en fiktiv webbapp för surfrapporter i södra Kalifornien.

Webbplatsen med surfrapporter kan användas till att ladda upp foton och videoklipp som visar förhållandena på lokala surfplatser. Tittarna använder innehållet för att välja den strand som har de bästa surfvillkoren. Din lista över design- och funktionsmål är:

- Videoinnehåll måste läsas in snabbt.
- Platsen måste kunna hantera oväntade toppar i uppladdningsvolym.
- Inaktuellt innehåll tas bort allteftersom surfvillkoren ändras så att webbplatsen alltid visar aktuella villkor.

För att uppfylla dessa, kan du välja att buffra uppladdat innehåll i en Azure-kö för bearbetning och sedan överföra den till en Azure-blobb för beständig lagring. Du behöver ett lagringskonto som kan innehålla både köer och blobbar samtidigt som det ger åtkomst med kort svarstid till ditt innehåll.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="use-the-azure-portal-to-create-a-storage-account"></a>Använd Azure Portal för att skapa ett lagringskonto

1. Logga in på [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du har aktiverat sandbox-miljön med.

1. Välj **Skapa en resurs** uppe till vänster i Azure Portal.

1. Välj **Storage** i urvalsfönstret som visas.

1. Välj **Lagringskonto – blobb, fil, tabell, kö** till höger i fönstret.

    ![Skärmbild av Azure Portal som visar bladet Skapa en resurs med Storage-kategorin och alternativet för lagringskonto markerat.](..\media\5-portal-storage-select.png)

### <a name="configure-the-basic-options"></a>Konfigurera de grundläggande alternativen

[!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

Under **PROJEKTINFORMATION**:


1. Välj lämplig **prenumeration**.

1. Välj den befintliga resursgruppen (”**<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**”) från listrutan.

    > [!NOTE]
    > Den här kostnadsfria resursgruppen har angetts av Microsoft som en del av inlärningsupplevelsen. När du skapar ett konto för ett riktigt program, kommer du att skapa en ny resursgrupp i din prenumeration för att lagra alla resurser för appen.

Under **INSTANSINFORMATION**:


1. Ange ett **namn på lagringskontot**. Namnet används för att generera den offentliga webbadress som används för att få åtkomst till data på kontot. Namnet måste vara unikt för alla befintliga lagringskontonamn i Azure. Namnen måste vara 3 till 24 tecken långt och får bara innehålla gemena bokstäver och siffror.

1. Välj en **plats** nära dig från listan ovan.

1. Lämna **distributionsmodellen** som _resurshanterare_. Det här är den föredragna modellen för alla resursdistributioner i Azure och gör att du kan gruppera relaterade resurser för din app i en _resursgrupp_ för enklare hantering.

1. Välj _Standard_ för alternativet **Prestanda**. Detta avgör vilken typ av disklagring som används för att lagra data i lagringskontot. Standard använder traditionella hårddiskar och Premium använder solid state-hårddiskar (SSD) för snabbare åtkomst. Men kom ihåg att Premium endast har stöd för _sidblobbar_. Du behöver _blockblobbar_ för dina videor och en kö för buffring. Båda dessa är bara tillgängliga med alternativet _Standard_.

1. Välj _StorageV2 (generell användning v2)_ för **Typ av konto**. Det ger åtkomst till de senaste funktionerna och prissättningarna. I synnerhet har Blob-lagringskonton fler alternativ som är tillgängliga med den här kontotypen. Du behöver en blandning av blobbar och en kö, så alternativet _Blob-lagring_ fungerar inte. För det här programmet skulle det inte finnas någon fördel med att välja ett konto för _Storage (generell användning v1)_ eftersom det skulle begränsa de funktioner du kan komma åt och förmodligen inte skulle minska kostnaden för den förväntade arbetsbelastningen.

1. Välj **Lokalt redundant lagring (LRS)** för alternativet _Replikering_. Data i Azure Storage-konton replikeras alltid för att säkerställa hög tillgänglighet – med det här alternativet kan du välja hur långt bort replikeringen sker för att matcha dina krav på hög tillförlitlighet. I vårt fall blir bilderna och videorna snabbt inaktuella och tas bort från platsen. Det innebär att det inte finns någon större poäng med att betala extra för global redundans. I händelse av en katastrof som leder till dataförlust kan du starta om webbplatsen med nytt innehåll från användarna.

1. Ange **Åtkomstnivån** till _Frekvent_. Den här inställningen används bara för Blob Storage. **Frekvent åtkomstnivå** passar perfekt för data som används ofta, och **Lågfrekvent åtkomstnivå** passar bättre för data som inte används ofta. Lägg märke till att detta endast ställer in _standardvärdet_ – när du skapar en blob kan du ställa in ett annat datavärde. I vårt fall vill vi att videor ska läsas in snabbt, och därför använder du alternativet med höga prestanda för dina blobbar.

Följande skärmbild visar slutförda inställningar för fliken **Grundläggande inställningar**. Observera att resursgruppen, prenumerationen och namnet har olika värden.

![Skärmbild av ett blad för att skapa ett lagringskonto med fliken **Grundläggande inställningar** markerad.](../media/5-create-storage-account-basics.png)

### <a name="configure-the-advanced-options"></a>Konfigurera avancerade alternativ

1. Klicka på knappen **Nästa: Avancerat >** för att flytta till fliken **Avancerat** eller välj fliken **Avancerat** högst upp på skärmen.

1. Ställ in **Säker överföring krävs** till _Aktiverat_. Inställningen **Säker överföring krävs** kontrollerar om **HTTP** kan användas för REST API:er som används för att komma åt data i lagringskontot. Att ställa in det här alternativet till _Aktiverat_ tvingar alla klienter att använda SSL (**HTTPS**). I de flesta fall vill du ställa in detta till _Aktiverat_ eftersom användning av HTTPS över nätverket anses vara bästa praxis.

    > [!WARNING]
    > Om det här alternativet aktiveras framtvingar det vissa ytterligare begränsningar. Azure-filtjänstanslutningar utan kryptering misslyckas, inklusive scenarier med hjälp av SMB 2.1 eller 3.0 på Linux. Eftersom Azure Storage inte stöder SSL för anpassade domännamn, kan det här alternativet inte användas med ett anpassat domännamn.

1. Ange alternativet **Virtuella nätverk** till _Inget_. Med det här alternativet kan du isolera lagringskontot på ett virtuellt Azure-nätverk. Vi vill använda offentlig Internet-åtkomst. Innehållet är offentligt, och du behöver tillåta åtkomst från offentliga klienter.

1. Lämna alternativet **Data Lake Storage Gen2** som _Inaktiverat_. Det här är för stordata-program som inte är relevanta för den här modulen.

Följande skärmbild visar slutförda inställningar för fliken **Avancerat**.

![Skärmbild av ett blad för att skapa ett lagringskonto med fliken **Avancerat** markerad.](../media/5-create-storage-account-advanced.png)

### <a name="create"></a>Skapa

1. Du kan utforska inställningarna **Taggar** om du vill. Detta gör att du kan associera nyckel-/värdepar till kontot för din kategorisering och är en funktion som är tillgänglig för alla Azure-resurser.

1. Klicka på **Granska + skapa** för att granska dina inställningar. Detta kommer att göra en snabb verifiering av alternativen för att se till att alla obligatoriska fält har valts. Om det finns problem, kommer de att rapporteras här. När du har granskat inställningarna, klickar du på **Skapa** för att etablera lagringskontot.

Det tar några minuter att distribuera kontot. Medan Azure arbetar med det ska vi ta en titt på API:erna vi ska använda med kontot.

### <a name="verify"></a>Verifiera

1. Välj länken **Lagringskonton** i det vänstra sidofältet.

1. Leta upp det nya lagringskontot i listan för att verifiera skapandet.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

När du arbetar i din egen prenumeration kan du köra följande steg i Azure Portal för att ta bort resursgruppen och alla associerade resurser.

1. Välj länken **Resursgrupper** i det vänstra sidofältet.

1. Leta upp resursgruppen som du skapade i listan.

1. Högerklicka på resursgruppsposten och välj **Ta bort resursgrupp** på snabbmenyn. Du kan också klicka på ”...”-menyelementet på höger sida av posten för att komma till samma snabbmeny.

1. Ange resursgruppens namn i bekräftelsefältet.

1. Klicka på knappen **Ta bort**. Det här kan ta flera minuter.

Du har skapat ett lagringskonto med inställningar som styrs av dina affärsbehov. Till exempel valde du kanske ett datacenter i USA, västra eftersom dina kunder främst fanns i södra Kalifornien. Det här är ett typiskt flöde: först analyserar du dina data och mål och sedan konfigurerar du alternativen för lagringskonton så att de matchar.