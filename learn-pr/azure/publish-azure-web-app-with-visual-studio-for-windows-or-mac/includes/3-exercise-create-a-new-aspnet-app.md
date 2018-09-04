Nu när du har kört igång appen på din lokala dator är det dags att publicera den till Azure.
I den här övningen skapar, bygger och kör du ett nytt ASP.NET-webbprogram på den lokala datorn.

## <a name="create-a-new-project"></a>Skapa ett nytt projekt

### <a name="visual-studio-for-windows"></a>Visual Studio för Windows

Det första steget är att starta Visual Studio och skapa ett lokalt ASP.NET Core-webbprogram.

1. På startsidan för Visual Studio väljer du **Arkiv** och klickar sedan på **Nytt** följt av **Projekt**.

1. I dialogrutan **Nytt projekt** går du till fönsterrutan till vänster och väljer **Webb**.

1. I den mittersta fönsterrutan klickar du på **ASP.NET Core-webbprogram**.

1. Längst ned i dialogrutan går du till **Namn** och anger **Alpine Ski House**

1. Välj en **plats** för den nya lösningen.

1. Klicka på knappen **OK** för att skapa projektet.

1. I dialogrutan **Nytt ASP.NET Core-webbprogram** visas ett urval av startmallar. För den här övningen väljer du **Webbprogram** och klickar sedan på OK för att skapa projektet.

    ![Dialogrutan Nytt projekt](../media-draft/3-aspnet-templates.png)

    > [!NOTE]
    > Du kan även välja olika startmallar i den här dialogrutan beroende på dina behov för utveckling av webbprogram.  Längst upp i dialogrutan kan du även välja version av ASP.NET Core. Du bör välja ASP.NET Core 2.0 eller senare.

1. Du bör nu ha din nya lösning för ASP.NET Core-program.

    ![Dialogrutan Nytt projekt](../media-draft/3-new-solution.png)

### <a name="visual-studio-mac"></a>Visual Studio Mac

1. På startsidan för Visual Studio väljer du **Arkiv** och klickar sedan på **Nytt** följt av **Projekt**.

1. Under. **NET Core** väljer du en **ASP.NET Core-webbapp** och klickar sedan på **Nästa**.

1. I **Projektnamn** skriver du **AlpineSkiHouse**. Detta bör även fylla i lösningsnamnet automatiskt.

1. Välj en **plats** på den lokala datorn för projektet.

1. Klicka på **Skapa**.

## <a name="build-and-test-on-your-local-machine"></a>Bygga och testa på din lokala dator

Nu ska vi bygga och testa det nya projektet på din lokala dator för att se till att det byggs och distribueras lokalt innan du distribuerar till Azure.

1. Tryck på F5 för att köra appen i felsökningsläge eller på Ctrl-F5 för att köra utan att koppla felsökaren.

    ![Dialogrutan Nytt projekt](../media-draft/3-webapp-launch.png)

Visual Studio startar IIS Express och kör appen. När Visual Studio skapar ett webbprojekt används en slumpmässig port för webbservern. I den föregående bilden är portnumret 44381. När du kör appen visas sannolikt ett annat portnummer.

> [!TIP]
> Om du startar appen med Ctrl + F5 (icke-felsökningsläge) kan du göra kodändringar, spara filen, uppdatera webbläsaren och se kodändringarna. Många utvecklare föredrar att använda icke-felsökningsläge för att snabbt starta appen och visa ändringar.

> [!IMPORTANT]
> Du lägger kanske märker till avsnittet längst upp på webbplatsen som är till för din policy om sekretess och användning av cookies. Välj Godkänn för att ge samtycke till spårning. Den här appen spårar inte personlig information. Koden som genereras av mallen innehåller resurser som hjälper till att uppfylla dataskyddsförordningen (GDPR).

## <a name="summary"></a>Sammanfattning

Det första steget för att köra igång ASP.NET-webbplats är att skapa och köra den lokalt. Nu när webbplatsen har skapats är du redo att distribuera den till Azure.
