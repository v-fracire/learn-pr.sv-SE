Nu när du vet hur Azure IoT Hub samlar in data från IoT-enheter, till exempel Raspberry Pi, kan du gå vidare med att etablera andra IoT-enheter som också ska skicka sina data. En fullständig installation av IoT-enheter i ett litet projekt (1–5 enheter) kan göras 1 till 1. Större distributioner kräver en bättre metod för etableringen.

Med Azure IoT Hub Device Provisioning Service kan kunderna konfigurera zero-touch-enhetsetablering på Azure IoT Hub, med all skalbarhet i molnet för något som en gång var en tidskrävande engångsprocess. Processen har utformats med utmaningarna i leveranskedjan i åtanke för att kunna tillhandahålla den infrastruktur som krävs för att etablera miljontals enheter på ett säkert och skalbart sätt.

Automatisk enhetsetablering med Device Provisioning Service har nu stöd för alla protokoll som stöds av IoT Hub, inklusive HTTP, AMQP, MQTT, AMQP via WebSockets och MQTT via WebSockets. Den här versionen motsvarar också utökat SDK-språkstöd på både enhets- och klientsidan.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Länka Device Provisioning Service till en IoT-hubb

Nästa steg är att länka Device Provisioning-tjänsten och IoT Hub så att IoT Hub Device Provisioning-tjänsten kan registrera enheter till den hubben. Tjänsten kan endast etablera enheter till IoT-hubbar som har länkats till Device Provisioning-tjänsten. Följ de här stegen.

1.  På sidan **Alla resurser** i Azure-portalen, klickar du på tjänstinstansen Device Provisioning som du skapade tidigare.

2.  Välj **Länkade IoT-hubbar** på Device Provisioning Service-sidan.

3.  Klicka på **Lägg till**.

4.  På sidan **Lägg till länk till IoT-hubb** anger du följande information och klickar på **Spara**:

    - **Prenumeration:** Se till att den prenumeration som innehåller IoT-hubben är markerad. Du kan länka till en IoT-hubb som finns i en annan prenumeration.

    - **IoT-hubb:** Välj namnet på den IoT-hubb som du vill länka med den här instansen för enhetsetableringstjänsten.

    - **Åtkomstprincip:** Välj **iothubowner** som de autentiseringsuppgifter som ska användas vid etablering av länken till IoT-hubben.

