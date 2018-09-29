<span data-ttu-id="9587a-101">Nu när vi har tränat modellen är det dags att testa den.</span><span class="sxs-lookup"><span data-stu-id="9587a-101">Now that we've trained our model, it's time to test it.</span></span> <span data-ttu-id="9587a-102">Vi ger modellen nya bilder och ser hur väl den klassificerar dem.</span><span class="sxs-lookup"><span data-stu-id="9587a-102">We'll give the model new images and see how well it classifies it.</span></span>

1. <span data-ttu-id="9587a-103">Klicka på **Quick Test** (Snabbtest) högst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="9587a-103">Click **Quick Test** at the top of the page.</span></span>

    ![Testa modellen](../media/4-portal-click-quick-test.png)

1. <span data-ttu-id="9587a-105">Klicka på **Bläddra bland lokala filer** och bläddra till mappen Quick Tests (Snabbtest) i mappen för modulresurser som du laddade ner tidigare.</span><span class="sxs-lookup"><span data-stu-id="9587a-105">Click **Browse local files**, and then browse to the "Quick Tests" folder in the module resources folder you download previously.</span></span> <span data-ttu-id="9587a-106">Välj **PicassoTest_01.jpg** och klicka på **Öppna**.</span><span class="sxs-lookup"><span data-stu-id="9587a-106">Select **PicassoTest_01.jpg**, and click **Open**.</span></span>

    ![Välja en testbild av Picasso](../media/4-portal-select-test-01.png)

1. <span data-ttu-id="9587a-108">Granska testresultatet i dialogrutan Quick Test (Snabbtest).</span><span class="sxs-lookup"><span data-stu-id="9587a-108">Examine the results of the test in the "Quick Test" dialog.</span></span> <span data-ttu-id="9587a-109">Vad är sannolikheten för att målningen är en Picasso?</span><span class="sxs-lookup"><span data-stu-id="9587a-109">What is the probability that the painting is a Picasso?</span></span> <span data-ttu-id="9587a-110">Vad är sannolikheten att den målats av Rembrandt eller Pollock?</span><span class="sxs-lookup"><span data-stu-id="9587a-110">What is the probability that it's a Rembrandt or Pollock?</span></span>

    ![Välja en testbild av Picasso](../media/4-quick-test-result.png)

1. <span data-ttu-id="9587a-112">Stäng dialogrutan Quick Test (Snabbtest).</span><span class="sxs-lookup"><span data-stu-id="9587a-112">Close the "Quick Test" dialog.</span></span> <span data-ttu-id="9587a-113">Klicka sedan på **Predictions** (Förutsägelser) överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="9587a-113">Then click **Predictions** at the top of the page.</span></span>

    ![Visa tester som utförts](../media/4-portal-select-predictions.png)

1. <span data-ttu-id="9587a-115">Klicka på den testbild som du laddade upp för att visa detaljer i den.</span><span class="sxs-lookup"><span data-stu-id="9587a-115">Click the test image that you uploaded to show a detail of it.</span></span> <span data-ttu-id="9587a-116">Tagga bilden som en ”Picasso” genom att välja **Picasso** i den nedrullningsbara listan och klicka på **Save and close** (Spara och stäng).</span><span class="sxs-lookup"><span data-stu-id="9587a-116">Then tag the image as a "Picasso" by selecting **Picasso** from the drop-down list and clicking **Save and close**.</span></span>

    > <span data-ttu-id="9587a-117">Genom att tagga bilder på det här sättet kan du förfina modellen utan att behöva ladda upp fler träningsbilder.</span><span class="sxs-lookup"><span data-stu-id="9587a-117">By tagging test images this way, you can refine the model without uploading additional training images.</span></span>

    ![Tagga testbilden](../media/4-tag-test-image.png)

1. <span data-ttu-id="9587a-119">Kör ännu ett snabbtest, den här gången med filen som heter **FlowersTest.jpg** i mappen Quick Test (Snabbtest).</span><span class="sxs-lookup"><span data-stu-id="9587a-119">Run another quick test, this time using the file named **FlowersTest.jpg** in the "Quick Test" folder.</span></span> <span data-ttu-id="9587a-120">Bekräfta att den här bilden har tilldelats en låg sannolikhet för att vara målad av Picasso, Rembrandt eller Pollock.</span><span class="sxs-lookup"><span data-stu-id="9587a-120">Confirm that this image is assigned a low probability of being a Picasso, a Rembrandt, or a Pollock.</span></span>

<span data-ttu-id="9587a-121">Modellen är nu tränad och beredd på att identifiera konstverk av utvalda konstnärer.</span><span class="sxs-lookup"><span data-stu-id="9587a-121">The model is trained and ready to go and appears to be skilled at identifying paintings by certain artists.</span></span> <span data-ttu-id="9587a-122">Vi anropar förutsägelseslutpunkten via HTTP och ser vad som händer.</span><span class="sxs-lookup"><span data-stu-id="9587a-122">Let's call the prediction endpoint over HTTP and see what happens.</span></span>