Цикли та їх типи в PHP
======================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	uk: cikli-ta-ih-tipi-v-php
> 
> perex:
> 	- 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> 	- 'Цикл for, while, foreach і рекурсія в PHP + пояснення відмінностей між циклами. Можливості ітеративної обробки завдань.'
> 
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

Цикли в загальному випадку використовуються для повторення однієї і тієї ж ділянки коду (зазвичай над набором даних) Для кожного типу задач підходить свій тип циклу, який відрізняється в основному за змістом і синтаксисом. Також важливо відзначити, що всі типи циклів конвертуються між собою, просто це не завжди може бути доцільно з точки зору складності коду та часу обчислень.

Основний поділ контурів за типом елементів:

- for: загальний цикл, в якому ми заздалегідь знаємо кількість повторень, або можемо просто обчислити її заздалегідь,
- while і until-while: загальний цикл, в якому ми не знаємо кількість повторень заздалегідь,
- foreach: Перегляд масивів та об'єктів,
- "рекурсія": розкладання складної проблеми на сукупність більш дрібних.

Загальні властивості петель
-----------------------

Цикли, як правило, працюють шляхом повторення позначеного фрагмента коду **знову і знову**, **до тих пір, поки виконується умова завершення**. Зупинка циклу відбувається або при невиконанні завершальної умови, або шляхом ручної зупинки викликом `break;`.

Якщо цикл ніщо не зупиняє, то ніщо не заважає йому працювати нескінченно довго, або до тих пір, поки додаток не буде завершено вручну, або не закінчиться ліміт часу на обробку PHP-скрипта (зазвичай 30 секунд).

**Важливі умови:** *Важливі терміни

- "Цикл" - вираз мови, що дозволяє повторювати певну частину коду
- `Ітерація` - одне конкретне виконання тіла циклу
- Ініціалізація - запуск виконання циклу (перед початком першої ітерації)
- `Increment` - збільшення змінної ітерації на одиницю
- Умова виходу - умова, яка перевіряється на кожній ітерації (місце перевірки залежить від типу циклу). Цикл виконується до тих пір, поки виконується умова

Для петлі
----------

"Для" корисний для випадків, коли ми знаємо кількість повторень заздалегідь (або можемо легко її розрахувати). Він ідеально підходить, наприклад, для лінійного проходження інтервалів.

Загалом вона є письмовою (3 частини):

```php
for (inicializace; výraz; inkrementace) {
    // тіло циклу
}
```

- Ініціалізація: визначення початкового стану циклу (які змінні нам потрібні),
- вираз: Умова. Поки він дійсний, цикл буде виконуватися,
- `increment`: Зміна стану циклу з попереднього проходу (`increment` - приріст на одиницю).

Прикладом може бути виведення серії чисел від `0` до `10`:

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

Або кратні двом від `2` до `100`:

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

Цикл for корисний для різних трюків, таких як <a href="/abeceda-cisla-intervals">отримання алфавіту, масиву чисел та інтервалів</a>.

В той час як цикл з умовою виконання
-----------------------

Цикл While виконується до тих пір, поки виконується умова. Різниця між "поки" і "доки" полягає в тому, коли буде оцінюватися стан.

```php
while (výraz) {
    // тіло циклу
}
```

Альтернативно:

```php
do {
    // тіло циклу
} while(výraz);
```

- В "while" спочатку оцінюється умова, а потім виконується внутрішній цикл,
- "do-while" спочатку обробляє внутрішній цикл, а потім оцінює стан.

Це особливо вигідно, коли ми не можемо знати заздалегідь, чи буде коли-небудь виконуватися цикл, і хочемо гарантувати, що він завжди виконується хоча б один раз (тоді ми використовуємо `do-while`).

Приклад:

> У змінній `$x = 2` зберігається число. Ми хочемо також згенерувати число в інтервалі `<0; 10>` у змінній `$y` так, щоб воно відрізнялось від числа у змінній `$x`. Простіше кажучи: згенерувати випадкове число від `0` до `10`, яке не дорівнює `2`.

