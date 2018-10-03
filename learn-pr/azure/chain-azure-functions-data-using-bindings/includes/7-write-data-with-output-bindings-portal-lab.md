<span data-ttu-id="98bc6-101">I vår senaste övning implementerade vi ett scenario för att leta upp bokmärken i en Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="98bc6-101">In our last exercise, we implemented a scenario to look up bookmarks in an Azure Cosmos DB database.</span></span> <span data-ttu-id="98bc6-102">Vi konfigurerade en indatabindning att läsa data från vår bokmärkessamling.</span><span class="sxs-lookup"><span data-stu-id="98bc6-102">We configured an input binding to read data from our bookmarks collection.</span></span> <span data-ttu-id="98bc6-103">Men vi kan göra mer.</span><span class="sxs-lookup"><span data-stu-id="98bc6-103">But, we can do more.</span></span> <span data-ttu-id="98bc6-104">Vi utökar scenariot till att även omfatta dataskrivning.</span><span class="sxs-lookup"><span data-stu-id="98bc6-104">Let's expand the scenario to include writing.</span></span> <span data-ttu-id="98bc6-105">Titta på följande flödesschema:</span><span class="sxs-lookup"><span data-stu-id="98bc6-105">Consider the following flowchart:</span></span>

![Flödesschema som visar processen med att hitta ett bokmärke i vår Cosmos-DB-serverdel.](../media/7-add-bookmark-flow-small.png)

<span data-ttu-id="98bc6-110">I det här scenariot får vi begäranden om att lägga till bokmärken i vår samling.</span><span class="sxs-lookup"><span data-stu-id="98bc6-110">In this scenario, we'll receive requests to add bookmarks to our collection.</span></span> <span data-ttu-id="98bc6-111">Begärandena skickas i önskad nyckel eller önskat ID tillsammans med bokmärkets URL.</span><span class="sxs-lookup"><span data-stu-id="98bc6-111">The requests pass in the desired key, or ID, along with the bookmark URL.</span></span> <span data-ttu-id="98bc6-112">Som du ser i flödesschemat svarar vi med ett fel om nyckeln redan finns i vår serverdel.</span><span class="sxs-lookup"><span data-stu-id="98bc6-112">As you can see in the flowchart, we'll respond with an error if the key already exists in our back end.</span></span>

<span data-ttu-id="98bc6-113">Om den nyckel som skickades till oss *inte* hittas lägger vi till det nya bokmärket i vår databas.</span><span class="sxs-lookup"><span data-stu-id="98bc6-113">If the key that was passed to us is *not* found, we'll add the new bookmark to our database.</span></span> <span data-ttu-id="98bc6-114">Vi skulle kunna nöja oss här, men vi ska göra lite till.</span><span class="sxs-lookup"><span data-stu-id="98bc6-114">We could stop there, but let's do a little more.</span></span>

<span data-ttu-id="98bc6-115">Lägger du märke till ytterligare ett steg i flödesschemat?</span><span class="sxs-lookup"><span data-stu-id="98bc6-115">Notice another step in the flowchart?</span></span> <span data-ttu-id="98bc6-116">Så här långt vi inte gjort mycket med de data som vi tar emot när det gäller bearbetning.</span><span class="sxs-lookup"><span data-stu-id="98bc6-116">So far we haven't done much with the data that we receive in terms of processing.</span></span> <span data-ttu-id="98bc6-117">Vi flytta det som vi tar emot till en databas.</span><span class="sxs-lookup"><span data-stu-id="98bc6-117">We move what we receive into a database.</span></span> <span data-ttu-id="98bc6-118">I en verklig lösning skulle vi dock förmodligen bearbeta data på något sätt.</span><span class="sxs-lookup"><span data-stu-id="98bc6-118">However, in a real solution, it is possible that we'd probably process the data in some fashion.</span></span> <span data-ttu-id="98bc6-119">Vi kan välja att utföra all bearbetning i samma funktion, men i den här övningen visar vi ett mönster som avlastar vidare bearbetning till en annan komponent eller affärslogik.</span><span class="sxs-lookup"><span data-stu-id="98bc6-119">We can decide to do all processing in the same function, but in this lab we'll show a pattern that offloads further processing to another component or piece of business logic.</span></span>

<span data-ttu-id="98bc6-120">Vad kan vara ett bra exempel på den här avlasta arbete i vårt scenario med bokmärken?</span><span class="sxs-lookup"><span data-stu-id="98bc6-120">What might be a good example of this offloading of work in our bookmarks scenario?</span></span> <span data-ttu-id="98bc6-121">Så vad händer om vi skickar nytt bokmärke till en QR-kodgenererad tjänst?</span><span class="sxs-lookup"><span data-stu-id="98bc6-121">Well, what if we send the new bookmark to a QR code generation service?</span></span> <span data-ttu-id="98bc6-122">Den här tjänsten skulle, i sin tur, generera en QR-kod för URL:en, lagra avbildningen i blob-minnet och lägga till bildens qr-adress till posten i vår bokmärkessamling.</span><span class="sxs-lookup"><span data-stu-id="98bc6-122">That service would, in turn, generate a QR code for the URL, store the image in blob storage, and add the address of the QR image back into the entry in our bookmarks collection.</span></span> <span data-ttu-id="98bc6-123">Att anropa en tjänst för att skapa en QR-avbildning tar tid, så i stället för att vänta på resultatet lämnar vi över den till en funktion och låter denna ta hand om detta asynkront.</span><span class="sxs-lookup"><span data-stu-id="98bc6-123">Calling a service to generate a QR image is time consuming so, rather than wait for the result, we hand it off to a function and let it take care of this asynchronously.</span></span>

