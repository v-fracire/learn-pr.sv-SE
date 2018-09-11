I den här övningen skapar och konfigurerar du en anpassad instrumentpanel.

## <a name="create-a-new-dashboard"></a>Skapa en ny instrumentpanel

1. Klicka på knappen **Ny instrumentpanel** i Azure Portal.

2. I rutan **Min instrumentpanel** ändrar du namnet till **Kundens instrumentpanel**.

## <a name="add-and-configure-the-clock-tile"></a>Lägga till och konfigurera klockpanel

1. Gå till panelgalleriet, och dra och släpp klockan på arbetsytan. Placera den längst upp höger i det tillgängliga utrymmet.

2. Gå till bladet Redigera klocka och ändra platsen till **Pacific Time (USA och Kanada)**.

3. Gå till **Tidsformat** och klicka på **24-timmars**.

4. Klicka på **Klar**.

5. Upprepa de fyra stegen ovan, men välj **Eastern Time (USA och Kanada)** som plats. Du bör nu ha två klockor, en som visar tiden på USA:s västkust och den andra på USA:s östkust.

## <a name="resize-a-tile"></a>Ändra storlek på en panel

1. Gå till Panelgalleriet och klicka på **Alla resurser**. Dra och släpp sedan panelen längst upp till vänster på den nya instrumentpanelsytan.

2. Klicka på panelen, högerklicka på ellipsen och klicka på **6 x 6**.

3. Klicka på det grå hörnet längst ned till höger på panelen och ändra storlek på panelen till 3,5 lodrätt gånger 6 vågrätt. Observera att när du släpper hörnet ändras storleken till 4 x 6.

4. Klicka på panelen **Resursgrupper** i Panelgalleriet och dra den till arbetsytan. Placera den nedanför panelen **Alla resurser**.

5. Klicka på panelen **Service Health** i Panelgalleriet och dra den till arbetsytan. Placera den till höger om panelen **Alla resurser**.

6. Fortsätt lägga till följande paneler och ordna dem enligt önskemål:

    * Hjälp + Support
    * Snabbuppgifter
    * Marketplace
    * Nyheter

7. När du har lagt till panelerna klickar du på **Anpassning klar**. Instrumentpanelen **Kundens instrumentpanel** bör nu visas.

## <a name="clone-a-dashboard"></a>Klona en instrumentpanel

Nu ska du skapa en liknande instrumentpanel för några andra kunder

1. Klicka på knappen **Klona**.

2. Byt namn på instrumentpanelen från **Klon av Kundens instrumentpanel** till **Instrumentpanel för Administrationscenter för Azure AD**.

3. På panelen **Resursgrupper** klickar du på papperskorgen för att ta bort panelen.

4. I Panelgalleriet lägger du till följande paneler:

    * Organisationsidentitet
    * Användare och grupper
    * Sammanfattning av användaraktivitet
    * Välkommen till Administrationscenter för Azure AD

5. Ordna om panelerna som du vill och klicka sedan på **Anpassningen är klar**.

## <a name="share-a-dashboard"></a>Dela en instrumentpanel

Nu ska du göra instrumentpanelen tillgänglig för andra användare. Gör så här:

1. Markera Instrumentpanel för Azure AD-administratör och klicka sedan på **Dela**.

2. På sidan **Delning och åtkomstkontroll** kontrollerar du att **Publicera till instrumentpanelens resursgrupp** är markerat.

3. Ange **Plats** som något som är lämpligt för ditt geografiska område. Som standard är värdet ditt närmaste datacenter.

4. Klicka på **Publicera** och stäng sidan **Delning och åtkomstkontroll**.

5. Klicka på **Instrumentpanel för Azure AD-administratör** och välj **Kundens instrumentpanel**.

6. Observera att i **Alla resurser** visas nu en delad instrumentpanelresurs, och i **Resursgrupper** visas också en resursgrupp med instrumentpaneler.

7. Dela Kundens instrumentpanel genom att upprepa Steg 1 till 3.

## <a name="edit-a-dashboard-file"></a>Redigera en instrumentpanelsfil

Så här kan du ladda ned och redigera en instrumentpanelsfil:

1. Klicka på **Hämta**.

2. Öppna Utforskaren i Windows och gå till mappen Hämtade filer.

3. Leta rätt på filen **Kundens instrumentpanel.json** och dubbelklicka på den.

4. I din filredigerare letar du rätt på texten ”ClockPart”

