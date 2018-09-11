Azure-produkter har en djup hierarki. Anta till exempel att du behöver skapa en virtuell Linux-dator. Du kan gå igenom dessa nivåer: **Start** > **Virtuella datorer** > **Compute** > **Ubuntu Server**. Azure-portalen optimerar användargränssnittet för att göra den här typen av navigeringssekvens intuitiv. Här kommer du att undersöka de viktiga UI-elementen som gör detta möjligt. Du navigerar genom menyer och undermenyer och använder blad för att hitta och konfigurera tjänster.

## <a name="azure-portal-layout"></a>Layout för Azure-portalen

Azure-portalen är det viktigaste grafiska användargränssnittet (GUI) som du använder för att styra Microsoft Azure. Du kan utföra de flesta viktiga hanteringsåtgärder på portalen och det är vanligtvis det bästa gränssnittet för att utföra enskilda åtgärder eller för att se konfigurationsalternativ i detalj.

Fönstret på vänster sida i portalen är resursfönstret som visar de viktigaste resurstyperna. Observera att Azure har många fler resurstyper än de som visas.

## <a name="using-blades-in-the-azure-portal"></a>Använda blad på Azure-portalen

Azure-portalen använder modellen med blad för navigering. Ett _blad_ är en öppningsbar panel som innehåller gränssnitt för en enda nivå i en navigeringssekvens. Till exempel representeras var och en av de här elementen i den här sekvensen av ett blad: **Virtuella datorer** > **Compute** > **Ubuntu Server**.

Varje blad i användargränssnittet innehåller vanligtvis ett antal konfigurerbara alternativ. Vissa av dessa alternativ genererar ett annat blad, som visas till höger om eventuella befintliga blad. På det nya bladet visas eventuella ytterligare konfigurerbara alternativ som kan starta ett nytt blad och så vidare. Efter ett tag kan du ha flera blad öppna samtidigt. Du kan maximera blad så att de fyller hela skärmen.

Om du försöker stänga ett blad utan att spara de konfigurationsändringar du gjort kommer du att få en fråga om du verkligen vill fortsätta.

## <a name="configuring-settings-in-the-azure-portal"></a>Konfigurera inställningar i Azure-portalen

Azure-portalen visar flera konfigurationsalternativ, för det mesta i statusfältet längst upp till höger på skärmen.

### <a name="notifications"></a>Meddelanden

När du klickar på klockikonen visas fönstret **Meddelanden**. Det här fönstret visar de senaste åtgärderna som utförts samt deras status.

![Meddelandebladet](../media-draft/2-notifications-blade.PNG)

### <a name="cloud-shell"></a>Cloud Shell

Om du klickar på ikonen för **Cloud Shell** (>_) skapar du en ny Azure Cloud Shell-session. Du uppmanas att använda Linux Bash eller PowerShell på Linux i den aktuella sessionen.

![Cloud Shell](../media-draft/2-choose-shell.PNG)

### <a name="settings"></a>Inställningar

Klicka på **kugghjulsikonen** för att ändra inställningarna för Azure-portalen. Några exempel på inställningar är:

* Utloggningstid
* Färgschema
* Högkontrastteman
* Popup-meddelanden (till en mobil enhet)
* Dubbelklicka för att ändra tema
* Språk
* Regionalt format

![Inställningar för portalen](../media-draft/2-settings-blade.PNG)

När du har ändrat inställningarna klickar du på **Tillämpa** för att godkänna ändringarna.

### <a name="feedback-blade"></a>Feedbackbladet

Ikonen med **uttryckssymbolen** öppnar bladet **Skicka feedback**. Här kan du skicka feedback till Microsoft om Azure. Observera att du kan ange om Microsoft kan svara på din feedback via e-post.

![Feedback](../media-draft/2-feedback-blade.PNG)

### <a name="help-blade"></a>Hjälpbladet

Klicka på **frågetecknet** för att visa bladet **Hjälp**. Här kan du välja bland ett antal ämnen, t.ex.:

* Nyheter
* Azure-översikt
* Starta guidad visning
* Kortkommandon
* Visa diagnostik
* Sekretess + villkor

### <a name="directory-and-subscription"></a>Katalog och prenumeration

Klicka på ikonen **Boka och filtrera** för att visa bladet **Katalog + prenumeration**.

Azure gör det möjligt för dig att ha mer än en prenumeration associerad med en katalog. Du kan ändra mellan prenumerationer på bladet **Katalog och prenumeration**. Här kan du kan ändra din prenumeration eller ändra till en annan katalog.

![Katalog](../media-draft/2-directory-blade-1.PNG)

### <a name="profile-settings"></a>Profilinställningar

Om du klickar på ditt namn i det övre högra hörnet kan du sedan ändra profilinställningarna.
Alternativen är:

* Logga ut från Azure
* Ändra lösenord
* Ändra kontaktuppgifter
* Visa behörigheter
* Skicka en idé till Azure-teamet
* Visa din faktura
* Växla katalog (visar bladet **Katalog + prenumeration** som i föregående avsnitt)

![Profilinställningar](../media-draft/2-portal-menu.png)

Om du nu klickar på **Visa min faktura** tar Azure dig till sidan **Kostnadshantering + Fakturering – Fakturor** som hjälper dig att analysera var Azure genererar kostnader.

![Faktureringssida](../media-draft/2-portal-billing.PNG)

## <a name="summary"></a>Sammanfattning

Azure är en stor produkt och det återspeglas i portalens användargränssnitt. Det huvudsakliga sätt som portalen hjälper dig att navigera genom den här komplexiteten är med hjälp av blad som visar hierarkin. Blad gör det möjligt för dig att fokusera på en viss uppgift, samtidigt som det tydligt visar hur du gjorde för att nå den punkten.