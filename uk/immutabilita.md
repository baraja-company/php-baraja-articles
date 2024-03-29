Незмінність об'єкта - ключова концепція проектування
====================================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	uk: nezminnist-ob-ekta-klyucova-koncepciya-proektuvannya
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

Незмінність є однією з найважливіших концепцій проектування для побудови стабільних додатків. Основний принцип полягає в тому, що після того, як держава записана, вона може бути лише прочитана пізніше без можливості її модифікації. Якщо нам потрібно змінити стан, ми повинні створити новий екземпляр і замінити весь об'єкт на інший.

Таким чином, типи даних можна дуже грубо розділити на дві великі категорії:

- **Mutable** (змінний стан в межах одного екземпляру)
- **Immutable** (незмінний внутрішній стан)

Об'єкти, що змінюються, можуть бути змінені внутрішньо. Тобто, вони забезпечують операції, які при виклику в різних комбінаціях призводять до того, що ми отримуємо різні результати. Незмінність намагається запобігти такій поведінці.

Визначення
--------

> Клас є **незмінним** саме тоді, коли дані екземпляру не можуть бути змінені жодним чином після створення екземпляру.

Тобто всі дані фіксуються в конструкторі. Всі скалярні типи даних також є автоматично незмінними.

Основна перевага
--------------

Проектування додатків з незмінними станами дає принципову перевагу в безпеці виконання операцій. Якщо ми знаємо, що одного разу записані дані не можуть бути змінені (мутовані) пізніше, ми можемо, наприклад, дуже надійно налагоджувати, або розбивати додаток на підфункції без ризику забути будь-який з проміжних станів.

Ідея незмінності, як правило, протистоїть принципу зберігання станів у властивостях об'єктів/сутностей, а скоріше описує функціональний підхід, коли дані просто "протікають" через додаток, як це робить, наприклад, javascript.

З точки зору продуктивності, ми можемо автоматично сказати про незмінні об'єкти, що їх можна кешувати нескінченно довго, тому що вони ніколи не будуть застарілими.

Реальний приклад з PHP
--------------------

На сьогоднішній день найпоширенішим використанням незмінних об'єктів в PHP є об'єкт `DateTimeImmutable`, який після створення може бути викликаний тільки методами форматування. Якщо ми змінимо внутрішні налаштування, метод поверне новий екземпляр. Ця функція має вирішальне значення при використанні ORM, який використовує так званий шаблон ідентичності - вона дозволяє нам, наприклад, гарантувати, що коли ми зчитуємо дату створення замовлення, вона буде однаковою скрізь у додатку і посилальна цілісність не буде порушена.

Конкретний приклад мутабельного об'єкту:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 день');
echo $date->format('Д-д-д'); // 2021-05-15
echo $tomorrow->format('Д-д-д'); // 2021-05-15
```

Була виведена та сама дата, тому що метод `modify()` лише змінив внутрішній стан об'єкту `DateTime` і повернув той самий екземпляр. Таким чином, не було так званої **мутації внутрішнього стану**, яка є базовою поведінкою об'єктно-орієнтованого програмування. Оновлення змінної також змінило вихідну змінну.

А тепер приклад незмінного об'єкта:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 день');
echo $date->format('Д-д-д'); // 2021-05-14
echo $tomorrow->format('Д-д-д'); // 2021-05-15
```

Об'єкт `DateTimeImmutable` є незмінним, що означає, що його внутрішній стан ніколи не змінюється. Новий модифікований екземпляр (також незмінний) зберігається у змінній після виклику методу `modify()`. Якби ми не зберегли нове значення у змінній, вона не була б придатною для подальшого використання.

Оригінальна вартість ніколи не чіпається і залишається надійно збереженою.

Коли клас повинен бути незмінним?
---------------------------

Якщо у вас немає дуже вагомої причини зробити його змінюваним, завжди пишіть клас або функцію як незмінний. Це спростить ваше проектування в майбутньому.

Змінювані класи повинні змінюватися якомога менше. Я завжди рекомендую документувати поведінку незмінності.

Мабуть, єдиним недоліком незмінності є те, що при кожній зміні атрибутів необхідно створювати новий екземпляр, що має незначний вплив на продуктивність. Якщо ваша програма (як і більшість програм) має тенденцію до відображення даних і рідше їх змінює, цей недолік є досить незначним з урахуванням продуктивності сучасних комп'ютерів.

Які види даних мають бути незмінними?
------------------------------------

- Всі ідентифікатори та унікальні коди
- Більшість сеансів роботи з базами даних ManyToOne і OneToOne
- Дати, час, календарні значення
- Циклічно перебираємо масиви, де хочемо виконати одну і ту ж операцію над кожним елементом
- Інтервали, пари, трійки, ...
- Геометричні фігури, точки, лінії, GPS координати, фізичні адреси, ...
- Журнали та історичні записи
- Інформація про оброблені замовлення та більшість фінансових даних
- Метадані про пов'язану особу

Що не повинно бути непорушним:

- Великі об'єкти з великою кількістю властивостей
- Більшість табличних виводів з бази даних, таких як сутності Доктрини
- Прогресивно побудовані об'єкти з менших частин
