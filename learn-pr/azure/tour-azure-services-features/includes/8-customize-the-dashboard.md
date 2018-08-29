Här får du skapa och ändra instrumentpaneler med hjälp av portalens användargränssnitt och genom att redigera den underliggande JSON-filen direkt.

## <a name="what-is-a-dashboard"></a>Vad är en instrumentpanel?

En _instrumentpanel_ består av en anpassningsbar panelsamling i användargränssnittet i Azure-portalen. Du kan lägga till, ta bort och placera panelerna för att skapa den vy du vill ha och sedan spara vyn som en instrumentpanel. Det går att ha fler instrumentpaneler och du kan växla mellan dem efter behov. Du kan även dela instrumentpaneler med andra teammedlemmar.

Med hjälp av instrumentpaneler får du stor flexibilitet när du hanterar Azure. Du kan till exempel skapa instrumentpaneler för specifika roller i organisationen och sedan använda rollbaserad åtkomstkontroll (RBAC) för att kontrollera vem som kan komma åt instrumentpanelen. Din databasadministratör kan således ha en instrumentpanel som innehåller vyer för SQL-databastjänsten, medan Azure Active Directory-administratören har vyer för användare och grupper i Azure AD.

Instrumentpaneler lagras som .JSON-filer (JavaScript Object Notation). Det innebär att de kan laddas upp och laddas ned till andra datorer eller delas med andra i Azure-katalogen. Azure lagrar instrumentpaneler i resursgrupper, precis som virtuella datorer eller lagringskonton som du kan hantera i portalen.

Eftersom instrumentpaneler är .JSON-filer kan du även anpassa dem programmässigt, vilket gör dem till mycket kraftfulla administrativa verktyg. Dessutom kan vissa typer av paneler vara frågebaserade så att de uppdateras automatiskt när källdata ändras.

## <a name="default-dashboard"></a>Standardinstrumentpanel

Standardinstrumentpanelen heter helt enkelt Instrumentpanel. När du loggar in på portalen visas den här instrumentpanelen och dess fem webbdelar.

![Standardwebbdelar](../images/4-dashboard-default-webparts.png)

Standardwebbdelarna är

1. Alla resurser
2. Snabbstarter och självstudier
3. Service Health
4. Marketplace
5. Komma igång med Azure

## <a name="creating-and-managing-dashboards"></a>Skapa och hantera instrumentpaneler

Överst på instrumentpanelen finns det kontroller för att skapa, ladda upp, ladda ned, redigera och dela en instrumentpanel. Du kan även visa en instrumentpanel i fullskärmsläge samt klona eller ta bort den.

![Anpassa instrumentpanelskontroller](../images/7-customise-dashboard-controls.PNG)

### <a name="select-dashboard"></a>Välj instrumentpanel

Till vänster finns den nedrullningsbara kontrollen för att välja instrumentpanel. Klicka på den här kontrollen om du vill välja bland instrumentpaneler som du redan har definierat för ditt konto. Den här kontrollen gör det enkelt för dig att definiera flera instrumentpaneler för olika syften, och sedan växla mellan dem beroende på vad du behöver göra.

Observera att alla instrumentpaneler som du skapar först är privata. Det vill säga att bara du kan se dem. Du måste dela en instrumentpanel om du vill göra den tillgänglig för hela företaget. Mer information om att dela/avsluta delning för instrumentpaneler finns senare i avsnittet.

### <a name="create-a-new-dashboard"></a>Skapa en ny instrumentpanel

Om du vill skapa en ny instrumentpanel klickar du helt enkelt på **Ny instrumentpanel**. Arbetsytan för instrumentpanel öppnas utan paneler. Här kan du lägga till paneler enligt beskrivningen under Redigera en instrumentpanel med användargränssnittet senare i den här modulen. När du är klar med att lägga till och justera paneler, och har ändrat instrumentpanelens namn, klickar du bara på **Anpassningen är klar** för att spara och växla till instrumentpanelen.

### <a name="upload-and-download"></a>Ladda upp och ladda ned

