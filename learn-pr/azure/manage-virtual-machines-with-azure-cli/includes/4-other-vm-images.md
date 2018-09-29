<span data-ttu-id="ca282-101">Vi använde `Debian` för avbildningen för att skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ca282-101">We used `Debian` for the image to create the virtual machine.</span></span> <span data-ttu-id="ca282-102">Azure har flera VM-standardavbildningar som du kan använda för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ca282-102">Azure has several standard VM images you can use to create a virtual machine.</span></span> 

## <a name="listing-images"></a><span data-ttu-id="ca282-103">Visa bilder i en lista</span><span class="sxs-lookup"><span data-stu-id="ca282-103">Listing images</span></span>

<span data-ttu-id="ca282-104">Du kan hämta en lista över tillgängliga VM-avbildningar med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="ca282-104">You can get a list of the available VM images using the following command.</span></span> 

```azurecli
az vm image list --output table
```

<span data-ttu-id="ca282-105">Det genererar de populäraste avbildningarna som är en del av en offlinelista som är inbyggd i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ca282-105">This will output the most popular images that are part of an offline list built into the Azure CLI.</span></span> <span data-ttu-id="ca282-106">Det finns dock _hundratals_ tillgängliga alternativ för avbildningar på Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ca282-106">However, there are _hundreds_ of image options available in the Azure Marketplace.</span></span> 

## <a name="getting-all-images"></a><span data-ttu-id="ca282-107">Hämta alla avbildningar</span><span class="sxs-lookup"><span data-stu-id="ca282-107">Getting all images</span></span>

<span data-ttu-id="ca282-108">Du kan hämta en fullständig lista genom att lägga till `--all`-flaggan i kommandot.</span><span class="sxs-lookup"><span data-stu-id="ca282-108">You can get a full list by adding the `--all` flag to the command.</span></span> <span data-ttu-id="ca282-109">Eftersom listan med avbildningar på Marketplace är mycket stor är det praktiskt att filtrera listan med alternativen `--publisher`, `--sku` eller `–-offer`.</span><span class="sxs-lookup"><span data-stu-id="ca282-109">Since the list of images in the Marketplace is very large, it is helpful to filter the list with the `--publisher`, `--sku` or `–-offer` options.</span></span>

<span data-ttu-id="ca282-110">Prova till exempel följande kommando för att se _alla_ tillgängliga Wordpress-avbildningar:</span><span class="sxs-lookup"><span data-stu-id="ca282-110">For example, try the following command to see _all_ Wordpress images available:</span></span>

```azurecli
az vm image list --sku Wordpress --output table --all
```

<span data-ttu-id="ca282-111">Eller det här kommandot för att se alla avbildningar som tillhandahålls av Microsoft:</span><span class="sxs-lookup"><span data-stu-id="ca282-111">Or this command to see all images provided by Microsoft:</span></span>

```azurecli
az vm image list --publisher Microsoft --output table --all
```

## <a name="location-specific-images"></a><span data-ttu-id="ca282-112">Platsspecifika avbildningar</span><span class="sxs-lookup"><span data-stu-id="ca282-112">Location-specific images</span></span>

<span data-ttu-id="ca282-113">Vissa avbildningar är bara tillgängliga på vissa platser.</span><span class="sxs-lookup"><span data-stu-id="ca282-113">Some images are only available in certain locations.</span></span> <span data-ttu-id="ca282-114">Försök att lägga till flaggan `--location [location]` till kommandot för att begränsa resultatet till dem som är tillgängliga i regionen där du vill skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ca282-114">Try adding the `--location [location]` flag to the command to scope the results to ones available in the region where you want to create the virtual machine.</span></span> <span data-ttu-id="ca282-115">Skriv exempelvis följande i Azure Cloud Shell för att få en lista över de avbildningar som är tillgängliga i regionen `eastus`.</span><span class="sxs-lookup"><span data-stu-id="ca282-115">For example, type the following into Azure Cloud Shell to get a list of images available in the `eastus` region.</span></span>

```azurecli
az vm image list --location eastus --output table
```

<span data-ttu-id="ca282-116">Prova att titta på några av avbildningarna på de andra tillgängliga Azure Sandbox-platserna:</span><span class="sxs-lookup"><span data-stu-id="ca282-116">Try checking some of the images in the other Azure sandbox available locations:</span></span>

[!include[](../../../includes/azure-sandbox-regions-note.md)]

> [!TIP]
> <span data-ttu-id="ca282-117">Det här är standardavbildningarna som Azure tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="ca282-117">These are the standard images that are provided by Azure.</span></span> <span data-ttu-id="ca282-118">Tänk på att du även kan [skapa och ladda upp dina egna anpassade avbildningar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) för att skapa virtuella datorer baserat på unika konfigurationer eller mindre vanliga versioner eller distributioner av ett operativsystem.</span><span class="sxs-lookup"><span data-stu-id="ca282-118">Keep in mind that you can also [create and upload your own custom images](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) to create VMs based on unique configurations or less common versions or distributions of an operating system.</span></span>

<span data-ttu-id="ca282-119">Kommandofönstret är förmodligen fullt nu, så du kan skriva `clear` för att rensa skärmen om du vill.</span><span class="sxs-lookup"><span data-stu-id="ca282-119">Your command window is probably full now, you can type `clear` to clear the screen if you like.</span></span>