Här lägger du till en Borttagningsprincip till Azure Redis Cache.

## <a name="set-an-eviction-policy"></a>Ange en Borttagningsprincip

Ange en Borttagningsprincip i Azure och använda vi helt enkelt en nedrullningsbar meny i Azure-portalen.

1. Öppna din Azure Redis Cache i Azure-portalen.

1. Välj den **avancerade inställningar** bladet.

1. Använd den **princip för max. minne** nedrullningsbara menyn och välj **allkeys slumpmässiga**.

1. Klicka på **Spara**. 

Nu om du kör slut på minne, Välj en slumpmässig nyckel tas bort för att göra plats för din nya data i Azure Redis Cache.