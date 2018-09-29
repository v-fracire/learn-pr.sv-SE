<span data-ttu-id="c76d9-101">Mobilappen är nu en enkel ”Hello World”-app.</span><span class="sxs-lookup"><span data-stu-id="c76d9-101">At this point, the mobile app is a simple "Hello World" app.</span></span> <span data-ttu-id="c76d9-102">Här kommer du att lägga till ett användargränssnitt och lite grundläggande programlogik.</span><span class="sxs-lookup"><span data-stu-id="c76d9-102">In this unit, you add the UI and some basic application logic.</span></span>

<span data-ttu-id="c76d9-103">Appens användargränssnittet kommer att bestå av följande:</span><span class="sxs-lookup"><span data-stu-id="c76d9-103">The UI for the app will consist of:</span></span>

- <span data-ttu-id="c76d9-104">en textinmatningskontroll där du skriver in telefonnummer</span><span class="sxs-lookup"><span data-stu-id="c76d9-104">A text-entry control to enter some phone numbers.</span></span>
- <span data-ttu-id="c76d9-105">en knapp för att skicka din plats till numren via en Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="c76d9-105">A button to send your location to those numbers using an Azure function.</span></span>
- <span data-ttu-id="c76d9-106">en etikett som visar aktuell status för användaren, som att platsen skickas eller att sändningen är färdig.</span><span class="sxs-lookup"><span data-stu-id="c76d9-106">A label that will show a message to the user of the current status, such as the location being sent and location sent successfully.</span></span>

<span data-ttu-id="c76d9-107">Xamarin.Forms har stöd för ett designmönster som kallas Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="c76d9-107">Xamarin.Forms supports a design pattern called Model-View-ViewModel (MVVM).</span></span> <span data-ttu-id="c76d9-108">Du kan läsa mer om MVVM i [dokumentationen om Xamarin MVVM](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm?azure-portal=true), men i grunden går den ut på att varje sida (View) har en ViewModel som exponerar egenskaper och beteende.</span><span class="sxs-lookup"><span data-stu-id="c76d9-108">You can read more about MVVM in the [Xamarin MVVM docs](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm?azure-portal=true), but the essence of it is, each page (View) has a ViewModel that exposes properties and behavior.</span></span>

<span data-ttu-id="c76d9-109">ViewModel-egenskaperna är ”kopplade” till komponenter i användargränssnittet via namnen, och den här bindningen synkroniserar data mellan View och ViewModel.</span><span class="sxs-lookup"><span data-stu-id="c76d9-109">ViewModel properties are 'bound' to components on the UI by name, and this binding synchronizes data between the View and ViewModel.</span></span> <span data-ttu-id="c76d9-110">Egenskapen `string` för en ViewModel med namn `Name` kan till exempel vara bunden till egenskapen `Text` för ett textinmatningsfält i användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c76d9-110">For example, a `string` property on a ViewModel called `Name` could be bound to the `Text` property of a text-entry control on the UI.</span></span> <span data-ttu-id="c76d9-111">Värdet för egenskapen `Name` visas i textinmatningskontrollen, och när användaren ändrar texten i gränssnittet uppdateras egenskapen `Name`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-111">The text-entry control shows the value in the `Name` property and, when the user changes the text in the UI, the `Name` property is updated.</span></span> <span data-ttu-id="c76d9-112">Om värdet för egenskapen `Name` ändras i ViewModel-objektet genereras en händelse som informerar användargränssnittet om uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="c76d9-112">If the value of the `Name` property is changed in the ViewModel, an event is raised to tell the UI to update.</span></span>

<span data-ttu-id="c76d9-113">ViewModel-beteendet exponeras i form av kommandoegenskaper, där ett kommando är ett objekt som omsluter en åtgärd som körs när kommandot anropas.</span><span class="sxs-lookup"><span data-stu-id="c76d9-113">ViewModel behavior is exposed as command properties, a command being an object that wraps an action that is executed when the command is invoked.</span></span> <span data-ttu-id="c76d9-114">Dessa kommandon är bundna via namn till kontroller som knappar, och när du trycker på knappen så anropas kommandot.</span><span class="sxs-lookup"><span data-stu-id="c76d9-114">These commands are bound by name to controls like buttons, and tapping a button will invoke the command.</span></span>

## <a name="create-a-base-viewmodel"></a><span data-ttu-id="c76d9-115">Skapa en grundläggande ViewModel</span><span class="sxs-lookup"><span data-stu-id="c76d9-115">Create a base ViewModel</span></span>

