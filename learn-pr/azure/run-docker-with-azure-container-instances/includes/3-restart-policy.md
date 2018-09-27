Eftersom det går snabbt och är enkelt att distribuera containrar i Azure Container Instances är det en bra lösning för att köra engångsuppgifter som att skapa, testa och rendera avbildningar.

Du kan konfigurera en omstartsprincip, så du kan ange att containern ska stoppas när processen är slutförd. Eftersom du faktureras per sekund för containerinstanser debiteras du endast för de beräkningsresurser som används när containern kör dina uppgifter.

## <a name="container-restart-policies"></a>Principer för containeromstart

Azure Container Instances har tre alternativ omstartsprinciper:

| Omstartsprincip   | Beskrivning |
| ---------------- | :---------- |
| `Always` | Containrar i containergruppen startas alltid om. Den här principen passar för långvariga uppgifter, till exempel en webbserver. Det här är **standardvärdet** som används om du inte anger någon omstartsprincip när du skapar containern. |
| `Never` | Containers i containergruppen startas aldrig om. Containers körs högst en gång. |
| `OnFailure` | Containers i containergruppen startas bara om när processen som körs i containern inte slutförs utan fel (när den avslutas med en annan slutkod än noll). Containrar körs minst en gång. Den här principen fungerar bra för containrar som kör kortvariga uppgifter. |

## <a name="run-to-completion"></a>Kör till slutförande

Om du vill se omstartsprincipen i praktiken skapar du en containerinstans från avbildningen *microsoft/aci-wordcount* och anger principen *OnFailure*. Den här exempelcontainern kör ett Python-skript som analyserar texten i Shakespeares Hamlet, skriver ut de 10 vanligaste orden till STDOUT och sedan avslutas.

Kör exempelcontainern med kommandot `az container create`:

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --location eastus
```

Azure Container Instances startar containern och stoppar den sedan när appen (eller skriptet i det här fallet) avslutas. När Azure Container Instances stoppar en container vars omstartsprincip är *Aldrig* eller *OnFailure* sätts containerns status till **Avslutad**.

Du kan kontrollera statusen för en container med kommandot `az container show`:

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

När exempelcontainerns status blir **Avslutad** kan du se utdata för uppgiften i containerloggarna. Kör kommandot `az container logs` för att visa utdata för skriptet:

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo
```

Utdata:

```json
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```