<span data-ttu-id="98bc6-124">Precis som Azure Functions har stöd för indatabindningar för integrering av olika källor, har den även en uppsättning mallar för utdatabindningar för att göra det lättare för dig att skriva data till datakällor.</span><span class="sxs-lookup"><span data-stu-id="98bc6-124">Just as Azure Functions supports input bindings for various integration sources, it also has a set of output bindings templates to make it easy for you to write data to data sources.</span></span> <span data-ttu-id="98bc6-125">Utdatabindningar har även konfigurerats i *function.json*-filen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-125">Output bindings are also configured in the *function.json* file.</span></span>  <span data-ttu-id="98bc6-126">Som vi kommer att se i den här övningen kan vi konfigurera vår funktion så att den fungerar med flera datakällor och tjänster.</span><span class="sxs-lookup"><span data-stu-id="98bc6-126">As you'll see in this exercise, we can configure our function to work with multiple data sources and services.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="98bc6-127">Den här övningen bygger vidare på den förra.</span><span class="sxs-lookup"><span data-stu-id="98bc6-127">This exercise builds on the previous one.</span></span> <span data-ttu-id="98bc6-128">Den använder samma Azure Cosmos DB-databas och indatabindning.</span><span class="sxs-lookup"><span data-stu-id="98bc6-128">It uses the same Azure Cosmos DB database and input binding.</span></span> <span data-ttu-id="98bc6-129">Om du inte har arbetat igenom den här delen rekommenderar vi gör det innan du fortsätter med den här övningen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-129">If you haven't worked through that unit, we recommend doing so before you proceed with this one.</span></span>

## <a name="create-an-http-triggered-function"></a><span data-ttu-id="98bc6-130">Skapa en HTTP-utlöst funktion</span><span class="sxs-lookup"><span data-stu-id="98bc6-130">Create an HTTP-triggered function</span></span>

1. <span data-ttu-id="98bc6-131">Kontrollera att du är inloggad på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.</span><span class="sxs-lookup"><span data-stu-id="98bc6-131">Make sure you are signed into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

2. <span data-ttu-id="98bc6-132">Navigera till den funktionsapp som du skapade i föregående avsnitt i portalen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-132">In the portal, navigate to the function app that you created in this module.</span></span>

3. <span data-ttu-id="98bc6-133">Välj Lägg till (**+**) bredvid knappen **Functions**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-133">Select the Add (**+**) button next to **Functions**.</span></span> <span data-ttu-id="98bc6-134">Den här åtgärden startar funktionsskapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-134">This action starts the function creation process.</span></span> 
4. <span data-ttu-id="98bc6-135">Sidan visar oss den aktuella uppsättningen med utlösare som stöds.</span><span class="sxs-lookup"><span data-stu-id="98bc6-135">The page shows us the current set of supported triggers.</span></span> <span data-ttu-id="98bc6-136">Välj **HTTP-utlösare**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-136">Select **HTTP trigger**.</span></span>

5. <span data-ttu-id="98bc6-137">I rutan **Ny funktion** som visas till höger anger du nedanstående värden:</span><span class="sxs-lookup"><span data-stu-id="98bc6-137">Fill out the **New Function** pane that's displayed at the right by using the following values:</span></span>

    |<span data-ttu-id="98bc6-138">Fält</span><span class="sxs-lookup"><span data-stu-id="98bc6-138">Field</span></span>  |<span data-ttu-id="98bc6-139">Värde</span><span class="sxs-lookup"><span data-stu-id="98bc6-139">Value</span></span>  |
    |---------|---------|
    |<span data-ttu-id="98bc6-140">Namn</span><span class="sxs-lookup"><span data-stu-id="98bc6-140">Name</span></span>     |   [!INCLUDE [func-name-add](./func-name-add.md)]     |
    | <span data-ttu-id="98bc6-141">Auktoriseringsnivå</span><span class="sxs-lookup"><span data-stu-id="98bc6-141">Authorization level</span></span> | <span data-ttu-id="98bc6-142">**Funktion**</span><span class="sxs-lookup"><span data-stu-id="98bc6-142">**Function**</span></span> |

6. <span data-ttu-id="98bc6-143">Skapa den nya funktionen genom att välja **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-143">Select **Create** to create your function.</span></span> <span data-ttu-id="98bc6-144">Detta öppnar filen **index.js** i kodredigeraren och en standardimplementering av den HTTP-utlösta funktionen visas.</span><span class="sxs-lookup"><span data-stu-id="98bc6-144">This action opens the **index.js** file in the code editor and displays a default implementation of the HTTP-triggered function.</span></span>

    > [!NOTE]
    > <span data-ttu-id="98bc6-145">I den här övningen snabbar vi upp saker och ting genom att använda *koden* och *konfigurationen* från den föregående delen som en utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="98bc6-145">In this exercise, we'll speed up things by using the *code* and *configuration* from the previous unit as a starting point.</span></span>

7. <span data-ttu-id="98bc6-146">Ersätt all kod i **index.js** med koden från följande kodavsnitt och klicka på **Spara** så att ändringen sparas:</span><span class="sxs-lookup"><span data-stu-id="98bc6-146">Replace all code in the **index.js** file with the code from the following snippet, and then select **Save** to save the change:</span></span>

   [!code-javascript[](../code/find-bookmark-single.js)]

   <span data-ttu-id="98bc6-147">Om den här koden ser bekant ut beror det på att den utgör implementeringen av vår [!INCLUDE [func-name-find](./func-name-find.md)]-funktion.</span><span class="sxs-lookup"><span data-stu-id="98bc6-147">If this code looks familiar, that's because it's the implementation of our [!INCLUDE [func-name-find](./func-name-find.md)] function.</span></span> <span data-ttu-id="98bc6-148">Som förväntat fungerar inte funktionen förrän vi definierar samma bindningar.</span><span class="sxs-lookup"><span data-stu-id="98bc6-148">As you would expect, the function won't work until we define the same bindings.</span></span>

