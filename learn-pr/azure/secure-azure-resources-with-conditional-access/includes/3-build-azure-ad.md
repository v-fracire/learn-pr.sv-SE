Du bestämmer dig för att distribuera Azure AD och använda principer för villkorsstyrd åtkomst som gör att Azure kräver multifaktorautentisering när någon använder Azure-portalen. Du behöver skapa en katalog och använda tillfälliga licenser.

## <a name="create-a-directory"></a>Skapa en katalog
Vi skapar en ny katalog för First Up Consultants, där vi kan testa utan att det påverkar produktionsanvändarna.

1. Logga in på [Azure-portalen](https://portal.azure.com/).
1. I det vänstra navigeringsfönstret klickar du på **Skapa en resurs** > **Identitet** > **Azure Active Directory**.
1. På bladet **Skapa katalog** anger du följande värden för **Organisationsnamn** och **Ursprungligt domännamn**:

   1. ORGANISATIONSNAMN: `First Up Consultants`.
   1. URSPRUNGLIGT DOMÄNNAMN: `firstupconsultants<XXXX>.onmicrosoft.com`.

1. Vänta tills katalogen skapas. Klicka på länken för att växla till den nya katalogen eller klicka på **Katalog- och prenumerationsfilter** längst upp i fönstret och välj sedan den nya katalogen.

## <a name="get-trial-licenses"></a>Skaffa utvärderingslicenser

För att kunna använda funktioner som villkorsstyrd åtkomst och multifaktorautentisering behöver du minst en utvärderingslicens. Följande steg visar hur du aktiverar en utvärderingslicens:

1. I Azure AD-fönsterrutan **Översikt** klickar du på länken **Starta en kostnadsfri utvärdering**.
1. Under objektet **Azure AD Premium P2** klickar du på **Kostnadsfri utvärderingsversion** och klickar sedan på **Aktivera**.

## <a name="create-a-test-user"></a>Skapa en testanvändare

Vi behöver testa det här med en användare. Isabella Simonsen (en annan medlem i teamet) har ställt upp som frivillig för att hjälpa till. Hon behöver ett konto i katalogen, så vi går igenom stegen för att skapa kontot.

1. Gå till **Azure Active Directory** > **Användare** > **Alla användare**.
1. Klicka på **Ny användare**.
1. Skapa en användare med namnet **Isabella Simonsen** med användarnamnet:

   * `Isabella@firstupconsultants<XXXX>.onmicrosoft.com`

1. Markera kryssrutan för att **Visa lösenord** för användaren. Anteckna lösenordet så att du kan använda det senare när du testar.
1. Klicka på **Skapa**.

## <a name="create-a-pilot-group"></a>Skapa en pilotgrupp

Vi kommer att tilldela den princip som vi skapar till en grupp med användare, men vi måste skapa en grupp för den här principen. Följande steg hjälper dig att skapa en säkerhetsgrupp för pilotdistributionen.

1. Gå till **Azure Active Directory** > **Grupper** > **Alla grupper**.
1. Klicka på **Ny grupp**.
1. Grupptyp **Säkerhet**.
1. Gruppnamn **CA-MFA-AzurePortal**.
1. Medlemskapstyp **Tilldelad**.
1. Välj den användare som vi skapade i föregående steg och välj **Välj**.
1. Klicka på **Skapa**.

## <a name="summary"></a>Sammanfattning

I den här enheten lärde du dig hur du skapar en katalog med utvärderingslicens, en testanvändare och en pilotgrupp i Azure-portalen.
