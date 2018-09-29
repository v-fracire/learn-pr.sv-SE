<span data-ttu-id="1cba5-101">Åtkomst till och bearbetning av data är viktiga uppgifter i många programvarulösningar.</span><span class="sxs-lookup"><span data-stu-id="1cba5-101">Accessing and processing data are key tasks in many software solutions.</span></span> <span data-ttu-id="1cba5-102">Tänk på några av följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="1cba5-102">Consider some of these scenarios:</span></span>

* <span data-ttu-id="1cba5-103">Du har blivit ombedd att implementera ett sätt att flytta inkommande data från blogglagring till Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1cba5-103">You've been asked to implement a way to move incoming data from Blob storage to Azure Cosmos DB.</span></span>
* <span data-ttu-id="1cba5-104">Du vill skicka inkommande meddelanden till en kö för bearbetning av en annan komponent i företaget.</span><span class="sxs-lookup"><span data-stu-id="1cba5-104">You want to post incoming messages to a queue for processing by another component in your enterprise.</span></span>
* <span data-ttu-id="1cba5-105">Di tjänst måste hämta spelresultat från en kö och uppdatera en poängtavla online.</span><span class="sxs-lookup"><span data-stu-id="1cba5-105">Your service needs to grab gamer scores from a queue and update an online scoreboard.</span></span>

<span data-ttu-id="1cba5-106">Alla dessa exempel handlar om att flytta data.</span><span class="sxs-lookup"><span data-stu-id="1cba5-106">All of these examples are about moving data.</span></span> <span data-ttu-id="1cba5-107">Datakällan och -målen skiljer sig för olika scenarier men mönstret är liknande.</span><span class="sxs-lookup"><span data-stu-id="1cba5-107">The data source and destinations differ from scenario to scenario, but the pattern is similar.</span></span> <span data-ttu-id="1cba5-108">Du ansluter till en datakälla och läser och skriver data.</span><span class="sxs-lookup"><span data-stu-id="1cba5-108">You connect to a data source, and you read and write data.</span></span> <span data-ttu-id="1cba5-109">Azure Functions hjälper dig att integrera data och tjänster med bindningar.</span><span class="sxs-lookup"><span data-stu-id="1cba5-109">Azure Functions helps you integrate with data and services by using bindings.</span></span> 

## <a name="what-is-a-binding"></a><span data-ttu-id="1cba5-110">Vad är en bindning?</span><span class="sxs-lookup"><span data-stu-id="1cba5-110">What is a binding?</span></span>

<span data-ttu-id="1cba5-111">I Azure Functions utgör bindningar ett deklarativt sätt att ansluta till data i koden.</span><span class="sxs-lookup"><span data-stu-id="1cba5-111">In Azure Functions, bindings provide a declarative way to connect to data from within your code.</span></span> <span data-ttu-id="1cba5-112">De gör det lättare att integrera dataströmmar konsekvent i en funktion.</span><span class="sxs-lookup"><span data-stu-id="1cba5-112">They make it easier to integrate with data streams consistently in a function.</span></span> <span data-ttu-id="1cba5-113">Du kan ha flera bindningar som ger åtkomst till olika dataelement.</span><span class="sxs-lookup"><span data-stu-id="1cba5-113">You can have multiple bindings providing access to different data elements.</span></span> <span data-ttu-id="1cba5-114">Detta är kraftfullt eftersom du kan ansluta till dina datakällor utan att behöva koda en specifik anslutningslogik (som databasanslutningar eller webb-API-gränssnitt).</span><span class="sxs-lookup"><span data-stu-id="1cba5-114">This is powerful because you can connect to your data sources without having to code specific connection logic (like database connections or web API interfaces).</span></span>

### <a name="types-of-bindings"></a><span data-ttu-id="1cba5-115">Typer av bindningar</span><span class="sxs-lookup"><span data-stu-id="1cba5-115">Types of bindings</span></span>

<span data-ttu-id="1cba5-116">Det finns två typer av bindningar du kan använda för funktioner:</span><span class="sxs-lookup"><span data-stu-id="1cba5-116">There are two kinds of bindings you can use with functions:</span></span>

