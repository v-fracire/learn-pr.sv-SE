<span data-ttu-id="1ca11-101">I den här enheten skapar du en tjänst för API för visuellt innehåll med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="1ca11-101">In this unit, you will create a Computer Vision API service using the Azure CLI.</span></span>

# <a name="exercise-create-a-computer-vision-service"></a><span data-ttu-id="1ca11-102">Övning: Skapa en tjänst för visuellt innehåll</span><span class="sxs-lookup"><span data-stu-id="1ca11-102">Exercise: Create a Computer Vision service</span></span>

<span data-ttu-id="1ca11-103">[Visuellt innehåll](/azure/cognitive-services/computer-vision/home) ingår i [Microsoft Cognitive Services](/azure/cognitive-services/welcome), som är en fullständig uppsättning maskininlärningsalgoritmer som är tillgänglig som en tjänst för allmän användning.</span><span class="sxs-lookup"><span data-stu-id="1ca11-103">[Computer Vision](/azure/cognitive-services/computer-vision/home) is part of [Azure Cognitive Services](/azure/cognitive-services/welcome), which is a complete set of machine learning algorithms available as a service for anyone to use.</span></span> <span data-ttu-id="1ca11-104">I stället för att generera tillräckligt med data för att träna en modell från grunden tillhandahåller vi förtränade modeller för allmän användning.</span><span class="sxs-lookup"><span data-stu-id="1ca11-104">Instead of generating enough data to train a model from scratch, we make available pre-trained models for anyone to use.</span></span>

<span data-ttu-id="1ca11-105">Bland de många tjänsterna vi gör tillgängliga finns tjänster som kan hantera röst, bild, video, sökning, språk och mer.</span><span class="sxs-lookup"><span data-stu-id="1ca11-105">Among the many services available, we provide services that can handle voice, image, video, search, language, and more.</span></span>

<span data-ttu-id="1ca11-106">I den här övningen ska vi fokusera på att skapa tjänsten som gör det möjligt för oss att hantera bilder.</span><span class="sxs-lookup"><span data-stu-id="1ca11-106">In this exercise, we're going to focus on creating the necessary service that will allow us to handle images.</span></span> <span data-ttu-id="1ca11-107">Efter den här övningen kommer du att kunna skapa en API-slutpunkt som kan identifiera bilder.</span><span class="sxs-lookup"><span data-stu-id="1ca11-107">After this exercise, you will be able to create an API endpoint that will be able to identify images.</span></span>

# <a name="create-a-computer-vision-api-service"></a><span data-ttu-id="1ca11-108">Skapa en tjänst för API för visuellt innehåll</span><span class="sxs-lookup"><span data-stu-id="1ca11-108">Create a Computer Vision API service</span></span>

<span data-ttu-id="1ca11-109">Innan du skapar tjänsten för API för visuellt innehåll måste du ha en *resursgrupp* att distribuera den till.</span><span class="sxs-lookup"><span data-stu-id="1ca11-109">Before you create your Computer Vision API service, you need a *resource group* to deploy it to.</span></span> <span data-ttu-id="1ca11-110">En resursgrupp är en logisk samling dit alla Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="1ca11-110">A resource group is a logical collection into which all Azure resources are deployed and managed.</span></span>

```azurecli
az group create --name ComputerVisionRG --location westus2
```

<span data-ttu-id="1ca11-111">När du har skapat resursgruppen skapar du en tjänst med kommandot `az cognitiveservices account create` och namnet på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="1ca11-111">Once you've created the resource group, create the service with the `az cognitiveservices account create` command and the name of your service.</span></span> 

```azurecli
az cognitiveservices account create --kind ComputerVision -l westus2 -n ComputerVisionService --sku F0 -g ComputerVisionRG
```