<span data-ttu-id="c76d9-116">ViewModel-objekt implementerar gränssnittet `INotifyPropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-116">ViewModels all implement the `INotifyPropertyChanged` interface.</span></span> <span data-ttu-id="c76d9-117">Det här gränssnittet har en enda händelse, `PropertyChanged`, som används till att informera användargränssnittet om uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="c76d9-117">This interface has a single event, `PropertyChanged`, which is used to notify the UI of any updates.</span></span> <span data-ttu-id="c76d9-118">Den här händelsen har händelseargument som innehåller namnet på den egenskap som ändrats.</span><span class="sxs-lookup"><span data-stu-id="c76d9-118">This event has event args that contain the name of the property that has changed.</span></span> <span data-ttu-id="c76d9-119">Det är vanligt att skapa en ViewModel-basklass som implementerar det här gränssnittet och tillhandahåller några hjälpmetoder.</span><span class="sxs-lookup"><span data-stu-id="c76d9-119">It's common practice to create a base ViewModel class implementing this interface and providing some helper methods.</span></span>

1. <span data-ttu-id="c76d9-120">Skapa en ny klass i .NET-standardprojektet `ImHere` med namnet `BaseViewModel` genom att högerklicka på projektet och sedan välja *Lägg till -> Klass...*. Ge den nya klassen namnet ”BaseViewModel” och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c76d9-120">Create a new class in the `ImHere` .NET Standard project called `BaseViewModel` by right-clicking on the project, and then selecting *Add->Class...*. Name the new class "BaseViewModel" and click **Add**.</span></span>

1. <span data-ttu-id="c76d9-121">Gör klassen `public` och härled från `INotifyPropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-121">Make the class `public` and derive from `INotifyPropertyChanged`.</span></span> <span data-ttu-id="c76d9-122">Du måste lägga till ett användningsdirektiv för `System.ComponentModel`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-122">You'll need to add a using directive for `System.ComponentModel`.</span></span>

1. <span data-ttu-id="c76d9-123">Implementera gränssnittet `INotifyPropertyChanged` genom att lägga till händelsen `PropertyChanged`:</span><span class="sxs-lookup"><span data-stu-id="c76d9-123">Implement the `INotifyPropertyChanged` interface by adding the `PropertyChanged` event:</span></span>

    ```cs
    public event PropertyChangedEventHandler PropertyChanged;
    ```

1. <span data-ttu-id="c76d9-124">Det vanliga mönstret för ViewModel-egenskaper är att ha en offentlig egenskap med ett privat stödfält.</span><span class="sxs-lookup"><span data-stu-id="c76d9-124">The common pattern for ViewModel properties is to have a public property with a private backing field.</span></span> <span data-ttu-id="c76d9-125">I set-metoden för egenskapen kontrolleras stödfältet mot det nya värdet.</span><span class="sxs-lookup"><span data-stu-id="c76d9-125">In the property setter, the backing field is checked against the new value.</span></span> <span data-ttu-id="c76d9-126">Om det nya värdet skiljer sig från stödfältet uppdateras stödfältet och händelsen `PropertyChanged` genereras.</span><span class="sxs-lookup"><span data-stu-id="c76d9-126">If the new value is different to the backing field, the backing field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="c76d9-127">Den här logiken är enkel att lägga ut i en metod, så lägg till metoden `Set`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-127">This logic is easy to factor out into a method, so add the `Set` method.</span></span> <span data-ttu-id="c76d9-128">Du måste lägga till ett användningsdirektiv för namnområdet `System.Runtime.CompilerServices`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-128">You'll need to add a using directive for the `System.Runtime.CompilerServices` namespace.</span></span>

    ```cs
    protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(field, value)) return;
        field = value;
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    ```

    <span data-ttu-id="c76d9-129">Den här metoden tar en referens till stödfältet, det nya värdet och egenskapsnamnet som indata.</span><span class="sxs-lookup"><span data-stu-id="c76d9-129">This method takes a reference to the backing field, the new value, and the property name.</span></span> <span data-ttu-id="c76d9-130">Om fältet inte har ändrats returnerar metoden, annars uppdateras fältet och händelsen `PropertyChanged` genereras.</span><span class="sxs-lookup"><span data-stu-id="c76d9-130">If the field hasn't changed, the method returns, otherwise, the field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="c76d9-131">Parametern `propertyName` i metoden `Set` är en standardparameter och markeras med attributet `CallerMemberName`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-131">The `propertyName` parameter on the `Set` method is a default parameter and is marked with the `CallerMemberName` attribute.</span></span> <span data-ttu-id="c76d9-132">När den här metoden anropas från set-metoden för en egenskap lämnas parametern normalt som standardvärde.</span><span class="sxs-lookup"><span data-stu-id="c76d9-132">When this method is called from a property setter, this parameter is normally left as the default value.</span></span> <span data-ttu-id="c76d9-133">Kompilatorn sätter då automatiskt värdet på parametern till den anropande egenskapens namn.</span><span class="sxs-lookup"><span data-stu-id="c76d9-133">The compiler will then automatically set the parameter value to be the name of the calling property.</span></span>

