Тест на базові знання Nette
===========================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	uk: test-na-bazovi-znannya-nette
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Поріг успіху: 15 балів

*За кожну правильну відповідь ви отримуєте 1 бал. За неправильну відповідь на будь-яке питання ви нічого не отримаєте. Якщо відповідь є лише частковою (і на її основі неможливо було б запрограмувати річ), питання вважається неправильним (не можна отримати половину балу). Якщо рішення містить баг безпеки, або помилку в коді, або описку в коді, відповідь вважається неправильною, тому що вона не буде виконуватися.*.

-----------

1 Поясніть різницю між циклами `for`, `while` та `foreach`. Для кожного навести 1 конкретний приклад його використання, який наочно демонструє його основну перевагу.


2. маємо змінну, про яку майже нічого не знаємо (знаємо лише її назву). Як можна ознайомитися з його змістом? Наприклад, він називається `$data`.


3. для роботи з Git-репозиторієм напишіть наступні команди, наведені нижче:
- Завантажити останні зміни з сервера
- тег `Statistic.php` у файлі `Statistic.php`.
- позначити всі файли в проекті тегами
- помітити всі файли в каталозі `cron` тегами
- фіксація змін з повідомленням "My commit"
- відправлення коміту на сервер


4. нехай у змінній знаходиться текстовий рядок. Навести приклад функції для обчислення контрольної суми.


5. написати фрагмент коду, який створює дію `delete` в `Presenter`, що приймає ідентифікатор елемента як ціле число і видаляє рядок з таблиці `question` за вказаним ідентифікатором. Після успішного видалення буде надруковано повідомлення "Питання видалено" та перенаправлення до дії "список".

Під питанням додатковий бал: Якщо видалення з якихось причин не вдається, вона не викидає фатальну помилку, а ще й інформує про це користувача повідомленням (флеш-повідомленням).

6. коли я створюю форму Nette, вона стає компонентом. Що таке компонент Nette?

7. мені потрібно створити просту форму Nette для вставки запису в таблицю `question`, яка містить список запитань. Структура таблиці виглядає наступним чином:

| Стовпчик | Властивості
|-----------|----------------------------------|
id | int(8), unsigned, auto increment | id | int(8), unsigned, auto increment
| питання | varchar(255) |
| is_active | tinyint(1), беззнаковий, значення за замовчуванням: 1

Створіть відповідні поля форми для вставки нового рядка в цю таблицю. Після вставки запису повинно бути видано FlashMessage, що інформує про успішну вставку запису + перенаправлення на редагування запису (дія `edit`).

- Підтвердження того, що поле форми заповнене
- Перевірка відповідності тексту питання у варчар відповідно до структури таблиці
- Підтвердження того, що питання з таким текстом більше не існує
- Визначити ще одну користувацьку таблицю `group`, яка буде містити інформацію про групи. При створенні питання потім можна буде визначити, до якої групи відноситься питання. Вам потрібно буде налаштувати сеанс зв'язку між таблицями (опишіть, як це робиться і як він буде налаштований).
- Який макрос Latte використовувати для рендеринга форми в шаблон (рендеринг за замовчуванням)?

8. нехай у `Презентері` є форма редагування, яка створюється як компонент. Ми хочемо передавати значення за замовчуванням з того, що є в базі даних, тобто нам потрібно отримати дані з таблиці якимось зручним способом.
- Як ви будете діяти і які елементи в коді нам потрібні?
- Як ви будете передавати на форму ідентифікатор елемента, що редагується в даний момент?
- Як встановити значення за замовчуванням для елемента форми?
- Як перевірити, що користувач намагається відредагувати елемент, якого не існує в базі даних, і як належним чином проінформувати його про це?

9 Розглянемо наступні дані, отримані з бази даних (використовується звичайна база даних Nette):

```php
$questions = $this->db->questions()->fetchAll();
```

- Як перерахувати текст всіх питань у вигляді маркованого списку?
- Як передати дані з таблиці в шаблон Latte?
- Які макроси Latte нам знадобляться для перерахування елементів? Наведіть конкретну реалізацію перерахування стовпців `id` та `name` у форматі:

	*1024: Як справи?
	*1025: Що ти сьогодні їв на обід?

10. наведіть приклад щонайменше 3 різних полів форми, які заповнюються у формі:

```php
$form->add(tady bude příklad);
```

і для кожної з них пояснити, для чого вона використовується і який результат повертає (тип даних + приклад).


11. давайте подамо форму Нетте.
- Як нам відправити всі поля (назви та значення)?
- Навести приклад виведення поля з іменем `question`.
- Надати конкретну реалізацію коду, який буде здійснювати обхід масиву значень та ключів і повертати єдиний рядок, що містить загальний список ключів та значень, щоб ми могли, наприклад, зберегти цей рядок в базі даних або відправити його електронною поштою (зберігання та відправка не є предметом завдання і не буде оцінюватись).


