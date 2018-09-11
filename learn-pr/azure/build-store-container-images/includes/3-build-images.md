Med Azure Container Registry Build kan du skapa containeravbildningar i molnet utan att behöva några lokala Docker-verktyg. I Container Registry Build kan du även integrera DevOps-processer med automatiserad kompilering när källkod bekräftas.

I den här utbildningsenheten kommer du att skapa en containeravbildning automatiskt med Azure Container Registry Build.

## <a name="create-a-container-image-with-build"></a>Skapa en containeravbildning med Build

I Azure Container Registry Build anger du skapandeinstruktioner i en vanlig Dockerfile. Du kan använda valfri Dockerfile som för närvarande används i din miljö i Azure Container Registry Build, även om skapandet görs i flera steg.

Skapa en fil med namnet `Dockerfile` och öppna den i en textredigerare.

```bash
code Dockerfile
```

Kopiera följande innehåll, spara filen och avsluta textredigeraren. Denna Dockerfile lägger till en Node.js-app i avbildningen `node:9-alpine` och konfigurerar containern så att appen körs via port 80.

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

Kör följande kommando för att skapa containeravbildningen från din Dockerfile.

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

När den här åtgärden körs ser du att avbildningen skapas och push-överförs till containerregistret.

## <a name="verify-the-image"></a>Verifiera avbildningen

När skapandeåtgärden har slutförts kör du följande kommando för att verifiera att avbildningen har skapats och lagras i registret.

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

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten använde du Azure Container Registry Build till att skapa en containeravbildning automatiskt. Avbildningen lagrades sedan i containerregistret i Azure. I nästa utbildningsenhet distribuerar du en instans av den nya containeravbildningen till Azure Container Instances.