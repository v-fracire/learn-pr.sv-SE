När du har identifierat vilken typ av data som du hanterar (strukturerade, delvis strukturerade eller Ostrukturerade), är nästa steg att avgöra hur du ska använda data. Till exempel som en näthandelsföretagen vet du kunder behöver Snabbåtkomst till produktdata och företagsanvändare behöver köra komplexa analytiska frågor. När du har gått igenom dessa krav kan tar din dataklassificering hänsyn, du börja planera din lösning för datalagring.

Här kan du går igenom några av frågorna när du bestämma vad som skulle göra med dina data.

## <a name="operations-and-latency"></a>Åtgärder och svarstider

Vilka är de viktigaste åtgärderna som du kommer att slutföra varje datatyp och vilka är prestandakraven på?

Ställa dig följande frågor:
* Som du utför enkla sökningar med hjälp av ett ID? 
* Behöver du köra frågor mot ett eller flera fält i databasen? 
* Hur ofta kommer data skapas, uppdateras och tas bort? 
* Behöver du kunna köra komplexa analytiska frågor? 
* Hur snabbt de här åtgärderna måste slutföra?

Nu ska vi gå igenom var och en av de uppgifter som med dessa frågor i åtanke och diskutera vad som behövs.

## <a name="product-catalog-data"></a>Data i produktkatalogen

För produkten katalogdata i onlinebutiker scenariot är kundernas behov högsta prioritet. Kunder vill produktkatalogen för att hitta, till exempel, alla män situation, och sedan män skor på försäljning och sedan män skor på försäljningen i en viss storlek. Så kan kundernas behov kräva mycket läsåtgärder, med möjlighet att fråga på vissa fält.

Dessutom när kunder beställa, måste programmet uppdatera produktkvantiteter. Uppdateringsåtgärder måste ske precis så snabbt som läsåtgärder så att användare inte placera ett objekt i sina kundvagnar när objektet har bara slutsåld. Du behöver därför inte bara många läsåtgärder utan också många skrivåtgärder för data i produktkatalogen. Var noga med att fastställa prioriteten för alla som använder databasen, inte bara de primära användarna.

## <a name="photos-and-videos"></a>Foton och videor

Foton och videoklipp som visas på produktsidor har olika krav. De behöver snabb datahämtning gånger så att de visas på webbplatsen samtidigt som produktdata för katalogen, men de behöver inte efterfrågas oberoende av varandra. Du kan i stället förlita dig på produkten frågans resultat och inkluderar video-ID eller URL: en som en egenskap på produktdata. Så, foton och videoklipp behöver endast hämtas av deras ID.

Dessutom kan gör användare inte uppdateringar för befintliga foton och videor. De kan dock lägga till nya foton för produktrecensioner. Exempelvis kan en användare kan också ladda upp en bild av själva som visar av sina nya skor. Som en anställd du också ladda upp och ta bort produktfoton från leverantören, men att uppdateringen inte behöver så snart som dina andra datauppdateringar för produkten. 

Sammanfattningsvis, foton och videoklipp kan frågas efter ID att returnera hela filen, men skapar och uppdateringar kommer att vara mer sällan och är lika viktig.  

## <a name="business-data"></a>Affärsdata

För affärsdata görs all analys på historiska data. Inga ursprungliga data uppdateras baserat på analysen, så att affärsdata är skrivskyddad. Användare inte förväntar sig sin komplexa analyser för att köras direkt så att ha viss fördröjning i resultatet är okej. Dessutom lagras affärsdata i flera olika datauppsättningar eftersom användarna ska ha olika skrivåtkomst till varje datauppsättning. Affärsanalytiker kommer dock att kunna läsa från alla databaser.

## <a name="summary"></a>Sammanfattning

När du bestämmer vilken lösning du använder du tänka på hur dina data kommer att användas. Hur ofta kommer dina data att användas? Är dina data skrivskyddade? Spelar svarstiderna någon roll? När du har svar på de här frågorna kan du bestämma vilken lagringslösning som passar bäst för dina data.