1. <span data-ttu-id="98bc6-149">Öppna filen **function.json** från funktionen [!INCLUDE [func-name-add](./func-name-add.md)].</span><span class="sxs-lookup"><span data-stu-id="98bc6-149">Open the **function.json** file from the [!INCLUDE [func-name-add](./func-name-add.md)] function.</span></span>

11. <span data-ttu-id="98bc6-150">Ersätt innehållet i filen med följande JSON:</span><span class="sxs-lookup"><span data-stu-id="98bc6-150">Replace the contents of this file with the following JSON:</span></span>

    ```json
    {
      "bindings": [
        {
          "authLevel": "function",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req"
        },
        {
          "type": "http",
          "direction": "out",
          "name": "res"
        },
        {
          "type": "documentDB",
          "name": "bookmark",
          "databaseName": "func-io-learn-db",
          "collectionName": "Bookmarks",
          "connection": "unit3test_DOCUMENTDB",
          "direction": "in",
          "id": "{id}"
        }
      ],
      "disabled": false
    }
    ```

12. <span data-ttu-id="98bc6-151">Se till att **Spara** alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="98bc6-151">Make sure to **Save** all changes.</span></span>

<span data-ttu-id="98bc6-152">I föregående steg har vi konfigurerat bindningar för vår nya funktion genom att kopiera bindningsdefinitionerna från en annan.</span><span class="sxs-lookup"><span data-stu-id="98bc6-152">In the preceding steps, you configured bindings for your new function by copying binding definitions from another function.</span></span> <span data-ttu-id="98bc6-153">Vi kan naturligtvis skapa en ny bindning via användargränssnittet, men det är bra att förstå att det här alternativet är tillgängligt för dig.</span><span class="sxs-lookup"><span data-stu-id="98bc6-153">Of course, you could have created a new binding through the UI, but it is good to understand that this alternative is available to you.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="98bc6-154">Prova</span><span class="sxs-lookup"><span data-stu-id="98bc6-154">Try it out</span></span>

1. <span data-ttu-id="98bc6-155">Välj **Hämta funktionswebbadress** längst upp till höger och välj **Standard (funktionsnyckel)**. Sedan kopierar du funktionens URL genom att klicka på **Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-155">Select **Get function URL** at the top right, select **default (Function key)**, and then select **Copy** to copy the function's URL.</span></span>

2. <span data-ttu-id="98bc6-156">Klistra in URL:en i adressfältet för din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="98bc6-156">Paste the copied URL into your browser's address bar.</span></span> <span data-ttu-id="98bc6-157">Lägg till frågesträngsvärdet `&id=docs` i slutet av den här webbadressen och tryck på Enter för att utföra begäran.</span><span class="sxs-lookup"><span data-stu-id="98bc6-157">Add the query string value `&id=docs` to the end of the URL, and then press Enter to execute the request.</span></span> <span data-ttu-id="98bc6-158">Om allt går som det ska bör du se ett svar som innehåller en URL till den här resursen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-158">If all goes well, you should see a response that includes a URL to that resource.</span></span>

<span data-ttu-id="98bc6-159">Så, var befinner vi oss?</span><span class="sxs-lookup"><span data-stu-id="98bc6-159">So, where are we at?</span></span> <span data-ttu-id="98bc6-160">Dessutom har hittills egentligen bara replikerat det som vi gjorde i föregående övning.</span><span class="sxs-lookup"><span data-stu-id="98bc6-160">Well, so far we've really just replicated what we did in the previous lab.</span></span> <span data-ttu-id="98bc6-161">Men det är okej.</span><span class="sxs-lookup"><span data-stu-id="98bc6-161">But that's okay.</span></span> <span data-ttu-id="98bc6-162">Vi kopierar det vi som gjorde i den senaste testmiljön så att det kan fungera som en startpunkt för denna.</span><span class="sxs-lookup"><span data-stu-id="98bc6-162">We're copying what we did in the last lab to serve as a starting point for this one.</span></span> <span data-ttu-id="98bc6-163">Vi ska snart jobba på de nya momenten.</span><span class="sxs-lookup"><span data-stu-id="98bc6-163">We'll work on the new stuff next.</span></span> <span data-ttu-id="98bc6-164">Det vill säga, vi ska skriva till databasen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-164">That is, we'll write to our database.</span></span> <span data-ttu-id="98bc6-165">För detta behöver vi en *utdatabindning*.</span><span class="sxs-lookup"><span data-stu-id="98bc6-165">For that, we'll need an *output binding*.</span></span>

## <a name="define-azure-cosmos-db-output-binding"></a><span data-ttu-id="98bc6-166">Definiera Cosmos-DB-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="98bc6-166">Define Azure Cosmos DB output binding</span></span>

<span data-ttu-id="98bc6-167">I stället för att definiera en ny utdatabindning genom att gå via användargränssnittet, ska du skapa den här bindningen genom att uppdatera konfigurationsfilen *function.json*, manuellt.</span><span class="sxs-lookup"><span data-stu-id="98bc6-167">Rather than define a new output binding by going through the user interface, you'll create this binding by updating the configuration file, *function.json*, by hand.</span></span>

1. <span data-ttu-id="98bc6-168">Kontrollera att filen *function.json* för [!INCLUDE [func-name-add](./func-name-add.md)] är öppen i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="98bc6-168">Make sure the *function.json* file for [!INCLUDE [func-name-add](./func-name-add.md)] is open in the editor.</span></span>

1. <span data-ttu-id="98bc6-169">Kopiera bindningen `bookmark` i filen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-169">Copy the binding that's named `bookmark` in that file.</span></span>

