Härnäst ska vi titta på hur du skapar och ändrar instrumentpaneler med hjälp av Azure Portal och genom att redigera den underliggande JSON-filen direkt.

## <a name="what-is-a-dashboard"></a>Vad är en instrumentpanel?

En _instrumentpanel_ består av en anpassningsbar panelsamling i användargränssnittet i Azure-portalen. Du kan lägga till, ta bort och placera panelerna för att skapa den vy du vill ha och sedan spara vyn som en instrumentpanel. Det går att ha flera instrumentpaneler och du kan växla mellan dem efter behov. Du kan även dela instrumentpaneler med andra teammedlemmar.

Instrumentpaneler ger dig stor flexibilitet för hur du hanterar Azure. Du kan till exempel skapa instrumentpaneler för specifika roller i organisationen och sedan använda rollbaserad åtkomstkontroll (RBAC) för att kontrollera vem som kan komma åt instrumentpanelen. Din databasadministratör kan således ha en instrumentpanel som innehåller vyer för SQL-databastjänsten, medan Azure Active Directory-administratören har vyer för användare och grupper i Azure AD. Du kan även anpassa portalen mellan produktions- och utvecklingsmiljöer i portalen – skapa en specifik instrumentpanel för varje miljö som du hanterar.

Instrumentpaneler lagras som JSON-filer (JavaScript Object Notation). Det innebär att de kan laddas upp och laddas ned till andra datorer eller delas med andra i Azure-katalogen. Azure lagrar instrumentpaneler i resursgrupper, precis som virtuella datorer eller lagringskonton som du kan hantera i portalen.

> [!TIP]
> Eftersom instrumentpaneler är JSON-filer, du kan också [anpassa dem programmässigt](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically), vilket gör dem övertygande administrativa verktyg. Dessutom kan vissa typer av paneler vara frågebaserade, så uppdateras automatiskt när källdata ändras.

## <a name="explore-the-default-dashboard"></a>Utforska standardinstrumentpanelen

Standardinstrumentpanelen heter ”instrumentpanel”. När du loggar in på portalen visas den här instrumentpanelen och dess fem webbdelar.

![Standardwebbdelar](../media-draft/8-dashboard-default-webparts.png)

Standardwebbdelarna är

1. Alla resurser

1. Komma igång med Azure

1. Snabbstarter och självstudier

1. Marketplace

1. Service Health

## <a name="creating-and-managing-dashboards"></a>Skapa och hantera instrumentpaneler

Överst på instrumentpanelen finns det kontroller för att skapa, ladda upp, ladda ned, redigera och dela en instrumentpanel. Du kan även visa en instrumentpanel i fullskärmsläge samt klona eller ta bort den.

![Anpassa instrumentpanelskontroller](../media-draft/8-customise-dashboard-controls.png)

