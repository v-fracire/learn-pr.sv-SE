<span data-ttu-id="91800-101">I den här kursdelen skapar du ett enkelt AngularJS-program som hanteras i Node.js och använder Express för routning.</span><span class="sxs-lookup"><span data-stu-id="91800-101">In this unit, you're going to create a simple AngularJS application hosted in Node.js and use Express for routing.</span></span> <span data-ttu-id="91800-102">I serverdelen fungerar MongoDB som datalager.</span><span class="sxs-lookup"><span data-stu-id="91800-102">On the back end, MongoDB will serve as your data store.</span></span> <span data-ttu-id="91800-103">Programmet är en bokdatabas där du kommer att kunna visa, lägga till och ta bort böcker.</span><span class="sxs-lookup"><span data-stu-id="91800-103">The application is a book database, where you will be able to list, add, and delete books.</span></span>

> [!Important]
> <span data-ttu-id="91800-104">Det här är ett enkelt program.</span><span class="sxs-lookup"><span data-stu-id="91800-104">This is a simple application.</span></span> <span data-ttu-id="91800-105">Dess syfte är att testa den nyinstallerade MEAN-stacken.</span><span class="sxs-lookup"><span data-stu-id="91800-105">Its purpose is to test the newly installed MEAN stack.</span></span> <span data-ttu-id="91800-106">Programmet är inte tillräckligt säkert eller redo för produktionsanvändning.</span><span class="sxs-lookup"><span data-stu-id="91800-106">This application is not sufficiently secure or ready for production use.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="91800-107">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="91800-107">Create the application</span></span>

<span data-ttu-id="91800-108">Först ska vi skapa kod, skript och HTML-filer för vårt program.</span><span class="sxs-lookup"><span data-stu-id="91800-108">First, we're going to create the code, script and HTML files for our application.</span></span> <span data-ttu-id="91800-109">Vi ska göra detta i Cloud Shell-redigeraren och kopierar sedan filerna till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="91800-109">We'll do this in the Cloud Shell editor then copy the files to the VM.</span></span>

1. <span data-ttu-id="91800-110">I Cloud Shell, om du är fortfarande ansluten med SSH till den virtuella datorn, använder du `exit` för att återgå till Cloud Shell-filsystemet.</span><span class="sxs-lookup"><span data-stu-id="91800-110">In Cloud Shell, if you are still SSHed into your VM, use `exit` to return to the Cloud Shell filesystem.</span></span>

    ```bash
    exit
    ```

1. <span data-ttu-id="91800-111">Skapa mappar och filer för ditt program och öppna dem i Cloud Shell-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="91800-111">Create the folders and files for your application and open them in the Cloud Shell editor.</span></span>

    ```bash
    cd ~
    mkdir Books
    mkdir Books/app
    mkdir Books/public
    touch Books/app/model.js
    touch Books/app/routes.js
    touch Books/server.js
    touch Books/public/script.js
    touch Books/public/index.html
    code Books
    ```

    <span data-ttu-id="91800-112">Detta skapar en mapp med namnet ”Books” (böcker) som ska innehålla projektets app och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="91800-112">This creates a folder called "Books" to contain your project's app and its dependencies.</span></span> <span data-ttu-id="91800-113">I den mappen skapade du en ”app”-mapp som ska innehålla alla programresurser och skript.</span><span class="sxs-lookup"><span data-stu-id="91800-113">Within that folder, you created an "app" folder to contain all your application resources and scripts.</span></span> <span data-ttu-id="91800-114">Slutligen skapar vi en ”public” (offentlig) mapp som ska lagra alla filer på klientsidan som skickas direkt till lämpliga HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="91800-114">Finally, we will also create a "public" folder to hold all of the client-side files that will be served up directly to appropriate HTTP requests.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="91800-115">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="91800-115">Create the application</span></span>

