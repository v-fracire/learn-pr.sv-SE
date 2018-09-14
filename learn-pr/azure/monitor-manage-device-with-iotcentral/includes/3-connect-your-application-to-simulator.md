<span data-ttu-id="400ee-101">I verkligheten förutsätts det att du ansluter Azure IoT Central till en verklig enhet eller i det här fallet en verklig kaffebryggare.</span><span class="sxs-lookup"><span data-stu-id="400ee-101">In real life, it's assumed that you will connect Azure IoT Central to a real device or in this case, a real coffee machine.</span></span> <span data-ttu-id="400ee-102">Men i den här modulen är syftet att ansluta Azure IoT Central-programmet till ett allmänt Node.js-program som representerar en fysisk kaffebryggare.</span><span class="sxs-lookup"><span data-stu-id="400ee-102">But for the purpose of this module, you connect the Azure IoT Central application to a generic Node.js application representing a physical coffee machine.</span></span> <span data-ttu-id="400ee-103">Till följd av den här anslutningen skickas sensoriska data från kaffebryggaren till IoT Central för övervakning och analys.</span><span class="sxs-lookup"><span data-stu-id="400ee-103">As a result of this connection, sensory data from the coffee machine is sent to IoT Central for monitoring and analysis.</span></span>

![Kaffebryggare](../images/3-coffee-machine.png) 

## <a name="add-the-simulated-coffee-machine-in-iot-central"></a><span data-ttu-id="400ee-105">Lägg till den simulerade kaffebryggaren i IoT Central</span><span class="sxs-lookup"><span data-stu-id="400ee-105">Add the simulated coffee machine in IoT Central</span></span> 
<span data-ttu-id="400ee-106">Du lägger till den simulerade kaffebryggaren till ditt program med hjälp av enhetsmallen **Ansluten kaffebryggare** som du skapade i föregående modul.</span><span class="sxs-lookup"><span data-stu-id="400ee-106">To add your simulated coffee machine to your application, you use the **Connected Coffee Maker** device template you created in the previous module.</span></span>

1. <span data-ttu-id="400ee-107">Om du vill lägga till en ny enhet som operatör väljer du **Device Explorer** i den vänstra navigeringsmenyn:</span><span class="sxs-lookup"><span data-stu-id="400ee-107">To add a new device as an operator choose **Device Explorer** in the left navigation menu:</span></span>

    <span data-ttu-id="400ee-108">**Device Explorer** visar enhetsmallen **Ansluten kaffebryggare** och enhetssimulatorn som skapades automatiskt när enhetsmallen skapades.</span><span class="sxs-lookup"><span data-stu-id="400ee-108">The **Device Explorer** shows the **Connected Coffee Maker** device template and the device simulator that was automatically created when the builder created the device template.</span></span>

1.  <span data-ttu-id="400ee-109">Anslut din simulerade kaffebryggare genom att välja **Ny** och sedan **Verklig**.</span><span class="sxs-lookup"><span data-stu-id="400ee-109">To start connecting your simulated coffee machine, choose **New**, then **Real**.</span></span>

1.  <span data-ttu-id="400ee-110">Alternativt kan du byta namn på den nya enheten genom att välja enhetsnamnet och redigera värdet:</span><span class="sxs-lookup"><span data-stu-id="400ee-110">Optionally, you can rename your new device by choosing the device name and editing the value:</span></span>
 
## <a name="get-connection-string-for-the-coffee-machine-from-your-application"></a><span data-ttu-id="400ee-111">Hämta kaffebryggarens anslutningssträng från ditt program</span><span class="sxs-lookup"><span data-stu-id="400ee-111">Get connection string for the coffee machine from your application</span></span>
<span data-ttu-id="400ee-112">Du bäddar in den verkliga kaffebryggarens anslutningssträng i den kod som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="400ee-112">You embed the connection string for your real coffee machine in the code that runs on the device.</span></span> <span data-ttu-id="400ee-113">Anslutningssträngen gör att kaffebryggaren kan ansluta säkert till Azure IoT Central-programmet.</span><span class="sxs-lookup"><span data-stu-id="400ee-113">The connection string enables the coffee machine to connect securely to your Azure IoT Central application.</span></span> <span data-ttu-id="400ee-114">Varje enhetsinstans har en unik anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="400ee-114">Every device instance has a unique connection string.</span></span> <span data-ttu-id="400ee-115">Så här hittar du anslutningssträngen för en enhetsinstans i programmet:</span><span class="sxs-lookup"><span data-stu-id="400ee-115">Here is how to find the connection string for a device instance in your application:</span></span>

1.  <span data-ttu-id="400ee-116">På skärmen Enhet för den riktiga, anslutna kaffebryggaren väljer du **Anslut den här enheten**.</span><span class="sxs-lookup"><span data-stu-id="400ee-116">On the Device screen for your real connected coffee machine, choose **Connect this device**.</span></span>

