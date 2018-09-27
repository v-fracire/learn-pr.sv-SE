> [!TIP]
> Användarnamnet och lösenordet som du behöver för att logga in på den virtuella datorn finns på fliken **Resurser**.

> [!NOTE]
> Om du använder en Mac kan du när du startar den virtuella datorn behöva använda antingen blixtikonen i verktygsfältet eller alternativet **Ctrl+Alt+Delete** på fliken **Resurser** bredvid anvisningarna för att låsa upp den virtuella datorn.


I den här modulen skapar du en plattformsoberoende Xamarin.Forms-app med en serverlös serverdel. Appen hämtar användarnas position från deras enheter och skickar både den och en lista med telefonnummer till en Azure-funktion. Funktionen använder sedan en bindning till en tjänst från tredje part (Twilio) och skickar positionen som ett SMS till de angivna telefonnumren.

Den här processen omfattar följande steg:

1. Appen samlar in platsinformationen med Xamarin.Essentials som en abstraktion över via enhetsspecifika plats-API:er.

1. Platsinformationen och telefonnumren paketeras i en JSON-nyttolast och skickas till en Azure-funktion.

1. Azure-funktionen avkodar JSON-nyttolasten och skapar SMS:en.

1. SMS:en skickas via [Twilio](https://www.twilio.com/?azure-portal=true).

Följande bild visar en översikt över den här processen.

![En illustration med en övergripande arkitektur för processen för med att dela plats via SMS.](../media/1-architecture.png)

## <a name="create-a-twilio-account"></a>Skapa ett Twilio-konto

Om du vill kunna skicka SMS från en Azure-funktion måste du ha ett Twilio-konto. Det kostnadsfria kontot räcker för att du ska komma igång.

1. Gå till [twilio.com](https://www.twilio.com?azure-portal=true).

1. Klicka på den röda knappen **Sign Up** (registrera dig) uppe till höger.

1. Fyll i dina uppgifter och klicka på **Get Started** (kom igång).

1. Du måste verifiera ditt telefonnummer. Med Twilios kostnadsfria konto kan du bara skicka meddelanden till verifierade telefonnummer, så att de inte ska kunna användas för att skicka skräppost. Twilio skickar en verifieringskod som du måste ange för att verifiera din telefon.

1. Välj fliken **Produkter**, klicka på **Programmable SMS** (Programmerbart SMS) och klicka sedan på **Fortsätt**.

1. Ange ett namn för ditt första projekt, till exempel ”Hej världen”, och klicka sedan på **Fortsätt**.

1. Hoppa över det här steget om du vill bjuda in en teammedlem.

1. Expandera panelen **Projektinformation** från meddelandeinstrumentpanelen i Twilio.

1. Anteckna värdena för **KONTO-SID** och **AUTENTISERINGSTOKEN** eftersom du behöver dem senare.

    När du skapar ett Twilio-konto tilldelas du ett telefonnummer som du kan skicka meddelanden från. Du hittar det här telefonnumret på Twilio-instrumentpanelen **Phone Numbers** (telefonnummer).

1. Välj ellipserna längst ned i den vänstra menyn på Twilio-webbplatsen. Välj sedan *SUPER NETWORK -> Phone Numbers*. Du kan fästa den här instrumentpanelen vid den vänstra menyn med en knappnålsikon. Du ser ditt Twilio-nummer under *Manage Numbers -> Active Numbers* (hantera nummer -> aktiva nummer).

    ![Hitta ditt Twilio-nummer](../media/7-twilio-find-number.png)

    > [!TIP]
    > Om du inte ännu har ett aktivt nummer väljer du **Get Started** (Kom igång) på sidan Aktiva nummer för att starta processen med att skapa ett nummer.

1. Notera ditt aktiva telefonnummer. Det kommer att användas senare i modulen.


> [!NOTE]
> När du registrerar dig tilldelas du ett Twilio-telefonnummer som används för att skicka SMS. I vissa länder kan det hända att dessa nummer inte kan skicka SMS. Twilio-dokumentationen anger [vilka länder som har begränsningar](https://support.twilio.com/hc/articles/223183068-Twilio-international-phone-number-availability-and-their-capabilities?azure-portal=true) och visar olika sätt att skicka SMS med hjälp av ett [internationellt nummer eller alfanumeriskt avsändar-ID](https://support.twilio.com/hc/articles/226690868-Using-Twilio-when-SMS-numbers-are-unavailable-in-your-country?azure-portal=true).

## <a name="launch-visual-studio"></a>Starta Visual Studio

I den här modulen kommer du att utveckla mobilappen och Azure Functions-appen i Visual Studio 2017, som är tillgängligt via en virtuell dator. Även om du kan skapa Xamarin.Forms-appar för körning i iOS, Android och Universal Windows Platform (UWP) så ligger fokus i den här modulen på UWP så att appen fungerar i den virtuella labbdatorn.

Starta Visual Studio 2017 från den virtuella datorns startmeny eller från genvägen på skrivbordet.

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du skapat ett Twilio-konto som du ska använda till att skicka SMS, och du har startat Visual Studio. I nästa steg får du lära dig att skapa en Xamarin.Forms-app och lägga till NuGet-paketet Xamarin.Essentials.

> [!IMPORTANT]
> Notera värdena för **KONTO-SID** och **AUTENTISERINGSTOKEN** och **Active Phone Number** (Aktivt telefonnummer) i Twilio som du samlade in i den här enheten, eftersom du behöver dessa värden senare.