5. Vid den första förekomsten av ClockPart ändrar du det tidigare värdet för **rowSpan** till 1.

6. Vid den andra förekomsten av ClockPart ändrar du också det tidigare värdet för **rowSpan** till 1.

7. Vid den andra förekomsten av Clockpart ändrar du Y-värdet från 2 till 1.

8. Spara filen Kundens instrumentpanel.json och stäng kodredigeraren.

9. På Azure-instrumentpanelen klickar du på **Ladda upp**.

10. I dialogrutan **Öppna** bläddrar du till mappen Hämtade filer och dubbelklickar på **Kundens instrumentpanel.json**.

11. Observera hur klockornas storlek har ändrats till en rads höjd, och klockan längst ned har flyttats upp en rad.

## <a name="select-a-shared-dashboard"></a>Välja en delad instrumentpanel

Du har insett att du inte gillar de mindre klockorna utan vill återgå till den tidigare delade versionen av Kundens instrumentpanel. Du kan göra det antingen genom att redigera filen och ladda upp den igen eller genom att öppna den delade versionen. Gör så här:

1. Klicka på den nedåtriktade pilen bredvid **Kundens instrumentpanel**.

2. Klicka på **Bläddra bland alla instrumentpaneler**.

3. På sidan **Alla instrumentpaneler** går du till **TYP** och väljer **Delade instrumentpaneler**.

4. Klicka på **Kundens instrumentpanel**.

5. Stäng sidan **Alla instrumentpaneler**.

6. Observera att klockorna nu har återgått till sin ursprungliga storlek.

## <a name="switch-to-full-screen"></a>Växla till helskärm

1. Klicka på den nedåtriktade pilen bredvid **Kundens instrumentpanel**. Du ser att det finns ytterligare en Kundens instrumentpanel, utan delningssymbolen bredvid. Klicka på den versionen av Kundens instrumentpanel, så blir klockorna små igen.

2. Växla tillbaka till den delade versionen av Kundens instrumentpanel.

3. Klicka på knappen **Helskärm**. Nu försvinner webbläsarmenyerna och fälten.

4. Klicka på **Avsluta helskärmsläge** för att återgå till den vanliga skärmen.

## <a name="unshare-a-dashboard"></a>Ta bort delning av en instrumentpanel

Om du inte vill att en viss instrumentpanel ska vara tillgänglig för delning kan du ta bort delningen. Om du vill ta bort delning för en instrumentpanel gör du så här:

1. Klicka på knappen **Sluta dela**. Sidan **Delning och åtkomstkontroll** visas.

2. Klicka på knappen **Avpublicera**.

3. I bekräftelserutan klickar du på **OK**.

4. Klicka på den nedåtriktade pilen bredvid **Kundens instrumentpanel**.

5. Klicka på **Bläddra bland alla instrumentpaneler**.

6. På sidan **Alla instrumentpaneler** går du till **TYP** och väljer **Delade instrumentpaneler**.

7. Nu tas Kundens instrumentpanel bort från listan med tillgängliga instrumentpaneler.

8. Stäng sidan Alla instrumentpaneler.

## <a name="delete-a-dashboard"></a>Ta bort en instrumentpanel

1. Kontrollera att Instrumentpanel för Azure AD-administratör har markerats.

2. Klicka på knappen **Ta bort**.

3. I meddelanderutan **Bekräftelse** markerar du kryssrutan för att bekräfta att den här instrumentpanelen inte ska visas längre. Klicka sedan på **OK**.

## <a name="reset-a-dashboard"></a>Återställa en instrumentpanel

1. Kontrollera att Kundens instrumentpanel är markerad.

2. Klicka på **Redigera**.

3. Högerklicka på arbetsytan och klicka på **Återställ till standardtillstånd**.

4. I meddelanderutan **Återställ instrumentpanelen till standardtillstånd** väljer du **Ja**.

5. Kundens instrumentpanel återställs nu till standardpanelerna.

6. Klicka på **Anpassningen är klar**.

7. Klicka på ditt namn högst upp i portalen.

8. Klicka på Logga ut.

9. Stäng webbläsaren.

## <a name="summary"></a>Sammanfattning

Nu har du skapat och redigerat instrumentpaneler, delat dem, ändrat dem som **.JSON**-filer, tagit bort delning och slutligen återställt dem till standard. Du har fått se vilka effektiva verktyg instrumentpaneler kan vara, och hur du kan använda dem för att skapa praktiska gränssnitt för olika roller i en organisation.
