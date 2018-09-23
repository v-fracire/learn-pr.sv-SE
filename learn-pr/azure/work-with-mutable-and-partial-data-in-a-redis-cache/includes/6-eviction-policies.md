Minne är den viktigaste resursen för Redis eftersom det är en minnesintern databas. Du kan stöta på problem när du börjar lägga till data som överskrider mängden tillgängligt minne. Azure Redis Cache stöder borttagningsprinciper, som anger hur data ska hanteras när du får slut på minne.

Här anger du en borttagningsprincip för att fastställa vad dina data ska vara när du överskrider den maximala mängden minne.

## <a name="what-is-an-eviction-policy"></a>Vad är en borttagningsprincip?

En borttagningsprincip är en plan som bestämmer hur dina data ska hanteras när du överskrider den maximala mängden tillgängligt minne. Genom att använda en borttagningsprincip kan du till exempel säga till Azure Redis Cache att ta bort en slumpmässig nyckel för att göra plats för nya data som infogas.

### <a name="types-of-eviction-policies"></a>Typer av borttagningsprinciper

Det finns sex olika borttagningsprinciper som tillhandahålls av Azure Redis Cache. Alla dessa värden utför en åtgärd när du försöker infoga data när minnesutrymmet är slut.

* **noeviction:** ingen borttagningsprincip. Returnerar ett meddelanden om du försöker infoga data.

* **allkeys-lru:** tar bort den tidigast använda nyckeln.

* **allkeys-random:** tar bort en slumpmässig nyckel.

* **volatile-lru:** tar bort den tidigast använda nyckeln av alla nycklar med angiven förfallotid.

* **volatile-ttl:** tar bort nyckeln med kortaste livslängd baserat på den angivna förfallotiden.

* **volatile-random:** tar bort en slumpmässig nyckel som har en angiven förfallotid.

## <a name="how-to-set-an-eviction-policy"></a>Så här anger du en borttagningsprincip

Azure har en enkel nedrullningsbar meny för att ange borttagningsprincip för Azure Redis Cache. Välj **Avancerade inställningar** och använd listmenyn **maxmemory-policy**.

Eftersom minne är kritiskt för Azure Redis Cache så finns det stöd för borttagningsprinciper. En borttagningsprincip bestämmer vad som ska göras med befintliga data när minnesutrymmet är slut och du försöker infoga nya data.