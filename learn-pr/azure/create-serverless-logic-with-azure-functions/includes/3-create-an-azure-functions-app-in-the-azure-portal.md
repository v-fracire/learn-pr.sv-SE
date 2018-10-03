Du är nu redo att börja implementera temperaturtjänsten. I den föregående delen fastställde du att en lösning utan server skulle passa bäst för dina behov. Låt oss börja med att skapa en funktionsapp för att lagra våra Azure-funktion.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="what-is-a-function-app"></a>Vad är en funktionsapp?

Funktioner tillhandahålls i en körningskontext som kallas **funktionsapp**. Du definierar funktionsappar för att logiskt gruppera och strukturera funktionerna och en beräkningsresurs i Azure. I vårt exempel med en hiss skulle en funktionsapp ha skapats som värd för temperaturtjänsten för maskineriets drivhjul. Det finns några beslut som måste fattas för att skapa en funktionsapp. Du måste välja en tjänstplan och ett kompatibelt lagringskonto.

### <a name="choosing-a-service-plan"></a>Välja en tjänstplan

Funktionsappar kan användas med en av två typer av tjänstplaner. Den första är **förbrukningsplanen**. Välj den här planen om du använder Azures programplattform utan server. Förbrukningsplanen innehåller automatisk skalning, och du debiteras när funktionerna körs. I förbrukningsplanen ingår en konfigurerbar tidsgräns för funktionens körning. Den är som standard 5 minuter, men kan konfigureras för upp till 10 minuter.

Den andra planen är **Azure App Service-planen**. Med den här planen kan du undvika tidsgränsperioder genom att funktionen körs kontinuerligt på en virtuell dator som du definierar. När du använder en App Service-plan ansvarar du för att hantera de appresurser som funktionen körs på, så det är tekniskt sett inte en serverlös plan. Men det kan vara ett bättre alternativ om dina funktioner används kontinuerligt eller kräver mer processorkraft eller körningstid än vad som ingår i förbrukningsplanen.

### <a name="storage-account-requirements"></a>Krav för lagringskonto

När du skapar en funktionsapp måste den kopplas till ett lagringskonto. Du kan välja ett befintligt konto eller skapa ett nytt. Lagringskontot används av funktionsappen vid interna åtgärder som att logga funktionskörningar och hantera körningsutlösare. Med förbrukningsplanen är det även här som funktionskoden och konfigurationsfilen lagras.

## <a name="create-a-function-app"></a>Skapa en funktionsapp

Nu ska vi skapa en funktionsapp i Azure Portal.

1. Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. Välj knappen **Skapa en resurs** längst upp till vänster på Azure Portal och sedan **Kom igång > Serverlös funktionsapp** för att öppna bladet Funktionsapp *Skapa*. Du kan också använda alternativet **Compute > Funktionsapp** då samma blad öppnas.

  ![Skärmbild av Azure Portal som visar bladet Skapa resurs med Compute-avsnittet och Funktionsapp markerade.](../media/3-create-function-app-blade.png)

1. Välj ett globalt unikt appnamn. Det kommer att fungera som bas-URL för din tjänst. Du kan till exempel kalla den **escalator-functions-xxxxxxx**, där kryssen kan ersättas med dina initialer och ditt födelseår. Om den ändå inte är globalt unikt kan du testa vilken kombination som helst. Giltiga tecken är a-z, 0-9 och -.

1. Välj den Azure-prenumeration som du vill ska vara värd för funktionsappen.

1. Välj den befintliga resursgruppen som heter ”**<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**”.

1. Välj **Windows** som OS.

1. Välj **Förbrukningsplan** som **Värdplan**. Det är alltså det serverlösa värdalternativet.

1. Välj den geografiska plats som är närmast dig från listan nedan. I ett produktionssystem vill du helst välja en plats nära dina kunder eller funktionens kunder.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. För **Körningsstack** väljer du JavaScript i listrutan, som är det språk som vi implementerar funktionsexemplen i den här övningen i.

1. Skapa ett nytt lagringskonto. Azure ger det ett namn baserat på appnamnet. Du kan ändra det om du vill, men det måste också vara unikt.

1. Se till att Azure Application Insights är **På** och välj den region som är närmast dig (eller dina kunder).

1. Välj **Skapa**. Distributionen kan ta några minuter. Du får ett meddelande när den är klar.

## <a name="verify-your-azure-function-app"></a>Verifiera din Azure-funktionsapp

1. Välj **Resursgrupper** i den vänstra menyn i Azure-portalen. Du bör då se en resursgrupp med namnet **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>** i listan över tillgängliga grupper.

    ![Skärmbild av Azure-portalen som visar bladet Resursgrupper med menyalternativet Resursgrupper och listobjektet <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn> markerat.](../media/3-resource-group.png)

1. Välj resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**. Du bör se en lista över resurser som liknar följande lista.

    ![Skärmbild av Azure-portalen som visar alla resurser i gruppen <rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>, inklusive poster för en App Service-plan, ett lagringskonto, en Application Insights-resurs och en App Service.](../media/3-resource-list.png)

Objektet med blixtikonen, som visas som en App Service, är din nya funktionsapp. Du kan klicka på den för att öppna information om den nya funktionen – den har tilldelats en offentlig URL, om du öppnar den i en webbläsare bör du få en standardsida som visar att din funktionsapp körs.
