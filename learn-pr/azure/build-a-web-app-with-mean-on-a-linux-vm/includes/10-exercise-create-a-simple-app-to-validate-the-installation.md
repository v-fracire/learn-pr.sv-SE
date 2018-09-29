I den här kursdelen skapar du ett enkelt AngularJS-program som hanteras i Node.js och använder Express för routning. I serverdelen fungerar MongoDB som datalager. Programmet är en bokdatabas där du kommer att kunna visa, lägga till och ta bort böcker.

> [!Important]
> Det här är ett enkelt program. Dess syfte är att testa den nyinstallerade MEAN-stacken. Programmet är inte tillräckligt säkert eller redo för produktionsanvändning.

## <a name="create-the-application"></a>Skapa programmet

Först ska vi skapa kod, skript och HTML-filer för vårt program. Vi ska göra detta i Cloud Shell-redigeraren och kopierar sedan filerna till den virtuella datorn.

1. I Cloud Shell, om du är fortfarande ansluten med SSH till den virtuella datorn, använder du `exit` för att återgå till Cloud Shell-filsystemet.

    ```bash
    exit
    ```

1. Skapa mappar och filer för ditt program och öppna dem i Cloud Shell-redigeraren.

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

    Detta skapar en mapp med namnet ”Books” (böcker) som ska innehålla projektets app och dess beroenden. I den mappen skapade du en ”app”-mapp som ska innehålla alla programresurser och skript. Slutligen skapar vi en ”public” (offentlig) mapp som ska lagra alla filer på klientsidan som skickas direkt till lämpliga HTTP-begäranden.

## <a name="create-the-application"></a>Skapa programmet

1. Skapa datamodellen Mongoose. I redigeraren, öppna `app/model.js` och klistra in följande kod.

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
    > När du klistrar in eller ändrar koden i en fil i redigeraren, se till att spara efteråt via menyn ”...” eller använd snabbtangenten (<kbd>Ctrl + S</kbd> på Windows och Linux, <kbd>Cmd + S</kbd> på macOS).

    Den här koden ansluter till en databas som heter "Books" på den lokala virtuella datorns MongoDB-server. Den skapar sedan ett databasdokument som kallas "Book" med det schema som definieras av variabeln `bookSchema`.

2. Skapa expressvägar som hanterar HTTP-begäranden. I redigeraren, öppna `app/routes.js` och klistra in följande kod.

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

    Den här koden skapar fyra vägar för programmet. De första tre anger vad som görs när någon skickar en API GET-, POST- eller DELETE-begäran till `/book`-resursen. Den sista är en generell routning för att skicka den som skickar begäran till indexsidan.

    Express kan skicka HTTP-svar direkt i koden för routningshantering eller skicka statiskt innehåll från filer. Vi gör både och i det här exempelwebbprogrammet. Vi svarar med JSON-data för book-API-begäranden och med HTML-data direkt från filen index.html.

3. Skapa expresservern som värd för programmet. I redigeraren, öppna `server.js` och klistra in följande kod:

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

    Den här koden skapar själva webbprogrammet. Den skickar statiska filer från en mapp med namnet **public** (som skapas härnäst) och använder de vägar som definierades i föregående steg.

4. Skapa JavaScript-programmet för klientsidan. I redigeraren, öppna `public/script.js` och klistra in följande kod:

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

    Den här AngularJS-koden för klientsidan skapar det nya Angular-programmet `myApp` som innehåller styrenheten `myCtrl`. När programmet körs i användarens webbläsare utfärdar det en HTTP GET-begäran för att hämta listan med böcker i databasen.

5. Skapa användargränssnittet för appen. Öppna `public/index.html` i redigeraren och klistra in den här koden:

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

    Den här koden skapar ett enkelt HTML-formulär med fyra fält för att skicka nya bokdata och en tabell för att visa alla böcker som lagras i databasen. De olika `ng-`-HTML-attributen kommer att ställa in AngularJS-koden till användargränssnittet.

6. Vi är klara med att redigera filerna. Kontrollera att du har sparat alla och kör sedan följande kommando för att kopiera dem till den virtuella datorn. När du uppmanas till det anger du ditt lösenord.

    ```bash
    scp -r ~/Books <vm-admin-username>@<vm-public-ip>:~/Books
    ```

## <a name="install-node-packages"></a>Installera Node-paket

1. SSH tillbaka till den virtuella datorn.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. Ändra sökvägen till katalogen `Books`.

    ```bash
    cd ~/Books
    ```

1. Installera **Express** för att hantera routning av dina HTTP-begäranden och bestämma vilket innehåll som ska returneras till användare av ditt webbprogram.

    Kör följande kommando för att lägga till Express som ett paket som ditt webbprogram ska använda.

    ```bash
    npm install express
    ```

1. Installera **Mongoose** för att hjälpa till med vidarebefordran av dina bokdata mellan MongoDB och HTTP-begäranderoutning.

    Det körs frågor mot bokinformationen via REST API-begäranden. För att förenkla överföring av data till och från MongoDB till vårt API kan använder vi Mongoose. Mongoose är ett schemabaserat system för modellering av data. Vi använder det i vårt exempelprogram för att hålla våra datamodeller konsekventa över olika GET-, POST- och DELETE HTTP-begäranden.

    Kör följande kommando för att lägga till Mongoose som ett paket som ditt webbprogram ska använda.

      ```bash
      npm install mongoose
      ```

1. Installera **body-parsern** för att förbearbeta JSON-begärandedata för användning i vår Express-routning.

    I serverdelen fungerar `body-parser` som ett mellanprogram mellan Node.js och Express för parsning av inkommande JSON-begärandedata.

    Kör följande kommando för att lägga till `body-parser` som ett paket som ditt webbprogram ska använda.

      ```bash
      npm install body-parser
      ```

    > [!TIP]
    > När du installerar flera npm-paket kan du inkludera allihop i ett enda kommando som t.ex. detta:
    >
    > ```bash
    > npm install express mongoose body-parser
    > ```

## <a name="test-the-application"></a>Testa programmet

1. Starta programmet med Node.js med hjälp av följande kommando.

    ```bash
    sudo node server.js
    ```

    Detta startar serverdelen av programmet, som sedan startar lyssnar på port 80 för inkommande HTTP-begäranden.

1. Testa programmets funktionalitet.

    Öppna valfri webbläsare och navigera till den offentliga IP-adressen för din virtuella Azure-dator som URL:en ([http://X.X.X.X](http://X.X.X.X)).

    Om allt är som det ska bör du se en skärm som ser ut ungefär så här:

    Följande skärmbild visar användargränssnittet för att skicka in bokinformation för lagring i MongoDB-databasen.

    ![Skärmbild av en webbläsare som visar datainmatningsforumläret för att lägga till en bok.](../media/10-book-page.png)

    Du bör nu kunna skicka böcker som ska sparas till MongoDB-databasen. Dessutom kan du se en fullständig lista över böcker som lästs in från databasen.