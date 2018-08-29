Föreställ dig ett scenario där en hektisk hårsalong har ett återkommande problem: deras kunder missar ofta sina avtalade tider. De avtalade tiderna är reserverade tidsintervaller. Om en kund missar en avtalad tid förlorar salongen pengar. För att åtgärda problemet kontaktar salongen dig, en programvaruutvecklare. För att förbättra problemet väljer du att skicka påminnelser i form av textmeddelanden. Varje morgon skickar du ett SMS till alla kunder som har en avtalad tid den dagen.

Som Azure-utvecklare bestämmer du dig för att lösa problemet med hjälp av en Azure-funktion. Du vet redan hur du implementerar logik för att skicka ett SMS. Nu ska du lära dig hur du skickar meddelandet vid en viss tidpunkt. Som tur är har Azure-funktioner stöd för en funktion som kallas _utlösare_. Utlösare används för att bestämma hur en Azure-funktion ska utföras.

## <a name="learning-objectives"></a>Utbildningsmål
> [!div class="checklist"]
> * Avgör vilken utlösare som fungerar bäst för dina affärsbehov.
> * Skapa en timerutlösare för att anropa en funktion enligt ett konsekvent schema.
> * Skapa en HTTP-utlösare för att anropa en funktion när en HTTP-begäran tas emot.
> * Skapa en blob-utlösare för att anropa en funktion när en blob skapas eller uppdateras i Azure Storage.
