<span data-ttu-id="2b167-101">I den här enheten lägger du till bilder med berömda konstverk av Picasso, Pollock och Rembrandt i Artworks-projektet och taggar dem så att Custom Vision Service kan lära sig att skilja mellan olika konstnärer.</span><span class="sxs-lookup"><span data-stu-id="2b167-101">In this unit, you will add images of famous paintings by Picasso, Pollock, and Rembrandt to the Artworks project, and tag the images so the Custom Vision Service can learn to differentiate one artist from another.</span></span>
  
1. <span data-ttu-id="2b167-102">Klicka på **Lägg till bilder** för att lägga till bilder i projektet.</span><span class="sxs-lookup"><span data-stu-id="2b167-102">Click **Add images** to add images to the project.</span></span>

    ![Lägga till bilder i Artworks-projektet](../media-draft/2-portal-click-add-images.png)

1. <span data-ttu-id="2b167-104">Klicka på **Bläddra bland lokala filer**.</span><span class="sxs-lookup"><span data-stu-id="2b167-104">Click **Browse local files**.</span></span>

    ![Bläddra bland lokala bilder](../media-draft/2-portal-click-browse-local-files.png)

    <span data-ttu-id="2b167-106">_Bläddra bland lokala bilder_</span><span class="sxs-lookup"><span data-stu-id="2b167-106">_Browsing for local images_</span></span> 
 
