Minne är den viktigaste resursen för Azure Redis Cache, eftersom det är en databas i minnet. Du kan köra får problem när du börjar lägga till data som överskrider mängden tillgängligt minne. Azure Redis Cache stöder avlägsningsprinciper som indikerar hur data ska hanteras när du kör slut på minne.

Här kan du ange en Borttagningsprincip för att fastställa vad dina data ska göra om du överskrider den maximala mängden minne.

## <a name="what-is-an-eviction-policy"></a>Vad är en Borttagningsprincip?

En Borttagningsprincip är en plan som avgör hur dina data ska hanteras när du överskrider den maximala mängden tillgängligt minne. Till exempel kan med en Borttagningsprincip, du konfigurera Azure Redis Cache och ta bort en slumpmässig nyckel för att göra plats för nya data infogas.

### <a name="types-of-eviction-policies"></a>Typer av avlägsningsprinciper

Det finns sex olika avlägsningsprinciper som tillhandahålls av Azure Redis Cache. Alla dessa värden kan utföra en åtgärd när du försöker infoga data när du är slut på minne.

* **noeviction:** ingen princip för borttagning. Returnerar ett felmeddelande visas om du försöker infoga data.

* **allkeys lru:** tar bort minst senast använda nyckeln.

* **allkeys slumpmässiga:** tar bort en slumpmässig nyckel.

* **beräkningsbara lru:** tar bort minst senast använda nyckeln utanför alla nycklar med ett förfallodatum.

* **beräkningsbara ttl:** tar bort nyckeln med kortaste time to live utifrån förfallodatum som angetts för den.

* **beräkningsbara slumpmässiga:** tar bort en slumpmässig nyckel som har en giltighetstid ange.

## <a name="how-to-set-an-eviction-policy"></a>Hur du ställer in en princip för borttagning

Azure tillhandahåller en enkel nedrullningsbar meny för att ställa in avlägsningsprincipen för Azure Redis Cache. Välj **avancerade inställningar**, och använda den **princip för max. minne** nedrullningsbara menyn.

Eftersom minne är viktiga för Azure Redis Cache, finns det stöd för avlägsningsprinciper. En Borttagningsprincip avgör vad ska göras med befintliga data när du får slut på minne och försök att infoga nya data.