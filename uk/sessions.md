Сесії - серверні файли cookie в PHP
===================================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	uk: sesii-serverni-fajli-cookie-v-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

Часто нам потрібно зберігати більше інформації в <a href="/cookies">cookies</a>, але максимальний ліміт для файлів cookie - 4 кБ, що не так вже й багато. Сесії вирішують цю проблему, зберігаючи дані на веб-сервері, а в браузері клієнта зберігається лише короткий ідентифікатор, який дозволяє визначити, які дані належать якому клієнту.

Початок сесії
---------------------

Перш ніж робити якусь роботу з сеансами, треба їх спочатку запустити. Це робиться шляхом виклику функції `session_start()` в самому початку скрипта:

```php
session_start();
```

> Суворе попередження: перед викликом функції `session_start()` не повинно виконуватись жодного виводу в HTML-код!

Безпека сесії
-------------------

Вміст сесії зберігається на сервері, а клієнтському браузеру надсилається лише ідентифікатор, тому користувач не має можливості дізнатися, що зберігається в сесії. Єдиний спосіб, яким скрипт може вплинути на користувача - це видалити ідентифікатор (після чого скрипт згенерує новий).

Отримання даних з сесії
----------------------

Всі сесії зберігаються в суперглобальній змінній `$_SESSION` і можуть бути переглянуті як масив.

Наприклад, ім'я користувача, що увійшов до системи, можна отримати за записом:

```php
echo $_SESSION['користувач'];
```

Примітка: Сесія може існувати не завжди (наприклад, якщо ви новий користувач). Тому ми завжди повинні перевіряти наявність помилки перед розміщенням і пропонувати альтернативне повідомлення про помилку, якщо це необхідно.

```php
if (isset($_SESSION['користувач']) && $_SESSION['користувач']) {
    echo 'Авторизований користувач:' . $_SESSION['користувач'];
} else {
    echo 'Ніхто не записався.';
}
```

Збереження даних у сесії
----------------------

Збереження відбувається як просте збереження даних у змінну:

```php
$_SESSION['користувач'] = 'Хонзік';
```

Веб-сервер подбає про технічне забезпечення коректного зберігання на сервері та відправлення ідентифікатора користувачеві.

Видалення сесій
----------------

Окремі значення можуть бути видалені окремо відповідно до ключа:

```php
unset($_SESSION['користувач']);
```

Або ж всі доступні сесії:

```php
unset($_SESSION);
```

> Примітка: Видалення конкретного сеансу не очищає значення ключа, а повністю видаляє ключ. Тому при спробі зчитування неіснуючого ключа буде видано попередження про помилку. Ми завжди можемо легко перевірити наявність ключа за допомогою функції `isset()`.

Максимальний термін дії сесії
---------------------------------

Кожна збережена сесія має ліміт часу, протягом якого вона буде зберігатися на сервері. PHP безпосередньо містить скрипт cron, який періодично видаляє старі сесії.

За замовчуванням зазвичай використовується "1440 секунд", що становить "24 хвилини".

Підвищення вартості потрібно зробити в 2 місцях:

- В <a href="/info">`php.ini` задається максимальна тривалість дії, яку буде підтримувати сервер</a>. Значення задається директивою session.gc_maxlifetime,
- На стороні PHP-скрипта потрібно вказати поточну запитувану валідність.

Використання в PHP:

```php
// тепер сервер буде тримати сесію до 3600 секунд = 1 година
ini_set('session.gc_maxlifetime', '3600');

// усі клієнти (браузери) будуть
// сесія відправлена з терміном дії рівно 3600 секунд
session_set_cookie_params(3600);

session_start(); // ми можемо розпочати сесію!
```
