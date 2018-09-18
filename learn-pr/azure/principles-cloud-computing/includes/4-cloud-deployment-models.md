Det finns tre olika molndistributionsmodeller. En molndistributionsmodell definierar var dina data lagras och hur dina kunder interagerar med dem – hur kommer de åt dem, och var körs programmen? Vilken modell som passar dig beror också på hur mycket av din egen infrastruktur som du vill eller måste hantera.

Här ska vi utforska olika typer av distributionsmetoder för dina molnresurser.

:::row:::
    :::column:::
        ![Ikon för offentligt moln](../media/4-public-cloud.png)
    :::column-end:::
    :::column span="3"::: **Offentligt moln**

Det här är den vanligaste distributionsmodellen. I detta fall har du ingen lokal maskinvara som du hanterar eller håller uppdaterad – allt körs på molnleverantörens maskinvara. I vissa fall kan du spara ytterligare kostnader genom att dela beräkningsresurser med andra molnanvändare.

Några av fördelarna med det offentliga molnet är:

- Hög skalbarhet – du behöver inte köpa en ny server för att skala upp
- Priser för användningsbaserad betalning – du betalar bara för det du använder
- Du slipper bekymra dig om underhåll eller uppdatering av maskinvaran :::column-end:::
  :::row-end:::
:::row:::
   :::column:::
        ![Ikon för offentligt moln](../media/4-private-cloud.png)
    :::column-end:::
    :::column span="3"::: **Privat moln**

I ett privat moln skapar du en molnmiljö i ditt eget datacenter och ger användare i din organisation åtkomst till beräkningsresurser via självbetjäning. Detta fungerar som en simulering av ett offentligt moln för dina användare, men du ansvara helt för inköp och underhåll av de maskin- och programvarutjänster som du tillhandahåller.

Några skäl till varför team överger det privata molnet är:

- Egen maskinvara krävs för att komma igång och för underhåll
- Privata moln kräver avancerade IT-kunskaper och expertis
:::column-end:::
:::row-end:::
 :::row:::
    :::column:::
        ![Ikon för hybridmoln](../media/4-hybrid-cloud.png)
    :::column-end:::
    :::column span="3"::: **Hybridmoln**

Ett hybridmoln kombinerar offentliga och privata moln, så att du kan köra dina program på den lämpligaste platsen. Du kan till exempel lagra en webbplats i det offentliga molnet och länka den till en mycket säker databas som finns i ditt privata moln (eller i ett lokalt datacenter).

Detta är bra om det finns vissa saker som du inte kan placera i molnet, till exempel av juridiska skäl. Du kanske exempelvis har data som inte kan exponeras offentligt (till exempel medicinska uppgifter). Ett annat exempel är ett eller flera program som körs på äldre maskinvara som inte kan uppdateras. I detta fall kan det gamla systemet fortsätta att köras lokalt och anslutas det till det offentliga molnet för auktorisering eller lagring.

Några av fördelarna med ett hybridmoln jämfört med ett privat moln är:

- Du kan fortsätta att köra och använda system med gammal maskinvara eller med ett föråldrat operativsystem
- Du har flexibilitet att välja vad du vill köra lokalt och vad du vill köra i molnet

Några saker som du bör ha i åtanke är:

- Det kan bli dyrare än om du väljer en distributionsmodell
- Det kan vara mer komplicerat att konfigurera och hantera :::column-end:::
  :::row-end:::

## <a name="summary"></a>Sammanfattning

Molntjänster är flexibla och ger dig möjlighet att välja hur du vill distribuera dem. Vilken molndistributionsmodell du väljer beror på din budget och på dina säkerhets-, skalbarhets- och underhållsbehov.