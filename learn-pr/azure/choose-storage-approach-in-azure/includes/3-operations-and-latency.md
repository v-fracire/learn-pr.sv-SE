När du har identifierat vilken typ av data det gäller (strukturerade, halvstrukturerade eller ostrukturerade) är nästa viktiga steg att avgöra hur data ska användas. I en onlinebutik vill kunderna till exempel snabbt komma åt produktdata och användare i verksamheten vill kunna köra komplexa analytiska frågor. När går igenom kraven med hänsyn till din dataklassificering kan du börja formulera en metod för datalagring.

Här går vi igenom några av de frågor du bör ställa när du avgör hur data ska hanteras.

## <a name="operations-and-latency"></a>Åtgärder och svarstider

Vilka åtgärder kommer primärt att utföras på de olika datatyperna och hur ser prestandakraven ut?

Ställ dig följande frågor:

* Kommer du att utföra enkla sökningar per ID?
* Behöver du köra frågor mot ett eller flera fält i databasen?
* Hur ofta kommer data skapas, uppdateras och tas bort?
* Behöver du kunna köra komplexa analytiska frågor?
* Hur snabbt måste de här åtgärderna gå att utföra?

Nu ska vi gå igenom var och en av datauppsättningarna med dessa frågor i åtanke och diskutera vad som behövs.

## <a name="product-catalog-data"></a>Data i produktkatalogen

När det gäller data i produktkatalogen för en onlinebutik så har användarnas behov högsta prioritet. Kunderna kommer att vilja köra frågor mot produktkatalogen för att exempelvis hitta alla herrskor, alla herrskor i rabatt och sedan alla herrskor i rabatt i en viss storlek. Därför behöver kunderna göra många läsåtgärder och köra frågor mot många olika fält i databasen.

Dessutom när kunderna beställer måste programmet kunna uppdatera produktkvantiteterna. De här uppdateringarna måste gå lika snabbt som läsåtgärderna så att användarna inte placerar artiklar i kundvagnen när de precis sålts slut. Detta resulterar inte bara i ett stort antal läsåtgärder, men kräver även ökade skrivåtgärder för produktkatalogdata. Var noga med att fastställa prioriteten för alla som använder databasen, inte bara de primära användarna.

## <a name="photos-and-videos"></a>Foton och videor

Det gäller andra krav för de foton och videor som visas på produktsidorna. De behöver snabb datahämtningstid så att de visas på webbplatsen samtidigt som produktkataloginformationen, men de behöver inte efterfrågas oberoende av varandra. Du kan i stället förlita dig på produktfrågorna och behöver bara ha video-ID:t eller webbadressen som en egenskap för produktdata. Foton och videoklipp behöver alltså endast hämtas per ID.

Dessutom kommer användare inte att göra uppdateringar av befintliga foton eller videoklipp. De kan dock lägga till nya foton för produktrecensioner. En användare kan till exempel ladda upp en bild av sig själv där de visar upp sina nya skor. Som medarbetare laddar du också upp och tar bort produktfoton från leverantören, men de här uppdateringarna behöver inte gå lika snabbt som andra uppdateringar av produktdata. 

Sammanfattningsvis behöver du bara köra frågor mot foton och videor per ID och returnera hela filen och data kommer att skapas och uppdateras mer sällan och med lägre prioritet.  

## <a name="business-data"></a>Affärsdata

För affärsdata görs all analys på historiska data. Eftersom inga ursprungsdata uppdateras baserat på analysen är affärsdata skrivskyddade. Användarna förväntar sig inte heller att komplexa analyser ska utföras omedelbart, så det är okej med en viss svarstid för resultaten. Dessutom lagras affärsdata i flera olika datauppsättningar eftersom användarna ska ha olika skrivåtkomst till varje datauppsättning. Affärsanalytiker kommer dock att kunna läsa från alla databaser.

## <a name="summary"></a>Sammanfattning

När du bestämmer dig för en lagringslösning ska du tänka igenom hur dina data kommer att användas. Hur ofta kommer dina data att användas? Är dina data skrivskyddade? Spelar svarstiderna någon roll? När du har svar på de här frågorna kan du bestämma vilken lagringslösning som passar bäst för dina data.