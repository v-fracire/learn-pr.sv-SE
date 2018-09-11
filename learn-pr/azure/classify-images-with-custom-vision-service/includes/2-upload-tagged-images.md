### <a name="exercise-2-upload-tagged-images"></a><span data-ttu-id="97955-101">Övning 2: Ladda upp taggade bilder</span><span class="sxs-lookup"><span data-stu-id="97955-101">Exercise 2: Upload tagged images</span></span>

<span data-ttu-id="97955-102">I den här övningen lägger du till bilder med berömda konstverk av Picasso, Pollock och Rembrandt i Artworks-projektet och taggar dem så att Custom Vision Service kan lära sig att skilja mellan olika konstnärer.</span><span class="sxs-lookup"><span data-stu-id="97955-102">In this exercise, you will add images of famous paintings by Picasso, Pollock, and Rembrandt to the Artworks project, and tag the images so the Custom Vision Service can learn to differentiate one artist from another.</span></span>
  
1. <span data-ttu-id="97955-103">Klicka på **Lägg till bilder** för att lägga till bilder i projektet.</span><span class="sxs-lookup"><span data-stu-id="97955-103">Click **Add images** to add images to the project.</span></span>

    ![Lägga till bilder i Artworks-projektet](../images/portal-click-add-images.png)

    <span data-ttu-id="97955-105">_Lägga till bilder i Artworks-projektet_</span><span class="sxs-lookup"><span data-stu-id="97955-105">_Adding images to the Artworks project_</span></span> 
 
1. <span data-ttu-id="97955-106">Klicka på **Bläddra bland lokala filer**.</span><span class="sxs-lookup"><span data-stu-id="97955-106">Click **Browse local files**.</span></span>

    ![Bläddra bland lokala bilder](../images/portal-click-browse-local-files.png)

    <span data-ttu-id="97955-108">_Bläddra bland lokala bilder_</span><span class="sxs-lookup"><span data-stu-id="97955-108">_Browsing for local images_</span></span> 
 
