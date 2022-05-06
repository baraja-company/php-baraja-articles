Differences between CLI and CGI
===============================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	en: differences-between-cli-and-cgi
> 
> perex: Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP can run in different environments. The most common environment is `CGI`, which runs when PHP processes an HTTP request. However, it is also possible to run a PHP script from the Terminal, in which case it is a so-called CLI (Command-line interface) task.

The most important differences between CLI and CGI
-------------------------------------

- Unlike `CGI SAPI`, `CLI` does not write any headers to the output by default.
- There are some `php.ini` directives that are overridden in `CLI SAPI` because they are meaningless in a shell environment:
   - `html_errors`: CLI defaults to `FALSE`.
   - `implicit_flush`: default CLI value is `TRUE`
   - `max_execution_time`: default CLI value is `0` (unlimited)
   - `register_argc_argv`: default CLI value is `TRUE`
- The script can take command line arguments! The `$argc` variable gives you the number of arguments passed to the application. And the `$argv` field gives you an array of actual arguments
- There are 3 new constants defined for the shell environment: `STDIN`, `STDOUT`, `STDERR`. All are file handlers for the corresponding shell device. For example, `STDIN` is a file handler for `fopen('php://stdin', 'r')`. So you can read a line from `STDIN` like this: `$strLine = trim(fgets(STDIN));`. The `STDIN` is already defined for you using the `PHP CLI`.
- The PHP CLI does not change the current directory to the directory of the script being executed. The current directory for the script would be the directory in which you run the PHP CLI command.
- There are a number of USEFUL options available for the PHP CLI. Which allow you to get some valuable information about your php setup, your php script or run it in different modes.
- In PHP 5 there have been some changes to the CLI and CGI file names. In PHP 5, the CGI version has been renamed to `php-cgi.exe` (formerly `php.exe`) and the CLI version is now located in the main directory (formerly `cli/php.exe`).
- A new mode has also been introduced in PHP 5: `php-win.exe`. This is equivalent to the CLI version, except that in `php-win` nothing is printed, and thus provides no console (no "dos box" is displayed on the screen). This behaviour is similar to `PHP GTK`.
