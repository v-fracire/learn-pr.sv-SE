AngularJS är ett ramverk för att skapa tydliga och koncisa dynamiska webbprogram. Du använder HTML som språk för innehållsmallen men utökar din HTML med databindningssyntax. Med AngularJS för databindning och beroendeinmatning slipper du mycket av den kod som du annars skulle behöva skriva för att hantera uppdateringar av innehållet. Uppdatering av dina användares innehåll sker i webbläsaren, vilket gör att AngularJS kan kombineras med valfri värdteknik för serversidan.

## <a name="what-must-be-installed"></a>Vad måste finnas installerat?

Eftersom AngularJS är ett JavaScript-ramverk för klientdelen behöver det bara göras tillgängligt för de klienter som kommer åt programmet. Detta kan göras på flera olika sätt.

1. Referera till AngularJS via ett nätverk för innehållsleverans (CDN).
1. Skicka till AngularJS från ditt värdbaserade innehåll som ett Node.js-paket med hjälp av npm som en pakethanterare.
1. Skicka till AngularJS från ditt värdbaserade innehåll som ett Bower-paket.

Vi kommer att använda AngularJS direkt från ett CDN för den här modulen för enkelhetens skull. Det här gör att skriptfilberoendet skickas direkt till CDN i stället för att det hanteras på vårt serverinnehåll direkt, vilket även kan förbättra nedladdningshastigheter baserat på hastigheten och den geografiska fördelningen av den CDN som används för en viss resurs.

> [!Note]
> AngularJS är föregångare till Angular, som var en fullständig omskrivning av webbprogramplattformen. Många av begreppen i de två versionerna liknar varandra, men de är nu separata projekt. Angular har versioner som är 2.x.y och högre, medan AngularJS har versioner som slutar på 1.x.y. AngularJS används fortfarande ofta för webbprogramscenarier.

## <a name="how-to-install-angularjs-via-cdn"></a>Så här installerar du AngularJS via CDN

    You don't really _install_ AngularJS; you just add a reference to the JavaScript file via a script tag in you HTML page. Since AngularJS is critical to any functionality of our web application, we include it within the `<head>` tag like this (as compared to later loading of JavaScript files toward the end of the `<body>` tag).

    ```html
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.2/angular.min.js"></script>
    ```