- [Välj instrumentpanelen](#select-dashboard)
- [Skapa en ny instrumentpanel](#create-new)
- [Ladda upp och ned](#upload-download)
- [Redigera](#edit-dashboard)
- [Filresurs](#share-dashboard)
- [Helskärm](#full-screen)
- [Clone](#clone-dashboard)
- [Ta bort](#delete-dashboard)

<a name="select-dashboard"></a>

## <a name="select-dashboard"></a>Välj instrumentpanelen

Längst till vänster i verktygsfältet är den **Markera en instrumentpanel** nedrullningsbar listruta. Klicka på den här kontrollen om du vill välja bland instrumentpaneler som du redan har definierat för ditt konto. Den här kontrollen gör det enkelt för dig att definiera flera instrumentpaneler för olika syften och sedan växla från varandra och tillbaka igen, beroende på vad du försöker göra när.

Observera att alla instrumentpaneler som du skapar först är privata. Det vill säga att bara du kan se dem. Du måste dela en instrumentpanel om du vill göra den tillgänglig för hela företaget. Vi ska titta på det alternativet inom kort.

<a name="create-new"></a>

## <a name="create-a-new-dashboard"></a>Skapa en ny instrumentpanel

Om du vill skapa en ny instrumentpanel klickar du på **Ny instrumentpanel**. Arbetsytan för instrumentpanel öppnas utan paneler. Du kan sedan lägga till, ta bort och justera paneler, hur du vill. När du är klar med att anpassa instrumentpanelen klickar du på **Anpassningen är klar** för att spara och växla till den instrumentpanelen.

<a name="upload-download"></a>

## <a name="upload-and-download"></a>Ladda upp och ladda ned

Med knapparna **Ladda upp** och **Ladda ned** kan du ladda ned din befintliga instrumentpanel som en JSON-fil, anpassa den och sedan distribuera och ladda upp den eller låta någon annan ladda upp filen igen till Azure Portal, så att deras befintliga instrumentpanel ersätts.

Om du klickar på **Ladda ned** laddas den aktuella instrumentpanelen ned till din standardmapp för nedladdningar. När du öppnar den nedladdade filen visas JSON-koden.

![JSON-kod för instrumentpanel](../media-draft/8-dashboard-json-code.png)

Du kan sedan redigera koden manuellt (till exempel genom att ändra panelstorlekar) och sedan ladda upp till Azure genom att klicka på knappen **Ladda upp**.

<a name="edit-dashboard"></a>

### <a name="edit-a-dashboard"></a>Redigera en instrumentpanel

Men du kan redigera en instrumentpanel genom att ladda ned JSON-filen, tillbaka ändra värden i filen och ladda upp filen till Azure, att metoden inte är intuitivt för att designa ett användargränssnitt. Om du vill använda det grafiska Användargränssnittet för att konfigurera din aktuella instrumentpanel som du kan ange redigeringsläget på flera olika sätt:

1. Klicka på den **redigera** knappen
1. Högerklicka på instrumentpanelen och klicka på **redigera**. 
1. Hovra över en panel på instrumentpanelen – en `...` menyn visas i det övre högra hörnet med redigeringsalternativ för.

Instrumentpanelen övergår till redigeringsläget.

![Redigera instrumentpanel](../media-draft/8-edit-dashboard.png)

På vänster sida visas panelgalleriet med flera tillgängliga paneler. Du kan filtrera Panelgalleriet efter kategori och resurs-typ:

![Panelgalleri](../media-draft/8-tile-gallery.png)

Att lägga till paneler är lika enkelt som att välja panelen i listan till vänster och dra den till arbetsytan. Sedan kan du flytta varje panel till önskad plats, ändra dess storlek eller vilka data som visas.

> [!TIP]
> En fiffig funktion som en massa personer inte känner till är att du kan ta element på underordnade blad och placerar dem i din instrumentpanel. Bara hovra över objektet och leta efter den `...` ikonen Redigera-menyn – detta har ett ”Fäst på instrumentpanelen”-alternativ som gör att du snabbt hämta en panel från en tjänst och placera den på instrumentpanelen.

Arbetsytan är indelad i rutor i redigeringsläget. Varje panel måste fylla minst en ruta och panelerna fästs intill närmaste största panelavgränsare. Överlappande paneler flyttas undan. När du gör en panel mindre flyttas omgivande paneler och placeras intill den panelen.

#### <a name="change-tile-sizes"></a>Ändra panelstorlekar

Vissa paneler har en storlek och du kan redigera deras storlek endast programmässigt. Du kan däremot redigera paneler med ett grått hörn längst ned till höger genom att dra hörnindikatorn.

![Panel som kan storleksanpassas](../media-draft/8-resizable-tile.png)

Du kan också högerklicka på snabbmenyn och ange önskad storlek.

![Panelstorlek](../media-draft/8-tile-size.png)

Hämta paneler från Panelgalleriet till arbetsytan och sedan ordna dem för att skapa din instrumentpanel.

#### <a name="change-tile-settings"></a>Ändra panelinställningar

Vissa paneler har inställningar som kan redigeras. När du till exempel drar klockpanelen till arbetsytan öppnas panelen **Redigera klockan**. Här kan du ange den tidszon som visas och välja 12- eller 24-timmarsformat.

![Redigera klockan](../media-draft/8-edit-clock.png)

Om ditt företag har verksamhet i flera länder eller på olika kontinenter kan du lägga till klockor – var och en i olika tidszoner.

#### <a name="accepting-your-edits"></a>Godkänna redigeringarna

När du är klar med att ordna panelerna kan du klicka på **Anpassningen är klar** eller högerklicka och välja **Anpassningen är klar**.

## <a name="edit-a-dashboard-by-changing-the-json-file"></a>Redigera en instrumentpanel genom att ändra JSON-filen

Du kan även redigera en instrumentpanel genom att ändra JSON-filen. Med den här metoden får du fler alternativ för att ändra inställningar, men ändringarna visas inte förrän du har laddat upp filen till Azure igen.

![JSON-inställningar](../media-draft/8-json-code.png)

I exemplet ovan ändrar du storlek på panelen genom att redigera variablerna **colSpan** och **rowSpan**. Spara sedan filen och ladda upp den till Azure igen. Filen kan även distribueras till andra användare.

## <a name="reset-a-dashboard"></a>Återställa en instrumentpanel

En instrumentpanel kan när som helst återställas till standardtillståndet. Högerklicka och välj **Återställ till standardtillstånd** i redigeringsläget. En dialogruta öppnas där du ombeds bekräfta att du vill återställa instrumentpanelen.

<a name="share-dashboard"></a>

## <a name="share-or-unshare-a-dashboard"></a>Dela eller sluta dela en instrumentpanel

När du definierar en ny instrumentpanel är den privat och syns endast på ditt konto. Om du vill göra instrumentpanelen synlig för andra måste du dela den. Som för alla andra Azure-resurser behöver du ange en resursgrupp (eller använda en befintlig resursgrupp) för att lagra delade instrumentpaneler. Om du inte har en resursgrupp skapar Azure en resursgrupp för *instrumentpaneler* på den plats som du anger. Om du har resursgrupper kan du välja en resursgrupp för att lagra instrumentpaneler.

![Delning och åtkomstkontroll 1](../media-draft/8-share-dashboards-default.png)

När du har delat mallen visas andra bladet för **Delning och åtkomstkontroll**.

![Delning och åtkomstkontroll 2](../media-draft/8-share-dashboards-access-control.png)

Nu kan du klicka på **Hantera användare** och ange vilka användare som ska ha åtkomst till instrumentpanelen.

### <a name="switching-to-a-shared-dashboard"></a>Växla till en delad instrumentpanel

Om du vill växla till en delad instrumentpanel klickar du på listan över instrumentpaneler och sedan på **Bläddra bland alla instrumentpaneler**.

![Bläddra bland alla instrumentpaneler](../media-draft/8-browse-dashboards.png)

Nu visas bladet **Alla instrumentpaneler**, och namnen på alla delade instrumentpaneler visas. Klicka bara på en instrumentpanel för att tillämpa den på Azure-portalen.

![Delade instrumentpaneler](../media-draft/8-select-shared-dashboard.png)

<a name="full-screen"></a>

## <a name="display-a-dashboard-as-a-full-screen"></a>Visa en instrumentpanel i fullskärmsläge

Om du vill att den största fastigheten instrumentpanelen, klickar du på den **helskärm** knappen för att visa din aktuella instrumentpanel utan någon Webbläsarmenyer. Om det finns paneler utanför skärmvisningens gränser visas skjutreglage till höger och längst ned på skärmen.

När du är klar med att arbeta i fullskärmsläge trycker du på ESC eller klickar på **Avsluta fullskärmsläge** bredvid instrumentpanelens namn högst upp på skärmen.

<a name="clone-dashboard"></a>

## <a name="clone-a-dashboard"></a>Klona en instrumentpanel

Kloning av en instrumentpanel skapar en omedelbar kopia som kallas ”klona av \<instrumentpanelens namn >” och växlar till den kopian som den aktuella instrumentpanelen. Det är också ett enkelt sätt att skapa instrumentpaneler innan du delar dem att Kloningen. Till exempel om du har en instrumentpanel som är nästan som du vill klona den, gör de ändringar som du behöver och sedan dela den.

<a name="delete-dashboard"></a>

## <a name="delete-a-dashboard"></a>Ta bort en instrumentpanel

När du tar bort en instrumentpanel tas den även bort från listan med tillgängliga instrumentpaneler. Du uppmanas att bekräfta att instrumentpanelen ska tas bort. Det går inte att återställa en instrumentpanel som har tagits bort.

Nu ska vi prova att använda några av dessa alternativ genom att skapa en ny instrumentpanel.