1. <span data-ttu-id="1cba5-117">**Indatabindning** En indatabindning är en anslutning till en **datakälla**.</span><span class="sxs-lookup"><span data-stu-id="1cba5-117">**Input binding** An input binding is a connection to a data **source**.</span></span> <span data-ttu-id="1cba5-118">Vår funktion kan läsa data från dessa indata.</span><span class="sxs-lookup"><span data-stu-id="1cba5-118">Our function can read data from these inputs.</span></span>

1. <span data-ttu-id="1cba5-119">**Utdatabindning** En utdatabindning är en anslutning till ett **datamål**.</span><span class="sxs-lookup"><span data-stu-id="1cba5-119">**Output binding** An output binding is a connection to a data **destination**.</span></span> <span data-ttu-id="1cba5-120">Vår funktion kan skriva data till dessa mål.</span><span class="sxs-lookup"><span data-stu-id="1cba5-120">Our function can write data to these destinations.</span></span>

<span data-ttu-id="1cba5-121">Det finns även utlösare.</span><span class="sxs-lookup"><span data-stu-id="1cba5-121">There are also triggers.</span></span> <span data-ttu-id="1cba5-122">Utlösare är särskilda typer av indatabindningar som kör en funktion.</span><span class="sxs-lookup"><span data-stu-id="1cba5-122">Triggers are special types of input bindings that cause a function to execute.</span></span> <span data-ttu-id="1cba5-123">Till exempel kan ett Azure Event Grid-meddelande konfigureras som en utlösare.</span><span class="sxs-lookup"><span data-stu-id="1cba5-123">For example, an Azure Event Grid notification can be configured as a trigger.</span></span> <span data-ttu-id="1cba5-124">När en händelse inträffar, körs funktionen.</span><span class="sxs-lookup"><span data-stu-id="1cba5-124">When an event occurs, the function will run.</span></span>

### <a name="types-of-supported-bindings"></a><span data-ttu-id="1cba5-125">Typer av bindningar som stöds</span><span class="sxs-lookup"><span data-stu-id="1cba5-125">Types of supported bindings</span></span>

<span data-ttu-id="1cba5-126">*Typen* av bindning definierar var data läses eller vart de skickas.</span><span class="sxs-lookup"><span data-stu-id="1cba5-126">The *type* of binding defines where we are reading or sending data.</span></span> <span data-ttu-id="1cba5-127">Det finns en bindning om att svara på webbförfrågningar och ett stort urval av bindningar om att interagera direkt med olika Azure-tjänster samt tjänster från tredje part.</span><span class="sxs-lookup"><span data-stu-id="1cba5-127">There is a binding to respond to web requests and a large selection of bindings to interact directly with various Azure services as well as third-party services.</span></span>

<span data-ttu-id="1cba5-128">En bindningstyp kan användas som indata, utdata eller båda.</span><span class="sxs-lookup"><span data-stu-id="1cba5-128">A binding type can be used as an input, an output or both.</span></span> <span data-ttu-id="1cba5-129">Till exempel kan en funktion skriva till en Azure Blob Storage-utdatabindning, men en uppdatering i Blob Storage kan utlösa en annan funktion.</span><span class="sxs-lookup"><span data-stu-id="1cba5-129">For example, a function can write to Azure Blob Storage output binding, but a blob storage update could trigger another function.</span></span>

