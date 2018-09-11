När du har identifierat vilken typ av data det gäller (strukturerade, halvstrukturerade eller ostrukturerade) är nästa viktiga steg att avgöra hur data ska användas. I en onlinebutik vill kunderna till exempel snabbt komma åt produktdata, och användare i verksamheten vill kunna köra komplexa analytiska frågor. När du börjar gå igenom kraven med hänsyn till din dataklassificering kan du börja formulera en metod för datalagring.

Här går vi igenom några av de frågor du bör ställa när du avgör hur data ska hanteras.

## <a name="operations-and-latency"></a>Åtgärder och svarstider

Vilka åtgärder kommer primärt att utföras på de olika datatyperna och hur ser prestandakraven ut?

Kommer du mer specifikt utföra enkla sökningar per ID? Behöver du köra frågor mot ett eller flera fält i databasen? Hur ofta kommer data skapas, uppdateras och tas bort? Behöver du kunna köra komplexa analytiska frågor? Och hur snabbt måste de här åtgärderna gå att utföra.

Nu ska vi gå igenom var och en av datamängderna och diskutera vad som behövs.

### <a name="product-catalog-data"></a>Data i produktkatalogen

När det gäller data i produktkatalogen för en onlinebutik så har användarnas behov högsta prioritet. Användarna kommer att köra frågor mot produktkatalogen för att exempelvis hitta alla herrskor, och sedan alla herrskor i en viss storlek. Du kan därför kategorisera kundernas behov som att de behöver många läsåtgärder med möjlighet att köra frågor mot vissa fält.

När kunderna lägger ordrar måste appen dessutom uppdatera kvantiteter i lagret. De här uppdateringarna måste gå lika snabbt som läsåtgärderna så att användarna inte placerar artiklar i kundvagnen när de precis sålts slut. Du behöver därför inte bara många läsåtgärder utan också många skrivåtgärder för data i produktkatalogen. Var noga med att fastställa prioriteten för alla som använder databasen, inte bara de primära användarna.

### <a name="photos-and-videos"></a>Foton och videor

Det gäller dock andra krav för de foton och videor som visas på produktsidorna. De måste gå snabbt att hämta för visning på webbplatsen, men användarna kör inte frågor mot dem separat. Du kan i stället förlita dig på produktfrågorna och behöver bara ha video-ID:t eller webbadressen som en egenskap för produktdata. Du behöver alltså inte kunna köra frågor mot någonting annat än ID:t när det gäller foton och videor.

Dessutom uppdaterar inte användarna befintliga foton eller videor, men de kan lägga till nya foton i produktrecensioner. De kan till exempel ladda upp en bild av själva där de visar upp sina nya skor. Du laddar också upp och tar bort produktfoton från leverantören, men de här uppdateringarna behöver inte gå lika snabbt som andra uppdateringar av produktdata. Sammanfattningsvis behöver du bara köra frågor mot foton och videor per ID och returnera hela filen, och data kommer att skapas och uppdateras mer sällan och med lägre prioritet.  

### <a name="business-data"></a>Affärsdata

För affärsdata görs all analys på historiska data. Inga originaldata uppdateras baserat på analysen. Därför kan affärsdata vara skrivskyddade. Användarna förväntar sig inte heller att komplexa analyser ska utföras omedelbart, så det är okej med en viss svarstid för resultaten. Dessutom lagras affärsdata i flera olika datamängder eftersom användarna ska ha olika skrivåtkomst till olika mängder. Affärsanalytikerna kan dock läsa från samtliga databaser.

## <a name="summary"></a>Sammanfattning

När du bestämmer dig för en lagringslösning ska du tänka igenom hur dina data kommer att användas. Hur ofta kommer olika data att användas? Kan data vara skrivskyddade? Spelar svarstiderna för frågor någon roll? När du har svar på de här frågorna kan du bestämma vilken lagringslösning som passar bäst för dina data.

