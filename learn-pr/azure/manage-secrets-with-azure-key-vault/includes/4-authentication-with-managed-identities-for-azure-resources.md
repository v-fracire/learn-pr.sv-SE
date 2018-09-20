Azure Key Vault använder **Azure Active Directory** till att autentisera användare och program som försöker få åtkomst till ett valv. För att ge vår webbapp åtkomst till valvet, måste vi först registrera appen med Azure Active Directory. Vid registreringen skapas en identitet för appen. När appen har en identitet kan vi tilldela valvbehörigheter till den.

Appar och användare autentiserar till Key Vault med hjälp av en Azure Active Directory-autentiseringstoken. För att få en token från Azure Active Directory krävs en hemlighet eller ett certifikat, eftersom alla med en token kan använda programidentiteten för att komma åt samtliga hemligheter i valvet.

Våra programhemligheter är säkra i valvet, men vi måste ha en hemlighet eller ett certifikat utanför valvet för att komma åt dem! Det här problemet kallas för *startproblemet*, och Azure har en lösning.

## <a name="managed-identities-for-azure-resources"></a>Hanterade identiteter för Azure-resurser

Hanterade identiteter för Azure-resurser är en Azure-funktion som din app kan använda för att få åtkomst till Key Vault och andra Azure-tjänster utan att behöva hantera en enda hemlighet utanför valvet. Att använda hanterad identitet är ett enkelt och säkert sätt att utnyttja Key Vault från din webbapp.

När du aktiverar hanterad identitet på din webbapp aktiverar Azure en separat tokenbeviljande REST-tjänst som ska användas specifikt av din app. Appen kommer att begära token från den här tjänsten i stället för direkt från Azure Active Directory. Din app måste använda en hemlighet för att få åtkomst till den här tjänsten, men den hemligheten matas in i appens miljövariabler med App Service när den startas. Du behöver inte hantera eller lagra det här hemliga värdet någonstans, och inget utanför appen kan komma åt den här hemligheten eller den hanterade identitetens tokentjänstslutpunkt.

Hanterade identiteter för Azure-resurser registrerar även din app i Azure Active Directory åt dig och tar bort registreringen om du tar bort webbappen eller inaktiverar dess hanterade identitet.

Hanterade identiteter är tillgängliga i alla utgåvor av Azure Active Directory, inklusive den kostnadsfria versionen som ingår i en Azure-prenumeration. Att använda dem i App Service medför inte någon extra kostnad och kräver inte någon konfiguration. De kan aktiveras eller inaktiveras i en app när som helst.

> [!NOTE]
> Hanterade identiteter för Azure-resurser stöds inte för Linux eller containerwebbappar för närvarande.

När du aktiverar en hanterad identitet för en webbapp krävs bara ett enda Azure CLI-kommando utan konfiguration. Vi ska göra detta senare när vi konfigurerar en App Service-app och distribuerar den till Azure. Innan dess ska vi dock använda vår kunskap om hanterade identiteter till att skriva koden för appen.