Ditt företag använder sig av containeravbildningar för att hantera företagets beräkningsbelastningar. Du kan använda lokala Docker-verktyg till att bygga dina containeravbildningar. Beslutet att använda ett Azure Container Registry gör att du kan skapa containeravbildningarna i molnet. 

Nu kan du skapa dina containrar med Azure Container Registry Build. I Container Registry Build kan du även integrera DevOps-processer med automatiserad kompilering när källkod checkas in.

Låt oss automatisera skapandet av en containeravbildning med Azure Container Registry Build.

## <a name="create-a-container-image-with-azure-container-registry-build"></a>Skapa en containeravbildning med Azure Container Registry Build

Du kan använda en Docker-standardfil till att ge kompileringsinstruktioner. I Azure Container Registry Build kan du återanvända valfri Docker-fil som finns i din miljö, inklusive kompileringar i flera steg.

Vi använder en ny Docker-fil i vårt exempel. 

Det första steget är att skapa en ny fil med namnet `Dockerfile`. Du kan redigera filen i valfri textredigerare. Vi använder Visual Studio Code i det här exemplet.

```bash
code Dockerfile
```

Kopiera följande innehåll till din nya Docker-fil. Se till att säkra filen. 

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

Den här konfigurationen lägger till ett Node.js-program i avbildningen `node:9-alpine`. Sedan konfigureras containern för att leverera programmet på port 80 via instruktionen *EXPOSE*.

Kör nu Azure CLI-kommandot `az acr build` för att skapa containeravbildningen från Docker-filen.

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
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
helloacrbuild
```

Avbildningen `helloacrbuild` är nu redo att användas.
