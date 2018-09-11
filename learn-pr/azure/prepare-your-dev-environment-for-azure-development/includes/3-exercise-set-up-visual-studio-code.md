I den här kursdelen kommer du installera Visual Studio Code och Azure App Service-tillägget. Det kommer göra dig redo att utveckla för Microsoft Azure och distribuera en webbapp.

## <a name="exercise-steps"></a>Övningssteg

Börja med att identifiera vilket operativsystem du använder. Följ sedan stegen i aktuellt avsnitt nedan för att installera Visual Studio Code.

### <a name="windows"></a>Windows

1. Ladda ned Visual Studio Code-installationsprogrammet för Windows.

1. Kör installationsprogrammet. Det går fort.

1. Öppna VS Code genom att navigera till installationsmappen (standardsökvägen är C:\Program Files\Microsoft VS-kod för en 64-bitars dator).

### <a name="macos"></a>macOS

1. Ladda ned Visual Studio Code för macOS.

1. Dubbelklicka på det nedladdade arkivet för att visa innehållet.

1. Dra Visual Studio Code.app till programmappen för att göra den tillgänglig i Launchpad.

1. Lägg till VS Code till Dock genom att högerklicka på ikonen och välja Options (Alternativ) -> Keep in Dock (Fäst i Dock).

### <a name="linux--debian-and-ubuntu"></a>Linux – Debian och Ubuntu

1. Ladda ned och installera [.deb paketet (64-bitars)](https://go.microsoft.com/fwlink/?LinkID=760868), antingen via det grafiska programvarucentret, eller via kommandoraden (om tillgängligt) (ersätt `<file>` med det .deb-filnamn som du laddade ned):

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

### <a name="linux--rhel-fedora-and-centos"></a>Linux – RHEL, Fedora och CentOS

1. Använd följande skript för att installera nyckeln och databasen:

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    ```

1. Uppdatera paketcachen och installera paketet med dnf (Fedora 22 och senare):

    ```bash
    dnf check-update
    sudo dnf install code
    ```

### <a name="linux--opensuse-and-sle"></a>Linux – openSUSE och SLE

1. Yum-lagringsplatsen fungerar även för openSUSE- och SLE-baserade system. Följande skript installerar nyckeln och databasen:

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
    ```

1. Uppdatera paketcachen och installera paketet med:

    ```bash
    sudo zypper refresh
    sudo zypper install code
    ```

> [!NOTE]
> Mer information om hur du installerar eller uppdaterar VS Code på olika Linux-distributioner finns under avsnittet om hur du [kör VS Code i Linux-dokumentation](https://code.visualstudio.com/docs/setup/linux).

## <a name="install-azure-app-service-extension"></a>Installera tillägget för Azure App Service

Öppna VS Code när det har installerats.

1. Gå till fliken Tillägg.

1. Leta upp Azure App Service.

1. Klicka på Installera.

    Följande skärmbild visar Azure App Service-tillägget som valts i sökresultaten för Visual Studio Code-tillägget.

    ![Skärmbild av VS Code som visar fliken Tillägg med Azure App Service-tillägget markerat i sökresultaten.](../media/3-install-azure-extension.png)

Detta installerar tillägget. Du kan ansluta till din Azure-prenumeration och utveckla och distribuera dina webb-, mobil- eller API-appar till en Azure App Service.
