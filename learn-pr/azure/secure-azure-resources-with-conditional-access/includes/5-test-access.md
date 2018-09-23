I föregående övningar så skapade vi en katalog, skapade en användare och en grupp och skapade sedan en regel för villkorlig åtkomst som kräver multifaktorautentisering för åtkomst till Azure Portal. Nu ska vi testa om vi kan komma åt våra resurser.

## <a name="test-access-to-resources"></a>Testa åtkomsten till resurser

Eftersom du vet att användarna kommer att logga in och komma åt alla sina SaaS-program via MyApps-portalen så väljer vi att testa just detta.

1. Öppna ett InPrivate-webbläsarfönster.

1. Gå till https://myapps.microsoft.com.

1. Logga in som användaren som vi skapade i del 3.

   * Observera att du loggas in på portalen utan att utföra multifaktorautentisering.

1. Gå till https://portal.azure.com i samma webbläsarfönster.

   * Nu uppmanas du att ange mer information för att skydda ditt konto. Det här avbrottet är i själva verket Azure Multi-Factor Authentication som aktiveras via principen för villkorlig åtkomst som vi skapade. Du kan avbryta här och stänga webbläsarfönstret.