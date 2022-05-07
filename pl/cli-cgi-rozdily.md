Różnice między CLI a CGI
========================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	pl: roznice-miedzy-cli-a-cgi
> 
> perex:
> 	- Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> 	- Najważniejsze różnice między CLI a CGI. Informacje o środowisku.
> 
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP może działać w różnych środowiskach. Najbardziej powszechnym środowiskiem jest `CGI`, które jest uruchamiane, gdy PHP przetwarza żądanie HTTP. Możliwe jest jednak również uruchomienie skryptu PHP z poziomu Terminala, w tym przypadku jest to tzw. zadanie CLI (Command-line interface).

Najważniejsze różnice między CLI a CGI
-------------------------------------

- W przeciwieństwie do `CGI SAPI`, `CLI` domyślnie nie zapisuje żadnych nagłówków na wyjściu.
- Istnieją pewne dyrektywy `php.ini`, które są nadpisane w `CLI SAPI`, ponieważ są bez znaczenia w środowisku powłoki:
   - `html_errors`: Domyślnie CLI ustawia wartość `FALSE`.
   - `implicit_flush`: domyślną wartością CLI jest `TRUE`.
   - `max_execution_time`: domyślną wartością CLI jest `0` (bez ograniczeń)
   - `register_argc_argv`: domyślną wartością CLI jest `TRUE`.
- Skrypt może przyjmować argumenty z wiersza poleceń! Zmienna `$argc` podaje liczbę argumentów przekazanych do aplikacji. Pole `$argv` zawiera tablicę rzeczywistych argumentów
- Zdefiniowano 3 nowe stałe dla środowiska powłoki: `STDIN`, `STDOUT`, `STDERR`. Wszystkie są urządzeniami obsługującymi pliki dla odpowiedniego urządzenia powłoki. Na przykład, `STDIN` jest obsługą pliku dla `fopen('php://stdin', 'r')`. Możesz więc odczytać linię z `STDIN` w następujący sposób: `$strLine = trim(fgets(STDIN));`. Numer `STDIN` jest już zdefiniowany za pomocą `PHP CLI`.
- PHP CLI nie zmienia katalogu bieżącego na katalog wykonywanego skryptu. Bieżącym katalogiem dla skryptu będzie katalog, w którym uruchomiono polecenie PHP CLI.
- W PHP CLI dostępnych jest wiele użytecznych opcji. Dzięki temu można uzyskać cenne informacje o konfiguracji php, skrypcie php lub uruchomić go w różnych trybach.
- W PHP 5 wprowadzono pewne zmiany w nazewnictwie plików CLI i CGI. W PHP 5 wersja CGI została przemianowana na `php-cgi.exe` (dawniej `php.exe`), a wersja CLI znajduje się teraz w katalogu głównym (dawniej `cli/php.exe`).
- W PHP 5 wprowadzono także nowy tryb: `php-win.exe`. Jest to odpowiednik wersji CLI, z tą różnicą, że w `php-win` nic nie jest drukowane, a więc nie ma konsoli (na ekranie nie jest wyświetlane "okienko dosowe"). To zachowanie jest podobne do zachowania `PHP GTK`.
