Azure Key Vault använder **Azure Active Directory** till att autentisera användare och program som försöker få åtkomst till ett valv. För att ge vår webbapp åtkomst till valvet, måste vi först registrera appen med Azure Active Directory. Vid registreringen skapas en identitet för appen. När appen har en identitet kan vi tilldela valvbehörigheter till den.

Appar och användare autentiserar till Key Vault med hjälp av en Azure Active Directory-autentiseringstoken. För att få en token från Azure Active Directory krävs en hemlighet eller ett certifikat, eftersom alla med en token kan använda programidentiteten för att komma åt samtliga hemligheter i valvet.

Våra programhemligheter är säkra i valvet, men vi måste ha en hemlighet eller ett certifikat utanför valvet för att komma åt dem! Det här problemet kallas för *startproblem* och Azure har en lösning.

## <a name="managed-identities-for-azure-resources"></a>Hanterade identiteter för Azure-resurser

Hanterade identiteter för Azure-resurser är en funktion i Azure som din app kan använda för att få åtkomst till Key Vault och andra Azure-tjänster utan att behöva hantera även en enda hemlighet utanför valvet. Med hjälp av en hanterad identitet är ett enkelt och säkert sätt att dra nytta av Key Vault från ditt program.

När du aktiverar hanterad identitet på web Apps, aktiverar Azure en separat token-granting REST-tjänst som ska användas av din app. Din app kommer att begära token från den här tjänsten i stället för direkt från Azure Active Directory. Din app måste använda en hemlighet för att få åtkomst till den här tjänsten, men den hemligheten injiceras i appens miljövariabler med App Service när den startas. Du behöver inte hantera eller lagra den här hemligt värde var som helst och inget utanför appen kan komma åt den här hemligheten eller hanterad identitet token tjänstslutpunkten.

Hanterade identiteter för Azure-resurser kan du även registrerar din app i Azure Active Directory för dig, och tar bort registreringen om du tar bort webbappen eller inaktivera dess hanterad identitet.

Hanterade identiteter är tillgängliga i alla utgåvor av Azure Active Directory, inklusive den kostnadsfria versionen ingår i en Azure-prenumeration. Att använda MSI i App Service medför inte någon extra kostnad och kräver inte någon konfiguration. Det kan aktiveras eller inaktiveras i en app när som helst.

> [!NOTE]
> Hanterade identiteter för Azure-resurser stöds inte för Linux eller behållare web apps.

Aktivera en hanterad identitet för en webbapp måste bara en enda Azure CLI-kommando utan konfiguration. Vi ska göra detta senare när vi skapar en App Service-app och distribuerar den till Azure. Innan som, men vi att använda vår kunskap om hanterade identiteter att skriva kod för appen.