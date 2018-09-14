I den här korta övningen lär vi dig att migrera en befintlig, ohanterade VHD till en hanterad virtuell Hårddisk. 

## <a name="sign-in-to-azure"></a>Logga in på Azure

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

## <a name="migrate-our-disks-to-managed-disks"></a>Migrera våra diskar till managed disks

1. Azure-portalen i navigeringsfönstret till vänster, Välj **virtuella datorer**.

1. I listan över virtuella datorer, väljer du våra VM **MailSenderVM**.

1. I den **MailSenderVM** fönstret under **inställningar**väljer **diskar**.

1. I den **MailSenderVM - diskar** väljer **migrera till managed disks**.

1. I den **migrera till managed disks** väljer **migrera**. Azure stoppar den virtuella datorn, migrerar diskar och startar sedan om den virtuella datorn. Den här processen kan ta flera minuter.

Vi har migrerat våra diskar till hanterade diskar i den här övningen. Genom att använda hanterade diskar kan du inte konfigurera lagringskonton för diskarna eftersom Azure hanterar dem åt dig.
