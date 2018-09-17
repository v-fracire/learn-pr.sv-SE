<span data-ttu-id="5e2a6-101">I den här kursdelen genererar du miniatyrer från en källbild med hjälp av tjänsten API för visuellt innehåll som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-101">In this unit, you will generate thumbnails from a source image with the Computer Vision API service that we created previously.</span></span>

# <a name="generate-a-thumbnail-from-an-image-with-computer-vision-api"></a><span data-ttu-id="5e2a6-102">Generera en miniatyr från en bild med hjälp av API för visuellt innehåll</span><span class="sxs-lookup"><span data-stu-id="5e2a6-102">Generate a thumbnail from an image with Computer Vision API</span></span>

<span data-ttu-id="5e2a6-103">Hämta en nyckel som används för att autentisera mot API genom att köra kommandot `az cognitiveservices account keys list`.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="5e2a6-104">Lagra utdata från kommandot inom variabeln `key`.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="5e2a6-105">Kör kommandot `curl`, så görs en HTTP-begäran mot API för visuellt innehåll. Återanvänd den nyss deklarerade variabeln `key`.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reuse the previously declared variable `key`.</span></span>

<span data-ttu-id="5e2a6-106">Genom att ge olika parametrar till API:t kan du generera den miniatyr du behöver.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-106">Different parameters can be provided to the API to generate the proper thumbnail for your needs.</span></span> <span data-ttu-id="5e2a6-107">`width` och `height` krävs och anger för API:t vilken storlek du behöver för en viss bild.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-107">`width` and `height` are required and will tell the API which size you need for a specific image.</span></span> <span data-ttu-id="5e2a6-108">Avslutningsvis genererar parametern `smartCropping` en smartare beskärning genom att analysera det intressantaste området i bilden och behålla det i miniatyren.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-108">Finally, the `smartCropping` parameter generates smarter cropping by analyzing the region of interest in your image to keep it within the thumbnail.</span></span> <span data-ttu-id="5e2a6-109">När smart beskärning är aktiverat behålls till exempel ansiktet i en beskuren profilbild inom bildramen fastän bilden inte har samma proportioner som den vi efterfrågade.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-109">As an example, with smart cropping enabled, a cropped profile picture would keep someone's face within the picture frame even when the picture isn't in the same ratio as the one that we asked.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" -o clouddrive/thumbnail.jpg
```

# <a name="downloading-the-thumbnail"></a><span data-ttu-id="5e2a6-110">Ladda ned miniatyren</span><span class="sxs-lookup"><span data-stu-id="5e2a6-110">Downloading the thumbnail</span></span>

<span data-ttu-id="5e2a6-111">Du hittar den genererade miniatyren i ditt Azure Cloud Shell-lagringskonto i en resursgrupp med namnet `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-111">The generated thumbnail will be found in your Azure Cloud Shell storage account within a resource group named `cloud-shell-storage-<region>`.</span></span>

1. <span data-ttu-id="5e2a6-112">Öppna det automatiskt genererade lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-112">Get into the automatically generated storage account.</span></span>

![image](../images/storage-account.png)

2. <span data-ttu-id="5e2a6-114">Klicka på avsnittet för filer.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-114">Click on the files section.</span></span>

![image](../images/storage-account-click-on-files.png)

3. <span data-ttu-id="5e2a6-116">Du hittar miniatyren i roten för containern.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-116">You will find the thumbnail at the root of the container.</span></span>

![image](../images/storage-account-thumbnail.png)

4. <span data-ttu-id="5e2a6-118">Klicka på filen och ladda sedan ned den.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-118">Click on the file, and then download it.</span></span>

<span data-ttu-id="5e2a6-119">I mappen med nedladdade filer kan du öppna bilden med `100x100` bildpunkter med valfritt bildvisningsprogram.</span><span class="sxs-lookup"><span data-stu-id="5e2a6-119">From within your download folder, you can open the `100x100`-pixels image with any image viewer.</span></span>