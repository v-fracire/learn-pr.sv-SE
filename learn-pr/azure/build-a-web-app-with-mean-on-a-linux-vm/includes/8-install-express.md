<span data-ttu-id="14a9a-101">I MEAN-stacken representerar **E** Express-komponenten.</span><span class="sxs-lookup"><span data-stu-id="14a9a-101">In the MEAN stack, the **E** represents the Express component.</span></span> <span data-ttu-id="14a9a-102">Express är ett webbserverramverk som skapats för Node.js.</span><span class="sxs-lookup"><span data-stu-id="14a9a-102">Express is a web server framework that's built for Node.js.</span></span> <span data-ttu-id="14a9a-103">Det hanterar vår begäranderoutning och vårt hjälpkomponentsystem.</span><span class="sxs-lookup"><span data-stu-id="14a9a-103">It will handle our request routing and helper system.</span></span> <span data-ttu-id="14a9a-104">Express gör det enklare att skapa webbprogram i Node.js-körningen.</span><span class="sxs-lookup"><span data-stu-id="14a9a-104">It's designed to simplify building web applications that are hosted on the Node.js runtime.</span></span> <span data-ttu-id="14a9a-105">Kärnan i Express är routning, men det hjälper även webbprogram att fungera med HTTP-cookies och begära frågesträngsdata.</span><span class="sxs-lookup"><span data-stu-id="14a9a-105">While the core of Express is routing, it also helps web applications work with HTTP cookies and request query string data.</span></span>

## <a name="express-version"></a><span data-ttu-id="14a9a-106">Express-version</span><span class="sxs-lookup"><span data-stu-id="14a9a-106">Express version</span></span>

<span data-ttu-id="14a9a-107">Eftersom Express är ett Node.js-programramverk måste vi ha Node.js och nodpakethanteraren installerade redan.</span><span class="sxs-lookup"><span data-stu-id="14a9a-107">Because Express is a Node.js web application framework, we need to have Node.js and the node package manager installed.</span></span>

<span data-ttu-id="14a9a-108">Vid tidpunkten då den här artikeln skrivs är Express-versionen `4.16.0`.</span><span class="sxs-lookup"><span data-stu-id="14a9a-108">At the time of this writing, the Express version is `4.16.0`.</span></span>

## <a name="how-to-install-express"></a><span data-ttu-id="14a9a-109">Så installerar du Express</span><span class="sxs-lookup"><span data-stu-id="14a9a-109">How to install Express</span></span>

<span data-ttu-id="14a9a-110">Installationen är så enkel som att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="14a9a-110">The installation is as simple as running the following command:</span></span>

   ```bash
   npm install express
   ```

<span data-ttu-id="14a9a-111">Du behöver inte göra detta på den virtuella datorn ännu, vi kommer till det snart.</span><span class="sxs-lookup"><span data-stu-id="14a9a-111">You don't need to do this on your VM yet, we'll get to it shortly.</span></span>

## <a name="summary"></a><span data-ttu-id="14a9a-112">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="14a9a-112">Summary</span></span>

<span data-ttu-id="14a9a-113">Med Express installerat har vi en genväg till att svara på HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="14a9a-113">With Express installed, we have a shortcut path to responding to HTTP requests.</span></span> <span data-ttu-id="14a9a-114">Vi kan snabbt konfigurera begäranderoutning och hanterare för hantering av API och webbplatssvarsdata.</span><span class="sxs-lookup"><span data-stu-id="14a9a-114">We can quickly set up request routing and handlers for handling API and web page response data.</span></span>