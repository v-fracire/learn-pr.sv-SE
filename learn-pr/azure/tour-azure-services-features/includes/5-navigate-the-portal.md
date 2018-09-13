Nu när vi har ett konto kan vi logga in på **Azure-portalen**. Portalen är en webbaserad administrationsplats som låter dig interagera med alla dina prenumerationer och resurser som du har skapat. Nästan allt du gör med Azure kan göras via det här webbgränssnittet.

## <a name="azure-portal-layout"></a>Layout för Azure-portalen

Azure-portalen är det primära grafiska användargränssnittet (GUI) som du använder för att styra Microsoft Azure. Du kan utföra de flesta viktiga hanteringsåtgärder på portalen och det är vanligtvis det bästa gränssnittet för att enskilda åtgärder eller där du vill se konfigurationsalternativen i detalj.

![Azure-portalen](../media-draft/5-portal.png)

:::row:::

    :::column:::
    ![The resources and favorites](../media-draft/5-favorites.png)
    :::column-end:::

    :::column span="3":::
    **Resource Panel**
    
    In the left-hand sidebar of the portal is the resource pane, which lists the main resource types. Note that Azure has more resource types than just those shown. The resources listed are part of your _favorites_. 

    You can customize this with the specific resource types you tend to create or administer most often. 

    You can also collapse this pane; with the **<<** caret. This will minimize it to just icons which can be convenient if you are working with limited screen real-estate.
    :::column-end:::

:::row-end:::

Resten av portalvyn är för de specifika element som du arbetar med. Standardsidan (startsidan) är _instrumentpanelen_. Vi tar upp det lite senare, men det representerar en anpassningsbar överblick över dina resurser. Du kan använda den för att hoppa till specifika resurser som du vill hantera eller söka efter resurser med inlägget **Alla resurser** i resurspanelen. När du hanterar en resurs som en virtuell dator eller en webbapp, kommer du att arbeta med ett _blad_ som visar specifik information om resursen.

## <a name="what-is-a-blade"></a>Vad är ett blad?

Azure-portalen använder en bladmodell för navigering. Ett _blad_ är en öppningsbar panel som innehåller gränssnitt för en enda nivå i en navigeringssekvens. Till exempel skulle vart och ett av elementen i den här sekvensen representeras av ett blad: **Virtuella datorer** > **Compute** > **Ubuntu Server**.

Varje blad innehåller viss information och de konfigurerbara alternativ. Vissa av dessa alternativ genererar ett annat blad som visas till höger om eventuella befintliga blad. På det nya bladet visas eventuella ytterligare konfigurerbara alternativ som kan starta ett nytt blad och så vidare. Efter ett tag kan du ha flera blad öppna samtidigt. Du kan maximera bladen så att de fyller hela skärmen.

Eftersom nya blad alltid läggs till höger om sin ägare, kan du använda rullningslisten längst ned i fönstret för att gå bakåt och se hur du kom hit i konfigurationen. Alternativt kan du stänga bladen individuellt genom att klicka på `X`-knappen i det övre hörnet på bladet. Om du har ändringar som inte sparats, kommer Azure informera dig att ändringarna går förlorade om du fortsätter.

## <a name="configuring-settings-in-the-azure-portal"></a>Konfigurera inställningarna i Azure-portalen

Azure-portalen visar flera konfigurationsalternativ, för det mesta i statusfältet längst upp till höger på skärmen.

### <a name="notifications"></a>Meddelanden

När du klickar på klockikonen visas fönstret **Meddelanden**. Det här fönstret visar de senaste åtgärderna som utförts samt deras status.

![Meddelandebladet](../media-draft/5-notifications-blade.png)

### <a name="cloud-shell"></a>Cloud Shell

Om du klickar på ikonen för **Cloud Shell** (>_) skapar du en ny Azure Cloud Shell-session. Azure Cloud Shell är ett interaktivt, webbläsartillgängligt skal för att hantera Azure-resurser. Det ger dig flexibilitet att välja den skalupplevelse som bäst passar ditt sätt att arbeta. Linux-användare kan välja en Bash-upplevelse och Windows-användare kan välja PowerShell. Den här webbläsarbaserade terminalen låter dig styra och administrera alla dina Azure-resurser i den aktuella prenumerationen via ett kommandoradsgränssnitt som är inbyggt i portalen.

![Cloud Shell](../media-draft/5-choose-shell.png)

### <a name="settings"></a>Inställningar

Klicka på **kugghjulsikonen** för att ändra inställningarna för Azure-portalen. Några exempel på inställningar är:

- Utloggningstid
- Färgschema
- Högkontrastteman
- Toast-meddelanden (till en mobil enhet)
- Dubbelklicka för att ändra temat
- Språk
- Regionalt format

![Inställningar för portalen](../media-draft/5-settings-blade.png)

När du har ändrat inställningarna klickar du på **Tillämpa** för att godkänna ändringarna.

### <a name="feedback-blade"></a>Feedbackbladet

Ikonen med **uttryckssymbolen** öppnar bladet **Skicka feedback**. Här kan du skicka feedback till Microsoft om Azure. Observera att du kan ange om Microsoft kan svara på din feedback via e-post.

![Feedback](../media-draft/5-feedback-blade.png)

### <a name="help-blade"></a>Hjälpbladet

Klicka på **frågetecknet** för att visa bladet **Hjälp**. Här kan du välja mellan flera alternativ, inklusive:

- Nyheter
- Azure-översikt
- Starta guidad visning
- Kortkommandon
- Visa diagnostik
- Sekretess + villkor

### <a name="directory-and-subscription"></a>Katalog och prenumeration

Klicka på ikonen **Boka och filtrera** för att visa bladet **Katalog + prenumeration**.

Azure gör det möjligt för dig att ha mer än en prenumeration associerad med en katalog. Du kan ändra mellan prenumerationer på bladet **Katalog och prenumeration**. Här kan du kan ändra din prenumeration eller ändra till en annan katalog.

![Katalog](../media-draft/5-directory-blade-1.png)

### <a name="profile-settings"></a>Profilinställningar

Om du klickar på ditt namn i det övre högra hörnet kan du sedan ändra profilinställningarna.
Alternativen är:

- Logga ut från Azure
- Ändra lösenord
- Ändra kontaktuppgifter
- Visa behörigheter
- Skicka en idé till Azure-teamet
- Visa din faktura
- Växla katalog (visar bladet **Katalog + prenumeration** som i föregående avsnitt)

![Profilinställningar](../media-draft/5-portal-menu.png)

Om du nu klickar på **Visa min faktura** tar Azure dig till sidan **Kostnadshantering + Fakturering – Fakturor** som hjälper dig att analysera var Azure genererar kostnader.

![Faktureringssida](../media-draft/5-portal-billing.png)

Azure är en stor produkt och det återspeglas i portalens användargränssnitt. Metoden med glidande blad låter dig navigera fram och tillbaka mellan de olika administrationsuppgifterna med enkelhet. Nu ska vi testa lite med det här användargränssnittet så att du får öva lite.