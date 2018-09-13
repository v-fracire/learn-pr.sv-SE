### <a name="create-a-nothotdog-app"></a>Skapa en NotHotDog-app

I den här enheten kommer du att använda [Visual Studio Code](https://code.visualstudio.com/) – Microsofts kostnadsfria, plattformsoberoende källkodsredigerare som installeras automatiskt i Data Science VM – för att skriva en NotHotDog-app i Python. Det grafiska användargränssnittet bygger på [Tkinter](https://wiki.python.org/moin/TkInter), ett populärt GUI-ramverk för Python som gör det möjligt för dig att välja bilder från det lokala filsystemet. Bilderna skickas till modellen som du tränade i den föregående övningen och du får sedan besked om huruvida det fins en varmkorv på någon av bilderna eller inte.

1. Klicka på **Program** i det övre vänstra hörnet av skrivbordet och välj **Tillbehör > Visual Studio Code** för att starta Visual Studio Code. Använd kommandot **File > Open Folder...** (Arkiv > Öppna mapp ...) i Visual Studio Code för att öppna mappen ”notebooks/tensorflow-for-poets-2/tf_files” som innehåller filen **retrained_graph_hotdog.pb** som skapades när du tränade modellen.

1. Skapa en ny fil med namnet **classify.py** i den aktuella mappen. Om Visual Studio Code frågar om du vill installera Python-tillägget klickar du på **Installera**. Kopiera koden nedan till Urklipp och använd **Skift+Insert** för att klistra in den i **classify.py**. Spara sedan filen:

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

    Det viktigaste kodavsnittet här är anropet till ```subprocess.check_output```, som anropar den tränade modellen genom att köra Python-skriptet **label_image.py** från mappen ”scripts” och skickar in den bild som användaren har valt. Det här skriptet kommer från den lagringsplats som du klonade i den föregående övningen.

1. Använd en sökmotor för att leta reda på matbilder – några med varmkorv och några utan. Ladda ned bilderna och lagra dem på valfri plats i filsystemet för den virtuella datorn.

1. Använd kommandot **View > Integrated Terminal** (Visa > Integrerad terminal) i Visual Studio Code för att öppna den integrerade terminalen. Kör appen genom att ange följande kommando i den integrerade terminalen:

     ```bash
     python classify.py
     ```

1. Klicka på knappen **Välj** i appen och välj en bild med en varmkorv (en av bilderna som du laddade ned i steg 3). Vänta tills en meddelanderuta visas med besked om huruvida bilden innehåller varmkorv eller inte. Funkade det?

    > Om du får ett felmeddelande om en saknad kerneldrivrutin i terminalfönstret när du bearbetar en bild kan du ignorera detta. Felmeddelandet visas eftersom Data Science VM saknar en virtuell GPU.

    ![Välja en bild](../media-draft/4-select-image.png)

1. Upprepa föregående steg med en bild som inte föreställer en varmkorv. Gjorde modellen rätt den här gången?

Fortsätta mata in matbilder i appen tills du märker att den kan känna igen bilder med varmkorvar. Du kan inte förvänta dig att den alltid ska ha rätt, men den ska *oftast* ha rätt.