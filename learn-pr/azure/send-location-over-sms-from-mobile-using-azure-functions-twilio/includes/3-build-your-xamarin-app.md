<span data-ttu-id="94056-101">Mobilappen är nu en enkel ”Hello World”-app.</span><span class="sxs-lookup"><span data-stu-id="94056-101">At this point, the mobile app is a simple "Hello World" app.</span></span> <span data-ttu-id="94056-102">Här kommer du att lägga till ett användargränssnitt och lite grundläggande programlogik.</span><span class="sxs-lookup"><span data-stu-id="94056-102">In this unit, you add the UI and some basic application logic.</span></span>

<span data-ttu-id="94056-103">Appens användargränssnittet kommer att bestå av följande:</span><span class="sxs-lookup"><span data-stu-id="94056-103">The UI for the app will consist of:</span></span>

* <span data-ttu-id="94056-104">en textinmatningskontroll där du skriver in telefonnummer</span><span class="sxs-lookup"><span data-stu-id="94056-104">A text-entry control to enter some phone numbers.</span></span>
* <span data-ttu-id="94056-105">en knapp för att skicka din plats till numren via en Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="94056-105">A button to send your location to those numbers using an Azure function.</span></span>
* <span data-ttu-id="94056-106">en etikett som visar aktuell status för användaren, som att platsen skickas eller att sändningen är färdig.</span><span class="sxs-lookup"><span data-stu-id="94056-106">A label that will show a message to the user of the current status, such as the location being sent and location sent successfully.</span></span>

<span data-ttu-id="94056-107">Xamarin.Forms har stöd för ett designmönster som kallas Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="94056-107">Xamarin.Forms supports a design pattern called Model-View-ViewModel (MVVM).</span></span> <span data-ttu-id="94056-108">Du kan läsa mer om MVVM i [dokumentationen om Xamarin MVVM](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm), men i grunden går den ut på att varje sida (View) har en ViewModel som exponerar egenskaper och beteende.</span><span class="sxs-lookup"><span data-stu-id="94056-108">You can read more about MVVM in the [Xamarin MVVM docs](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm), but the essence of it is, each page (View) has a ViewModel that exposes properties and behavior.</span></span>

<span data-ttu-id="94056-109">ViewModel-egenskaperna är ”kopplade” till komponenter i användargränssnittet via namnen, och den här bindningen synkroniserar data mellan View och ViewModel.</span><span class="sxs-lookup"><span data-stu-id="94056-109">ViewModel properties are 'bound' to components on the UI by name, and this binding synchronizes data between the View and ViewModel.</span></span> <span data-ttu-id="94056-110">Egenskapen `string` för en ViewModel med namn `Name` kan till exempel vara bunden till egenskapen `Text` för ett textinmatningsfält i användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="94056-110">For example, a `string` property on a ViewModel called `Name` could be bound to the `Text` property of a text-entry control on the UI.</span></span> <span data-ttu-id="94056-111">Värdet för egenskapen `Name` visas i textinmatningskontrollen, och när användaren ändrar texten i gränssnittet uppdateras egenskapen `Name`.</span><span class="sxs-lookup"><span data-stu-id="94056-111">The text-entry control shows the value in the `Name` property and, when the user changes the text in the UI, the `Name` property is updated.</span></span> <span data-ttu-id="94056-112">Om värdet för egenskapen `Name` ändras i ViewModel-objektet genereras en händelse som informerar användargränssnittet om uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="94056-112">If the value of the `Name` property is changed in the ViewModel, an event is raised to tell the UI to update.</span></span>

<span data-ttu-id="94056-113">ViewModel-beteendet exponeras i form av kommandoegenskaper, där ett kommando är ett objekt som omsluter en åtgärd som körs när kommandot anropas.</span><span class="sxs-lookup"><span data-stu-id="94056-113">ViewModel behavior is exposed as command properties, a command being an object that wraps an action that is executed when the command is invoked.</span></span> <span data-ttu-id="94056-114">Dessa kommandon är bundna via namn till kontroller som knappar, och när du trycker på knappen så anropas kommandot.</span><span class="sxs-lookup"><span data-stu-id="94056-114">These commands are bound by name to controls like buttons, and tapping a button will invoke the command.</span></span>

