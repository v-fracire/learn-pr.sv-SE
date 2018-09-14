Instrumentpaneler är ett flexibelt verktyg för att hantera olika aspekter av Azure-tjänster via portalen. De gör det enkelt att övervaka status för dina tjänster. Eftersom de är delbart säkerställer de att alla i teamet ser samma data och är medveten om tillståndet för dina viktiga komponenter. Nu ska vi skapa en ny instrumentpanel och lägger till vissa paneler.

## <a name="create-a-new-dashboard"></a>Skapa en ny instrumentpanel

1. Klicka på knappen **Ny instrumentpanel** i Azure Portal.

1. I rutan **Min instrumentpanel** ändrar du namnet till **Kundens instrumentpanel**.

## <a name="add-and-configure-the-clock-tile"></a>Lägga till och konfigurera klockpanelen

1. Gå till panelgalleriet och dra klockan till arbetsytan. Placera längst upp till höger i det tillgängliga utrymmet.

1. Gå till bladet **Redigera klockan** och ändra platsen till **Pacific Time (USA och Kanada)**.

1. Gå till **Tidsformat** och klicka på **24-timmars**.

1. Klicka på **Klar**.

1. Upprepa de fyra stegen ovan, men välj **Eastern Time (USA och Kanada)** som plats. Du bör nu ha två klockor, en som visar tiden på USA:s västkust och den andra på USA:s östkust.

## <a name="resize-a-tile"></a>Ändra storlek på en panel

1. Gå till fönstret **Panelgalleri** och klicka på **Alla resurser**. Dra och släpp sedan panelen längst upp till vänster på den nya instrumentpanelsytan.

1. Klicka på panelen, högerklicka på ellipsen och klicka på **6 x 6**.

1. Klicka på det grå hörnet till höger nederkant av panelen och ändra storlek på panelen för 3.5 lodrätt av sex vågrätt. Observera att när du är klar med att ändra storlek på den justeras till 4 x 6.

1. Klicka på panelen **Resursgrupper** i Panelgalleriet och dra den till arbetsytan. Placera den nedanför panelen **Alla resurser**.

1. Klicka på panelen **Service Health** i Panelgalleriet och dra den till arbetsytan. Placera den till höger om panelen **Alla resurser**.

1. Fortsätt lägga till följande paneler och ordna dem enligt önskemål:

    - Hjälp + Support
    - Snabbuppgifter
    - Marketplace
    - Nyheter

1. När du har lagt till panelerna klickar du på **Anpassning klar**. Instrumentpanelen **Kundens instrumentpanel** bör nu visas.

## <a name="clone-a-dashboard"></a>Klona en instrumentpanel

Nu ska du skapa en liknande instrumentpanel för några andra kunder.

1. Klicka på knappen **Klona**.

1. Byt namn på instrumentpanelen från **Klon av Kundens instrumentpanel** till **Instrumentpanel för Administrationscenter för Azure AD**.

1. På panelen **Resursgrupper** klickar du på papperskorgen för att ta bort panelen.

1. I Panelgalleriet lägger du till följande paneler:

    - Organisationsidentitet
    - Användare och grupper
    - Sammanfattning av användaraktivitet
    - Välkommen till Administrationscenter för Azure AD

1. Ordna om panelerna som du vill och klicka sedan på **Anpassningen är klar**.

## <a name="share-a-dashboard"></a>Dela en instrumentpanel

Nu ska du göra instrumentpanelen tillgänglig för andra användare. Gör så här:

1. Markera administratörsinstrumentpanelen för Azure Active Directory (Azure AD) och klicka sedan på **Dela**.

1. På bladet **Delning och åtkomstkontroll** kontrollerar du att **Publicera till instrumentpanelens resursgrupp** är markerat.

1. Ange **Plats** som något som är lämpligt för ditt geografiska område. Som standard är värdet ditt närmaste datacenter.

1. Klicka på **Publicera** och stäng bladet **Delning och åtkomstkontroll**.

1. Klicka på **Instrumentpanel för Azure AD-administratör** och välj **Kundens instrumentpanel**.

    Observera att i **Alla resurser** visas nu en delad instrumentpanelresurs, och i **Resursgrupper** visas också en resursgrupp med instrumentpaneler.

1. Dela Kundens instrumentpanel genom att upprepa steg 1 till 3.

## <a name="edit-a-dashboardjson-file"></a>Redigera en instrumentpanelsfil (JSON)

Så här kan du ladda ned och redigera en instrumentpanelsfil:

1. Klicka på **Hämta**.

1. Öppna Utforskaren i Windows och gå till mappen Hämtade filer.

