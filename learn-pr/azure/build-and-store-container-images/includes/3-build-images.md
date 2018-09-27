Anta att ditt företag använder sig av containeravbildningar för att hantera beräkningsbelastningar. Du kan använda de lokala Docker-verktygen till att bygga dina containeravbildningar.

Nu kan du använda Azure Container Registry Tasks för att skapa dessa containrar. I Container Registry Tasks kan du även integrera DevOps-processer med automatiserat skapande vid källkodsincheckning.

Låt oss automatisera skapandet av en containeravbildning med Azure Container Registry Tasks.

## <a name="create-a-container-image-with-azure-container-registry-tasks"></a>Skapa en containeravbildning med Azure Container Registry Tasks

En Docker-standardfil ger kompileringsinstruktioner. I Azure Container Registry Tasks kan du återanvända valfri Docker-fil som finns i din miljö, inklusive kompileringar i flera steg.

Vi använder en ny Docker-fil i vårt exempel.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

Det första steget är att skapa en ny fil med namnet `Dockerfile`. Du kan redigera filen i valfri textredigerare. Vi använder Cloud Shell Editor i det här exemplet. Öppna redigeringsprogrammet genom att ange följande kommando i Cloud Shell-fönstret.

```bash
code
```

Kopiera följande innehåll till din nya Docker-fil. Var noga med att spara filen.

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

Spara genom att använda tangentkombinationen <kbd>Ctrl+S</kbd>. Ge filen namnet `Dockerfile` när du tillfrågas.

Den här konfigurationen lägger till ett Node.js-program i `node:9-alpine`-avbildningen. Därefter konfigureras containern för att leverera programmet via port 80 med instruktionen *EXPOSE*.

Skapa nu containeravbildningen från Docker-filen genom att köra Azure CLI-kommandot `az acr build`.

```azurecli
az acr build --registry <acrName> --image helloacrtasks:v1 .
```

När det här kommandot körs ser du att avbildningen skapas och push-överförs till ditt containerregister.

## <a name="verify-the-image"></a>Verifiera avbildningen

Kör följande kommando för att verifiera att avbildningen har skapats och lagrats i registret.

```azurecli
az acr repository list --name <acrName> --output table
```

Utdata bör se ut ungefär så här:

```console
Result
-------------
helloacrtasks
```

Avbildningen `helloacrbuild` är nu redo att användas.
