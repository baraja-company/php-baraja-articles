Вступ до PHP
============

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	uk: vstup-do-php
> 
> perex:
> 	- 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> 	- 'Вступний підручник з мови PHP, можливості мови, інформація про веб-розробку на PHP.'
> 
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

Це онлайн-курс з вивчення мови PHP, розрахований на загальну освіту. З нею можна ознайомитися безкоштовно на цьому сайті. Тексти не повинні використовуватися на будь-яких платних курсах без письмового підтвердження. Ви можете використовувати зразки, використані на сайті, у своїй заявці без подальших обмежень. [Загальні положення та умови] (https://baraja.cz/vseobecne-obchodni-podminky).

Оновлення з HTML
--------------

"Модернізація"? Це більше схоже на інформаційний вкид, і так воно і є.

Ніякої модернізації не відбувається. PHP зберігає всю функціональність і можливості чистого HTML-документа, він просто додає нові можливості написання і розгортання. Чудово, правда?

Ви вже знаєте з розробки статичних HTML-сторінок, що код складається з тегів, які визначаються як ключові слова, укладені в загострені дужки (наприклад, `<b>Hello!</b>` виражає виділений жирним шрифтом текст **Hello!**).

PHP вставляється в HTML-сторінку у вигляді тегів `<?php` і `?>`, всередині яких пишеться інша логіка програми. Важливо, що PHP має власний синтаксис (правила, за якими пишеться код) і, на відміну від HTML, не є толерантним до помилок.

Конкретні приклади того, як це зробити і що означає кожен тег, будуть наведені пізніше. Для початку важливо зрозуміти загальні принципи роботи PHP на сервері і те, як обробляється код.

Потік зв'язку між користувачем і сервером
---------------------------------------

Для звичайної HTML-сторінки це працює приблизно так:

> Користувач надсилає запит (запитує певну HTML-сторінку), сервер дивиться на диск і надсилає назад саме те, що зберігається. Нічого особливого не відбувається, і не варто очікувати чогось більшого. Сторінки будуть просто статичними, без можливості взаємодії з сервером.

Якщо ж додати PHP, то почнуть відбуватися чудеса:

> Користувач знову запитує сторінку. Сервер відкриває файл на диску, але бачить, що він містить не просто чистий HTML, а спеціальні теги, які вказують на PHP-скрипт. Тому він спочатку оцінює їх, а потім відправляє те, що згенерував PHP.

Оцінка PHP-коду виконується за замовчуванням при кожному завантаженні сторінки, в подальшому ви навчитеся кешувати код (зберігати його в скомпільованому вигляді для більш швидкої обробки).

Відмінність обробки PHP-скриптів від обробки на C/C++
------------------------------------------

Можливо, ви навчилися використовувати мову `C` або `C++` в школі. PHP безпосередньо базується на синтаксисі мови `C` і всередині ядра використовується мова `C`, тому корисно знати деякі спільні риси і, навпаки, відмінності.

Основним принципом звичайних скомпільованих програм (які виконуються локально безпосередньо на вашому комп'ютері або смартфоні) є завантаження логіки програми в оперативну пам'ять, яка взаємодіє безпосередньо з операційною системою, яка отримує вхідні дані від користувача і потім виводить на екран вихідні дані програми. Важливим є те, що програма працює ізольовано на всьому шляху від запуску до завершення.

PHP запускається при кожному запиті на відображення сторінки, щоразу перезавантажує весь код і дані, а потім завершує роботу. Тому PHP-скрипти мають буквально яппі-життя і, як правило, існують лише десятки мілісекунд.

Перевагою такого підходу є більш високий рівень ізоляції - якщо щось піде не так, все перезавантажується при наступному завантаженні сторінки. З іншого боку, такий підхід має вищі вимоги до продуктивності, оскільки доводиться неодноразово підключатися до бази даних, зчитувати файли з диска тощо, наприклад.

Надалі ви дізнаєтеся, що зберігати завантажені PHP-скрипти в оперативній пам'яті можна за допомогою розширення `OP Cache`, яке більшість нових серверів (починаючи з версії PHP 7.1 і вище) встановлюють в базовій конфігурації.

Завантаження іноземних PHP-скриптів
--------------------------

Досить поширеним питанням, яке ми обговорюємо зі студентами, є те, як завантажити іноземні PHP-скрипти з сервера і подивитися їх вихідний код. Цьому питанню передує розгляд того, що HTML-код сторінки може бути легко відображений у веб-браузері.

Відповідь - **PHP-скрипти не можуть бути завантажені**. Це пов'язано з тим, що PHP-код спочатку обробляється на веб-сервері, а потім згенерований HTML-код (або інший висновок) надсилається в браузер користувача. Тому завантажити можна лише вивід з PHP-скрипту, а не сам скрипт.

Як працює генерація в HTML?
---------------------------------

Це не працює буквально так, ви будете переглядати HTML-сторінки. PHP-скрипт завжди повинен бути в PHP-файлі.

До цього часу більшість з вас звикли створювати величезні папки з файлами, що закінчуються розширенням **.html**. Тепер це буде значно менше файлів. У крайньому випадку, це може бути один файл.
