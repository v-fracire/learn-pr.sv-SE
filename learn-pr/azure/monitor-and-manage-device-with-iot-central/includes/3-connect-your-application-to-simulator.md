[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="13300-101">I praktiken ansluter du Azure IoT Central till en fysisk enhet, t.ex. en IoT-aktiverad kaffebryggare.</span><span class="sxs-lookup"><span data-stu-id="13300-101">In practice, you will connect Azure IoT Central to a physical device, i.e. an IoT enabled coffee machine.</span></span> <span data-ttu-id="13300-102">Här ska vi simulera en enhet med en Node.js-app och ansluta den till programmet Azure IoT Central.</span><span class="sxs-lookup"><span data-stu-id="13300-102">Here, you'll simualate a device with a Node.js applicatio and connect it to the Azure IoT Central application.</span></span> <span data-ttu-id="13300-103">Telemetrimätningar från den simulerade kaffebryggaren skickas till IoT Central för övervakning och analys.</span><span class="sxs-lookup"><span data-stu-id="13300-103">Telemetry measurements from the simulated coffee machine are sent to IoT Central for monitoring and analysis.</span></span>

![En bild som visar en kaffebryggare.](../media/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a><span data-ttu-id="13300-105">Lägga till kaffebryggaren i IoT Central</span><span class="sxs-lookup"><span data-stu-id="13300-105">Add the coffee machine in IoT Central</span></span> 
<span data-ttu-id="13300-106">Du lägger till kaffebryggaren till ditt program med hjälp av enhetsmallen **Ansluten kaffebryggare** som du skapade i föregående enhet.</span><span class="sxs-lookup"><span data-stu-id="13300-106">To add your coffee machine to your application, you use the **Connected Coffee Maker** device template you created in the previous unit.</span></span>

1. <span data-ttu-id="13300-107">Om du vill lägga till en ny enhet väljer du **Device Explorer** i den vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="13300-107">To add a new device, choose **Device Explorer** in the left navigation menu.</span></span>

    <span data-ttu-id="13300-108">Anslut kaffebryggaren genom att välja **+ Ny**, **Verklig** och sedan **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="13300-108">To start connecting your coffee machine, choose **+ New**, **Real**, and then **Create**.</span></span> <span data-ttu-id="13300-109">När det är klart ser du en lista över enheter som du skapade med samma enhetsmall, Ansluten kaffebryggare.</span><span class="sxs-lookup"><span data-stu-id="13300-109">When you're finished, you see a list of devices you've created using the same Connected Coffee Maker template.</span></span>
   
    *   <span data-ttu-id="13300-110">Den anslutna kaffebryggaren läggs till i listan när du väljer **+ Ny** och därefter **Verklig**.</span><span class="sxs-lookup"><span data-stu-id="13300-110">The Connected Coffee Maker is added to the list when you choose **+ New** and then **Real**.</span></span> 
    *   <span data-ttu-id="13300-111">Ansluten kaffebryggare (Simulerad) skapas automatiskt av IoT Central för testning.</span><span class="sxs-lookup"><span data-stu-id="13300-111">The Connected Coffee Maker (Simulate) is automatically created by IoT Central for testing purposes.</span></span> 

1.  <span data-ttu-id="13300-112">Alternativt kan du skilja den nyligen tillagda kaffebryggaren genom att lägga till ordet ”Verklig” i namnet.</span><span class="sxs-lookup"><span data-stu-id="13300-112">Optionally, you can differentiate the newly added coffee machine by appending the word “Real” in its name.</span></span> <span data-ttu-id="13300-113">Om du vill byta namn på den nya enheten väljer du enheten och redigerar namnet i namnfältet.</span><span class="sxs-lookup"><span data-stu-id="13300-113">To rename your new device, choose the device and edit the name in the name field.</span></span> 

    ![Kaffebryggare](../media/3-connect-device-a.png) 

    <span data-ttu-id="13300-115">Anteckna platsen för **Anslut den här enheten** för att ansluta kaffebryggaren i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="13300-115">Note the location of **Connect this device** for connecting your coffee machine in the next section.</span></span> <span data-ttu-id="13300-116">För tillfället visar skärmen Data saknas eftersom du inte har anslutit den till kaffebryggaren.</span><span class="sxs-lookup"><span data-stu-id="13300-116">For now, the screen displays "Missing Data" because you haven't connected to the coffee machine.</span></span> <span data-ttu-id="13300-117">Riktig telemetri börjar fylla skärmen när anslutningen görs.</span><span class="sxs-lookup"><span data-stu-id="13300-117">The real telemetry begins to populate the screen once the connection is made.</span></span> 
 
## <a name="generate-connection-string-for-the-coffee-machine-from-your-application"></a><span data-ttu-id="13300-118">Generera kaffebryggarens anslutningssträng från ditt program</span><span class="sxs-lookup"><span data-stu-id="13300-118">Generate connection string for the coffee machine from your application</span></span>
<span data-ttu-id="13300-119">Du bäddar in den verkliga kaffebryggarens anslutningssträng i den kod som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="13300-119">You embed the connection string for your real coffee machine in the code that runs on the device.</span></span> <span data-ttu-id="13300-120">Anslutningssträngen gör att kaffebryggaren kan ansluta säkert till Azure IoT Central-programmet.</span><span class="sxs-lookup"><span data-stu-id="13300-120">The connection string enables the coffee machine to connect securely to your Azure IoT Central application.</span></span> <span data-ttu-id="13300-121">Varje enhetsinstans har en unik anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="13300-121">Every device instance has a unique connection string.</span></span> <span data-ttu-id="13300-122">Härnäst ska du generera anslutningssträngen som en del av att du skapar ditt node.js-program.</span><span class="sxs-lookup"><span data-stu-id="13300-122">In the next steps, you generate the connection string as part of creating your node.js application.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="13300-123">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="13300-123">Create a Node.js application</span></span>
<span data-ttu-id="13300-124">Följande steg visar hur du skapar ett klientprogram som implementerar kaffebryggaren som du lade till i programmet.</span><span class="sxs-lookup"><span data-stu-id="13300-124">The following steps show you how to create a client application that implements the coffee machine you added to the application.</span></span>

> [!TIP]
> <span data-ttu-id="13300-125">I den här övningen skapar vi appen i Azure Cloud Shell så att du inte behöver installera något på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="13300-125">In this exercise, we'll create the app in the Azure Cloud Shell so that you don't have to install anything on your local machine.</span></span> 

1. <span data-ttu-id="13300-126">Kör följande kommando i Azure Cloud Shell för att skapa en `coffee-maker`-mapp och navigera till den:</span><span class="sxs-lookup"><span data-stu-id="13300-126">Execute the following command in the Azure Cloud Shell to create a `coffee-maker` folder and navigate to it:</span></span>

    ```azurecli
    mkdir ~/coffee-maker
    cd ~/coffee-maker
    ```

1. <span data-ttu-id="13300-127">Kör följande kommando från mappen `coffee-maker` i Cloud Shell för att installera DPS-nyckelgeneratorn:</span><span class="sxs-lookup"><span data-stu-id="13300-127">From the `coffee-maker` folder in Cloud Shell, execute the following command to install the DPS key generator:</span></span> 

   ```azurecli
    npm install dps-keygen
   ```
    <span data-ttu-id="13300-128">Det här kommandot installerar dps-keygen-paketet i vår lokala mapp, `coffee-maker`.</span><span class="sxs-lookup"><span data-stu-id="13300-128">This command installs the dps-keygen package to our local folder, `coffee-maker`.</span></span> <span data-ttu-id="13300-129">Vi hoppar över alternativet `-g` eftersom vi inte har behörighet att installera som ett globalt paket.</span><span class="sxs-lookup"><span data-stu-id="13300-129">We leave out the `-g` option because we don't have permissions to install as a global package.</span></span>

    <!-- TODO: Add more information about the DPS key generator and what it's used for -->
 
1. <span data-ttu-id="13300-130">Kör följande kommando i Cloud Shell för att ladda ned verktyget för DPS-anslutningssträng från GitHub:</span><span class="sxs-lookup"><span data-stu-id="13300-130">Execute the following command in the Cloud Shell to download the DPS connection string utility from GitHub:</span></span> 

    ```azurecli
    wget https://github.com/Azure/dps-keygen/blob/master/bin/linux/dps_cstr?raw=true -O dps_cstr 
    ```

    > [!NOTE]
    > <span data-ttu-id="13300-131">Vi laddade ned Linux-versionen av **dps_cstr** eftersom vi använder Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="13300-131">We downloaded the Linux version of **dps_cstr** because we're running in the Cloud Shell.</span></span>

1. <span data-ttu-id="13300-132">Kör följande kommando för att ge `dps_cstr` körningsbehörighet:</span><span class="sxs-lookup"><span data-stu-id="13300-132">Execute the following command to give `dps_cstr` execute permissions:</span></span>

    ```azurecli
    chmod +x dps_cstr
    ```

    <span data-ttu-id="13300-133">För att kunna generera en anslutningssträng för vår enhet behöver vi tre uppgifter från Azure IoT Central-portalen:</span><span class="sxs-lookup"><span data-stu-id="13300-133">To generate a connection string for our device, we need three pieces of information from the Azure IoT Central portal:</span></span>
    - <span data-ttu-id="13300-134">**Omfångs-ID**</span><span class="sxs-lookup"><span data-stu-id="13300-134">**Scope ID**</span></span>
    - <span data-ttu-id="13300-135">**Enhets-ID**</span><span class="sxs-lookup"><span data-stu-id="13300-135">**Device ID**</span></span>
    - <span data-ttu-id="13300-136">**Primär nyckel**</span><span class="sxs-lookup"><span data-stu-id="13300-136">**Primary Key**</span></span>

1. <span data-ttu-id="13300-137">Gå tillbaka till IoT Central-portalen.</span><span class="sxs-lookup"><span data-stu-id="13300-137">Return to the IoT Central portal.</span></span> <span data-ttu-id="13300-138">På enhetsskärmen för enheten *Ansluten kaffebryggare – Verklig* väljer du **Anslut** överst till höger på skärmen.</span><span class="sxs-lookup"><span data-stu-id="13300-138">On the device screen for the *Connected Coffee Maker - Real* device, select **Connect** in the top right of the screen.</span></span>

1. <span data-ttu-id="13300-139">I dialogrutan för enhetsanslutning som öppnas ska du spara värdena för **Omfångs-ID**, **Enhets-ID** och **Primärnyckel** någonstans, eftersom vi ska använda dem senare i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="13300-139">On the Device Connection dialog that opens, save the values **Scope ID**, **Device ID** and **Primary Key** somewhere, because we'll use them later in this exercise.</span></span> 

1. <span data-ttu-id="13300-140">Kör följande kommando i Cloud Shell och ersätt **<scope_id>**, **<device_id>** och **<primary_key>** med de värden du sparade under föregående steg.</span><span class="sxs-lookup"><span data-stu-id="13300-140">Execute the following command in the Cloud Shell, replacing **<scope_id>**, **<device_id>**, and **<primary_key>** with the values you saved in the last step.</span></span> 

   ```azurecli
   ./dps_cstr [scope_id] [device_id] [primary_key] > connection.txt
   ```
  
    <span data-ttu-id="13300-141">Det här kommandot genererar en anslutningssträng som bygger på de värden som du angav och skriver dem till en fil som vi har gett namnet **connection.txt**.</span><span class="sxs-lookup"><span data-stu-id="13300-141">This command generates a connection string based on the values you gave it and writes them to a file that we've named **connection.txt**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="13300-142">Kommandot `dps_cstr` finns inte i din SÖKVÄG i gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="13300-142">The command `dps_cstr` is not in your PATH in the shell.</span></span> <span data-ttu-id="13300-143">Se därför till att anropa det med `./dps_cstr`</span><span class="sxs-lookup"><span data-stu-id="13300-143">So, make sure to call it with `./dps_cstr`</span></span>

1. <span data-ttu-id="13300-144">Öppna den integrerade kodredigeraren i Cloud Shell genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="13300-144">Open the integrated code editor in the Cloud Shell by running the following command:</span></span> 

    ```azurecli
    code
    ```
1. <span data-ttu-id="13300-145">Välj **connection.txt** i listan över filer på menyn **FILER** i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="13300-145">Select **connection.txt** from the list of files in the **FILES** menu of the editor.</span></span>

1. <span data-ttu-id="13300-146">Kontrollera att **connection.txt** innehåller en anslutningssträng som börjar med ``HostName=``.</span><span class="sxs-lookup"><span data-stu-id="13300-146">Verify that **connection.txt** contains a connection string that starts with ``HostName=``.</span></span>

1. <span data-ttu-id="13300-147">Stäng redigeraren genom att välja **Stäng redigeringsprogrammet** på menyn (*...*) längst upp höger i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="13300-147">Close the editor by selecting **Close Editor** from the menu (*...*) at the top right of the editor.</span></span> 

1. <span data-ttu-id="13300-148">Kör följande kommando i Cloud Shell för att initiera ett Node.js-projekt i mappen `coffee-maker`:</span><span class="sxs-lookup"><span data-stu-id="13300-148">Execute the following command in the Cloud Shell to initialize a Node.js project in our `coffee-maker` folder:</span></span>

    ```azurecli
    npm init
    ```
    > [!NOTE]
    > <span data-ttu-id="13300-149">Du uppmanas av init-skriptet att ange projektegenskaper.</span><span class="sxs-lookup"><span data-stu-id="13300-149">The init script prompts you to enter project properties.</span></span> <span data-ttu-id="13300-150">Tryck på RETUR för att använda alla standardvärden för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="13300-150">For this exercise, press ENTER to use all default values.</span></span>

1. <span data-ttu-id="13300-151">Kör följande kommando i vår `coffee-maker`-mapp för att installera de nödvändiga paketen:</span><span class="sxs-lookup"><span data-stu-id="13300-151">To install the necessary packages, run the following command in our `coffee-maker` folder:</span></span>

    ```azurecli
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="13300-152">Kör följande kommando för att skapa en ny fil i Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="13300-152">Execute the following command to create a new file in the Cloud Shell:</span></span>

    ```azurecli
    touch coffeeMaker.js
    ```
1. <span data-ttu-id="13300-153">Öppna den integrerade kodredigeraren genom att köra följande på kommandoraden i Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="13300-153">Open the integrated code editor by executing the following at the command line in the Cloud Shell:</span></span> 

     ```azurecli
    code
    ```
    
1. <span data-ttu-id="13300-154">När kodredigeraren öppnas väljer du uppdateringsknappen i listan **FILER** och välj vår nya fil **coffeeMaker.js**.</span><span class="sxs-lookup"><span data-stu-id="13300-154">When the code editor opens, select the refresh button on the **FILES** list and select our new file **coffeeMaker.js**.</span></span> 

1. <span data-ttu-id="13300-155">Kopiera och klistra in följande kod i det tomma redigeringsfönstret:</span><span class="sxs-lookup"><span data-stu-id="13300-155">Copy and paste the following code into the empty editor window:</span></span>

    ```js
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;

    // Connection string (TODO: CHANGE HERE)
    const connectionString = `{your device connection string}`;

    // Global variables
    var client = clientFromConnectionString(connectionString);
    var optimalTemperature = 96;
    var cupState = true;
    var brewingState = false;
    var cupTimer = 20;
    var brewingTimer = 0;
    var maintenanceState = false;
    var warrantyState = Math.random() < 0.5?0:1;

    // Helper function to produce nice numbers (##.#)
    function niceNumber(value) 
    {
        var number = (Math.round(value * 10.0)).toString();
        return number.substr(0, 2) + '.' + number.substr(2, 1);
    }

    // Send device simulated telemetry measurements
    function sendTelemetry() 
    {   
        // Simulate the telemetry values
        var temperature = optimalTemperature + (Math.random() * 4) - 2;
        var humidity = 20 + (Math.random() * 80);
        
        // Cup timer - every 20 seconds randomly decide if the cup is present or not
        cupTimer--;
        
        if (cupTimer == 0)
        {
            cupTimer = 20;
            cupState = Math.random() < 0.5?false:true;
        }
        
        // Brewing timer
        if (brewingTimer > 0)
        {
            brewingTimer--;
            
            // Finished brewing
            if (brewingTimer == 0)
            {
                brewingState = false;
            }
        }
    
        // Create the data JSON package
        var data = JSON.stringify(
        { 
            waterTemperature: temperature, 
            airHumidity: humidity, 
        });

        // Create the message with the above defined data
        var message = new Message(data);
        
        // Set the state flags
        message.properties.add('stateCupDetected', cupState);
        message.properties.add('stateBrewing', brewingState);
        
        // Show the information in console
        var infoTemperature = niceNumber(temperature);
        var infoHumidity = niceNumber(humidity);
        var infoCup = cupState ? 'Y' :'N';
        var infoBrewing = brewingState ? 'Y':'N';
        var infoMaintenance = maintenanceState ? 'Y':'N';
        
        console.log('Telemetry send: Temperature: ' + infoTemperature + 
                    ' Humidity: ' + infoHumidity + '%' + 
                    ' Cup Detected: ' + infoCup + 
                    ' Brewing: ' + infoBrewing + 
                    ' Maintenance Mode: ' + infoMaintenance);
        
        // Send the message
        client.sendEvent(message, function (errorMessage) 
        {
            // Error
            if (errorMessage) 
            {
                console.log('Failed to send message to Azure IoT Hub: ${err.toString()}');
            }
        });
    }

    // Send device properties
    function sendDeviceProperties(deviceTwin) 
    {
        var properties = 
        {
            propertyWarrantyExpired: warrantyState
        };
        
        console.log(' * Property - Warranty State: ' + warrantyState.toString());
        
        deviceTwin.properties.reported.update(properties, (errorMessage) => 
            console.log(` * Sent device properties ` + (errorMessage ? `Error: ${errorMessage.toString()}` : `(success)`)));
    }

    // Optimal temperature setting
    var settings = 
    {
        'setTemperature': (newValue, callback) => 
        {
            setTimeout(() => 
            {
                optimalTemperature = newValue;
                callback(optimalTemperature, 'completed');
            }, 1000);
        }
    };

    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(deviceTwin) 
    {
        deviceTwin.on('properties.desired', function (desiredChange) 
        {
            // Iterate all settings looking for the defined one
            for (let setting in desiredChange) 
            {
                // Setting we defined
                if (settings[setting]) 
                {
                    // Console info
                    console.log(` * Received setting: ${setting}: ${desiredChange[setting].value}`);
                    
                    // Update 
                    settings[setting](desiredChange[setting].value, (newValue, status, message) => 
                    {
                        var patch = 
                        {
                            [setting]: 
                            {
                                value: newValue,
                                status: status,
                                desiredVersion: desiredChange.$version,
                                message: message
                            }
                        }
                        deviceTwin.properties.reported.update(patch, (err) => console.log(` * Sent setting update for ${setting} ` +
                        (err ? `error: ${err.toString()}` : `(success)`)));
                    });
                }
            }
        });
    }

    // Maintenance mode command
    function onCommandMaintenance(request, response) 
    {
        // Display console info
        console.log(' * Maintenance command received');

        // Console warning
        if (maintenanceState)
        {
            console.log(' - Warning: The device is already in the maintenance mode.');
        }
        
        // Set state
        maintenanceState = true;

        // Respond
        response.send(200, 'Success', function (errorMessage) 
        {
            // Failure
            if (errorMessage) 
            {
                console.error('[IoT hub Client] Failed sending a method response:\n' + errorMessage.message);
            }
        });
    }

    function onCommandStartBrewing(request, response) 
    {
        // Display console info
        console.log(' * Brewing command received');

        // Console warning
        if (brewingState == true)
        {
            console.log(' - Warning: The device is already brewing.');
        }
        
        if (cupState == false)
        {
            console.log(' - Warning: The cup has not been detected.');
        }
        
        if (maintenanceState == true)
        {
            console.log(' - Warning: The device is in maintenance state.');
        }
        
        // Set state - brew for 30 seconds
        if ((cupState == true) && (brewingState == false) && (maintenanceState == false))
        {
            brewingState = true;
            brewingTimer = 30;
        }
        
        // Respond
        response.send(200, 'Success', function (errorMessage) 
        {
            // Failure
            if (errorMessage) 
            {
                console.error('[IoT hub Client] Failed sending a method response:\n' + errorMessage.message);
            }
        });
    }

    // Handle device connection to Azure IoT Central
    var connectCallback = (errorMessage) => 
    {
        // Connection error
        if (errorMessage) 
        {
            console.log(`Device could not connect to Azure IoT Central: ${errorMessage.toString()}`);
        } 
        // Successfully connected
        else 
        {
            // Notify the user
            console.log('Device successfully connected to Azure IoT Central');

            // Send telemetry measurements to Azure IoT Central every 1 second.
            setInterval(sendTelemetry, 1000);
            
            // Set up device command callbacks
            client.onDeviceMethod('cmdSetMaintenance', onCommandMaintenance);
            client.onDeviceMethod('cmdStartBrewing', onCommandStartBrewing);
            
            // Get device twin from Azure IoT Central
            client.getTwin((errorMessage, deviceTwin) => 
            {
                // Failed to retrieve device twin
                if (errorMessage) 
                {
                    console.log(`Error getting device twin: ${errorMessage.toString()}`);
                } 
                // Success
                else 
                {
                    // Notify the user
                    console.log('Device Twin successfully retrieved from Azure IoT Central');
                
                    // Send device properties once on device startup
                    sendDeviceProperties(deviceTwin);
                    
                    // Apply device settings and handle changes to device settings
                    handleSettings(deviceTwin);
                }
            });
        }
    };

    // Start the device (connect it to Azure IoT Central)
    client.open(connectCallback);

    ```

    <span data-ttu-id="13300-156">Vår kaffebryggare skrivs i Node.js.</span><span class="sxs-lookup"><span data-stu-id="13300-156">Our coffee machine is written in Node.js.</span></span> <span data-ttu-id="13300-157">Först ansluter den till Azure IoT Central.</span><span class="sxs-lookup"><span data-stu-id="13300-157">It first connects to Azure IoT Central.</span></span> <span data-ttu-id="13300-158">Appen skickar sedan inledande egenskaper till Azure IoT Central, synkroniserar inställningarna, registrerar två kommandohanterare för Underhåll och Bryggning och startar slutligen timern för att skicka telemetridata varje sekund.</span><span class="sxs-lookup"><span data-stu-id="13300-158">Then the app sends initial properties to Azure IoT Central, synchronizes settings, registers two command handlers for maintenance and brewing, and finally starts the timer for sending the telemetry information every second.</span></span>

1.  <span data-ttu-id="13300-159">Uppdatera platshållaren `{your device connection string}` överst i koden med anslutningssträngen du skapade tidigare och sparade i **connection.txt**.</span><span class="sxs-lookup"><span data-stu-id="13300-159">Update the placeholder `{your device connection string}` at the top of this code with the connection string you created earlier and saved in **connection.txt**.</span></span> <span data-ttu-id="13300-160">Anslutningssträngen börjar med `HostName=`.</span><span class="sxs-lookup"><span data-stu-id="13300-160">The connection string begins with `HostName=`.</span></span>

1. <span data-ttu-id="13300-161">Välj de tre punkterna `...` längst upp till höger om redigeraren för att expandera redigeringsprogrammets meny.</span><span class="sxs-lookup"><span data-stu-id="13300-161">Select the three dots `...` to the top right of the editor to expand the editor menu.</span></span> <span data-ttu-id="13300-162">Välj sedan **Spara** för att spara de ändringar vi har gjort i ”coffeeMaker.js”</span><span class="sxs-lookup"><span data-stu-id="13300-162">Then select **Save** to save the edits we made to \`coffeeMaker.js'</span></span>

1. <span data-ttu-id="13300-163">Kör följande kommando i Cloud Shell-fönstret för att starta appen:</span><span class="sxs-lookup"><span data-stu-id="13300-163">Execute the following command in the Cloud Shell window to start the app:</span></span>

    ```azurecli
    node coffeeMaker.js
    ```
1. <span data-ttu-id="13300-164">Kontrollera att appen startar i fönstret Cloud Shell med meddelandet *Device successfully connected to Azure IoT Central* (Enheten har anslutits till Azure IoT Central) tillsammans med meddelanden om *Telemetry send:* (Skicka telemetri).</span><span class="sxs-lookup"><span data-stu-id="13300-164">Verify that the app starts in the Cloud Shell window with the message *Device successfully connected to Azure IoT Central* along with *Telemetry send:* messages.</span></span> <span data-ttu-id="13300-165">Grattis!</span><span class="sxs-lookup"><span data-stu-id="13300-165">Congratulations!</span></span> <span data-ttu-id="13300-166">Din app körs och kommunicerar med IoT Central!</span><span class="sxs-lookup"><span data-stu-id="13300-166">Your app is up and running and communicating with IoT Central!</span></span>