1. <span data-ttu-id="98bc6-170">Placera markören direkt efter den avslutande klammern (}) och precis före den avslutande hakparentesen (]).</span><span class="sxs-lookup"><span data-stu-id="98bc6-170">Place your cursor directly after the closing curly bracket (}), and right before the closing square bracket (]).</span></span> <span data-ttu-id="98bc6-171">Lägg till ett kommatecken (,) och klistra sedan in kopian av bindningen här.</span><span class="sxs-lookup"><span data-stu-id="98bc6-171">Add a comma (,), and then paste the copy of the binding here.</span></span> <span data-ttu-id="98bc6-172">Konfigurationsfilen *function.json* bör nu se ut så här:</span><span class="sxs-lookup"><span data-stu-id="98bc6-172">Your *function.json* config file should now look like the following:</span></span>

   [!code-json[](../code/config-new-entry.json?highlight=22-31)]

1. <span data-ttu-id="98bc6-173">Redigera den bindning som du klistrade in, med följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="98bc6-173">Edit the binding you pasted, with the following changes:</span></span>

    |<span data-ttu-id="98bc6-174">Egenskap</span><span class="sxs-lookup"><span data-stu-id="98bc6-174">Property</span></span>   |<span data-ttu-id="98bc6-175">Tidigare värde</span><span class="sxs-lookup"><span data-stu-id="98bc6-175">Old value</span></span>  |<span data-ttu-id="98bc6-176">Nytt värde</span><span class="sxs-lookup"><span data-stu-id="98bc6-176">New value</span></span>  |
    |---------|---------|---------|
    |<span data-ttu-id="98bc6-177">namn</span><span class="sxs-lookup"><span data-stu-id="98bc6-177">name</span></span>     |   <span data-ttu-id="98bc6-178">bokmärke</span><span class="sxs-lookup"><span data-stu-id="98bc6-178">bookmark</span></span>      |  <span data-ttu-id="98bc6-179">**newbookmark**</span><span class="sxs-lookup"><span data-stu-id="98bc6-179">**newbookmark**</span></span>       |
    |<span data-ttu-id="98bc6-180">riktning</span><span class="sxs-lookup"><span data-stu-id="98bc6-180">direction</span></span>     |   <span data-ttu-id="98bc6-181">in</span><span class="sxs-lookup"><span data-stu-id="98bc6-181">in</span></span>      |   <span data-ttu-id="98bc6-182">**ut**</span><span class="sxs-lookup"><span data-stu-id="98bc6-182">**out**</span></span>      |
    |<span data-ttu-id="98bc6-183">id</span><span class="sxs-lookup"><span data-stu-id="98bc6-183">id</span></span>     |      <span data-ttu-id="98bc6-184">{id}</span><span class="sxs-lookup"><span data-stu-id="98bc6-184">{id}</span></span>   |   <span data-ttu-id="98bc6-185">**ta bort den här egenskapen. Den finns inte för utdatabindningen.**</span><span class="sxs-lookup"><span data-stu-id="98bc6-185">**delete this property. It does not exist for the output binding.**</span></span>      |

1. <span data-ttu-id="98bc6-186">När du har gjort dessa ändringar bör din fil som ser ut som följande JSON:</span><span class="sxs-lookup"><span data-stu-id="98bc6-186">After you've made these changes, your file looks like the following JSON:</span></span>

    [!code-json[](../code/config-q-complete.json?highlight=22-30)]

<span data-ttu-id="98bc6-187">Detta var bara en demonstration av hur du även kan skapa bindningar direkt i konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-187">That was just a demo of how you can also create bindings directly in the configuration file.</span></span> <span data-ttu-id="98bc6-188">Det är klokt i detta exempel eftersom du återanvänder egenskaperna från en annan bindning.</span><span class="sxs-lookup"><span data-stu-id="98bc6-188">In this example, it makes sense because you are reusing the properties from another binding.</span></span> <span data-ttu-id="98bc6-189">Det vill säga du ska återanvända `databaseName`, `collectionName`, och `connection` som du redan har konfigurerat för din Azure Cosmos DB-indatabindning.</span><span class="sxs-lookup"><span data-stu-id="98bc6-189">That is, you're reusing the `databaseName`, `collectionName`, and `connection` that you already configured for your Azure Cosmos DB input binding.</span></span>

> [!NOTE]
> <span data-ttu-id="98bc6-190">Det faktiska värdet för `connection` i ovanstående JSON-fil blir det namn som anslutningen fick när den skapades.</span><span class="sxs-lookup"><span data-stu-id="98bc6-190">The actual value of `connection` in the preceding JSON file is whatever name your connection was given when it was created.</span></span>

<span data-ttu-id="98bc6-191">Innan vi uppdaterar vår kod vi lägger till ännu en bindning som gör att vi kan skicka meddelanden till en kö.</span><span class="sxs-lookup"><span data-stu-id="98bc6-191">Before we update our code, let's add one more binding that will enable us to post messages to a queue.</span></span>

## <a name="define-azure-queue-storage-output-binding"></a><span data-ttu-id="98bc6-192">Definiera Azure Queue Storage-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="98bc6-192">Define Azure Queue Storage output binding</span></span>

<span data-ttu-id="98bc6-193">Azure Queue Storage är en tjänst för lagring av meddelanden som kan nås var som helst i världen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-193">Azure Queue storage is a service for storing messages that can be accessed from anywhere in the world.</span></span> <span data-ttu-id="98bc6-194">Storleken på ett enda meddelande kan vara upp till 64 KB stort och en kö kan innehålla miljontals meddelanden&mdash;, upp till den totala kapaciteten för ett lagringskonto som har definierats.</span><span class="sxs-lookup"><span data-stu-id="98bc6-194">The size of a single message can be as much as 64 KB, and a queue can contain millions of messages&mdash;up to the total capacity of the storage account in which it is defined.</span></span> <span data-ttu-id="98bc6-195">I följande diagram visas på en hög nivå hur en kö används i vårt scenario:</span><span class="sxs-lookup"><span data-stu-id="98bc6-195">The following diagram shows at a high level how a queue is used in our scenario:</span></span>