## <a name="create-a-base-viewmodel"></a><span data-ttu-id="94056-115">Skapa en grundläggande ViewModel</span><span class="sxs-lookup"><span data-stu-id="94056-115">Create a base ViewModel</span></span>

<span data-ttu-id="94056-116">ViewModel-objekt implementerar gränssnittet `INotifyPropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="94056-116">ViewModels all implement the `INotifyPropertyChanged` interface.</span></span> <span data-ttu-id="94056-117">Det här gränssnittet har en enda händelse, `PropertyChanged`, som används till att informera användargränssnittet om uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="94056-117">This interface has a single event, `PropertyChanged`, which is used to notify the UI of any updates.</span></span> <span data-ttu-id="94056-118">Den här händelsen har händelseargument som innehåller namnet på den egenskap som ändrats.</span><span class="sxs-lookup"><span data-stu-id="94056-118">This event has event args that contain the name of the property that has changed.</span></span> <span data-ttu-id="94056-119">Det är vanligt att skapa en ViewModel-basklass som implementerar det här gränssnittet och tillhandahåller några hjälpmetoder.</span><span class="sxs-lookup"><span data-stu-id="94056-119">It's common practice to create a base ViewModel class implementing this interface and providing some helper methods.</span></span>

1. <span data-ttu-id="94056-120">Skapa en ny klass i .NET-standardprojektet `ImHere` med namnet `BaseViewModel` genom att högerklicka på projektet och sedan välja *Lägg till -> Klass*. Ge den nya klassen namnet ”BaseViewModel” och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="94056-120">Create a new class in the `ImHere` .NET standard project called `BaseViewModel` by right-clicking on the project, and then selecting *Add->Class...*. Name the new class "BaseViewModel" and click **Add**.</span></span>

2. <span data-ttu-id="94056-121">Gör klassen `public` och härled från `INotifyPropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="94056-121">Make the class `public` and derive from `INotifyPropertyChanged`.</span></span> <span data-ttu-id="94056-122">Du måste lägga till ett användningsdirektiv för `System.ComponentModel`.</span><span class="sxs-lookup"><span data-stu-id="94056-122">You'll need to add a using directive for `System.ComponentModel`.</span></span>

3. <span data-ttu-id="94056-123">Implementera gränssnittet `INotifyPropertyChanged` genom att lägga till händelsen `PropertyChanged`:</span><span class="sxs-lookup"><span data-stu-id="94056-123">Implement the `INotifyPropertyChanged` interface by adding the `PropertyChanged` event:</span></span>

    ```cs
    public event PropertyChangedEventHandler PropertyChanged;
    ```

4. <span data-ttu-id="94056-124">Det vanliga mönstret för ViewModel-egenskaper är att ha en offentlig egenskap med ett privat stödfält.</span><span class="sxs-lookup"><span data-stu-id="94056-124">The common pattern for ViewModel properties is to have a public property with a private backing field.</span></span> <span data-ttu-id="94056-125">I set-metoden för egenskapen kontrolleras stödfältet mot det nya värdet.</span><span class="sxs-lookup"><span data-stu-id="94056-125">In the property setter, the backing field is checked against the new value.</span></span> <span data-ttu-id="94056-126">Om det nya värdet skiljer sig från stödfältet uppdateras stödfältet och händelsen `PropertyChanged` genereras.</span><span class="sxs-lookup"><span data-stu-id="94056-126">If the new value is different to the backing field, the backing field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="94056-127">Den här logiken är enkel att lägga ut i en metod, så lägg till metoden `Set`.</span><span class="sxs-lookup"><span data-stu-id="94056-127">This logic is easy to factor out into a method, so add the `Set` method.</span></span> <span data-ttu-id="94056-128">Du måste lägga till ett användningsdirektiv för namnområdet `System.Runtime.CompilerServices`.</span><span class="sxs-lookup"><span data-stu-id="94056-128">You'll need to add a using directive for the `System.Runtime.CompilerServices` namespace.</span></span>

    ```cs
    protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(field, value)) return;
        field = value;
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    ```

    <span data-ttu-id="94056-129">Den här metoden tar en referens till stödfältet, det nya värdet och egenskapsnamnet som indata.</span><span class="sxs-lookup"><span data-stu-id="94056-129">This method takes a reference to the backing field, the new value, and the property name.</span></span> <span data-ttu-id="94056-130">Om fältet inte har ändrats returnerar metoden, annars uppdateras fältet och händelsen `PropertyChanged` genereras.</span><span class="sxs-lookup"><span data-stu-id="94056-130">If the field hasn't changed, the method returns, otherwise, the field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="94056-131">Parametern `propertyName` i metoden `Set` är en standardparameter och markeras med attributet `CallerMemberName`.</span><span class="sxs-lookup"><span data-stu-id="94056-131">The `propertyName` parameter on the `Set` method is a default parameter and is marked with the `CallerMemberName` attribute.</span></span> <span data-ttu-id="94056-132">När den här metoden anropas från set-metoden för en egenskap lämnas parametern normalt som standardvärde.</span><span class="sxs-lookup"><span data-stu-id="94056-132">When this method is called from a property setter, this parameter is normally left as the default value.</span></span> <span data-ttu-id="94056-133">Kompilatorn sätter då automatiskt värdet på parametern till den anropande egenskapens namn.</span><span class="sxs-lookup"><span data-stu-id="94056-133">The compiler will then automatically set the parameter value to be the name of the calling property.</span></span>

