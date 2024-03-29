UUID і продуктивність великомасштабних додатків
===============================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	uk: uuid-i-produktivnist-velikomasstabnih-dodatkiv
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

Коли розмір бази даних перевищує мільйони рядків, доцільно почати масштабування додатку і розбити базу даних на кілька фізичних серверів.

Найбільшою проблемою поділу бази даних на декілька частин є її подальша синхронізація у разі запиту користувачем конкретних даних.

Навіщо використовувати UUID і в чому його переваги над автоінкрементом
--------------------------------------------------------

Припустимо, у вас є таблиця "статей", але через те, що у вас величезний сайт, вище розташовані десятки мільйонів статей, і вам доводиться фізично розподіляти їх між декількома машинами.

Якби ми використовували звичайне ціле число в якості "id" (первинного ключа) з налаштуванням автоінкременту, то дуже швидко виявили б, що при створенні записів на різних машинах децентралізовано, а потім їх синхронізації, виникають колізії ідентифікаторів і доводиться складним чином перенумеровувати записи. Крім того, якщо ми вирішуємо багато сесій за іншими столами, це може бути дуже складною накладною роботою, в якій легко припуститися помилок.

Тому замість числового ідентифікатора ми можемо генерувати `UUID`, який являє собою текстовий рядок, що генерується за складним алгоритмом, який гарантує, що він буде унікальним, навіть якщо буде згенерований незалежно на декількох машинах.

Переваги:

- Якщо у вас є кілька незалежних баз даних, які ви потім синхронізуєте, використання UUID означає, що один ідентифікатор є унікальним для всіх баз даних, а не тільки для тієї, де ви перебуваєте і де він був згенерований. При об'єднанні в єдиний кластер конфліктів не виникає.
- Ви можете знати свій "первинний ключ" до того, як вставити запис в базу даних. Це зменшує кількість SQL-запитів, спрощує логіку транзакцій, і ви можете легко використовувати його як зовнішній ключ до того, як колекція записів буде створена.
- UUID не розкриває інформацію про кількість дат і послідовностей і є більш безпечним для використання в URL-адресах. Наприклад, якщо я виявив, що я користувач `19010018`, то неважко здогадатися, що існує також користувач `19010017` та інші. Атака називається векторною.

Генерація нового UUID
----------------------

UUID можна отримати або простим SQL запитом `SELECT UUID();`, але при цьому збільшується кількість запитів до бази даних і ми втрачаємо можливість підготувати дані спочатку масово в логіці програми, а потім записати їх відразу.

Тому мені подобається користуватися пакетом <a href="https://github.com/ramsey/uuid">ramsey/uuid</a>, отриманим за допомогою Composer, як хорошим рішенням. Сам UUID має кілька версій, і пакет може грайливо генерувати всі види за потреби.

Це робить його простим у використанні:

```php
require 'vendor/autoload.php';

use Ramsey\Uuid\Uuid;

// Створює об'єкт UUID версії 1 (на основі часу)
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Генерує об'єкт UUID версії 3 (на основі імені та хешований як MD5)
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Формує об'єкт UUID версії 4 (випадковий)
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Генерує об'єкт UUID версії 5 (на основі імені та хешований як SHA1)
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Якщо ви використовуєте Doctrine, існує розширення <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a>, яке генерує ідентифікатор безпосередньо як тип даних.

Фізичне зберігання в базі даних
---------------------------

У своїх перших спробах я використовував `varchar(36)` як первинний ключ (ID), але <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">це зовсім не найкраща ідея</a>.

> Пояснення внутрішньої логіки:**.
>
> Бази даних MySql (і багато інших) не можуть ефективно використовувати `varchar`, `char` або інші типи даних, що виражають рядок як первинний ключ.
> У деяких базах даних існує тип даних `GUID`, який призначений для безпосереднього зберігання UUID. Якщо ви не можете використовувати цей тип, є відповідний замінник у вигляді `binary(16)`.

При фізичному дослідженні бази даних ідентифікатор після цього представляється в HEX форматі (оскільки двійковий формат не відображається), замість красивого ідентифікатора `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6` ви побачите просто `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, який в запиті INSERT виглядає як `'?kYߟKg2c;'`.

Перетворення вихідних даних з `varchar(36)` в `binary(16)
----------------------------------------------------

Я припускаю, що ви представляєте (або плануєте представляти) новий встановлений ідентифікатор в базі даних як:

```sql
`id` binary(16) NOT NULL
```

Однак, просто змінити тип даних не вийде, тому щось на кшталт:

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

Причин, в основному, дві:

- Первинний ключ та сесія до нього "повинні мати однаковий тип даних". Тому потрібно змінити як тип даних для ідентифікатора статті, так і, наприклад, в реляційній таблиці, яка зіставляє статті з авторами.
- Двійковий формат містить дещо інше, ніж вихідний рядок. Потрібно скористатися функцією конвертації.

Тому єдине правильне рішення - зробити резервну копію даних (але це все одно потрібно робити перед кожною міграцією), підготувати порожню базу даних з функціональними зв'язками і знову перенести туди дані за допомогою міграції.

Якщо ви раніше генерували UUID дивним чином, краще вибрати якийсь послідовний метод отримання UUID і перенумерувати всі записи. Причина полягає в тому, що послідовне розміщення дозволяє краще впорядкувати значення і створити `btree`, що робить продуктивність практично ідентичною `bigint`.

**Якщо ви знаєте кращий спосіб конвертації існуючої бази даних з UUID, що зберігається у вигляді varchar, у двійковий формат без необхідності розробки складних міграцій та зі збереженням зовнішніх ключів, буду дуже вдячний за зворотній зв'язок**.
