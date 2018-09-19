Föreställ dig ett scenario där en hektisk hårsalong har ett återkommande problem: deras kunder missar ofta sina avtalade tider. De avtalade tiderna är reserverade tidsintervaller. Om en kund missar en avtalad tid förlorar salongen pengar. För att åtgärda problemet kontaktar salongen dig, en programvaruutvecklare. För att förbättra problemet väljer du att skicka påminnelser i form av textmeddelanden. Dessa kan skickas så fort den avtalade tiden schemaläggs eller ändras och varje morgon skickar du ett sms till alla kunder med en avtalad tid den dagen.

Du måste skapa en tjänst som kan enkelt schemaläggas, uppdateras och skalas. Du bestämmer dig för att lösa problemet med hjälp av en funktionsapp. Du vet redan hur man implementerar logik för att skicka ett SMS. Nu ska du lära dig hur du skickar meddelandet vid en viss tidpunkt eller när en viss händelse inträffar. Som tur är har Azure Functions stöd för en funktion som kallas _utlösare_. Utlösare används för att bestämma hur en Azure-funktion ska utföras.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:
- Avgöra vilken utlösare som fungerar bäst för dina verksamhetsbehov
- Skapa en timerutlösare för att anropa en funktion enligt ett konsekvent schema
- Skapa en HTTP-utlösare för att anropa en funktion när en HTTP-begäran tas emot
- Skapa en blob-utlösare för att anropa en funktion när en blob skapas eller uppdateras i Azure Storage