### <a name="exercise-6-delete-the-data-science-vm"></a><span data-ttu-id="45c5c-101">Övning 6: Ta bort en Data Science VM</span><span class="sxs-lookup"><span data-stu-id="45c5c-101">Exercise 6: Delete the Data Science VM</span></span>

<span data-ttu-id="45c5c-102">I den här övningen ska du ta bort resursgruppen som skapades i övning 1 när du skapade en Data Science VM.</span><span class="sxs-lookup"><span data-stu-id="45c5c-102">In this exercise, you will delete the resource group created in Exercise 1 when you created the Data Science VM.</span></span> <span data-ttu-id="45c5c-103">Om du tar bort resursgruppen raderas även allt innehåll i den. Därmed kan inga fler kostnader uppkomma.</span><span class="sxs-lookup"><span data-stu-id="45c5c-103">Deleting the resource group deletes everything in it and prevents any further charges from being incurred for it.</span></span> <span data-ttu-id="45c5c-104">Det går inte att återställa borttagna resursgrupper, så se noga till så att du är helt klar med den innan du tar bort den.</span><span class="sxs-lookup"><span data-stu-id="45c5c-104">Resource groups that are deleted can't be recovered, so be certain you're finished using it before deleting it.</span></span> <span data-ttu-id="45c5c-105">**Tänk på att du inte distribuera den här resursgruppen längre än nödvändigt** eftersom en Data Science VM är ganska dyr i drift.</span><span class="sxs-lookup"><span data-stu-id="45c5c-105">However, it is **important not to leave this resource group deployed any longer than necessary** because a Data Science VM is moderately expensive.</span></span>

1. <span data-ttu-id="45c5c-106">Gå tillbaka till bladet för resursgruppen som du skapade i övning 1.</span><span class="sxs-lookup"><span data-stu-id="45c5c-106">Return to the blade for the resource group you created in Exercise 1.</span></span> <span data-ttu-id="45c5c-107">Klicka sedan på **Ta bort resursgrupp** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="45c5c-107">Then click the **Delete resource group** button at the top of the blade.</span></span>

    ![Ta bort resursgruppen](../images/delete-resource-group.png)

    <span data-ttu-id="45c5c-109">_Ta bort resursgruppen_</span><span class="sxs-lookup"><span data-stu-id="45c5c-109">_Deleting the resource group_</span></span>

1. <span data-ttu-id="45c5c-110">För säkerhets skull måste du ange resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="45c5c-110">For safety, you are required to type in the resource group's name.</span></span> <span data-ttu-id="45c5c-111">(När du väl tar tagit bort en resursgrupp går det inte att återställa den.) Skriv namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="45c5c-111">(Once deleted, a resource group cannot be recovered.) Type the name of the resource group.</span></span> <span data-ttu-id="45c5c-112">Klicka på knappen **Ta bort** för att ta bort alla spår av den här labbuppgiften från din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="45c5c-112">Then click the **Delete** button to remove all traces of this lab from your Azure subscription.</span></span>

<span data-ttu-id="45c5c-113">Efter några minuter tas resursgruppen och de resurser som finns i den bort.</span><span class="sxs-lookup"><span data-stu-id="45c5c-113">After a few minutes, the resource group and all of its resources will be deleted.</span></span> <span data-ttu-id="45c5c-114">Debiteringen upphör när du klickar på **Ta bort**. Du kommer alltså inte att debiteras för den tid det tar att radera resurserna.</span><span class="sxs-lookup"><span data-stu-id="45c5c-114">Billing stops when you click **Delete**, so you're not charged for the time required to delete the resources.</span></span> <span data-ttu-id="45c5c-115">På samma sätt inleds inte debiteringen förrän resurserna är fullständigt distribuerade och klara att köras.</span><span class="sxs-lookup"><span data-stu-id="45c5c-115">Similarly, billing doesn't start until the resources are fully and successfully deployed.</span></span>

### <a name="summary"></a><span data-ttu-id="45c5c-116">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="45c5c-116">Summary</span></span>

<span data-ttu-id="45c5c-117">Stegen i den här labbuppgiften är generella och kan även användas i samband med andra typer av bildklassificeringar.</span><span class="sxs-lookup"><span data-stu-id="45c5c-117">The steps in this lab may be generalized to perform other types of image-classification tasks.</span></span> <span data-ttu-id="45c5c-118">Du kan till exempel träna samma TensorFlow-modell till att känna igen kattbilder eller identifiera defekter på produkter på ett löpande band.</span><span class="sxs-lookup"><span data-stu-id="45c5c-118">For example, you could train the same TensorFlow model to recognize cat images or identify defective parts parts produced on an assembly line.</span></span> <span data-ttu-id="45c5c-119">Bildklassificering är ett av de vanligaste användningsområdena för maskininlärning idag och användbarheten kommer troligen att öka med tiden.</span><span class="sxs-lookup"><span data-stu-id="45c5c-119">Image classification is one of the most prevalent uses of machine learning today, and its usefulness will only increase over time.</span></span> <span data-ttu-id="45c5c-120">Nu när du har tillägnat dig grundkunskaperna kan du prova att skapa egna modeller för bildklassificering.</span><span class="sxs-lookup"><span data-stu-id="45c5c-120">Now that you have a basis to work from, try creating some image-classification models of your own.</span></span> <span data-ttu-id="45c5c-121">Kanske är du något nytt på spåren!</span><span class="sxs-lookup"><span data-stu-id="45c5c-121">You never know what might come of it!</span></span>