Forskelle mellem CLI og CGI
===========================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	da: forskelle-mellem-cli-og-cgi
> 
> perex:
> 	- Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> 	- De vigtigste forskelle mellem CLI og CGI. Oplysninger om miljøet.
> 
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP kan køre i forskellige miljøer. Det mest almindelige miljø er `CGI`, som kører, når PHP behandler en HTTP-forespørgsel. Det er dog også muligt at køre et PHP-script fra terminalen, og i så fald er det en såkaldt CLI-opgave (Command-line interface).

De vigtigste forskelle mellem CLI og CGI
-------------------------------------

- I modsætning til `CGI SAPI` skriver `CLI` som standard ikke nogen headere til output.
- Der er nogle `php.ini`-direktiver, som er tilsidesat i `CLI SAPI`, fordi de er meningsløse i et shell-miljø:
   - `html_errors`: CLI har som standardindstilling `FALSE`.
   - `implicit_flush`: standard CLI-værdien er `TRUE`.
   - `max_execution_time`: standard CLI-værdien er `0` (ubegrænset)
   - `register_argc_argv`: standard CLI-værdien er `TRUE`.
- Scriptet kan tage imod kommandolinjeargumenter! Variablen `$argc` angiver antallet af argumenter, der er sendt til programmet. Og feltet `$argv` giver dig et array af de faktiske argumenter
- Der er 3 nye konstanter defineret for shell-miljøet: `STDIN`, `STDOUT`, `STDERR`. Alle er filhåndteringsenheder for den tilsvarende shell-enhed. F.eks. er `STDIN` en filhåndteringsprogram for `fopen('php://stdin', 'r')`. Så du kan læse en linje fra `STDIN` på følgende måde: `$strLine = trim(fgets(STDIN));`. `STDIN` er allerede defineret for dig ved hjælp af `PHP CLI`.
- PHP CLI ændrer ikke den aktuelle mappe til mappen for det script, der udføres. Den aktuelle mappe for scriptet vil være den mappe, hvor du kører PHP CLI-kommandoen.
- Der er en række nyttige indstillinger til rådighed for PHP CLI. Hvilket giver dig mulighed for at få værdifulde oplysninger om din php-opsætning, dit php-script eller køre det i forskellige tilstande.
- I PHP 5 er der sket nogle ændringer i CLI- og CGI-filnavne. I PHP 5 er CGI-versionen blevet omdøbt til `php-cgi.exe` (tidligere `php.exe`), og CLI-versionen ligger nu i hovedmappen (tidligere `cli/php.exe`).
- Der er også blevet indført en ny tilstand i PHP 5: `php-win.exe`. Dette svarer til CLI-versionen, bortset fra at der i `php-win` ikke udskrives noget, og der er således ingen konsol (der vises ingen "dos box" på skærmen). Denne opførsel svarer til `PHP GTK`.
