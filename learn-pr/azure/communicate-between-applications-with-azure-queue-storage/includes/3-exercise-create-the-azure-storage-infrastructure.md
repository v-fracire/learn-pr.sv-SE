I den här övningen skapar du ett lagringskonto som är lämpligt för användning med en kö.

Anta att du arbetar för en nyhetsorganisation som får artiklar, blogginlägg och recensioner från ett världsomspännande nätverk med journalister. Artikelbidrag kan överbelasta systemet när globalt viktiga händelser inträffar. För att hantera detta har du bestämt dig för att lägga till en kö mellan klientdelen och mellannivån i ditt program för artikeluppladdning. 

Köer är en del av Azure-lagringskonton av typen generell användning, så du måste börja med att skapa ett lagringskonto.

## <a name="steps"></a>Steg

Skapa ett lagringskonto och en resursgrupp genom att följa dessa steg:

1. Logga in på [Azure-portalen](https://portal.azure.com?azure-portal=true).

1. I det övre vänstra hörnet klickar du på **Alla tjänster**.

1. Rulla ned till avsnittet **Lagring** och klicka på **Lagringskonton**

    ![Skapa ett lagringskonto](../media-draft/3-create-storage-account-1.png)

1. Längst upp till vänster på bladet **lagringskonton** klickar du på **Lägg till**.

1. Ange ett unikt namn för lagringskontot i textrutan **Namn**. Exempel: ”articlesubmission” plus *dina initialer* + *dagens datum*

1. Under **Distributionsmodell** väljer du **Resource Manager**.

1. I listrutan **Typ av konto** väljer du **Lagring (general-purpose v2)**.

1. Välj en region nära dig i listrutan **Plats**.

1. I listrutan **Replikering** väljer du **Lokalt redundant lagring (LRS)**.

1. Under **Prestanda** väljer du **Standard**.

1. Under **Åtkomstnivå** väljer du **Lågfrekvent**.

1. Under **Säker överföring krävs** väljer du **Inaktiverat**.

1. Välj din prenumeration under **Prenumeration**.

    ![Skapa ett lagringskonto](../media-draft/3-create-storage-account-2.png)

1. Under **Resursgrupp** väljer du **Skapa ny** och skriver **ArticleSubmissionResourceGroup** i textrutan.

1. Under **Virtuella nätverk** väljer du **Inaktiverat**.

1. Klicka på **Skapa** för att skapa det nya lagringskontot och den nya resursgruppen.

    ![Skapa ett lagringskonto](../media-draft/3-create-storage-account-3.png)

## <a name="summary"></a>Sammanfattning

Här har du skapat ett Azure-lagringskonto med inställningar som är anpassade för användning med en kö.