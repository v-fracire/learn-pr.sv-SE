<span data-ttu-id="d87f0-101">I den här enheten skapar en ASP.NET Core-webbapp.</span><span class="sxs-lookup"><span data-stu-id="d87f0-101">In this unit, you will create an ASP.NET Core web app.</span></span>

## <a name="create-a-new-web-project"></a><span data-ttu-id="d87f0-102">Skapa ett nytt webbprojekt</span><span class="sxs-lookup"><span data-stu-id="d87f0-102">Create a new web project</span></span>

<span data-ttu-id="d87f0-103">Navet bland .NET CLI-verktygen är kommandoradsverktyget `dotnet`.</span><span class="sxs-lookup"><span data-stu-id="d87f0-103">The heart of the .NET CLI tools is the `dotnet` command line tool.</span></span> <span data-ttu-id="d87f0-104">Med det här kommandot skapar du ett nytt ASP.NET Core-webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="d87f0-104">Using this command, you will create a new ASP.NET Core web project.</span></span>

<span data-ttu-id="d87f0-105">Till höger i Cloud Shell skapar du ett nytt ASP.NET Core MVC-program.</span><span class="sxs-lookup"><span data-stu-id="d87f0-105">In the Cloud Shell on the right, create a new ASP.NET Core MVC application.</span></span> <span data-ttu-id="d87f0-106">Ge det namnet ”BestBikeApp”.</span><span class="sxs-lookup"><span data-stu-id="d87f0-106">Name it "BestBikeApp".</span></span>

```bash
dotnet new mvc --name BestBikeApp
```

<span data-ttu-id="d87f0-107">Kommandot skapar en ny mapp som heter ”BestBikeApp”, där projektet ska lagras.</span><span class="sxs-lookup"><span data-stu-id="d87f0-107">The command will create a new folder named "BestBikeApp" to hold your project.</span></span> <span data-ttu-id="d87f0-108">`cd` där, skapa sedan och kör programmet för att verifiera att det är klart.</span><span class="sxs-lookup"><span data-stu-id="d87f0-108">`cd` there, then build and run the application to verify it is complete.</span></span>

```bash
cd BestBikeApp
dotnet run
```

<span data-ttu-id="d87f0-109">Du bör se något som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="d87f0-109">You should get something like:</span></span>

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

<span data-ttu-id="d87f0-110">Utdata beskriver situationen efter att du har startat appen: programmet körs och lyssnar på port 5000.</span><span class="sxs-lookup"><span data-stu-id="d87f0-110">The output describes the situation after starting your app: the application is running and listening at port 5000.</span></span>

<span data-ttu-id="d87f0-111">Om appen kördes på vår egen dator skulle vi ha kunnat öppna en webbläsare till http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="d87f0-111">If were running the app on our own machine we'd be able to open a browser to http://localhost:5000.</span></span> <span data-ttu-id="d87f0-112">Eftersom vi är i Azure Cloud Shell måste vi distribuera appen till en plats med en offentlig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d87f0-112">Since we're on Azure Cloud Shell, we'll need to deploy the app to somewhere with a public endpoint.</span></span> <span data-ttu-id="d87f0-113">Den App Service-instans som vi skapade tidigare är perfekt för det.</span><span class="sxs-lookup"><span data-stu-id="d87f0-113">The App Service instance we created earlier is perfect for that.</span></span>