<span data-ttu-id="1cba5-130">Några vanliga bindningstyper visas nedan:</span><span class="sxs-lookup"><span data-stu-id="1cba5-130">Some common binding types are listed below:</span></span>
- <span data-ttu-id="1cba5-131">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="1cba5-131">Blob Storage</span></span>
- <span data-ttu-id="1cba5-132">Azure Service Bus-köer</span><span class="sxs-lookup"><span data-stu-id="1cba5-132">Azure Service Bus Queues</span></span>
- <span data-ttu-id="1cba5-133">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1cba5-133">Azure Cosmos DB</span></span>
- <span data-ttu-id="1cba5-134">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="1cba5-134">Azure Event Hubs</span></span>
- <span data-ttu-id="1cba5-135">Externa filer</span><span class="sxs-lookup"><span data-stu-id="1cba5-135">External Files</span></span>
- <span data-ttu-id="1cba5-136">Externa tabeller</span><span class="sxs-lookup"><span data-stu-id="1cba5-136">External Tables</span></span>
- <span data-ttu-id="1cba5-137">HTTP-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="1cba5-137">HTTP endpoints</span></span>

<span data-ttu-id="1cba5-138">Dessa typer är bara ett urval.</span><span class="sxs-lookup"><span data-stu-id="1cba5-138">These types are just a sample.</span></span> <span data-ttu-id="1cba5-139">Det finns fler och funktioner har en utöknignsmodell för att lägga till fler bindningar.</span><span class="sxs-lookup"><span data-stu-id="1cba5-139">There are more, plus functions have an extensibility model to add more bindings.</span></span>

### <a name="binding-properties"></a><span data-ttu-id="1cba5-140">Bindningsegenskaper</span><span class="sxs-lookup"><span data-stu-id="1cba5-140">Binding properties</span></span>

<span data-ttu-id="1cba5-141">Tre egenskaper krävs i alla bindningar.</span><span class="sxs-lookup"><span data-stu-id="1cba5-141">Three properties are required in all bindings.</span></span> <span data-ttu-id="1cba5-142">Du kanske måste ange ytterligare egenskaper utifrån den typ av bindning och lagring du använder.</span><span class="sxs-lookup"><span data-stu-id="1cba5-142">You may have to supply additional properties based on the type of binding and storage you are using.</span></span>

1. <span data-ttu-id="1cba5-143">**Namn** Definierar funktionsparametern genom vilken du får åtkomst till data.</span><span class="sxs-lookup"><span data-stu-id="1cba5-143">**Name** Defines the function parameter through which you access the data.</span></span> <span data-ttu-id="1cba5-144">I till exempel en köindatabindning är detta namnet på funktionsparametern som tar emot kömeddelandeinnehållet.</span><span class="sxs-lookup"><span data-stu-id="1cba5-144">For example, in a queue input binding, this is the name of the function parameter that receives the queue message content.</span></span> 

1. <span data-ttu-id="1cba5-145">**Typ** Identifierar typen av bindning, alltså den typ av data eller tjänst som vi vill interagera med.</span><span class="sxs-lookup"><span data-stu-id="1cba5-145">**Type** Identifies the type of binding, i.e., the type of data or service we want to interact with.</span></span>

1. <span data-ttu-id="1cba5-146">**Riktning** Identifierar vilken riktning data flödar, alltså om det är en indata- eller utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="1cba5-146">**Direction** Indicates the direction data is flowing, i.e., is it an input or output binding?</span></span>

<span data-ttu-id="1cba5-147">De flesta bindningstyper behöver dessutom en fjärde egenskap:</span><span class="sxs-lookup"><span data-stu-id="1cba5-147">Additionally, most binding types also need a fourth property:</span></span> 

4. <span data-ttu-id="1cba5-148">**Anslutning** Innehåller namnet på en appinställningsnyckel som innehåller anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="1cba5-148">**Connection** Provides the name of an app setting key that contains the connection string.</span></span> <span data-ttu-id="1cba5-149">Bindningar med anslutningssträngar som lagras i appinställningar för att hålla hemligheter utanför funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="1cba5-149">Bindings use connection strings stored in app settings to keep secrets out of the function code.</span></span> <span data-ttu-id="1cba5-150">Detta gör koden mer konfigurerbar och säker.</span><span class="sxs-lookup"><span data-stu-id="1cba5-150">This makes your code more configurable and secure.</span></span>

## <a name="create-a-binding"></a><span data-ttu-id="1cba5-151">Skapa en bindning</span><span class="sxs-lookup"><span data-stu-id="1cba5-151">Create a binding</span></span>