1. <span data-ttu-id="91800-116">Skapa datamodellen Mongoose.</span><span class="sxs-lookup"><span data-stu-id="91800-116">Create the Mongoose data model.</span></span> <span data-ttu-id="91800-117">I redigeraren, öppna `app/model.js` och klistra in följande kod.</span><span class="sxs-lookup"><span data-stu-id="91800-117">In the editor, open `app/model.js` and paste in the following code.</span></span>

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

    > [!IMPORTANT]
    > <span data-ttu-id="91800-118">När du klistrar in eller ändrar koden i en fil i redigeraren, se till att spara efteråt via menyn ”...” eller använd snabbtangenten (<kbd>Ctrl + S</kbd> på Windows och Linux, <kbd>Cmd + S</kbd> på macOS).</span><span class="sxs-lookup"><span data-stu-id="91800-118">Whenever you paste or change code into a file in the editor, make sure to save afterwards using the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

    <span data-ttu-id="91800-119">Den här koden ansluter till en databas som heter "Books" på den lokala virtuella datorns MongoDB-server.</span><span class="sxs-lookup"><span data-stu-id="91800-119">This code is connecting to a database called "Books" on the local VM's MongoDB server.</span></span> <span data-ttu-id="91800-120">Den skapar sedan ett databasdokument som kallas "Book" med det schema som definieras av variabeln `bookSchema`.</span><span class="sxs-lookup"><span data-stu-id="91800-120">It then creates a database document called "Book" with the schema defined by the `bookSchema` variable.</span></span>

2. <span data-ttu-id="91800-121">Skapa expressvägar som hanterar HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="91800-121">Create the Express routes that will handle HTTP requests.</span></span> <span data-ttu-id="91800-122">I redigeraren, öppna `app/routes.js` och klistra in följande kod.</span><span class="sxs-lookup"><span data-stu-id="91800-122">Open `app/routes.js` in the editor and paste in the following code.</span></span>

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

    <span data-ttu-id="91800-123">Den här koden skapar fyra vägar för programmet.</span><span class="sxs-lookup"><span data-stu-id="91800-123">This code will create four routes for our application.</span></span> <span data-ttu-id="91800-124">De första tre anger vad som görs när någon skickar en API GET-, POST- eller DELETE-begäran till `/book`-resursen.</span><span class="sxs-lookup"><span data-stu-id="91800-124">The first three specify what to do when someone sends an API GET, POST, or DELETE request to the `/book` resource.</span></span> <span data-ttu-id="91800-125">Den sista är en generell routning för att skicka den som skickar begäran till indexsidan.</span><span class="sxs-lookup"><span data-stu-id="91800-125">The last one is a catch-all route to send the requester to the index page.</span></span>

    <span data-ttu-id="91800-126">Express kan skicka HTTP-svar direkt i koden för routningshantering eller skicka statiskt innehåll från filer.</span><span class="sxs-lookup"><span data-stu-id="91800-126">Express can serve up HTTP responses directly in the route handling code, or it can serve up static content from files.</span></span> <span data-ttu-id="91800-127">Vi gör både och i det här exempelwebbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="91800-127">We are doing both in this sample web application.</span></span> <span data-ttu-id="91800-128">Vi svarar med JSON-data för book-API-begäranden och med HTML-data direkt från filen index.html.</span><span class="sxs-lookup"><span data-stu-id="91800-128">We respond with JSON data for book API requests and with HTML data direct from the index.html file.</span></span>

3. <span data-ttu-id="91800-129">Skapa expresservern som värd för programmet.</span><span class="sxs-lookup"><span data-stu-id="91800-129">Create the Express server to host the application.</span></span> <span data-ttu-id="91800-130">I redigeraren, öppna `server.js` och klistra in följande kod:</span><span class="sxs-lookup"><span data-stu-id="91800-130">Open `server.js` in the editor and paste in the following code:</span></span>

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

    <span data-ttu-id="91800-131">Den här koden skapar själva webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="91800-131">This code creates the web application itself.</span></span> <span data-ttu-id="91800-132">Den skickar statiska filer från en mapp med namnet **public** (som skapas härnäst) och använder de vägar som definierades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="91800-132">It will serve static files from a folder named **public** (created next) and will use the routes defined in the previous step.</span></span>

4. <span data-ttu-id="91800-133">Skapa JavaScript-programmet för klientsidan.</span><span class="sxs-lookup"><span data-stu-id="91800-133">Create the client-side JavaScript application.</span></span> <span data-ttu-id="91800-134">I redigeraren, öppna `public/script.js` och klistra in följande kod:</span><span class="sxs-lookup"><span data-stu-id="91800-134">Open `public/script.js` in the editor and paste in this code:</span></span>

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

    <span data-ttu-id="91800-135">Den här AngularJS-koden för klientsidan skapar det nya Angular-programmet `myApp` som innehåller styrenheten `myCtrl`.</span><span class="sxs-lookup"><span data-stu-id="91800-135">This client-side AngularJS code creates a new angular application `myApp` containing one controller `myCtrl`.</span></span> <span data-ttu-id="91800-136">När programmet körs i användarens webbläsare utfärdar det en HTTP GET-begäran för att hämta listan med böcker i databasen.</span><span class="sxs-lookup"><span data-stu-id="91800-136">When the application is run in the viewer's browser, it will issue an HTTP GET request to retrieve the list of books in the database.</span></span>

