Eftersom det går snabbt att distribuera containers i Azure Container Instances är det en bra plattform för att köra engångsuppgifter som att skapa, testa och återge avbildningar i en containerinstans.

Du kan konfigurera en omstartsprincip, så du kan ange att containern ska stoppas när processen är slutförd. Eftersom du faktureras per sekund för containerinstanser debiteras du endast för de beräkningsresurser som används när containern kör dina uppgifter.

## <a name="container-restart-policies"></a>Principer för containeromstart

När du skapar en container i Azure Container Instances kan ange du en av tre principinställningar för omstarter:

| Omstartsprincip   | Beskrivning |
| ---------------- | :---------- |
| `Always` | Containers i containergruppen startas alltid om. Det här är **standardvärdet** som används om du inte anger någon omstartsprincip när du skapar containern. |
| `Never` | Containers i containergruppen startas aldrig om. Containers körs högst en gång. |
| `OnFailure` | Containers i containergruppen startas bara om när processen som körs i containern inte slutförs utan fel (när den avslutas med en annan slutkod än noll). Containrar körs minst en gång. |

I föregående enhet i den här modulen skapade du en container utan någon angiven omstartsprincip. Som standard fick den här containern omstartsprincipen *Alltid*. Eftersom arbetsbelastningen i containern körs länge (en webbserver) är den här principen lämplig.

## <a name="run-to-completion"></a>Kör till slutförande

Om du vill se omstartsprincipen i praktiken skapar du en containerinstans från avbildningen *microsoft/aci-wordcount* och anger principen *OnFailure*. Den här exempelcontainern kör ett Python-skript som analyserar texten i Shakespeares Hamlet, skriver ut de 10 vanligaste orden till STDOUT och sedan avslutas.

Kör exempelcontainern med kommandot `az container create`:

```azureclu
az container create \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances startar containern och stoppar den när appen (eller skriptet i det här fallet) avslutas. När Azure Container Instances stoppar en container vars omstartsprincip är *Aldrig* eller *OnFailure* sätts containerns status till **Avslutad**.

Du kan kontrollera statusen för en container med kommandot `az container show`:

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

När exempelcontainerns status blir **Avslutad** kan du se utdata för uppgiften i containerloggarna. Kör kommandot **az container logs** för att visa utdata för skriptet:

```azurecli
az container logs --resource-group myResourceGroup --name mycontainer-restart-demo
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

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten skapade du en containerinstans med omstartsprincipen *OnFailure*. Den här konfigurationen fungerar bra för containers som kör kortvariga aktiviteter.

I nästa utbildningsenhet ställer du in miljövariabler för Azure Container Instances.