<span data-ttu-id="94056-134">Nedan ser du klassens fullständiga kod.</span><span class="sxs-lookup"><span data-stu-id="94056-134">The full code for this class is below.</span></span>

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

## <a name="create-a-viewmodel-for-the-page"></a><span data-ttu-id="94056-135">Skapa en ViewModel för sidan</span><span class="sxs-lookup"><span data-stu-id="94056-135">Create a ViewModel for the page</span></span>

<span data-ttu-id="94056-136">`MainPage` har en textinmatningskontroll för telefonnummer och en etikett för att visa ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="94056-136">The `MainPage` will have a text-entry control for phone numbers and a label to display a message.</span></span> <span data-ttu-id="94056-137">De här kontrollerna binds till egenskaper för en ViewModel.</span><span class="sxs-lookup"><span data-stu-id="94056-137">These controls will be bound to properties on a ViewModel.</span></span>

1. <span data-ttu-id="94056-138">Skapa en klassen med namnet `MainViewModel` i .NET-standardprojektet `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="94056-138">Create a class called `MainViewModel` in the `ImHere` .NET standard project.</span></span>

2. <span data-ttu-id="94056-139">Gör den här klassen public och härled från `BaseViewModel`.</span><span class="sxs-lookup"><span data-stu-id="94056-139">Make this class public and derive from `BaseViewModel`.</span></span>

3. <span data-ttu-id="94056-140">Lägg till två `string`-egenskaper, `PhoneNumbers` och `Message`, och låt dem ha var sitt stödfält.</span><span class="sxs-lookup"><span data-stu-id="94056-140">Add two `string` properties, `PhoneNumbers` and `Message`, each with a backing field.</span></span> <span data-ttu-id="94056-141">I set-metoden för egenskaperna använder du basklassens `Set`-metod till att uppdatera värdet och generera händelsen `PropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="94056-141">In the property setter, use the base class `Set` method to update the value and raise the `PropertyChanged` event.</span></span>

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

4. <span data-ttu-id="94056-142">Lägg till en skrivskyddad kommandoegenskap med namnet `SendLocationCommand`.</span><span class="sxs-lookup"><span data-stu-id="94056-142">Add a read-only command property called `SendLocationCommand`.</span></span> <span data-ttu-id="94056-143">Det här kommandot har typen `ICommand` från namnområdet `System.Windows.Input`.</span><span class="sxs-lookup"><span data-stu-id="94056-143">This command will have a type of `ICommand` from the `System.Windows.Input` namespace.</span></span>

    ```cs
    public ICommand SendLocationCommand { get; }
    ```