12. для кожної умови визначте, чи є результат ІСТИНА або ХИБНІСТЬ:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- '1 == true'
- `1 === true ''
- `1 === false`'
- `1 == "1" && 1=== true


13. з якими типами даних ми знайомі в PHP?
- Наведіть не менше 5 прикладів імен типів даних
- Для кожного прикладу перерахувати не менше 3 можливих значень, які може зберігати тип даних (якщо тип даних не може зберігати таку кількість можливих значень, так і написати)
- Для кожного типу даних навести типовий приклад його використання (що в ньому зберігається на практиці)
- Чим відрізняється умова `==` (дві рівності) від `===` (три рівності)?
- Пояснити, в чому недолік використання `==` в даних умовах і як конкретно `==` вирішує цю проблему (приклад, коли `==` може не спрацювати, а `==` врятує ситуацію)


14. нехай у нас є координаційна таблиця (таблиця узгоджень), в якій перераховані всі узгодження між 2 людьми. Один з них організовує координацію, а інший є гостем. Напишіть вибірку з бази даних, яка повертає всі рядки з координаціями, в яких беру участь я (чи я організатор координації, чи я гість координації). Таблиця має стовпці `id`, `id_user_organizer` (ідентифікатор організатора), `id_user_quest` (ідентифікатор гостя). Мій ідентифікатор зберігається у звичайному режимі в "Презентері".


15. група питань про латте:
- Що таке латте?
- Яка різниця між поняттями "змінна", "макрос", "фільтр" та "n:атрибут"? Що і де використовується?
- Як створити посилання `DashboardPresenter` на дію `за замовчуванням`?
- Перелічуючи таблицю питань, як згенерувати посилання на конкретне редагування (`QuestionPresenter`, `edit` дію) питання, щоб передати ідентифікатор поточного переліченого питання? Напишіть конкретний код латте.

Символічно записано (зразок на PHP, перекласти на Latte):

```php
foreach ($questions as $question) {
   echo $question->id; // Ідентифікатор питання
   echo $question->question; // текст питання
}
```

- Як написати фіксований HTML-пробіл?


16. для чого використовуються послуги в Нетте?
- Як це реалізується?
- Що потрібно зробити, щоб скористатися послугою?
- Як завантажити послугу в Presenter?
- Для прикладу розглянемо сервіс `StatisticManager`, який має загальнодоступний метод `getStatistics()`, що не приймає жодних параметрів. Як завантажити цей сервіс в Presenter і в дії за замовчуванням викликати загальнодоступний метод `getStatistics()` і передати результат в шаблон?
- У чому різниця між поняттями "об'єкт", "клас", "послуга"?
- Що таке "модель", "сутність" та "об'єкт цінності"?
- Всі послуги мають головного менеджера, який їх знає, і до нього можна звернутися. Як звати цього менеджера? Як зареєструвати в ньому нову послугу?


17. неон
- Що таке файли Neon?
- Які види бувають і за якими ознаками вони сортуються?
- Що містять файли Neon? Які дані в них зберігаються?
- Наведіть приклад, як прописати наступне поле (рецепт на PHP, потрібно буде його перекласти), щоб його можна було використовувати як параметр:

```php
$imageGenerator = [
   "бали" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Наведемо приклад реєстрації сервісу в Neon, якому передаємо параметр `imageGenerator`, який ми зареєстрували в попередньому завданні, щоб сервіс отримав його в конструкторі і міг використовувати в сервісі (в сенсі конфігурації). Для сервісу навести приклад реалізації конструктора таким чином, щоб перший вхідний параметр розглядався як тип даних для масиву.


18. об'єкти в цілому
- Що таке "метод", "властивості" і "константи"? У чому різниця між ними?
- Як методи, так і властивості мають 3 основні стани доступності (`публічний`, `приватний`, `захищений`), пояснюємо різницю та конкретний приклад використання і хто що і коли може бачити.
- У мене є клас `course` в якому є приватна властивість `currentCourse` в якій зберігається поточний курс. Як зробити властивість тільки для читання, а не для запису ззовні?


19. коли я створюю таблиці в базі даних, які логічно пов'язані між собою (наприклад, таблиця для користувача, а потім таблиця для його статей), мені потрібно подбати про те, щоб дані були коректно пов'язані.
- Що забезпечує коректне зв'язування даних всередині таблиць в базі даних?
- Що таке посилальна цілісність і яка її роль в базі даних?
- Які види сесій ми проводимо? Яке призначення кожного виду?
- Які параметри ми встановлюємо для сесій, щоб по-різному обробляти видалення або зміну даних? Наведіть 3 приклади і конкретні застосування + опис того, як це працює.


20. для чого призначені фабрики (ООП-проектування)?
- Наведіть приклад використання.
- Чи є сервісні центри Nette фабриками?
- Яка мета ін'єкції від залежності?
- У чому різниця між "ДІ" та "ОПК"?
