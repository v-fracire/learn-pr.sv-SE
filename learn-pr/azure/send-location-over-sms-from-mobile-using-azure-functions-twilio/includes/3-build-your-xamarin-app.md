Mobilappen är nu en enkel ”Hello World”-app. Här kommer du att lägga till ett användargränssnitt och lite grundläggande programlogik.

Appens användargränssnittet kommer att bestå av följande:

* en textinmatningskontroll där du skriver in telefonnummer
* en knapp för att skicka din plats till numren via en Azure-funktion
* en etikett som visar aktuell status för användaren, som att platsen skickas eller att sändningen är färdig.

Xamarin.Forms har stöd för ett designmönster som kallas Model-View-ViewModel (MVVM). Du kan läsa mer om MVVM i [dokumentationen om Xamarin MVVM](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm), men i grunden går den ut på att varje sida (View) har en ViewModel som exponerar egenskaper och beteende.

ViewModel-egenskaperna är ”kopplade” till komponenter i användargränssnittet via namnen, och den här bindningen synkroniserar data mellan View och ViewModel. Egenskapen `string` för en ViewModel med namn `Name` kan till exempel vara bunden till egenskapen `Text` för ett textinmatningsfält i användargränssnittet. Värdet för egenskapen `Name` visas i textinmatningskontrollen, och när användaren ändrar texten i gränssnittet uppdateras egenskapen `Name`. Om värdet för egenskapen `Name` ändras i ViewModel-objektet genereras en händelse som informerar användargränssnittet om uppdateringen.

ViewModel-beteendet exponeras i form av kommandoegenskaper, där ett kommando är ett objekt som omsluter en åtgärd som körs när kommandot anropas. Dessa kommandon är bundna via namn till kontroller som knappar, och när du trycker på knappen så anropas kommandot.

## <a name="create-a-base-viewmodel"></a>Skapa en grundläggande ViewModel

ViewModel-objekt implementerar gränssnittet `INotifyPropertyChanged`. Det här gränssnittet har en enda händelse, `PropertyChanged`, som används till att informera användargränssnittet om uppdateringar. Den här händelsen har händelseargument som innehåller namnet på den egenskap som ändrats. Det är vanligt att skapa en ViewModel-basklass som implementerar det här gränssnittet och tillhandahåller några hjälpmetoder.

1. Skapa en ny klass i .NET-standardprojektet `ImHere` med namnet `BaseViewModel` genom att högerklicka på projektet och sedan välja *Lägg till -> Klass*. Ge den nya klassen namnet ”BaseViewModel” och klicka på **Lägg till**.

2. Gör klassen `public` och härled från `INotifyPropertyChanged`. Du måste lägga till ett användningsdirektiv för `System.ComponentModel`.

3. Implementera gränssnittet `INotifyPropertyChanged` genom att lägga till händelsen `PropertyChanged`:

    ```cs
    public event PropertyChangedEventHandler PropertyChanged;
    ```

4. Det vanliga mönstret för ViewModel-egenskaper är att ha en offentlig egenskap med ett privat stödfält. I set-metoden för egenskapen kontrolleras stödfältet mot det nya värdet. Om det nya värdet skiljer sig från stödfältet uppdateras stödfältet och händelsen `PropertyChanged` genereras. Den här logiken är enkel att lägga ut i en metod, så lägg till metoden `Set`. Du måste lägga till ett användningsdirektiv för namnområdet `System.Runtime.CompilerServices`.

    ```cs
    protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(field, value)) return;
        field = value;
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    ```

    Den här metoden tar en referens till stödfältet, det nya värdet och egenskapsnamnet som indata. Om fältet inte har ändrats returnerar metoden, annars uppdateras fältet och händelsen `PropertyChanged` genereras. Parametern `propertyName` i metoden `Set` är en standardparameter och markeras med attributet `CallerMemberName`. När den här metoden anropas från set-metoden för en egenskap lämnas parametern normalt som standardvärde. Kompilatorn sätter då automatiskt värdet på parametern till den anropande egenskapens namn.

Nedan ser du klassens fullständiga kod.

