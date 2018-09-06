### <a name="exercise-4-create-a-nothotdog-app"></a><span data-ttu-id="ae011-101">Övning 4: Skapa en NotHotDog-app</span><span class="sxs-lookup"><span data-stu-id="ae011-101">Exercise 4: Create a NotHotDog app</span></span>

<span data-ttu-id="ae011-102">I den här övningen ska du använda [Visual Studio Code](https://code.visualstudio.com/) – Microsofts kostnadsfria, plattformsoberoende källkodsredigerare som installeras automatiskt i Data Science VM – för att skriva en NotHotDog-app i Python.</span><span class="sxs-lookup"><span data-stu-id="ae011-102">In this exercise, you will use [Visual Studio Code](https://code.visualstudio.com/), Microsoft's free, cross-platform source-code editor which is preinstalled in the Data Science VM, to write a NotHotDog app in Python.</span></span> <span data-ttu-id="ae011-103">Det grafiska användargränssnittet bygger på [Tkinter](https://wiki.python.org/moin/TkInter), ett populärt GUI-ramverk som gör det möjligt för dig att välja bilder från det lokala filsystemet.</span><span class="sxs-lookup"><span data-stu-id="ae011-103">The app will use [Tkinter](https://wiki.python.org/moin/TkInter), which is a popular GUI framework for Python, to implement its user interface, and it will allow you to select images from your local file system.</span></span> <span data-ttu-id="ae011-104">Bilderna skickas till modellen som du tränade i den föregående övningen och du får sedan besked om huruvida det fins en varmkorv på någon av bilderna eller inte.</span><span class="sxs-lookup"><span data-stu-id="ae011-104">Then it will pass those images to the model you trained in the previous exercise and tell you whether they contain a hot dog.</span></span>

1. <span data-ttu-id="ae011-105">Klicka på **Program** i det övre vänstra hörnet av skrivbordet och välj **Tillbehör > Visual Studio Code** för att starta Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ae011-105">Click **Applications** in the upper-left corner of the desktop and select **Accessories > Visual Studio Code** to start Visual Studio Code.</span></span> <span data-ttu-id="ae011-106">Använd kommandot **File > Open Folder...** (Arkiv > Öppna mapp ...) i Visual Studio Code för att öppna mappen ”notebooks/tensorflow-for-poets-2/tf_files” som innehåller filen **retrained_graph_hotdog.pb** som skapades när du tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="ae011-106">Use Visual Studio Code's **File > Open Folder...** command to open the "notebooks/tensorflow-for-poets-2/tf_files" folder containing the **retrained_graph_hotdog.pb** file created when you trained the model.</span></span>

1. <span data-ttu-id="ae011-107">Skapa en ny fil med namnet **classify.py** i den aktuella mappen.</span><span class="sxs-lookup"><span data-stu-id="ae011-107">Create a new file named **classify.py** in the current folder.</span></span> <span data-ttu-id="ae011-108">Om Visual Studio Code frågar om du vill installera Python-tillägget klickar du på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="ae011-108">If Visual Studio Code offers to install the Python extension, click **Install** to install it.</span></span> <span data-ttu-id="ae011-109">Kopiera koden nedan till Urklipp och använd **Skift+Insert** för att klistra in den i **classify.py**.</span><span class="sxs-lookup"><span data-stu-id="ae011-109">Copy the code below to the clipboard and use **Shift+Ins** to paste it into **classify.py**.</span></span> <span data-ttu-id="ae011-110">Spara sedan filen:</span><span class="sxs-lookup"><span data-stu-id="ae011-110">Then save the file:</span></span>

    ```python
    import tkinter as tk
    from tkinter import messagebox, filedialog, font
    from PIL import ImageTk, Image
    import subprocess

    def select_image_click(img_label):
        try:
            file = filedialog.askopenfilename()

            img = Image.open(file)
            img = img.resize((300, 300))
            selected_img = ImageTk.PhotoImage(img)

            img_label.configure(image=selected_img, width=240)

            output = subprocess.check_output(["python",
                "../scripts/label_image.py",
                "--graph=retrained_graph_hotdog.pb",
                "--image={0}".format(file),
                "--labels=retrained_labels_hotdog.txt"])

            highest = str(output).split("\\n")[3].split(" ")

            if len(highest) == 3:
                score = float(highest[2])
                is_hotdog = True
            else:
                score = float(highest[3])
                is_hotdog = False

            if score > 0.95:
                if is_hotdog:
                    messagebox.showinfo("Result", "That's a hot dog!")
                else:
                    messagebox.showinfo("Result", "That's not a hot dog.")
            else:
                messagebox.showinfo("Result", "Can't tell.")

        except FileNotFoundError as e:
            messagebox.showerror("File not found", "File {0} was not found.".format(e.filename))

    def run():
        window = tk.Tk()

        window.title("Hotdog or Not Hotdog")
        window.geometry('400x600')

        text_font = font.Font(size=18, family="Helvetica Neue")
        welcome_text = tk.Label(window, text="Hot Dog or Not Hot Dog", font=text_font)
        welcome_text.pack()

        instructions_text = tk.Label(window, text="\n\nUse a neural network built with Tensorflow\n"
            "to identify photos containing hot dogs")
        instructions_text.pack(fill=tk.X)

        select_btn = tk.Button(window, text="Select", bg="#0063B1", fg="white", width=5, height=1)
        select_btn.pack(pady=30)

        image_label = tk.Label(window)
        image_label.pack()

        select_btn.configure(command=lambda: select_image_click(image_label))
        window.mainloop()

    if __name__ == "__main__":
        run()
    ```

    <span data-ttu-id="ae011-111">Det viktigaste kodavsnittet här är anropet till ```subprocess.check_output```, som anropar den tränade modellen genom att köra Python-skriptet **label_image.py** från mappen ”scripts” och skickar in den bild som användaren har valt.</span><span class="sxs-lookup"><span data-stu-id="ae011-111">They key code here is the call to ```subprocess.check_output```, which invokes the trained model by executing a Python script named **label_image.py** found in the "scripts" folder, passing in the image that the user selected.</span></span> <span data-ttu-id="ae011-112">Det här skriptet kommer från den lagringsplats som du klonade i den föregående övningen.</span><span class="sxs-lookup"><span data-stu-id="ae011-112">This script came from the repo that you cloned in the previous exercise.</span></span>

1. <span data-ttu-id="ae011-113">Använd en sökmotor för att leta reda på matbilder – några med varmkorv och några utan.</span><span class="sxs-lookup"><span data-stu-id="ae011-113">Use your favorite search engine to find a few food images — some containing hot dogs, and some not.</span></span> <span data-ttu-id="ae011-114">Ladda ned bilderna och lagra dem på valfri plats i filsystemet för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ae011-114">Download these images and store them in the location of your choice in the VM's file system.</span></span>

1. <span data-ttu-id="ae011-115">Använd kommandot **View > Integrated Terminal** (Visa > Integrerad terminal) i Visual Studio Code för att öppna den integrerade terminalen.</span><span class="sxs-lookup"><span data-stu-id="ae011-115">Use Visual Studio Code's **View > Integrated Terminal** command to open an integrated terminal.</span></span> <span data-ttu-id="ae011-116">Kör appen genom att ange följande kommando i den integrerade terminalen:</span><span class="sxs-lookup"><span data-stu-id="ae011-116">Then execute the following command in the integrated terminal to run the app:</span></span>

     ```bash
     python classify.py
     ```

1. <span data-ttu-id="ae011-117">Klicka på knappen **Välj** i appen och välj en bild med en varmkorv (en av bilderna som du laddade ned i steg 3).</span><span class="sxs-lookup"><span data-stu-id="ae011-117">Click the app's **Select** button and pick one of the hot-dog images you downloaded in Step 3.</span></span> <span data-ttu-id="ae011-118">Vänta tills en meddelanderuta visas med besked om huruvida bilden innehåller varmkorv eller inte.</span><span class="sxs-lookup"><span data-stu-id="ae011-118">Wait for a message box to appear, indicating whether the image contains a hot dog.</span></span> <span data-ttu-id="ae011-119">Funkade det?</span><span class="sxs-lookup"><span data-stu-id="ae011-119">Did the model get it correct?</span></span>

    > <span data-ttu-id="ae011-120">Om du får ett felmeddelande om en saknad kerneldrivrutin i terminalfönstret när du bearbetar en bild kan du ignorera detta.</span><span class="sxs-lookup"><span data-stu-id="ae011-120">If you see error messages regarding a missing kernel driver in the terminal window when you process an image, you can safely ignore them.</span></span> <span data-ttu-id="ae011-121">Felmeddelandet visas eftersom Data Science VM saknar en virtuell GPU.</span><span class="sxs-lookup"><span data-stu-id="ae011-121">They result from the fact that the Data Science VM does not contain a virtual GPU.</span></span>

    ![Välja en bild](../images/select-image.png)

    <span data-ttu-id="ae011-123">_Välja en bild_</span><span class="sxs-lookup"><span data-stu-id="ae011-123">_Selecting an image_</span></span>

1. <span data-ttu-id="ae011-124">Upprepa föregående steg med en bild som inte föreställer en varmkorv.</span><span class="sxs-lookup"><span data-stu-id="ae011-124">Repeat the previous step using an image that doesn't contain a hot dog.</span></span> <span data-ttu-id="ae011-125">Gjorde modellen rätt den här gången?</span><span class="sxs-lookup"><span data-stu-id="ae011-125">Was the model right this time?</span></span>

<span data-ttu-id="ae011-126">Fortsätta mata in matbilder i appen tills du märker att den kan känna igen bilder med varmkorvar.</span><span class="sxs-lookup"><span data-stu-id="ae011-126">Continue feeding food images into the app until you're satisfied that it can identify images containing hot dogs.</span></span> <span data-ttu-id="ae011-127">Du kan inte förvänta dig att den alltid ska ha rätt, men den ska *oftast* ha rätt.</span><span class="sxs-lookup"><span data-stu-id="ae011-127">Don't expect it to be right 100% of the time, but do expect it to be right *most* of the time.</span></span>