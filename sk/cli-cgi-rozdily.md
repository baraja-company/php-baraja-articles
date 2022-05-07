Rozdiely medzi CLI a CGI
========================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	sk: rozdiely-medzi-cli-a-cgi
> 
> perex: Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP môže bežať v rôznych prostrediach. Najbežnejším prostredím je `CGI`, ktoré sa spustí, keď PHP spracuje požiadavku HTTP. Skript PHP je však možné spustiť aj z terminálu, v tomto prípade ide o takzvanú úlohu CLI (Command-line interface).

Najdôležitejšie rozdiely medzi CLI a CGI
-------------------------------------

- Na rozdiel od `CGI SAPI`, `CLI` štandardne nezapisuje na výstup žiadne hlavičky.
- Niektoré direktívy `php.ini` sú v `CLI SAPI` prepísané, pretože v prostredí shellu nemajú význam:
   - `html_errors`: CLI má predvolenú hodnotu `FALSE`.
   - `implicit_flush`: predvolená hodnota CLI je `TRUE`
   - `max_execution_time`: predvolená hodnota CLI je `0` (neobmedzené)
   - `register_argc_argv`: predvolená hodnota CLI je `TRUE`
- Skript môže prijímať argumenty príkazového riadku! Premenná `$argc` udáva počet argumentov odovzdaných aplikácii. A pole `$argv` poskytuje pole skutočných argumentov
- Pre prostredie shellu sú definované 3 nové konštanty: `STDIN`, `STDOUT`, `STDERR`. Všetky sú spracovateľmi súborov pre príslušné zariadenie shell. Napríklad `STDIN` je obsluha súboru pre `fopen('php://stdin', 'r')`. Riadok z `STDIN` môžete prečítať takto: `$strLine = trim(fgets(STDIN));`. `STDIN` je už pre vás definovaný pomocou `PHP CLI`.
- PHP CLI nemení aktuálny adresár na adresár vykonávaného skriptu. Aktuálny adresár pre skript bude adresár, v ktorom spustíte príkaz PHP CLI.
- Pre CLI PHP je k dispozícii množstvo UŽITOČNÝCH možností. Ktoré vám umožnia získať cenné informácie o nastaveniach php, vašom php skripte alebo ho spustiť v rôznych režimoch.
- V PHP 5 došlo k určitým zmenám v názvoch súborov CLI a CGI. V PHP 5 bola verzia CGI premenovaná na `php-cgi.exe` (predtým `php.exe`) a verzia CLI sa teraz nachádza v hlavnom adresári (predtým `cli/php.exe`).
- V PHP 5 bol zavedený aj nový režim: `php-win.exe`. Táto verzia je ekvivalentná verzii CLI, až na to, že v `php-win` sa nič nevypisuje, a teda neposkytuje žiadnu konzolu (na obrazovke sa nezobrazuje žiadny "dos box"). Toto správanie je podobné ako pri `PHP GTK`.
