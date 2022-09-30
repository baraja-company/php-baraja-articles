HTML-форми - частина в браузері
===============================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	uk: html-formi-castina-v-brauzeri
> 
> perex:
> 	- 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> 	- 'Обробка форм на PHP, особливо можливість відправки отриманих даних на сервер за допомогою методів GET і POST.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Перш ніж ми зможемо обробляти будь-які дані користувача на стороні сервера за допомогою PHP, ми повинні їх спочатку отримати. Це робиться в браузері за допомогою HTML-форм, які визначають основні елементи для отримання даних. Метою цієї статті є не викладення всіх можливостей форм, а лише основні можливості прийняття даних та розуміння принципу.

Базове джерело HTML форми
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

Кожна форма починається з HTML-тега `<form>` і закінчується тегом `</form>`. Всі поля форми, розміщені між цими тегами, будуть відправлені.

Далі потрібно задати куди відправляти форму атрибутом `action` (ім'я скрипта), і яким методом відправляти її атрибутом `method` (GET або POST). Якщо не вказати метод і місце призначення, то за замовчуванням форма відправляє сама себе методом GET.

Основні поля форми
-------------------------

Найбільш використовуване поле використовується для отримання тексту (рядка). Кожне поле має свій тип та назву, за якою його можна розпізнати після заповнення.

Загальні текстові поля
------------------

Найголовніше, мені потрібне звичайне текстове поле:

```html
<input type="text" name="food">
```

<input type="text" name="food">

Поле "Пароль
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="password">

Прапорець
--------

Використовується для перевірки булевих значень (`TRUE` та `FALSE`):

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked"> Чи згодні Ви з умовами?
</label> </label>.

Перемикач для вибору декількох варіантів
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

Він дозволяє обирати з декількох варіантів. Обрана опція надсилає своє значення. За замовчуванням добре вибрати одне поле з атрибутом `checked="checked"`:

<label>
	<input type="radio" name="language" value="cz" checked="checked"> Чеська мова
</label><br></label></label
<label>
	<input type="radio" name="language" value="en"> Словацька
</label><br></label></label
<label>
	<input type="radio" name="language" value="en"> Англійська
</label> </label>.

Велике текстове поле
------------------

Створена для введення багаторядкового тексту. Він також використовується для в'їзду:

- `cols` ~ кількість стовпців
- `rows` ~ кількість рядків

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="article" cols="40" rows="6">
Агов, хлопці!
</textarea> </textarea

Selectbox
---------

Представляє зручний спосіб вибору з багатьох даних.

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

Після відправки форми відправляється значення в `value`.

<select name="gender">
	<option value="man">Чоловічий</option>
	<option value="woman">Жінка</option>
</select> </select>.

Кнопка "Надіслати
---------------------

Форма може мати необмежену кількість кнопок відправки. До них легко потрапити:

```html
<input type="submit" value="Odeslat">
```

При натисканні він бере всі дані з полів форми і відправляє їх на заданий скрипт:

<input type="submit" value="Submit">

Обробка даних на сервері
-------------------------

Далі потрібно відправити дані на сервер і там їх обробити, про це йдеться в <a href="/processing-formula-in-php">наступній статті</a>.
