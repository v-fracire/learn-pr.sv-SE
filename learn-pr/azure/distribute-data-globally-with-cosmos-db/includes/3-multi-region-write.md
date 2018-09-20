Att garantera en bra användarupplevelse i ett sådant e-handelsscenario som ditt kräver hög tillgänglighet och låg latens.

Genom att möjliggöra stöd för flera original på det Azure Cosmos DB-konto du precis har skapat får du flera fördelar för båda dessa prestandaaspekter, och alla fördelar garanteras av de ekonomiskt uppbackade serviceavtalen som tillhandahålls av Azure Cosmos DB.

## <a name="what-is-multi-master-support"></a>Vad är stöd för flera original?

Stöd för flera original är ett alternativ som kan aktiveras på nya Azure Cosmos DB-konton. När kontot har replikerats i flera regioner är varje region en originalregion som deltar på lika villkor i en write-anywhere-modell (skriv var som helst), som också kallas för aktiv/aktiv-mönster.

Azure Cosmos DB-regioner som fungerar som originalregioner i en konfiguration med flera original arbetar automatiskt för att konvergera data skrivna till alla repliker och säkerställa global konsekvens och dataintegritet.

Med stöd för flera original i Azure Cosmos kan du utföra skrivningar på vilken container som helst i en skrivningsaktiverad region globalt. Skrivna data sprids till alla andra regioner direkt.  

## <a name="what-are-the-benefits-of-multi-master-support"></a>Vilka är fördelarna med stöd för flera original?

Fördelarna med stöd för flera original är:

* Ensiffrig skrivfördröjning – Konton med flera original har en förbättrad skrivfördröjning på <10 ms för 99 % av skrivningarna, upp från <15 ms för konton som inte har flera original.
* 99,999 % tillgänglighet för läsning/skrivning – Ökad tillgänglighet för skrivning till 99,999 %, upp från 99,99 % för konton som inte har flera original.
* Obegränsad skalbarhet för skrivning och dataflöde – Med konton med flera original kan du skriva till alla regioner, vilket ger obegränsad skalbarhet för skrivning och dataflöde för att stödja flera miljarder enheter.
* Inbyggd konfliktlösning – Konton med flera original har tre metoder för at lösa konflikter för att säkerställa global dataintegritet och konsekvens. 

## <a name="conflict-resolution"></a>Konfliktlösning

Med stödet för flera original följer risken att stöta på konflikter för skrivningar till olika regioner. Konflikter är mycket sällsynta i Azure Cosmos DB och kan bara förekomma när ett objekt ändras samtidigt i flera regioner, innan en spridning har skett mellan regionerna. Med tanke på den hastighet som replikering sker globalt borde du inte uppleva detta ofta, om alls. Men Azure Cosmos DB har inte konfliktlösningslägen där användarna kan bestämma hur de ska hantera scenarier där samma post uppdateras samtidigt av olika skrivare i två eller flera regioner.  

Det finns tre konfliktlösningslägen i Azure Cosmos DB. 
* **Last-Writer-Wins (LWW)** (senaste skrivning vinner), där konflikter löses utifrån värdet för en användardefinierad heltalsegenskap i dokumentet. Som standard används _ts för att fastställa det senast skrivna dokumentet. Last-Writer-Wins är standardmekanismen för konflikthantering.
* **Custom - User Defined Procedure** (Anpassad – användardefinierad procedur), där du helt kan styra konfliktlösningen genom att registrera en användardefinierad procedur för samlingen. En användardefinierad procedur är en särskild typ av lagrad procedur med den mycket specifik signatur. Om den användardefinierade proceduren misslyckas eller inte finns lägger Azure Cosmos DB till alla konflikter i feeden för skrivskyddade konflikter så att de kan bearbetas asynkront.  
* **Custom - Async** (Anpassad – Asynkron), där Azure Cosmos DB exkluderar alla konflikter från att checkas in och registrerar dem i feeden för skrivskyddade konflikter för uppskjuten lösning av användarens program. Programmet kan utföra konfliktlösning asynkront och använda logik eller referera till en extern källa, ett externt program eller en extern tjänst för att lösa konflikten.

## <a name="summary"></a>Sammanfattning

I den här kursdelen har du fått lära dig om konton med flera original, där du kan skriva data till flera regioner och därmed få bättre tillgänglighet och prestanda.