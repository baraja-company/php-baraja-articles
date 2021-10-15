Rozdíly mezi CLI a CGI
======================

> id: "79b00e74-b59a-4ae6-8a59-8eecfe2a01ca"
> slug:
> 	cs: cli-cgi-rozdily
> 
> perex: "Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí."
> publicationDate: "2021-10-15 10:00:00"
> mainCategoryId: "4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154"

PHP může běžet v různých prostředích. Nečastější prostředí je `CGI`, které se spustí, když PHP zpracovává HTTP request. PHP script je ale také možné spustit z Terminálu, v takovém případě jde o tzv. CLI úlohu (Command-line interface).

Nejdůležitější rozdíly mezi CLI a CGI
-------------------------------------

- Na rozdíl od `CGI SAPI` nezapisuje `CLI` ve výchozím nastavení na výstup žádné hlavičky.
- Existují některé direktivy `php.ini`, které jsou v `CLI SAPI` přepsány, protože v prostředí shellu nemají smysl:
   - `html_errors`: Výchozí hodnota CLI je `FALSE`
   - `implicit_flush`: výchozí hodnota CLI je `TRUE`
   - `max_execution_time`: výchozí hodnota CLI je `0` (neomezeně)
   - `register_argc_argv`: výchozí hodnota CLI je `TRUE`
- Skript může mít argumenty příkazového řádku! Proměnná `$argc` vám poskytuje počet argumentů předávaných aplikaci. A pole `$argv` vám poskytne pole skutečných argumentů
- Pro prostředí shellu jsou definovány 3 nové konstanty: `STDIN`, `STDOUT`, `STDERR`. Všechny jsou obsluhami souborů pro odpovídající zařízení shellu. Například `STDIN` je obsluha pro `fopen('php://stdin', 'r')`. Řádek ze `STDIN` tedy můžete přečíst takto: `$strLine = trim(fgets(STDIN));`. `STDIN` je již definován za vás pomocí `PHP CLI`.
- PHP CLI nemění aktuální adresář na adresář prováděného skriptu. Aktuálním adresářem pro skript by byl adresář, ve kterém spustíte příkaz PHP CLI.
- Pro PHP CLI je k dispozici řada UŽITEČNÝCH voleb. Které vám umožní získat některé cenné informace o vašem nastavení php, vašem php skriptu nebo ho spustit v různých režimech.
- V PHP 5 došlo k některým změnám v názvech souborů CLI a CGI. V PHP 5 byla verze CGI přejmenována na `php-cgi.exe` (dříve `php.exe`) a verze CLI se nyní nachází v hlavním adresáři (dříve `cli/php.exe`).
- V PHP 5 byl také zaveden nový režim: `php-win.exe`. Ten se rovná verzi CLI, s tím rozdílem, že ve `php-win` se nic nevypisuje, a neposkytuje tedy žádnou konzoli (na obrazovce se nezobrazuje "dos box"). Toto chování je podobné jako u `PHP GTK`.
