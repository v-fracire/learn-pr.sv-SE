Första sakerna först vi måste se till att du har konfigurerat några konton och har starter-koden som klonas lokalt.

## <a name="create-an-azure-account"></a>Skapa ett Azure-konto

Du behöver ett konto på Azure. Om du inte redan har en så kan du registrera dig och få en kostnadsfri år leverantör av tjänster genom att följa den här länken:

[Skapa Azure-konto](https://azure.microsoft.com/free)

## <a name="create-a-slack-workspace"></a>Skapa en Slack-arbetsyta

Om du vill skapa ett slack-kommando, behöver du administratörsrättigheter på en slack-arbetsyta.

Så du antingen måste du kontrollera att du har administratörsrättigheter på en befintlig arbetsyta eller skapa en helt ny slack arbetsyta som det:

[Skapa Slack arbetsyta](https://slack.com/create)

## <a name="clone-the-starter-code"></a>Klona starter-koden

För att få ut bäst av den här modulen kan rekommenderar jag att du klonar starter-koden och följa de stegvisa anvisningarna.

Vi ger den kod som du behöver för att slutföra programmet samt vissa inledande bootstrap-kod att komma igång.

Att börja klona detta _stater_ kod till din lokala dator.

```bash
git clone https://github.com/jawache/mojifier-slack.git
```

Sedan installera de nödvändiga paketen med:

```bash
npm install
```

Vi rekommenderar att du följer den här modulen steg för steg. Men om du kör fast och behöver sedan du hittar den färdiga koden i den `completed` gren, t.ex:

```bash
git checkout completed
```

Vi ska skriva våra program med hjälp av `TypeScript`. NodeJS inte vet hur du **kör** `TypeScript`, så när vi utvecklar vi måste konvertera vår `TypeScript` koda till `JavaScript`. Alla nödvändiga verktyg och utföra konverteringen har installerats i ovanstående steg för att konvertera den `TypeScript` kör du följande kommando:

```bash
npm run build
```

Om ovanstående kommando har körts har vid den `*.ts` filer, bör nu även se `*.js` och `*.map` filer.

> **Obs!**
>
> Håll det här kommandot som körs i en terminal shell, detta söker efter ändringar i TypeScript-filer och konverterar dem till JavaScript-filer.
>
> Om du av någon anledning filerna inte är komma konverteras du sedan kontrollera konsolens utdata i terminalfönstret genom finnas det fel i TypeScript-kod.

## <a name="install-visual-studio-code--azure-functions-extention"></a>Installera Visual Studio Code och Azure Functions-tillägg

Det finns många sätt som du kan interagera med Azure, i den här modulen som vi interagerar med Azure med hjälp av Visual Studio Code och Azure Functions Extension associerade plugin-programmet.

1. Ladda ned och installera visual studio code från https://code.visualstudio.com/

2. Installera Azure Functions Code-tillägg genom att följa instruktionerna här: https://code.visualstudio.com/tutorials/functions-extension/getting-started