5. <span data-ttu-id="94056-144">Lägg till en konstruktor i klassen, och initiera `SendLocationCommand` som nytt `Command` från namnområdet `Xamarin.Forms` i konstruktorn.</span><span class="sxs-lookup"><span data-stu-id="94056-144">Add a constructor to the class, and in this constructor, initialize the `SendLocationCommand` as a new `Command` from the `Xamarin.Forms` namespace.</span></span> <span data-ttu-id="94056-145">Konstruktorn för kommandot tar som indata en `Action` som ska köras när kommandot anropas, så skapa en `async`-metod med namnet `SendLocation` och skicka en lambdafunktion med `await`-kod för anropet till konstruktorn.</span><span class="sxs-lookup"><span data-stu-id="94056-145">The constructor for this command takes an `Action` to run when the command is invoked, so create an `async` method called `SendLocation` and pass a lambda function that `await`s this call to the constructor.</span></span> <span data-ttu-id="94056-146">Innehållet i metoden `SendLocation` implementeras i senare övningar i den här modulen.</span><span class="sxs-lookup"><span data-stu-id="94056-146">The body of the `SendLocation` method will be implemented in later units in this module.</span></span> <span data-ttu-id="94056-147">Du måste lägga till ett användningsdirektiv för namnområdet `System.Threading.Tasks` så att du kan returnera en `Task`.</span><span class="sxs-lookup"><span data-stu-id="94056-147">You'll need to add a using directive for the `System.Threading.Tasks` namespace to be able to return a `Task`.</span></span>

    ```cs
    public MainViewModel()
    {
        SendLocationCommand = new Command(async () => await SendLocation());
    }

    async Task SendLocation()
    {
    }
    ```

<span data-ttu-id="94056-148">Nedan ser du klassens kod.</span><span class="sxs-lookup"><span data-stu-id="94056-148">The code for this class is shown below.</span></span>

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

## <a name="create-the-user-interface"></a><span data-ttu-id="94056-149">Skapa användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="94056-149">Create the user interface</span></span>

<span data-ttu-id="94056-150">Du kan skapa Xamarin.Forms-gränssnitt med XAML.</span><span class="sxs-lookup"><span data-stu-id="94056-150">Xamarin.Forms UIs can be built using XAML.</span></span>