5. <span data-ttu-id="91800-137">Skapa användargränssnittet för appen.</span><span class="sxs-lookup"><span data-stu-id="91800-137">Create the user interface for the app.</span></span> <span data-ttu-id="91800-138">Öppna `public/index.html` i redigeraren och klistra in den här koden:</span><span class="sxs-lookup"><span data-stu-id="91800-138">Open `public/index.html` in the editor and paste in this code:</span></span>

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

    <span data-ttu-id="91800-139">Den här koden skapar ett enkelt HTML-formulär med fyra fält för att skicka nya bokdata och en tabell för att visa alla böcker som lagras i databasen.</span><span class="sxs-lookup"><span data-stu-id="91800-139">This code will create a simple HTML form with four fields to submit new book data and a table to display all the books already stored in the database.</span></span> <span data-ttu-id="91800-140">De olika `ng-`-HTML-attributen kommer att ställa in AngularJS-koden till användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="91800-140">The various `ng-` HTML attributes will wire up the AngularJS code to the UI.</span></span>

6. <span data-ttu-id="91800-141">Vi är klara med att redigera filerna.</span><span class="sxs-lookup"><span data-stu-id="91800-141">We're done editing files.</span></span> <span data-ttu-id="91800-142">Kontrollera att du har sparat alla och kör sedan följande kommando för att kopiera dem till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="91800-142">Make sure you have saved all of them, then run the following command to copy them to the VM.</span></span> <span data-ttu-id="91800-143">När du uppmanas till det anger du ditt lösenord.</span><span class="sxs-lookup"><span data-stu-id="91800-143">Enter your password when prompted.</span></span>

    ```bash
    scp -r ~/Books <vm-admin-username>@<vm-public-ip>:~/Books
    ```

## <a name="install-node-packages"></a><span data-ttu-id="91800-144">Installera Node-paket</span><span class="sxs-lookup"><span data-stu-id="91800-144">Install Node packages</span></span>

1. <span data-ttu-id="91800-145">SSH tillbaka till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="91800-145">SSH back into your VM.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. <span data-ttu-id="91800-146">Ändra sökvägen till katalogen `Books`.</span><span class="sxs-lookup"><span data-stu-id="91800-146">Change directories to the `Books` directory.</span></span>

    ```bash
    cd ~/Books
    ```

1. <span data-ttu-id="91800-147">Installera **Express** för att hantera routning av dina HTTP-begäranden och bestämma vilket innehåll som ska returneras till användare av ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="91800-147">Install **Express** to handle routing of your HTTP requests, to decide what content to return to a user of your web application.</span></span>

    <span data-ttu-id="91800-148">Kör följande kommando för att lägga till Express som ett paket som ditt webbprogram ska använda.</span><span class="sxs-lookup"><span data-stu-id="91800-148">Run the following command to add Express as a package for your web application to use.</span></span>

    ```bash
    npm install express
    ```

1. <span data-ttu-id="91800-149">Installera **Mongoose** för att hjälpa till med vidarebefordran av dina bokdata mellan MongoDB och HTTP-begäranderoutning.</span><span class="sxs-lookup"><span data-stu-id="91800-149">Install **Mongoose** to help relay your book data between MongoDB and the HTTP request routing.</span></span>

    <span data-ttu-id="91800-150">Det körs frågor mot bokinformationen via REST API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="91800-150">The book information will be queried via REST API requests.</span></span> <span data-ttu-id="91800-151">För att förenkla överföring av data till och från MongoDB till vårt API kan använder vi Mongoose.</span><span class="sxs-lookup"><span data-stu-id="91800-151">To simplify the transfer of data in and out of MongoDB to our API, we will use Mongoose.</span></span> <span data-ttu-id="91800-152">Mongoose är ett schemabaserat system för modellering av data.</span><span class="sxs-lookup"><span data-stu-id="91800-152">Mongoose is a schema-based system for modeling data.</span></span> <span data-ttu-id="91800-153">Vi använder det i vårt exempelprogram för att hålla våra datamodeller konsekventa över olika GET-, POST- och DELETE HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="91800-153">We will be using it in our sample application to keep our data models consistent through the various GET, POST, and DELETE HTTP requests.</span></span>

    <span data-ttu-id="91800-154">Kör följande kommando för att lägga till Mongoose som ett paket som ditt webbprogram ska använda.</span><span class="sxs-lookup"><span data-stu-id="91800-154">Run the following command to add Mongoose as a package for your web application to use.</span></span>

      ```bash
      npm install mongoose
      ```