![Diagram som visar konceptet med en lagringskö och två funktioner varav den ena push-överför och den andra stoppar in meddelanden i kön](../media/7-q-logical-small.png)

<span data-ttu-id="98bc6-197">Här ser vi att den nya funktionen, [!INCLUDE [func-name-add](./func-name-add.md)], lägger till meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="98bc6-197">Here you can see that the new function, [!INCLUDE [func-name-add](./func-name-add.md)], adds messages to a queue.</span></span> <span data-ttu-id="98bc6-198">En annan funktion&mdash;, till exempel en fiktiv funktion med namnet *gen-qr-code*&mdash;, visar popup-meddelanden från samma kö och bearbetar begäran.</span><span class="sxs-lookup"><span data-stu-id="98bc6-198">Another function&mdash;for example, a fictitious function called *gen-qr-code*&mdash;will pop messages from the same queue and process the request.</span></span>  <span data-ttu-id="98bc6-199">Eftersom vi skriver, eller *pushar*, meddelanden till kön från [!INCLUDE [func-name-add](./func-name-add.md)], lägger vi till en ny utdatabindning i vår lösning.</span><span class="sxs-lookup"><span data-stu-id="98bc6-199">Since we write, or *push*, messages to the queue from [!INCLUDE [func-name-add](./func-name-add.md)], we'll add a new output binding to your solution.</span></span> <span data-ttu-id="98bc6-200">Den här gången ska vi skapa bindningen i portalanvändargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="98bc6-200">Let's create the binding through the portal UI this time.</span></span>

1. <span data-ttu-id="98bc6-201">Välj **Integrera** i funktionsmenyn till vänster och öppna integrationsfliken.</span><span class="sxs-lookup"><span data-stu-id="98bc6-201">Select **Integrate** in the left function menu to open the integration tab.</span></span>

2. <span data-ttu-id="98bc6-202">Välj **Nya utdata** i kolumnen **Utdata**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-202">Select **New Output** in the **Outputs** column.</span></span>
    <span data-ttu-id="98bc6-203">En lista över alla typer av möjliga utdatabindningar visas.</span><span class="sxs-lookup"><span data-stu-id="98bc6-203">A list of all possible output binding types is displayed.</span></span>

3. <span data-ttu-id="98bc6-204">Välj **Azure Queue Storage** i listan och välj sedan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-204">In the list, select **Azure Queue Storage**, then select **Select**.</span></span>
    <span data-ttu-id="98bc6-205">Den här åtgärden öppnar konfigurationssidan för Azure Queue Storage-utdata.</span><span class="sxs-lookup"><span data-stu-id="98bc6-205">This action opens the Azure Queue Storage output configuration page.</span></span>

   <span data-ttu-id="98bc6-206">Nu ska vi konfigurera en anslutning till ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="98bc6-206">Next, we'll set up a storage account connection.</span></span> <span data-ttu-id="98bc6-207">Här kommer vår kö att finnas.</span><span class="sxs-lookup"><span data-stu-id="98bc6-207">This is where our queue will be hosted.</span></span>

4. <span data-ttu-id="98bc6-208">Välj **nya** till höger om fältet **lagringskontoanslutning**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-208">To the right of the **Storage account connection** field, select **new**.</span></span>
   <span data-ttu-id="98bc6-209">Markeringsfönstret **Lagringskonto** öppnas.</span><span class="sxs-lookup"><span data-stu-id="98bc6-209">The **Storage Account** selection pane opens.</span></span>

5. <span data-ttu-id="98bc6-210">När vi satte igång den här modulen och skapade vår funktionsapp, skapades även ett lagringskonto vid samma tillfälle.</span><span class="sxs-lookup"><span data-stu-id="98bc6-210">When we started this module and you created your function app, a storage account was also created at that time.</span></span> <span data-ttu-id="98bc6-211">Det visas i det här fösntret så du kan välja det.</span><span class="sxs-lookup"><span data-stu-id="98bc6-211">It's listed in this pane, so select it.</span></span> <span data-ttu-id="98bc6-212">Fältet **Lagringskontoanslutning** fylls i med namnet på en anslutning.</span><span class="sxs-lookup"><span data-stu-id="98bc6-212">The **Storage account connection** field is populated with the name of a connection.</span></span> <span data-ttu-id="98bc6-213">Om du vill visa anslutningssträngens värde väljer du **Visa värde**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-213">If you want to see the connection string value, select **show value**.</span></span>