Med knapparna för att **ladda upp** och **ladda ned** kan du ladda ned din befintliga instrumentpanel som en .JSON-fil, anpassa den och sedan distribuera och ladda upp den eller låta någon annan ladda upp filen igen till Azure Portal, så att deras aktuella instrumentpanel ersätts.

Om du klickar på **Ladda ned** laddas den aktuella instrumentpanelen ned till din standardmapp för nedladdningar. När du öppnar den nedladdade filen visas .JSON-koden.

![JSON-kod för instrumentpanel](../images/7-dashboard-json-code.PNG)

Du kan sedan redigera koden manuellt, till exempel genom att ändra panelstorlekar, och sedan ladda upp till Azure genom att klicka på **uppladdningsknappen**.

### <a name="edit-a-dashboard"></a>Redigera en instrumentpanel

I avsnittet Redigera en instrumentpanel med användargränssnittet hittar du mer information om det här ämnet.

### <a name="shareunshare-a-dashboard"></a>Dela/sluta dela en instrumentpanel

När du definierar en ny instrumentpanel är den privat och syns endast på ditt konto. Om du vill göra instrumentpanelen synlig för andra måste du dela den. Som för alla andra Azure-resurser behöver du ange en resursgrupp (eller använda en befintlig resursgrupp) för att lagra delade instrumentpaneler. Om du inte har en resursgrupp skapar Azure en resursgrupp för instrumentpaneler på den plats som du anger. Om du har resursgrupper kan du välja en resursgrupp för att lagra instrumentpaneler.

![Delning och åtkomstkontroll 1](../images/7-share-dashboards-default.PNG)

När du har delat mallen visas andra bladet för **Delning och åtkomstkontroll**.

![Delning och åtkomstkontroll 2](../images/7-share-dashboards-access-control.PNG)

Nu kan du klicka på **Hantera användare** och ange vilka användare som ska ha åtkomst till instrumentpanelen.

### <a name="switching-to-a-shared-dashboard"></a>Växla till en delad instrumentpanel

Om du vill växla till en delad instrumentpanel klickar du på listan över instrumentpaneler och sedan på **Bläddra bland alla instrumentpaneler**.

![Bläddra bland alla instrumentpaneler](../images/7-browse-dashboards.PNG)

Nu visas bladet Alla instrumentpaneler, och namnen på alla delade instrumentpaneler visas. Klicka bara på en instrumentpanel för att tillämpa den på Azure-portalen.

![Delade instrumentpaneler](../images/7-select-shared-dashboard.png)

### <a name="display-a-dashboard-as-a-full-screen"></a>Visa en instrumentpanel i fullskärmsläge

Om du vill visa instrumentpanelen i största möjliga storlek klickar du på **fullskärmsknappen** så visas din aktuella instrumentpanel utan några webbläsarmenyer. Om det finns paneler utanför skärmvisningens gränser visas skjutreglagen till höger och längst ned på skärmen.

När du är klar med att arbeta i fullskärmsläge trycker du på ESC eller klickar på **Avsluta fullskärmsläge** bredvid instrumentpanelens namn högst upp på skärmen.

### <a name="clone-a-dashboard"></a>Klona en instrumentpanel

När du klonar en instrumentpanel skapas en kopia som kallas ”Klon av (instrumentpanelens namn)” och kopian blir den aktuella instrumentpanelen. Att klona är även ett enkelt sätt att skapa instrumentpaneler innan du delar dem. Om du till exempel har en instrumentpanel som är nästan klar, kan du klona den, göra de ändringar som behövs och sedan dela den.

### <a name="delete-a-dashboard"></a>Ta bort en instrumentpanel

När du tar bort en instrumentpanel tas den även bort från listan med tillgängliga instrumentpaneler. Du uppmanas att bekräfta att instrumentpanelen ska tas bort. Det går inte att återställa en instrumentpanel som har tagits bort.

## <a name="edit-a-dashboard-through-the-user-interface"></a>Redigera en instrumentpanel med användargränssnittet

