Innan du kör en container eller ett containerintegrerat program i Azure, kommer du troligen att arbeta i en lokal utvecklingsmiljö som t.ex. din bärbara dator. Här kan förbereder du datorn för utveckling av behållare med Docker.

## <a name="why-use-containers"></a>Varför använda behållare?

Behållare och behållaravbildningar använda effektivt värdresurser som diskutrymme, minne och CPU. Detta innebär att det går snabbt att starta containrar. I vissa fall startas en ny instans av en container nästan omedelbart. Detta gör att snabb etablering av program och gör en ny modell för på begäran bearbetning och skala åtgärder.

Tänk dig det här scenariot: Du kör en satsvis bearbetning som ibland har ökad efterfrågan. Behållare kan du kan skapa ett system som reagerar på ökad efterfrågan genom att snabbt etablera nya instanser. Det är kraftfullt och inte lika enkelt att uppnå med traditionella virtuella datorer.

Behållare kan du få ”hyper densitet”. Det innebär att du kan köra flera program och processer med mindre virtuella eller fysiska resurser.

Även om containrar är en utmärkt plattform för att köra traditionella arbetsbelastningar som exempelvis webbservrar, kan de också användas till anpassningsbar satsvis bearbetning, program som skapats med en modern och distribuerad arkitektur och allt det som krävs vid skalning på begäran.

## <a name="docker-for-windows-and-mac"></a>Docker för Windows och Mac

Docker, Inc. har publicerat två program för att installera och konfigurera lokal behållare utvecklingsmiljöer. Programmen Förbered datorn med Docker verktyg, till exempel nödvändiga verktyg för CLI och automatisering. En virtuell dator skapas också som blir värd för Docker-plattformen. Miljön är konfigurerad så att Docker-kommandon skickas till den virtuella datorn. Dessa program fungerar ungefär likadant och innehåller följande funktioner:

- **Docker-plattformen:** kärnkomponenter som krävs för att skapa och köra behållare.
- **Docker CLI:** kommandoradsgränssnittet för att interagera med Docker-behållare.
- **Docker Compose:** automatiseringsverktyg för att definiera och köra program med flera behållare.

Öppna länken nedan i en ny flik för att installera Docker på ditt operativsystem. 

- [Docker för Windows](https://www.docker.com/docker-windows)
- [Docker för Mac](https://www.docker.com/docker-mac)

## <a name="docker-for-windows-environments"></a>Docker för Windows-miljöer

När du använder Docker för Windows är två miljöer tillgängliga: Linux och Windows. I Linux-miljön kan du köra Linux-containrar på ditt Windows-system. Du kan välja en miljö genom att högerklicka på ikonen Docker i aktivitetsfältet, välja alternativet att **växla till Linux-containrar** och följa anvisningarna på skärmen.

> [!NOTE]
> I den här självstudien förutsätter vi att datorn är konfigurerad att fungera med Linux-containrar.

## <a name="docker-on-linux"></a>Docker på Linux

Det finns för närvarande inget installationsprogrammet program för Linux. Om du arbetar på en Linux-baserat system, måste Docker-serverkomponenter och CLI-verktyg installeras manuellt. Följ instruktionerna på [om Docker CE](https://docs.docker.com/install/#server) för din specifika Linux-distribution.

## <a name="validate-configuration"></a>Verifiera konfigurationen

Du kan kontrollera att Docker har installerats och konfigurerats genom att öppna en terminal och köra följande kommando:

```bash
docker search nginx
```

Om du ser utdata som liknar följande din miljö är redo för nästa enhet.

```output
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                                                  Official build of Nginx.                        9034                [OK]
jwilder/nginx-proxy                                    Automated Nginx reverse proxy for docker con…   1362                                    [OK]
richarvey/nginx-php-fpm                                Container running Nginx + PHP-FPM capable of…   589                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion                 LetsEncrypt container to use with nginx as p…   390                                     [OK]
kong                                                   Open-source Microservice & API Management la…   204                 [OK]
webdevops/php-nginx                                    Nginx with PHP-FPM                              106                                     [OK]
kitematic/hello-world-nginx                            A light-weight nginx container that demonstr…   102
zabbix/zabbix-web-nginx-mysql                          Zabbix frontend based on Nginx web-server wi…   59                                      [OK]
bitnami/nginx                                          Bitnami nginx Docker Image                      54                                      [OK]
linuxserver/nginx                                      An Nginx container, brought to you by LinuxS…   37
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5   ubuntu-16-nginx-php-phpmyadmin-mysql-5          36                                      [OK]
tobi312/rpi-nginx                                      NGINX on Raspberry Pi / armhf                   20                                      [OK]
nginxdemos/nginx-ingress                               NGINX Ingress Controller for Kubernetes . Th…   11
wodby/drupal-nginx                                     Nginx for Drupal container image                9                                       [OK]
blacklabelops/nginx                                    Dockerized Nginx Reverse Proxy Server.          9                                       [OK]
webdevops/nginx                                        Nginx container                                 8                                       [OK]
nginxdemos/hello                                       NGINX webserver that serves a simple page co…   7                                       [OK]
centos/nginx-18-centos7                                Platform for running nginx 1.8 or building n…   6
1science/nginx                                         Nginx Docker images that include Consul Temp…   4                                       [OK]
centos/nginx-112-centos7                               Platform for running nginx 1.12 or building …   3
pebbletech/nginx-proxy                                 nginx-proxy sets up a container running ngin…   2                                       [OK]
toccoag/openshift-nginx                                Nginx reverse proxy for Nice running on same…   1                                       [OK]
travix/nginx                                           NGinx reverse proxy                             1                                       [OK]
ansibleplaybookbundle/nginx-apb                        An APB to deploy NGINX                          0                                       [OK]
mailu/nginx                                            Mailu nginx frontend                            0                                       [OK]
```