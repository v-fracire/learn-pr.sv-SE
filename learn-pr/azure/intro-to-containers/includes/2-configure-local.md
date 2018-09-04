Innan du kör en container eller ett containerintegrerat program i Azure, kommer du troligen att arbeta i en lokal utvecklingsmiljö som t.ex. din bärbara dator. Det här avsnittet förbereder din dator för utveckling av containrar.

## <a name="docker-for-windows-and-mac"></a>Docker för Windows och Mac

Docker, Inc. har publicerat två program som kan användas till att installera och konfigurera en lokal utvecklingsmiljö för containrar. I princip utrustar varje program datorn med Docker-verktyg, till exempel verktyg som krävs för CLI och automatisering. En virtuell dator skapas också som blir värd för Docker-plattformen. Miljön är konfigurerad så att Docker-kommandon skickas till den virtuella datorn. Dessa program fungerar ungefär likadant och innehåller följande funktioner:

- Docker-plattformen – Kärnkomponenter som krävs för att skapa och köra containrar.
- Docker CLI – Kommandoradsgränssnitt som interagerar med Docker-containrar
- Docker Compose – Automation-verktyg som definierar och kör program med flera containrar.

Om du vill installera Docker på datorn följer du länken för ditt operativsystem.

- Docker för Windows – https://www.docker.com/docker-windows
- Docker för Mac – https://www.docker.com/docker-mac

## <a name="docker-for-windows-environments"></a>Docker för Windows-miljöer

När du använder Docker för Windows är två miljöer tillgängliga: Linux och Windows. I Linux-miljön kan du köra Linux-containrar på ditt Windows-system. Du kan välja en miljö genom att högerklicka på ikonen Docker i aktivitetsfältet, välja alternativet att **växla till Linux-containrar** och följa anvisningarna på skärmen.

![Docker för Windows, växla till Linux-containrar](../media-draft/2-docker-linux.png)

> [!NOTE]
> I den här självstudien förutsätter vi att datorn är konfigurerad att fungera med Linux-containrar.

## <a name="docker-on-linux"></a>Docker på Linux

Om du arbetar i ett Linux-baserat system kan Dockers serverkomponenter och CLI-verktyg installeras manuellt. Följ anvisningarna i [Om Docker CE](https://docs.docker.com/install/#server) för din specifika Linux-distribution.

## <a name="validate-configuration"></a>Verifiera konfigurationen

Du kan kontrollera att Docker har installerats och konfigurerats genom att öppna en terminal och köra följande kommando:

```bash
docker search nginx
```

Om du ser utdata som liknar följande har miljön lästs för nästa avsnitt.

```bash
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

## <a name="summary"></a>Sammanfattning

I det här avsnittet förbereder du en lokal miljö för containerutveckling. I nästa avsnitt lär du dig några grundläggande Docker-åtgärder.