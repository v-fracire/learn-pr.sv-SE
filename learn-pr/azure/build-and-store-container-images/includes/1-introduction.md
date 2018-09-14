Azure Container Registry är en hanterad Docker-registertjänst som baseras på Docker Registry 2.0. Container Registry är privat, körs i Azure och gör att du kan skapa, lagra och hantera avbildningar för alla typer av containerdistribution.

Du kan push-överföra och hämta containeravbildningar i Container Registry med hjälp av Docker CLI eller Azure CLI. Integreringen med Azure-portalen gör att du kan inspektera containeravbildningarna visuellt i containerregistret. I distribuerade miljöer kan du använda funktionen för georeplikering i Container Registry till att distribuera containeravbildningar till olika Azure-datacenter i en lokaliserad distribution.

Förutom att lagra containeravbildningar kan du även skapa containeravbildningar i Azure med Azure Container Registry Build. I Build används en vanlig Dockerfile till att skapa och lagra en containeravbildning i Azure Container Registry utan att du behöver några lokala Docker-verktyg. Med Azure Container Registry Build kan du skapa på begäran eller automatisera containeravbildningarna med hjälp av DevOps-processer och verktyg.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att:

- Skapa ett Azure-containerregister
- Skapa en containeravbildning med Azure Container Registry Build
- Distribuera den här containern till en Azure-containerinstans
- Replikera containeravbildningen till flera Azure-datacenter