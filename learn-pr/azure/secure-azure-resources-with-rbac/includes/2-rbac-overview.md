Anta att du behöver hantera åtkomsten till resurser i Azure för utvecklings-, teknik- och marknadsföringsteam. Du har börjat få förfrågningar och behöver snabbt lära dig hur åtkomsthanteringen fungerar för Azure-resurser.

## <a name="what-is-rbac"></a>Vad är RBAC?

Rollbaserad åtkomstkontroll (RBAC) är ett auktoriseringssystem som bygger på Azure Resource Manager och som ger detaljerad åtkomsthantering av resurser i Azure. Azure har många resurser, men några exempel är virtuella datorer, webbplatser, nätverk och lagring.

#### <a name="what-is-role-based-access-control"></a>Vad är rollbaserad åtkomstkontroll?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvk]

## <a name="what-can-i-do-with-rbac"></a>Vad kan jag göra med RBAC?

Med RBAC kan du bevilja åtkomst till Azure-resurser som du kontrollerar.

Här följer några exempel:

- Tillåt en användare att hantera virtuella datorer i en prenumeration och en annan användare att hantera virtuella nätverk
- Tillåt en databasadministratörsgrupp att hantera SQL-databaser i en prenumeration
- Tillåt en användare att hantera alla resurser i en resursgrupp, till exempel virtuella datorer, webbplatser och undernät
- Tillåt att ett program får åtkomst till alla resurser i en resursgrupp

## <a name="rbac-in-the-azure-portal"></a>RBAC i Azure Portal

I flera områden i Azure Portal finns ett blad med namnet **Åtkomstkontroll (IAM)**, även kallad identitets- och åtkomsthantering. På det här bladet kan se du vilka som har åtkomst till området och deras roll. Med samma blad kan du bevilja eller ta bort åtkomst.

Nedan visas ett exempel på bladet Åtkomstkontroll (IAM) för en resursgrupp. I det här exemplet har Alain Charon tilldelats rollen som ansvarig för säkerhetskopiering för den här resursgruppen.

![Åtkomstkontroll (IAM) på Azure-portalen](../media/2-resource-group-access-control.png)

## <a name="how-does-rbac-work"></a>Hur fungerar RBAC?

Du kan styra åtkomsten till resurser med hjälp av RBAC, genom att skapa rolltilldelningar som styr hur behörigheter tillämpas. Om du vill skapa en rolltilldelning behöver du tre element: ett säkerhetsobjekt, en rolldefinition och ett omfång. Du kan tänka på de här elementen som ”vem”, ”vad” och ”var”.

### <a name="1-security-principal-who"></a>1. Säkerhetsobjekt (vem)

Ett *säkerhetsobjekt* är ett annat namn på en användare, en grupp eller ett program som du vill bevilja åtkomst för.

![En bild som visar säkerhetsobjektet, inklusive användare, grupp och tjänstens huvudnamn.](../media/2-rbac-security-principal.png)

### <a name="2-role-definition-what-you-can-do"></a>2. Rolldefinition (vad du kan göra)

En *rolldefinition* är en samling behörigheter. Ibland kallas det helt enkelt för en roll. En rolldefinition visar en lista på de behörigheter som kan utföras, till exempel läsa, skriva och ta bort. Roller kan vara på hög nivå som t.ex. Ägare, eller specifika som t.ex. Virtuell datordeltagare.

![En bild som visar en lista med olika inbyggda och anpassade roller där definitionen av deltagarrollen är inzoomad.](../media/2-rbac-role-definition.png)

Azure innehåller flera inbyggda roller som du kan använda. I följande lista finns fyra grundläggande inbyggda roller:

- Ägare – Har fullständig åtkomst till alla resurser, inklusive rätten att delegera åtkomst till andra.
- Deltagare – Kan skapa och hantera alla typer av Azure-resurser, men kan inte bevilja åtkomst till andra.
- Läsare – Kan se befintliga Azure-resurser.
- Administratör för användaråtkomst – Låter dig hantera användarnas åtkomst till Azure-resurser.

Om de inbyggda rollerna inte uppfyller organisationens specifika krav, kan du skapa egna anpassade roller.

### <a name="3-scope-where"></a>3. Omfång (var)

*Omfång* är det område som åtkomsten gäller för. Det här är användbart om du vill göra någon till en webbplatsdeltagare, men endast för en resursgrupp.

I Azure kan du ange ett omfång på flera nivåer: hanteringsgrupp, prenumeration, resursgrupp eller resurs. Omfång är strukturerade i en överordnad/underordnad relation. När du beviljar åtkomst i ett överordnat omfång, ärvs dessa behörigheter av underordnade omfång. Om du tilldelar rollen Deltagare till en grupp i prenumerationsomfånget, ärvs den rollen av alla resursgrupper och resurser i prenumerationen.

![En bild som visar en hierarkisk representation av olika Azure-nivåer som omfång kan tillämpas på. Hierarkin, som börjar på den högsta nivån, har följande ordning: hanteringsgrupp, prenumeration, resursgrupp och resurs.](../media/2-rbac-scope.png)

### <a name="role-assignment"></a>Rolltilldelning

När du har bestämt vem, vad och var, kan du kombinera dessa element för att bevilja åtkomst. En *rolltilldelning* innebär att en roll kopplas till ett säkerhetsobjekt i ett visst omfång, i syfte att bevilja åtkomst. För att bevilja åtkomst skapar du en rolltilldelning. För att återkalla åtkomst tar du bort en rolltilldelning.

I det här exemplet har marknadsföringsgruppen tilldelats rollen Deltagare i försäljningsresursgruppens omfång.

![En bild som visar ett exempel på en rolltilldelningsprocess för marknadsföringsgruppen, som är en kombination av säkerhetsobjekt, rolldefinition och omfång. Marknadsföringsgruppen ligger under gruppsäkerhetsobjektet och har tilldelats rollen som deltagare för omfånget Resursgrupp.](../media/2-rbac-overview.png)

## <a name="rbac-is-an-allow-model"></a>RBAC är en Tillåt-modell

RBAC är en Tillåt-modell. Det innebär att när du har tilldelats en roll ger RBAC dig behörighet att utföra vissa åtgärder, till exempel att läsa, skriva eller ta bort. Så om en rolltilldelning ger dig läsbehörighet till en resursgrupp och en annan rolltilldelning ger dig skrivbehörighet till samma resursgrupp, kommer du ha skrivbehörighet i den resursgruppen.

RBAC har något som kallas `NotActions`-behörigheter. `NotActions` är inte en neka-regel – det är bara ett praktiskt sätt att skapa en uppsättning tillåtna behörigheter när specifika behörigheter behöver undantas.

I det här avsnittet har du lärt dig grunderna i hur RBAC fungerar. Nu när du har RBAC-grunderna avklarade, kan du börja använda RBAC. Det enklaste sättet att komma igång på är att använda Azure Portal. I resten av den här modulen kommer du att utföra praktiska övningar som rör RBAC.