<span data-ttu-id="686b4-101">Vårt mål är att skapa en ny virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="686b4-101">Our goal is to create a new Azure virtual machine.</span></span> <span data-ttu-id="686b4-102">Vi behöver ange flera uppgifter för att identifiera resursplats, operativsystem som ska användas och maskinvarukonfiguration som behövs för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="686b4-102">We'll need to supply several pieces of information to identify the resource location, OS to use, and the hardware configuration needed for the VM.</span></span> <span data-ttu-id="686b4-103">Vi börjar med **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="686b4-103">Let's start with the **resource group**.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="686b4-104">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="686b4-104">Create a resource group</span></span>

<span data-ttu-id="686b4-105">Azure använder _resursgrupper_ till att gruppera relaterade resurser, till exempel virtuella datorer och databaser.</span><span class="sxs-lookup"><span data-stu-id="686b4-105">Azure uses _resource groups_ to group related resources such as virtual machines and databases together.</span></span> <span data-ttu-id="686b4-106">Resursgruppen identifierar även en specifik plats (s.k. ”region”) som avgör vilket datacenter resursen placeras i.</span><span class="sxs-lookup"><span data-stu-id="686b4-106">The resource group also identifies a specific location (called a "region") which will decide what data center the resource is placed into.</span></span>

<span data-ttu-id="686b4-107">Eftersom vi experimenterar börjar vi med att skapa en ny resursgrupp med namnet `ExerciseResources` och placerar den i regionen `eastus`.</span><span class="sxs-lookup"><span data-stu-id="686b4-107">Since we're experimenting, start by creating a new resource group named `ExerciseResources` and place it into the `eastus` region.</span></span>

<!-- TODO: replace with free ed-tier -->

<span data-ttu-id="686b4-108">Skriv följande Azure CLI-kommando i Azure Cloud Shell för att skapa resursgruppen i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="686b4-108">Type the following Azure CLI command in Azure Cloud Shell to create the resource group in your subscription.</span></span>

```azurecli
az group create --name ExerciseResources --location eastus
```

<span data-ttu-id="686b4-109">Detta returnerar ett JSON-block som visar att resursgruppen har skapats.</span><span class="sxs-lookup"><span data-stu-id="686b4-109">This will return a JSON block indicating the resource group has been created.</span></span> <span data-ttu-id="686b4-110">Det bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="686b4-110">It should look something like:</span></span>

```json
{
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources",
  "location": "eastus",
  "managedBy": null,
  "name": "ExerciseResources",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

<span data-ttu-id="686b4-111">Observera att det även returnerar den unika identifieraren för prenumerationen, platsen och namnet som en del av svaret.</span><span class="sxs-lookup"><span data-stu-id="686b4-111">Notice that it returns the subscription unique identifier, location, and name as part of the response.</span></span> <span data-ttu-id="686b4-112">Du kan använda dem för att kontrollera att den skapades i rätt prenumeration och på rätt plats.</span><span class="sxs-lookup"><span data-stu-id="686b4-112">You can use these to verify it got created in the proper subscription and location.</span></span>

<span data-ttu-id="686b4-113">Nu när vi har en resursgrupp ska vi skapa en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="686b4-113">Now that we have a resource group, let's create a new virtual machine.</span></span>