У цьому випадку зручно використовувати цикл "do-while". Ми не знаємо заздалегідь, скільки разів доведеться виконати цикл (нам може не пощастити і ми будемо постійно потрапляти на одне і те ж число, і тоді цикл буде працювати довго), а також хочемо гарантувати, що він виконається хоча б один раз до того, як обчислиться умова, так як спочатку нам потрібно згенерувати число.

```php
$x = 2;
$y = $x;

do {
   $y = rand(0, 10);
} while($x == $y);

echo 'X :' . $x . '<br>';
echo 'Y:' . $y;
```

Foreach
-------

`foreach` ідеально підходить для перегляду полів і об'єктів. Ми можемо отримати не тільки конкретні значення, а й ключі.

Якщо ми хочемо отримати тільки конкретні цифри:

```php
foreach ($data as $value) {
    // обробка даних
}
```

Як варіант, ми можемо отримати ключі:

```php
foreach ($data as $key => $value) {
    // обробка даних
}
```

Для перегляду реальних даних - наприклад, результатів з бази даних або полів з ключами - ідеально підходить "foreach":

```php
$countries = [
    'en' => 'Чеська Республіка',
    'Див.' => 'Словаччина',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ':' . $name . '<br>';
}
```

Рекурсія
-------

Рекурсія виникає, коли функція або метод викликає сам себе. Він служить для того, щоб розбити складну проблему на групу більш дрібних.

> Розуміння рекурсії може бути складним для початківців, оскільки вона заснована на ідеї передачі відповідальності.
>
> Функція фактично говорить щось на кшталт "я не можу вирішити цю проблему, але я знаю того, хто може...", тому вона дзвонить йому, він дзвонить комусь, ... поки не буде викликаний останній член, і це вирішить проблему.

Безумовно, варто відзначити, що будь-який рекурсивний алгоритм можна переписати так, щоб використовувати цикли, де рекурсія не потрібна (і навпаки теж). Рекурсія - хороший слуга, але поганий господар. Він допомагає вирішувати деякі типи завдань просто і дуже ефективно, в той час як зациклення на циклах корисно для інших речей.

Загалом, рекурсія є пам'яткоємною, оскільки весь час створює нові екземпляри та контексти функцій та методів. Однак, наприклад, для обходу деревовидних структур це єдиний розумний спосіб вирішення проблеми.

Приклад:

Нам потрібно обчислити факторіал числа `5`, так це робиться в математиці:

`5! = 5 * 4 * 3 * 2 * 1`

Тобто, відбувається послідовне множення ряду чисел від `1` до числа, яке нас цікавить, на факторіал.

Рекурсивно це можна вирішити дуже елегантно:

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

Альтернативним варіантом є ще більш коротка реалізація з використанням тернарного оператора:

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

Так само можна переписати для використання циклів:

```php
function factorial(int $n): int
{
    $result = 1;

    for ($i = $n; $i > 0; $i--) {
        $result *= $i;
    }

    return $result;
 }
```

Запис з циклом трохи довший, але знову ж таки має значно менший обсяг пам'яті. Для розрахунку використовується лише одна змінна `$result`, значення якої ми змінюємо безперервно і не створюємо нових екземплярів `factorial()`.

Дуже уважно пишіть нескінченні цикли
-----------------------------------

Цикл може дуже легко стати нескінченним (ми досягаємо цього, ніколи не задовольняючи умову завершення). Ви повинні це врахувати і, можливо, самостійно зупинити цикл за допомогою "брейк".

Наприклад, кидайте кубик до тих пір, поки не випаде число `6`. При реалізації є спокуса використати цикл "поки":

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Вартість впала' . $value;
      break;
   }
}
```

Написавши "поки (істина)", ми забезпечили собі нескінченну кількість повторень, бо "істина" завжди буде істиною. Отже, ми повинні самі зупинити цикл конструкцією `break;`, але що робити, якщо значення `6` ніколи не падає і ми ніколи не зупинимо цикл?

Особисто я завжди рахую кількість повторень і якщо вона перевищує якусь критичну межу, то примусово припиняю цикл. Наприклад, якщо ми перевищимо "1000" спроб. В такому випадку краще використовувати "за", а не "поки" і рахувати кількість запусків.

Якщо ми перевищуємо кількість прогонів, ввічливо повідомити про це розробника, кинувши виняток.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Вартість впала' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('Перевищено максимальну кількість кидків.');
}
```
