Addcslashes
===========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	uk: addcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Підтримка PHP4, PHP5

`addcslashes` - косий рядок в стилі C

Опис
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Повертає рядок зі зворотними косими рисками перед символами, які вказані в параметрі charlist.

Параметри
--------------------------

**str** Текстовий рядок

**список**

символи, які потрібно видалити. Якщо charlist містить символи `\n`, `\r` та інші, то вони перетворюються до C-стилю. Інші нелітерні ASCI-символи довжиною менше 32 і більше 126 змінюються.

Коли ви визначаєте послідовність символів в аргументі charlist, переконайтеся, що ви знаєте, які символи ви ставите як початок і кінець діапазону.

```php
echo addcslashes('фуу[ ]', 'A..z');

// Значення: \f\o\o\o\o[ \]
// Видаляє всі літери нижнього та верхнього регістру
```

Значення, що повертаються
--------------------------

Повертає змінений рядок.

Приклад
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `\0.\37!@\177..\377`, видаляє всі символи з ASCII кодом від 0 до 31.
