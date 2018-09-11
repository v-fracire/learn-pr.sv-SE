<span data-ttu-id="63d96-101">I den här övningen skapar du ett enkelt AngularJS-program som hanteras i Node.js och använder Express för routning.</span><span class="sxs-lookup"><span data-stu-id="63d96-101">In this exercise, you're going to create a simple AngularJS application hosted in Node.js and use Express for routing.</span></span> <span data-ttu-id="63d96-102">I serverdelen fungerar MongoDB som datalager.</span><span class="sxs-lookup"><span data-stu-id="63d96-102">On the back end, MongoDB will serve as your data store.</span></span> <span data-ttu-id="63d96-103">Programmet är en bokdatabas där du kommer att kunna visa, lägga till och ta bort böcker.</span><span class="sxs-lookup"><span data-stu-id="63d96-103">The application is a book database, where you will be able to list, add, and delete books.</span></span>

> [!Important]
> <span data-ttu-id="63d96-104">Det här är ett enkelt program.</span><span class="sxs-lookup"><span data-stu-id="63d96-104">This is a simple application.</span></span> <span data-ttu-id="63d96-105">Dess syfte är att testa den nyinstallerade MEAN-stacken.</span><span class="sxs-lookup"><span data-stu-id="63d96-105">It's purpose is to test the newly installed MEAN stack.</span></span> <span data-ttu-id="63d96-106">Det här programmet är inte tillräckligt säkert eller redo för produktionsanvändning.</span><span class="sxs-lookup"><span data-stu-id="63d96-106">This application is not sufficiently secure or ready for production use.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="63d96-107">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="63d96-107">Connect to the VM</span></span>

<span data-ttu-id="63d96-108">Om du inte fortfarande är ansluten till den virtuella datorn kör du följande kommando.</span><span class="sxs-lookup"><span data-stu-id="63d96-108">If you aren't still connected to your VM, run the following command.</span></span> <span data-ttu-id="63d96-109">Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.</span><span class="sxs-lookup"><span data-stu-id="63d96-109">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="create-the-back-end"></a><span data-ttu-id="63d96-110">Skapa serverdelen</span><span class="sxs-lookup"><span data-stu-id="63d96-110">Create the back end</span></span>

1. <span data-ttu-id="63d96-111">Skapa en mappstruktur för det nya exempelprogrammet med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="63d96-111">Create a folder structure for your new sample application with the following command.</span></span>

    ```bash
    mkdir ~/Books
    mkdir ~/Books/app
    mkdir ~/Books/public
    ```

    <span data-ttu-id="63d96-112">På din administratörsanvändares startplats skapade du en mapp med namnet ”Books” (böcker) som ska innehålla projektets app och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="63d96-112">In your admin user's home location, you created a folder called "Books" to contain your project's app and its dependencies.</span></span> <span data-ttu-id="63d96-113">I den mappen skapade du en ”app”-mapp som ska innehålla alla programresurser och skript.</span><span class="sxs-lookup"><span data-stu-id="63d96-113">Within that folder, you created an "app" folder to contain all your application resources and scripts.</span></span> <span data-ttu-id="63d96-114">Slutligen skapar vi en ”public” (offentlig) mapp som ska lagra alla filer på klientsidan som skickas direkt till lämpliga HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="63d96-114">Finally, we will also create a "public" folder to hold all of the client-side files that will be served up directly to appropriate HTTP requests.</span></span>

1. <span data-ttu-id="63d96-115">Installera **Express** för att hantera routning av dina HTTP-begäranden och bestämma vilket innehåll som ska returneras till användare av ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="63d96-115">Install **Express** to handle routing of your HTTP requests, to decide what content to return to a user of your web application.</span></span>

    <span data-ttu-id="63d96-116">Kör följande kommando för att lägga till Express som ett paket som ditt webbprogram ska använda.</span><span class="sxs-lookup"><span data-stu-id="63d96-116">Run the following command to add Express as a package for your web application to use.</span></span>

      ```bash
      npm install express
      ```