6. <span data-ttu-id="98bc6-214">Även om vi kan lämna övriga fält på den här sidan med sina standardvärden ska vi ändra följande för att göra egenskaperna mer begripliga:</span><span class="sxs-lookup"><span data-stu-id="98bc6-214">Although we could keep the default values in all the other fields, let's change the following to lend more meaning to the properties:</span></span>

    |<span data-ttu-id="98bc6-215">Egenskap</span><span class="sxs-lookup"><span data-stu-id="98bc6-215">Property</span></span>  |<span data-ttu-id="98bc6-216">Tidigare värde</span><span class="sxs-lookup"><span data-stu-id="98bc6-216">Old value</span></span>  |<span data-ttu-id="98bc6-217">Nytt värde</span><span class="sxs-lookup"><span data-stu-id="98bc6-217">New value</span></span>  | <span data-ttu-id="98bc6-218">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="98bc6-218">Description</span></span> |
    |---------|---------|---------|---------|
    |<span data-ttu-id="98bc6-219">Könamn</span><span class="sxs-lookup"><span data-stu-id="98bc6-219">Queue name</span></span>     |    <span data-ttu-id="98bc6-220">utkö</span><span class="sxs-lookup"><span data-stu-id="98bc6-220">outqueue</span></span>     |  <span data-ttu-id="98bc6-221">**bookmarks-post-process**</span><span class="sxs-lookup"><span data-stu-id="98bc6-221">**bookmarks-post-process**</span></span>      | <span data-ttu-id="98bc6-222">Det här är namnet på den kö där vi placerar bokmärken så att de kan bearbetas ytterligare via en annan funktion.</span><span class="sxs-lookup"><span data-stu-id="98bc6-222">The name of the queue where we're placing bookmarks so that they can be processed further by another function.</span></span> |
    | <span data-ttu-id="98bc6-223">Meddelandeparameternamn</span><span class="sxs-lookup"><span data-stu-id="98bc6-223">Message parameter name</span></span>    |  <span data-ttu-id="98bc6-224">outputQueueItem</span><span class="sxs-lookup"><span data-stu-id="98bc6-224">outputQueueItem</span></span>       |   <span data-ttu-id="98bc6-225">**newmessage**</span><span class="sxs-lookup"><span data-stu-id="98bc6-225">**newmessage**</span></span>      | <span data-ttu-id="98bc6-226">Det här är den bindningsegenskap som vi ska använda i koden.</span><span class="sxs-lookup"><span data-stu-id="98bc6-226">The binding property we'll use in code.</span></span> |

7. <span data-ttu-id="98bc6-227">Kom ihåg att välja **Spara** för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="98bc6-227">Remember to select **Save** to save your changes.</span></span>

## <a name="update-function-implementation"></a><span data-ttu-id="98bc6-228">Uppdatera funktionsimplementeringen</span><span class="sxs-lookup"><span data-stu-id="98bc6-228">Update function implementation</span></span>

<span data-ttu-id="98bc6-229">Nu har vi alla våra bindningar konfigurerade för funktionen [!INCLUDE [func-name-add](./func-name-add.md)].</span><span class="sxs-lookup"><span data-stu-id="98bc6-229">We now have all our bindings set up for the [!INCLUDE [func-name-add](./func-name-add.md)] function.</span></span> <span data-ttu-id="98bc6-230">Nu är det dags att använda dem i vår funktion.</span><span class="sxs-lookup"><span data-stu-id="98bc6-230">It's time to use them in our function.</span></span>

1.  <span data-ttu-id="98bc6-231">Välj vår funktion [!INCLUDE [func-name-add](./func-name-add.md)] så öppnas filen **index.js** i kodredigeraren.</span><span class="sxs-lookup"><span data-stu-id="98bc6-231">Select your function, [!INCLUDE [func-name-add](./func-name-add.md)], to open the **index.js** file in the code editor.</span></span>

2. <span data-ttu-id="98bc6-232">Ersätt all kod i *index.js* med koden från följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="98bc6-232">Replace all the code in the *index.js* file with the code from the following snippet:</span></span>

   [!code-javascript[](../code/add-bookmark.js)]

<span data-ttu-id="98bc6-233">Nu ska vi analysera på detaljnivå vad den här koden gör:</span><span class="sxs-lookup"><span data-stu-id="98bc6-233">Let's break down what this code does:</span></span>

* <span data-ttu-id="98bc6-234">Eftersom den här funktionen förändrar våra data kan vi förvänta oss att HTTP-begäran är en POST och att bokmärkesdata ska ingå i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="98bc6-234">Because this function changes our data, we expect the HTTP request to be a POST and the bookmark data to be part of the request body.</span></span>
* <span data-ttu-id="98bc6-235">Vår Azure Cosmos DB-indatabindning försöker hämta ett dokument eller ett bokmärke med hjälp av den `id` som vi tar emot.</span><span class="sxs-lookup"><span data-stu-id="98bc6-235">Our Azure Cosmos DB input binding attempts to retrieve a document, or bookmark, by using the `id` that we receive.</span></span> <span data-ttu-id="98bc6-236">Om den hittar en post ställs objektet `bookmark` in.</span><span class="sxs-lookup"><span data-stu-id="98bc6-236">If it finds an entry, the `bookmark` object will be set.</span></span> <span data-ttu-id="98bc6-237">Villkoret `if(bookmark)` kontrollerar om en post hittades.</span><span class="sxs-lookup"><span data-stu-id="98bc6-237">The `if(bookmark)` condition checks to see whether an entry was found.</span></span>
* <span data-ttu-id="98bc6-238">Att lägga till den i databasen är lika enkelt som att ställa in `context.bindings.newbookmark`-bindningsparametern för den nya bokmärkespost som vi har skapat som en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="98bc6-238">Adding to the database is as simple as setting the `context.bindings.newbookmark` binding parameter to the new bookmark entry, which we have created as a JSON string.</span></span>
* <span data-ttu-id="98bc6-239">Att skicka ett meddelande till vår kö är lika enkelt som att konfigurera `context.bindings.newmessage parameter`.</span><span class="sxs-lookup"><span data-stu-id="98bc6-239">Posting a message to our queue is as simple as setting the  `context.bindings.newmessage parameter`.</span></span>

> [!NOTE]
> <span data-ttu-id="98bc6-240">Den enda aktivitet som du utförde var att skapa en köbindning.</span><span class="sxs-lookup"><span data-stu-id="98bc6-240">The only task you performed was to create a queue binding.</span></span> <span data-ttu-id="98bc6-241">Du skapade aldrig kön uttryckligen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-241">You never created the queue explicitly.</span></span> <span data-ttu-id="98bc6-242">Du har nu sett hur kraftfulla bindningar är!</span><span class="sxs-lookup"><span data-stu-id="98bc6-242">You are witnessing the power of bindings!</span></span> <span data-ttu-id="98bc6-243">Precis som följande text anger skapas kön automatiskt åt dig om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="98bc6-243">As the following callout says, the queue is automatically created for you if it doesn't exist.</span></span>