Det går att redigera en instrumentpanel genom att ladda ned .JSON-filen, ändra värdena i filen och ladda upp den till Azure igen, men den här metoden är inte särskilt intuitiv för användargränssnittets utformning. Om du vill använda det grafiska användargränssnittet för att konfigurera din befintliga instrumentpanel klickar du på knappen **Redigera** eller högerklickar på instrumentpanelen och klickar på **Redigera**. Instrumentpanelen övergår till redigeringsläget.

![Redigera instrumentpanel](../images/7-edit-dashboard.PNG)

På vänster sida visas panelgalleriet med ett antal paneler nedanför. Du kan filtrera panelgalleriet med följande kriterier:

* Allmänt
* Typ
* Search
* Resursgrupp
* Tagga

![Panelgalleri](../images/7-tile-gallery.png)

Varje alternativ kan filtreras ytterligare med kategorier som Azure Active Directory, Sakernas Internet och Microsoft Intune.

Du kan lägga till paneler genom att helt enkelt välja en panel i listan till vänster och sedan dra och släppa den på arbetsytan. Sedan kan du flytta varje panel till önskad plats, ändra dess storlek eller vilka data som visas.

Arbetsytan är indelad i rutor i redigeringsläget. Varje panel måste fylla minst en ruta och panelerna fästs intill närmaste största panelavgränsare. Överlappande paneler flyttas undan. När du gör en panel mindre flyttas omgivande paneler och placeras intill den panelen.

### <a name="change-tile-sizes"></a>Ändra panelstorlekar

Vissa paneler har en fast storlek och du kan bara redigera deras storlek programmässigt. Paneler med ett grått hörn längst ned till höger går däremot att redigera genom att dra och släppa hörnindikatorn.

![Panel som kan storleksanpassas](../images/7-resizable-tile.png)

Du kan också högerklicka på snabbmenyn och ange önskad storlek.

![Panelstorlek](../images/7-tile-size.png)

Skapa en instrumentpanel genom att dra paneler från panelgalleriet till arbetsytan och sedan ordna dem.

### <a name="change-tile-settings"></a>Ändra panelinställningar

Vissa paneler har inställningar som kan redigeras. När du till exempel drar klockpanelen till arbetsytan öppnas panelen **Redigera klockan**. Här kan du ange den tidszon som visas och välja 12- eller 24-timmarsformat.

![Redigera klockan](../images/7-edit-clock.png)

Om ditt företag har verksamhet i flera länder eller på olika kontinent kan du lägga till ytterligare klockor – var och en i olika tidszoner.

### <a name="accepting-your-edits"></a>Godkänna redigeringarna

När du är klar med att ordna panelerna kan du klicka på **Anpassningen är klar** eller högerklicka och välja **Anpassningen är klar**.

## <a name="edit-a-dashboard-by-changing-the-json-file"></a>Redigera en instrumentpanel genom att ändra .JSON-filen

Du kan även redigera en instrumentpanel genom att ändra .JSON-filen. Med den här metoden får du fler alternativ för att ändra inställningar, men ändringarna visas inte förrän du har laddat upp filen till Azure igen.

![JSON-inställningar](../images/7-json-code.png)

Följ exemplet ovan för att ändra storlek på panelen och redigera variablerna colSpan och rowSpan. Spara sedan filen och ladda upp den till Azure igen. Filen kan även distribueras till andra användare.

## <a name="reset-a-dashboard"></a>Återställa en instrumentpanel

En instrumentpanel kan när som helst återställas till standardtillståndet. Högerklicka och välj **Återställ till standardtillstånd** i redigeringsläget. En dialogruta öppnas där du ombeds bekräfta att du vill återställa instrumentpanelen.

## <a name="summary"></a>Sammanfattning

Med instrumentpaneler har du ett flexibelt verktyg för att hantera olika aspekter av Azure-tjänsterna via portalen. De gör det enkelt att övervaka status för dina tjänster. Och eftersom de är delbara är det lättare att se till att alla i teamet ser samma data och är medvetna om tillståndet för viktiga komponenter.