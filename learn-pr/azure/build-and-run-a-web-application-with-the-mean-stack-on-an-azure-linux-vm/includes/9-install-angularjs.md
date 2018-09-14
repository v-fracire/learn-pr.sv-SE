AngularJS är ett ramverk för att skapa tydliga och koncisa dynamiska webbprogram. Du använder HTML som språk för innehållsmallen men utökar din HTML med databindningssyntax. Med AngularJS för databindning och beroendeinmatning slipper du mycket av den kod som du annars skulle behöva skriva för att hantera uppdateringar av innehållet. Uppdatering av dina användares innehåll sker i webbläsaren, vilket gör att AngularJS kan kombineras med valfri värdteknik för serversidan.

## <a name="angularjs-information"></a>Information om Angular.js

Eftersom AngularJS är ett JavaScript-ramverk för klientdelen behöver det bara göras tillgängligt för de klienter som kommer åt programmet. Detta kan göras på flera olika sätt.

- Referera till AngularJS via ett nätverk för innehållsleverans (CDN).
- Skicka till AngularJS från ditt värdbaserade innehåll som ett Node.js-paket med hjälp av npm som en pakethanterare.
- Skicka till AngularJS från ditt värdbaserade innehåll som ett Bower-paket.

Vi kommer att använda AngularJS direkt från ett CDN för den här modulen för enkelhetens skull. Det innebär att skriftfilberoendet skickas till CDN i stället för att hanteras direkt i serverinnehållet. Det ger också bättre nedladdningshastighet baserat på hastigheten och den geografiska spridningen för den CDN som används för en viss resurs.

> [!NOTE]
> AngularJS är föregångare till Angular, som var en fullständig omskrivning av webbprogramplattformen. Många av begreppen i de två versionerna liknar varandra, men de är nu separata projekt. Angular har versioner som är 2.x.y och högre, medan AngularJS har versioner som slutar på 1.x.y. AngularJS används fortfarande ofta för webbprogramscenarier.

## <a name="how-to-install-angularjs-via-cdn"></a>Så här installerar du AngularJS via CDN

Du _installerar_ egentligen inte AngularJS. Du lägger bara till en referens till JavaScript-filen via en skripttagg på HTML-sidan. Eftersom AngularJS behövs för alla funktioner i vårt webbprogram tar vi med det i `<head>`-taggen på det här sättet (jämfört med senare inläsning av JavaScript-filer i slutet av `<body>`-taggen).

```html
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.2/angular.min.js"></script>
```
