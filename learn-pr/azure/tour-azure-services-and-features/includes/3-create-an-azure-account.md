Med ett kostnadsfritt Azure-konto kan du skapa, testa och distribuera företagsprogram, skapa anpassade webb- och mobilmiljöer och få insikter från data med hjälp av maskininlärning och kraftfull analys.

## <a name="what-is-an-azure-account"></a>Vad är ett Azure-konto?

Ett _Azure-konto_ är knutet till en viss identitet och innehåller information som:

- Namn, e-postadress och kontaktinställningar
- Faktureringsinformation, till exempel ett kreditkort

Ett Azure-konto är kopplat till en eller flera _prenumerationer_.

## <a name="what-is-an-azure-subscription"></a>Vad är en Azure-prenumeration?

En _Azure-prenumeration_ är en logisk behållare som används för att etablera resurser i Microsoft Azure. Den innehåller information om alla dina resurser, som virtuella datorer och databaser. Den har också en förtroenderelation till en _klientorganisation_ i Azure AD, som används till att autentisera användare och roller för de resurser som lagras i prenumerationen.

Faktureringen sker på prenumerationsnivå. Du kan konfigurera utgiftsgränser för varje prenumeration så att du slipper överraskningar i slutet av månaden. 

## <a name="what-is-an-azure-ad-tenant"></a>Vad är en klientorganisation i Azure AD?

Azure AD (Azure Active Directory) är en modern identitetsprovider som har stöd för flera autentiseringsprotokoll för att skydda program och tjänster i molnet. Azure AD bör _inte_ förväxlas med Windows Active Directory, som fokuserar på att skydda Windows-baserade datorer och servrar. Azure AD handlar i stället om webbaserade autentiseringsstandarder som OpenID och OAuth.

En enda klientorganisation representerar en logisk organisation och gör att flera identiteter kan komma åt och använda resurser som skyddas av klientorganisationen. En Azure-prenumeration har alltid en förtroenderelation med en _enda_ Azure AD-klientorganisation, men _flera_ prenumerationer kan dela en enskild klientorganisation. Den här strukturen gör att organisationen kan hantera flera prenumerationer och definiera säkerhetsregler för alla resurser som de innehåller.

Här är en enkel representation av konton, prenumerationer, klientorganisationer och resurser.

![Diagram som visar hur konton, klientorganisationer, prenumerationer och resurser fungerar tillsammans](../media/3-azure-ad-tenant.png)

Observera att varje Azure AD-klientorganisation har en _kontoägare_. Det här är det ursprungliga Azure-kontot som ansvarar för faktureringen. Du kan lägga till ytterligare användare i klientorganisationen och även bjuda in gäster från andra Azure AD-klientorganisationer för åtkomst till resurser i prenumerationer.

## <a name="azure-account-types"></a>Typer av Azure-konton

Azure har flera typer av konton för olika kundtyper. De vanligaste kontona är:

- Kostnadsfritt
- Betala per användning
- Enterprise-avtal

### <a name="azure-free-account"></a>Kostnadsfritt Azure-konto

Ett kostnadsfritt Azure-konto innehåller en **kredit på 200 USD** som du kan spendera under de första 30 dagarna, kostnadsfri tillgång till de populäraste Azure-produkterna i 12 månader och tillgång till fler än 25 produkter som alltid är kostnadsfria. Det här är ett utmärkt sätt för nya användare att komma igång. Om du vill skaffa ett kostnadsfritt konto behöver du ett telefonnummer, ett kreditkort och ett Microsoft-konto.

> [!NOTE]
> Kreditkortsinformationen används endast för identitetsverifiering. Du debiteras inte för några tjänster förrän du uppgraderar.

### <a name="azure-pay-as-you-go-account"></a>Azure-konto där du betalar per användning

Med ett konto där du betalar per användning (PAYG) debiteras du varje månad för de tjänster som du använder. Den här typen av konto är lämplig för ett stort antal användare, från enskilda användare till små företag och stora organisationer.

### <a name="azure-enterprise-agreement"></a>Azure Enterprise-avtal

Ett Enterprise-avtal erbjuder flexibilitet för att köpa molntjänster och programvarulicenser genom ett avtal, med rabatter för nya licenser och Software Assurance. Det är framförallt avsett för stora organisationer.

## <a name="summary"></a>Sammanfattning

Oavsett om du är en individ, ett litet företag eller ett större företag behöver du ett konto för att kunna använda Azure-tjänster. Det vanligaste sättet att gå tillväga är att börja med ett kostnadsfritt konto så att du kan utvärdera Azure-tjänsterna. När utvärderingsperioden upphör konverterar du från det kostnadsfria kontot till att betala per användning.