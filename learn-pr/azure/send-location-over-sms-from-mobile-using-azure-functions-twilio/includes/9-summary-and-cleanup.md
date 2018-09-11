<span data-ttu-id="1f10a-101">Du skapat har en mobilapp för flera plattformar med Xamarin och en Azure-funktion med en Twilio-bindning.</span><span class="sxs-lookup"><span data-stu-id="1f10a-101">You've successfully created a cross-platform mobile app using Xamarin and an Azure function with a Twilio binding.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="1f10a-102">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="1f10a-102">Clean up resources</span></span>

<span data-ttu-id="1f10a-103">När du har arbetat klart med programmet Azure Functions kan du ta bort alla resurser som skapades under självstudien från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1f10a-103">When you're done working with this Azure Functions application, you can delete all resources created during the tutorial from the Azure portal.</span></span>

1. <span data-ttu-id="1f10a-104">Från Visual Studio väljer du *Visa->Cloud Explorer*.</span><span class="sxs-lookup"><span data-stu-id="1f10a-104">From Visual Studio, select *View->Cloud Explorer*.</span></span>

2. <span data-ttu-id="1f10a-105">I listrutan överst i den här panelen väljer du *Resursgrupper*.</span><span class="sxs-lookup"><span data-stu-id="1f10a-105">From the drop-down at the top of this panel, select *Resource Groups*.</span></span>

3. <span data-ttu-id="1f10a-106">Utöka prenumerationen som du använde när du skapade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1f10a-106">Expand the subscription that you used to create the resource group.</span></span> <span data-ttu-id="1f10a-107">Högerklicka på resursgruppen ”ImHere” och välj *Open in Portal* (Öppna i portal).</span><span class="sxs-lookup"><span data-stu-id="1f10a-107">Right-click on the "ImHere" resource group and select *Open in Portal*.</span></span>

    ![Öppna resursgruppen i portalen från Cloud Explorer-fönstret](../media-drafts/9-open-resource-group-in-portal.png)

4. <span data-ttu-id="1f10a-109">Logga in på Azure-portalen i webbläsaren om det behövs.</span><span class="sxs-lookup"><span data-stu-id="1f10a-109">Log into the Azure portal in your browser, if necessary.</span></span>

5. <span data-ttu-id="1f10a-110">Portalen öppnas i resursgruppen ”ImHere”.</span><span class="sxs-lookup"><span data-stu-id="1f10a-110">The portal will open on the "ImHere" resource group.</span></span> <span data-ttu-id="1f10a-111">Klicka på knappen **Ta bort resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="1f10a-111">Click the **Delete Resource Group** button.</span></span>

    ![Ta bort resursgruppen](../media-drafts/9-delete-resource-group.png)

6. <span data-ttu-id="1f10a-113">Ange namnet på resursgruppen och klicka på **Ta bort** för att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="1f10a-113">Enter the name of the resource group to confirm the deletion and click **Delete**.</span></span>

    ![Ange resursgruppens namn för att bekräfta borttagningen](../media-drafts/9-confirm-delete-resource-group.png)

## <a name="summary"></a><span data-ttu-id="1f10a-115">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="1f10a-115">Summary</span></span>

<span data-ttu-id="1f10a-116">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="1f10a-116">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="1f10a-117">Skapa en Xamarin.Forms-app för flera plattformar som använder Xamarin.Essentials.</span><span class="sxs-lookup"><span data-stu-id="1f10a-117">Create a cross-platform Xamarin.Forms app that uses Xamarin.Essentials.</span></span>
> * <span data-ttu-id="1f10a-118">Skapa ett plattformsoberoende gränssnitt med XAML med programlogik i en ViewModel och bind egenskaper i en ViewModel till användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="1f10a-118">Create a cross-platform UI using XAML with application logic in a ViewModel, as well as bind properties in a ViewModel to the UI.</span></span>
> * <span data-ttu-id="1f10a-119">Identifiera användarens plats.</span><span class="sxs-lookup"><span data-stu-id="1f10a-119">Detect the user's location.</span></span>
> * <span data-ttu-id="1f10a-120">Skapa en Azure-funktion med en HTTP-utlösare och kör den lokalt.</span><span class="sxs-lookup"><span data-stu-id="1f10a-120">Create an Azure function with an HTTP trigger and run it locally.</span></span>
> * <span data-ttu-id="1f10a-121">Anropa en Azure-funktion från en mobilapp och skicka data som JSON.</span><span class="sxs-lookup"><span data-stu-id="1f10a-121">Call an Azure function from a mobile app, passing data as JSON.</span></span>
> * <span data-ttu-id="1f10a-122">Bind en Azure-funktion till Twilio för att skicka ett SMS.</span><span class="sxs-lookup"><span data-stu-id="1f10a-122">Bind an Azure function to Twilio to send an SMS message.</span></span>
> * <span data-ttu-id="1f10a-123">Publicera en Azure-funktion till Azure.</span><span class="sxs-lookup"><span data-stu-id="1f10a-123">Publish an Azure function to Azure.</span></span>
