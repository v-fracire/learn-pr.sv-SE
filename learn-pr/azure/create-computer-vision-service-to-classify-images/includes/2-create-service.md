I den här lektionen skapar du en tjänst för API:t för visuellt innehåll med hjälp av Azure CLI.

# <a name="exercise-create-a-computer-vision-service"></a>Övning: Skapa en tjänst för visuellt innehåll

[Visuellt innehåll](/azure/cognitive-services/computer-vision/home) ingår i [Microsoft Cognitive Services](/azure/cognitive-services/welcome), som är en fullständig uppsättning maskininlärningsalgoritmer som är tillgänglig som en tjänst för allmän användning. I stället för att du ska behöva generera tillräckligt med data för att träna en modell från grunden så tillhandahåller vi förtränade modeller för allmän användning.

Bland de många tjänsterna vi gör tillgängliga finns tjänster som kan hantera röst, bild, video, sökning, språk och mer.

I den här övningen ska vi fokusera på att skapa tjänsten som gör det möjligt för oss att hantera bilder. Efter den här övningen kommer du att kunna skapa en API-slutpunkt som kan identifiera bilder.

# <a name="create-a-computer-vision-api-service"></a>Skapa en tjänst för API för visuellt innehåll

Innan du skapar tjänsten för API för visuellt innehåll måste du ha en *resursgrupp* att distribuera den till. En resursgrupp är en logisk samling där alla Azure-resurser distribueras och hanteras.

```azurecli
az group create --name ComputerVisionRG --location westus2
```

När du har skapat resursgruppen skapar du en tjänst med kommandot `az cognitiveservices account create` och namnet på din tjänst. 

```azurecli
az cognitiveservices account create --kind ComputerVision -l westus2 -n ComputerVisionService --sku F0 -g ComputerVisionRG
```
