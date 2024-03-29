Файли cookie в PHP
==================

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	uk: fajli-cookie-v-php
> 
> perex:
> 	- 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> 	- 'Файл cookie - це короткий фрагмент текстової інформації в пам''яті браузера, де ми можемо записати і позначити користувача на мові PHP.'
> 
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> Попередження:** Ця стаття була написана багато років тому і деяка інформація може бути застарілою або невірною. Будь ласка, майте це на увазі при читанні.

Файли cookie - це невеликі фрагменти текстової інформації, що зберігаються в браузері відвідувача веб-сайту. Вони завжди передаються з кожною сторінкою, яка завантажується знову, і можуть бути видалені, змінені та прочитані користувачем у будь-який час, тому вони не дуже добре підходять для зберігання особистої інформації.

*Попередження: якщо ваш веб-сайт використовує файли cookie для відстеження користувачів або сторонні доповнення (наприклад, кнопка Facebook like, лічильник трафіку Google analytics, рекламні банери), ви повинні повідомити про це користувача.*

> "Ще одне зауваження: ваш сайт не повинен містити рекламні або вимірювальні коди, поки ви не отримаєте на це згоду. І це відстій".
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">Девід Грудль</a>

Зчитати значення з файлу cookie
--------------------------

Всі файли cookie зберігаються в суперглобальній змінній `$_COOKIE`, яка зберігає кожен ключ у вигляді масиву.

Наприклад, якщо ми зберегли в файлі cookie ім'я користувача, який увійшов до системи, під ключем `user`, ми можемо легко його відновити:

```php
echo $_COOKIE['користувач'];
```

Зверніть увагу: файли cookie можуть існувати не завжди (наприклад, якщо ви новий користувач). Тому ми завжди повинні перевіряти наявність файлів cookie перед будь-яким розміщенням і пропонувати альтернативне повідомлення про помилку, якщо це необхідно.

```php
if (isset($_COOKIE['користувач']) && $_COOKIE['користувач']) {
    echo 'Авторизований користувач:' . $_COOKIE['користувач'];
} else {
    echo 'Ніхто не записався.';
}
```

Отримання всіх доступних файлів cookie
--------------------------------

Оскільки всі файли cookie зберігаються в суперглобальній змінній `$_COOKIE`, їх можна легко перерахувати:

```php
var_dump($_COOKIE);
```

Або ж пройти весь цикл і отримати всі ключі та значення:

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// вивести ключ та значення
    echo '<br>';				// обернути рядок
}
```

Зберігання значення в файлі cookie
--------------------------

Функція `setcookie()` використовується для збереження даних у файлах cookie.

Перший параметр - це ключ cookie, який використовується для його зчитування з поля `$_COOKIE`, а другий параметр - самі дані у вигляді рядка.

За допомогою третього параметра ми можемо (за бажанням) встановити термін дії, протягом якого файл cookie буде доступний. Час доступності задається у вигляді <a href="/date">timestamp</a>, тому якщо ми хочемо встановити cookie з терміном дії 1 годину з цього моменту, нам достатньо написати `time() + 3600`.

```php
$data = 'Деякий контент ми хочемо зберігати.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Зберігання великих даних
-------------------

Файли cookie не підходять для зберігання великих даних (браузери зазвичай дозволяють зберігати лише 4 кБ і максимум 20 файлів cookie, розмір також включає в себе імена файлів cookie, налаштування терміну дії і т.д.).

Краще зберігати на сервері більші дані і просто ставити в куки ідентифікатор, за яким ми зможемо визначити, якому користувачеві він належить. Цей метод називається `$_SESSION` і розглядається в окремій статті.

Якщо вам не обов'язково зберігати дані завжди синхронно на сервері, ви можете використовувати **<a href="https://jecas.cz/localstorage">localstorage</a>** сховище, доступне в javascript. Його ємність складає близько мегабайта, а термін зберігання даних не обмежений.
