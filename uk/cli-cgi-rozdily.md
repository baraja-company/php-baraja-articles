Відмінності між CLI і CGI
=========================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	uk: vidminnosti-miz-cli-i-cgi
> 
> perex:
> 	- Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> 	- Найважливіші відмінності між CLI та CGI. Інформація про навколишнє середовище.
> 
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP може працювати в різних середовищах. Найпоширенішим середовищем є `CGI`, яке запускається, коли PHP обробляє HTTP-запит. Однак, також можна запустити PHP-скрипт з Терміналу, в цьому випадку це так зване завдання CLI (Command-line interface - інтерфейс командного рядка).

Найважливіші відмінності між CLI і CGI
-------------------------------------

- На відміну від `CGI SAPI`, `CLI` за замовчуванням не записує ніяких заголовків у вивід.
- Існують деякі директиви `php.ini`, які перевизначаються в `CLI SAPI`, оскільки вони не мають сенсу в середовищі оболонки:
   - `html_errors`: CLI за замовчуванням має значення `FALSE`.
   - `implicit_flush`: значення за замовчуванням в CLI - `TRUE
   - `max_execution_time`: значення за замовчуванням в CLI - `0` (необмежено)
   - `register_argc_argv`: значення за замовчуванням в CLI - `TRUE
- Скрипт може приймати аргументи командного рядка! Змінна `$argc` містить кількість аргументів, переданих програмі. А поле `$argv` дає масив фактичних аргументів
- Для середовища оболонки визначено 3 нові константи: `STDIN`, `STDOUT`, `STDERR`. Всі вони є обробниками файлів для відповідного пристрою оболонки. Наприклад, `STDIN` є обробником файлу для `fopen('php://stdin', 'r')`. Таким чином, прочитати рядок з `STDIN` можна так: `$strLine = trim(fgets(STDIN));`. STDIN вже визначений для вас за допомогою `PHP CLI`.
- PHP CLI не змінює поточний каталог на каталог виконуваного скрипта. Поточною директорією для скрипта буде директорія, в якій ви запускаєте команду PHP CLI.
- Існує ряд КОРИСНИХ опцій, доступних для PHP CLI. Які дозволяють отримати деяку цінну інформацію про налаштування вашого php, вашого php скрипта або запустити його в різних режимах.
- У PHP 5 відбулися деякі зміни в іменах CLI і CGI файлів. У PHP 5 CGI-версія перейменована в `php-cgi.exe` (раніше `php.exe`), а CLI-версія тепер знаходиться в головному каталозі (раніше `cli/php.exe`).
- У PHP 5 також з'явився новий режим: `php-win.exe`. Це еквівалентно CLI-версії, за винятком того, що в `php-win` нічого не друкується, а отже, відсутня консоль (на екран не виводиться "dos box"). Така поведінка схожа на "PHP GTK".
