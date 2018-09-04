Det sista vi ska prova i vår virtuella dator är att installera en webbserver. En av de enklaste paketen att installera är `nginx`.

1. Leta reda på den offentliga IP-adressen till din virtuella Linux-dator. Kom ihåg att du kan använda kommandot `vm list-ip-addresses` till att leta upp den.

1. Därefter öppnar du en `ssh`-anslutning till datorn, precis som du gjorde när vi testade den. Kom ihåg att du måste kunna ange administratörsnamnet (”**aldis**”).

1. I gränssnittet som visas kör du följande kommando för att installera `nginx`-webbservern.

```azurecli
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. I Azure Cloud Shell använder du `curl` till att läsa standardsidan från din Linux-webbserver. Du kan också öppna en ny webbläsarflik och bläddra till den offentliga IP-adressen.

```azurecli
curl 168.61.54.62
```

Detta kommer att misslyckas eftersom den virtuella Linux-datorn inte visar port 80 (`http`) genom den inbyggda brandväggen. Som tur är har Azure CLI ett kommando för detta: `vm open-port`. 

1. Skriv följande i Cloud Shell för att öppna port 80:

```
az vm open-port --port 80 --resource-group ExerciseResources --name SampleVM
```

Försök slutligen med `curl` igen. Den här gången bör den returnera data. Du kan se sidan i en webbläsare också.
