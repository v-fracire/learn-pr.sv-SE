## <a name="creating-key-vaults-for-your-applications"></a>Skapa nyckelvalv för dina program

Det är bra att ge skapa ett separat valv för varje distributionsmiljö för varje program, till exempel utveckling, testning och produktion. Det är möjligt att använda valv för att dela hemligheter mellan flera appar, men risken för en angripare som får läsåtkomst till ett valv ökar med antalet hemligheter i valvet.

> [!TIP]
> Om du använder samma namn för hemligheter i olika miljöer för ett program, kommer den enda miljöspecifika konfiguration som behöver ändras i din app vara valvets URL.

Det krävs ingen inledande konfiguration för att skapa ett valv. Din användaridentitet beviljas automatiskt en full uppsättning med behörigheter för hemlighetshantering och du kan börja lägga till hemligheter omedelbart. När du har ett valv kan du lägga till och hantera hemligheter från alla Azure-administrationsgränssnitt, inklusive Azure-portalen, Azure CLI och Azure PowerShell. När du konfigurerar programmet till att använda valvet måste du tilldela korrekt behörighet, vilket vi tittar närmare på i nästa enhet.

## <a name="vault-authentication-and-permissions"></a>Valvautentisering och behörigheter

API:n för Azure Key Vaults använder Azure Active Directory till att autentisera användare och program. Principerna för valvåtkomst baseras på *åtgärder* och tillämpas i hela valvet. Till exempel kan ett program med behörigheterna **Hämta** (läsa hemligheter), **Lista** (göra en lista med namnen på alla hemligheter) och **Ange** (skapa eller uppdatera hemligheter) för ett valv skapa hemligheter, visa en lista med alla namn på hemligheterna, samt hämta och ange alla hemligheter i det valvet.

*Alla* åtgärder som utförs i ett valv kräver autentisering och auktorisering &mdash; det finns inget sätt att bevilja någon form av anonym åtkomst.

> [!TIP]
> När valvåtkomst beviljas för utvecklare och appar, beviljas endast den minsta uppsättningen behörigheter som krävs. Tack vare behörighetsbegränsningarna kan man undvika incidenter på grund av kodbuggar och minska effekterna vid stulna autentiseringsuppgifter eller skadlig kod som sprids till din app.

Utvecklare kommer vanligtvis bara behöva behörigheterna **Hämta** och **Lista** i ett valvs utvecklingsmiljö. En erfaren utvecklare behöver fullständig behörighet till valvet för att kunna ändra och lägga till hemligheter när det behövs. Fullständig behörighet till produktionsmiljövalv är vanligtvis reserverade för erfaren personal.

För appar krävs vanligtvis endast behörigheten **Hämta**. Vissa appar kan kräva **Lista**, beroende på hur appen implementeras. Appen som vi implementerar i den här modulövningen kräver behörigheten **Lista**, på grund av den teknik som används för att läsa hemligheter från valvet.

På grund av alla problem som företaget har haft med programhemligheter, har ledningen bett dig att skapa en liten startapp som hjälper de andra utvecklarna. Appen ska visa metodtips för att hantera hemligheter så enkelt och säkert som möjligt.

## <a name="create-the-vault-and-store-the-secret-in-it"></a>Skapa valvet och lagra hemligheten i det
Till att börja med måste du skapa ett valv och lagra en hemlighet.

###  <a name="create-the-vault"></a>Skapa valvet

Först skapar vi valvet och lagrar hemligheten i det.

[!include[](../../../includes/azure-sandbox-activate.md)]

**Namn på nyckelvalv måste vara globalt unika, så du måste välja ett unikt namn**. Valvnamn måste vara mellan 3 och 24 tecken långa och får endast innehålla alfanumeriska tecken och streck. Skriv ned det valvnamn du väljer, eftersom du behöver det i den här övningen.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Kör följande kommando i Cloud Shell för att skapa valvet. Du kan ersätta värdet `--location` om du vill placera det någon annanstans från ovanstående val.

```azurecli
az keyvault create \
    --name <your-unique-vault-name> \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --location eastus
```

När det är klart visas JSON-utdata med en beskrivning av det nya valvet.

> [!TIP]
> Kommandot använde resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>** som har skapats på förhand. När du arbetar med din egen prenumeration skulle antingen skapa en ny resursgrupp eller använda en befintlig som du har skapat tidigare.

### <a name="add-the-secret"></a>Lägga till en hemlighet

Lägg nu till hemligheten: Vår hemlighet får namnet **SecretPassword** med värdet **reindeer_flotilla**.

```azurecli
az keyvault secret set \
    --name SecretPassword \
    --value reindeer_flotilla \
    --vault-name <your-unique-vault-name>
```

Vi ska skriva koden för våra program strax, men först måste vi ta reda på lite mer om hur vår app ska autentiseras till ett valv.