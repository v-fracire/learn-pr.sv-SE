Det finns tre olika molndistributionsmodeller. En molndistributionsmodell definierar var dina data lagras och hur dina kunder interagerar med dem – hur kommer de åt dem, och var körs programmen? Vilken modell som passar dig beror också på hur mycket av din egen infrastruktur som du vill eller måste hantera.

Här ska vi utforska olika typer av distributionsmetoder för dina molnresurser. 

## <a name="public-cloud"></a>Offentligt moln

Det här är den vanligaste distributionsmodellen. I detta fall har du ingen lokal maskinvara som du hanterar eller håller uppdaterad – allt körs på molnleverantörens maskinvara. I vissa fall kan du spara ytterligare kostnader genom att dela beräkningsresurser med andra molnanvändare. 

### <a name="advantages"></a>Fördelar

- Hög skalbarhet – du behöver inte köpa en ny server för att skala upp
- Priser för användningsbaserad betalning – du betalar bara för det du använder
- Du slipper bekymra dig om underhåll eller uppdatering av maskinvaran

### <a name="disadvantages"></a>Nackdelar

- Större säkerhetshot
- Delad maskinvara

## <a name="private-cloud"></a>Privat moln

Ett privat moln är ofta ett lokalt datacenter som ditt företag hanterar. Det har ingen anslutning till någon av tjänsterna som du kör i molnet. Ett exempel på den här modellen är ett företag som inte har migrerat någonting till molnet och som inte planerar att göra det.

### <a name="advantages"></a>Fördelar

- Högre säkerhet eftersom allt finns i företagets nätverk
- Maskinvara/resurser delas inte

### <a name="disadvantages"></a>Nackdelar

- Egen maskinvara krävs för att komma igång och för underhåll
- Kräver IT-kunskaper

## <a name="hybrid-cloud"></a>Hybridmoln

Det här är en kombination av det lokala datacentret som hanteras av ditt företag och det offentliga molnet. De två molnen är anslutna och kan utbyta data med varandra. Detta är bra om det finns vissa saker som du inte kan placera i molnet, till exempel av juridiska skäl. Du kanske exempelvis har data som inte kan exponeras offentligt (till exempel medicinska uppgifter). Ett annat exempel är ett eller flera program som körs på äldre maskinvara som inte kan uppdateras. I detta fall kan det gamla systemet fortsätta att köras lokalt och anslutas det till det offentliga molnet för auktorisering eller lagring.

### <a name="advantages"></a>Fördelar

- Du kan fortsätta att köra och använda system med gammal maskinvara eller med ett föråldrat operativsystem
- Flexibilitet att välja vad du vill köra lokalt och vad du vill köra i molnet

### <a name="disadvantages"></a>Nackdelar

- Kan bli dyrare än om du väljer en distributionsmodell
- Mer komplicerat att konfigurera och hantera

## <a name="summary"></a>Sammanfattning

Molntjänster är flexibla och ger dig möjlighet att välja hur du vill distribuera dem. Normalt sett beror din distribution på din budget samt dina behov av säkerhet, skalbarhet och underhåll.