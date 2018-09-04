<span data-ttu-id="35400-101">Du har skapat och distribuerat ett komplett webbprogram med Azure App Services.</span><span class="sxs-lookup"><span data-stu-id="35400-101">You've successfully created and deployed a full-featured Web application using Azure App Services.</span></span>

<span data-ttu-id="35400-102">Azure App Services gör det enklare att hantera och kontrollera webbappar jämfört med traditionella värdalternativ.</span><span class="sxs-lookup"><span data-stu-id="35400-102">Azure App Services simplifies managing and controlling your Web app in comparison to traditional hosting options.</span></span> <span data-ttu-id="35400-103">Med Azure Web Apps krävs mindre tid och arbete för att köra och hantera en webbapp, samtidigt som du har tillgång till avancerade funktioner som automatisk skalning och git-integrering.</span><span class="sxs-lookup"><span data-stu-id="35400-103">Azure Web Apps can help you reduce the time and effort spent running and managing your Web app, and provide advanced cloud features such as auto scaling and git integration.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="35400-104">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="35400-104">Clean up resources</span></span>

<span data-ttu-id="35400-105">När du är klar med det här programmet kan du följa stegen nedan för att ta bort alla resurser som skapats i självstudien.</span><span class="sxs-lookup"><span data-stu-id="35400-105">When you are done working with this application, you can follow the steps below to delete all resources created during the tutorial.</span></span>

<span data-ttu-id="35400-106">Som du vet innehåller en resursgrupp alla andra resurser som skapas och kopplas till webbappen.</span><span class="sxs-lookup"><span data-stu-id="35400-106">As you know, a Resource Group embraces all the other resources that are created and related to the Web App.</span></span> <span data-ttu-id="35400-107">När du rensar upp i Azure behöver du alltså bara ta bort resursgruppen så tas alla resurser som skapats i den bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="35400-107">So, cleaning up after yourself on Azure, is a matter of deleting the resource group and hence, all the resources created under that group disappear.</span></span>

<span data-ttu-id="35400-108">Nu ska vi se hur du kan ta bort en resursgrupp med hjälp av Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="35400-108">Let's explore together how you can delete a resource group using the Azure Portal:</span></span>

1. <span data-ttu-id="35400-109">Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true)</span><span class="sxs-lookup"><span data-stu-id="35400-109">Login to the [Azure Portal](https://portal.azure.com?azure-portal=true)</span></span>

1. <span data-ttu-id="35400-110">Leta upp och klicka på menyalternativet **Resursgrupper** till vänster på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="35400-110">Locate and click the **Resource groups** menu item on the left-side of the Dashboard.</span></span> <span data-ttu-id="35400-111">Bladet **Resursgrupper** visar en lista över alla resursgrupper som du har skapat på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="35400-111">The **Resource groups** blade lists all the resource groups that you have created on the Azure Portal.</span></span>

1. <span data-ttu-id="35400-112">Leta upp och klicka på namnet på den resursgrupp som du skapade i del 2.</span><span class="sxs-lookup"><span data-stu-id="35400-112">Locate and click the resource group name that you have created in Unit #2.</span></span> <span data-ttu-id="35400-113">Du kommer till bladet Webbapp.</span><span class="sxs-lookup"><span data-stu-id="35400-113">The portal navigates you to the Web App blade.</span></span>

    ![Resursgrupper](../media-draft/8-resource-groups.png)

1. <span data-ttu-id="35400-115">Bladet **Resursgrupp** öppnas.</span><span class="sxs-lookup"><span data-stu-id="35400-115">The Azure Portal navigates you to the **Resource Group** blade.</span></span> <span data-ttu-id="35400-116">Här kan du se en lista över alla resurser som du har skapat i den här modulen under den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="35400-116">There, you can see a list of all the resources that you've created during this module under this resource group.</span></span>

    ![Bladet Resursgrupp](../media-draft/8-resource-group-blade.png)

1. <span data-ttu-id="35400-118">Leta upp och klicka på länken **Ta bort resursgrupp** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="35400-118">Locate and click on the **Delete resource group** link at the top of the blade.</span></span>

1. <span data-ttu-id="35400-119">Azure verifierar att du verkligen vill ta bort resursgruppen genom att uppmana dig att ange namnet på den.</span><span class="sxs-lookup"><span data-stu-id="35400-119">Azure verifies that you really want to delete this resource group by asking you to type the name of it.</span></span> <span data-ttu-id="35400-120">Om du vill fortsätta skriver du namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="35400-120">To proceed, type the name of the resource group.</span></span>

    ![Bekräfta borttagning av resursgrupp](../media-draft/8-resource-group-delete.png)

1. <span data-ttu-id="35400-122">Leta upp och klicka på knappen **Ta bort** längst ned i bekräftelsefönstret.</span><span class="sxs-lookup"><span data-stu-id="35400-122">Locate and click on the **Delete** button at the bottom of the confirmation window.</span></span>

1. <span data-ttu-id="35400-123">Det tar några sekunder att rensa resursgruppen och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="35400-123">Azure takes a few seconds in order to wipe out the resource group and all the related resources.</span></span>
