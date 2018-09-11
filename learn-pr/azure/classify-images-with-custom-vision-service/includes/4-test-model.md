### <a name="exercise-4-test-the-model"></a><span data-ttu-id="48cfe-101">Övning 4: Testa modellen</span><span class="sxs-lookup"><span data-stu-id="48cfe-101">Exercise 4: Test the model</span></span>

<span data-ttu-id="48cfe-102">I [övning 5](../5-build-app.yml) ska du skapa en Node.js-app som använder modellen till att känna igen konstnären bakom ett verk.</span><span class="sxs-lookup"><span data-stu-id="48cfe-102">In [Exercise 5](../5-build-app.yml), you will create a Node.js app that uses the model to identify the artist of paintings presented to it.</span></span> <span data-ttu-id="48cfe-103">Du behöver inte skriva någon ny app för att testa modellen. Du kan göra dina tester i portalen och förfina modellen med hjälp av testbilderna.</span><span class="sxs-lookup"><span data-stu-id="48cfe-103">But you don't have to write an app to test the model; you can do your testing in the portal, and you can further refine the model using the images that you test with.</span></span> <span data-ttu-id="48cfe-104">I den här övningen testar du modellens förmåga att känna igen konstnären bakom verket med utgångspunkt från testbilderna.</span><span class="sxs-lookup"><span data-stu-id="48cfe-104">In this exercise, you will test the model's ability to identify the artist of a painting using test images provided for you.</span></span>

1. <span data-ttu-id="48cfe-105">Klicka på **Quick Test** (Snabbtest) högst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="48cfe-105">Click **Quick Test** at the top of the page.</span></span>
 
    ![Testa modellen](../images/portal-click-quick-test.png)

    <span data-ttu-id="48cfe-107">_Testa modellen_</span><span class="sxs-lookup"><span data-stu-id="48cfe-107">_Testing the model_</span></span> 

1. <span data-ttu-id="48cfe-108">Klicka på **Bläddra bland lokala filer** och bläddra till mappen Quick Tests (Snabbtest) bland labbresurserna.</span><span class="sxs-lookup"><span data-stu-id="48cfe-108">Click **Browse local files**, and then browse to the "Quick Tests" folder in the lab resources.</span></span> <span data-ttu-id="48cfe-109">Välj **PicassoTest_01.jpg** och klicka på **Öppna**.</span><span class="sxs-lookup"><span data-stu-id="48cfe-109">Select **PicassoTest_01.jpg**, and click **Open**.</span></span>

    ![Välja en testbild av Picasso](../images/portal-select-test-01.png)

    <span data-ttu-id="48cfe-111">_Välja en testbild av Picasso_</span><span class="sxs-lookup"><span data-stu-id="48cfe-111">_Selecting a Picasso test image_</span></span> 

1. <span data-ttu-id="48cfe-112">Granska testresultatet i dialogrutan Quick Test (Snabbtest).</span><span class="sxs-lookup"><span data-stu-id="48cfe-112">Examine the results of the test in the "Quick Test" dialog.</span></span> <span data-ttu-id="48cfe-113">Vad är sannolikheten för att målningen är en Picasso?</span><span class="sxs-lookup"><span data-stu-id="48cfe-113">What is the probability that the painting is a Picasso?</span></span> <span data-ttu-id="48cfe-114">Vad är sannolikheten att den målats av Rembrandt eller Pollock?</span><span class="sxs-lookup"><span data-stu-id="48cfe-114">What is the probability that it is a Rembrandt or Pollock?</span></span>

1. <span data-ttu-id="48cfe-115">Stäng dialogrutan Quick Test (Snabbtest).</span><span class="sxs-lookup"><span data-stu-id="48cfe-115">Close the "Quick Test" dialog.</span></span> <span data-ttu-id="48cfe-116">Klicka sedan på **Predictions** (Förutsägelser) överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="48cfe-116">Then click **Predictions** at the top of the page.</span></span>
 
    ![Visa tester som utförts](../images/portal-select-predictions.png)

    <span data-ttu-id="48cfe-118">_Visa tester som utförts_</span><span class="sxs-lookup"><span data-stu-id="48cfe-118">_Viewing the tests that have been performed_</span></span> 

1. <span data-ttu-id="48cfe-119">Klicka på den testbild som du laddade upp för att visa detaljer i den.</span><span class="sxs-lookup"><span data-stu-id="48cfe-119">Click the test image that you uploaded to show a detail of it.</span></span> <span data-ttu-id="48cfe-120">Tagga bilden som en ”Picasso” genom att välja **Picasso** i den nedrullningsbara listan och klicka på **Save and close** (Spara och stäng).</span><span class="sxs-lookup"><span data-stu-id="48cfe-120">Then tag the image as a "Picasso" by selecting **Picasso** from the drop-down list and clicking **Save and close**.</span></span>

    > <span data-ttu-id="48cfe-121">Genom att tagga bilder på det här sättet kan du förfina modellen utan att behöva ladda upp fler träningsbilder.</span><span class="sxs-lookup"><span data-stu-id="48cfe-121">By tagging test images this way, you can refine the model without uploading additional training images.</span></span>
 
    ![Tagga testbilden](../images/tag-test-image.png)

    <span data-ttu-id="48cfe-123">_Tagga testbilden_</span><span class="sxs-lookup"><span data-stu-id="48cfe-123">_Tagging the test image_</span></span> 

1. <span data-ttu-id="48cfe-124">Gör ännu ett snabbtest med filen som heter **FlowersTest.jpg** i mappen Quick Test (Snabbtest).</span><span class="sxs-lookup"><span data-stu-id="48cfe-124">Perform another quick test using the file named **FlowersTest.jpg** in the "Quick Test" folder.</span></span> <span data-ttu-id="48cfe-125">Bekräfta att den här bilden har tilldelats en låg sannolikhet för att vara målad av Picasso, Rembrandt eller Pollock.</span><span class="sxs-lookup"><span data-stu-id="48cfe-125">Confirm that this image is assigned a low probability of being a Picasso, a Rembrandt, or a Pollock.</span></span>

<span data-ttu-id="48cfe-126">Modellen är nu tränad och beredd på att identifiera konstverk av utvalda konstnärer.</span><span class="sxs-lookup"><span data-stu-id="48cfe-126">The model is trained and ready to go and appears to be adept at identifying paintings by certain artists.</span></span> <span data-ttu-id="48cfe-127">Nu ska vi ta det hela ett steg längre och införliva modellens intelligens i en app.</span><span class="sxs-lookup"><span data-stu-id="48cfe-127">Now let's go a step further and incorporate the model's intelligence into an app.</span></span>