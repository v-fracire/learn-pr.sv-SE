### <a name="exercise-2-upload-tagged-images"></a>Övning 2: Ladda upp taggade bilder

I den här övningen lägger du till bilder med berömda konstverk av Picasso, Pollock och Rembrandt i Artworks-projektet och taggar dem så att Custom Vision Service kan lära sig att skilja mellan olika konstnärer.
  
1. Klicka på **Lägg till bilder** för att lägga till bilder i projektet.

    ![Lägga till bilder i Artworks-projektet](../images/portal-click-add-images.png)

    _Lägga till bilder i Artworks-projektet_ 
 
1. Klicka på **Bläddra bland lokala filer**.

    ![Bläddra bland lokala bilder](../images/portal-click-browse-local-files.png)

    _Bläddra bland lokala bilder_ 
 
1. Bläddra till mappen ”Artists\Picasso” i [resursen som medföljer den här labben](https://a4r.blob.core.windows.net/public/cvs-resources.zip), markera alla filer i mappen och klicka på **Öppna**.

    ![Välja en bild](../images/fe-browse-picasso-01.png)

    _Välja en bild_ 
 
1. Skriv ”painting” (utan citattecken) i fältet **Lägg till några taggar ...** . Klicka sedan på **+** för att tilldela taggen till bilderna.

    ![Lägga till taggen ”painting” för bilderna](../images/portal-add-tags-01.png)

    _Lägga till taggen ”painting” för bilderna_ 

1. Upprepa steg 4 och lägg till taggen ”Picasso” för bilderna.

1. Klicka på **Ladda upp 7 filer** för att ladda upp bilderna. När överföringen är färdig klickar du på **Klart**.

    ![Ladda upp taggade bilder](../images/upload-picasso-images.png)

    _Ladda upp taggade bilder_ 

1. Bekräfta att bilderna du laddat upp visas i portalen tillsammans med de taggar du tilldelat.

    ![De uppladdade bilderna](../images/portal-tagged-01.png)

    _De uppladdade bilderna_ 

1. Med sju målningar av Picasso kan Custom Vision Service identifiera konstverk av Picasso ganska bra. Men om du skulle träna modellen nu skulle den bara förstå hur målningar av Picasso ser ut och inte kunna identifiera konstverk av andra konstnärer.

    Nästa steg är att ladd upp målningar av en annan konstnär. Klicka på **Lägg till bilder** och markera alla bilder i mappen ”Artists\Rembrandt” i labbresursen. Tagga dem med etiketterna ”painting” och ”Rembrandt” (inte ”Picasso”) och ladda upp dem till projektet.

    > När du lägger till taggen ”painting” behöver du inte skriva in den igen. Du kan välja den från listrutan vid fältet **Lägg till några taggar ...** , se nedan. Du **måste** däremot skriva ”Rembrandt” och klicka på **+** för att lägga till en ”Rembrandt”-tagg.

    ![Välja en befintlig tagg](../images/select-painting-tag.png)

    _Välja en befintlig tagg_ 

1. Kontrollera att Rembrandt-bilderna visas tillsammans med Picasso-bilderna i projektet och att ”Rembrandt” visas i listan med taggar.

    ![Bilder av Picasso och Rembrandt](../images/portal-tagged-02.png)

    _Bilder av Picasso och Rembrandt_ 

1. Lägg nu till konstverk av den gåtfulle konstnären Jackson Pollock så att Custom Vision Service även ska kunna identifiera konstverk av Pollock. Välj alla bilder i mappen ”Artists\Pollock” i labbresursen, tagga dem med termerna ”painting” och ”Pollock” och ladda upp dem till projektet.

När de taggade bilderna är uppladdade är nästa steg att träna modellen med bilderna så att den kan skilja mellan konstverk målade av Picasso, Rembrandt och Pollock, och avgöra om en tavla är målad av någon av de här konstnärerna.