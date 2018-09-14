Azure Active Directory (Azure AD) är inte bara en domänkontrollant i molnet. Azure AD är en molnbaserad katalog- och identitetshanteringstjänst för flera klientorganisationer. Den kombinerar viktiga katalogtjänster, åtkomsthantering för program och identitetsskydd i en och samma lösning. Azure AD kan arbeta tillsammans med en befintlig Windows Server Active Directory-miljö via Azure AD Connect, vilket gör att du kan utnyttja befintliga lokala identitetsinvesteringar.

Azure AD finns i fyra versioner. I den här övningen fokuserar vi endast på funktioner i versionerna Azure AD Premium P1 och P2.

I den här modulen skapar vi en Azure AD-katalog där alla våra användare och grupper kommer att finnas, på ett liknande sätt som en lokal katalog.

När vi skapar katalogen blir vår konto automatiskt en global administratör. Konton med behörighet som global administratör bör regleras eftersom de har fullständig åtkomst till alla administrativa funktioner i Azure AD. Du kan ha fler än en global administratör, men det är endast globala administratörer som kan tilldela de andra administratörsrollerna till användare.

## <a name="how-can-azure-ad-help-you-protect-access-to-applications"></a>Hur kan Azure AD hjälpa dig att skydda åtkomsten till program?

Azure AD Premium innehåller funktioner som multifaktorautentisering och principer för villkorsstyrd åtkomst. När dessa funktioner används tillsammans ger de högst kornighet när de skyddar åtkomst till program.

En princip för villkorsstyrd åtkomst består av:

- Användare eller grupper
- Program som de försöker få åtkomst till
- Kontroller som ska uppfyllas, till exempel multifaktorautentisering

## <a name="summary"></a>Sammanfattning

I den här enheten har du lärt dig grunderna om Azure AD och vilka funktioner som är tillgängliga för säker åtkomst till program. Nu när du har grunderna undan den nu sätter vi igång med Azure AD. Resten av den här modulen innehåller praktiska övningar så att du kan aktivera och testa multifaktorautentisering med villkorsstyrd åtkomst.
