De flesta appar behöver en databas. MongoDB, M:et i MEAN-stacken, är en av de mest populära NoSQL-databaslösningarna – kostnadsfritt och med öppen källkod. NoSQL-databas kräver inte att data struktureras på ett fördefinierat sätt, som med en relationsdatabas som SQL Server eller MySQL.

MongoDB lagrar data i JSON-liknande dokument som inte kräver fasta datastrukturer. Vi interagerar sedan med MongoDB med hjälp av frågor och kommandon som skickas som JavaScript Object Notation (JSON).

## <a name="mongodb-versions"></a>MongoDB-versioner

Det finns två versioner av MongoDB:

- MongoDB Community Server
- MongoDB Enterprise Server

I den här modulen använder vi MongoDB Community Server, version 3.6 när detta skrivs, som webbappens datalager.

## <a name="how-to-install-mongodb"></a>Installera MongoDB

MongoDB kan installeras på Linux, macOS och Windows. Vi installerar det på vår Ubuntu Linux-dator för den här modulen men den rekommenderade pakethanteraren är olika beroende på operativsystem och distribution. Installationsprocessen för alla operativsystem är beskrivs utförligt i [installationsdokumentationen för MongoDB](https://docs.mongodb.com/manual/administration/install-community/).

Om du vill installera på vår virtuella Ubuntu-dator använder vi pakethanteraren **apt-get**.