1. <span data-ttu-id="97955-109">Bläddra till mappen ”Artists\Picasso” i [resursen som medföljer den här labben](https://a4r.blob.core.windows.net/public/cvs-resources.zip), markera alla filer i mappen och klicka på **Öppna**.</span><span class="sxs-lookup"><span data-stu-id="97955-109">Browse to the "Artists\Picasso" folder in the [resources that accompany this lab](https://a4r.blob.core.windows.net/public/cvs-resources.zip), select all of the files in the folder, and click **Open**.</span></span>

    ![Välja en bild](../images/fe-browse-picasso-01.png)

    <span data-ttu-id="97955-111">_Välja en bild_</span><span class="sxs-lookup"><span data-stu-id="97955-111">_Selecting an image_</span></span> 
 
1. <span data-ttu-id="97955-112">Skriv ”painting” (utan citattecken) i fältet **Lägg till några taggar ...** .</span><span class="sxs-lookup"><span data-stu-id="97955-112">Type "painting" (without quotation marks) into the **Add some tags...** box.</span></span> <span data-ttu-id="97955-113">Klicka sedan på **+** för att tilldela taggen till bilderna.</span><span class="sxs-lookup"><span data-stu-id="97955-113">Then click **+** to assign the tag to the images.</span></span>

    ![Lägga till taggen ”painting” för bilderna](../images/portal-add-tags-01.png)

    <span data-ttu-id="97955-115">_Lägga till taggen ”painting” för bilderna_</span><span class="sxs-lookup"><span data-stu-id="97955-115">_Adding a "painting" tag to the images_</span></span> 

1. <span data-ttu-id="97955-116">Upprepa steg 4 och lägg till taggen ”Picasso” för bilderna.</span><span class="sxs-lookup"><span data-stu-id="97955-116">Repeat Step 4 to add a "Picasso" tag to the images.</span></span>

1. <span data-ttu-id="97955-117">Klicka på **Ladda upp 7 filer** för att ladda upp bilderna.</span><span class="sxs-lookup"><span data-stu-id="97955-117">Click **Upload 7 files** to upload the images.</span></span> <span data-ttu-id="97955-118">När överföringen är färdig klickar du på **Klart**.</span><span class="sxs-lookup"><span data-stu-id="97955-118">Once the upload has completed, click **Done**.</span></span>

    ![Ladda upp taggade bilder](../images/upload-picasso-images.png)

    <span data-ttu-id="97955-120">_Ladda upp taggade bilder_</span><span class="sxs-lookup"><span data-stu-id="97955-120">_Uploading tagged images_</span></span> 

1. <span data-ttu-id="97955-121">Bekräfta att bilderna du laddat upp visas i portalen tillsammans med de taggar du tilldelat.</span><span class="sxs-lookup"><span data-stu-id="97955-121">Confirm that the images you uploaded appear in the portal, along with the tags assigned to them.</span></span>

    ![De uppladdade bilderna](../images/portal-tagged-01.png)

    <span data-ttu-id="97955-123">_De uppladdade bilderna_</span><span class="sxs-lookup"><span data-stu-id="97955-123">_The uploaded images_</span></span> 

1. <span data-ttu-id="97955-124">Med sju målningar av Picasso kan Custom Vision Service identifiera konstverk av Picasso ganska bra.</span><span class="sxs-lookup"><span data-stu-id="97955-124">With seven Picasso images, the Custom Vision Service can do a decent job of identifying paintings by Picasso.</span></span> <span data-ttu-id="97955-125">Men om du skulle träna modellen nu skulle den bara förstå hur målningar av Picasso ser ut och inte kunna identifiera konstverk av andra konstnärer.</span><span class="sxs-lookup"><span data-stu-id="97955-125">But if you trained the model right now, it would only understand what a Picasso looks like, and it would not be able to identify paintings by other artists.</span></span>

    <span data-ttu-id="97955-126">Nästa steg är att ladd upp målningar av en annan konstnär.</span><span class="sxs-lookup"><span data-stu-id="97955-126">The next step is to upload some paintings by another artist.</span></span> <span data-ttu-id="97955-127">Klicka på **Lägg till bilder** och markera alla bilder i mappen ”Artists\Rembrandt” i labbresursen.</span><span class="sxs-lookup"><span data-stu-id="97955-127">Click **Add images** and select all of the images in the "Artists\Rembrandt" folder in the lab resources.</span></span> <span data-ttu-id="97955-128">Tagga dem med etiketterna ”painting” och ”Rembrandt” (inte ”Picasso”) och ladda upp dem till projektet.</span><span class="sxs-lookup"><span data-stu-id="97955-128">Tag them with the labels "painting" and "Rembrandt" (not "Picasso"), and upload them to the project.</span></span>

    > <span data-ttu-id="97955-129">När du lägger till taggen ”painting” behöver du inte skriva in den igen.</span><span class="sxs-lookup"><span data-stu-id="97955-129">When you add the tag "painting," you don't have to type it in again.</span></span> <span data-ttu-id="97955-130">Du kan välja den från listrutan vid fältet **Lägg till några taggar ...** , se nedan.</span><span class="sxs-lookup"><span data-stu-id="97955-130">You can select it from the drop-down list attached to the **Add some tags...** box, as shown below.</span></span> <span data-ttu-id="97955-131">Du **måste** däremot skriva ”Rembrandt” och klicka på **+** för att lägga till en ”Rembrandt”-tagg.</span><span class="sxs-lookup"><span data-stu-id="97955-131">You **will** have to type "Rembrandt" and click **+** to add a "Rembrandt" tag.</span></span>

    ![Välja en befintlig tagg](../images/select-painting-tag.png)

    <span data-ttu-id="97955-133">_Välja en befintlig tagg_</span><span class="sxs-lookup"><span data-stu-id="97955-133">_Selecting an existing tag_</span></span> 

1. <span data-ttu-id="97955-134">Kontrollera att Rembrandt-bilderna visas tillsammans med Picasso-bilderna i projektet och att ”Rembrandt” visas i listan med taggar.</span><span class="sxs-lookup"><span data-stu-id="97955-134">Confirm that the Rembrandt images appear alongside the Picasso images in the project, and that "Rembrandt" appears in the list of tags.</span></span>

    ![Bilder av Picasso och Rembrandt](../images/portal-tagged-02.png)

    <span data-ttu-id="97955-136">_Bilder av Picasso och Rembrandt_</span><span class="sxs-lookup"><span data-stu-id="97955-136">_Picasso and Rembrandt images_</span></span> 

1. <span data-ttu-id="97955-137">Lägg nu till konstverk av den gåtfulle konstnären Jackson Pollock så att Custom Vision Service även ska kunna identifiera konstverk av Pollock.</span><span class="sxs-lookup"><span data-stu-id="97955-137">Now add paintings by the enigmatic artist Jackson Pollock to enable the Custom Vision Service to recognize Pollock paintings, too.</span></span> <span data-ttu-id="97955-138">Välj alla bilder i mappen ”Artists\Pollock” i labbresursen, tagga dem med termerna ”painting” och ”Pollock” och ladda upp dem till projektet.</span><span class="sxs-lookup"><span data-stu-id="97955-138">Select all of the images in the "Artists\Pollock" folder in the lab resources, tag them with the terms "painting" and "Pollock", and upload them to the project.</span></span>

<span data-ttu-id="97955-139">När de taggade bilderna är uppladdade är nästa steg att träna modellen med bilderna så att den kan skilja mellan konstverk målade av Picasso, Rembrandt och Pollock, och avgöra om en tavla är målad av någon av de här konstnärerna.</span><span class="sxs-lookup"><span data-stu-id="97955-139">With the tagged images uploaded, the next step is to train the model with these images so it can distinguish between paintings by Picasso, Rembrandt, and Pollock, as well as determine whether a painting is a work by one of these famous artists.</span></span>