![Länka hubbnamnet för att länka till DPS i portalen](../media/ee6e78754a1d39d86de71fb6872723f3.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Ange allokeringsprincip för enhetsetableringstjänsten

Allokeringsprincipen är en inställning för IoT Hub Device Provisioning-tjänsten som bestämmer hur enheter tilldelas till en IoT-hubb. Det finns tre allokeringsprinciper som stöds:

1. **Kortast svarstid**: Enheter etableras till en IoT-hubb baserat på hubben med kortast svarstid till enheten.

2. **Jämnt viktad distribution** (standard): Det är lika sannolikt att länkade IoT-hubbar får enheter etablerade till sig. Den här inställningen är standardinställningen. Om du endast etablerar enheter till en IoT-hubb kan du behålla den här inställningen.

3. **Statisk konfiguration via registreringslistan**: Specificering av den önskade IoT-hubben på registreringslistan har högre prioritet än allokeringsprincipen på Device Provisioning-tjänstnivå.

Om du vill ställa in allokeringsprincipen går du till Device Provisioning-tjänstsidan och klickar på **Hantera allokeringsprincip**. Kontrollera att allokeringsprincipen är inställd på **Jämnt viktad distribution** (standardinställningen). Om du gör några ändringar ska du klicka på **Spara** när du är klar.

![Hantera allokeringsprincip](../media/0c5fa5193156f17b4f5d64aab65a414d.png)

## <a name="enroll-the-device"></a>Registrera enheten

I det här steget ska du lägga till enhetens unika säkerhetsartefakter till Device Provisioning Service. Säkerhetsartefakterna baseras på enhetens [attesteringsmekanism](https://docs.microsoft.com/azure/iot-dps/concepts-device#attestation-mechanism) enligt följande:

För TPM-baserade enheter behöver du:

- *Bekräftelsenyckeln* som är unik för varje TPM-krets eller -simulering, som hämtas från tillverkaren av TPM-kretsen. Läs [Understand TPM Endorsement Key](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation#terminology) (Förstå TPM-bekräftelsenyckeln) för mer information.

- *Registrerings-ID:t* som används för att unikt identifiera en enhet i namnrymden/omfattningen. ID:t kanske eller kanske inte är samma som enhetens ID. ID:t är obligatoriskt för alla enheter. För TPM-baserade enheter kan registrerings-ID:t härledas från själv TPM, till exempel en SHA-256-hash för TPM-bekräftelsenyckeln.

![Registreringsinformation för TPM i portalen](../media/11db90b7128e1cf222a4da45de7cbac8.png)

För X.509-baserade enheter behöver du:

- [Certifikatet som utfärdats till X.509](https://docs.microsoft.com/windows/desktop/SecCertEnroll/about-x-509-public-key-certificates)-kretsen eller simuleringen är antingen en *.pem*- eller *.cer*-fil. Vid enskild registrering måste du använda *undertecknarens         certifikat* per enhet för ditt X.509-system, och för registreringsgrupper måste du använda *rotcertifikatet*.

   ![Lägga till en enskild registrering för X.509-attestering i portalen](../media/8d56752f453f27e55dd15b7c894ae406.png)

Det finns två sätt att registrera enheten i Device Provisioning Service på:

- **Registreringsgrupper** Det här representerar en grupp med enheter som delar en specifik attesteringsmekanism. Vi rekommenderar att du använder en registreringsgrupp för ett stort antal enheter som delar en önskad inledande konfiguration, eller för enheter som ska till samma klient. Mer information om identitetsattestering för registreringsgrupper finns i [Säkerhet](https://docs.microsoft.com/azure/iot-dps/concepts-security#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

   ![Lägga till gruppregistrering för X.509-attestering i portalen](../media/4a9d9ea822887c70f1ff1e4b64b138f1.png)

- **Enskilda registreringar** Detta motsvarar en inmatning för en enskild enhet som kan registreras med Device Provisioning Service. Enskilda registreringar kan använda antingen x509-certifikat eller SAS-token (i en verklig eller virtuell TPM) som attesteringsmekanismer. Vi rekommenderar att du använder enskilda registreringar för enheter som kräver unika första konfigurationer och enheter som endast kan använda SAS-token via TPM eller virtuella TPM:er som attesteringsmekanism. Enskilda registreringar kan ha angivet önskat enhets-ID för IoT Hub.

Nu registrerar du enheten med enhetsetableringstjänstens instans, med de säkerhetsartefakter som krävs baserat på enhetens attesteringsmekanism:

1. Logga in på Azure Portal, klicka på knappen **Alla resurser** i den vänstra menyn och öppna Device Provisioning-tjänsten.

2. På sammanfattningsbladet för Device Provisioning Service väljer du **Hantera registreringar**. Välj fliken **Enskilda registreringar** eller **Registreringsgrupper** enligt enhetskonfigurationen. Klicka på knappen **Lägg till** högst upp. Välj **TPM** eller **X.509** som identitetsattesteringens *Metod* och ange de lämpliga säkerhetsartefakter som beskrevs     tidigare. Du kan ange ett nytt **Enhets-ID för IoT Hub**. Klicka på knappen **Spara** när det är klart.

3. När enheten har registrerats bör du se den i portalen på följande sätt:

![Lyckad TPM-registrering i portalen](../media/cb277b2e5bc21cd02669775d536e89c0.png)

Efter registreringen väntar etableringstjänsten tills enheten har startats och ansluter till den vid en senare tidpunkt. När enheten startar för första gången interagerar klient-SDK-biblioteket med din krets för att extrahera säkerhetsartefakterna från enheten. Registreringen verifieras med Device Provisioning-tjänsten.

## <a name="start-the-iot-device"></a>Starta IoT-enheten

IoT-enheten kan vara en riktig enhet eller en simulerad enhet. Eftersom IoT-enheten nu har registrerats med en instans av enhetsetableringstjänsten kan enheten nu starta och anropa etableringstjänsten för att identifieras med hjälp av attesteringsmetoden. När etableringstjänsten har identifierat enheten tilldelas den till en IoT-hubb.

Exempel på simulerade enheter, med både TPM- och X.509-attestering, ingår för C, Java, C\#, Node.js och Python. Till exempel skulle en simulerad enhet med TPM och [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) följa den process som beskrivs i avsnittet om att [Simulera den första startsekvensen för en enhet](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device#simulate-first-boot-sequence-for-the-device). För samma enhet som använder X.509-certifikatattestering skulle det här avsnittet om [startsekvens](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509#simulate-first-boot-sequence-for-the-device) gälla.

Se [instruktionsguiden för MXChip Iot DevKit](https://docs.microsoft.com/azure/iot-dps/how-to-connect-mxchip-iot-devkit) för ett exempel på en riktig enhet.

Starta enheten för att låta enhetens klientprogram starta registreringen med din Device Provisioning-tjänst.

## <a name="verify-the-device-is-registered"></a>Kontrollera att enheten är registrerad

När enheten startar ska följande åtgärder utföras:

1. Enheten skickar en registreringsbegäran till Device Provisioning-tjänsten.

2. För TPM-enheten skickar enhetsetableringstjänsten tillbaka en registreringskontroll som enheten svarar på.

3. Om registringen har lyckats skickar enhetsetableringstjänsten tillbaka IoT-hubbens URI, enhets-ID och den krypterade nyckeln till enheten.

4. IoT Hub-klientprogrammet på enheten ansluter därefter till hubben.

5. Om anslutningen till hubben lyckas bör enheten visas i IoT-hubbens utforskare för **IoT-enheter**.

![Lyckad anslutning till hubben i portalen](../media/12ea6da6eef9bf96be6bd80aa1721173.png)

<!--Reference links

-   <https://docs.microsoft.com/azure/iot-dps/tutorial-set-up-cloud>

-   <https://docs.microsoft.com/azure/iot-dps/tutorial-provision-device-to-hub>-->