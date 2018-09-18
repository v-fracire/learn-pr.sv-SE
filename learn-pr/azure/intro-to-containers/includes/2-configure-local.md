<span data-ttu-id="7f5a9-101">Innan du kör en container eller ett containerintegrerat program i Azure kommer du förmodligen arbeta i en lokal utvecklingsmiljö, som din bärbara dator.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-101">Before you run a container or container-integrated application in Azure, you'll most likely work in a local development environment like your laptop.</span></span> <span data-ttu-id="7f5a9-102">I den här lektionen förbereder du ditt system för containerutveckling med Docker.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-102">This unit helps you prepare your system for container development with Docker.</span></span>

## <a name="docker-for-windows-and-mac"></a><span data-ttu-id="7f5a9-103">Docker för Windows och Mac</span><span class="sxs-lookup"><span data-stu-id="7f5a9-103">Docker for Windows and Mac</span></span>

<span data-ttu-id="7f5a9-104">Docker, Inc. har publicerat två program för att installera och konfigurera lokala miljöer för containerutveckling.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-104">Docker, Inc. has published two applications to install and configure local container development environments.</span></span> <span data-ttu-id="7f5a9-105">I princip utrustar varje program datorn med Docker-verktyg, till exempel verktyg som krävs för CLI och automatisering.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-105">Essentially, each application prepares your system with Docker tooling, such as the necessary CLI and automation tools.</span></span> <span data-ttu-id="7f5a9-106">En virtuell dator skapas också som blir värd för Docker-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-106">A virtual machine is also created that hosts the Docker platform.</span></span> <span data-ttu-id="7f5a9-107">Miljön är konfigurerad så att Docker-kommandon skickas till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-107">The environment is configured such that Docker commands are passed through to the virtual machine.</span></span> <span data-ttu-id="7f5a9-108">Dessa program fungerar ungefär likadant och innehåller följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="7f5a9-108">Each of these applications is similar in functionality and includes the following features:</span></span>

- <span data-ttu-id="7f5a9-109">**Docker-plattformen:** De kärnkomponenter som behövs för att skapa och köra containrar.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-109">**Docker platform:** The core components necessary to create and run containers.</span></span>
- <span data-ttu-id="7f5a9-110">**Docker CLI:** Kommandoradsgränssnittet för interaktion med Docker-containrar.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-110">**Docker CLI:** The command-line interface for interacting with Docker containers.</span></span>
- <span data-ttu-id="7f5a9-111">**Docker Compose:** Automationsverktyg som definierar och kör program med flera containrar.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-111">**Docker Compose:** Automation tooling for defining and running multi-container applications.</span></span>

<span data-ttu-id="7f5a9-112">Öppna motsvarande länk nedan i en ny flik för att installera Docker i ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-112">Open the appropriate link below in a new tab to install Docker on your operating system.</span></span> 

- [<span data-ttu-id="7f5a9-113">Docker för Windows</span><span class="sxs-lookup"><span data-stu-id="7f5a9-113">Docker for Windows</span></span>](https://www.docker.com/docker-windows)
- [<span data-ttu-id="7f5a9-114">Docker för Mac</span><span class="sxs-lookup"><span data-stu-id="7f5a9-114">Docker for Mac</span></span>](https://www.docker.com/docker-mac)

## <a name="docker-for-windows-environments"></a><span data-ttu-id="7f5a9-115">Docker för Windows-miljöer</span><span class="sxs-lookup"><span data-stu-id="7f5a9-115">Docker for Windows environments</span></span>

<span data-ttu-id="7f5a9-116">När du använder Docker för Windows är två miljöer tillgängliga: Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-116">When you use Docker for Windows, two environments are available: Linux and Windows.</span></span> <span data-ttu-id="7f5a9-117">I Linux-miljön kan du köra Linux-containrar på ditt Windows-system.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-117">Using the Linux environment allows you to run Linux containers on your Windows system.</span></span> <span data-ttu-id="7f5a9-118">Du kan välja en miljö genom att högerklicka på ikonen Docker i aktivitetsfältet, välja alternativet att **växla till Linux-containrar** och följa anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-118">You can select an environment by right-clicking on the Docker task bar icon, selecting **Switch to Linux containers**, and following the on-screen prompts.</span></span>

![Docker för Windows, växla till Linux-containrar](../media-draft/2-docker-linux.png)

> [!NOTE]
> <span data-ttu-id="7f5a9-120">I den här självstudien förutsätter vi att datorn är konfigurerad att fungera med Linux-containrar.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-120">The steps in this tutorial assume that your system is configured to work with Linux containers.</span></span>

## <a name="docker-on-linux"></a><span data-ttu-id="7f5a9-121">Docker på Linux</span><span class="sxs-lookup"><span data-stu-id="7f5a9-121">Docker on Linux</span></span>

<span data-ttu-id="7f5a9-122">Om du arbetar i ett Linux-baserat system kan Dockers serverkomponenter och CLI-verktyg installeras manuellt.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-122">If you're working on a Linux-based system, the Docker server components and CLI tools can be manually installed.</span></span> <span data-ttu-id="7f5a9-123">Följ anvisningarna i [Om Docker CE](https://docs.docker.com/install/#server) för din specifika Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-123">Follow the instructions found on [About Docker CE](https://docs.docker.com/install/#server) for your specific Linux distribution.</span></span>

## <a name="validate-configuration"></a><span data-ttu-id="7f5a9-124">Verifiera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="7f5a9-124">Validate configuration</span></span>

<span data-ttu-id="7f5a9-125">Du kan kontrollera att Docker har installerats och konfigurerats genom att öppna en terminal och köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7f5a9-125">To validate that Docker has been successfully installed and configured, open a terminal and run the following command:</span></span>

```bash
docker search nginx
```

<span data-ttu-id="7f5a9-126">Om du ser utdata som liknar följande är miljön redo för nästa lektion.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-126">If you see output similar to the following, your environment is ready for the next unit.</span></span>

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

## <a name="summary"></a><span data-ttu-id="7f5a9-127">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7f5a9-127">Summary</span></span>

<span data-ttu-id="7f5a9-128">I det här avsnittet förbereder du en lokal miljö för containerutveckling.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-128">In this unit, you prepared a local container development environment.</span></span> <span data-ttu-id="7f5a9-129">I nästa avsnitt lär du dig några grundläggande Docker-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7f5a9-129">In the next unit, you will learn about some basic Docker operations.</span></span>