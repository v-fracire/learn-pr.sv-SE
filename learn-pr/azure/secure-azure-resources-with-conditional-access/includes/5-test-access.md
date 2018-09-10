I den förra övningen gick vi igenom grunderna i Azure AD. Vi skapade en katalog, skapade en användare och en grupp och skapade sedan en regel för villkorlig åtkomst som kräver multifaktorautentisering för åtkomst till Azure Portal.

Nu ska vi testa om vi kan komma åt våra resurser.

## <a name="test-access-to-resources"></a>Testa åtkomsten till resurser

Eftersom du vet att användarna kommer att logga in och komma åt alla sina SaaS-program via MyApps-portalen så väljer vi att testa just detta.

1. Öppna ett InPrivate-webbläsarfönster.

1. Gå till https://myapps.microsoft.com.

1. Logga in som användaren som vi skapade i del 2.

   * Observera att du loggas in på portalen utan att utföra multifaktorautentisering.

1. Gå till https://portal.azure.com i samma webbläsarfönster.

   * Nu uppmanas du att ange mer information för att skydda ditt konto. Det här avbrottet är i själva verket Azure Multi-Factor Authentication som aktiveras via principen för villkorlig åtkomst som vi skapade. Du kan avbryta här och stänga webbläsarfönstret.

## <a name="summary"></a>Sammanfattning

I det här avsnittet har du lärt dig hur du beviljar behörighet till en användare att skapa och hantera virtuella datorer i en resursgrupp via Azure PowerShell. I nästa del ska vi se hur du skapar en anpassad roll och definierar dina egna behörigheter.