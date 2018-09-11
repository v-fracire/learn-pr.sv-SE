Mobilappen körs och den första versionen av Azure-funktionen har skapats. I den här enheten anropar du Azure-funktionen från mobilappen, och skickar som indata användarens plats och en lista med telefonnummer som ska få SMS.

## <a name="calling-the-azure-function-from-the-mobile-app"></a>Anropa Azure-funktionen från mobilappen

1. Öppna `MainViewModel`.

2. Lägg till ett privat `HttpClient`-fält med namnet `client` i klassen. Du måste lägga till en referens till namnområdet `System.Net.Http`.

    ```cs
    HttpClient client = new HttpClient();
    ```

3. Lägg till ett konstant fält för funktionens basadress. Använd adressen som den lokala körningsmiljön för Azure Functions lyssnar vid. När funktionen distribueras till Azure kan du ändra den här konstanten till webbadressen för Azure.

    ```cs
    const string baseUrl = "http://localhost:7071";
    ```

4. När du har hittat platsen ska du i metoden `SendLocation` skapa en ny instans av `PostData` med hjälp av platsen och en lista med telefonnummer som användaren anger. Du måste lägga till ett användningsdirektiv för namnområdet `ImHere.Data`.

    ```cs
    PostData postData = new PostData
    {
        Latitude = location.Latitude,
        Longitude = location.Longitude,
        ToNumbers = PhoneNumbers.Split('\n')
    };
    ```

    > Det här förutsätter att telefonnumren har angetts med rätt format, ett per rad i `Editor`-kontrollen. I en produktionsklar app skulle det kontrolleras att minst ett telefonnummer har angetts med rätt format.

5. Om du vill serialisera `PostData` som JSON är det enklast att använda NuGet-paketet Newtonsoft.JSON. Lägg till det här NuGet-paketet i projektet `ImHere` på samma sätt som du lade till Xamarin.Essentials i en tidigare utbildningsenhet.

6. Serialisera `PostData` till en `string` med hjälp av den statiska klassen `JsonConvert`. Du måste lägga till ett användningsdirektiv för namnområdet `Newtonsoft.Json`. Koda om den här strängen till en `StringContent`-klass så att den kan skickas till Azure-funktionen som JSON.

    ```cs
    string data = JsonConvert.SerializeObject(postData);
    StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
    ```

7. Publicera dessa data till funktionen och hämta resultatet.

   ```cs
    HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                        content);
   ```

   Du kommer åt Azure-funktioner med `/api/<function name>`, så om den port som väljs i den lokala körningsmiljön för Functions är 7071 så kan funktionen `SendLocation` nås via `http://localhost:7071/api/SendLocation`.

8. Visa ett meddelande i gränssnittet beroende på resultatet.

    ```cs
    if (result.IsSuccessStatusCode)
        Message = "Location sent successfully";
    else
        Message = $"Error - {result.ReasonPhrase}";
    ```

Nedan visas den fullständiga koden för de nya fälten och metoden `SendLocation`.

```cs
HttpClient client = new HttpClient();
const string baseUrl = "http://localhost:7071";

async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";

        PostData postData = new PostData
        {
            Latitude = location.Latitude,
            Longitude = location.Longitude,
            ToNumbers = PhoneNumbers.Split('\n')
        };

        string data = JsonConvert.SerializeObject(postData);
        StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
        HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                            content);

        if (result.IsSuccessStatusCode)
            Message = "Location sent successfully";
        else
            Message = $"Error - {result.ReasonPhrase}";
    }
}
```

## <a name="testing-it-out"></a>Testa den

1. Se till att Azure-funktionen fortfarande körs lokalt och att porten matchar metoden `SendLocation`.

2. Ange appen UWP som startapp och kör den. Klicka på knappen **Send Location**. Du ser utdata från funktionen som anropas i konsolfönstret för Functions och loggar med den genererade webbadressen.

    ![Utdata från funktionen som anropas](../media-drafts/6-function-called.png)

3. Du kan testa adressgenereringen genom att klistra in adressen från konsolen i en webbläsare. Du bör se din aktuella plats.

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du fått lära dig att anropa en Azure-funktion från mobilappen. I anropet skickades användarens plats och angivna telefonnummer som JSON. I nästa utbildningsenhet kommer du att binda Azure-funktionen till Twilio och skicka platsen som ett SMS.