1. <span data-ttu-id="2b167-107">Bläddra till mappen ”Artists\Picasso” i [resursen som medföljer den här modulen](https://a4r.blob.core.windows.net/public/cvs-resources.zip), markera alla filer i mappen och klicka på **Öppna**.</span><span class="sxs-lookup"><span data-stu-id="2b167-107">Browse to the "Artists\Picasso" folder in the [resources that accompany this module](https://a4r.blob.core.windows.net/public/cvs-resources.zip), select all of the files in the folder, and click **Open**.</span></span>

    ![Välja en bild](../media-draft/2-fe-browse-picasso-01.png)

1. <span data-ttu-id="2b167-109">Skriv ”painting” (utan citattecken) i fältet **Lägg till några taggar...**</span><span class="sxs-lookup"><span data-stu-id="2b167-109">Type "painting" (without quotation marks) into the **Add some tags...** box.</span></span> <span data-ttu-id="2b167-110">Klicka sedan på **+** för att tilldela taggen till bilderna.</span><span class="sxs-lookup"><span data-stu-id="2b167-110">Then click **+** to assign the tag to the images.</span></span>

    ![Lägga till taggen ”painting” för bilderna](../media-draft/2-portal-add-tags-01.png)

1. <span data-ttu-id="2b167-112">Upprepa steg 4 och lägg till taggen ”Picasso” för bilderna.</span><span class="sxs-lookup"><span data-stu-id="2b167-112">Repeat Step 4 to add a "Picasso" tag to the images.</span></span>

1. <span data-ttu-id="2b167-113">Klicka på **Ladda upp 7 filer** för att ladda upp bilderna.</span><span class="sxs-lookup"><span data-stu-id="2b167-113">Click **Upload 7 files** to upload the images.</span></span> <span data-ttu-id="2b167-114">När överföringen är klar klickar du på **Klart**.</span><span class="sxs-lookup"><span data-stu-id="2b167-114">Once the upload has finished, click **Done**.</span></span>

    ![Ladda upp taggade bilder](../media-draft/2-upload-picasso-images.png)

1. <span data-ttu-id="2b167-116">Bekräfta att bilderna du laddat upp visas i portalen tillsammans med de taggar du tilldelat.</span><span class="sxs-lookup"><span data-stu-id="2b167-116">Confirm that the images you uploaded appear in the portal, along with the tags assigned to them.</span></span>

    ![De uppladdade bilderna](../media-draft/2-portal-tagged-01.png)

1. <span data-ttu-id="2b167-118">Med sju målningar av Picasso kan Custom Vision Service identifiera konstverk av Picasso ganska bra.</span><span class="sxs-lookup"><span data-stu-id="2b167-118">With seven Picasso images, the Custom Vision Service can do a decent job of identifying paintings by Picasso.</span></span> <span data-ttu-id="2b167-119">Men om du skulle träna modellen nu skulle den bara förstå hur målningar av Picasso ser ut och inte kunna identifiera konstverk av andra konstnärer.</span><span class="sxs-lookup"><span data-stu-id="2b167-119">But if you trained the model right now, it would only understand what a Picasso looks like, and it would not be able to identify paintings by other artists.</span></span>

    <span data-ttu-id="2b167-120">Nästa steg är att ladd upp målningar av en annan konstnär.</span><span class="sxs-lookup"><span data-stu-id="2b167-120">The next step is to upload some paintings by another artist.</span></span> <span data-ttu-id="2b167-121">Klicka på **Lägg till bilder** och markera alla bilder i mappen ”Artists\Rembrandt” i modulresurserna.</span><span class="sxs-lookup"><span data-stu-id="2b167-121">Click **Add images** and select all of the images in the "Artists\Rembrandt" folder in the module resources.</span></span> <span data-ttu-id="2b167-122">Tagga dem med etiketterna ”painting” och ”Rembrandt” (inte ”Picasso”) och ladda upp dem till projektet.</span><span class="sxs-lookup"><span data-stu-id="2b167-122">Tag them with the labels "painting" and "Rembrandt" (not "Picasso"), and upload them to the project.</span></span>

    > <span data-ttu-id="2b167-123">När du lägger till taggen ”painting” behöver du inte skriva in den igen.</span><span class="sxs-lookup"><span data-stu-id="2b167-123">When you add the tag "painting," you don't have to type it in again.</span></span> <span data-ttu-id="2b167-124">Du kan välja den från listrutan vid fältet **Lägg till några taggar ...** , se nedan.</span><span class="sxs-lookup"><span data-stu-id="2b167-124">You can select it from the drop-down list attached to the **Add some tags...** box, as shown below.</span></span> <span data-ttu-id="2b167-125">Du **måste** däremot skriva ”Rembrandt” och klicka på **+** för att lägga till en ”Rembrandt”-tagg.</span><span class="sxs-lookup"><span data-stu-id="2b167-125">You **will** have to type "Rembrandt" and click **+** to add a "Rembrandt" tag.</span></span>

    ![Välja en befintlig tagg](../media-draft/2-select-painting-tag.png)

1. <span data-ttu-id="2b167-127">Kontrollera att Rembrandt-bilderna visas tillsammans med Picasso-bilderna i projektet och att ”Rembrandt” visas i listan med taggar.</span><span class="sxs-lookup"><span data-stu-id="2b167-127">Confirm that the Rembrandt images appear alongside the Picasso images in the project, and that "Rembrandt" appears in the list of tags.</span></span>

    ![Bilder av Picasso och Rembrandt](../media-draft/2-portal-tagged-02.png)

1. <span data-ttu-id="2b167-129">Lägg nu till konstverk av den gåtfulle konstnären Jackson Pollock så att Custom Vision Service även ska kunna identifiera konstverk av Pollock.</span><span class="sxs-lookup"><span data-stu-id="2b167-129">Now add paintings by the enigmatic artist Jackson Pollock to enable the Custom Vision Service to recognize Pollock paintings, too.</span></span> <span data-ttu-id="2b167-130">Välj alla bilder i mappen ”Artists\Pollock” i modulresurserna, tagga dem med termerna ”painting” och ”Pollock” och ladda upp dem till projektet.</span><span class="sxs-lookup"><span data-stu-id="2b167-130">Select all of the images in the "Artists\Pollock" folder in the module resources, tag them with the terms "painting" and "Pollock", and upload them to the project.</span></span>

<span data-ttu-id="2b167-131">När de taggade bilderna är uppladdade är nästa steg att träna modellen med bilderna så att den kan skilja mellan konstverk målade av Picasso, Rembrandt och Pollock, och avgöra om en tavla är målad av någon av de här konstnärerna.</span><span class="sxs-lookup"><span data-stu-id="2b167-131">With the tagged images uploaded, the next step is to train the model with these images so it can distinguish between paintings by Picasso, Rembrandt, and Pollock, as well as determine whether a painting is a work by one of these famous artists.</span></span>