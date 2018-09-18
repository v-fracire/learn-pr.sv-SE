<span data-ttu-id="e27a5-101">I verkligheten förutsätts det att du ansluter Azure IoT Central till en verklig enhet eller i det här fallet en verklig kaffebryggare.</span><span class="sxs-lookup"><span data-stu-id="e27a5-101">In real life, it's assumed that you will connect Azure IoT Central to a real device or in this case, a real coffee machine.</span></span> <span data-ttu-id="e27a5-102">Men i den här enheten ansluter du ett Node.js-program, som representerar en fysisk/verklig kaffebryggare, till Azure IoT Central-programmet.</span><span class="sxs-lookup"><span data-stu-id="e27a5-102">But for the purpose of this unit, you connect a Node.js application, representing a physical/real coffee machine, to the Azure IoT Central application.</span></span> <span data-ttu-id="e27a5-103">Till följd av den här anslutningen skickas telemetrimätningar från kaffebryggaren till IoT Central för övervakning och analys.</span><span class="sxs-lookup"><span data-stu-id="e27a5-103">As a result of this connection, telemetry measurements from the coffee machine is sent to IoT Central for monitoring and analysis.</span></span>

![Kaffebryggare](../images/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a><span data-ttu-id="e27a5-105">Lägga till kaffebryggaren i IoT Central</span><span class="sxs-lookup"><span data-stu-id="e27a5-105">Add the coffee machine in IoT Central</span></span> 
<span data-ttu-id="e27a5-106">Du lägger till kaffebryggaren till ditt program med hjälp av enhetsmallen **Ansluten kaffebryggare** som du skapade i föregående enhet.</span><span class="sxs-lookup"><span data-stu-id="e27a5-106">To add your coffee machine to your application, you use the **Connected Coffee Maker** device template you created in the previous unit.</span></span>

1. <span data-ttu-id="e27a5-107">Om du vill lägga till en ny enhet väljer du **Device Explorer** i den vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="e27a5-107">To add a new device, choose **Device Explorer** in the left navigation menu.</span></span>

    <span data-ttu-id="e27a5-108">Anslut kaffebryggaren genom att välja **+ Ny** och sedan **Verklig**.</span><span class="sxs-lookup"><span data-stu-id="e27a5-108">To start connecting your coffee machine, choose **+ New**, then **Real**.</span></span> <span data-ttu-id="e27a5-109">När det är klart ser du en lista över enheter som du skapade med samma enhetsmall, Ansluten kaffebryggare.</span><span class="sxs-lookup"><span data-stu-id="e27a5-109">When you're finished, you see a list of devices you've created using the same Connected Coffee Maker template.</span></span>
   
    *   <span data-ttu-id="e27a5-110">Ansluten kaffebryggare läggs till i listan när du väljer + Ny och Real (Verklig).</span><span class="sxs-lookup"><span data-stu-id="e27a5-110">The Connected Coffee Maker is added to the list when you choose + New, then Real.</span></span> 
    *   <span data-ttu-id="e27a5-111">Ansluten kaffebryggare (Simulerad) skapas automatiskt av IoT Central för testning.</span><span class="sxs-lookup"><span data-stu-id="e27a5-111">The Connected Coffee Maker (Simulate) is automatically created by IoT Central for testing purposes.</span></span> 

1.  <span data-ttu-id="e27a5-112">Alternativt kan du skilja den nyligen tillagda kaffebryggaren genom att lägga till ordet ”Verklig” i namnet.</span><span class="sxs-lookup"><span data-stu-id="e27a5-112">Optionally, you can differentiate the newly added coffee machine by appending the word “Real” in its name.</span></span> <span data-ttu-id="e27a5-113">Om du vill byta namn på den nya enheten väljer du enheten och redigerar namnet i namnfältet.</span><span class="sxs-lookup"><span data-stu-id="e27a5-113">To rename your new device, choose the device and edit the name in the name field.</span></span> 

    ![Kaffebryggare](../images/3-connect-device-a.png) 

    <span data-ttu-id="e27a5-115">Anteckna platsen för **Anslut den här enheten** för att ansluta kaffebryggaren i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e27a5-115">Note the location of **Connect this device** for connecting your coffee machine in the next section.</span></span> <span data-ttu-id="e27a5-116">För tillfället visar skärmen ”Missing Data” (Data saknas) eftersom du inte har anslutit den till kaffebryggaren.</span><span class="sxs-lookup"><span data-stu-id="e27a5-116">For now the screen displays "Missing Data", this is because you haven't connected to the coffee machine.</span></span> <span data-ttu-id="e27a5-117">Riktig telemetri börjar fylla skärmen när anslutningen görs.</span><span class="sxs-lookup"><span data-stu-id="e27a5-117">The real telemetry begins to populate the screen once the connection is made.</span></span> 
 
## <a name="get-connection-string-for-the-coffee-machine-from-your-application"></a><span data-ttu-id="e27a5-118">Hämta kaffebryggarens anslutningssträng från ditt program</span><span class="sxs-lookup"><span data-stu-id="e27a5-118">Get connection string for the coffee machine from your application</span></span>
<span data-ttu-id="e27a5-119">Du bäddar in den verkliga kaffebryggarens anslutningssträng i den kod som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="e27a5-119">You embed the connection string for your real coffee machine in the code that runs on the device.</span></span> <span data-ttu-id="e27a5-120">Anslutningssträngen gör att kaffebryggaren kan ansluta säkert till Azure IoT Central-programmet.</span><span class="sxs-lookup"><span data-stu-id="e27a5-120">The connection string enables the coffee machine to connect securely to your Azure IoT Central application.</span></span> <span data-ttu-id="e27a5-121">Varje enhetsinstans har en unik anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="e27a5-121">Every device instance has a unique connection string.</span></span> <span data-ttu-id="e27a5-122">Så här hittar du anslutningssträngen för en enhetsinstans i programmet:</span><span class="sxs-lookup"><span data-stu-id="e27a5-122">Here is how to find the connection string for a device instance in your application:</span></span>

1.  <span data-ttu-id="e27a5-123">På enhetsskärmen för Ansluten kaffebryggare (Riktig) väljer du **Anslut den här enheten**.</span><span class="sxs-lookup"><span data-stu-id="e27a5-123">On the device screen for your Connected Coffee Machine Real, choose **Connect this device**.</span></span>

1.  <span data-ttu-id="e27a5-124">På sidan **Anslut** kopierar du den **primära anslutningssträngen** och sparar den.</span><span class="sxs-lookup"><span data-stu-id="e27a5-124">On the **Connect** page, copy the **Primary connection string**, and save it.</span></span> <span data-ttu-id="e27a5-125">Du använder det här värdet i klientprogrammet som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="e27a5-125">You use this value in the client application that runs on the device.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="e27a5-126">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="e27a5-126">Create a Node.js application</span></span>
<span data-ttu-id="e27a5-127">Följande steg visar hur du skapar ett klientprogram som implementerar kaffebryggaren som du lade till programmet.</span><span class="sxs-lookup"><span data-stu-id="e27a5-127">The following steps show you how to create a client application that implements the coffee machine you added to the application.</span></span>
1. <span data-ttu-id="e27a5-128">Installera [Node.js](https://nodejs.org/)-versionen 4.0.x eller senare på datorn.</span><span class="sxs-lookup"><span data-stu-id="e27a5-128">Install [Node.js](https://nodejs.org/) version 4.0.x or later on your machine.</span></span> <span data-ttu-id="e27a5-129">Node.js är tillgängligt för många olika operativsystem.</span><span class="sxs-lookup"><span data-stu-id="e27a5-129">Node.js is available for a wide variety of operating systems.</span></span>

1. <span data-ttu-id="e27a5-130">Skapa en mapp med namnet kaffebryggare på datorn.</span><span class="sxs-lookup"><span data-stu-id="e27a5-130">Create a folder called coffee-maker on your machine.</span></span> <span data-ttu-id="e27a5-131">Gå till den mappen i kommandoradsmiljön.</span><span class="sxs-lookup"><span data-stu-id="e27a5-131">Navigate to that folder in your command-line environment.</span></span>

1. <span data-ttu-id="e27a5-132">Initiera Node.js-projektet genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="e27a5-132">To initialize your Node.js project, run the following commands:</span></span>
    ```cmd/sh
    npm init
    ```
    > [!NOTE]
    > <span data-ttu-id="e27a5-133">Du uppmanas av init-skriptet att ange projektegenskaper.</span><span class="sxs-lookup"><span data-stu-id="e27a5-133">The init script prompts you to enter project properties.</span></span> <span data-ttu-id="e27a5-134">För den här övningen rekommenderas och räcker det att använda standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="e27a5-134">For this exercise, using the default values is sufficient and recommended.</span></span> 
1. <span data-ttu-id="e27a5-135">Kör följande kommando för att installera de nödvändiga paketen:</span><span class="sxs-lookup"><span data-stu-id="e27a5-135">To install the necessary packages, run the following command:</span></span>
    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="e27a5-136">Skapa en fil med namnet coffeeMaker.js i mappen kaffebryggare med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="e27a5-136">Using a text editor, create a file called coffeeMaker.js in the coffee-maker folder.</span></span>

1. <span data-ttu-id="e27a5-137">Kopiera och klistra in koden i filen coffeeMaker.js och **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e27a5-137">Copy and paste the code into your coffeeMaker.js file and **Save**.</span></span>

    <span data-ttu-id="e27a5-138">Koden som representerar den verkliga kaffebryggaren i den här enheten är skriven i Node.js.</span><span class="sxs-lookup"><span data-stu-id="e27a5-138">The code, representing the real coffee machine in this unit, is written in Node.js.</span></span> <span data-ttu-id="e27a5-139">Du börjar med att upprätta en anslutning till programmet. Sedan skickar du inledande egenskaper till Azure IoT Central, synkroniserar inställningarna, registrerar två kommandohanterare för Underhåll och Bryggning och startar slutligen timern för att skicka telemetridata varje sekund.</span><span class="sxs-lookup"><span data-stu-id="e27a5-139">You begin by establishing connection with the application, then you send initial properties to Azure IoT Central, synchronize the settings, register two command handlers for Maintenance and Brewing, and finally start the timer for sending the telemetry information every second.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e27a5-140">Uppdatera platshållaren `{your device connection string}`.</span><span class="sxs-lookup"><span data-stu-id="e27a5-140">Update the placeholder `{your device connection string}`.</span></span> 


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
        
        // Cup timer - every 20 second randomly decide if the cup is present or not
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
            
            // Setup device command callbacks
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
                
                    // Send device properties once on device start up
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

1.  <span data-ttu-id="e27a5-141">Uppdatera platshållaren {enhetens anslutningssträng} med enhetens anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="e27a5-141">Update the placeholder {your device connection string} with your device connection string.</span></span> <span data-ttu-id="e27a5-142">Du kopierade det här värdet från anslutningens informationssida när du lade till din verkliga enhet.</span><span class="sxs-lookup"><span data-stu-id="e27a5-142">You copied this value from the connection details page when you added your real device.</span></span> 

## <a name="run-your-nodejs-application"></a><span data-ttu-id="e27a5-143">Kör Node.js-programmet</span><span class="sxs-lookup"><span data-stu-id="e27a5-143">Run your Node.js application</span></span>
```cmd/sh
node coffeeMaker.js
```

## <a name="summary"></a><span data-ttu-id="e27a5-144">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e27a5-144">Summary</span></span>
<span data-ttu-id="e27a5-145">I den här enheten har du anslutit din kaffebryggare till Azure IoT Central och börjat skicka data för övervakning och analys.</span><span class="sxs-lookup"><span data-stu-id="e27a5-145">In this unit, you connected your coffee machine to Azure IoT Central and began sending data for monitoring and analysis.</span></span> <span data-ttu-id="e27a5-146">Du har upprättat anslutningen genom att först skaffa en anslutningssträng från IoT Central och sedan konfigurera strängen i kaffebryggaren.</span><span class="sxs-lookup"><span data-stu-id="e27a5-146">You achieved the connectivity by first acquiring a connection string from IoT Central, followed by configuring the string in the coffee machine.</span></span>
