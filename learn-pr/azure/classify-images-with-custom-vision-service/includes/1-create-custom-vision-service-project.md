### <a name="exercise-1-create-a-custom-vision-service-project"></a><span data-ttu-id="939c3-101">Övning 1: Skapa ett Custom Vision Service-projekt</span><span class="sxs-lookup"><span data-stu-id="939c3-101">Exercise 1: Create a Custom Vision Service project</span></span>

<span data-ttu-id="939c3-102">När du ska skapa en modell för bildklassificering med Custom Vision Service börjar du med att skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="939c3-102">The first step in building an image-classification model with the Custom Vision Service is to create a project.</span></span> <span data-ttu-id="939c3-103">I den här övningen ska du skapa ett Custom Vision Service-projekt med hjälp av Custom Vision Service-portalen.</span><span class="sxs-lookup"><span data-stu-id="939c3-103">In this exercise, you will use the Custom Vision Service portal to create a Custom Vision Service project.</span></span>

1. <span data-ttu-id="939c3-104">Öppna [Custom Vision Service-portalen](https://www.customvision.ai/) i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="939c3-104">Open the [Custom Vision Service portal](https://www.customvision.ai/) in your browser.</span></span> <span data-ttu-id="939c3-105">Klicka på **Logga in**.</span><span class="sxs-lookup"><span data-stu-id="939c3-105">Then click **Sign In**.</span></span> 
 
    ![Logga in på Custom Vision Service-portalen](../images/portal-sign-in.png)

    <span data-ttu-id="939c3-107">_Logga in på Custom Vision Service-portalen_</span><span class="sxs-lookup"><span data-stu-id="939c3-107">_Signing in to the Custom Vision Service portal_</span></span>

1. <span data-ttu-id="939c3-108">Om du ombeds logga in ska du göra det med autentiseringsuppgifterna för ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="939c3-108">If you are asked to sign in, do so using the credentials for your Microsoft account.</span></span> <span data-ttu-id="939c3-109">Om du ombeds ge appen åtkomst till din information ska du klicka på **Ja**. Godkänn också tjänstvillkoren om du ombeds göra det.</span><span class="sxs-lookup"><span data-stu-id="939c3-109">If you are asked to let this app access your info, click **Yes**, and if prompted, agree to the terms of service.</span></span>

1. <span data-ttu-id="939c3-110">Klicka på **Nytt projekt** för att skapa ett nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="939c3-110">Click **New Project** to create a new project.</span></span>
  
    ![Skapa ett Custom Vision Service-projekt](../images/portal-click-new-project.png)

    <span data-ttu-id="939c3-112">_Skapa ett Custom Vision Service-projekt_</span><span class="sxs-lookup"><span data-stu-id="939c3-112">_Creating a Custom Vision Service project_</span></span>

1. <span data-ttu-id="939c3-113">Ge projektet namnet ”Artwork” i dialogrutan New project (Nytt projekt) och se till så att alternativet **General** (Allmänt) är valt som domän. Klicka på **Create project** (Skapa projekt).</span><span class="sxs-lookup"><span data-stu-id="939c3-113">In the "New project" dialog, name the project "Artworks," ensure that **General** is selected as the domain, and click **Create project**.</span></span>

    > <span data-ttu-id="939c3-114">Med hjälp av domäner kan du optimera modellen för en viss typ av bilder.</span><span class="sxs-lookup"><span data-stu-id="939c3-114">A domain optimizes a model for specific types of images.</span></span> <span data-ttu-id="939c3-115">Om du till exempel vill klassificera matbilder efter vilka typer av maträtter eller vilket kök som visas kan det vara bra att välja domänen Food (Mat).</span><span class="sxs-lookup"><span data-stu-id="939c3-115">For example, if your goal is to classify food images by the types of food they contain or the ethnicity of the dishes, then it might be helpful to select the Food domain.</span></span> <span data-ttu-id="939c3-116">Domänen General (Allmänt) väljer du i de fall där det inte finns någon passande domän eller om du känner dig osäker på vad du ska välja.</span><span class="sxs-lookup"><span data-stu-id="939c3-116">For scenarios that don't match any of the offered domains, or if you are unsure of which domain to choose, select the General domain.</span></span>

    ![Skapa ett Custom Vision Service-projekt](../images/portal-create-project.png)

    <span data-ttu-id="939c3-118">_Skapa ett Custom Vision Service-projekt_</span><span class="sxs-lookup"><span data-stu-id="939c3-118">_Creating a Custom Vision Service project_</span></span>

<span data-ttu-id="939c3-119">Nästa steg är att ladda upp bilder till projektet och klassificera bilder genom att tilldela taggar.</span><span class="sxs-lookup"><span data-stu-id="939c3-119">The next step is to upload images to the project and assign tags to those images to classify them.</span></span>