<span data-ttu-id="c76d9-134">Nedan ser du klassens fullständiga kod.</span><span class="sxs-lookup"><span data-stu-id="c76d9-134">The full code for this class is below.</span></span>

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

## <a name="create-a-viewmodel-for-the-page"></a><span data-ttu-id="c76d9-135">Skapa en ViewModel för sidan</span><span class="sxs-lookup"><span data-stu-id="c76d9-135">Create a ViewModel for the page</span></span>

<span data-ttu-id="c76d9-136">`MainPage` har en textinmatningskontroll för telefonnummer och en etikett för att visa ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="c76d9-136">The `MainPage` will have a text-entry control for phone numbers and a label to display a message.</span></span> <span data-ttu-id="c76d9-137">De här kontrollerna binds till egenskaper för en ViewModel.</span><span class="sxs-lookup"><span data-stu-id="c76d9-137">These controls will be bound to properties on a ViewModel.</span></span>

1. <span data-ttu-id="c76d9-138">Skapa en klassen med namnet `MainViewModel` i .NET-standardprojektet `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-138">Create a class called `MainViewModel` in the `ImHere` .NET Standard project.</span></span>

1. <span data-ttu-id="c76d9-139">Gör den här klassen public och härled från `BaseViewModel`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-139">Make this class public and derive from `BaseViewModel`.</span></span>

1. <span data-ttu-id="c76d9-140">Lägg till två `string`-egenskaper, `PhoneNumbers` och `Message`, och låt dem ha var sitt stödfält.</span><span class="sxs-lookup"><span data-stu-id="c76d9-140">Add two `string` properties, `PhoneNumbers` and `Message`, each with a backing field.</span></span> <span data-ttu-id="c76d9-141">I set-metoden för egenskaperna använder du basklassens `Set`-metod till att uppdatera värdet och generera händelsen `PropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-141">In the property setter, use the base class `Set` method to update the value and raise the `PropertyChanged` event.</span></span>

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

1. <span data-ttu-id="c76d9-142">Lägg till en skrivskyddad kommandoegenskap med namnet `SendLocationCommand`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-142">Add a read-only command property called `SendLocationCommand`.</span></span> <span data-ttu-id="c76d9-143">Det här kommandot har typen `ICommand` från namnområdet `System.Windows.Input`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-143">This command will have a type of `ICommand` from the `System.Windows.Input` namespace.</span></span>

    ```cs
    public ICommand SendLocationCommand { get; }
    ```

1. <span data-ttu-id="c76d9-144">Lägg till en konstruktor i klassen och initiera `SendLocationCommand` som en ny Xamarin.Forms `Command` i denna konstruktor.</span><span class="sxs-lookup"><span data-stu-id="c76d9-144">Add a constructor to the class, and in this constructor, initialize the `SendLocationCommand` as a new Xamarin.Forms `Command`.</span></span> <span data-ttu-id="c76d9-145">Du måste lägga till ett användningsdirektiv för namnområdet `Xamarin.Forms`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-145">You will need to add a using directive for the `Xamarin.Forms` namespace.</span></span> <span data-ttu-id="c76d9-146">Konstruktorn för kommandot tar som indata en `Action` som ska köras när kommandot anropas, så skapa en `async`-metod med namnet `SendLocation` och skicka en lambdafunktion med `await`-kod för anropet till konstruktorn.</span><span class="sxs-lookup"><span data-stu-id="c76d9-146">The constructor for this command takes an `Action` to run when the command is invoked, so create an `async` method called `SendLocation` and pass a lambda function that `await`s this call to the constructor.</span></span> <span data-ttu-id="c76d9-147">Innehållet i metoden `SendLocation` implementeras i senare övningar i den här modulen.</span><span class="sxs-lookup"><span data-stu-id="c76d9-147">The body of the `SendLocation` method will be implemented in later units in this module.</span></span> <span data-ttu-id="c76d9-148">Du måste lägga till ett användningsdirektiv för namnområdet `System.Threading.Tasks` så att du kan returnera en `Task`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-148">You'll need to add a using directive for the `System.Threading.Tasks` namespace to be able to return a `Task`.</span></span>

    ```cs
    public MainViewModel()
    {
        SendLocationCommand = new Command(async () => await SendLocation());
    }

    async Task SendLocation()
    {
    }
    ```

