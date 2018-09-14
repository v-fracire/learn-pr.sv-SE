Anta att du måste välja ett verktyg för att administrera Azure-resurserna som används för att testa systemet för hantering av kundrelationer (CRM). Dina tester måste du skapa resursgrupper och etablera virtuella datorer (VM).

Vill du något som är lätt för administratörer att lära dig, men kraftfullt nog för att automatisera installationen och konfigurationen av flera virtuella datorer eller skriva en fullständig programmiljö. Flera olika verktyg finns tillgängliga. Du måste hitta den bästa för din personal och dina aktiviteter.

## <a name="what-tools-are-available"></a>Vilka verktyg är tillgängliga?
Azure tillhandahåller tre administrationsverktyg som du kan välja mellan:

- Azure-portalen
- Azure CLI
- Azure PowerShell

Alla erbjuder ungefär samma mängd kontroll. Alla aktiviteter som du kan göra med ett av verktygen kan du förmodligen även göra med de andra två. Alla tre verktyg är plattformsoberoende och kan köras på Windows, macOS och Linux. De skiljer sig åt i syntax, konfigurationskrav och om de stöder automatisering eller ej.

Vi kommer här att beskriva var och ett av de tre alternativen samt ge vägledning om hur du kan välja bland dem. 

## <a name="what-is-the-azure-portal"></a>Vad är Azure-portalen?
Azure-portalen är en webbplats som låter dig skapa, konfigurera och ändra resurser i din Azure-prenumeration. Portalen är ett grafiskt användargränssnitt (GUI) som gör det enklare att hitta de resurser du behöver och utföra de ändringar som behövs. Den vägleder dig även genom avancerade administrativa uppgifter genom att tillhandahålla guider och knappbeskrivningar.

Portalen tillhandahåller inte några sätt att automatisera repetitiva uppgifter. Om du till exempel vill konfigurera 15 virtuella datorer behöver du skapa dem en i taget genom att slutföra guiden för varje virtuell dator. Detta kan vara ett tidskrävande och felbenäget tillvägagångssätt för komplexa uppgifter. 

## <a name="what-is-the-azure-cli"></a>Vad är Azure CLI?
Azure CLI är ett plattformsoberoende kommandoradsprogram som används för att ansluta till Azure och köra administrativa kommandon på Azure-resurser. Om du till exempel skulle vilja skapa en virtuell dator skulle du använda ett kommando som följande:

```azurecli
az vm create \
  --resource-group CrmTestingResourceGroup \
  --name CrmUnitTests \
  --image UbuntuLTS
  ...
```

Azure CLI finns tillgängligt på två sätt: i en webbläsare via Azure Cloud Shell eller via en lokal installation på Linux, Mac eller Windows. I båda fallen kan det användas interaktivt eller skriptas. För interaktiv användning startar du först ett gränssnitt som `cmd.exe` på Windows eller Bash i Linux eller macOS och sedan anger du kommandot i gränssnittets kommandotolk. Om du vill automatisera repetitiva uppgifter sätter du samman kommandon till ett kommandoskript med hjälp av skriptsyntax för det valda gränssnittet och kör skriptet.

## <a name="what-is-azure-powershell"></a>Vad är Azure PowerShell?
Azure PowerShell är en modul som du lägger till i Windows PowerShell eller PowerShell Core så att du kan ansluta till din Azure-prenumeration och hantera resurser. Azure PowerShell kräver PowerShell för att fungera. PowerShell tillhandahåller tjänster som gränssnittsfönster, kommandoparsning och så vidare. Azure PowerShell lägger till Azure-specifika kommandon.

Till exempel Azure PowerShell tillhandahåller den **New-AzureRmVM** kommando som skapar en virtuell dator åt dig i din Azure-prenumeration. För att använda det startar du PowerShell-programmet och sedan anger ett kommando så här:

```powershell
New-AzureRmVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Azure PowerShell finns tillgängligt på två sätt: i en webbläsare via Azure Cloud Shell eller via en lokal installation på Linux, Mac eller Windows. I båda fallen kan du välja mellan två lägen. Du kan använda det i interaktivt läge, där du manuellt anger ett kommando i taget, eller i skriptläge, där du kör ett skript som består av flera kommandon.

## <a name="how-to-choose-an-administrative-tool"></a>Så här väljer du ett administrativt verktyg
Portalen, Azure CLI och Azure PowerShell är ungefär likvärdiga med avseende på de Azure-objekt som de kan administrera och de konfigurationer de kan skapa. Alla tre är även plattformsoberoende. Det innebär att du vanligtvis kommer att överväga flera andra faktorer när du gör ditt val:

- **Automatisering**: Behöver du automatisera en uppsättning komplexa eller repetitiva uppgifter? Azure PowerShell och Azure CLI har stöd för detta men inte portalen.

- **Inlärningskurva**: Behöver du slutföra en uppgift snabbt utan att lära dig nya kommandon och syntax? Azure-portalen kräver inte att du lär dig syntax eller memorerar kommandon. Du måste känna till den detaljerade syntaxen för varje kommando som du använder i Azure PowerShell och Azure CLI.

- **Gruppfärdigheter**: Har ditt team befintlig expertis? Ditt team kan till exempel ha använt PowerShell för att administrera Windows. I så fall kommer de snabbt vänja sig vid att använda Azure PowerShell.

## <a name="example"></a>Exempel
Kom ihåg att du väljer ett administrativt verktyg för att skapa testmiljöer för ditt CRM-program. Administratörerna har två specifika Azure-uppgifter som de behöver utföra:

1. Skapa en resursgrupp för varje kategori för testning (enhet, integrering och godkännande).
2. Skapa flera virtuella datorer i varje resursgrupp före varje omgång av testning.

Azure-portalen är ett rimligt val om du vill skapa resursgrupper. Det här är engångsuppgifter, så du inte behöver inte använda skript för att utföra dem.

Att hitta det bästa verktyget för att skapa de virtuella datorerna är mer utmanande. Du måste skapa flera av dem och du måste du göra det upprepade gånger, förmodligen flera gånger i veckan. Det här innebär att du behöver automatisering, så Azure-portalen inte är ett bra alternativ. I det här fallet passar Azure PowerShell eller Azure CLI dina behov. Om dina gruppmedlemmar har någon befintlig kunskap om PowerShell så kommer Azure PowerShell antagligen att vara det bästa valet. Azure PowerShell är tillgängligt på operativsystem som ditt administratörsteam använder, det har stöd för automatisering och bör vara enkelt för ditt team att lära sig.

## <a name="summary"></a>Sammanfattning
De flesta administratörers första erfarenhet av Azure är i portalen. Det är en bra plats att börja på eftersom det ger dig ett rent, välstrukturerat grafiskt gränssnitt, men alternativen för automatisering är begränsade. När du behöver automatisering har Azure två alternativ: Azure PowerShell för administratörer med erfarenhet av PowerShell och Azure CLI för alla andra.

I praktiken har de flesta företag en blandning av engångsuppgifter och repetitiva uppgifter. Det här innebär att det är vanligt att både använda portalen och en skriptlösning. I vårt CRM-exempel är det lämpligt att skapa resursgrupper via portalen och automatisera skapandet av virtuella datorer med PowerShell.

Resten av den här modulen fokuserar på att installera och använda Azure PowerShell.