1. <span data-ttu-id="94056-151">Öppna filen `MainPage.xaml` från projektet `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="94056-151">Open the `MainPage.xaml` file from the `ImHere` project.</span></span> <span data-ttu-id="94056-152">Sidan öppnas i XAML-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="94056-152">The page will open in the XAML editor.</span></span>

    <span data-ttu-id="94056-153">Obs! Projektet `ImHere.UWP` innehåller också en fil med namnet `MainPage.xaml`.</span><span class="sxs-lookup"><span data-stu-id="94056-153">NOTE - The `ImHere.UWP` project also contains a file called `MainPage.xaml`.</span></span> <span data-ttu-id="94056-154">Se till att du redigerar filen i .NET-standardbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="94056-154">Make sure you're editing the one in the .NET standard library.</span></span>

2. <span data-ttu-id="94056-155">Innan du kan binda kontroller till egenskaperna i en ViewModel måste du ange en ViewModel-instans bindningskontext för sidan.</span><span class="sxs-lookup"><span data-stu-id="94056-155">Before you can bind controls to properties on a ViewModel, you have to set an instance of the ViewModel as the binding context of the page.</span></span> <span data-ttu-id="94056-156">Lägg till följande XAML i `ContentPage` på översta nivån.</span><span class="sxs-lookup"><span data-stu-id="94056-156">Add the following XAML inside the top-level `ContentPage`.</span></span>

    ```xml
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    ```

3. <span data-ttu-id="94056-157">Ta bort innehållet i `StackLayout` och lägg till lite utfyllnad så att gränssnittet blir snyggare.</span><span class="sxs-lookup"><span data-stu-id="94056-157">Delete the contents of the `StackLayout` and add some padding inside it to help make the UI look better.</span></span>

    ```xml
    <StackLayout Padding="20">
    </StackLayout>
    ```

4. <span data-ttu-id="94056-158">Lägg till en `Editor`-kontroll där användaren kan lägga till telefonnummer i `StackLayout`, med en `Label` ovanför som beskriver vad kontrollen används till.</span><span class="sxs-lookup"><span data-stu-id="94056-158">Add an `Editor` control that the user can use to add phone numbers to the `StackLayout`, with a `Label` above to describe what the entry control is for.</span></span> <span data-ttu-id="94056-159">Kontrollerna i `StackLayout` ordnas antingen vågrätt eller lodrätt i den ordning som kontrollerna läggs till, så om du lägger till `Label` först hamnar den ovanför `Editor`.</span><span class="sxs-lookup"><span data-stu-id="94056-159">`StackLayout`'s stack child controls either horizontally or vertically in the order in which the controls are added, so adding the `Label` first will put it above the `Editor`.</span></span> <span data-ttu-id="94056-160">`Editor`-kontroller har flera rader så att användaren kan ange flera telefonnummer, ett per rad.</span><span class="sxs-lookup"><span data-stu-id="94056-160">`Editor` controls are multi-line entry controls, allowing the user to enter multiple phone numbers, one per line.</span></span>

    ```xml
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Center"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
    </StackLayout>
    ```

    <span data-ttu-id="94056-161">Egenskapen `Text` för `Editor` är bunden till egenskapen `PhoneNumbers` för `MainViewModel`.</span><span class="sxs-lookup"><span data-stu-id="94056-161">The `Text` property on the `Editor` is bound to the `PhoneNumbers` property on the `MainViewModel`.</span></span> <span data-ttu-id="94056-162">Syntaxen för bindningen är att sätta egenskapsvärdet till `"{Binding <property name>}"`.</span><span class="sxs-lookup"><span data-stu-id="94056-162">The syntax for binding is to set the property value to `"{Binding <property name>}"`.</span></span> <span data-ttu-id="94056-163">Klamrarna anger för XAML-kompilatorn att det här värdet är speciellt och ska behandlas annorlunda än ett vanligt `string`-värde.</span><span class="sxs-lookup"><span data-stu-id="94056-163">The curly braces will tell the XAML compiler that this value is special and should be treated differently from a simple `string`.</span></span>

5. <span data-ttu-id="94056-164">Lägg till en `Button` som skickar användarens plats under `Editor`-kontrollen.</span><span class="sxs-lookup"><span data-stu-id="94056-164">Add a `Button` to send the user's location below the `Editor`.</span></span>

    ```xml
    <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
            Command="{Binding SendLocationCommand}" />
    ```

    <span data-ttu-id="94056-165">Egenskapen `Command` är bunden till kommandot `SendLocationCommand` i ViewModel-objektet.</span><span class="sxs-lookup"><span data-stu-id="94056-165">The `Command` property is bound to the `SendLocationCommand` command on the ViewModel.</span></span> <span data-ttu-id="94056-166">När du trycker på knappen körs kommandot.</span><span class="sxs-lookup"><span data-stu-id="94056-166">When the button is tapped, the command will be executed.</span></span>

6. <span data-ttu-id="94056-167">Lägg till en `Label` som visar statusmeddelanden under din `Button`.</span><span class="sxs-lookup"><span data-stu-id="94056-167">Add a `Label` to show the status message below the `Button`.</span></span>

    ```xml
    <Label Text="{Binding Message}"
           HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    ```

<span data-ttu-id="94056-168">Nedan ser du hela koden för sidan.</span><span class="sxs-lookup"><span data-stu-id="94056-168">The full code for this page is below.</span></span>

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

<span data-ttu-id="94056-169">Kör appen så att du ser det nya användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="94056-169">Run the app to see the new UI.</span></span> <span data-ttu-id="94056-170">Om du vill kontrollera bindningarna kan du göra det genom att lägga till brytpunkter i egenskaperna eller i metoden `SendLocation`.</span><span class="sxs-lookup"><span data-stu-id="94056-170">If you want to validate the bindings at this point, you can do so by adding breakpoints to the properties or the `SendLocation` method.</span></span>

![Appens nya gränssnitt](../media-drafts/3-new-ui.png)

## <a name="summary"></a><span data-ttu-id="94056-172">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="94056-172">Summary</span></span>

<span data-ttu-id="94056-173">Nu har du lärt dig hur du skapar ett användargränssnitt för appen med XAML, tillsammans med en ViewModel som hanterar appens logik.</span><span class="sxs-lookup"><span data-stu-id="94056-173">In this unit, you learned how to create the UI for the app using XAML, along with a ViewModel to handle the applications logic.</span></span> <span data-ttu-id="94056-174">Du har också fått lära dig att binda ViewModel-objektet till gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="94056-174">You also learned how to bind the ViewModel to the UI.</span></span> <span data-ttu-id="94056-175">I nästa del kommer du att lägga till platssökning i din ViewModel.</span><span class="sxs-lookup"><span data-stu-id="94056-175">In the next unit, you add location lookup to the ViewModel.</span></span>