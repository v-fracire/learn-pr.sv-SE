I den föregående övningen aktiverade vi utvärderingslicenser, skapade en katalog, skapade en användare och skapade en grupp för att testa vår lösning. I den här kursdelen ska vi skapa vår regel för villkorlig åtkomst för att kräva multifaktorautentisering för Azure Portal.

## <a name="enable-conditional-access-based-multi-factor-authentication"></a>Aktivera multifaktorautentisering baserat på villkorlig åtkomst

Villkorlig åtkomst gör att administratörer kan konfigurera när de vill, och när de inte vill, att något ska hända. De kan använda flera regler parallellt för att bevilja eller neka åtkomst till resurser. Här är den regel som vi behöver skapa:

**Vid åtkomst till Azure Portal – kräv multifaktorautentisering**

Följande steg beskriver steg för steg hur du skapar en regel för villkorlig åtkomst som kräver att användarna utför multifaktorautentisering när de ansluter till Azure Portal.

1. Gå till **Azure Active Directory** > **Villkorlig åtkomst**.

1. Klicka på **Ny princip**.

1. Ge principen namnet **Require MFA for Azure portal** (Kräv MFA för Azure Portal).

1. Välj **Användare och grupper** under **Tilldelningar** > **Användare och grupper**. Välj gruppen som vi skapade, **CA-MFA-AzurePortal**. Klicka på **Klar**.

1. Välj **Microsoft Azure Management** (Microsoft Azure-hantering) under **Molnappar** > **Välj appar**.

1. Välj **Kräv multifaktorautentisering** under **Åtkomstkontroller** > **Bevilja**.

1. Välj **På** för **Aktivera princip**.

1. Klicka på **Skapa**.

I den här kursdelen har du lärt dig hur du skapar en regel för villkorsstyrd åtkomst. Regeln kräver multifaktorautentisering för åtkomst till Azure Portal.