![Skärmbild som visar att kön kommer att skapas automatiskt.](../media/7-q-auto-create-small.png)

<span data-ttu-id="98bc6-245">Klart!</span><span class="sxs-lookup"><span data-stu-id="98bc6-245">So, that's it.</span></span> <span data-ttu-id="98bc6-246">Låt oss titta på vårt arbete i praktiken i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="98bc6-246">Let's see our work in action in the next section.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="98bc6-247">Prova</span><span class="sxs-lookup"><span data-stu-id="98bc6-247">Try it out</span></span>

<span data-ttu-id="98bc6-248">Nu när vi har flera utdatabindningar blir testen lite svårare.</span><span class="sxs-lookup"><span data-stu-id="98bc6-248">Now that we have multiple output bindings, testing becomes a little trickier.</span></span> <span data-ttu-id="98bc6-249">Medan vi i tidigare övningar nöjde oss med att testa genom att skicka en HTTP-begäran och en frågesträng, vill vi nu utföra en HTTP-post.</span><span class="sxs-lookup"><span data-stu-id="98bc6-249">In previous labs we were content to test by sending an HTTP request and a query string, but we'll want to perform an HTTP post this time.</span></span> <span data-ttu-id="98bc6-250">Vi måste också kontrollera om meddelandena kommer fram till kön.</span><span class="sxs-lookup"><span data-stu-id="98bc6-250">We also need to check to see whether messages are making it into a queue.</span></span>

1. <span data-ttu-id="98bc6-251">Med vår funktion, [!INCLUDE [func-name-add](./func-name-add.md)], vald i portalen Funktionsappar väljer du menyalternativet Test längst till vänster så att det expanderas.</span><span class="sxs-lookup"><span data-stu-id="98bc6-251">With our function, [!INCLUDE [func-name-add](./func-name-add.md)], selected in the Function Apps portal, select the Test menu item at the far left to expand it.</span></span>

2. <span data-ttu-id="98bc6-252">Välj menyalternativet **Testa** och se till att testpanelen är öppen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-252">Select the **Test** menu item, and verify that you have the test pane open.</span></span> <span data-ttu-id="98bc6-253">Det bör se ut ungefär som på följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="98bc6-253">The following screenshot shows what it should look like:</span></span>

    ![Skärmbild som visar funktionen Testpanel expanderad.](../media/7-test-panel-open-small.png)

    > [!IMPORTANT]
    > <span data-ttu-id="98bc6-255">Se till att **POST** är valt i listrutan för HTTP-metod.</span><span class="sxs-lookup"><span data-stu-id="98bc6-255">Make sure that **POST** is selected in the HTTP method drop-down list.</span></span>

3. <span data-ttu-id="98bc6-256">Ersätt hela innehållet i begärandetexten med följande JSON-nyttolast:</span><span class="sxs-lookup"><span data-stu-id="98bc6-256">Replace the content of the request body with the following JSON payload:</span></span>

    ```json
    {
        "id": "docs",
        "url": "https://docs.microsoft.com/azure"
    }
    ```

4. <span data-ttu-id="98bc6-257">Välj **Kör** längst ned på testpanelen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-257">Select **Run** at the bottom of the test pane.</span></span>

5. <span data-ttu-id="98bc6-258">Kontrollera att meddelandet ”Bokmärket har lagts till” visas i fönstret **Utdata** som du ser i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="98bc6-258">Verify that the **Output** window displays the "Bookmark already exists" message, as shown in the following diagram:</span></span>

    ![En skärmbild som visar testpanelen och resultatet av ett misslyckat test.](../media/7-test-exists-small.png)

6. <span data-ttu-id="98bc6-260">Ersätt nu begärandetexten med följande nyttolast:</span><span class="sxs-lookup"><span data-stu-id="98bc6-260">Replace the request body with the following payload:</span></span>

    ```json
    {
        "id": "github",
        "url": "https://www.github.com"
    }
    ```
7. <span data-ttu-id="98bc6-261">Välj **Kör** längst ned på testpanelen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-261">Select **Run** at the bottom of the test pane.</span></span>

8. <span data-ttu-id="98bc6-262">Kontrollera att meddelandet ”Bokmärket har lagts till” visas i rutan *Utdata* som du ser i följande diagram.</span><span class="sxs-lookup"><span data-stu-id="98bc6-262">Verify the that *Output* box displays the "bookmark added" message as shown in the following diagram.</span></span>

    ![Skärmbild som visar testpanelen och resultatet av ett lyckat test.](../media/7-test-success-small.png)

<span data-ttu-id="98bc6-264">Grattis!</span><span class="sxs-lookup"><span data-stu-id="98bc6-264">Congratulations!</span></span> <span data-ttu-id="98bc6-265">[!INCLUDE [func-name-add](./func-name-add.md)] fungerar som avsett, men vad gäller för den köåtgärd som vi hade i koden?</span><span class="sxs-lookup"><span data-stu-id="98bc6-265">The [!INCLUDE [func-name-add](./func-name-add.md)] works as designed, but what about that queue operation we had in the code?</span></span> <span data-ttu-id="98bc6-266">Så nu ska vi se om något har skrivits till en kö.</span><span class="sxs-lookup"><span data-stu-id="98bc6-266">Well, let's go see whether something was written to a queue.</span></span>

### <a name="verify-that-a-message-is-written-to-the-queue"></a><span data-ttu-id="98bc6-267">Kontrollera att ett meddelande skrivs till vår kö</span><span class="sxs-lookup"><span data-stu-id="98bc6-267">Verify that a message is written to the queue</span></span>