1. Leta rätt på filen *Kundens instrumentpanel.json* och dubbelklicka på den.

1. I din filredigerare letar du rätt på texten *ClockPart*.

1. Vid den första förekomsten av ClockPart ändrar du det tidigare värdet för **rowSpan** till 1.

1. Vid den andra förekomsten av ClockPart ändrar du också det tidigare värdet för **rowSpan** till 1.

1. Vid den andra förekomsten av Clockpart ändrar du Y-värdet från 2 till 1.

1. Spara filen *Kundens instrumentpanel.json* och stäng kodredigeraren.

1. På Azure-instrumentpanelen klickar du på **Ladda upp**.

1. I dialogrutan **Öppna** bläddrar du till mappen Hämtade filer och dubbelklickar på *Kundens instrumentpanel.json*.

    Observera att klockornas storlek har ändrats till en rads höjd, och klockan längst ned har flyttats upp en rad.

## <a name="select-a-shared-dashboard"></a>Välja en delad instrumentpanel

Du har insett att du inte gillar de mindre klockorna utan vill återgå till den tidigare delade versionen av Kundens instrumentpanel. Du kan göra det antingen genom att redigera filen och ladda upp den igen eller genom att gå till den delade versionen. Gör så här:

1. Klicka på den nedåtriktade pilen bredvid **Kundens instrumentpanel**.

1. Klicka på **Bläddra bland alla instrumentpaneler**.

1. På bladet **Alla instrumentpaneler** går du till **TYP** och väljer **Delade instrumentpaneler**.

1. Klicka på **Kundens instrumentpanel**.

1. Stäng bladet **Alla instrumentpaneler**.

    Observera att klockorna nu har återgått till sin ursprungliga storlek.

## <a name="switch-to-full-screen"></a>Växla till helskärm

1. Klicka på den nedåtriktade pilen bredvid **Kundens instrumentpanel**. 

    Du ser att det finns ytterligare en Kundens instrumentpanel, utan delningssymbolen bredvid. Klicka på den versionen av kunden instrumentpanelen och klockorna blir små igen.

1. Växla tillbaka till den delade versionen av Kundens instrumentpanel.

1. Klicka på knappen **Helskärm**. 

    Nu försvinner webbläsarmenyerna och fälten.

1. Klicka på den **avsluta Fullskärmsläge** att återgå till skärmen standard.

## <a name="unshare-a-dashboard"></a>Ta bort delning av en instrumentpanel

Om du vill förhindra att välja en delad instrumentpanel kan du _sluta dela_ den. Om du vill ta bort delning för en instrumentpanel gör du så här:

1. Klicka på knappen **Sluta dela**. Sidan **Delning och åtkomstkontroll** visas.

1. Klicka på knappen **Avpublicera**.

1. I bekräftelserutan klickar du på **OK**.

1. Klicka på den nedåtriktade pilen bredvid **Kundens instrumentpanel**.

1. Klicka på **Bläddra bland alla instrumentpaneler**.

1. På bladet **Alla instrumentpaneler** går du till **TYP** och väljer **Delade instrumentpaneler**.

    Nu tas **Kundens instrumentpanel** bort från listan med tillgängliga instrumentpaneler.

1. Stäng bladet **Alla instrumentpaneler**.

## <a name="delete-a-dashboard"></a>Ta bort en instrumentpanel

1. Kontrollera att Instrumentpanel för **Azure AD-administratör** har markerats.

1. Klicka på knappen **Ta bort**.

1. I den **bekräftelse** meddelanderutan, markera kryssrutan för att bekräfta att den här instrumentpanelen kommer inte längre vara synliga och klicka sedan på **OK**.

## <a name="reset-a-dashboard"></a>Återställa en instrumentpanel

1. Kontrollera att **Kundens instrumentpanel** är markerad.

1. Klicka på **Redigera**.

1. Högerklicka på arbetsytan och klicka på **Återställ till standardtillstånd**.

1. I meddelanderutan **Återställ instrumentpanelen till standardtillstånd** väljer du **Ja**.

    Kundens instrumentpanel återställs nu till standardpanelerna.

1. Klicka på **Anpassningen är klar**.

1. Klicka på ditt namn högst upp till höger i portalen.

1. Klicka på **Logga ut**.

1. Stäng webbläsaren.

## <a name="summary"></a>Sammanfattning

Nu har du skapat och redigerat instrumentpaneler, delat dem, ändrat dem som **.JSON**-filer, tagit bort delning och slutligen återställt dem till standard. Du har fått se vilka effektiva verktyg instrumentpaneler kan vara, och hur du kan använda dem för att skapa praktiska gränssnitt för olika roller i en organisation.
