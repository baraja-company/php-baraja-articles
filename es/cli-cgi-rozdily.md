Diferencias entre CLI y CGI
===========================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	es: diferencias-entre-cli-y-cgi
> 
> perex:
> 	- Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> 	- Las diferencias más importantes entre CLI y CGI. Información sobre el medio ambiente.
> 
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP puede funcionar en diferentes entornos. El entorno más común es `CGI`, que se ejecuta cuando PHP procesa una petición HTTP. Sin embargo, también es posible ejecutar un script PHP desde la Terminal, en cuyo caso se trata de una tarea denominada CLI (Command-line interface).

Las diferencias más importantes entre CLI y CGI
-------------------------------------

- A diferencia de `CGI SAPI`, `CLI` no escribe ninguna cabecera en la salida por defecto.
- Hay algunas directivas de `php.ini` que se anulan en `CLI SAPI` porque no tienen sentido en un entorno de shell:
   - `html_errors`: CLI por defecto a `FALSE`.
   - `implicit_flush`: el valor por defecto de la CLI es `TRUE`.
   - `max_execution_time`: el valor por defecto de la CLI es `0` (ilimitado)
   - `register_argc_argv`: el valor por defecto de la CLI es `TRUE`.
- El script puede aceptar argumentos de la línea de comandos. La variable `$argc` le da el número de argumentos pasados a la aplicación. Y el campo `$argv` te da una matriz de argumentos reales
- Hay 3 nuevas constantes definidas para el entorno del shell: `STDIN`, `STDOUT`, `STDERR`. Todos son manejadores de archivos para el dispositivo shell correspondiente. Por ejemplo, `STDIN` es un manejador de archivos para `fopen('php://stdin', 'r')`. Así, puedes leer una línea de `STDIN` de la siguiente manera: `$strLine = trim(fgets(STDIN));`. El `STDIN` ya está definido para usted usando el `PHP CLI`.
- El PHP CLI no cambia el directorio actual al directorio del script que se está ejecutando. El directorio actual para el script sería el directorio en el que se ejecuta el comando PHP CLI.
- Hay un número de opciones ÚTILES disponibles para el CLI de PHP. Lo que le permite obtener alguna información valiosa sobre su configuración de php, su script de php o ejecutarlo en diferentes modos.
- En PHP 5 ha habido algunos cambios en los nombres de los archivos CLI y CGI. En PHP 5, la versión CGI ha sido renombrada a `php-cgi.exe` (antes `php.exe`) y la versión CLI se encuentra ahora en el directorio principal (antes `cli/php.exe`).
- También se ha introducido un nuevo modo en PHP 5: `php-win.exe`. Esto es equivalente a la versión CLI, excepto que en `php-win` no se imprime nada, y por lo tanto no proporciona ninguna consola (no se muestra ninguna "caja de dos" en la pantalla). Este comportamiento es similar al de `PHP GTK`.