<span data-ttu-id="98bc6-268">Azure Queue Storage-köer finns på ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="98bc6-268">Azure Queue Storage queues are hosted in a storage account.</span></span> <span data-ttu-id="98bc6-269">Du har valt lagringskontot i den här övningen redan när du skapar utdatabindningen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-269">You already selected the storage account in this exercise when you created the output binding.</span></span>

1. <span data-ttu-id="98bc6-270">Gå till huvudsökrutan i Azure-portalen och skriv **lagringskonton**. Välj **Lagringskonton** bland sökresultaten under kategorin **Tjänster**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-270">In the main search box in the Azure portal, type **storage accounts**, and in the results list, under **Services**, select **Storage accounts**.</span></span>

      ![Skärmbild som visar sökresultatet för lagringskontot i den huvudsakliga sökrutan.](../media/7-search-for-sa-small.png)

2. <span data-ttu-id="98bc6-272">I listan över konton som ska returneras väljer du det lagringskonto som du använde för att skapa utdatabindningen **newmessage**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-272">In the list of storage accounts that are returned, select the storage account that you used to create the **newmessage** output binding.</span></span>
   <span data-ttu-id="98bc6-273">Inställningarna för lagringskontot visas i huvudfönstret i portalen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-273">The storage account settings are displayed in the main window of the portal.</span></span>

3. <span data-ttu-id="98bc6-274">Välj alternativet **Köer** från listan över **tjänster**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-274">In the **Services** list, select the **Queues** item.</span></span>
   <span data-ttu-id="98bc6-275">Då visas en lista över köer som värdhanteras av det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="98bc6-275">A list of queues hosted by this storage account is displayed.</span></span> <span data-ttu-id="98bc6-276">Kontrollera att kön **bookmarks-post-process** finns, enligt följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="98bc6-276">Verify that the **bookmarks-post-process** queue exists, as shown in the following screenshot:</span></span>

      ![Skärmbild som visar vår kö i listan över köer som värdhanteras av det här lagringskontot](../media/7-q-in-list-small.png)

4. <span data-ttu-id="98bc6-278">Öppna kön genom att välja **bookmarks-post-process**.</span><span class="sxs-lookup"><span data-stu-id="98bc6-278">Select **bookmarks-post-process** to open the queue.</span></span>
   <span data-ttu-id="98bc6-279">De meddelanden som finns i kön visas i en lista.</span><span class="sxs-lookup"><span data-stu-id="98bc6-279">The messages that are in the queue are displayed in a list.</span></span> <span data-ttu-id="98bc6-280">Om allt gått enligt planen innehåller kön meddelandet som du har skapade när du skickade ett bokmärke till databasen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-280">If all went according to plan, the queue includes the message that you posted when you added a bookmark to the database.</span></span> <span data-ttu-id="98bc6-281">Det ska se ut som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="98bc6-281">It should look like the following:</span></span>

    ![Skärmbild som visar vårt meddelande i kön](../media/7-message-in-q-small.png)

   <span data-ttu-id="98bc6-283">I det här exemplet ser du att meddelandet har fått ett unikt ID och fältet **MEDDELANDETEXT** visar vårt bokmärke i JSON-strängformat.</span><span class="sxs-lookup"><span data-stu-id="98bc6-283">In this example, you can see that the message was given a unique ID, and the **MESSAGE TEXT** field displays your bookmark in JSON string format.</span></span>

5. <span data-ttu-id="98bc6-284">Du kan testa funktionen ytterligare genom att ändra begärandetexten i panelen Test med nya ID-/url-datauppsättningar och köra funktionen.</span><span class="sxs-lookup"><span data-stu-id="98bc6-284">You can test the function further by changing the request body in the test pane with new id/url sets and running the function.</span></span> <span data-ttu-id="98bc6-285">Titta på den här kön för att se fler meddelanden tas emot.</span><span class="sxs-lookup"><span data-stu-id="98bc6-285">Watch this queue to see more messages arrive.</span></span> <span data-ttu-id="98bc6-286">Du kan även leta i databasen för att kontrollera att nya poster har lagts till.</span><span class="sxs-lookup"><span data-stu-id="98bc6-286">You can also look at the database to verify that new entries have been added.</span></span>

<span data-ttu-id="98bc6-287">I den här övningen har vi utökat dina kunskaper om bindningar till utdatabindningar genom att skriva data till din Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="98bc6-287">In this lab, we expanded your knowledge of bindings to output bindings, writing data to your Azure Cosmos DB.</span></span> <span data-ttu-id="98bc6-288">Vi har gått vidare och lagt till ännu en utgående bindning för att skicka meddelanden till en Azure-kö.</span><span class="sxs-lookup"><span data-stu-id="98bc6-288">We went further and added another output binding to post messages to an Azure queue.</span></span> <span data-ttu-id="98bc6-289">Detta visar den verkliga kraften i bindningar för att hjälpa dig att forma och flytta data från inkommande källor till en mängd olika mål.</span><span class="sxs-lookup"><span data-stu-id="98bc6-289">This demonstrates the true power of bindings to help you shape and move data from incoming sources to a variety of destinations.</span></span> <span data-ttu-id="98bc6-290">Vi har inte skrivit någon databaskod eller varit tvungna att hantera anslutningssträngar själva.</span><span class="sxs-lookup"><span data-stu-id="98bc6-290">We haven't written any database code or had to manage connection strings ourselves.</span></span> <span data-ttu-id="98bc6-291">I stället vi har konfigurerat bindningar deklarativt och låtit plattformen ta hand om säkra anslutningar, skala vår funktion och skala våra anslutningar.</span><span class="sxs-lookup"><span data-stu-id="98bc6-291">Instead, we configured bindings declaratively and let the platform take care of securing connections, scaling our function, and scaling our connections.</span></span>