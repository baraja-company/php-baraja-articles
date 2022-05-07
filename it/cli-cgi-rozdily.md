Differenze tra CLI e CGI
========================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	it: differenze-tra-cli-e-cgi
> 
> perex:
> 	- Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> 	- Le differenze più importanti tra CLI e CGI. Informazioni sull'ambiente.
> 
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP può funzionare in diversi ambienti. L'ambiente più comune è `CGI`, che viene eseguito quando PHP elabora una richiesta HTTP. Tuttavia, è anche possibile eseguire uno script PHP dal terminale, nel qual caso è un cosiddetto compito CLI (Command-line interface).

Le differenze più importanti tra CLI e CGI
-------------------------------------

- A differenza di `CGI SAPI`, `CLI` non scrive alcuna intestazione sull'output per default.
- Ci sono alcune direttive `php.ini` che sono sovrascritte in `CLI SAPI` perché non hanno senso in un ambiente shell:
   - `html_errors`: il valore predefinito della CLI è `FALSE`.
   - `implicit_flush`: il valore predefinito della CLI è `TRUE`.
   - `max_execution_time`: il valore predefinito della CLI è `0` (illimitato)
   - `register_argc_argv`: il valore predefinito della CLI è `TRUE`.
- Lo script può accettare argomenti dalla linea di comando! La variabile `$argc` dà il numero di argomenti passati all'applicazione. E il campo `$argv` ti dà un array di argomenti reali
- Ci sono 3 nuove costanti definite per l'ambiente della shell: `STDIN`, `STDOUT`, `STDERR`. Tutti sono gestori di file per il dispositivo shell corrispondente. Per esempio, `STDIN` è un gestore di file per `fopen('php://stdin', 'r')`. Quindi puoi leggere una linea da `STDIN` in questo modo: `$strLine = trim(fgets(STDIN));`. Il `STDIN` è già definito per te usando la `PHP CLI`.
- La PHP CLI non cambia la directory corrente nella directory dello script in esecuzione. La directory corrente per lo script sarebbe la directory in cui si esegue il comando PHP CLI.
- Ci sono un certo numero di opzioni UTILI disponibili per la CLI di PHP. Che ti permettono di ottenere alcune informazioni preziose sul tuo setup php, il tuo script php o di eseguirlo in diverse modalità.
- In PHP 5 ci sono stati alcuni cambiamenti nei nomi dei file CLI e CGI. In PHP 5, la versione CGI è stata rinominata in `php-cgi.exe` (precedentemente `php.exe`) e la versione CLI si trova ora nella directory principale (precedentemente `cli/php.exe`).
- Un nuovo modo è stato introdotto in PHP 5: `php-win.exe`. Questo è equivalente alla versione CLI, eccetto che in `php-win` non viene stampato nulla, e quindi non fornisce alcuna console (non viene visualizzato alcun "dos box" sullo schermo). Questo comportamento è simile a quello di `PHP GTK`.