1. <span data-ttu-id="63d96-117">Installera **Mongoose** för att hjälpa till med vidarebefordran av dina bokdata mellan MongoDB och HTTP-begäranderoutning.</span><span class="sxs-lookup"><span data-stu-id="63d96-117">Install **Mongoose** to help relay your book data between MongoDB and the HTTP request routing.</span></span>

    <span data-ttu-id="63d96-118">Det körs frågor mot bokinformationen via REST API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="63d96-118">The book information will be queried via REST API requests.</span></span> <span data-ttu-id="63d96-119">För att förenkla överföring av data till och från MongoDB till vårt API kan använder vi Mongoose.</span><span class="sxs-lookup"><span data-stu-id="63d96-119">To simplify the transfer of data in and out of MongoDB to our API, we will use Mongoose.</span></span> <span data-ttu-id="63d96-120">Mongoose är ett schemabaserat system för modellering av data.</span><span class="sxs-lookup"><span data-stu-id="63d96-120">Mongoose is a schema-based system for modeling data.</span></span> <span data-ttu-id="63d96-121">Vi använder det i vårt exempelprogram för att hålla våra datamodeller konsekventa över olika GET-, POST- och DELETE HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="63d96-121">We will be using it in our sample application to keep our data models consistent through the various GET, POST, and DELETE HTTP requests.</span></span>

    <span data-ttu-id="63d96-122">Kör följande kommando för att lägga till Mongoose som ett paket som ditt webbprogram ska använda.</span><span class="sxs-lookup"><span data-stu-id="63d96-122">Run the following command to add Mongoose as a package for your web application to use.</span></span>

      ```bash
      npm install mongoose
      ```

1. <span data-ttu-id="63d96-123">Installera **body-parsern** för att förbearbeta JSON-begärandedata för användning i vår Express-routning.</span><span class="sxs-lookup"><span data-stu-id="63d96-123">Install **body-parser** to pre-process JSON request data for use in our Express routing.</span></span>

    <span data-ttu-id="63d96-124">I serverdelen fungerar `body-parser` som ett mellanprogram mellan Node.js och Express för parsning av inkommande JSON-begärandedata.</span><span class="sxs-lookup"><span data-stu-id="63d96-124">On the back end, `body-parser` will serve as a middleware between Node.js and Express for parsing incoming JSON request data.</span></span>

    <span data-ttu-id="63d96-125">Kör följande kommando för att lägga till `body-parser` som ett paket som ditt webbprogram ska använda.</span><span class="sxs-lookup"><span data-stu-id="63d96-125">Run the following command to add `body-parser` as a package for your web application to use.</span></span>

      ```bash
      npm install body-parser
      ```

    > [!TIP]
    > <span data-ttu-id="63d96-126">När du installerar flera npm-paket kan du även inkludera allihop i ett enda kommando som detta.</span><span class="sxs-lookup"><span data-stu-id="63d96-126">When installing multiple npm packages, you can also include them all in a single command such as this.</span></span>
    > ```bash
    > npm install express mongoose body-parser
    > ```

1. <span data-ttu-id="63d96-127">Skapa datamodellserverdelen för bokwebbprogrammet med hjälp av Mongoose.</span><span class="sxs-lookup"><span data-stu-id="63d96-127">Create the data model back end for your books web application using Mongoose.</span></span>

    1. <span data-ttu-id="63d96-128">I mappen **app** i mappen för **Books**-programmet skapar du en ny JavaScript-fil som heter **model.js** som ska innehåll din boks Mongoose-baserade datamodell.</span><span class="sxs-lookup"><span data-stu-id="63d96-128">In the **app** folder within your **Books** application folder, create a new JavaScript file called **model.js** to contain your book's Mongoose-based data model.</span></span>

        ```bash
        nano ~/Books/app/model.js
        ```

    1. <span data-ttu-id="63d96-129">Klistra in följande kod i den här nya filen för att skapa vårt bokschema med hjälp av Mongoose.</span><span class="sxs-lookup"><span data-stu-id="63d96-129">Paste the following code into this new file to create our book schema using Mongoose.</span></span>

        ```javascript
        var mongoose = require('mongoose');
        var dbHost = 'mongodb://localhost:27017/Books';
        mongoose.connect(dbHost,  { useNewUrlParser: true } );
        mongoose.connection;
        mongoose.set('debug', true);
        var bookSchema = mongoose.Schema( {
            name: String,
            isbn: {type: String, index: true},
            author: String,
            pages: Number
        });
        var Book = mongoose.model('Book', bookSchema);
        module.exports = Book // mongoose.model('Book', bookSchema);
        ```

        > [!TIP]
        > <span data-ttu-id="63d96-130">Om du vill spara den aktuella filen i **nano** måste du trycka på **Ctrl**+**O**.</span><span class="sxs-lookup"><span data-stu-id="63d96-130">To save the current file in **nano**, you need to press **Ctrl**+**O**.</span></span> <span data-ttu-id="63d96-131">Om du vill avsluta **nano** måste du trycka på **Ctrl**+**X**.</span><span class="sxs-lookup"><span data-stu-id="63d96-131">To exit **nano**, you need to press **Ctrl**+**X**.</span></span>

        <span data-ttu-id="63d96-132">Den här koden ansluter till en databas som heter "Books" på den lokala virtuella datorns MongoDB-server.</span><span class="sxs-lookup"><span data-stu-id="63d96-132">This code is connecting to a database called "Books" on the local VM's MongoDB server.</span></span> <span data-ttu-id="63d96-133">Den skapar sedan ett databasdokument som kallas "Book" med det schema som definieras av variabeln `bookSchema`.</span><span class="sxs-lookup"><span data-stu-id="63d96-133">It then creates a database document called "Book" with the schema defined by the `bookSchema` variable.</span></span>

