Du skapat har en mobilapp för flera plattformar med Xamarin och en Azure-funktion med en Twilio-bindning.

## <a name="clean-up-resources"></a>Rensa resurser

När du har arbetat klart med programmet Azure Functions kan du ta bort alla resurser som skapades under självstudien från Azure-portalen.

1. Från Visual Studio väljer du *Visa->Cloud Explorer*.

1. I listrutan överst i den här panelen väljer du *Resursgrupper*.

1. Utöka prenumerationen som du använde när du skapade resursgruppen. Högerklicka på resursgruppen ”ImHere” och välj *Open in Portal* (Öppna i portal).

    ![Öppna resursgruppen i portalen från Cloud Explorer-fönstret](../media-drafts/9-open-resource-group-in-portal.png)

1. Logga in på Azure-portalen i webbläsaren om det behövs.

1. Portalen öppnas i resursgruppen ”ImHere”. Klicka på knappen **Ta bort resursgrupp**.

    ![Ta bort resursgruppen](../media-drafts/9-delete-resource-group.png)

1. Ange namnet på resursgruppen och klicka på **Ta bort** för att bekräfta.

    ![Ange resursgruppens namn för att bekräfta borttagningen](../media-drafts/9-confirm-delete-resource-group.png)

## <a name="summary"></a>Sammanfattning

I den här modulen har du lärt dig att:
- Skapa en Xamarin.Forms-app för flera plattformar som använder Xamarin.Essentials.
- Skapa ett plattformsoberoende gränssnitt med XAML med programlogik i en ViewModel och bind egenskaper i en ViewModel till användargränssnittet.
- Identifiera användarens plats.
- Skapa en Azure-funktion med en HTTP-utlösare och kör den lokalt.
- Anropa en Azure-funktion från en mobilapp och skicka data som JSON.
- Bind en Azure-funktion till Twilio för att skicka ett SMS.
- Publicera en Azure-funktion till Azure.