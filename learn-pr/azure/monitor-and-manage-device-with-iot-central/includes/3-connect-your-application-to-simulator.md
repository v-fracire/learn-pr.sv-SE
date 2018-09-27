[!include[](../../../includes/azure-sandbox-activate.md)]

I praktiken ansluter du Azure IoT Central till en fysisk enhet, t.ex. en IoT-aktiverad kaffebryggare. Här ska vi simulera en enhet med en Node.js-app och ansluta den till programmet Azure IoT Central. Telemetrimätningar från den simulerade kaffebryggaren skickas till IoT Central för övervakning och analys.

![En bild som visar en kaffebryggare.](../media/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a>Lägga till kaffebryggaren i IoT Central 
Du lägger till kaffebryggaren till ditt program med hjälp av enhetsmallen **Ansluten kaffebryggare** som du skapade i föregående enhet.

1. Om du vill lägga till en ny enhet väljer du **Device Explorer** i den vänstra navigeringsmenyn.

    Anslut kaffebryggaren genom att välja **+ Ny**, **Verklig** och sedan **Skapa**. När det är klart ser du en lista över enheter som du skapade med samma enhetsmall, Ansluten kaffebryggare.
   
    *   Den anslutna kaffebryggaren läggs till i listan när du väljer **+ Ny** och därefter **Verklig**. 
    *   Ansluten kaffebryggare (Simulerad) skapas automatiskt av IoT Central för testning. 

1.  Alternativt kan du skilja den nyligen tillagda kaffebryggaren genom att lägga till ordet ”Verklig” i namnet. Om du vill byta namn på den nya enheten väljer du enheten och redigerar namnet i namnfältet. 

    ![Kaffebryggare](../media/3-connect-device-a.png) 

    Anteckna platsen för **Anslut den här enheten** för att ansluta kaffebryggaren i nästa avsnitt. För tillfället visar skärmen Data saknas eftersom du inte har anslutit den till kaffebryggaren. Riktig telemetri börjar fylla skärmen när anslutningen görs. 
 
## <a name="generate-connection-string-for-the-coffee-machine-from-your-application"></a>Generera kaffebryggarens anslutningssträng från ditt program
Du bäddar in den verkliga kaffebryggarens anslutningssträng i den kod som körs på enheten. Anslutningssträngen gör att kaffebryggaren kan ansluta säkert till Azure IoT Central-programmet. Varje enhetsinstans har en unik anslutningssträng. Härnäst ska du generera anslutningssträngen som en del av att du skapar ditt node.js-program.

## <a name="create-a-nodejs-application"></a>Skapa ett Node.js-program
Följande steg visar hur du skapar ett klientprogram som implementerar kaffebryggaren som du lade till i programmet.

> [!TIP]
> I den här övningen skapar vi appen i Azure Cloud Shell så att du inte behöver installera något på den lokala datorn. 

1. Kör följande kommando i Azure Cloud Shell för att skapa en `coffee-maker`-mapp och navigera till den:

    ```azurecli
    mkdir ~/coffee-maker
    cd ~/coffee-maker
    ```

1. Kör följande kommando från mappen `coffee-maker` i Cloud Shell för att installera DPS-nyckelgeneratorn: 

   ```azurecli
    npm install dps-keygen
   ```
    Det här kommandot installerar dps-keygen-paketet i vår lokala mapp, `coffee-maker`. Vi hoppar över alternativet `-g` eftersom vi inte har behörighet att installera som ett globalt paket.

    <!-- TODO: Add more information about the DPS key generator and what it's used for -->
 
1. Kör följande kommando i Cloud Shell för att ladda ned verktyget för DPS-anslutningssträng från GitHub: 

    ```azurecli
    wget https://github.com/Azure/dps-keygen/blob/master/bin/linux/dps_cstr?raw=true -O dps_cstr 
    ```

    > [!NOTE]
    > Vi laddade ned Linux-versionen av **dps_cstr** eftersom vi använder Cloud Shell.

1. Kör följande kommando för att ge `dps_cstr` körningsbehörighet:

    ```azurecli
    chmod +x dps_cstr
    ```

    För att kunna generera en anslutningssträng för vår enhet behöver vi tre uppgifter från Azure IoT Central-portalen:
    - **Omfångs-ID**
    - **Enhets-ID**
    - **Primär nyckel**

1. Gå tillbaka till IoT Central-portalen. På enhetsskärmen för enheten *Ansluten kaffebryggare – Verklig* väljer du **Anslut** överst till höger på skärmen.

1. I dialogrutan för enhetsanslutning som öppnas ska du spara värdena för **Omfångs-ID**, **Enhets-ID** och **Primärnyckel** någonstans, eftersom vi ska använda dem senare i den här övningen. 

1. Kör följande kommando i Cloud Shell och ersätt **<scope_id>**, **<device_id>** och **<primary_key>** med de värden du sparade under föregående steg. 

   ```azurecli
   ./dps_cstr [scope_id] [device_id] [primary_key] > connection.txt
   ```
  
    Det här kommandot genererar en anslutningssträng som bygger på de värden som du angav och skriver dem till en fil som vi har gett namnet **connection.txt**.

    > [!IMPORTANT]
    > Kommandot `dps_cstr` finns inte i din SÖKVÄG i gränssnittet. Se därför till att anropa det med `./dps_cstr`

1. Öppna den integrerade kodredigeraren i Cloud Shell genom att köra följande kommando: 

    ```azurecli
    code
    ```
1. Välj **connection.txt** i listan över filer på menyn **FILER** i redigeraren.

1. Kontrollera att **connection.txt** innehåller en anslutningssträng som börjar med ``HostName=``.

1. Stäng redigeraren genom att välja **Stäng redigeringsprogrammet** på menyn (*...*) längst upp höger i redigeraren. 

1. Kör följande kommando i Cloud Shell för att initiera ett Node.js-projekt i mappen `coffee-maker`:

    ```azurecli
    npm init
    ```
    > [!NOTE]
    > Du uppmanas av init-skriptet att ange projektegenskaper. Tryck på RETUR för att använda alla standardvärden för den här övningen.

1. Kör följande kommando i vår `coffee-maker`-mapp för att installera de nödvändiga paketen:

    ```azurecli
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Kör följande kommando för att skapa en ny fil i Cloud Shell:

    ```azurecli
    touch coffeeMaker.js
    ```
1. Öppna den integrerade kodredigeraren genom att köra följande på kommandoraden i Cloud Shell: 

     ```azurecli
    code
    ```
    
1. När kodredigeraren öppnas väljer du uppdateringsknappen i listan **FILER** och välj vår nya fil **coffeeMaker.js**. 

1. Kopiera och klistra in följande kod i det tomma redigeringsfönstret:

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

    Vår kaffebryggare skrivs i Node.js. Först ansluter den till Azure IoT Central. Appen skickar sedan inledande egenskaper till Azure IoT Central, synkroniserar inställningarna, registrerar två kommandohanterare för Underhåll och Bryggning och startar slutligen timern för att skicka telemetridata varje sekund.

1.  Uppdatera platshållaren `{your device connection string}` överst i koden med anslutningssträngen du skapade tidigare och sparade i **connection.txt**. Anslutningssträngen börjar med `HostName=`.

1. Välj de tre punkterna `...` längst upp till höger om redigeraren för att expandera redigeringsprogrammets meny. Välj sedan **Spara** för att spara de ändringar vi har gjort i ”coffeeMaker.js”

1. Kör följande kommando i Cloud Shell-fönstret för att starta appen:

    ```azurecli
    node coffeeMaker.js
    ```
1. Kontrollera att appen startar i fönstret Cloud Shell med meddelandet *Device successfully connected to Azure IoT Central* (Enheten har anslutits till Azure IoT Central) tillsammans med meddelanden om *Telemetry send:* (Skicka telemetri). Grattis! Din app körs och kommunicerar med IoT Central!