<span data-ttu-id="1cba5-152">Bindningar definieras i JSON.</span><span class="sxs-lookup"><span data-stu-id="1cba5-152">Bindings are defined in JSON.</span></span> <span data-ttu-id="1cba5-153">En bindning konfigureras i funktionens konfigurationsfil, som har namnet *function-json* och finns i samma mapp som funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="1cba5-153">A binding is configured in your function's configuration file, which is named *function.json* and lives in the same folder as your function code.</span></span>

 <span data-ttu-id="1cba5-154">Låt oss nu undersöka ett exempel på en *indatabindning*:</span><span class="sxs-lookup"><span data-stu-id="1cba5-154">Let's examine a sample *input binding*:</span></span>

```json
    ...
    {
      "name": "headshotBlob",
      "type": "blob",
      "path": "thumbnail-images/{filename}",
      "connection": "HeadshotStorageConnection",
      "direction": "in"
    },
    ...
```

<span data-ttu-id="1cba5-155">Vi gör följande för att skapa bindningen:</span><span class="sxs-lookup"><span data-stu-id="1cba5-155">To create this binding, we:</span></span>

1. <span data-ttu-id="1cba5-156">Skapa en bindning i vår *function.json*-fil.</span><span class="sxs-lookup"><span data-stu-id="1cba5-156">Create a binding in our *function.json* file.</span></span>

1. <span data-ttu-id="1cba5-157">Ange värdet för variabeln `name`.</span><span class="sxs-lookup"><span data-stu-id="1cba5-157">Provide the value for the `name` variable.</span></span> <span data-ttu-id="1cba5-158">I det här exemplet ska variabeln innehålla blob-data.</span><span class="sxs-lookup"><span data-stu-id="1cba5-158">In this example, the variable holds the blob data.</span></span>

1. <span data-ttu-id="1cba5-159">Ange `type` för lagringen.</span><span class="sxs-lookup"><span data-stu-id="1cba5-159">Provide the storage `type`.</span></span> <span data-ttu-id="1cba5-160">I föregående exempel används Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="1cba5-160">In the preceding example, we are using Blob storage.</span></span>

1. <span data-ttu-id="1cba5-161">Ange `path`, som anger containern och objektnamnet som placerar där.</span><span class="sxs-lookup"><span data-stu-id="1cba5-161">Provide the `path`, which specifies the container and the item name that goes in it.</span></span> <span data-ttu-id="1cba5-162">Egenskapen `path` krävs för blobar.</span><span class="sxs-lookup"><span data-stu-id="1cba5-162">The `path` property is required for blobs.</span></span>

1. <span data-ttu-id="1cba5-163">Ange namnet på `connection`-stränginställningen som definieras programmets inställningsfil.</span><span class="sxs-lookup"><span data-stu-id="1cba5-163">Provide the `connection` string setting name defined in the application's settings file.</span></span> <span data-ttu-id="1cba5-164">Det används som nyckel för att hitta anslutningssträngen för att ansluta till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="1cba5-164">It's used as a key to find the connection string to connect to your storage account.</span></span>

1. <span data-ttu-id="1cba5-165">Definiera `direction` som `in`.</span><span class="sxs-lookup"><span data-stu-id="1cba5-165">Define the `direction` as `in`.</span></span> <span data-ttu-id="1cba5-166">Det läser data från bloben.</span><span class="sxs-lookup"><span data-stu-id="1cba5-166">It reads data from the blob.</span></span>

<span data-ttu-id="1cba5-167">Bindningar används till att ansluta till data i en funktion.</span><span class="sxs-lookup"><span data-stu-id="1cba5-167">Bindings are used to connect to data in your function.</span></span> <span data-ttu-id="1cba5-168">I det här exemplet har vi använt en indatabindning för att ansluta till användarbilder som ska bearbetas av funktionen som miniatyrbilder.</span><span class="sxs-lookup"><span data-stu-id="1cba5-168">In this example, we used an input binding to connect user images to be processed by our function as thumbnails.</span></span>