<span data-ttu-id="c76d9-149">Nedan ser du klassens kod.</span><span class="sxs-lookup"><span data-stu-id="c76d9-149">The code for this class is shown below.</span></span>

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

## <a name="create-the-user-interface"></a><span data-ttu-id="c76d9-150">Skapa användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="c76d9-150">Create the user interface</span></span>

<span data-ttu-id="c76d9-151">Du kan skapa Xamarin.Forms-gränssnitt med XAML.</span><span class="sxs-lookup"><span data-stu-id="c76d9-151">Xamarin.Forms UIs can be built using XAML.</span></span>

1. <span data-ttu-id="c76d9-152">Öppna filen `MainPage.xaml` från projektet `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-152">Open the `MainPage.xaml` file from the `ImHere` project.</span></span> <span data-ttu-id="c76d9-153">Sidan öppnas i XAML-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="c76d9-153">The page will open in the XAML editor.</span></span>

    > [!NOTE]
    >  <span data-ttu-id="c76d9-154">Projektet `ImHere.UWP` innehåller också en fil med namnet `MainPage.xaml`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-154">The `ImHere.UWP` project also contains a file called `MainPage.xaml`.</span></span> <span data-ttu-id="c76d9-155">Se till att du redigerar filen i .NET-standardbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="c76d9-155">Make sure you're editing the one in the .NET Standard library.</span></span>

1. <span data-ttu-id="c76d9-156">Innan du kan binda kontroller till egenskaperna i en ViewModel måste du ange en ViewModel-instans bindningskontext för sidan.</span><span class="sxs-lookup"><span data-stu-id="c76d9-156">Before you can bind controls to properties on a ViewModel, you have to set an instance of the ViewModel as the binding context of the page.</span></span> <span data-ttu-id="c76d9-157">Lägg till följande XAML i `ContentPage` på översta nivån.</span><span class="sxs-lookup"><span data-stu-id="c76d9-157">Add the following XAML inside the top-level `ContentPage`.</span></span>

    ```xml
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    ```

1. <span data-ttu-id="c76d9-158">Skriv över `StackLayout` med följande kod:</span><span class="sxs-lookup"><span data-stu-id="c76d9-158">Overwrite the `StackLayout` with the following code:</span></span>

     ```xml
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Start"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
    </StackLayout>
    ```
    - <span data-ttu-id="c76d9-159">Kontrollen `Editor` används för att lägga till telefonnummer, och `Label` ovan beskriver syftet med det här fältet för användaren.</span><span class="sxs-lookup"><span data-stu-id="c76d9-159">The `Editor` control will be used to add phone numbers, and the `Label` above describes the purpose of this field to the user.</span></span> 
    - <span data-ttu-id="c76d9-160">Kontrollerna i `StackLayout` ordnas antingen vågrätt eller lodrätt i den ordning som kontrollerna läggs till, så om du lägger till `Label` först hamnar den ovanför `Editor`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-160">`StackLayout`'s stack child controls either horizontally or vertically in the order in which the controls are added, so adding the `Label` first will put it above the `Editor`.</span></span>
    - <span data-ttu-id="c76d9-161">`Editor`-kontroller har flera rader så att användaren kan ange flera telefonnummer, ett per rad.</span><span class="sxs-lookup"><span data-stu-id="c76d9-161">`Editor` controls are multi-line entry controls, allowing the user to enter multiple phone numbers, one per line.</span></span>

   

    <span data-ttu-id="c76d9-162">Egenskapen `Text` för `Editor` är bunden till egenskapen `PhoneNumbers` för `MainViewModel`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-162">The `Text` property on the `Editor` is bound to the `PhoneNumbers` property on the `MainViewModel`.</span></span> <span data-ttu-id="c76d9-163">Syntaxen för bindningen är att sätta egenskapsvärdet till `"{Binding <property name>}"`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-163">The syntax for binding is to set the property value to `"{Binding <property name>}"`.</span></span> <span data-ttu-id="c76d9-164">Klamrarna anger för XAML-kompilatorn att det här värdet är speciellt och ska behandlas annorlunda än ett vanligt `string`-värde.</span><span class="sxs-lookup"><span data-stu-id="c76d9-164">The curly braces will tell the XAML compiler that this value is special and should be treated differently from a simple `string`.</span></span>

1. <span data-ttu-id="c76d9-165">Lägg till en `Button` under `Editor` kontrollen.</span><span class="sxs-lookup"><span data-stu-id="c76d9-165">Add a `Button` below the `Editor` control.</span></span> <span data-ttu-id="c76d9-166">Vi använder den här knappen för att skicka användarens plats.</span><span class="sxs-lookup"><span data-stu-id="c76d9-166">We'll use this button to send the user's location.</span></span>

    ```xml
    <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
            Command="{Binding SendLocationCommand}" />
    ```

    <span data-ttu-id="c76d9-167">Egenskapen `Command` är bunden till kommandot `SendLocationCommand` i ViewModel-objektet.</span><span class="sxs-lookup"><span data-stu-id="c76d9-167">The `Command` property is bound to the `SendLocationCommand` command on the ViewModel.</span></span> <span data-ttu-id="c76d9-168">När du trycker på knappen körs kommandot.</span><span class="sxs-lookup"><span data-stu-id="c76d9-168">When the button is tapped, the command will be executed.</span></span>

1. <span data-ttu-id="c76d9-169">Lägg till en `Label` under `Button` kontrollen.</span><span class="sxs-lookup"><span data-stu-id="c76d9-169">Add a `Label` below the `Button` control.</span></span> <span data-ttu-id="c76d9-170">Vi visar statusmeddelanden i den här etiketten.</span><span class="sxs-lookup"><span data-stu-id="c76d9-170">We'll display status messages in this label.</span></span>

    ```xml
    <Label Text="{Binding Message}"
           HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    ```

    <span data-ttu-id="c76d9-171">Nedan ser du hela koden för sidan.</span><span class="sxs-lookup"><span data-stu-id="c76d9-171">The full code for this page is below.</span></span>
    
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
            <Label Text="Phone numbers to send to:" HorizontalOptions="Start"/>
            <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
            <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
                    Command="{Binding SendLocationCommand}" />
            <Label Text="{Binding Message}"
                   HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    ```

