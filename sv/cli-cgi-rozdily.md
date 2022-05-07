Skillnader mellan CLI och CGI
=============================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	sv: skillnader-mellan-cli-och-cgi
> 
> perex:
> 	- Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> 	- De viktigaste skillnaderna mellan CLI och CGI. Information om miljön.
> 
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP kan köras i olika miljöer. Den vanligaste miljön är `CGI`, som körs när PHP behandlar en HTTP-förfrågan. Det är dock också möjligt att köra ett PHP-skript från terminalen, vilket i så fall är en så kallad CLI-uppgift (Command-line interface).

De viktigaste skillnaderna mellan CLI och CGI
-------------------------------------

- Till skillnad från `CGI SAPI` skriver `CLI` som standard inga rubriker till utdata.
- Det finns några `php.ini`-direktiv som åsidosätts i `CLI SAPI` eftersom de är meningslösa i en skalmiljö:
   - `html_errors`: CLI har som standardvärde `FALSE`.
   - `implicit_flush`: standardvärdet för CLI är `TRUE`.
   - `max_execution_time`: standardvärdet för CLI är `0` (obegränsad).
   - `register_argc_argv`: standardvärdet för CLI är `TRUE`.
- Skriptet kan ta emot kommandoradsargument! Variabeln `$argc` anger antalet argument som skickas till programmet. Och fältet `$argv` ger dig en matris med faktiska argument.
- Det finns tre nya konstanter definierade för skalmiljön: `STDIN`, `STDOUT`, `STDERR`. Alla är filhanterare för motsvarande skalenhet. Till exempel är `STDIN` en filhanterare för `fopen('php://stdin', 'r')`. Så du kan läsa en rad från `STDIN` på följande sätt: `$strLine = trim(fgets(STDIN));`. `STDIN` är redan definierad för dig med hjälp av `PHP CLI`.
- PHP CLI ändrar inte den aktuella katalogen till katalogen för det skript som körs. Den aktuella katalogen för skriptet är den katalog där du kör PHP CLI-kommandot.
- Det finns ett antal användbara alternativ för PHP CLI. Med dessa kan du få värdefull information om din php-installation, ditt php-skript eller köra det i olika lägen.
- I PHP 5 har det gjorts vissa ändringar i filnamnen för CLI och CGI. I PHP 5 har CGI-versionen bytt namn till `php-cgi.exe` (tidigare `php.exe`) och CLI-versionen finns nu i huvudkatalogen (tidigare `cli/php.exe`).
- Ett nytt läge har också införts i PHP 5: `php-win.exe`. Detta är likvärdigt med CLI-versionen, förutom att inget skrivs ut i `php-win` och att det därför inte finns någon konsol (ingen "dosbox" visas på skärmen). Detta beteende liknar `PHP GTK`.