1. <span data-ttu-id="91800-155">Installera **body-parsern** för att förbearbeta JSON-begärandedata för användning i vår Express-routning.</span><span class="sxs-lookup"><span data-stu-id="91800-155">Install **body-parser** to pre-process JSON request data for use in our Express routing.</span></span>

    <span data-ttu-id="91800-156">I serverdelen fungerar `body-parser` som ett mellanprogram mellan Node.js och Express för parsning av inkommande JSON-begärandedata.</span><span class="sxs-lookup"><span data-stu-id="91800-156">On the back end, `body-parser` will serve as a middleware between Node.js and Express for parsing incoming JSON request data.</span></span>

    <span data-ttu-id="91800-157">Kör följande kommando för att lägga till `body-parser` som ett paket som ditt webbprogram ska använda.</span><span class="sxs-lookup"><span data-stu-id="91800-157">Run the following command to add `body-parser` as a package for your web application to use.</span></span>

      ```bash
      npm install body-parser
      ```

    > [!TIP]
    > <span data-ttu-id="91800-158">När du installerar flera npm-paket kan du inkludera allihop i ett enda kommando som t.ex. detta:</span><span class="sxs-lookup"><span data-stu-id="91800-158">When installing multiple npm packages, you can include them all in a single command such as this:</span></span>
    >
    > ```bash
    > npm install express mongoose body-parser
    > ```

## <a name="test-the-application"></a><span data-ttu-id="91800-159">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="91800-159">Test the application</span></span>

1. <span data-ttu-id="91800-160">Starta programmet med Node.js med hjälp av följande kommando.</span><span class="sxs-lookup"><span data-stu-id="91800-160">Start the application with Node.js with the following command.</span></span>

    ```bash
    sudo node server.js
    ```

    <span data-ttu-id="91800-161">Detta startar serverdelen av programmet, som sedan startar lyssnar på port 80 för inkommande HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="91800-161">This will start the back end of our application, which will then start listening on port 80 for incoming HTTP requests.</span></span>

1. <span data-ttu-id="91800-162">Testa programmets funktionalitet.</span><span class="sxs-lookup"><span data-stu-id="91800-162">Test the application functionality.</span></span>

    <span data-ttu-id="91800-163">Öppna valfri webbläsare och navigera till den offentliga IP-adressen för din virtuella Azure-dator som webbadressen ([http://X.X.X.X](http://X.X.X.X)).</span><span class="sxs-lookup"><span data-stu-id="91800-163">Open your preferred browser, and navigate to the public IP address of your Azure VM as the URL ([http://X.X.X.X](http://X.X.X.X)).</span></span>

    <span data-ttu-id="91800-164">Om allt är som det ska bör du se en skärm som ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="91800-164">If everything is in order, you should see a screen similar to this:</span></span>

    <span data-ttu-id="91800-165">Följande skärmbild visar användargränssnittet för att skicka in bokinformation för lagring i MongoDB-databasen.</span><span class="sxs-lookup"><span data-stu-id="91800-165">The following screenshot displays the user interface to submit book details for storage in the MongoDB database.</span></span>

    ![Skärmbild av en webbläsare som visar datainmatningsforumläret för att lägga till en bok.](../media/10-book-page.png)

    <span data-ttu-id="91800-167">Du bör nu kunna skicka böcker som ska sparas till MongoDB-databasen.</span><span class="sxs-lookup"><span data-stu-id="91800-167">You should now be able to submit books to save to the MongoDB database.</span></span> <span data-ttu-id="91800-168">Dessutom kan du se en fullständig lista över böcker som lästs in från databasen.</span><span class="sxs-lookup"><span data-stu-id="91800-168">As well, you can see the full list of books loaded from the database.</span></span>