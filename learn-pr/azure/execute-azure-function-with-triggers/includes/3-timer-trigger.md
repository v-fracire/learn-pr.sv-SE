Det är vanligt att köra en del av logiken med ett angivet intervall. Anta att du har en blogg och du lägger märke till att dina prenumeranter inte har läst dina senaste inlägg. Du bestämmer dig för att den bästa åtgärden är att skicka ett e-postmeddelande till dem en gång i veckan för att påminna dem om att gå in på din blogg. Du implementerar den här logiken med hjälp av en Azure-funktion med en _timerutlösare_ som anropar funktionen en gång i veckan.

## <a name="what-is-a-timer-trigger"></a>Vad är en timerutlösare?

En timerutlösare är en utlösare som kör en funktion med ett konsekvent intervall. Om du vill skapa en timerutlösare måste du ange två typer av information. 

1. Ett *tidsstämpelparameternamn*, vilket helt enkelt är en identifierare som får åtkomst till utlösaren i koden. 
2. Ett *schema*, vilket är ett *CRON-uttryck* som anger intervallet för timern.

## <a name="what-is-a-cron-expression"></a>Vad är ett CRON-uttryck?

Ett *CRON-uttryck* är en sträng som består av sex fält som motsvarar olika tider.

Ordningen på de sex fälten i Azure är: `{second} {minute} {hour} {day} {month} {day of the week}`.

Ett *CRON-uttryck* som skapar en utlösare som körs var femte minut ut ser exempelvis ut så här:

```
0 */5 * * * *
```

Den här strängen kan se förvirrande ut först. Vi kommer tillbaka till de här begreppen när vi tar en närmare titt på *CRON-uttryck*.

För att kunna skapa ett *CRON-uttryck* måste du ha en grundläggande förståelse för vissa specialtecken.

| Specialtecken | Betydelse | Exempel |
| ------------- | ------------- | ------------- |
| *      | Väljer varje värde i ett fält | En asterisk ”*” i fältet för veckodag innebär *varje* dag. |
| ,      | Avgränsar objekt i en lista | Ett kommatecken ”1,3” i fältet för veckodag innebär enbart måndagar (dag 1) och onsdagar (dag 3). |
| -      | Anger ett intervall | Ett bindestreck ”10-12” i fältet för timme innebär ett intervall för timmarna 10, 11 och 12. |
| /      | Anger en ökning | Ett snedstreck ”*/10” i fältet för minuter innebär en ökning var tionde minut. |

Vi ska gå nu tillbaka till det ursprungliga exemplet på CRON-uttryck. Vi kan förstå det bättre om vi bryter ned ett fält i taget.

```
0 */5 * * * *
```

Det **första fältet** motsvarar sekunder. Det här fältet stöder värdena 0–59. Eftersom fältet innehåller en nolla, väljs det första möjliga värdet som är en sekund.

Det **andra fältet** motsvarar minuter. Värdet ”*/5” innehåller två specialtecken. Till att börja med innebär asterisken (\*) ”välj alla värden i fältet”. Eftersom det här fältet motsvarar minuter är 0–59 möjliga värden. Det andra specialtecknet är ett snedstreck (/) som motsvarar en ökning. När du kombinerar dessa tecken innebär det att var femte värde ska väljas för alla värden 0–59. Ett enklare sätt att säga det är helt enkelt ”var femte minut”.

De **återstående fyra fälten** motsvarar timme, dag, månad och veckodag. En asterisk i dessa fält innebär att alla möjliga värden ska väljas. I det här exemplet väljer vi ”varje timme varje dag i varje månad”.

När du sätter ihop alla fält läses uttrycket som ”den första sekunden, var femte minut i varje timme, varje dag, varje månad”.

## <a name="how-to-create-a-timer-trigger"></a>Skapa en timerutlösare

Du kan skapa en timerutlösare helt i Azure-portalen. I din Azure-funktion väljer du **timerutlösare** i listan med fördefinierade utlösartyper. Ange den logik som du vill köra. Ange ett **tidsstämpelparameternamn** och **CRON-uttrycket**.

## <a name="summary"></a>Sammanfattning

En timerutlösare anropar en Azure-funktion enligt ett konsekvent schema. För att definiera schemat för en timerutlösare skapade vi ett *CRON-uttryck*, vilket är en sträng som motsvarar olika tider.