1. <span data-ttu-id="63d96-134">Skapa Express-vägar för att programmet ska hantera olika HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="63d96-134">Create the Express routes for the application to handle the various HTTP requests.</span></span>

    1. <span data-ttu-id="63d96-135">I mappen **app** skapar du en ny JavaScript-fil som heter **routes.js**.</span><span class="sxs-lookup"><span data-stu-id="63d96-135">Within the **app** folder, create a new JavaScript file called **routes.js**.</span></span>

        ```bash
        nano ~/Books/app/routes.js
        ```

    1. <span data-ttu-id="63d96-136">Klistra in följande kod i den här nya filen för att upprätta vägarna med hjälp av Express.</span><span class="sxs-lookup"><span data-stu-id="63d96-136">Paste the following code into this new file to establish the routes using Express.</span></span>

        ```javascript
        var path = require('path');
        var Book = require('./model');
        var routes = function(app) {
            app.get('/book', function(req, res) {
                Book.find({}, function(err, result) {
                    if ( err ) throw err;
                    res.json(result);
                });
            });
            app.post('/book', function(req, res) {
                var book = new Book( {
                    name:req.body.name,
                    isbn:req.body.isbn,
                    author:req.body.author,
                    pages:req.body.pages
                });
                book.save(function(err, result) {
                    if ( err ) throw err;
                    res.json( {
                        message:"Successfully added book",
                        book:result
                    });
                });
            });
            app.delete("/book/:isbn", function(req, res) {
                Book.findOneAndRemove(req.query, function(err, result) {
                    if ( err ) throw err;
                    res.json( {
                        message: "Successfully deleted the book",
                        book: result
                    });
                });
            });
            app.get('*', function(req, res) {
                res.sendFile(path.join(__dirname + '/public', 'index.html'));
            });
        };
        module.exports = routes;
        ```

        <span data-ttu-id="63d96-137">Den här koden skapar fyra vägar för programmet.</span><span class="sxs-lookup"><span data-stu-id="63d96-137">This code will create four routes for our application.</span></span> <span data-ttu-id="63d96-138">De första tre anger vad som görs när någon skickar en API GET-, POST- eller DELETE-begäran till `/book`-resursen.</span><span class="sxs-lookup"><span data-stu-id="63d96-138">The first three specify what to do when someone sends an API GET, POST, or DELETE request to the `/book` resource.</span></span> <span data-ttu-id="63d96-139">Den sista är en generell routning för att skicka den som skickar begäran till indexsidan.</span><span class="sxs-lookup"><span data-stu-id="63d96-139">The last one is a catch-all route to send the requester to the index page.</span></span>

        <span data-ttu-id="63d96-140">Express kan skicka HTTP-svar direkt i koden för routningshantering eller skicka statiskt innehåll från filer.</span><span class="sxs-lookup"><span data-stu-id="63d96-140">Express can serve up HTTP responses directly in the route handling code, or it can serve up static content from files.</span></span> <span data-ttu-id="63d96-141">Vi gör både och i det här exempelwebbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="63d96-141">We are doing both in this sample web application.</span></span> <span data-ttu-id="63d96-142">Vi svarar med JSON-data för book-API-begäranden och med HTML-data direkt från filen index.html.</span><span class="sxs-lookup"><span data-stu-id="63d96-142">We respond with JSON data for book API requests and with HTML data direct from the index.html file.</span></span>

