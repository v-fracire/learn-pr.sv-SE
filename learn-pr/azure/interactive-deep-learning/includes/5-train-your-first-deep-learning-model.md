## <a name="train-your-first-deep-learning-model-using-pytorch-and-jupyter"></a><span data-ttu-id="a74c8-101">Träna din första modell för djupinlärning med hjälp av PyTorch och Jupyter</span><span class="sxs-lookup"><span data-stu-id="a74c8-101">Train your first deep learning model using PyTorch and Jupyter</span></span>

![PyTorch-logotyp](../media/5-image1.PNG) 

<span data-ttu-id="a74c8-103">Vanligtvis brukar inte djupinlärningsteknikerna hårdkoda matrisalgebraoperationer helt och hållet för hand.</span><span class="sxs-lookup"><span data-stu-id="a74c8-103">Typically deep learning engineers do not hard code matrix algebra operations all by hand.</span></span> <span data-ttu-id="a74c8-104">I stället använder de ramverk som PyTorch eller TensorFlow.</span><span class="sxs-lookup"><span data-stu-id="a74c8-104">They instead use frameworks such as PyTorch or TensorFlow.</span></span>  

<span data-ttu-id="a74c8-105">PyTorch är ett Python-baserat ramverk som ger flexibilitet som utvecklingsplattform för djupinlärning.</span><span class="sxs-lookup"><span data-stu-id="a74c8-105">PyTorch is a python-based framework that provides flexibility as a deep learning development platform.</span></span> <span data-ttu-id="a74c8-106">PyTorchs arbetsflöde bygger på Pythons NumPy-bibliotek för vetenskaplig databehandling.</span><span class="sxs-lookup"><span data-stu-id="a74c8-106">PyTorch's workflow is built on top of python scientific computing library numpy.</span></span> 

<span data-ttu-id="a74c8-107">Nu kanske du undrar varför vi ska använda PyTorch för att skapa modeller för djupinlärning?</span><span class="sxs-lookup"><span data-stu-id="a74c8-107">Now you might ask, why would we use PyTorch to build deep learning models?</span></span>  

- <span data-ttu-id="a74c8-108">Lättanvänd API – det är så enkelt Python kan vara.</span><span class="sxs-lookup"><span data-stu-id="a74c8-108">Easy to use API – It's as simple as python can be.</span></span>
- <span data-ttu-id="a74c8-109">Stöd för Python – PyTorch integreras smidigt med stacken för vetenskaplig databehandling.</span><span class="sxs-lookup"><span data-stu-id="a74c8-109">Python support – PyTorch smoothly integrates with the scientific computing stack.</span></span>
- <span data-ttu-id="a74c8-110">Dynamiska beräkningsdiagram – i stället för fördefinierade diagram med specifika funktioner bygger PyTorch databaserade diagram dynamiskt, vilka kan ändras under körning.</span><span class="sxs-lookup"><span data-stu-id="a74c8-110">Dynamic computation graphs – Instead of predefined graphs with specific functionalities, PyTorch build computational graphs dynamically that can be modified during runtime.</span></span> <span data-ttu-id="a74c8-111">Dynamiska beräkningsdiagram är värdefulla för kapslad batchbearbetning och när vi inte vet hur mycket minne som krävs för att skapa ett visst nätverk.</span><span class="sxs-lookup"><span data-stu-id="a74c8-111">Dynamic computation graphs are valuable for nested batching and when we do not know how much memory will be needed for creating a given network.</span></span>

## <a name="run-your-first-pytorch-model"></a><span data-ttu-id="a74c8-112">Köra din första PyTorch-modell</span><span class="sxs-lookup"><span data-stu-id="a74c8-112">Run your first PyTorch model</span></span>

<span data-ttu-id="a74c8-113">Gå till den Jupyter Notebook som du konfigurerade i det senaste kapitlet.</span><span class="sxs-lookup"><span data-stu-id="a74c8-113">Navigate to the Jupyter Notebook that you set up in the last chapter.</span></span>

- <span data-ttu-id="a74c8-114">[[VÄRDNAMN FÖR DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span><span class="sxs-lookup"><span data-stu-id="a74c8-114">[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span></span>

<span data-ttu-id="a74c8-115">Välja anteckningsboken first_pytorch_classifier.ipynb</span><span class="sxs-lookup"><span data-stu-id="a74c8-115">Select the first_pytorch_classifier.ipynb notebook</span></span>

![välja first_pytorch_classifier.ipynb](../media/5-image2.PNG)

<span data-ttu-id="a74c8-117">Följ anvisningarna i anteckningsboken för att träna din första PyTorch-klassificerare.</span><span class="sxs-lookup"><span data-stu-id="a74c8-117">Follow the instructions in the notebook to train your first PyTorch classifer.</span></span>

![skärmbild av anteckningsbok](../media/5-image3.PNG)