1. <span data-ttu-id="c76d9-172">Kör appen så att du ser det nya användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c76d9-172">Run the app to see the new UI.</span></span> <span data-ttu-id="c76d9-173">Om du vill kontrollera bindningarna kan du göra det genom att lägga till brytpunkter i egenskaperna eller i metoden `SendLocation`.</span><span class="sxs-lookup"><span data-stu-id="c76d9-173">If you want to validate the bindings at this point, you can do so by adding breakpoints to the properties or the `SendLocation` method.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c76d9-174">När du kompilerar den här appen visas en varning om att `SendLocation` saknar `await` modifierare.</span><span class="sxs-lookup"><span data-stu-id="c76d9-174">When you compile this app, you will see a warning about `SendLocation` lacking `await` modifiers.</span></span> <span data-ttu-id="c76d9-175">Du kan ignorera den här varningen eftersom detta kommer att lösas när mer kod som har lagts till i den här metoden i nästa enhet.</span><span class="sxs-lookup"><span data-stu-id="c76d9-175">You can ignore this warning as this will be resolved once more code is added to this method in the next unit.</span></span>
    
    
    ![Appens nya gränssnitt](../media/3-new-ui.png)

## <a name="summary"></a><span data-ttu-id="c76d9-177">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c76d9-177">Summary</span></span>

<span data-ttu-id="c76d9-178">Nu har du lärt dig hur du skapar ett användargränssnitt för appen med XAML, tillsammans med en ViewModel som hanterar appens logik.</span><span class="sxs-lookup"><span data-stu-id="c76d9-178">In this unit, you learned how to create the UI for the app using XAML, along with a ViewModel to handle the applications logic.</span></span> <span data-ttu-id="c76d9-179">Du har också fått lära dig att binda ViewModel-objektet till gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c76d9-179">You also learned how to bind the ViewModel to the UI.</span></span> <span data-ttu-id="c76d9-180">I nästa del kommer du att lägga till platssökning i din ViewModel.</span><span class="sxs-lookup"><span data-stu-id="c76d9-180">In the next unit, you add location lookup to the ViewModel.</span></span>