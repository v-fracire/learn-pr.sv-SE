Azure Key Vault använder **Azure Active Directory** till att autentisera användare och program som försöker få åtkomst till ett valv. För att ge vår webbapp åtkomst till valvet, måste vi först registrera appen med Azure Active Directory. Vid registreringen skapas en identitet för appen. När vi har en identitet kan vi tilldela valvbehörigheter till den.

Appar och användare autentiserar till Key Vault med hjälp av en Azure Active Directory-autentiseringstoken. För att få en token från Azure Active Directory krävs en hemlighet eller ett certifikat, eftersom alla med en token kan använda programidentiteten för att komma åt samtliga hemligheter i valvet.

Våra programhemligheter är säkra i valvet, men vi måste ha en hemlighet eller ett certifikat utanför valvet för att komma åt dem! Det här problemet kallas för *startproblem* och Azure har en lösning.

## <a name="managed-service-identity"></a>Hanterad tjänstidentitet

*Hanterad tjänstidentitet* (MSI) är en funktion i Azure App Service som din app kan använda för att få åtkomst till Key Vault och andra Azure-tjänster utan att behöva hantera en enda hemlighet. Det är enklare och säkrare att använda MSI än att hantera en hemlighet själv.

När du aktiverar MSI aktiverar Azure en separat tokenbeviljande REST-tjänst som ska användas specifikt av din app. När appen begär en token från MSI-tokentjänsten, kontaktar tjänsten Azure Active Directory för att få en token för MSI-identiteten. Den skickas sedan till din app för användning med Key Vault. Den viktigaste delen i det här arbetsflödet är vilken metod din app har för att autentisera till MSI-tokentjänsten &mdash; i stället för att kräva en hemlighet eller ett certifikat som du behöver hantera i din konfiguration, använder din app en hemlighet som Azure lägger in på ett säkert sätt i sina miljövariabler när den startas. Du behöver inte hantera eller lagra denna hemlighet någonstans. Inget utanför appen kan komma åt den här hemligheten eller MSI-tokentjänstens slutpunkt.

MSI registrerar även din app i Azure Active Directory åt dig och tar bort registreringen om du inaktiverar MSI eller tar bort appen.

MSI är tillgängligt i alla utgåvor av Azure Active Directory, inklusive den kostnadsfria versionen som ingår i en Azure-prenumeration. Att använda MSI i App Service medför inte någon extra kostnad och kräver inte någon konfiguration. Det kan aktiveras eller inaktiveras i en app när som helst.

> [!NOTE]
> MSI stöds inte för Linux eller behållarwebbappar för närvarande.

När du aktiverar MSI krävs bara ett enda Azure CLI-kommando utan konfiguration. Vi ska göra detta i enhet 5 när vi skapar en App Service-app och distribuerar den till Azure. Innan dess ska vi dock använda vår kunskap om MSI till att skriva koden för appen.