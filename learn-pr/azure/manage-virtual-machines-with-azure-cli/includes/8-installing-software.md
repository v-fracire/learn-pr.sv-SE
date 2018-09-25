Det sista vi ska prova i vår virtuella dator är att installera en webbserver. En av de enklaste paketen att installera är `nginx`.

## <a name="install-nginx-web-server"></a>Installera NGINX-webbserver

1. Leta reda på den offentliga IP-adressen till din virtuella Linux-dator. Kom ihåg att du kan använda kommandot `vm list-ip-addresses` till att leta upp den.

1. Därefter öppnar du en `ssh`-anslutning till datorn, precis som du gjorde när vi testade den. Kom ihåg att du måste kunna ange administratörsnamnet (”**aldis**”).

1. I gränssnittet som visas kör du följande kommando för att installera `nginx`-webbservern.

```bash
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. Avsluta Secure Shell.

```bash
exit
```

## <a name="retrieve-our-default-page"></a>Hämta standardsidan

1. I Azure Cloud Shell använder du `curl` till att läsa standardsidan från din Linux-webbserver. Du kan också öppna en ny webbläsarflik och bläddra till den offentliga IP-adressen.

```azurecli
curl 40.83.165.85
```

Detta kommer att misslyckas eftersom den virtuella Linux-datorn inte visar port 80 (`http`) genom den inbyggda brandväggen. Som tur är har Azure CLI ett kommando för detta: `vm open-port`. 

1. Skriv följande i Cloud Shell för att öppna port 80:

```azurecli
az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM
```

Det tar en stund att lägga till nätverksregeln och öppna porten genom brandväggen. Prova `curl` igen. Den här gången bör den returnera data. Du kan se sidan i en webbläsare också.

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx on Debian!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx on Debian!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working on Debian. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a></p>

<p>
      Please use the <tt>reportbug</tt> tool to report bugs in the
      nginx package with Debian. However, check <a
      href="http://bugs.debian.org/cgi-bin/pkgreport.cgi?ordering=normal;archive=0;src=nginx;repeatmerged=0">existing
      bug reports</a> before reporting a new bug.
</p>

<p><em>Thank you for using debian and nginx.</em></p>


</body>
</html>
```