1. <span data-ttu-id="63d96-143">Skapa **server.js**-fil i mappen **Books** för att konfigurera Node.js-värdtjänsten (med hjälp av ExpressRoute-anslutningar).</span><span class="sxs-lookup"><span data-stu-id="63d96-143">Create a **server.js** file in the **Books** folder to configure the Node.js hosting (using the Express routes).</span></span>

    1. <span data-ttu-id="63d96-144">När du är tillbaka i programrotmappen **Books** skapar du en ny JavaScript-fil som heter **server.js**.</span><span class="sxs-lookup"><span data-stu-id="63d96-144">Back in the application root **Books** folder, create a new JavaScript file called **server.js**.</span></span>

        ```bash
        nano ~/Books/server.js
        ```

    1. <span data-ttu-id="63d96-145">Klistra in följande kod i den här nya filen för att konfigurera ditt webbprogram och börja lyssna på standardporten för HTTP.</span><span class="sxs-lookup"><span data-stu-id="63d96-145">Paste the following code into this new file to configure your web application and start listening to the default HTTP port.</span></span>

        ```javascript
        var express = require('express');
        var bodyParser = require('body-parser');
        var app = express();
        app.use(express.static(__dirname + '/public'));
        app.use(bodyParser.json());
        require('./app/routes')(app);
        app.set('port', 80);
        app.listen(app.get('port'), function() {
            console.log('Server up: http://localhost:' + app.get('port'));
        });
        ```

        <span data-ttu-id="63d96-146">Den här koden skapar själva webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="63d96-146">This code creates the web application itself.</span></span> <span data-ttu-id="63d96-147">Den skickar statiska filer från en mapp med namnet **public** (som skapas härnäst) och använder de vägar som definierades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="63d96-147">It will serve static files from a folder named **public** (created next) and will use the routes defined in the previous step.</span></span>

1. <span data-ttu-id="63d96-148">Skapa HTML för klientsidan och JavaScript-programmet för klientsidan.</span><span class="sxs-lookup"><span data-stu-id="63d96-148">Create the front-end HTML and client-side JavaScript application.</span></span>

    1. <span data-ttu-id="63d96-149">I innehållsmappen **public** skapar du en ny JavaScript-fil som heter **script.js**.</span><span class="sxs-lookup"><span data-stu-id="63d96-149">Within the **public** content folder, create a new JavaScript file called **script.js**.</span></span>

        ```bash
        nano ~/Books/public/script.js
        ```

    1. <span data-ttu-id="63d96-150">Klistra in följande kod i den här nya filen för att konfigurera webbprogrammet på klientsidan att hantera kommunikationen med din webbserver.</span><span class="sxs-lookup"><span data-stu-id="63d96-150">Paste the following code into this new file to configure your client-side web application to handle communicating with your web server.</span></span>

        ```javascript
        var app = angular.module('myApp', []);
        app.controller('myCtrl', function($scope, $http) {
            var getData = function() {
                return $http( {
                    method: 'GET',
                    url: '/book'
                }).then(function successCallback(response) {
                    $scope.books = response.data;
                }, function errorCallback(response) {
                    console.log('Error: ' + response);
                });
            };
            getData();
            $scope.del_book = function(book) {
                $http( {
                    method: 'DELETE',
                    url: '/book/:isbn',
                    params: {'isbn': book.isbn}
                }).then(function successCallback(response) {
                    console.log(response);
                    return getData();
                }, function errorCallback(response) {
                    console.log('Error: ' + response);
                });
            };
            $scope.add_book = function() {
                var body = '{ "name": "' + $scope.Name +
                '", "isbn": "' + $scope.Isbn +
                '", "author": "' + $scope.Author +
                '", "pages": "' + $scope.Pages + '" }';
                $http({
                    method: 'POST',
                    url: '/book',
                    data: body
                }).then(function successCallback(response) {
                    console.log(response);
                    return getData();
                }, function errorCallback(response) {
                    console.log('Error: ' + response);
                });
            };
        });
        ```

        <span data-ttu-id="63d96-151">Den här AngularJS-koden för klientsidan skapar det nya Angular-programmet `myApp` som innehåller styrenheten `myCtrl`.</span><span class="sxs-lookup"><span data-stu-id="63d96-151">This client-side AngularJS code creates a new angular application `myApp` containing one controller `myCtrl`.</span></span> <span data-ttu-id="63d96-152">När programmet körs i användarens webbläsare utfärdar det en HTTP GET-begäran för att hämta listan med böcker i databasen.</span><span class="sxs-lookup"><span data-stu-id="63d96-152">When the application is run in the viewer's browser, it will issue an HTTP GET request to retrieve the list of books in the database.</span></span>

    1. <span data-ttu-id="63d96-153">I innehållsmappen **public** skapar du en ny HTML-fil som heter **index.html**.</span><span class="sxs-lookup"><span data-stu-id="63d96-153">Also within the **public** content folder, create a new HTML file called **index.html**.</span></span>

        ```bash
        nano ~/Books/public/index.html
        ```

    1. <span data-ttu-id="63d96-154">Klistra in följande kod i den här nya filen för att konfigurera webbprogrammets HTML-användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="63d96-154">Paste the following markup into this new file to set up your web application's HTML user interface.</span></span>

        ```html
        <!doctype html>
        <html ng-app="myApp" ng-controller="myCtrl">
        <head>
            <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.2/angular.min.js"></script>
            <script src="script.js"></script>
        </head>
        <body>
            <div>
            <table>
                <tr>
                <td>Name:</td>
                <td><input type="text" ng-model="Name"></td>
                </tr>
                <tr>
                <td>Isbn:</td>
                <td><input type="text" ng-model="Isbn"></td>
                </tr>
                <tr>
                <td>Author:</td>
                <td><input type="text" ng-model="Author"></td>
                </tr>
                <tr>
                <td>Pages:</td>
                <td><input type="number" ng-model="Pages"></td>
                </tr>
            </table>
            <button ng-click="add_book()">Add</button>
            </div>
            <hr>
            <div>
            <table>
                <tr>
                <th>Name</th>
                <th>Isbn</th>
                <th>Author</th>
                <th>Pages</th>
                </tr>
                <tr ng-repeat="book in books">
                <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
                <td>{{book.name}}</td>
                <td>{{book.isbn}}</td>
                <td>{{book.author}}</td>
                <td>{{book.pages}}</td>
                </tr>
            </table>
            </div>
        </body>
        </html>
        ```

        <span data-ttu-id="63d96-155">Den här koden skapar ett enkelt HTML-formulär med fyra fält för att skicka nya bokdata och en tabell för att visa alla böcker som lagras i databasen.</span><span class="sxs-lookup"><span data-stu-id="63d96-155">This code will create a simple HTML form with four fields to submit new book data and a table to display all the books already stored in the database.</span></span> <span data-ttu-id="63d96-156">De olika `ng-`-HTML-attributen kommer att ställa in AngularJS-koden till användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="63d96-156">The various `ng-` HTML attributes will wire up the AngularJS code to the UI.</span></span>

