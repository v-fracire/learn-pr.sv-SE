## <a name="intro-to-deep-learning"></a><span data-ttu-id="785a7-101">Introduktion till djupinlärning</span><span class="sxs-lookup"><span data-stu-id="785a7-101">Intro to deep learning</span></span>

<span data-ttu-id="785a7-102">Målet med Machine Learning (ML) är att hitta funktioner för att ta fram en modell som omvandlar indata (t.ex. bilder, tidsserier eller ljud) till vissa utdata (t.ex. bildtexter, prisvärden, transkriptioner).</span><span class="sxs-lookup"><span data-stu-id="785a7-102">The goal of Machine Learning ML is to find features to train a model that transforms input data (such as pictures, time series, or audio) to a given output (for example captions, price values, transcriptions).</span></span> <span data-ttu-id="785a7-103">I traditionella datavetenskap handplockas funktionerna ofta.</span><span class="sxs-lookup"><span data-stu-id="785a7-103">In traditional data science, features are often hand-picked.</span></span>

![Ett kanoniskt exempel på ett framåtmatande djupt neuralt nätverk.](../media/2-image1.PNG)

<span data-ttu-id="785a7-105">I Deep Learning (DL) är funktionsextraheringen en process som lärs in genom att indata representeras som vektorer och transformeras med ett antal smarta linjära algebraåtgärder till givna utdata.</span><span class="sxs-lookup"><span data-stu-id="785a7-105">In Deep Learning (DL), the process of feature extraction is learned through representing inputs as vectors and transforming them, with a series of clever linear algebra operations, into a given output.</span></span>  

<span data-ttu-id="785a7-106">Modellutdata jämförs sedan med förväntade utdata med hjälp av en förlustfunktionsekvation.</span><span class="sxs-lookup"><span data-stu-id="785a7-106">The model output is then compared against the expected output using an equation called a loss function.</span></span> <span data-ttu-id="785a7-107">Värdet som returneras av förlustfunktionen för respektive utbildnings indata används för att hjälpa modellen att extrahera funktioner som resulterar i ett lägre förlustvärde nästa pass.</span><span class="sxs-lookup"><span data-stu-id="785a7-107">The value returned by the loss function of each training input is used to guide the model to extract features that will result in a lower loss value on the next pass.</span></span>  
 
<span data-ttu-id="785a7-108">Den serie av matrisåtgärder som vi beräknar som en del av den linjära algebrakomponenten tenderar att vara beräkningsmässigt expansiv och är ofta tungt parallelliserbar, vilket kräver specialiserad databearbetning, t.ex. grafikprocessorer, för att kunna beräkna effektivt.</span><span class="sxs-lookup"><span data-stu-id="785a7-108">The series of matrix operations that we computer as part of the linear algebra component tend to be computationally expensive and are often heavily parallelizable, requiring specialized compute such as Graphics Processing Units GPUs to compute efficiently.</span></span>

## <a name="data-science-virtual-machine"></a><span data-ttu-id="785a7-109">Data Science Virtual Machine</span><span class="sxs-lookup"><span data-stu-id="785a7-109">Data Science Virtual Machine</span></span>

![DSVM-alternativ](../media/2-image2.PNG)

<span data-ttu-id="785a7-111">DSVM:er är Azure Virtual Machine-avbilder som har förinstallerats, konfigurerats och testats med flera populära verktyg som vanligen används för dataanalys, maskininlärning och utbildning inom djupinlärning.</span><span class="sxs-lookup"><span data-stu-id="785a7-111">DSVMs are Azure Virtual Machine images, pre-installed, configured, and tested with several popular tools that are commonly used for data analytics, machine learning, and deep learning training.</span></span>

<span data-ttu-id="785a7-112">De tillhandahåller:</span><span class="sxs-lookup"><span data-stu-id="785a7-112">They provide:</span></span>

- <span data-ttu-id="785a7-113">Enhetlig installation mellan team, vilket underlättar delning och samarbete, skalning och hantering i Azure, minimal konfigurering, helt molnbaserat skrivbord för datavetenskap.</span><span class="sxs-lookup"><span data-stu-id="785a7-113">Consistent setup across team, promote sharing and collaboration, Azure scale and management, Near-Zero Setup, full cloud-based desktop for data science.</span></span>
- <span data-ttu-id="785a7-114">Elastisk kapacitet på begäran med möjlighet att köra analyser på alla Azure-maskinvarukonfigurationer med vertikal och horisontell skalning.</span><span class="sxs-lookup"><span data-stu-id="785a7-114">On-demand elastic capacity Ability to run analytics on all Azure hardware configurations with vertical and horizontal scaling.</span></span> <span data-ttu-id="785a7-115">Betala endast för det du använder, och när du använder det.</span><span class="sxs-lookup"><span data-stu-id="785a7-115">Pay only for what you use, when you use it.</span></span>
- <span data-ttu-id="785a7-116">Djupinlärning med GPU:er, lättillgängliga GPU-kluster med förkonfigurerade djupinlärningsverktyg.</span><span class="sxs-lookup"><span data-stu-id="785a7-116">Deep Learning with GPUs Readily available GPU clusters with Deep Learning tools already pre-configured.</span></span> 

<span data-ttu-id="785a7-117">DSVM innehåller flera AI-verktyg, t.ex. populära GPU-versioner av djupinlärningsramverk och verktyg som Microsoft R Server Developer Edition, Anaconda Python, Jupyter-anteckningsböcker för Python och R, IDE:er för Python och R, SQL-databaser och många andra verktyg för datavetenskap och ML.</span><span class="sxs-lookup"><span data-stu-id="785a7-117">The DSVM contains several tools for AI including popular GPU editions of deep learning frameworks and tools such as Microsoft R Server Developer Edition, Anaconda Python, Jupyter notebooks for Python and R, IDEs for Python and R, SQL database and many other data science and ML tools.</span></span>

<span data-ttu-id="785a7-118">DSVM kan köras på VM-instanser i Azure GPU NC-serien.</span><span class="sxs-lookup"><span data-stu-id="785a7-118">The DSVM can run on Azure GPU NC-series VM instances.</span></span> <span data-ttu-id="785a7-119">Dessa GPU:er använder diskret enhetstilldelning, vilket resulterar i Bare Metal-liknande prestanda och som väl lämpar sig för djupinlärningsproblem.</span><span class="sxs-lookup"><span data-stu-id="785a7-119">These GPUs use discrete device assignment, resulting in performance close to bare-metal, and are well-suited to deep learning problems.</span></span>

<!--### Quiz? 

What is the goal of machine learning? 
How is traditional machine learning different from deep learning? 
Why are GPU's often used for deep learning? 
What does the DSVM provide? -->