1.  <span data-ttu-id="400ee-117">På sidan **Anslut** kopierar du den **primära anslutningssträngen** och sparar den.</span><span class="sxs-lookup"><span data-stu-id="400ee-117">On the **Connect** page, copy the **Primary connection string**, and save it.</span></span> <span data-ttu-id="400ee-118">Du använder det här värdet i klientprogrammet som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="400ee-118">You use this value in the client application that runs on the device.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="400ee-119">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="400ee-119">Create a Node.js application</span></span>
<span data-ttu-id="400ee-120">Följande steg visar hur du skapar ett klientprogram som implementerar kaffebryggaren som du lade till programmet.</span><span class="sxs-lookup"><span data-stu-id="400ee-120">The following steps show how to create a client application that implements the coffee machine you added to the application.</span></span>
1. <span data-ttu-id="400ee-121">Installera [Node.js](https://nodejs.org/)-versionen 4.0.x eller senare på datorn.</span><span class="sxs-lookup"><span data-stu-id="400ee-121">Install [Node.js](https://nodejs.org/) version 4.0.x or later in your machine.</span></span> <span data-ttu-id="400ee-122">Node.js är tillgängligt för många olika operativsystem.</span><span class="sxs-lookup"><span data-stu-id="400ee-122">Node.js is available for a wide variety of operating systems.</span></span>

1. <span data-ttu-id="400ee-123">Skapa en mapp med namnet kaffebryggare på datorn.</span><span class="sxs-lookup"><span data-stu-id="400ee-123">Create a folder called coffee-maker on your machine.</span></span> <span data-ttu-id="400ee-124">Gå till den mappen i kommandoradsmiljön.</span><span class="sxs-lookup"><span data-stu-id="400ee-124">Navigate to that folder in your command-line environment.</span></span>

1. <span data-ttu-id="400ee-125">Initiera Node.js-projektet genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="400ee-125">To initialize your Node.js project, run the following commands:</span></span>
    ```cmd/sh
    npm init
    ```

1. <span data-ttu-id="400ee-126">Kör följande kommando för att installera de nödvändiga paketen:</span><span class="sxs-lookup"><span data-stu-id="400ee-126">To install the necessary packages, run the following command:</span></span>
    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="400ee-127">Skapa en fil med namnet coffeeMaker.js i mappen kaffebryggare med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="400ee-127">Using a text editor, create a file called coffeeMaker.js in the coffee-maker folder.</span></span>

1. <span data-ttu-id="400ee-128">Kopiera och klistra in koden i filen coffeeMaker.js och **Spara**.</span><span class="sxs-lookup"><span data-stu-id="400ee-128">Copy and paste the code into your coffeeMaker.js file and **Save**.</span></span>

    <span data-ttu-id="400ee-129">Koden som representerar den verkliga kaffebryggaren i den här modulen är skriven i Node.js.</span><span class="sxs-lookup"><span data-stu-id="400ee-129">The code representing the real coffee machine in this module is written in Node.js.</span></span> <span data-ttu-id="400ee-130">Du börjar med att upprätta en anslutning till programmet, skicka inledande egenskaper till Azure IoT Central, synkronisera inställningarna, registrera två kommandohanterare för Underhåll och Bryggning och slutligen starta timern för att skicka telemetridata varje sekund.</span><span class="sxs-lookup"><span data-stu-id="400ee-130">You begin by establishing connection with the application, send initial properties to Azure IoT Central, synchronize the settings, register two command handlers for Maintenance and Brewing, and finally start the timer for sending the telemetry information every second.</span></span>

    > [!NOTE]
    > <span data-ttu-id="400ee-131">Kom ihåg att uppdatera platshållaren `{your device connection string}`.</span><span class="sxs-lookup"><span data-stu-id="400ee-131">Remeber to update the placeholder `{your device connection string}`.</span></span> 


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
    var deviceMinTemperature = 88 + (Math.random() * 4);
    var deviceMaxTemperature = 98 + (Math.random() * 2);
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

1.  <span data-ttu-id="400ee-132">Uppdatera platshållaren {enhetens anslutningssträng} med enhetens anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="400ee-132">Update the placeholder {your device connection string} with your device connection string.</span></span> <span data-ttu-id="400ee-133">Du kopierade det här värdet från anslutningens informationssida när du lade till din verkliga enhet.</span><span class="sxs-lookup"><span data-stu-id="400ee-133">You copied this value from the connection details page when you added your real device.</span></span> 

## <a name="run-your-nodejs-application"></a><span data-ttu-id="400ee-134">Kör Node.js-programmet</span><span class="sxs-lookup"><span data-stu-id="400ee-134">Run your Node.js application</span></span>
```cmd/sh
node coffeeMaker.js
```

## <a name="summary"></a><span data-ttu-id="400ee-135">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="400ee-135">Summary</span></span>
<span data-ttu-id="400ee-136">I den här modulen har du anslutit din simulerade kaffebryggare till Azure IoT Central och börjat skicka data för övervakning och analys.</span><span class="sxs-lookup"><span data-stu-id="400ee-136">In this module, you connected your simulated coffee machine to Azure IoT Central and began sending data for monitoring and analysis.</span></span> <span data-ttu-id="400ee-137">Du har upprättat anslutningen genom att först skaffa en anslutningssträng från IoT Central och sedan konfigurera strängen i kaffebryggaren.</span><span class="sxs-lookup"><span data-stu-id="400ee-137">You achieved the connectivity by first acquiring a connection string from IoT Central, followed by configuring the string in the coffee machine.</span></span>