1. <span data-ttu-id="63d96-157">Testa det fullständiga bokwebbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="63d96-157">Test the full books web application.</span></span>

    1. <span data-ttu-id="63d96-158">Starta programmet med Node.js med hjälp av följande kommando.</span><span class="sxs-lookup"><span data-stu-id="63d96-158">Start the application with Node.js with the following command.</span></span>

        ```bash
        sudo node ~/Books/server.js
        ```

        <span data-ttu-id="63d96-159">Detta startar serverdelen av programmet, som sedan startar lyssnar på port 80 för inkommande HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="63d96-159">This will start the back end of our application, which will then start listening on port 80 for incoming HTTP requests.</span></span>

    1. <span data-ttu-id="63d96-160">Testa programmets funktionalitet.</span><span class="sxs-lookup"><span data-stu-id="63d96-160">Test the application functionality.</span></span>

        <span data-ttu-id="63d96-161">Öppna valfri webbläsare och navigera till den offentliga IP-adressen för din virtuella Azure-dator som URL:en.</span><span class="sxs-lookup"><span data-stu-id="63d96-161">Open your preferred browser, and navigate to the public IP address of your Azure VM as the URL.</span></span>

        ```bash
        http://<vm-public-ip>
        ```

        <span data-ttu-id="63d96-162">Om allt är som det ska bör du se en skärm som ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="63d96-162">If everything is in order, you should see a screen similar to this:</span></span>

        <span data-ttu-id="63d96-163">Följande skärmbild visar användargränssnittet för att skicka in bokinformation för lagring i MongoDB-databasen.</span><span class="sxs-lookup"><span data-stu-id="63d96-163">The following screenshot displays the user interface to submit book details for storage in the MongoDB database.</span></span>


        ![Skärmbild av en webbläsare som visar datainmatningsforumläret för att lägga till en bok.](../media-draft/10-book-page.png)

    <span data-ttu-id="63d96-165">Du bör nu kunna skicka böcker som ska sparas till MongoDB-databasen.</span><span class="sxs-lookup"><span data-stu-id="63d96-165">You should now be able to submit books to save to the MongoDB database.</span></span> <span data-ttu-id="63d96-166">Dessutom kan du se en fullständig lista över böcker som lästs in från databasen.</span><span class="sxs-lookup"><span data-stu-id="63d96-166">As well, you can see the full list of books loaded from the database.</span></span>