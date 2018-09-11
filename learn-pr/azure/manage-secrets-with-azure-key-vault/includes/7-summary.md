I den här modulen har du skyddat en apps hemliga konfiguration i Azure Key Vault. Vår appkod autentiserar till valvet med Hanterad tjänstidentitet och läser automatiskt in hemligheterna från valvet till ASP.NET Core-konfigurationssystemet vid start.

## <a name="cleanup"></a>Rensa

Om du vill rensa din Azure-prenumeration kör du följande i Azure Cloud Shell för att ta bort resursgruppen som innehåller alla de resurser som vi skapade i den här modulen.

```console
az group delete --name keyvault-exercise-group
```

Du kan rensa Cloud Shell-lagringen genom att ta bort katalogen `KeyVaultDemoApp`.

## <a name="next-steps"></a>Nästa steg

Om det här var en verklig app, vad är nästa steg?

- Att placera alla apphemligheter i dina valv! Det finns inte längre någon anledning att ha dem i konfigurationsfiler.
- Fortsätt att utveckla programmet. Din produktionsmiljö har konfigurerats, och du behöver inte upprepa alla inställningar vid framtida distributioner till miljön.
- För att underlätta utvecklingen skapar du ett valv för utvecklingsmiljön som innehåller hemligheter med samma namn men med andra värden. Tilldela behörigheter till utvecklingsteamet och konfigurera valvnamnet i konfigurationsfilen för appens utvecklingsmiljö. När utvecklare kör appen lokalt identifierar `AddAzureKeyVault` automatiskt lokala installationer av Visual Studio och Azure CLI och använder Azure-autentiseringsuppgifterna som konfigurerats i programmen för att logga in och komma åt valvet.
- Skapa ytterligare miljöer för särskilda ändamål som till exempel acceptanstest för användare.
- Avgränsa valv för olika prenumerationer och/eller resursgrupper för att isolera dem.
- Bevilja åtkomst till andra miljövalv till rätt personer.

## <a name="further-reading"></a>Ytterligare läsning

- [Dokumentation om Key Vault](https://docs.microsoft.com/azure/key-vault/)
- [Mer om AddAzureKeyVault och dess avancerade alternativ](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x)
- [Den här självstudien](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application) beskriver hur du använder `KeyVaultClient`, inklusive manuell autentisering mot Azure Active Directory med hjälp av en klienthemlighet i stället för MSI.
- [Dokumentation om MSI-tokentjänsten](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) om du vill implementera autentiseringsarbetsflödet själv