```cs
using System.ComponentModel;
using System.Runtime.CompilerServices;

namespace ImHere
{
    public class BaseViewModel : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;

        protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
        {
            if (Equals(field, value)) return;
            field = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

## <a name="create-a-viewmodel-for-the-page"></a>Skapa en ViewModel för sidan

`MainPage` har en textinmatningskontroll för telefonnummer och en etikett för att visa ett meddelande. De här kontrollerna binds till egenskaper för en ViewModel.

1. Skapa en klassen med namnet `MainViewModel` i .NET-standardprojektet `ImHere`.

2. Gör den här klassen public och härled från `BaseViewModel`.

3. Lägg till två `string`-egenskaper, `PhoneNumbers` och `Message`, och låt dem ha var sitt stödfält. I set-metoden för egenskaperna använder du basklassens `Set`-metod till att uppdatera värdet och generera händelsen `PropertyChanged`.

   ```cs
    string message = "";
    public string Message
    {
        get => message;
        set => Set(ref message, value);
    }

    string phoneNumbers = "";
    public string PhoneNumbers
    {
        get => phoneNumbers;
        set => Set(ref phoneNumbers, value);
    }
   ```

4. Lägg till en skrivskyddad kommandoegenskap med namnet `SendLocationCommand`. Det här kommandot har typen `ICommand` från namnområdet `System.Windows.Input`.

    ```cs
    public ICommand SendLocationCommand { get; }
    ```

5. Lägg till en konstruktor i klassen, och initiera `SendLocationCommand` som nytt `Command` från namnområdet `Xamarin.Forms` i konstruktorn. Konstruktorn för kommandot tar som indata en `Action` som ska köras när kommandot anropas, så skapa en `async`-metod med namnet `SendLocation` och skicka en lambdafunktion med `await`-kod för anropet till konstruktorn. Innehållet i metoden `SendLocation` implementeras i senare övningar i den här modulen. Du måste lägga till ett användningsdirektiv för namnområdet `System.Threading.Tasks` så att du kan returnera en `Task`.

    ```cs
    public MainViewModel()
    {
        SendLocationCommand = new Command(async () => await SendLocation());
    }

    async Task SendLocation()
    {
    }
    ```

Nedan ser du klassens kod.

```cs
using System.Threading.Tasks;
using System.Windows.Input;
using Xamarin.Forms;

namespace ImHere
{
    public class MainViewModel : BaseViewModel
    {
        string message = "";
        public string Message
        {
            get => message;
            set => Set(ref message, value);
        }

        string phoneNumbers = "";
        public string PhoneNumbers
        {
            get => phoneNumbers;
            set => Set(ref phoneNumbers, value);
        }

        public MainViewModel()
        {
            SendLocationCommand = new Command(async () => await SendLocation());
        }

        public ICommand SendLocationCommand { get; }

        async Task SendLocation()
        {
        }
    }
}
```

## <a name="create-the-user-interface"></a>Skapa användargränssnittet

Du kan skapa Xamarin.Forms-gränssnitt med XAML.

1. Öppna filen `MainPage.xaml` från projektet `ImHere`. Sidan öppnas i XAML-redigeraren.

    Obs! Projektet `ImHere.UWP` innehåller också en fil med namnet `MainPage.xaml`. Se till att du redigerar filen i .NET-standardbiblioteket.

2. Innan du kan binda kontroller till egenskaperna i en ViewModel måste du ange en ViewModel-instans bindningskontext för sidan. Lägg till följande XAML i `ContentPage` på översta nivån.

    ```xml
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    ```

3. Ta bort innehållet i `StackLayout` och lägg till lite utfyllnad så att gränssnittet blir snyggare.

    ```xml
    <StackLayout Padding="20">
    </StackLayout>
    ```

4. Lägg till en `Editor`-kontroll där användaren kan lägga till telefonnummer i `StackLayout`, med en `Label` ovanför som beskriver vad kontrollen används till. Kontrollerna i `StackLayout` ordnas antingen vågrätt eller lodrätt i den ordning som kontrollerna läggs till, så om du lägger till `Label` först hamnar den ovanför `Editor`. `Editor`-kontroller har flera rader så att användaren kan ange flera telefonnummer, ett per rad.

    ```xml
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Center"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
    </StackLayout>
    ```

    Egenskapen `Text` för `Editor` är bunden till egenskapen `PhoneNumbers` för `MainViewModel`. Syntaxen för bindningen är att sätta egenskapsvärdet till `"{Binding <property name>}"`. Klamrarna anger för XAML-kompilatorn att det här värdet är speciellt och ska behandlas annorlunda än ett vanligt `string`-värde.

5. Lägg till en `Button` som skickar användarens plats under `Editor`-kontrollen.

    ```xml
    <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
            Command="{Binding SendLocationCommand}" />
    ```

    Egenskapen `Command` är bunden till kommandot `SendLocationCommand` i ViewModel-objektet. När du trycker på knappen körs kommandot.

6. Lägg till en `Label` som visar statusmeddelanden under din `Button`.

    ```xml
    <Label Text="{Binding Message}"
           HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    ```

Nedan ser du hela koden för sidan.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ImHere"
             x:Class="ImHere.MainPage">
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Center"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
        <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
                Command="{Binding SendLocationCommand}" />
        <Label Text="{Binding Message}"
               HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Kör appen så att du ser det nya användargränssnittet. Om du vill kontrollera bindningarna kan du göra det genom att lägga till brytpunkter i egenskaperna eller i metoden `SendLocation`.

![Appens nya gränssnitt](../media-drafts/3-new-ui.png)

## <a name="summary"></a>Sammanfattning

Nu har du lärt dig hur du skapar ett användargränssnitt för appen med XAML, tillsammans med en ViewModel som hanterar appens logik. Du har också fått lära dig att binda ViewModel-objektet till gränssnittet. I nästa del kommer du att lägga till platssökning i din ViewModel.